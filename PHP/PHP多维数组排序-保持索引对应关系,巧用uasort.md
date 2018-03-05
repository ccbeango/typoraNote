

# PHP 多维数组排序 保持索引对应关系 巧用 uasort

​	实际开发中，多多少少都会遇到数组的排序问题，除了常规的写简单的排序算法，PHP 还提供了内置数组排序函数，本次重点分享一下：[uasort](http://www.php.net/manual/zh/function.uasort.php)  使用用户自定义的比较函数对数组中的值进行排序并保持索引关联，可排序多维数组，本文重点讲解此函数。 

**uasort 函数**

​	参数类型：bool uasort ( array &$array, callable $cmp_function)

​	本函数对数组排序并保持索引和单元之间的关联。

​	主要用于对那些单元顺序很重要的结合数组进行排序。比较函数是用户自定义的。

​	成功时返回 TRUE， 或者在失败时返回 FALSE。 

**数组排序实例（非class中）：**

```php
/**
* 自定义排序函数
* @param $param1
* @param $param2
* @return 0(不移动) 1(正向调换顺序) -1(逆向调换顺序)
*/
function my_sort($param1, $param2){
    if($param1 == $param2) return 0;
    else return $param1 > $param2 ? 1 : -1;
}
$arr = array(
            'a'=>'20',
            'b'=>'1',
            'c'=>'10',
            'd'=>'5',
            'e'=>'21',
            'f'=>'4',
            'g'=>'3',
        );
uasort($arr, 'my_sort');
var_dump($arr);
/*输出值
array (size=7)
  'b' => string '1' (length=1)
  'g' => string '3' (length=1)
  'f' => string '4' (length=1)
  'd' => string '5' (length=1)
  'c' => string '10' (length=2)
  'a' => string '20' (length=2)
  'e' => string '21' (length=2)
*/

```



**多维数组排序实例（非class中）：**

```php
/**
* 自定义排序函数
* @param $param1
* @param $param2
* @return 0(不移动) 1(正向调换顺序) -1(逆向调换顺序)
*/
function my_sort($param1, $param2){
    if($param1['value'] == $param2['value']) return 0;
    else return $param1['value'] > $param2['value'] ? 1 : -1;
}
$arr = array(
            'a'=>array('key'=>'定义1', 'value'=>'20'),
            'b'=>array('key'=>'定义2', 'value'=>'1'),
            'c'=>array('key'=>'定义3', 'value'=>'10'),
            'd'=>array('key'=>'定义4', 'value'=>'5'),
            'e'=>array('key'=>'定义5', 'value'=>'21'),
            'f'=>array('key'=>'定义6', 'value'=>'4'),
            'g'=>array('key'=>'定义7', 'value'=>'3'),
        );
uasort($arr, 'my_sort');
var_dump($arr);
/*输出值
array (size=7)
  'b' =>
    array (size=2)
      'key' => string '定义2' (length=7)
      'value' => string '1' (length=1)
  'g' =>
    array (size=2)
      'key' => string '定义7' (length=7)
      'value' => string '3' (length=1)
  'f' =>
    array (size=2)
      'key' => string '定义6' (length=7)
      'value' => string '4' (length=1)
  'd' =>
    array (size=2)
      'key' => string '定义4' (length=7)
      'value' => string '5' (length=1)
  'c' =>
    array (size=2)
      'key' => string '定义3' (length=7)
      'value' => string '10' (length=2)
  'a' =>
    array (size=2)
      'key' => string '定义1' (length=7)
      'value' => string '20' (length=2)
  'e' =>
    array (size=2)
      'key' => string '定义5' (length=7)
      'value' => string '21' (length=2)
*/

```



**class中排序，为了方便以二维数组为例：**

​	uasort($arr1, array($this, 'public_my_sort'));

​	uasort($arr2, array('self', 'self_my_sort'));

```php
class myClassSort{
    
    /**
     * 排序主方法
     * @param $arr1  self静态排序
     * @param $arr2  this排序
     * @return 排序后的数组
     */
    public function main($arr1 = array(), $arr2 = array()){
    
        uasort($arr1, array($this, 'public_my_sort'));
        
        uasort($arr2, array('self', 'self_my_sort'));
        
        return array('arr1'=>$arr1, 'arr2'=>$arr2);
    
    }
    
    /**
     * 自定义排序函数
     * @param $param1
     * @param $param2
     * @return 0(不移动) 1(正向调换顺序) -1(逆向调换顺序)
     */
    private static function self_my_sort($param1, $param2){
        if($param1['value'] == $param2['value']) return 0;
        else return $param1['value'] > $param2['value'] ? 1 : -1;
    }
    
    
    //同上
    public function public_my_sort($param1, $param2){
        if($param1['value'] == $param2['value']) return 0;
        else return $param1['value'] > $param2['value'] ? 1 : -1;
    }
    
}
$arr = array(
            'a'=>array('key'=>'定义1', 'value'=>'20'),
            'b'=>array('key'=>'定义2', 'value'=>'1'),
            'c'=>array('key'=>'定义3', 'value'=>'10'),
            'd'=>array('key'=>'定义4', 'value'=>'5'),
            'e'=>array('key'=>'定义5', 'value'=>'21'),
            'f'=>array('key'=>'定义6', 'value'=>'4'),
            'g'=>array('key'=>'定义7', 'value'=>'3'),
        );
        
$myClassSort = new myClassSort();
var_dump($myClassSort->main($arr, $arr));
/*输出结果同以上实例*/

```





**类似函数扩展**

   [array_multisort](http://www.php.net/manual/zh/function.array-multisort.php) 对多个数组或多维数组进行排序，但是最终填入使用的还是具体一维数组

   [arsort](http://www.php.net/manual/zh/function.arsort.php)  对一维数组进行逆向排序并保持索引关系，保持索引对应关系

   [asort](http://www.php.net/manual/zh/function.asort.php)  对一维数组进行正向排序并保持索引关系，保持索引对应关系

   [krsort](http://www.php.net/manual/zh/function.krsort.php)  对数组按照键名逆向排序，保持索引对应关系

   [ksort](http://www.php.net/manual/zh/function.ksort.php)  对数组按照键名正向排序，保持索引对应关系

   [natcasesort](http://www.php.net/manual/zh/function.natcasesort.php)  用“自然排序”算法对一维数组进行不区分大小写字母的排序，可以用来排序数组内容中字母数字混合的情况，保持索引对应关系

   [natsort](http://www.php.net/manual/zh/function.natsort.php)  用“自然排序”算法对一维数组排序，区分大小写字母，可以用来排序数组内容中字母数字混合的情况，保持索引对应关系

   [rsort](http://www.php.net/manual/zh/function.rsort.php)  对一维数组逆向排序，不保持索引对应关系

   [sort](http://www.php.net/manual/zh/function.sort.php)  对一维数组正向排序，不保持索引对应关系

   [uasort](http://www.php.net/manual/zh/function.uasort.php)  使用用户自定义的比较函数对数组中的值进行排序并保持索引关联，可排序多维数组，本文重点讲解此函数

   [uksort](http://www.php.net/manual/zh/function.uksort.php)  使用用户自定义的比较函数对数组中的键名进行排序

   [usort](http://www.php.net/manual/zh/function.usort.php)  使用用户自定义的比较函数对数组中的值进行排序，不保持索引关联