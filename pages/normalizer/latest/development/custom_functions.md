---
title: Normalizer Custom Functions
version: latest
permalink: /normalizer_latest_custom_functions.html
toc: true
serviceImage: normalizer/logo.svg
---

## Functions Library

Currently there are custom functions developed for specific purposes. They allow you to process different kind of information.

You can check them at: [Normalizer functions](http://wizzie-io.github.io/normalizer-functions/){:target="_blank"}


## Development

On this section, we are trying to explain how to develop your own functions.
All the function classes implement the [Function](https://github.com/wizzie-io/normalizer/blob/master/functions/src/main/java/io/wizzie/normalizer/funcs/Function.java){:target="_blank"} interface.

If you want to build your own functions, you only need to implement some of the different abstract classes and add the JAR into the `lib` folder inside the normalizer distribution.

### MapperFunction

The base abstract class that you need to implement your own MapperFunction is [MapperFunction class](https://github.com/wizzie-io/normalizer/blob/master/functions/src/main/java/io/wizzie/normalizer/funcs/MapperFunction.java){:target="_blank"}

Using this class you can develop your mappers like:
 * [SimpleMapper](https://github.com/wizzie-io/normalizer/blob/master/functions/src/main/java/io/wizzie/normalizer/funcs/impl/SimpleMapper.java){:target="_blank"}
 * [MaxValueMapper](https://github.com/wizzie-io/normalizer/blob/master/functions/src/main/java/io/wizzie/normalizer/funcs/impl/MaxValueMapper.java){:target="_blank"}
 * [ClassificationMapper](https://github.com/wizzie-io/normalizer/blob/master/functions/src/main/java/io/wizzie/normalizer/funcs/impl/ClassificationMapper.java){:target="_blank"}
 * And more!!

The abstract class has one method:

```java
public KeyValue<String, Map<String, Object>> process(String key, Map<String, Object> value) {
    // some process that return a new KeyValue.
}
```

On each `process` call the normalize gives you a 'key' that is a kafka message key, and 'value' that is the deserialized json. You must return a KeyValue object with the new `key` and new Map instance with the transformations.

### FlatMapperFunction

The base abstract class that you need to implement your own FlatMapperFunction is [FlatMapperFunction class](https://github.com/wizzie-io/normalizer/blob/master/functions/src/main/java/io/wizzie/normalizer/funcs/FlatMapperFunction.java){:target="_blank"}

The FlatMapper allows us to generate zero, one or more message from one message. The method that we need to implement is:

```java
public Iterable<KeyValue<String, Map<String, Object>>> process(String key, Map<String, Object> value) {
   // some process that return a Array, List, or some collection.  
}
```

On each `process` call the normalize gives you a 'key' that is a kafka message key, and 'value' that is the deserialized json. You must return a empty collection or a collection with KeyValue objects.


### MapperStoreFunction

The base abstract class that you need to implement your own MapperStoreFunction is [MapperStoreFunction class](https://github.com/wizzie-io/normalizer/blob/master/functions/src/main/java/io/wizzie/normalizer/funcs/MapperStoreFunction.java){:target="_blank"}

The MapperStore function has the same method that MapperFunction:

```java
public KeyValue<String, Map<String, Object>> process(String key, Map<String, Object> value) {
    // some process that return a new KeyValue.
}
```

The unique difference is that you can use the method `getStore(String storeName)` to get a `KeyValueStore<String, Map<String, V>>`.

 * **Note1:** Also you can check the available store using `getAvailableStores()`.
 * **Note2:** You need to define the store on the properties using the `__STORES` property that is a String JSON Array.

The MapperStore also works with windows, so if you set the property `__WINDOW_TIME_MS` you can implement the method:

```java
    public abstract KeyValue<String, Map<String, Object>> window(long timestamp);
```

The message that are returned by window method are sent to kafka too.


### FilterFunction

The base abstract class that you need to implement your own FilterFunction is [FilterFunction class](https://github.com/wizzie-io/normalizer/blob/master/functions/src/main/java/io/wizzie/normalizer/funcs/FilterFunc.java){:target="_blank"}

```java
    public Boolean process(String key, Map<String, Object> value) {
        // some process that return a boolean
    }
```

## Functions Library

Currently there are custom functions developed for specific purposes. They allow you to process different kind of information.

You can check them at: [Normalizer functions](http://wizzie-io.github.io/normalizer-functions/){:target="_blank"}
