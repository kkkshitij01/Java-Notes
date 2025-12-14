
1. ==**toCharArray()**== : converts a string to char array
```java
String myStr = "abcde";
char[] myArray = myStr.toCharArray(); 

// output : {'a', 'b', 'c', 'd' ,'e'};
```

2. Converting A **==char Array to String==**
```java 
char ch[] = {'a', 'b', 'c', 'd' ,'e'};
String str = new String(ch);

// Output : "abcde"
```

3.  ==**Regular expression**== to split a string by **variable amounts of whitespace** and convert to array

```java
String input = "     Java is great for regex ";

String[] words = input.trim().split(" +"); // First

String[] words = input.trim().split("\\s+");// Second

// both method works but First fails for inputs like 

String input = " Java is great \t for regex ";


```