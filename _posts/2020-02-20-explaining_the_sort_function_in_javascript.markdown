---
layout: post
title:      "Explaining The Sort() Function In Javascript"
date:       2020-02-20 17:05:16 -0500
permalink:  explaining_the_sort_function_in_javascript
---

The sort() method in javascript is a class method of the Array class that can be called on an array to sort elements either numerically as integers or alphabetically as strings. The sort() method changes the original array destructively, meaining that the original array will be mutated as opposed to returning a new array. 

The sort() method also takes an optional argument of a function that defines an alternative order rather than the default numerical or alphabetical order.

Example:

```
//return the original array sorted in ascending order based on number of characters:

const array = ["bannana", 'apple', 'orange']

array.sort(function(element1, element2){
  return element1.length - element2.length
})

 array //=> ['apple', 'orange', 'bannana']


```







The function passed in as a parameter should return a negative integer, 0, or a positive integer. Element1 and element2 refer to the array elements being compared and sort() will iterate over the entire collection in this manner. If the function returns positive, element1 will be ordered after element2, and if negative element1 will be ordered before element2. If/Else statments can also be utilized to return a positive or negative integer in the event that you want to sort based on  an element characteristic that does not involve numeric values, like so:

```
//sort this array so that all string elements eqaul to 'blue' are moved to the end of the array: 

const array = ["blue", "orange", "green", "red", "blue", "yellow", "blue"];

 array.sort(function(element1, element2) {
  if (element1 === "blue") {
    return 1;
  } else {
    return -1;
  }
});

=> ["yellow", "red", "green", "orange", "blue", "blue", "blue"]
```

As you can see, the sort() method has many different uses and is an essential tool for any developer.




