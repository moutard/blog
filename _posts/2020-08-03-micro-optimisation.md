---
layout: post
title: Micro Optimisation, don't get lost down the rabbit hole.
description: A long journey from javascript to C++.
date:   2020-06-28 15:01:35 +0300
author: raphael
image:  '/images/micro-optimisation/tunnel-reshape.png'
tags:   [management]
tags_color: '#477690'
---

I had to settle a performance discussion within my team. Because of a simple PR, I started a 2 weeks journey in the dark twists and turns of javascript. To save you a lot of pain and frustrating questions, I sum up my research in this really long post. I tried my best to show you the train of thoughts but if you don’t care about the details, you can jump to the end for the TL;DR section.

# Call to Adventure
Everything started with a simple Pull Request on Github. It’s a javascript file, I let you read:

<img src="/images/micro-optimisation/original-commit.png" loading="lazy" alt="original commit">

To sum-up an engineer of my team wrote a one-liner to check if the value was valid, using the following code:

```javascript
['valid_value1', 'valid_value2'].includes(value);
```

And another one suggested a ‘performance improvement’ using instead:

```javascript
const VALID_VALUES = new Set(['valid_value1', 'valid_value2']);
VALID_VALUES.has(value);
```

The argument is about complexity saying we are moving from O(N) to O(1). The author of the PR started to argue that it was not an improvement as the time to create the Set from an array is O(N).

Disclaimer, after 10 years of development, I built a strong opinion about this: in term of Performance, it DOES NOT MATTER! This is called micro-optimisations, and they don’t make any difference in web development (not true for video games). So I used my manager authority to stop the discussion.

# A quest for the truth

I could have just stopped there and go back to my life. But I was afraid it would happen again. Indeed, I noticed in my career, that tech team don’t like managers to take decisions for them. Developers want to understand the reason. So I had to settle that once for all, and I started to dive deep…

As every programmer, I started googling to find a benchmark. JSPerf is a great resource. It lists hundreds of benchmarks just to compare array and set. The result was not the one I expected. The two most popular tests have opposite conclusions. In experiment (A) `array.includes()` is faster than `set.has()` but not in experiment (B).

<img src="/images/micro-optimisation/jsperf-1.png" loading="lazy" alt="(A) Experiment: includes() is faster">
<em>(A) Experiment: includes() is faster.</em>
<img src="/images/micro-optimisation/jsperf-2.png" loading="lazy" alt="original commit">
<em>(B) Experiment: has() is faster.</em>

# Benchmark: allies or enemies?
The reason they have different conclusions is that the two benchmarks don’t actually test the same thing. Can you spot the problem?

EXPERIMENT (A):

```javascript
// SETUP
const keys = ['build', 'connected', 'firmware', 'model', 'status']
const set = new Set(keys); 
// RUN 
const randomKey = keys[Math.floor(Math.random() * keys.length)];
keys.includes(randomKey) // --> Faster
set.has(randomKey) 
```

EXPERIMENT (B)

```javascript
// SETUP
var keys = [...Array(512)]
             .map((_, index) => String.fromCharCode(index + 32));
var set = new Set(keys); 
// RUN 
const randomKey = String.fromCharCode(~~(Math.random() * keys.length * 2) + 32);
keys.includes(randomKey)
set.has(randomKey) // --> Faster
```

They are 2 main differences:
- Experiment (A) only has 5 items in the array, as (B) has 512.
- Experiment (A) only test for hit (i.e. when the value is in the array). As (B) test for both hit and miss .

A miss is obviously slower for an array because that’s the worst-case scenario. You have to loop over all the items to know the element is not present! You are trying to compare the best case complexity to the average complexity. That was my first hard lesson. Do not trust benchmarks!

# A benchmark to rule them all?
As we saw the size of the array will make the performance vary. I wanted to understand what was the threshold. When does a set start becoming more efficient than an array?

As most of the developers, I thought I would be able to resolve this mystery by writing my own code. I focused on .has() and .includes() so I am voluntarily excluding the construction of the Set from the benchmark.

```javascript
// SETUP - not included in the perf measure.
var SIZE = 1000;
var GLOBAL_ARRAY = [];
for (var i = 0; i < SIZE; i++) {
	GLOBAL_ARRAY.push('key_' + i);
}
var GLOBAL_SET = new Set(GLOBAL_ARRAY);
var LAST_KEY = 'key_' + (SIZE - 1);

var suiteHasVsIncludes = new Benchmark.Suite;
```

```javascript
// BENCHMARK on MISS
GLOBAL_ARRAY.includes('key_unknown');
GLOBAL_SET.has('key_unknown');

// BENCHMARK on HIT - WORSE CASE SCENARIO
GLOBAL_ARRAY.includes(LAST_KEY);
GLOBAL_SET.has(LAST_KEY);

// BENCHMARK on HIT - BEST CASE SCENARIO
GLOBAL_ARRAY.includes('key_0');
GLOBAL_SET.has('key_0');
```


<img src="/images/micro-optimisation/ops-per-sec.png" loading="lazy" alt="operation per secondes">
<em>Ops/sec for a MISS (log scale in X-axis — Higher is better)</em>

As expected the time for a lookup using a Set doesn’t vary much with the Size, as it should have a complexity of O(1). The cost of a miss in the array is linear until 5 000 (hard to see because of the log scale in X-axis). So far it’s coherent with the complexity, as we have to loop over all the items O(N). However, there is a massive drop after.

To conclude, if you only compare `set.has()` and `array.includes()` until you reach a SIZE of 5 000, arrays perform x8 times faster, after a 100 000 `set.has()` is x10K times faster.

Once again, do not trust my own benchmark. I am not better than everyone else. For instance, I am using the same key, what if there is some kind of cache mechanism under the hood? I also realised that performance tests are not reliable when you run them on your local computer. As you are not in a controlled environment, many processes are competing for the CPU. Two consecutive runs can have different results (especially if you are listening to Spotify in the background). I tried running the same code 10 times on my local machine and I had up to x3 factor difference between tries.

```
Try 1: array.includes() x 27,033 ops/sec ±41.13% (82 runs sampled)
Try 2: array.includes() x  9,286 ops/sec ±15.05% (83 runs sampled)
```

The library I used `Benchmark.js` actually tells you to be careful. There was a ±41.13% difference within the 82 runs. It’s pretty fishy and you should probably discard this run.

# A remaining Snag
I was first happy with this result. It was coherent with my understanding of the complexity. Search in an array is O(N) while set uses an O(1) lookup in a Hashtable.

There was one thing that was still bothering me. With SIZE<5000, `array.includes()` is performing 8 times better than `set.has()`

I couldn’t live with this incoherence, how could I explain that smaller Array actually performs much better than Set.

# The cave: a step further in the dark
I started to poke around on the internet, and I found this article: [Optimizing hash tables: hiding the hash code.](https://v8.dev/blog/hash-code)

*Set, Map and WeakSet and WeakMap all use hash tables under the hood. A [hash function](https://en.wikipedia.org/wiki/Hash_function) is used to map a given key to a location in the hash table. A hash code is the result of running this hash function over a given key.*

*In V8, the hash code is just a random number, independent of the object value. Therefore, we can’t recompute it, meaning we must store it.*

When I read the last line, I couldn’t believe it. In javascript, a hashcode is not the result of a hash function, but a random number?! Why is it called a Hash table? I was completely lost 🤔. Then I figured it out. Let’s take an example of two players:

```javascript
var player1 = {
  name: "Alice",
  score: 87
};
var player2 = {
  name: "Bob",
  score: 56
}
var set = new Set();
set.add(player1);
set.add(player2);
player2.score = 66;
set.has(player2) // -> true
```

In Javascript, Objects are mutable, for instance, the score can change. So we can’t use the object content to generate a unique hash. If Bob improves his score the hash will be different. Moreover, the memory position can not be used because the garbage collector moves objects around. That’s why it generates a random number that is stored with the object. [(1)](https://news.ycombinator.com/item?id=16264621)

🔬 What about other language?
*The implementation for Java Both [OpenJDK 7](https://hg.openjdk.org/jdk7u/jdk7u/hotspot/file/5b9a416a5632/src/share/vm/runtime/globals.hpp#l1100) and OpenJDK 6 are using a random number as explained in the following article [How does the default hashCode() work?](https://srvaroa.github.io/jvm/java/openjdk/biased-locking/2017/01/30/hashCode.html) [(2)](https://srvaroa.github.io/jvm/java/openjdk/biased-locking/2017/01/30/hashCode.html)*

In Python, you simply can’t hash (some) mutable objects:

```python
mylist = []
d = {}
d[mylist] = 1
Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: unhashable type: 'list'

```

That was the first revelation, hash function in theory and in practice are actually two different things. That could explain performance difference. But wait a minute, the code of the benchmark is:

```javascript
GLOBAL_SET.has('key_0');
```

the key is not a mutable object, it’s a string, an **immutable** primitive data type in Javascript! [(3)](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)

🔬 string vs String
*Do not confuse string the primitive and String the standard built-in Object. [(4)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) that is a wrapper around the primitive type, and as an Object String are mutable.*

```javascript
var name = new String('Alice'); // returns a mutable object.
```

Why can’t we use a hash function for immutable keys like `string`? I had to know. I was back to the beginning, so I made a final move. The implementation of Set in node.js actually relies on Google V8 engine. As it’s open-source, I looked at the code…

# The crossing: truth is in the code
*From here we will dive deep into optimised C++ code. I know it’s not easy to read. I did my best to only pinpoint essential parts of the implementation, but feel free to trust me and skip the code samples.*

First I grep for `Set` in v8 codebase, I ended up with +3000 results, so I realised that was a bad idea 😅. I looked for WeakSet to narrow it down, as they both rely on the same hashmap implementation. I found my entry point:

```c++
class BaseCollectionsAssembler : public CodeStubAssembler {
 public:
  explicit BaseCollectionsAssembler(compiler::CodeAssemblerState* state) : CodeStubAssembler(state) {}

  virtual ~BaseCollectionsAssembler() = default;

 protected:
  enum Variant { kMap, kSet, kWeakMap, kWeakSet };

  // Adds an entry to a collection.  For Maps, properly handles extracting the
  // key and value from the entry (see LoadKeyValue()).
  void AddConstructorEntry(Variant variant, TNode<Context> context,
                           TNode<Object> collection, TNode<Object> add_function,
                           TNode<Object> key_value,
                           Label* if_may_have_side_effects = nullptr,
                           Label* if_exception = nullptr,
                           TVariable<Object>* var_exception = nullptr);
```

As you can see most of the code is actually shared between `Map` , `Set` , `WeakMap` and `WeakSet`. We already knew that by reading the v8 blog from the last section.

# Allocate Memory

```c++
TNode<HeapObject> CollectionsBuiltinsAssembler::AllocateTable(
    Variant variant, TNode<IntPtrT> at_least_space_for) {
  if (variant == kMap || variant == kWeakMap) {
    return AllocateOrderedHashTable<OrderedHashMap>();
  } else {
    return AllocateOrderedHashTable<OrderedHashSet>();
  }
}
```

The memory is allocated via `AllocateTable` method and it calls `AllocateOrderedHashTable<OrderedHashSet>`. I am going to spare you a couple of jumps. I ended up looking at the constructor of the class `OrderedHashTable`

```c++
// OrderedHashTable is a HashTable with Object keys that preserves
// insertion order. There are Map and Set interfaces (OrderedHashMap
// and OrderedHashTable, below). It is meant to be used by JSMap/JSSet.
template <class Derived, int entrysize>
class OrderedHashTable : public FixedArray {
 public:
  // Returns an OrderedHashTable (possibly |table|) with enough space
  // to add at least one new element.
  static MaybeHandle<Derived> EnsureGrowable(Isolate* isolate,
                                             Handle<Derived> table);
```
<em><a href="https://github.com/v8/v8/blob/master/src/objects/ordered-hash-table.h">v8/blob/master/src/objects/ordered-hash-table.h</a></em>

To optimise memory, `OrderedHashTable` has two different implementations, depending on the size required. `SmallOrderedHashTable` is similar to the `OrderedHashTable`, except for the memory layout uses byte instead of Smi (SMall Integer) as the hash key. It starts with 4 buckets, then it doubles capacity until it reaches 256. Beyond that limit, the code rehashes every item into a new `OrderedHashTable`. This doesn’t explain by itself the fact that Set is less performant for small size than an array.

**🔬 What’s the difference between HashTable and OrderedHashTable?** *Hash table are designed to provide O(1) access time to N items. To do so they allocate M memory slots, called buckets. To avoid collision they pick M >> N. Collision are stored as a link list in a bucket.*
*Hash table are not designed to list all N elements. For that you would need to loop over all the M buckets even if they are empty. Javascript specifies that all collections have a `.keys()` method, that allows you to iterate efficiently over the keys. An OrderedHashTable maintains another list of keys inserted in order. (it’s a tradeoff between speed and memory).*

```javascript
var set = new Set(['key1', 'key2']);
set.keys() // -> we want to iterate over the keys
```


<img src="/images/micro-optimisation/hash-table-memory.png" loading="lazy" alt="hash table memory">
<em><a href="https://en.wikipedia.org/wiki/Hash_table">Memory layout for HashTable</a></em>

# Find a Key
We know how a Set is stored in memory. The next step is to see how we find a key. When you call the method `set.has()` you end up calling an `OrderedHashTableMethod::hasKey()` , that will call `OrderedHashTableMethod::FindEntry()`

```c++
template <class Derived>
bool SmallOrderedHashTable<Derived>::HasKey(Isolate* isolate,
                                            Handle<Object> key) {
  DisallowHeapAllocation no_gc;
  return FindEntry(isolate, *key) != kNotFound;
}
```
<em><a href="https://github.com/v8/v8/blob/master/src/objects/ordered-hash-table.cc#L877">SmallOrderedHashTable::HasKey in ordered-hash-table.cc</a></em>

```c++

template <class Derived>
int SmallOrderedHashTable<Derived>::FindEntry(Isolate* isolate, Object key) {
  DisallowHeapAllocation no_gc;
  Object hash = key->GetHash();

  if (hash->IsUndefined(isolate)) return kNotFound;
  int entry = HashToFirstEntry(Smi::ToInt(hash));

  // Walk the chain in the bucket to find the key.
  while (entry != kNotFound) {
    Object candidate_key = KeyAt(entry);
    if (candidate_key->SameValueZero(key)) return entry;
    entry = GetNextEntry(entry);
  }
  return kNotFound;
}
```
<em><a href="https://github.com/v8/v8/blob/master/src/objects/ordered-hash-table.cc#L877">SmallOrderedHashTable::HasKey in ordered-hash-table.cc</a></em>

We are almost there! We only need to understand how `key->GetHash()` works. If you don’t want to read the code let me sum up for you. Depending on the type of the Isolate (Object, String, Array), the hash function will differ. For Object the hash is a random number (we already discovered that in the previous part), for `string` the hash code is generated by iteration on every character.

```c++
//  string.cc - Object Hash returns a random number.
int Isolate::GenerateIdentityHash(uint32_t mask) {
  int hash;
  int attempts = 0;
  do {
    hash = random_number_generator()->NextInt() & mask;
  } while (hash == 0 && attempts++ < 30);
  return hash != 0 ? hash : 1;
}
```
```c++
// String Hash returns hash based on iteration for each character.
template <typename Char>
uint32_t HashString(String string, size_t start, int length, uint64_t seed) {
  DisallowHeapAllocation no_gc;

  if (length > String::kMaxHashCalcLength) {
    return StringHasher::GetTrivialHash(length);
  }

  std::unique_ptr<Char[]> buffer;
  const Char* chars;

  if (string.IsConsString()) {
    DCHECK_EQ(0, start);
    DCHECK(!string.IsFlat());
    buffer.reset(new Char[length]);
    String::WriteToFlat(string, buffer.get(), 0, length);
    chars = buffer.get();
  } else {
    chars = string.GetChars<Char>(no_gc) + start;
  }

  return StringHasher::HashSequentialString<Char>(chars, length, seed);
}
```

**🔬 C++ Standard Library:** *If you are familiar with C++, you could wonder why they don’t use the std implementation of a HashTable. From my understanding is that because a HashTable in the standard lib is typed. You can only insert object of the same type. In javascript they need an extra wrapper to be able to store different types in the same set. Moreover in the std:lib Set are actually implemented as a binary search tree, not as a HashTable, so lookup is O(Nlog(N)).*

# The Truth about Array
Now we understand how `Set` is working in Javascript, to be able to understand why `array` performs better for the smaller sizes we need to understand how it’s implemented.

Here I am going to spare you the C++ code. Instead, I will refer to this post from the V8 blog.

> “JavaScript objects can have arbitrary properties associated with them. However JS engine is able to optimize properties whose names are purely numeric, most specifically array indices.”

[Elements kinds on v8](https://v8.dev/blog/elements-kinds)

In V8, properties with integer names — array indices — are handled differently. If the external behaviour of numerical property is the same as non-numeric properties, V8 chooses to store them separately for optimization purposes. Internally, V8 calls numeric property: *elements*. Objects have named-properties that map to values, whereas arrays have indices that map to elements.

<img src="/images/micro-optimisation/properties-vs-elements.png" loading="lazy" alt="property vs elements">
<em><a href="https://v8.dev/blog/fast-properties">Indexed properties are not sharing the same memory as named properties.</a></em>

If we dive even deeper, the are 6 types of elements:

<img src="/images/micro-optimisation/packed-elements.png" loading="lazy" alt="packed element">
<em><a href="https://v8.dev/blog/elements-kinds">The 6 types of elements used in Array.</a></em>

On one hand, `PACKED_ELEMENT` are used when the array is full and has no hole. In this case, the underlying memory layout is optimised in C++.

`HOLEY_ELEMENTS` , on the other hand, are used for sparse arrays, every time you create a hole in an array the memory layout change from `PACKED` to `HOLEY` and you can never go back to `PACKED`

The problem with `HOLEY_ELEMENTS` is that v8 cannot guarantee that they will have a value to return, and because prototype can be overridden, v8 has to go up the chain of the prototype to check if there is value somewhere. In `PACKED_ELEMENTS` we know the value exists so we don’t inspect the full prototype allowing fast access 🚀!

```javascript
array[8] // -> ??? ❌
8 >= 0 && array.length; // bounds check -> true ❌
hasOwnProperty(array, '8'); // -> false ❌
hasOwnProperty(Array.prototype, '8'); // -> false ❌
hasOwnProperty(Object.prototype, '8'); // -> false

```
<em>asking for array[8] on a spare array will force V8 engine to inspect the chain of the prototype.<em>
As a reminder, in my benchmark I set up my array using the following code:

```javascript
var SIZE = 1000;
var GLOBAL_ARRAY = []
for (var i = 0; i < SIZE; i++) {
  GLOBAL_ARRAY.push('key_' +  i);
}
```

By doing that I am creating a `PACKED_ELEMENTS` type. So because v8 knows there is no hole in my array, it will optimise the underlying memory and use fast access. If I had initialised my array differently, for instance, `var = GLOBAL_ARRAY = new Array(SIZE)` then I would make sure the creation is faster as v8 will allocate the right amount of memory upfront. But this would create holes, and I would end up with `HOLEY_ELEMENTS` so the lookup would be slower as I can’t use fast access anymore.

Here we are, the final conclusion 💪. Set uses spare memory access and `HOLLEY_ELEMENTS` properties, whereas `array` has indices that map to `elements` compactly stored in memory allowing extra fast access.

I was finally satisfied. Until you reach 5 000 elements the complexity overhead to go through the entire array is negligible compared to the memory access improvements made by V8 to store arrays.

*If you think, good I will use array now. Keep reading because you are missing my point.*

# How far do you go?
I had a cold realisation: this journey took me 2 weeks of deep research. I could even go further. It reminded me of a talk from Douglas Hawkins on Java Performance puzzlers. where he explains that the running through an array is optimised in Java because the Hardware (yes the Intel or AMD ship itself), it will preload not only the first item but all the continuous item in the L1 cache, then prefetch the following numbers in the L3 cache. You can’t do that for a Set, so you always need to go to memory.

Performance optimisation is especially interesting for language with no Garbage collectors.

**🔬 i++ vs ++i** *This one is a classic one in C++ interview, it says that i++ actually creates a copy of i then increment the variable then replace i with the new value. But ++i is directly incrementing the value, so it’s faster.*

You can find much more examples like this one in [Effective C++ by Scott Meyers](https://www.amazon.com/Effective-Specific-Improve-Programs-Designs/dp/0321334876). I also rediscover the blog by [Andy Gavin the creator of Crash Bandicoot](https://all-things-andy-gavin.com/2011/02/02/making-crash-bandicoot-part-1/), where he explains how he made the video game fit in 2MB RAM with a lot of optimisation tricks.

This is a never-ending journey. So I decided to stop here and write this article. I probably already went too far. The hard question then is: “Was this quest worth it?”

# 🚀 TL;DR & Conclusion
This article is the end of a 2 weeks journey. That’s a long time to settle a performance argument. So here are my main conclusions:

* Micro Optimisations are irrelevant for Web Development. On server-side, the bottleneck will always be network calls to third parties, I/O access and database queries. On the frontend side, the bottleneck will be the modification of your DOM. Even if React has a virtual DOM, a bad implementation of a render method can ruin your performance.
* Focus on what really matters, code should be easy to test and easy to read. Most of the time, micro optimisation will make your code hardly understandable, and challenging to test. In the long term, you will save more time by having a clear code. Put that into perspective, in the worst-case scenario, we are talking about 10 000ops/sec, if a dev loses an hour to understand a micro-optimised code, that saves you 0.1ms per run, you need 30 million runs to have a positive ROI.

🏎 If you want to talk about performance anyway:

* You probably should never have to handle 100 000 items in your javascript web application. Instead, try to use pagination, or map-reduce to avoid large data handling.
* Do not trust benchmarks! Depending on the implementation they may give you different results. If you have a specific issue write your own benchmark to make sure you test with your data. Make your code as close as possible as the problem you have.
* If you ever decide to trust a benchmark, run it on a controlled environment, like a dedicated instance with nothing else running on.

🧲 If you want to talk about complexity anyway:

* There is a massive difference between the theoretical complexity and the real implementation of a data structure like Set. You may have preconceived ideas, based on simplified descriptions used in books. Because objects are mutable and the garbage collector moves objects around, v8 uses random number stored on the object instead of hashing the content.
* Time is not the only factor you want to optimise, there are tradeoffs between space and memory access (disk vs RAM). V8 will optimise packed array where there is no hole.
* It’s always interesting to go back to the source code. It reminds us than javascript is not a magic language and the flexibility comes with a price on performance. That’s why video games mostly use C++.