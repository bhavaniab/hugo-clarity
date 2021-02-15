+++
author = "Bhavani Anantapur Bache"
title = "C++11-Significance of override keyword"
date = "2020-02-04"
description = "C++11-Significance of override keyword"
featured = true
tags = [
    "C++11",
    "Modern C++",
    "featured"
]
categories = [
    "themes",
    "syntax",
]
series = ["Software Engineering"]
aliases = ["migrate-from-jekyl"]
thumbnail = "images/cpp.png"
+++

Polymorphism is one of the most powerful features of Object Oriented Programming. It allows multiple methods to be implemented from a single interface. When multiple methods are defined with same name and different data types, it creates ambiguities during function calls at run-time. Such ambiguities show up as issues when the product is put to production. Override keyword is used to solve such issues.  

<!--more-->

A class allows the programmer to define a concept. If ‘Shape’ is defined as a class, the concept of shape is so abstract that it’s hard to define the implementation of ‘Shape’, or the object for ‘Shape’ class. It’s hard to define the area of ‘Shape’. Therefore, we define derived class ‘Circle’, which extends the class ‘Shape’ and class ‘Sphere’ extends the class ‘Circle’. When the member function area is defined, it can represent the area of a Circle or a Sphere. If the radius is defined as r, the area of circle A<sub>c</sub> is:


$$
 A_c = \pi r^2 
$$

Area of sphere A<sub>s</sub> can be defined as:

$$
 A_s = 4 \pi r^2 
$$


The code can also be accesssed in the [github page]("https://github.com/bhavaniab/sharkCppDevel/blob/shark-devel-branch/cpp11/overrideFinal/shape.cpp"). The C++ code below illustrates the implementation:
```C++
#include<iostream>
using namespace std;
#define pi 3.145926
class Shape
{
    public:
        Shape();
        ~Shape();
        virtual void calculateArea(double )=0;
};

/*
  class Circle final:public Shape
  final keyword is used when I don't want class derived
  to be inherited further
  class Circle final: public Shape
*/
class Circle:public Shape
{
    public:
        int i,j;
        Circle();
        ~Circle();
        /*
          Compiler indicates an error if the datatypes
          do not match. If override keyword is not
          used, then the compiler does not find the implementation 
          of the calculateArea function in the derived 
          class Sphere and calls the Circle
          class virtual function. 
        */
          virtual void calculateArea(double );
        //virtual void calculateArea(double ) override;
};
class Sphere:public Circle
{
    public:
        int x;
        Sphere();
        ~Sphere();
        virtual void calculateArea(int );
};
Shape::Shape()
{
}
Shape::~Shape()
{
}
Circle::Circle()
{
}
Circle::~Circle()
{
}
void Circle::calculateArea(double r)
{
    double area;
    area = (pi)*r*r;
    cout << "Area of the circle with radius " << r << " is " << area << std::endl;
}
Sphere::Sphere()
{
}
Sphere::~Sphere()
{
}
void Sphere::calculateArea(int r)
{
    double area;
    area = 4*(pi)*r*r;
    cout << "Area of the sphere with radius " << r << " is " << area << std::endl;
}
int main()
{
    Shape *p;
    Circle c;
    Sphere s;
    p=&s;
    p->calculateArea(5);

    return 0;
}
```
The function area can be declared in the base class of Shape, and re-implemented in the derived class of Sphere and Circle.  Therefore, virtual function allows multiple implementations to be defined from a single interface. As it’s not possible to define the area for ‘Shape’, the function calcualteArea is declared as the pure virtual function as shown below:

```C++
virtual void calculateArea(double )=0;
```
The program on github illustrates the base class of Shape and derived classes of Sphere and Circle.

In the main function, pointer to class ‘Shape’ *p points to the derived class Sphere object.
```C++
Shape *p;
Circle c;
Sphere s;
p=&s;
p->calculateArea(5);
```
The output of the code is:
```C++
Area of the circle with radius 5 is 78.6481
```
The function calculateArea is class Circle as:
```C++
 virtual void calculateArea(double );
```
an in class Sphere, it’s defined as:
```C++
 virtual void calculateArea(int );
```
The function is re-declared in the derived class with the type int. When the function calculateArea is called with the base class pointer pointing to the derived class object s of type Sphere, the function corresponding to the Circle class is called instead of the function corresponding to the Sphere class. The compiler sees an ambiguity and calls the function implemented in the base class instead of the derived class. This is a common problem in the industry as the classes are implemented by different teams. The code compiles fine, but generates incorrect results during execution. This could cause serious bugs in the code which are quite hard to fix. When the ‘override’ keyword is used in the derived class, the function corresponding to the derived class is called and if there is a mismatch in the data types of the virtual functions in the base class and the derived class, an error is thrown during the compile time.
