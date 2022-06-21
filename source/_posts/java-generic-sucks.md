---
title: Java Generic Sucks
date: 2022-06-22 00:07:56
tags:
---

Java's generics type erasure has been regarded as a failure design since JDK 1.5, but I haven't realized it until today when I run into a serious issue about it.

## The issue

I'm using [night-config](https://github.com/TheElectronWill/night-config) library to parse TOML config file for my program. I parsed and stored a list of `long` from TOML. Right after I got my program up and running, I noticed that `list.contains(x)` failures at random numbers. Those numbers seem to exist in the list and successfully printed out, but I cannot locate the issue based on the information I got.

So I read the TOML parser it self, trying to locate where it handles Integer/Long parsing and found the function that deal with number parsing:

```java
// https://github.com/TheElectronWill/night-config/blob/master/toml/src/main/java/com/electronwill/nightconfig/toml/ValueParser.java#L76
private static Number parseNumber(CharsWrapper valueChars) {
    ...
    long longValue;
    try {
        longValue = Utils.parseLong(numberChars, base);
    } catch (NumberFormatException ex) {
        throw new ParsingException("Invalid integer value: " + valueChars);
    }
    int intValue = (int)longValue;
    if (intValue == longValue) {
        return intValue;// returns an int if it is enough to represent the value correctly
    }
    return longValue;
}
```

Note that: `returns an int if it is enough to represent the value correctly`

So the output list, assumptively `ArrayList<Long>` typed, does consist **both** Integer and Long _if Integer is enought to represent the value_, calling `List.contains` will certainly fail if you pass a long parameter to check whether **the number, which is actually int**, is in the collection.

Having it double-checked by using fastutil's LongArrayList confirmed my thoughts, it fails to construct because there are int values in a marked `ArrayList<Long>` collection.

## Why blame Java's generic design?

In this case, `ArrayList<Long>` is merely a mark that give instructions for compiler to check, but it does not replace the `Object[]` array in the list, nor does it assert runtime type-checking. The issue may absolutely be avoided or easily solved in C#, Rust and other strong-typed generics language.

## Solution

Always use strong-typed fastutil or trove collections for storing primitive values.

Blame Java for it's bad design.

In this case, cast boxed int or long values to Number and cast it back to Long, then store them in a `LongArrayList`.
