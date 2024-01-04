# @property
```python
class Sum():
    def __init__(self,a,b):
        self.a=a
        self.b=b
    @property  # makes functions like getters and access a method like attributes
    def add(self):
        print("Sum = %d" %(self.a+self.b))
    def __call__(self):
        print(f"num1 = {self.a}, num2 = {self.b}") 
    def __del__(self):
        print("delete2")

class Sub(Sum):  # inherited class
    def __init__(self,a,b,c,d):
        super().__init__(a,b)   #initialis a,b
        self.c=c
        self.d=d
    def __call__(self):
        print("num3 = %s, num4 = %s" %(self.c,self.d)) # another way of printing just like C language
    @property
    def sub(self):
        print("Sub = %d" %(self.c-self.d+self.a+self.b))

obj1=Sum(10,20)   
obj1.add    # --(1)
obj2=Sub(10,20,30,40)  # sub class must have it's as well as super class's arguments
obj2.sub   # --(2)
obj1()   # --(3)
obj2()   # --(4)
```
# OUTPUT
```bash
Sum = 30
Sub = 20
num1 = 10, num2 = 20
num3 = 30, num4 = 40
delete
delete2
delete2
```
# Explanation 

It is clear that Sum=30 and Sub=20. In (1) and (2), @property allows methods to be called like attributes of class.
In (3) and (4), When obj1() is called, it automatically calls the __call__ function(else it won't be called). Had we changed the order of highlighted code lines,
we would have gotten a different output sequence.<br> Except __del__ which is called at the end, automatically unless the code doesnt exit in between OR we use del obj1
in between the code. Also, if were creating an instance of Sum and an instance of Sub, both instances have Sum as their base class.<br> When these instances are destroyed (either explicitly
via del or when Python's garbage collector automatically destroys them), the __del__ method of Sum is called, printing "delete2" and net twice.

# @property_name.setter
```python
class Circle:
    def __init__(self, radius):
        self._radius = radius

    @property
    def radius(self):
        return self._radius

   @radius.setter
   def radius(self, value):
       if value < 0:
           raise ValueError("Radius can't be negative")
       self._radius = value
    

c = Circle(5) # creates an instance of Circle of radius 5, also was possible because 5>0, else ValueError would be raised.
c.radius = 10  # OK                                            | These are done to change the radius
c.radius = -1  # Raises ValueError: Radius cannot be negative  | of the circle to some other value
```
# Explanation

Here, property_name is radius. So @radius.setter sets additional logic apart from setting radius when assigned. It checks if radius is following the @radius.setter condition
and if not, raises a ValueError here.<br> This is a powerful tool used to debug and write sometimes shorter but accurate codes.
NOTE: The name of the function "radius" is same under @property and @radius.setter.<br> `AttributeError: property 'radius' of 'Circle' object has no setter` is raised otherwise.


HAD we made the setter condition inside radius function only. Then we would face the below problems.

```python
class Circle:
    def __init__(self, radius):
        self._radius = radius

    @property
    def radius(self):
        if self._radius < 0:
            raise ValueError("Radius cannot be negative")
        return self._radius

c = Circle(-5)  # No error is raised
print(c.radius)  # Raises ValueError: Radius cannot be negative
```
```
Here, we see radius to be setted -5 but then when changed, gives error. In code before we were not even allowed to set inital radius to negative value.
The @radius.setter decorator allows us to define a method that changes the value of the radius property. When you use c.radius = 10,
Python calls this setter method with 10 as the argument. VERY USEFUL!
```
# property_name.deleter
@property_name.deleter: This is used to declare a method as the deleter of a property. The method must have the same name as the getter method.<br>
FOR EXAMPLE:
```python
class Circle:
    def __init__(self):
        self._radius = 0

    @property
    def radius(self):
        return self._radius

    @radius.setter
    def radius(self, value):
        self._radius = value

    @my_prop.deleter
    def radius(self):
        del self._radius
```
# Notice:

We use `_radius` instead of `radius` in `__init__` and other methods. In the Circle class, self._radius is used to store the actual data for the radius of the circle.
The radius property (defined with the @property decorator) is a way to control access to this internal _radius variable.When you use self._radius in the __init__ method,
you're setting the initial value of the _radius variable directly. If you used self.radius instead, you would be calling the radius setter method (if one exists).
