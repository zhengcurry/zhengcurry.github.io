---
title: kotlin
date: 2021-12-28 10:48:58
tags: kotlin
categories: kotlin

---

### **kotlin 关键字**

1. **sealed 密封类**

   1. 密封类和它的子类必须定义在一个文件中，而在kotlin1.0的时候 密封类的子类必须定义在密封类里
   2. `Sealed types cannot be instantiated` 密封类是不能被初始化的

   ```kotlin
   sealed class CheeseListItem(val name: String) {
       data class Item(val cheese: Cheese) : CheeseListItem(cheese.name)
       data class Separator(private val letter: Char) : CheeseListItem(letter.toUpperCase().toString())
   }
   ```

   
