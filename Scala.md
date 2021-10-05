```scala
// Constant
val pi = 3.14159

// Range
1 to 10
1.to(10)

// Definitions
def square(x: Double) = x * x
def area(radius: Double): Double = pi * square(radius)

area(10) // => 314.159
```

Conditional operators
```scala
!false        // => true
false && true // => false
false || true // => true
```

```scala
def sqrtIter(guess: Double, x: Double): Double =
  if(isClose(guess, x)) guess
  else sqrtIter(improve(guess, x), x)
```
