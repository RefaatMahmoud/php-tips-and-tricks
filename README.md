### (1) compute execution time among (for - foreach - while - do while - iterator) :rocket:
```php
<?php

// An array of 100 units with random string values
for ($i = 0; $i <= 100; $i++) {
    $units[] = rand(1, 9);
}

// for loop performance
function getForLoopTime($units)
{
    $startingTime = microtime(true);
    for ($i = 0; $i < count($units); $i++) {
    }
    $endingTime = microtime(true);
    return $endingTime - $startingTime;
}


// foreach loop performance
function getForEachLoopTime($units)
{
    $startingTime = microtime(true);
    foreach ($units as $unit) {
    }
    $endingTime = microtime(true);
    return $endingTime - $startingTime;
}


// while loop performance
function getWhileLoopTime($units){
    $startingTime = microtime(true);
    $i = 0;
    while ($i < count($units)) {
        $i++;
    }
    $endingTime = microtime(true);
    return $endingTime - $startingTime;
}



// do while loop performance
function getDoWhileLoopTime($units){
    $startingTime = microtime(true);
    $i = 0;
    do {
        $i++;
    } while ($i < count($units));
    $endingTime = microtime(true);
    return $endingTime - $startingTime;
}



// iterator loop performace
function getIteratorLoopTime($units){
    $startingTime = microtime(true);
    $arrayObj = new ArrayObject($units);
    $arrayIterator = $arrayObj->getIterator();
    while ($arrayIterator->valid()) {
        $arrayIterator->next();
    }
    $endingTime = microtime(true);
    return  $endingTime - $startingTime;
}



print_r("for loop evaluates to: " . number_format(getForLoopTime($units) * 1000, 3) . "<br>");
print_r("foreach loop evaluates to: " . number_format(getForEachLoopTime($units) * 1000, 3) . "<br>");
print_r("while loop evaluates to: " . number_format(getWhileLoopTime($units) * 1000, 3) . "<br>");
print_r("do while loop evaluates to: " . number_format(getDoWhileLoopTime($units) * 1000, 3) . "<br>");
print_r("Array Iterator evaluates to: " . number_format(getIteratorLoopTime($units) * 1000, 3) . "<br>");
``` 
**Output**
```
for loop evaluates to: 0.035
foreach loop evaluates to: 0.027
while loop evaluates to: 0.031
do while loop evaluates to: 0.031
Array Iterator evaluates to: 0.078
```
From Results show that 
1. foreach
2. while - do while
3. for
4. iterator

### (2) Forwarding and non-forwarding calls
Late static bindings' resolution will stop at a fully resolved static call with no fallback. On the other hand, static calls using keywords like parent:: or self:: will forward the calling information.
```php
<?php
class A {
    public static function foo() {
        static::who();
    }

    public static function who() {
        echo __CLASS__."\n";
    }
}

class B extends A {
    public static function test() {
        A::foo();
        parent::foo();
        self::foo();
    }

    public static function who() {
        echo __CLASS__."\n";
    }
}
class C extends B {
    public static function who() {
        echo __CLASS__."\n";
    }
}

C::test();
?>
```
**Output**
```
A C C
```
### (3) Difference between ```__CLASS__ ``` , ``` get_class()``` and ```get_called_class()```
magic constant ```__CLASS__``` returns the same result as ```get_class()``` function called without parameters and they
both return the name of the class where it was defined (i.e. where you wrote the function call/constant name ).
In contrast, ```get_class($this)``` and ```get_called_class()``` functions call, will both return the name of the actual class
which was instantiated:
```php
<?php
class Definition_Class {
public function say(){
echo '__CLASS__ value: ' . __CLASS__ . "\n";
echo 'get_called_class() value: ' . get_called_class() . "\n";
echo 'get_class($this) value: ' . get_class($this) . "\n";
echo 'get_class() value: ' . get_class() . "\n";
}
}
class Actual_Class extends Definition_Class {}
$c = new Actual_Class();
$c->say();
```
**Output**
```
__CLASS__ value: Definition_Class
get_called_class() value: Actual_Class
get_class($this) value: Actual_Class
get_class() value: Definition_Class
