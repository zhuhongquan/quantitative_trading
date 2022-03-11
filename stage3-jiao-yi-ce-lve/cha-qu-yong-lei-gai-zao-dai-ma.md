---
description: 说明：以下内容来源于网络以及助教个人的理解，并非权威参考，如有与专业资料描述不一致的地方，请以专业资料为准。
---

# 插曲：用类改造代码

## 一、对面向对象的理解

### 面向对象的编程---object oriented programming

简称：OOP，是一种编程的思想。OOP把对象当成一个程序的基本单元，一个对象包含了数据和操作数据的函数。面向对象的出现极大地提高了编程的效率，使其编程的重用性增高。

### python面向对象的重要术语

1. 多态（polymorphism）：一个函数有多种表现形式，调用一个方法有多种形式，但是表现出的方法是不一样的。
2. 继承（inheritance）子项继承父项的某些功能，在程序中表现某种联系。
3. 封装（encapsulation）把需要重用的函数或者功能封装，方便其他程序直接调用。
4. 类：对具有相同数据或者方法的一组对象的集合。
5. 对象：对象是一个类的具体事例。
6. 实例化：实例化是指在面向对象的编程中，把用类创建对象的过程称为实例化（new ...）。
7. 标识：每个对象的实例都需要一个可以唯一标识这个事例的标记。
8. 实例属性：实例属性每个实例各自拥有，相互独立。
9. 实例方法：实例方法也**叫做对象方法**。实例方法是属于类的某个对象的，**仅能被对象调用**。
10. 类属性：属于一个类中所有对象的属性，所有的实例都可以访问类属性，**且访问的类属性是同一个。**
11. 类方法：那些**无须特定的对象实例就能够工作**的从属于类的函数。&#x20;

## 二、封装、继承、多态

### 封装（Encapsulation）

封装，顾名思义就是将内容封装到某个地方，以后再去调用被封装在某处的内容。对于面向对象的封装来说，其实就是**使用构造方法**将内容**封装到对象**中，然后通过对象直接或者self间接获取被封装的内容。

```python
class Animal:
    def __init__(self, name, age, gender):  # 构造函数
        self.name = name
        self.age = age
        self.gender = gender

    def eat(self):
        print("{},{}岁,{},吃奶".format(self.name, self.age, self.gender))

    def drink(self):
        print("{},{}岁,{},喝水".format(self.name, self.age, self.gender))

    def sleep(self):
        print("{},{}岁,{},睡觉".format(self.name, self.age, self.gender))


a = Animal('jack', 10, '男')
a.eat()
a.drink()
a.sleep()

b = Animal('rose', 11, '女')
b.eat()
b.drink()
b.sleep()
```

### 继承（Inheritance）

继承，面向对象中的继承和现实生活中的继承相同，即：子可以继承父的内容。

```python
class Animal:
    def __init__(self, name, age, gender):
        # 定义在这里面的self.xxx变量均为实例变量
        self.name = name
        self.age = age
        self.gender = gender

    def eat(self):
        print("{},{}岁,{},吃奶".format(self.name, self.age, self.gender))

    def drink(self):
        print("{},{}岁,{},喝水".format(self.name, self.age, self.gender))

    def sleep(self):
        print("{},{}岁,{},睡觉".format(self.name, self.age, self.gender))


class Cat(Animal):
    def __init__(self, name, age, gender):
        super().__init__(name, age, gender)  # 继承父类的属性
        self.breed = "猫"  # 定义自己的属性

    def cry(self):  # 定义自己的方法
        print("喵喵喵")


class Dog(Animal):
    def __init__(self, name, age, gender):
        super().__init__(name, age, gender)
        self.breed = "狗"

    def cry(self):
        print("汪汪汪")


c1 = Cat('猫1', 2, "公")
c1.eat()
c1.cry()

c2 = Cat('猫2', 4, "母")
c2.drink()
c2.cry()

d1 = Dog('狗1', 3, "公")
d1.eat()
d1.cry()
```

注意： 关于多继承

* 在Python中，如果父类和子类都重新定义了构造方法init( )，在进行子类实例化的时候，子类的构造方法不会自动调用父类的构造方法，必须在子类中显示调用（`super().init`）。
* Python的类可以继承多个类，Java和C#中则只能继承一个类
* Python的类如果继承了多个类，那么其寻找方法的方式有两种，分别是：深度优先和广度优先
* 当类是经典类时，多继承情况下，会按照深度优先方式查找，当类是新式类时，多继承情况下，会按照广度优先方式查找。

### 多态

**多态**是指**一类事物有多种形态**，比如**动物类，可以有猫，狗，猪**等等。（一个抽象类有多个子类，因而多态的概念依赖于继承）

Python中多态的作用：让具有不同功能的函数可以使用相同的函数名，这样就可以用一个函数名调用不同内容(功能)的函数（**方法重写**）。

```python
class Birds:
    def fly(self):
        print("飞起来了")
        
# 设计两个类Duck和Swan，继承于Birds类，重写fly函数，要求输出“鸭子在水面飞”和“天鹅在空中翱翔”
```

> Java中多态性的表现： 多态性，可以理解为一个事物的多种形态。
>
> 同样python中也支持多态，但是是**有限地支持多态性**，主要是因为python中变量的使用不用声明，所以不存在父类引用指向子类对象的多态体现，同时python不支持重载。在python中多态的使用不如Java中那么明显，所以**python中刻意谈到多态的意义不是特别大**。&#x20;

## 三、代码重构

讲到这里，你应该知道怎么把上次的代码重构为“面向对象”的形式了。
