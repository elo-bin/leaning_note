# php魔术方法笔记
---
php中把以两个下划线__开头的方法叫做魔术方法，这些方法在php中的作用举足轻重。
#### 1. 类的构造函数 __construct()
php对象创建后第一个被调用的函数。在每个类中都有一个构造函数，可以自定义。如果没有显示声明它，则会默认存在一个没有参数并且内容为空的构造方法。  
1. 作用：通常用来执行初始化任务，比如对成员对象属性在创建对象时赋予初值。
2. 在类中的声明格式
``` php
public function __construct([参数值了列表]){
    //需要执行的操作，通常来说是对闯进来的参数进行初始化
}
```
3. 注意事项
    1. 在同一个类里面只能有一个构造函数；
    2. 构造函数是以两个英文的下划线__开始的。
4. 例如：
```php
<?php
class Car_Wheelsize
{
    public $Nose_Wheelsize;
    private $Rear_Wheelsize;
    public function __construct($NoseWheelsize,$RearWheelsize)
    {
        $this->Nose_Wheelsize=$NoseWheelsize;
        $this->Rear_Wheelsize=$RearWheelsize;
        echo "完成了初始化！";
    }
    public function show()
    {
        echo "你的前轮的尺寸是：". $this->Nose_Wheelsize;
        echo "你的后轮的尺寸是：". $this->Rear_Wheelsize;
    }
}
$p=new Car_Wheelsize(2,3);
$p->show();
?>//运行后输出：完成了初始化！你的前轮的尺寸是：2你的后轮的尺寸是：3
```
可以看到在定义对象时就调用了__construct()方法。
### 2. 类的析构函数__destruct()
php的析构函数允许在销毁类之前执行一些操作，比如释放变量空间、关闭文件和释放结果集等。是php5才引进的方法。
1. 作用：在销毁类之前进行一些额外的操作
2. 在类中的声明格式
``` php
    public function __destruct(){
        //要执行的操作
    }
```
3. 注意事项
    1. 析构函数与构造函数一样，同一个类中只能有一个；
    2. 析构函数不能带任何参数；
    3. 在类中定义了析构函数后，不管我们是否手动调用，系统都会在最后调用析构函数。
4. 例如：
```php
<?php
class Car_Wheelsize
{
    public $Nose_Wheelsize;
    private $Rear_Wheelsize;
    public function __construct($NoseWheelsize,$RearWheelsize)
    {
        $this->Nose_Wheelsize=$NoseWheelsize;
        $this->Rear_Wheelsize=$RearWheelsize;
        echo "完成了初始化！";
    }
    public function show()
    {
        echo "你的前轮的尺寸是：". $this->Nose_Wheelsize;
        echo "你的后轮的尺寸是：". $this->Rear_Wheelsize;
    }
    public function __destruct()
    {
        echo "调用了析构函数！";
    }
}
$p=new Car_Wheelsize(2,3);
$p->show();
?>//最后输出：完成了初始化！你的前轮的尺寸是：2你的后轮的尺寸是：3调用了析构函数！
```
可以看到最后我们没有手动调用析构函数，但是项目自动在最后调用了
### 3. __call(),在对象中调用一个不可访问方法时调用
该方法有两个参数，第一个参数会自动接受不存在的方法名，第二个参数会以数组的形式接受不存在的方法的多个参数。
1. 作用：用于提醒开发人员使用了为在类中声明的方法，提高工作效率。（若没有\_\_call();方法，那么程序执行到不存在的方法处时就会报错，使用了该方法则会调用\_\_call();方法，然后继续执行后面的语句，减少了编译时间。）
2. 在类中的声明格式
```php
    public function __call($function_name,$arguments){
        //执行的操作，直接写提醒就行
    }
```
3. 例如：
```php
<?php
class Car_Wheelsize
{
    public $Nose_Wheelsize;
    private $Rear_Wheelsize;
    public function __construct($NoseWheelsize,$RearWheelsize)
    {
        $this->Nose_Wheelsize=$NoseWheelsize;
        $this->Rear_Wheelsize=$RearWheelsize;
        echo "完成了初始化！";
    }
    public function show()
    {
        echo "你的前轮的尺寸是：". $this->Nose_Wheelsize;
        echo "你的后轮的尺寸是：". $this->Rear_Wheelsize;
    }
    public function __call($name, $arguments)
    {
        echo "调用了不存在的方法：".$name."参数：";
        print_r($arguments);
    }
}
$p=new Car_Wheelsize(2,3);
$p->say(200,300);
$p->show();
?>/*最后输出：完成了初始化！调用了不存在的方法：say参数：Array
(
    [0] => 200
    [1] => 300
)
你的前轮的尺寸是：2你的后轮的尺寸是：3 */
```
可以看到定义了\_\_call()方法之后，遇到不存在的方法时会自动调用\_\_call()方法，然后继续执行后面的代码。
### 4. __callStatic(),用静态方式去调用一个不可访问的方法时调用
与__call()方法功能除了callStatic()是静态调用外，其他都是一样的。此处就不做过多说明。
### 5. __get(),获得一个类的成员变量时调用
php编程中，如果类的成员属性被设置为private之后。我们想在外面访问它就会出现“不能访问私有成员”的错误。为了解决它，我们引进了__get()方法。
1. 作用：在类的外部访问私有成员
2. 在类中的声明格式
```php
    public function __get($name){
        //要执行的操作
    }
```
3. 例如：
```php
<?php
class Car_Wheelsize
{
    public $Nose_Wheelsize;
    private $Rear_Wheelsize;
    public function __construct($NoseWheelsize,$RearWheelsize)
    {
        $this->Nose_Wheelsize=$NoseWheelsize;
        $this->Rear_Wheelsize=$RearWheelsize;
        echo "完成了初始化！";
    }
    public function __get($name)
    {
        echo "调用了__get()方法！";
        if($name=="Nose_Wheelsize"){
            return $this->Nose_Wheelsize;
        }
        else{
            return $this->Rear_Wheelsize;
        }
    }
}
$p=new Car_Wheelsize(2,3);
echo $p->Nose_Wheelsize.",";
echo $p->Rear_Wheelsize;
?>//最后输出：完成了初始化！2,调用了__get()方法！3
```
可以看到在外部访问私有成员时，调用了__get()方法访问到了原本不能再类外访问的成员。
### 6. __set(),设置一个类的成员变量时调用
1. 作用：给一个未定义的私有属性赋值时，此方法会被触发，传递的参数时被设置的属性名和值。此方法还可用于在类的外部设置不可访问的属性的值。
2. 在类中的定义格式
```php
    public function __set($property,$value){
        //要执行的操作
    }
```
3. 例如：
```php
<?php
class Car_Wheelsize
{
    public $Nose_Wheelsize;
    private $Rear_Wheelsize;
    public function __construct($NoseWheelsize,$RearWheelsize=3)
    {
        $this->Nose_Wheelsize=$NoseWheelsize;
        $this->Rear_Wheelsize=$RearWheelsize;
        echo "完成了初始化！";
    }
    public function __get($name)
    {
        echo "调用了__get()方法！";
        if($name=='Rear_Wheelsize'){
            return $this->Rear_Wheelsize;
        }
        else{
            return $this->Nose_Wheelsize;
        }
    }

    public function __set($name, $value)
    {
        echo "调用了__set()方法！";
        $this->$name=$value;
    }
}
$p=new Car_Wheelsize(2,3);
$p->Rear_Wheelsize=10;
$p->Nose_Wheelsize=20;
echo $p->Nose_Wheelsize.",";
echo $p->Rear_Wheelsize;
?>//输出：完成了初始化！调用了__set()方法！20,调用了__get()方法！10
```
可以看到在类的外部对私有属性值赋值时调用了__set()方法。
### 7. __isset(),当对不可访问属性调用isset()或者empyy()时调用
1. \_\_isset()作用：当变量或者对象的属性不存在或者被设置为NULL时，返回false,其他时候返回true.一般用于判断某个变量是否被设置或者在类的外部判断某个属性名是否被设置。
2. \_\_isset()作用：在类的外部，isset()函数不能用来判断私有属性是否存在。需要在类的内部定义\_\_isset()魔术方法。当定义了\_\_isset()的魔术方法后，只要使用isset()函数判断类的私有属性或者未定义的属性，都会调用\_\_isset()方法。并且返回值和\_\_isset()的返回值一致。
3. 在类中的原型：
```php
public function __isset(){
    //要执行的操作
}
```
4. 例如：
```php
<?php
class Car_Wheelsize
{
    public $Nose_Wheelsize;
    private $Rear_Wheelsize;
    public function __construct($NoseWheelsize,$RearWheelsize=3)
    {
        $this->Nose_Wheelsize=$NoseWheelsize;
        $this->Rear_Wheelsize=$RearWheelsize;
        echo "完成了初始化！";
    }
    public function __isset($name)
    {
        echo "调用了__isset()方法！";
    }
}
$p=new Car_Wheelsize(2,3);
var_dump(isset($p->Nose_Wheelsize));
var_dump(isset($p->Rear_Wheelsize));
?>/*输出：
完成了初始化！bool(true)
调用了__isset()方法！bool(false)*/
```
可以看到在对私有属性$Rear_Wheelsize使用isset()函数进行判断之前调用了__siset()方法。
### 8. __unset(),当对不可访问属性调用unset()时被调用
1. 作用：当在类的外部调用unset()函数删除对象的属性时，不能删除在类的外部无法访问的属性。这时就需要在类的内部提供__unset()方法，当使用unset()函数删除不能访问的属性时自动调用它来帮助我们删除。
2. 在类中的原型：
```php
    public function __unset($content){
        //要执行的操作
    }
```
3. 例如：
```php
<?php
class Car_Wheelsize
{
    public $Nose_Wheelsize;
    private $Rear_Wheelsize;
    public function __construct($NoseWheelsize,$RearWheelsize=3)
    {
        $this->Nose_Wheelsize=$NoseWheelsize;
        $this->Rear_Wheelsize=$RearWheelsize;
        echo "完成了初始化！";
    }
    public function __unset($name)
    {
        echo "调用了__unset()方法！";
    }
}
$p=new Car_Wheelsize(2,3);
unset($p->Rear_Wheelsize);
?>//输出：完成了初始化！调用了__unset()方法！
```
可以看到初始化之后调用unset()函数删除私有属性时调用了__unset()方法。
### 9. __sleep(),执行serialize()时，会先调用这个函数
1. 作用：当调用serilize()函数时会检查类中是否有__sleep()方法。如果有该方法，则该方法会被优先调用并返回一个包含对象中所有应被序列化的变量名称的数组。如果该方法未返回任何内容，则NULL被序列化，并且产生一个E_NOTICE级别的错误。
2. 在类中的原型
```php
    public function __sleep(){
        //要执行的操作
        return arry('需要序列化的变量名1','需要序列化的变量名2','...');//这里一定要返回一个数组，数组里面的元素是需要序列化的变量的名字
    }
```
3. 例如：
```php
<?php
class Car_Wheelsize
{
    public $Nose_Wheelsize;
    private $Rear_Wheelsize;
    public function __construct($NoseWheelsize,$RearWheelsize=3)
    {
        $this->Nose_Wheelsize=$NoseWheelsize;
        $this->Rear_Wheelsize=$RearWheelsize;
        echo "完成了初始化！";
    }
    public function __sleep()
    {
        echo "当使用serialize()时自动调用!";
        return array('Rear_Wheelsize','Nose_Wheelsize');
    }
}
$p=new Car_Wheelsize(2,3);
echo serialize($p);
?>//输出：完成了初始化！当使用serialize()时自动调用!O:13:"Car_Wheelsize":2:{s:29:" Car_Wheelsize Rear_Wheelsize";i:3;s:14:"Nose_Wheelsize";i:2;}
```
可以看到在调用序列化函数serialize()之前调用了__sleep()方法。
### 10. __wakeup(),执行unserialize()时，会先调用这个函数。
1. 作用：与\_\_sleep()方法类似，在执行unserialize()函数时会检查类中是否存在一个\_\_wakeup()方法，如果存在，则先调用它。需要注意的是，\_\_wakeup()方法不需要返回数组。
2. 在类中的原型
```php
    public function __wakeup(){
        //需要执行的操作
    }
```
3. 例如：
```php
<?php
class Car_Wheelsize
{
    public $Nose_Wheelsize;
    private $Rear_Wheelsize;
    public function __construct($NoseWheelsize,$RearWheelsize=3)
    {
        $this->Nose_Wheelsize=$NoseWheelsize;
        $this->Rear_Wheelsize=$RearWheelsize;
        echo "完成了初始化！";
    }
    public function __sleep()
    {
        echo "当使用serialize()时自动调用!";
        return array('Rear_Wheelsize','Nose_Wheelsize');
    }
    public function __wakeup()
    {
        echo "当使用unserialize()函数时调用！";
        $this->Nose_Wheelsize=80;
        $this->Rear_Wheelsize=90;
    }
}
$p=new Car_Wheelsize(2,3);
var_dump(serialize($p));
var_dump(unserialize(serialize($p)));
?>/*输出：完成了初始化！当使用serialize()时自动调用!string(92) "O:13:"Car_Wheelsize":2:{s:29:" Car_Wheelsize Rear_Wheelsize";i:3;s:14:"Nose_Wheelsize";i:2;}"
当使用serialize()时自动调用!当使用unserialize()函数时调用！object(Car_Wheelsize)#2 (2) {
  ["Nose_Wheelsize"]=>
  int(80)
  ["Rear_Wheelsize":"Car_Wheelsize":private]=>
  int(90)
}*/
```
可以看到在调用unserialize()函数时先调用了__wakeup()方法。
### 11. __toString(),类被当成字符串时的回应方法。
1. 作用：当类被我们当成字符串使用时，如果没有\_\_toString()方法。那么会产生一个'E_RECOVERABLE_ERROR'级别的错误。定义\_\_toString()方法可以返回一个字符串提示我们错把类当成了字符串，提高了工作效率。要注意的是该方法必须返回一个字符串。
2. 在类中的原型
```php
    public function __toString(){
        //要进行的操作
        return "想要出现的提示语;";
    }
```
3. 例如：
```php
<?php
class Car_Wheelsize
{
    public $Nose_Wheelsize;
    private $Rear_Wheelsize;
    public function __construct($NoseWheelsize,$RearWheelsize=3)
    {
        $this->Nose_Wheelsize=$NoseWheelsize;
        $this->Rear_Wheelsize=$RearWheelsize;
        echo "完成了初始化！";
    }
public function __toString()
{
    $this->Nose_Wheelsize=20;
    return "把类当成字符串啦麻瓜！";
}

}
$p=new Car_Wheelsize(2,3);
echo $p;
echo $p->Nose_Wheelsize;
?>//输出：完成了初始化！把类当成字符串啦麻瓜！20
```
可以看到当直接将类的对象当作字符串使用时调用了__toString()方法。
### 12. __invoke()，用调用函数的方法调用一个对象时的回应方法
1. 作用：当用调用函数的方法调用一个对象时，该方法自定被调用用于提醒开发者。
2. 在类中的原型:
```php
    public function __invoke(){
        //要执行的操作
    }
```
3. 例如：
```php
<?php
class Car_Wheelsize
{
    public $Nose_Wheelsize;
    private $Rear_Wheelsize;
    public function __construct($NoseWheelsize,$RearWheelsize=3)
    {
        $this->Nose_Wheelsize=$NoseWheelsize;
        $this->Rear_Wheelsize=$RearWheelsize;
        echo "完成了初始化！";
    }
    public function __invoke()
    {
        echo "这是个对象我的老天！";
    }

}
$p=new Car_Wheelsize(2,3);
$p();
?>//输出：完成了初始化！这是个对象我的老天！
```
可以看到当用调用函数的方法调用类的对象时，__invoke()方法被自动调用
### 13. __clone(),当对象复制完成时完成调用
1. 作用：一般情况下我们不需要完全复制一个随心来获得其中属性，但是在某些特殊情况下需要复制。对象的复制通过关键字clone来完成（如果类中定义了\_\_clone()方法，将调用该方法），对象中的\_\_clone()方法不能直接调用。
2. 在类中的原型
```php
    public function __clone(){
        //要执行的操作
    }
```
3. 例如：
```php
<?php
class Car_Wheelsize
{
    public $Nose_Wheelsize;
    private $Rear_Wheelsize;
    public function __construct($NoseWheelsize,$RearWheelsize=3)
    {
        $this->Nose_Wheelsize=$NoseWheelsize;
        $this->Rear_Wheelsize=$RearWheelsize;
        echo "完成了初始化！";
    }
    public function __clone()
    {
        echo "你正在进行克隆操作！";
    }

}
$p=new Car_Wheelsize(2,3);
$p1=clone $p;
var_dump('$p:');
var_dump($p);
var_dump('$p1:');
var_dump($p1);
?>/*输出：
完成了初始化！你正在进行克隆操作！str(3) "$p:"
object(Car_Wheelsize)#1 (2) {
  ["Nose_Wheelsize"]=>
  int(2)
  ["Rear_Wheelsize":"Car_Wheelsize":private]=>
  int(3)
}
string(4) "$p1:"
object(Car_Wheelsize)#2 (2) {
  ["Nose_Wheelsize"]=>
  int(2)
  ["Rear_Wheelsize":"Car_Wheelsize":private]=>
  int(3)
}*/
```
可以看到当使用clone关键字克隆对象$P时，__clone()方法被调用