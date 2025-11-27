# Random Number Generators (RNG)

This library provides an `RNG` type (Random Number Generator) for
[Tomo](https://tomo.bruce-hill.com). This type represents a self-contained
piece of data that encapsulates the state of a relatively fast and relatively
secure pseudo-random number generator. The current implementation is based on
the [ChaCha20 stream
cipher,](https://en.wikipedia.org/wiki/Salsa20#ChaCha_variant) inspired by
[`arc4random` in OpenBSD.](https://man.openbsd.org/arc4random.3)

An `RNG` object can be used for deterministic, repeatable generation of
pseudorandom numbers (for example, to be used in a video game for creating
seeded levels). The default random number generator for Tomo is called `random`
and is, by default, initialized with random data from the operating system when
a Tomo program launches.

## RNG Functions

This documentation provides details on RNG functions available in the API.
Lists also have some methods which use RNG values:
`list.shuffle()`, `list.shuffled()`, `list.random()`, and `list.sample()`.

- [`func bool(rng: RandomNumberGenerator, p: Num = 0.5 -> Bool)`](#bool)
- [`func byte(rng: RandomNumberGenerator -> Byte)`](#byte)
- [`func bytes(rng: RandomNumberGenerator, count: Int -> [Byte])`](#bytes)
- [`func int(rng: RandomNumberGenerator, min: Int, max: Int -> Int)`](#int-int64-int32-int16-int8)
- [`func new(seed: [Byte] = (/dev/urandom).read_bytes(40)! -> RandomNumberGenerator)`](#new)
- [`func num(rng: RandomNumberGenerator, min: Num = 0.0, max: Num = 1.0 -> Num)`](#num-num32)

## Usage

Put this in your modules.ini:

```
[random]
version=v1.2
git=https://github.com/bruce-hill/tomo-random
```

Then either use the default RNG (seeded from OS random sources each run):

```
use random

func main()
    >> random.int(1, 100)
    
    my_rng := RandomNumberGenerator.new()
    >> my_rng.int(1, 100)
    
    my_list := ["A", "B", "C"]
    >> my_list.random(func(lo, hi:Int64) my_rng.int64(lo, hi))
```

-------------

### `bool`
Generate a random boolean value with a given probability.

```tomo
func bool(rng: RandomNumberGenerator, p: Num = 0.5 -> Bool)
```

- `rng`: The random number generator to use.
- `p`: The probability of returning a `yes` value. Values less than zero and
  `NaN` values are treated as equal to zero and values larger than zero are
  treated as equal to one.

**Returns:**  
`yes` with probability `p` and `no` with probability `1-p`.

**Example:**  
```tomo
>> random.bool()
= no
>> random.bool(1.0)
= yes
```

---

### `byte`
Generate a random byte with uniform probability.

```tomo
func byte(rng: RandomNumberGenerator -> Byte)
```

- `rng`: The random number generator to use.

**Returns:**  
A random byte (0-255).

**Example:**  
```tomo
>> random.byte()
= 103[B]
```

---

### `bytes`
Generate a list of uniformly random bytes with the given length.

```tomo
func bytes(rng: RandomNumberGenerator, count: Int -> [Byte])
```

- `rng`: The random number generator to use.
- `count`: The number of random bytes to return.

**Returns:**  
A list of length `count` random bytes with uniform random distribution (0-255).

**Example:**  
```tomo
>> random.bytes(4)
= [135[B], 169[B], 103[B], 212[B]]
```

---

### `int`, `int64`, `int32`, `int16`, `int8`
Generate a random integer value with the given range.

```tomo
func int(rng: RandomNumberGenerator, min: Int, max: Int -> Int)
func int64(rng: RandomNumberGenerator, min: Int64 = Int64.min, max: Int64 = Int64.max -> Int)
func int32(rng: RandomNumberGenerator, min: Int32 = Int32.min, max: Int32 = Int32.max -> Int)
func int16(rng: RandomNumberGenerator, min: Int16 = Int16.min, max: Int16 = Int16.max -> Int)
func int8(rng: RandomNumberGenerator, min: Int8 = Int8.min, max: Int8 = Int8.max -> Int)
```

- `rng`: The random number generator to use.
- `min`: The minimum value to be returned.
- `max`: The maximum value to be returned.

**Returns:**  
An integer uniformly chosen from the range `[min, max]` (inclusive). If `min`
is greater than `max`, an error will be raised.

**Example:**  
```tomo
>> random.int(1, 10)
= 8
```

---

### `new`
Return a new random number generator.

```tomo
func new(seed: [Byte] = (/dev/urandom).read_bytes(40)! -> RandomNumberGenerator)
```

- `seed`: The seed use for the random number generator. A seed length of 40
  bytes is recommended. Seed lengths of less than 40 bytes are padded with
  zeroes.

**Returns:**  
A new random number generator.

**Example:**  
```tomo
>> my_rng := RandomNumberGenerator.new([1[B], 2[B], 3[B], 4[B]])
>> my_rng.bool()
= yes
```

---

### `num`, `num32`
Generate a random floating point value with the given range.

```tomo
func num(rng: RandomNumberGenerator, min: Num = 0.0, max: Num = 1.0 -> Int)
func num32(rng: RandomNumberGenerator, min: Num = 0.0_f32, max: Num = 1.0_f32 -> Int)
```

- `rng`: The random number generator to use.
- `min`: The minimum value to be returned.
- `max`: The maximum value to be returned.

**Returns:**  
A floating point number uniformly chosen from the range `[min, max]`
(inclusive). If `min` is greater than `max`, an error will be raised.

**Example:**  
```tomo
>> random.num(1, 10)
= 9.512830439975572
```
