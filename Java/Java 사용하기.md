> Java 언어를 사용하면서 자주 사용하게 되는 자료구조, Collection, 알고리즘 등을 예제와 함께 정리하는 곳입니다. Java8버전을 기준으로 합니다.

# Collection
## HashMap
HashMap 자료구조는 말그대로 Hash + Map을 합쳐놓은 것입니다. Hash는 임의의 데이터를 해시 함수라는 알고리즘을 사용해서 고정된 길이의 데이터로 매핑하는 방법입니다. 이러한 해시를 자료구조로 사용하는 이유는 **검색을 O(1) 시간으로 할 수 있기 때문입니다.** 저장할 데이터에 대해 해쉬 함수를 적용하면 항상 같은 값이 나오므로 이를 색인(index)으로 사용하여 바로 접근할 수 있습니다.

Map은 (key, value)의 쌍으로 존재하는 데이터를 저장하는 자료구조입니다. HashMap은 **key 값에 hash를 적용하여 value에 대한 접근을 O(1) 시간에 하기위해 사용합니다.**

Java8 버전 이후부터는 해시 충돌을 Linked List가 아닌 Balanced Tree로 변경하여 최악의 시간복잡도가 O(N)에서 O(logN)으로 향상되었다고 합니다.

아래의 전체 예제 코드는 [여기](https://github.com/CODEMCD/java-example-code/blob/master/java-simple-code/src/test/java/collection/HashMapTest.java)에 있습니다.

### 생성하기

```java
HashMap<String, Integer> personAge = new HashMap<>();
```

### `put()`

```java
public V put(K key, V value)
```

```java
personAge.put("Park", 28);
personAge.put("Kim", 25);
personAge.put("Lee", 27);
```

#### 이미 존재하는 key 값으로 `put()` 하기

```java
personAge.put("Park", 30);

int age = personAge.get("Park");
System.out.println("Park age: " + age);
```

```
Park age: 30
```

이미 존재하는 key 값에 `put()`을 하면 위와 같이 value를 덮어씁니다.

### `get()`

```java
public V get(Object key)
```

```java
// key 값이 존재할 때
int age = personAge.get("Park");

// key 값이 존재하지 않을 때
int age = personAge.get("Jin");  // NullPointerException
```

### `getOrDefault()`

```java
public V getOrDefault(Object key, V defaultValue)
```

key가 존재하지 않는 경우 null이 아닌 특정한 값을 반환하고 싶을 때 사용합니다.

```java
int age = personAge.getOrDefault("Jin", 32);
// age = 32
```

### 순회하기 (`foreach()`)

```java
public void forEach(BiConsumer<? super K,? super V> action)
```

#### key 순회하기

```java
Set<String> keys = personAge.keySet();

for (String key : keys) {
    System.out.println(key);
}

// or
keys.forEach(key -> System.out.println(key));
keys.forEach(System.out::println);              // 위와 같이 동작합니다.(메서드 래퍼런스 사용)
```

#### value 순회하기

```java
Collection<Integer> values = personAge.values();
values.forEach(value -> System.out.println(value));
values.forEach(System.out::println);
```

#### key, value 순회하기

```java
Set<Map.Entry<String, Integer>> entries = personAge.entrySet();

for (Map.Entry<String, Integer> entry : entries) {
    System.out.print("key: "+ entry.getKey());
    System.out.println(", Value: "+ entry.getValue());
}

// or
personAge.forEach((key, value) -> {
    System.out.print("key: "+ key);
    System.out.println(", Value: "+ value);
});
```

### `containsKey()`

```java
public boolean containsKey(Object key)
```

key 값이 존재하는지 확인할 때 사용합니다.

```java
if (personAge.containsKey("Park")) {
    System.out.println("Park 은 존재합니다.");
}
else {
    System.out.println("Park 은 존재하지 않습니다.");
}
```

```
Park 은 존재합니다.
```

### `containsValue()`

```java
public boolean containsValue(Object value)
```

```java
if (personAge.containsValue(32)) {
    System.out.println("나이가 32살인 사람이 존재합니다.");
}
else {
    System.out.println("나이가 32살인 사람이 존재하지 않습니다.");
}
```

```
나이가 32살인 사람이 존재하지 않습니다.
```


### Reference
- [Java8 Oracle Reference - HashMap](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)
- [Java 8 HashMap 퍼포먼스 향상](https://johngrib.github.io/wiki/java8-performance-improvement-for-hashmap/)
- [자바 HashMap을 효과적으로 사용하는 방법](http://tech.javacafe.io/2018/12/03/HashMap/)

## Generic Type
Generic Type은 타입을 일반화하는 것입니다. 즉, 사용하는 곳에서 타입을 자유롭게 설정할 수 있어 코드의 재사용성을 높여줍니다. 그리고 제네릭 타입은 컴파일 시간에 결정되므로 안전하게 사용할 수 있습니다.

- [[JAVA] 제네릭(Generic) 문법 정리](https://cornswrold.tistory.com/180)

## Java8 Stream


# 기타
## 정렬하기
- Java에서는 1(양수)를 반환할 때 교환합니다.
    - C++은 false를 반환할 때 교환함(헷갈리기 쉬움)
- [[Java] Comparable와 Comparator의 차이와 사용법](https://gmlwjd9405.github.io/2018/09/06/java-comparable-and-comparator.html)
## 자료형의 최대값, 최소값

```java
//byte(8비트정수)  
System.out.println("byte 최대값 : " + Byte.MIN_VALUE);  
System.out.println("byte 최대값 : " + Byte.MAX_VALUE);  

//short(16비트정수)  
System.out.println("short 최대값 : " + Short.MIN_VALUE);  
System.out.println("short 최대값 : " + Short.MAX_VALUE);  

//char(16비트 부호없는정수)  
System.out.println("char 최대값 : " + (int)Character.MIN_VALUE);  
System.out.println("char 최대값 : " + (int)Character.MAX_VALUE);  

//int(32비트정수)  
System.out.println("int 최대값 : " + Integer.MIN_VALUE);  
System.out.println("int 최대값 : " + Integer.MAX_VALUE);  

//long(64비트정수)  
System.out.println("long 최대값 : " + Long.MIN_VALUE);  
System.out.println("long 최대값 : " + Long.MAX_VALUE);  
```

```
byte 최대값 : -128
byte 최대값 : 127
short 최대값 : -32768
short 최대값 : 32767
char 최대값 : 0
char 최대값 : 65535
int 최대값 : -2147483648
int 최대값 : 2147483647
long 최대값 : -9223372036854775808
long 최대값 : 9223372036854775807
```

### Reference
- <https://jwchoi85.tistory.com/169>