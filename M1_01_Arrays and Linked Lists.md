# Arrays and Linked List

---

## Properties of collections

- Don't have a particular order (so you can't say "give me the 3rd element in this collection")
- Don't have to have objects of the same type

## Properties of lists

- Have an **order** (so you can say things like "give me the 3rd item in the list")
- Have **no fixed length** (you can add or remove elements)

## Arrays

- There is a collection of items

- The items have an order to them

  ### Creation

  When an array is created, it is always given some initial size—that  is, the number of elements it should be able to hold (and how large each  element is). The computer then finds a block of memory and sets aside  the space for the array.

  Importantly, the space that gets set aside is one, continuous block. That is, all of the elements of the array are *contiguous*, meaning that they are all next to one another in memory.

  Another key characteristic of an array is that all of the elements are the same size.

总结：

1. 连续相同大小相同类型的元素
2. 固定内存空间，有索引，支持随机访问
3. 查询：O(1)
4. 插入删除：平均O(N)

## Python List

- **a Python list is essentially implemented like an array** (specifically, it behaves like a *dynamic array*, if you're curious). 
- In particular, **the elements of a Python list are contiguous in memory, and they can be accessed using an index.**

## Linked List

1. 随机存储，不连续
2. 查找是O(n)，插入删除平均O(1)