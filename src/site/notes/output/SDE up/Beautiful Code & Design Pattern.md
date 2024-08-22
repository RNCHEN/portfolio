---
{"dg-publish":true,"dg-permalink":"SDE/2024/design-pattern","permalink":"/SDE/2024/design-pattern/","title":"Design Pattern","dgHomeLink":true,"dgShowBacklinks":true,"dgShowFileTree":true,"dgEnableSearch":true,"dgShowToc":true,"dgShowTags":true}
---


# loop Array

## Single element 

易读性 
![更加美观的代码-20240715173245839.webp](/img/user/attachments/%E6%9B%B4%E5%8A%A0%E7%BE%8E%E8%A7%82%E7%9A%84%E4%BB%A3%E7%A0%81-20240715173245839.webp)


尽量用自带的处理数组的解构 why: focus on the element





JavaScript’s `forEach()` method on arrays can provide both the element and its index:

```
const elements = ['a', 'b', 'c', 'd'];
elements.forEach((element, index) => {
    console.log(`${index}: ${element}`);
});

```


 **Python**

In Python, you can use the `enumerate()` function to get both the index and the value when iterating:

```
elements = ['a', 'b', 'c', 'd']
for index, element in enumerate(elements):
    print(f"{index}: {element}")

```


Java 

```
String[] elements = {"a", "b", "c", "d"};
int index = 0;
for (String element : elements) {
    System.out.println(index + ": " + element);
    index++;
}

```


## Consecutive elements

handle consecutive elements
### 1. **Python**

Python provides a simple and clear way to iterate through pairs of consecutive items using a combination of `zip` with slicing:

```python
pythonCopy code
elements = [1, 2, 3, 4, 5]
for current, next_item in zip(elements, elements[1:]):
    print(f"Current: {current}, Next: {next_item}")

```

This method is elegant and takes advantage of Python's powerful slicing and iteration capabilities.

### 2. **JavaScript**

In JavaScript, you can use a traditional `for` loop to handle consecutive elements since JavaScript does not have built-in tuple unpacking or a direct equivalent to Python's `zip`:

```jsx
javascriptCopy code
const elements = [1, 2, 3, 4, 5];
for (let i = 0; i < elements.length - 1; i++) {
    console.log(`Current: ${elements[i]}, Next: ${elements[i + 1]}`);
}

```

This approach is straightforward and efficient, suitable for most cases where you need to process elements in pairs.

### 3. **Java**

Java typically uses a traditional `for` loop for such tasks:

```java
javaCopy code
int[] elements = {1, 2, 3, 4, 5};
for (int i = 0; i < elements.length - 1; i++) {
    System.out.println("Current: " + elements[i] + ", Next: " + elements[i + 1]);
}

```

This method is universally applicable in Java for any array or list where you need to access consecutive elements.





# 迭代器模式 

感觉很受用 



微服务等软件架构 
https://crimsonromance.github.io/2019/03/23/%E5%9B%9B%E7%A7%8D%E8%BD%AF%E4%BB%B6%E6%9E%B6%E6%9E%84%EF%BC%9A%E5%8D%95%E4%BD%93%E6%9E%B6%E6%9E%84%E3%80%81%E5%88%86%E5%B8%83%E5%BC%8F%E6%9E%B6%E6%9E%84%E3%80%81%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9E%B6%E6%9E%84%E3%80%81Serverless%E6%9E%B6%E6%9E%84/