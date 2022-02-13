---
layout: post
title: Traversing an array without using length property
date: 2018-08-05 00:53:49.000000000 +05:30
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Java
- Miscellaneous
tags:
- Binary Search
- Java
meta:
  _edit_last: '1'
  _wp_old_slug: traversing-an-array-without-length-property
author: ankit_katiyar
permalink: "/traversing-an-array-without-using-length-property/"
---

We often face an interview question when it is asked to search or traverse an array without knowing/using the length property. In Java, if you try to access the element that is outside the length of the array.

it will give you an exception as [**java.lang.ArrayIndexOutOfBoundsException**](https://docs.oracle.com/javase/7/docs/api/java/lang/ArrayIndexOutOfBoundsException.html).


Here I have one implementation&nbsp;of a binary search where I will not use the length property of the array.

### Example
```java
import static java.lang.Math.pow;

public class BinarySearchWithoutLength {

  public static Integer binarySearch(final Integer[] elements, final Integer find) {
    /*
     * Start looking for element at the 0th position and move in multiple of 2. like 2^0, 2^1, 2^2,
     * 2^3
     */
    Integer index = 0;
    Integer exp = 0; // initial exponent value will be 0
    try {
      while (true) {
        // check if we found the element at 2^exp index
        if (elements[index] == find) {
          return index;
        }
        if (elements[index] < find) {
          index = (int) pow(2, exp);
          exp += 1;
        } else {
          break;
        }
      }
    } catch (ArrayIndexOutOfBoundsException e) {
      /*
       * ignore this exception as we expect it to happen if we go beyond the size of the array
       * without finding element at position 2^exp
       */
    }
    int mid; // it will represent mid value where lookup will happen
    int left = (index / 2) + 1;
    int right = index - 1;
    /*
     * we will start our lookup from the middle of the array and then shift towards other half of
     * the array.
     */
    while (left <= right) {
      mid = ((right + left) / 2);
      try {
        if (elements[mid] == find) {
          /*
           * As we have found one matching element. Stop
           */
          return mid;
        } else {
          if (elements[mid] > find) {
            /* whenever you find and element which is greater than the find */
            right = mid - 1;
          } else {
            left = mid + 1;
          }
        }
      } catch (ArrayIndexOutOfBoundsException e) {
        /*
         * we got this because we reached beyond the size, shift our right index to one place left
         * of the mid
         */
        right = mid - 1;
      }
    }
    return -1;
  }

  public static void main(String[] args) {
    Integer[] sample_1 = new Integer[] {-3, -2, 3, 4, 7};
    Integer[] sample_2 = new Integer[] {-3, -2, -1, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13};
    Integer[] sample_3 = new Integer[] {1, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13};
    System.out.println("Index of 4: " + binarySearch(sample_1, 4));
    System.out.println("Index of 4: " + binarySearch(sample_2, 4));
    System.out.println("Index of 4: " + binarySearch(sample_3, 10));
    System.out.println("Index of 4: " + binarySearch(sample_3, 0));
  }
}
```

[Download from GitHub](https://github.com/ankitkatiyar91/bytefold/blob/master/java/BinarySearchWithoutLength.java)

