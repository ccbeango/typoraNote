# Python入门阅读笔记

> 《Python编程从入门到实践》

1. Python文件的后缀为`.py`

## 变量

1. 变量的命名和使用：
   * 变量名只能包含字母、数字和下划线。变量名可以字母或下划线打头，但不能以数字打头。 
   * 变量名不能包含空格，但可使用下划线来分隔其中的单词 。
   * 不要将Python关键字和函数名用作变量名，即不要使用Python保留用于特殊用途的单词 ，如`print`。
   * 变量名应既简短又具有描述性
   * 慎用小写字母l和大写字母O，因为它们可能被人错看成数字1和0

2. 在保存每个程序时，使用符合标准Python约定的文件名：使用**小写字母**和**下划线**，如`hello_world.py`。

3. 字符串就是一系列字符。用引号括起的都是字符串，其中的引号可以是单引号，也可以是双引号。这种灵活性让你能够在字符串中包含引号和撇号：

   ```python
   'I told my friend, "Python is my favorite language!"'
   
   "The language 'Python' is named after Monty Python, not the snake."
   ```

4. 字符串大小写：

   * `title()`：首字母大写，其余小写。
   * `upper()`：大写
   * `lower()`：小写

   ```python
   name = 'ccbAan gO'
   
   print(name.title()) # Ccbean Go
   print(name.upper()) # CCBEAN GO
   print(name.lower()) # ccbean go
   ```

5. 合并(拼接)字符串：使用`+`来合并字符串

6. 字符串制表符或换行符添加空白：**空白**泛指任何非打印字符，如空格、制表符和换行符。

   ```python
   print("Languages:\n\tPython\n\tC\n\tJavaScript");
   
   # Languages:
   #        Python
   #        C
   #        JavaScript
   ```

7. 字符串删除空白：

   * `strip()`：删除字符串两端的空白
   * `lstrip()`：删除字符串左边的空白
   * `rstrip()`：删除字符串右边的空白

   ```python
   favorite_language = ' python ' 
   
   print(favorite_language)  # ' python '
   print(favorite_language.strip())  # 'python'
   print(favorite_language.rstrip())  # ' python'
   print(favorite_language.lstrip())  # 'python '
   
   ```

8. 数字整数：可对整数执行加`+`、减`-`、乘`*`、除`/`、乘方`**`，可以使用括号来修改运算次序。

   ```python
   2 + 3*4 		# 14
   (2 + 3) * 4 # 20
   ```

9. 数字浮点数：将带有小数点的数字都称为浮点数。使用的是[`IEEE-754`](https://zh.wikipedia.org/wiki/IEEE_754)。

10. 使用`str()`避免类型错误：

    ```python
    age = 23
    message = "Happy " + age + "rd Birthday!"
    print(message)
    # 报错
    # TypeError: must be str, not int
    
    message = "Happy " + str(age) + "rd Birthday!"
    print(message) # Happy 23rd Birthday!
    ```

    这是类型错误，在代码中，使用了整数(int)变量，但它不知道如何解读这个值。`str()`将非字符串值表示为字符串。

11. 在Python 2中，将两个整数相除得到的结果稍有不同：

    ```pyth
    >>> python2.7
    >>> 3 / 2
    1
    >>> 3 / 2
    1
    >>> 3.0 / 2
    1.5
    >>> 3 / 2.0 
    1.5
    >>> 3.0 / 2.0 
    1.5
    ```

12. Python注释用井号`#`标识。`# hello world`
13. Python之禅：`import this`。

## 列表简介

1. 列表由一系列按特定顺序排列的元素组成，用方括号`[]`来表示列表，并用逗号来分隔其中的元素。相当于PHP中的索引数组和JS中的数组。
2. 列表是有序集合，因此要访问列表的任何元素，只需将该元素的位置或索引告诉Python即可。
   要访问列表元素，可指出列表的名称，再指出元素的索引，并将其放在方括号内。

3. 修改元素列表：与访问类似，直接对应索引即可修改。

4. 在列表中添加元素：

   * `append()`：在列表末尾添加元素。
   * `insert()`：在列表的任何位置插入元素，指定新元素的索引和值

   ```python
   names = [ 'tom', 'jack', 'andy' ]
   print(names) # ['tom', 'jack', 'andy'] 
   
   names.append('mike');
   print(names) # ['tom', 'jack', 'andy', 'mike']
   
   names.insert(1,'lily')
   print(names) # ['tom', 'lily', 'jack', 'andy', 'mike']
   ```

5. 在列表中删除元素：

   * `del`: 指定删除的索引
   * `pop()`：删除列表中的元素，默认删除末尾元素；也可删除列表中任何位置元素，参数中传入索引即可，返回删除值。
   * `remove()`：根据值删除元素。如果某个值在列表中出现多次，只删除第一次出现的值

   ```python
   
   names = ['jack', 'tom', 'andy', 'mike', 'lily', 'alex']
   print(names)
   
   del names[0] 
   print(names)  # ['tom', 'andy', 'mike', 'lily', 'alex']
   
   names.pop()  
   print(names)  # ['tom', 'andy', 'mike', 'lily']
   
   names.pop(2)
   print(names)  # ['tom', 'andy', 'lily']
   
   names.remove('lily')
   print(names)  # ['tom', 'andy']
   ```

6. 列表排序：

   * `sort()`：对列表进行永久性排序。
   * `sorted()`：对列表进行临时排序
   * `reverse()`：反转列表元素的排列顺序

   ```python
   # cars = ['bmw', 'audi', 'toyota', 'subaru'] 
   
   cars.sort()
   print(cars)  # ['audi', 'bmw', 'subaru', 'toyota']
   
   cars.sort(reverse=True)
   print(cars) # ['toyota', 'subaru', 'bmw', 'audi']
   
   print(sorted(cars)) # ['audi', 'bmw', 'subaru', 'toyota']
   
   print(cars) # ['bmw', 'audi', 'toyota', 'subaru']
   ```

   如果你要按与字母顺序相反的顺序显示列表，`sort()`和`sorted()`都可以传递参数`reverse=True`。

7. 确定列表长度

   * `len()`：获取列表长度。

   ```python
   cars = ['bmw', 'audi', 'toyota', 'subaru']
   len(cars) # 4
   ```

8. 避免索引错误：
   * 访问不存在的索引会报错`IndexError: list index out of range`
   * 索引`-1`总是返回列表最后一个元素，仅当列表为空时，会导致错误。

## 操作列表

1. 遍历整个列表使用`for`循环。使用`for`容易出现的错误是忘记使用冒号`:`。

   ```python
   cars = ['bmw', 'audi', 'toyota', 'subaru']
   
   for car in cars:
       print(car)
   ```

2. Python根据缩进来判断代码行与前一个代码行的关系。
3. 缩进常见的错误：
   * 忘记缩进
   * 忘记缩进额外的代码行，容易出现逻辑错误
   * 不必要缩进的时候缩进

4. 创建数值列表：

   * 使用函数`range()`：

     ```python
     for value in range(1,5):
     		print(value)
         
     # 1
     # 2
     # 3
     # 4
     ```

   * `list()`可以将`range()`的结果直接转换为列表。

     ```python
     numbers = list(range(1,5))
     
     print(numbers) # [1, 2, 3, 4, 5]
     ```

     函数`range()`也可以指定步长：

     ```python
     even_numbers = list(range(2,11,2))
     print(even_numbers) # [2, 4, 6, 8, 10]
     ```

5. 对数字列表执行简单的统计计算：

   * `max()`：最大值
   * `min()`：最小值
   * `sum()`：求和

   ```python
   digits = [1, 2, 3, 4, 5, 6, 7, 8, 9, 0]
   
   min(digits) # 0
   max(digits) # 9
   sum(digits) # 45
   ```

6. 列表解析：列表解析将for循环和创建新元素的代码合并成一行，并自动附加新元素。

   ```python
   squares = [value**2 for value in range(1, 11)] 
   
   print(squares) # [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
   ```

   要注意，这里的`for`语句末尾没有冒号。

7. 列表的部分元素，Python中称之为**切片**。要创建切片，可指定要使用的第一个元素和最后一个元素的索引。它如`range()`函数一样，都是左闭右开区间。

8. 使用切片可以生成列表的任何子集。

   提取2~4个元素：

   ```python
   numbers = [ 'one', 'two', 'three', 'four', 'five', 'six' ];
   
   print(numbers[1:4]) # ['two', 'three', 'four']
   ```

   如果没有指定第一个索引，将从列表开头开始：

   ```python
   numbers = [ 'one', 'two', 'three', 'four', 'five', 'six' ];
   
   print(numbers[:4]) # ['one', two', 'three', 'four']
   ```

   同样，不指定最后的索引，切片会直接到末尾：

   

   ```python
   numbers = [ 'one', 'two', 'three', 'four', 'five', 'six' ];
   
   print(numbers[2:]) # [ 'three', 'four', 'five', 'six']
   ```

   负数索引返回离列表末尾相应距离的元素：

   ```python
   numbers = [ 'one', 'two', 'three', 'four', 'five', 'six' ];
   
   print(numbers[-3:]) # [ 'four', 'five', 'six']
   ```

9. 遍历切片：如果要遍历列表的部分元素，可以在`for`循环中使用切片。

   ```python
   numbers = [ 'one', 'two', 'three', 'four', 'five', 'six' ];
   
   for number in numbers[2:5]:
       print(number)
   # three
   # four
   # five
   ```

   在很多情况下，切片都很有用。例如，编写游戏时，你可以在玩家退出游戏时将其最终得分
   加入到一个列表中。然后，为获取该玩家的三个最高得分，你可以将该列表按降序排列，再创建
   一个只包含前三个得分的切片。处理数据时，可使用切片来进行批量处理;编写Web应用程序时，
   可使用切片来分页显示信息，并在每页显示数量合适的信息。

10. 复制列表：复制列表是创建一个包含整个列表的切片，方法是同事省略起始索引和终止索引`[:]`。

    ```python
    my_foods = ['pizza', 'falafel', 'carrot cake'] 
    
    friend_foods = my_foods[:]
    friend_foods.append('rice')
    print("My favorite foods are:") 
    print(my_foods)
    print("\nMy friend's favorite foods are:")
    print(friend_foods)
    
    # My favorite foods are:
    # ['pizza', 'falafel', 'carrot cake']
    
    # My friend's favorite foods are:
    # ['pizza', 'falafel', 'carrot cake', 'rice']
    ```

    复制会创建一个新的列表，然后将变量与此关联起来。然而，不使用切片，直接将列表赋值给其他值，那么列表是共享的，其中一个发生改变，另一个也会发生变化。因为变量名称是一个指向列表的指针，同JS的引用类型。

11. 元组：不可变的列表。元组看起来犹如列表，但使用圆括号`()`来标识。定义元组后，就可以使用索引来访问其元素，就像访问列表元素一样。也可同列表一样进行遍历。

    ```python
    dimensions = (200, 50)
    
    print(dimensions[0]) # 200
    print(dimensions[1]) # 50
    
    # 修改
    dimensions[0] = 250
    # TypeError: 'tuple' object does not support item assignment
    ```

12. 若要提出Python语言修改建议，需要编写Python改进提案(Python Enhancement Proposal，PEP)，代码格式：
    * 缩进，建议每级缩进都使用四个空格，既可提高可阅读星，又留下了足够的多级缩进空间。
    * 行长，建议每行不超过**80**个字符。最初是因为大多数计算机中，终端窗口每行只能容纳79个字符；当前，计算机屏幕每行可容纳的字符数多得多，为何还要使用79字符的标准行长呢?这里有别的原因。专业程序员通常会在同一个屏幕上打开多个文件，使用标
      准行长可以让他们在屏幕上并排打开两三个文件时能同时看到各个文件的完整行。PEP 8还建议注释的行长都不超过72字符，因为有些工具为大型项目自动生成文档时，会在每行注释开头添加格式化字符。
    * 空行，合理运用，不要再程序中过多地使用空行。

## if语句

1. 每条if语句的核心都是一个值为`True`或`False`的表达式，这种表达式被称为条件测试。Python根据条件测试的值为`True`还是`False`来决定是否执行if语句中的代码。如果条件测试的值为`True`，Python就执行紧跟在`if`语句后面的代码;如果为`False`，Python就忽略这些代码。

2. 检查多个条件，可使用逻辑运算符：且`and`和或`or`

3. 检查特定值(不)包含在列表中，使用关键字`in`

   ```python
   requested_toppings = ['mushrooms', 'onions', 'pineapple']
   'mushrooms' in requested_toppings # True
   'pepperoni' in requested_toppings # False
   
   'pepperoni' not in requested_toppings # True
   ```

4. 布尔表达式，`True`和`False`，开头是大写，其他都是错的。。。

5. 语句格式：

   * `if`

     ```python
     if conditional_test: 
       do something
     ```

   * `if-else`

     ```python
     if conditional_test: 
       do something
     else:
       do something
     ```

   * `if-elif-else`

     ```python
     if conditional_test: 
       do something
     elif conditional_test:
       do something
     else:
       do something
     ```

6. 确定列表是否为空：

   ```python
   test = []
   if test:
       print('这个列表不是空的')
   else:
       print("这个列表是空的")
       
   # 输出：这个列表是空的
   ```

7. if语句的格式，在诸如`==`、`>=`、`<=`等比较运算符两边各添加一个空格。

## 字典

1. 在Python中，字典是一系列键-值对。每个键都与一个值相关联，可以使用键来访问与之相关联的值。可将任何Python对象用作字典中的值。键和值之间用冒号分割，键值对之间用逗号分割。

   ```python
   alien_0 = {'color': 'green', 'points': 5}
   print(alien_0['color']) # green
   
   # 添加值
   # {'color': 'green', 'points': 5, 'y_position': 25, 'x_position': 0}
   alien_0['x_position'] = 0
   alien_0['y_position'] = 25
   
   
   # 修改值
   # {'color': 'yellow', 'points': 5, 'y_position': 25, 'x_position': 0}
   alien_0['color'] = 'yellow'
   
   
   # 删除值
   # {'color': 'green', 'y_position': 25, 'x_position': 0}
   del alien_0['points']
   ```

2. 遍历字典中所有的键和值：

   方法`items()`返回一个键值对列表。

   ```python
   user = { 'name': 'wangge', 'age': '18', 'favorite': 'banana' }
   # user.items() 返回 dict_items([('name', 'wangge'), ('age', '18'), ('favorite', 'banana')])
   for key, value in user.items():
       print('\nKey: ' + key)
       print('\nValue: ' + value)
   ```

   遍历字典时，键值对的返回顺序与存储顺序可能不同，字典是无序的，Python并不关心存储顺序，而只跟踪键和值之间的关联关系。

3. 遍历字典中所有的键：

   方法`keys()`返回字典中所有的键列表

   ```python
   user = { 'name': 'wangge', 'age': '18', 'favorite': 'banana' }
   # user.keys() 返回 dict_keys(['name', 'age', 'favorite'])
   for key in user.keys():
       print('Key: ' + key)
   ```

   遍历代码时，会默认遍历所有的键，因此可省略`keys()`，输出将不变。

   因为字典是无序的，如果想对其进行排序，一种做法是，使用函数`sorted()`来获得按特定顺序排列的键列表副本：

   ```python
   favorite_languages = {
       'jen': 'python', 
       'sarah': 'c',
       'edward': 'ruby', 
       'phil': 'python'
   }
   
   for name in sorted(favorite_languages.keys()):
       print(name.title())
   
   ```

4. 遍历字典中的所有值：

   方法`values()`返回一个值列表。

   ```python
   favorite_languages = {
       'jen': 'python', 
       'sarah': 'c',
       'edward': 'ruby', 
       'phil': 'python'
   }
   
   for name in favorite_languages.values():
       print(name.title())
   ```

   以此法提取的所有值没有考虑是否有重复。为剔除重复，可以使用集合`set`，集合类似于列表，但每个元素都必须是独一无二的：

   ```python
   favorite_languages = {
       'jen': 'python', 
       'sarah': 'c',
       'edward': 'ruby', 
       'phil': 'python'
   }
   
   for name in set(favorite_languages.values()):
       print(name.title())
   ```

5. 嵌套：将一系列字典存储在列表中，或将列表作为值存储在字典中。

## 用户输入和while循环

1. 函数`input()`让程序暂停运行，等待用户输入一些文本。获取用户输入后，Python将其存储在一个变量中，以便之后使用。此函数接受一个参数， 要向用户显示的说明。

   ```python
   message = input("Tell me something, and I will repeat it back to you: ")
   print(message)
   ```

2. 函数`int()`获取数值输入，使用函数`input()`时，Python将用户输入解读为字符串。使用`int()`可将用户输入的数字字符串转为数值，不使用而直接作为数值使用会报错。

   ```python
   height = input("How tall are you, in centimeter? ") 
   height = int(height)
   
   if height >= 170:
       print("\nYou're tall enough to ride!")
   else:
       print("\nYou'll be able to ride when you're a little older.")
   ```

3. while循环简介：while  不断地运行，直到 定的条
   件不满足为止。

   ```python
   # 用户输入quit时程序退出
   prompt = "\nTell me something, and I will repeat it back to you:"
   prompt += "\nEnter 'quit' to end the program. "
   
   message = ""
   while message != 'quit':
       message = input(prompt)
       if message != 'quit'
       		print(message)
   ```

4. 使用标志可以简化while语句。

5. 使用`break`可立即退出`while`循环，也可用于`for`循环。

   ```python
   current_number = 0
   while current_number < 10:
       current_number += 1
       if current_number % 2 == 0:
           break
       print(current_number)
       # 1 
   ```

   

6. 使用`continue`跳出本次循环：

   ```python
   current_number = 0
   while current_number < 10:
       current_number += 1
       if current_number % 2 == 0:
           continue
       print(current_number)
       # 1 3 5 7 9
   ```

7. 使用while循环来处理列表和字典：for  是一种遍历列表的有效方式，但在for循环中不应修改列表，否则将导致Python难以跟踪其中的元素（？？？）。要在遍历列表的同时对其进行修改，可使用while循环。通过将while循环同列表和字典结合起来使用，可 集、存储并组织大量输入，供以后查看和显示。

8. 在列表间移动元素

9. 删除包含特定值的所有列表元素：

   ```python
   pets = ['dog', 'cat', 'dog', 'goldfish', 'cat', 'rabbit', 'cat'] 
   print(pets)
   
   while 'cat' in pets:
       pets.remove('cat')
   
   print(pets)
   ```

10. 使用用户输入来填充字典：可使用while  提示用户输入任意数量的信息。

    ```python
    responses = {}
    # 设置一个标志 指出调查是否继续 
    polling_active = True
    
    while polling_active:
        # 提示输入被调查 的名字和回
        name = input("\nWhat is your name? ")
        response = input("Which mountain would you like to climb someday? ")
        # 将答卷存储在字典中 
        responses[name] = response
        # 看看是否还有人要参与调查
        repeat = input("Would you like to let another person respond? (yes/ no) ") 
        if repeat == 'no':
            polling_active = False
    
    # 调查结束 显示结果
    print("\n--- Poll Results ---")
    for name, response in responses.items():
        print(name + " would like to climb " + response + ".")
    ```

## 函数

1. 函数定义：使用关键字`def`向Python指出函数名，可在括号内指出函数为完成其任务需要的参数。

   ```python
   def hello():
     print('hello world')
     
   hello() # hello world
   ```

2. 实参和形参：

   ```python
   def hello(username):
     print('hello ' + username)
   
   hello('wangge')  # hello wangge
   ```

   在定义函数`hello()`时，变量`username`是**形参**—函数完成其工作所需的一项信息。在代码`hello('wangge')`中，值`wangge`是一个**实参**—调用函数时传递给函数的信息。

3. 传递实参：

   * 位置实参：调用函数时，Python必须将函数调用中的每个实参都关联到函数定义中的每个形参。最简单的关联方式是基于实参的顺序。这种关联方式被称为位置实参。

     ```python
     def describe_pet(animal_type, pet_name):
         """显示 物的信息"""
         print("\nI have a " + animal_type + ".")
         print("My " + animal_type + "'s name is " + pet_name.title() + ".")
     
     describe_pet('hamster', 'harry') # 注意参数顺序
     ```

   * 关键字实参：传递给函数的名称-值对。直接在实参中将名称和值关联起来，向函数传递实参时无需关注参数传递顺序，而且还清楚地指出了函数调用中各个值的用途。

     ```python
     def describe_pet(animal_type, pet_name):
         """显示 物的信息"""
         print("\nI have a " + animal_type + ".")
         print("My " + animal_type + "'s name is " + pet_name.title() + ".")
     
     describe_pet(animal_type='hamster', pet_name='harry')
     describe_pet(pet_name='harry', animal_type='hamster')
     ```

   * 默认值：编写函数时，可以给每个形参指定默认值。给形参指定默认值后，可在函数调用中省略相应的实参。

     ```python
     def describe_pet(pet_name, animal_type = 'dog'):
         """显示 物的信息"""
         print("\nI have a " + animal_type + ".")
         print("My " + animal_type + "'s name is " + pet_name.title() + ".")
     
     describe_pet('Peter') 
     ```

     注意：使用默认值时，在形参列表中必须先列出没有默认值的形参，再列出有默认值的实参。这让Python依然能够正确地解读位置实参。

   * 等效的函数调用：由于可以混合使用位置实参、关键字实参和默认值，通常有多重等效的函数调用方式。

     函数定义：

     ```python
     def describe_pet(pet_name, animal_type='dog'):
     ```

     调用方式：

     ```python
     # 一条名为Willie的小  
     describe_pet('willie') 
     describe_pet(pet_name='willie')
     
     # 一只名为Harry的仓
     describe_pet('harry', 'hamster') 
     describe_pet(pet_name='harry', animal_type='hamster') 
     describe_pet(animal_type='hamster', pet_name='harry')
     ```

4. 禁止函数修改列表而又操作修改列表的方法是想函数传递副本即切片。

5. 传递任意数量的实参：Python允许函数从调用语句中收集任意数量的实参。

   ```python
   def make_pizza(*toppings):
       """打印顾客所点的所有配料"""
       print(toppings)
   
   make_pizza('pepperoni') # ('pepperoni',)
   make_pizza('mushrooms', 'green peppers', 'extra cheese') # ('mushrooms', 'green peppers', 'extra cheese')
     	
   ```

   * 结合使用位置实参和任意数量实参：如果要让函数接受不同类型的实参，必须在函数定义中将接纳任意数量实参的形参放在最后。Python先匹配位置实参和关键字实参，再将余下的实参都收集到最后一个形参中。

   ```python
   def make_pizza(size, *toppings):
       print("\nMaking a " + str(size) +
           "-inch pizza with the following toppings:")
       for topping in toppings:
           print("- " + topping)
   
   
   make_pizza(16, 'pepperoni')
   make_pizza(12, 'mushrooms', 'green peppers', 'extra cheese')
   ```

   * 使用任意数量的关键字实参：有时 ，需要接受任意数量的实参，但预先不知道传递给函数的会是什么样的信息。在这种情况下，可将函数编写成能够接受任意数量的键值对——调用语句提供了多少就接受多少。

   ```python
   def build_profile(first, last, **user_info): 
       """创建一个字典，其中包含有关用户的一切""" 
       profile = {}
       profile['first_name'] = first
       profile['last_name'] = last
       for key, value in user_info.items():
           profile[key] = value
       
       return profile
   
   user_profile = build_profile(
       'albert', 
       'einstein', 
       location='princeton',
       field='physics'
   )
   print(user_profile)
   ```

6. 将函数存储在模块中：将函数存储在被称为模块的独立文件中，再将模块导入到主程序中。`import`语句允许在当前运行的程序文件中使用模块中的代码。

7. 如果使用`import`语句导入了名为`module_name.py`的整个模块，就可以使用`module_name.function_name()`的语法使用其中任何一个函数。看下例子：

   先创建`pizza.py`文件：

   ```python
   # pizza.py
   def make_pizza(size, *toppings):
       print("\nMaking a " + str(size) +
           "-inch pizza with the following toppings:")
       for topping in toppings:
           print("- " + topping)
   ```

   再创建`making_pizzas.py`文件：

   ```python
   # making_pizzas.py
   import pizza
   
   pizza.make_pizza(16, 'pepperoni')
   pizza.make_pizza(12, 'mushrooms', 'green peppers', 'extra cheese')
   ```

   Python执行代码行`import pizza`时，会打开文件`pizza.py`，并将其中的所有函数都复制到这个程序中。

8. 导入特定函数，可使用下面的语法

   ```python
   from module_name import function_0, function_1, function_2
   ```

   对于**第7**中的`making_pizzas.py`，可以改成：

   ```python
   # making_pizzas.py
   from pizza import make_pizza
   
   make_pizza(16, 'pepperoni')
   make_pizza(12, 'mushrooms', 'green peppers', 'extra cheese')
   ```

9. 使用as给函数指定别名，语法如下：

   ```python
   from module_name import function_name as fn
   ```

   例子：

   ```python
   # making_pizzas.py
   from pizza import make_pizza as mp
   
   mp(16, 'pepperoni')
   mp(12, 'mushrooms', 'green peppers', 'extra cheese')
   ```

10. 导入模块中的所有函数：使用星号`*`运算符可以导入模块中所有函数。

    ```python
    from module_name import *
    ```

    

    ```python
    # making_pizzas.py
    from pizza import *
    
    make_pizza(16, 'pepperoni')
    make_pizza(12, 'mushrooms', 'green peppers', 'extra cheese')
    ```

    尽量避免使用此语法，Python可能遇到多个名称相同的函数或变量，进而函数覆盖。

11. 函数编写指南：

    * 应该给函数指定描述性名称，且只在其中使用小写字母和下划线。

    * 每个函数都应包含简要地阐述其功能的注释，该注释应该紧跟在函数定义后面，并采用文档字符串格式。

    * 给形参指定默认值时，等号两边不要有空格；函数调用中的关键字实参，也应如此。

      ```python
      # 形参指定默认值
      def function_name(parameter_0, parameter_1='default value')
      
      # 函数调用的关键字实参
      function_name(value_0, parameter_1='value')
      ```

    * PEP 8建议代码长度不要超过79字符，这样只要编辑器窗口适中，就能看到整行代码。如果形参很多，导致函数定义的长度超过79字符，可在函数定义中输入括号后按回车键，并在下一行按两次Tab键，从而将形参列表和只缩进一层的函数体区分开来。

      ```python
      def function_name(
              parameter_0, parameter_1, 
              parameter_2, parameter_3, 
              parameter_4, parameter_5):
          function body...
      ```

    * 如果程序或模块包含多个函数，可使用两个空行将相邻的函数分开，这样更容易知道前一个函数的结束位置和下一个函数的开始位置。

    * 所有的`import`语句都应该放在文件开头，唯一例外的情景是，在文件开头使用了注释来描述整个程序。

## 类

1. 创建一个`Dog`类：

   ```python
   class Dog():
       """一个简单的小狗类测试"""
   
       def __init__(self, name, age):
           """初始化属性name和age"""
           self.name = name
           self.age = age
   
   
       def sit(self):
           """小狗蹲下"""
           print(self.name.title() + ' is now sitting.')
   
       
       def roll_over(self):
           """小狗打滚"""
           print(self.name.title() + ' rolled over!')
   ```

2. 类中的函称为方法，函数的一切都适用方法，唯一重要的差别是调用方法的方式。方法`__init__()`是一个特殊的方法（构造函数），开头和结尾各有两个下划线，这是一种约定，旨在避免Python默认方法与普通方法发生名称冲突。

3. 在**第1**中，方法`__init__()`定义了包含三个形参：`self`、`name`和`age`。在此方法中，`self`是必须的，且必须位于其他形参前面，因为Python调用`__init__()`方法来创建实例时，将自动传入实参`self`。每个与类相关联的方法调用都自动传递实参`self`，它是一个指向实例本身的引用，让实例能够访问类中的属性和方法。

   我们将通过实参向`Dog()`传递名字和年龄；`self`会自动传递，因此我们不需要传递它。每当我们根据`Dog`类创建实例时，都只需 最后两个形参(`name`和`age`)提供值。

4. 以`self`为前缀的变量可供类中的所有方法使用，像这样可以通过实例访问的变量称为属性。

5. 创建实例：

   ```python
   my_dog = Dog('peter', 6)
   
   print('dog`s name is ' + my_dog.name.title() + '.')
   print('dog is ' + str(my_dog.age) + ' yeats old.')
   
   my_dog.sit()
   my_dog.roll_over()
   ```

6. 继承：一个类继承另一个类时，它将自动获得另一个类的所有属性和方法，这个类还可以定义自己的属性和方法。原有的类称为父类，而新类称为子类。

7. 子类的方法`__init__()`，创建子类的实例时，Python首先是给父类的所有属性赋值。

   ```python
   # 继承第1中的Dog
   class Corgi(Dog):
       """柯基"""
   
       def __init__(self, name, age):
           """初始化父类属性"""
           super().__init__(name, age)
           
           
       def barking(self):
           """子类方法 狗叫"""
           print('WuWang!')
   
   
   corgi = Corgi('tom', 3)
   corgi.roll_over() # Tom rolled over!
   corgi.barking() # WuWang！
   ```

8. 重写父类方法：在子类中定义与父类方法同名方法。这样，Python将不会考虑这个父类方法，而只关注在子类中定义的相应方法。

9. 将实例用作属性：可能需要将类的一部分作为一个独立的类提取出来，将大型的类拆分成多个协同工作的小类。

   ```python
   class Car
       """汽车"""
     
   class Battery():
       """一次模拟电动汽车电瓶的简单尝试"""
   
       def __init__(self, battery_size=70):
           """初始化电瓶属性""" 
           self.battery_size = battery_size
   
       def describe_battery(self):
           """打印一条 述  容量的消息"""
           print("This car has a " + str(self.battery_size) + "-kWh battery.")
      
     
   class ElectricCar(Car):
       """电瓶车的独特之处"""
   
       def __init__(self, make, model, year):
           """初始化父类属性"""
           super().__init__(make, model, year)
           self.battery = Battery()  
   ```

10. 导入类：

    创建文件`car.py`：

    ```python
    class Car():
        """一次模拟汽车的简单尝试"""
    
        def __init__(self, make, model, year): 
            """初始化描述汽车属性""" 
            self.make = make
            self.model = model
            self.year = year 
            self.odometer_reading = 0
    
    
        def get_descriptive_name(self):
            """返回整洁的描述性名称"""
            long_name = str(self.year) + ' ' + self.make + ' ' + self.model 
            return long_name.title()
    
    
        def read_odometer(self):
            """打印一条消息，指出汽车的里程数"""
            print("This car has " + str(self.odometer_reading) + " miles on it.")
    
    
        def update_odometer(self, mileage): 
            """
                将里程表读数设置为指定的值   
                拒绝将里程表往回拨 
            """
            if mileage >= self.odometer_reading:
                    self.odometer_reading = mileage 
            else:
                print("You can't roll back an odometer!")
    
    
        def increment_odometer(self, miles): 
            """将里程表读数增加指定的量""" 
            self.odometer_reading += miles
    ```

    创建另一个文件`my_car.py`，在其中导入`Car`类并创建其实例：

    ```python
    from car import Car
    
    my_new_car = Car('audi', 'a4', 2016)
    print(my_new_car.get_descriptive_name()) # 2016 Audi A4
    
    my_new_car.odometer_reading = 23
    my_new_car.read_odometer() # This car has 23 miles on it.
    ```

11. 在一个模块中存储多个类，导入时多个类用逗号隔开。

    ```python
    from module_name import class_name01, class_name02
    ```

    也可以导入整个模块

    ```python
    import car
    
    my_beetle = car.Car('volkswagen', 'beetle', 2016)
    print(my_beetle.get_descriptive_name())
    
    my_tesla = car.ElectricCar('tesla', 'roadster', 2016)
    print(my_tesla.get_descriptive_name())
    ```

    导入模块中的所有类，不推荐这样使用。

    ```python
    from module_name import *
    ```

12. Python标准库是一组模块，安装的Python都包含它。下面看下`collections`中的一个类`orderedDict`，这个类的实例的行为几乎与字典相同，区别只在于记录了键值对的添加顺序。

    ```python
    from collections import OrderedDict
    
    favorite_languages = OrderedDict()
    
    favorite_languages['jen'] = 'python' 
    favorite_languages['sarah'] = 'c' 
    favorite_languages['edward'] = 'ruby' 
    favorite_languages['phil'] = 'python'
    
    for name, language in favorite_languages.items(): 
        print(name.title() + "'s favorite language is " + language.title() + ".")
    
    # Jen's favorite language is Python. 
    # Sarah's favorite language is C. 
    # Edward's favorite language is Ruby. 
    # Phil's favorite language is Python.
    ```

13. 类编码风格：
    * 类名应采用**驼峰命名法**，即将类名中的每个单词的首字母都大写，而不使用下划线。
    * 实例名和模块名都采用小写格式，并在单词之间加上下划线。
    * 对于每个类，都应紧跟在类定义后面包含一个文档字符串。
    * 可使用空行来组织代码，但不要滥用。在类中，可使用一个空行来分隔方法；而在模块中，可使用两个空行来分隔类。
    * 需要同时导入标准库中的模块和自己编写的模块时，先编写导入标准库模块的`import`语句，再添加一个空行，然后编写导入自己编写的模块的`import`语句。

## 文件

1. 读取整个文件：

   创建一个文件`pi_digits.txt`，包含小数点后30位的圆周率值，且在小数点后每10位处都换行：

   ```python
   3.1415926535 
   	8979323846 
     2643383279
   ```

   读取文件：

   ```python
   with open('pi_digits.txt') as file_object:
       contents = file_object.read()
       print(contents.rstrip())
   ```

   关键字`with`在不需要访问文件后将其关闭

   函数`open()`接受一个参数：要打开的文件的名称；返回一个表示文件的对象。上面的代码中只是打开文件，并没有关闭，可手动进行关闭，但是未妥善的关闭文件可能会导致数据丢失或受损。其实我们只管打开文件，并在需要的时候使用它，Python自会在合适的时候自动将其关闭。

   `rstrip()`删除末尾的空白。

2. 文件路径：要让Python打开不与程序文件位于同一个目录中的文件，需要提供文件路径，它让Python到系统的特定位置去查找。可以使用绝对路径，也可使用相对路径。

   ```python
   with open('text_files/filename.txt') as file_object:
   ```

3. 逐行读取：要以每次一行的方式检查文件，可对文件对象使用`for`循环：

   ```python
   filename = 'pi_digits.txt'
   
   with open(filename) as file_object:
       for line in file_object:
           print(line.rstrip())
   ```

   输出与之前的方式完全相同。

4. 创建一个包含文件各行内容的列表：使用关键字`with`时，`open()`返回的文件对象只在`with`代码块内可用。如果要在其代码块外访问文件的内容，可在`with`代码块内将文件的各行存储在一个列表中。

   ```python
   filename = 'pi_digits.txt'
   
   with open(filename) as file_object:
       lines = file_object.readlines()
   
   for line in lines:
       print(line.rstrip())
   ```

   函数`readlines()`从文件中读取每一行，并将其存储在列表中。

5. 读取文本文件时，Python将其中所有文本都为字符串。如果你读取的是数字，并要将其作为数值使用，就必须使用函数`int()`将其转换为整数，或使用函数`float()`将其转换为浮点数。

6. 写入空文件：调用`open()`时需要提供另一个实参，告诉Python你要写入打开的文件。

   ```python
   filename = 'test.txt'
   
   with open(filename, 'w') as file_object:
       file_object.write("人生苦短，快学Python")
   ```

   `open()`接收的第二个参数是要告诉Python打开文件的模式：读取模式`r`、写入模式`w`、附加模式`a`、读取和写入文件模式`r+`。省略模式参数，默认以只读模式打开文件。

   如果要写入的文件不存在，函数`open()`将自动创建它。然而，以写入`w`模式打开文件，如果文件已经存在，Python将在返回文件对象前清空该文件。

   如果要写入多行，需要换行符`\n`。

7. 附加到文件：如果要给文件添加内容，而不是覆盖，可以使用附加模式`a`打开文件。

   ```python
   filename = 'test.txt'
   
   with open(filename, 'a') as file_object:
       file_object.write("Python大法好")
   ```

8. Python使用被称为异常的特殊对象来管理程序执行期间发生的错误。如果编写了处理异常的代码，程序将继续运行；如果未对异常进行处理，程序将停止，并显示一个`traceback`，其中包含有关异常的报告。
9. 异常是使用`try-except`代码块处理的。使用此处理，即便出现异常，程序也会继续运行。

10. 使用`try-except`代码块，处理`ZeroDivisionError`异常：

    ```python
    try:
        print(5/0)
    except ZeroDivisionError:
        print('You can`t divide by zero!')
    
    print('Hi') # 正常运行
    ```

    `try-except`代码后面的代码会正常运行，因为已经告诉了Python如何处理这种错误。

11. 使用`try-except-else`：

    ```python
    print("Give me two numbers, and I'll divide them.") 
    print("Enter 'q' to quit.")
    
    while True:
        first_number = input("\nFirst number: ") 
        if first_number == 'q':
            break
        second_number = input("Second number: ") 
        if second_number == 'q':
            break
        try:
            answer = int(first_number) / int(second_number)
        except ZeroDivisionError:
            print("You can't divide by 0!")
        else:
            print(answer)
    ```

    

12. pass语句：失败时一声不吭。Python中有一个`pass`语句，可在代码块中使用它来让Python什么都不要做：

    ```python
    def count_words(filename): 
        """计算一个文件大致包含多少个单词""" 
        try:
            --snip--
        except FileNotFoundError:
            pass 
        else:
            --snip--
    
    filenames = ['alice.txt', 'siddhartha.txt', 'moby_dick.txt', 'little_women.txt'] 
    for filename in filenames:
        count_words(filename)
    ```

13. 模块`json`能够将简单的Python数据结构转储到文件中，并在程序再次运行时加载该文件中的数据：
    * `json.dump()`：存储数据。接受两个实参，分别是要存储的数据和可用于存储数据的文件对象。
    * `json.load()`：加载数据。接收一个参数，存储数据的文件对象。

## 测试代码(第11章)

1. Python的标准库中的模块`unittest`提供了代码测试工具。**单元测试**用于核实函数的某个方面没有问题；**测试用例**是一组单元测试，这些单元测试一起核实函数在各种情形下的行为都符合要求。**全覆盖式测试**用例包含一整套单元测试，涵盖了各种可能的函数使用方式。
2. 对于大型项目，要实现全覆盖测试可能很难。通常，最初只要针对代码的重要行为编写测试即可，等项目被广泛使用时再考虑全覆盖。

3. 可通过的测试：

   创建一个文件`name_function.py`，写如下代码，它接收名和姓并返回整洁的姓名：

   ```python
   def get_fromatted_name(first, last):
       full_name = first + ' ' + last
       return full_name.title()
   ```

   创建`test_name_function.py`，可通过的测试：

   ```python
   import unittest
   
   from name_function import get_fromatted_name
   
   class NameTestCase(unittest.TestCase):
       """测试name_function.py"""
   
       def test_first_last_name(self):
           """能够正确地处理像Andy Liu这样的样的姓名吗 """
           format_name = get_fromatted_name('andy', 'liu')
           self.assertEqual(format_name, 'Andy Liu')
   
   unittest.main()
   ```

   首先，我们导入了模块`unittest`和要测试的函数`get_formatted_name()`。然后创建了一个名为`NameTestCase`的类，用于包含一系列针对`get_formatted_name()`的单元测试。你可随便给这个类命名,但最好让它看起来与要测试的函数相关，并包含字样`Test`，这个类必须继承`unittest.TestCase`类，这样 Python才知道如何运行你编写的测试。

   `NamesTestCase`只包含一个方法,用于测试`get_formatted_name()`的一个方面。我们将这个方法命名为`test_first_last_name()`，因为我们要核实的是只有名和姓的姓名能否被正确地格式化。我们运行 `test_name_function.py`时，所有以`test`打头的方法都将自动运行。在这个方法中，我们调用了要测试的函数，并存储了要测试的返回值在这个示例中,我们使用实参`Andy`和`Liu`调用`get_formatted_name()`，并将结果存储到变量`formatted_name`中。

   我们使用了`unittest`类中的一个**断言方法**—用来核实得到的结果是否与期望的结果一致。并向它传递`formatted_name`和`Andy Liu`。将两者进行比较，如果结果相等，就万事大吉，如果不相等，通知一声。

   代码`unittest.main()`让Python运行这个文件中的测试，运行`test_name_function.py`时，结果输出如下：

   ```python
   .
   ----------------------------------------------------------------------
   Ran 1 test in 0.005s
   
   OK
   ```

   第一行的句点`.`表示测试通过了。接下来的一行指出Python运行了一个测试，运行的时间不到0.005秒。最后的OK表示该测试用例中的所有单元测试都通过了。

4. 不能通过的测试：

   修改文件`name_function.py`中的`get_formatted_name()`，使其能够处理中间名，但这样做时，故意让这个函数无法正确处理想`Janis Joplin`这样只有名和姓的姓名。

   ```python
   def get_fromatted_name(first, middle, last):
       full_name = first + ' ' + middle + ' ' + last
       return full_name.title()
   
   ```

   运行测试文件`test_name_function.py`，输出如下：

   ```python
   E
   ======================================================================
   ERROR: test_first_last_name (__main__.NameTestCase)
   能够正确地处理像Andy Liu这样的样的姓名吗
   ----------------------------------------------------------------------
   Traceback (most recent call last):
     File "/Users/andy/Work/python/hello/my_car.py", line 10, in test_first_last_name
       format_name = get_fromatted_name('andy', 'liu')
   TypeError: get_fromatted_name() missing 1 required positional argument: 'last'
   
   ----------------------------------------------------------------------
   Ran 1 test in 0.004s
   
   FAILED (errors=1)
   ```

   第一行一个字母`E`，指出测试用例中有一个单元测试导致了错误。接下来谁导致的错误和标准的`traceBack`。最后一条信息，它指出整个测试用例都未通过，因为运行该测试用例时发生了一个错误。

5. 测试未通过时，不要修改测试，而应修复导致测试不能通过的代码：检查刚对函数所做的修改，找出导致函数行为不符合预期的修改。

   ```python
   def get_fromatted_name(first, last, middle = ''):
       if middle:
           full_name = first + ' ' + middle + ' ' + last
       else: 
           full_name = first + ' ' + last
       return full_name.title()
   ```

6. 添加新测试：

   ```python
   import unittest
   
   from name_function import get_fromatted_name
   
   class NameTestCase(unittest.TestCase):
       """测试name_function.py"""
   
       def test_first_last_name(self):
           """能够正确地处理像Andy Liu这样的样的姓名吗 """
           format_name = get_fromatted_name('andy', 'liu')
           self.assertEqual(format_name, 'Andy Liu')
   
       def test_first_middle_last_name(self):
           """能够处理像Wolfgang Amadeus Mozart这样的姓名吗"""
           formatted_name = get_fromatted_name('wolfgang', 'mozart', 'amadeus')
           self.assertEqual(formatted_name, 'Wolfgang Amadeus Mozart')
   
   unittest.main()
   ```

   运行测试：

   ```python
   ..
   ----------------------------------------------------------------------
   Ran 2 tests in 0.003s
   
   OK
   ```

7. 断言方法：检查你认为应该满足的条件是否确实满足。如果该条件确实满足，你对程序行为的假设就得到了确认，你就可以确信其中没有错误。如果你认为应该满足的条件实际上并不满足，Python将引发异常。

   `unittest Module`中的断言方法：

   | 方法                    | 用途               |
   | ----------------------- | ------------------ |
   | assertEqual(a, b)       | 核实`a == b`       |
   | assertNotEqual(a, b)    | 核实`a != b`       |
   | assertTrue(x)           | 核实x为True        |
   | assertFalse(x)          | 核实x为False       |
   | assertIn(item, list)    | 核实item在list中   |
   | assertNotIn(item, list) | 核实item不在list中 |

   注：只能在继承`unittest.TestCase`的类中使用这些方法。

8. 一个要测试的类：

   ```python
   class AnonymousSurvey():
       """收集匿名调查问卷的答案"""
   
       def __init__(self, question):
           """存储一个问题，并为存储答案做准备"""
           self.question = question
           self.responses = []
   
       def show_question(self):
           """显示调查问卷"""
           print(self.question)
   
       def store_response(self, new_response):
           """存储单份调查答案"""
           self.responses.append(new_response)
   
       def show_results(self):
           """显示收集到的所有答案"""
           print("Survey results:")
           for response in self.responses:
               print('- ' + response)
   
   ```

   编写使用程序：

   ```python
   # language_survey.py
   from survey import AnonymousSurvey
   
   # 定义一个问题，并创建一个表示调查的AnonymousSurvey对象
   question = "What language did you first learn to speak?"
   my_survey = AnonymousSurvey(question)
   
   # 显示问题并存储答案
   my_survey.show_question()
   print("Enter 'q' at any time to quit.\n")
   while True:
       response = input("Language: ")
       if response == 'q':
           break
       my_survey.store_response(response)
   
   # 显示调查结果
   print("\nThank you to everyone who participated in the survey!")
   my_survey.show_results()
   ```

9. 测试AnonymousSurvey类：

   方法`setUp()`：`unittest.TestCase`类包含此方法，让我们只需要创建对象一次，并在每个测试方法中使用它们。如果在`TestCase`类中包含了方法`setUp()`，Python将先运行它，再运行各个以`test_`打头的方法。这样，在编写的每个测试方法中就都可以使用在`setUp()`中创建的对象了。

   写测试代码：

   ```python
   import unittest
   
   from survey import AnonymousSurvey
   
   class TestAnonymousSurvey(unittest.TestCase):
       """ 针对Anonymous类测试 """
   
       def setUp(self):
           """ 
           创建一个调查对象和一组答案，供使用的测试方法使用
           """
           question = "What language did you first learn to speak?"
           self.my_survey = AnonymousSurvey(question)
           self.responses = ['English', 'Spanish', 'Mandarin']
   
       def test_store_signle_response(self):
           """ 测试单个答案会被妥善地存储 """
           self.my_survey.store_response(self.responses[0]) 
           self.assertIn(self.responses[0], self.my_survey.responses)
   
       def test_store_three_responses(self):
           """ 测试三个答案是否会被妥善地存储 """
           for response in self.responses:
               self.my_survey.store_response(response) 
           for response in self.responses:
               self.assertIn(response, self.my_survey.responses)
   
   unittest.main()
   ```

   运行结果：

   ```python
   ..
   ----------------------------------------------------------------------
   Ran 2 tests in 0.003s
   
   OK
   ```

10. 运行测试用例时，每当完成一个测试，Python都打印一个字符：测试通过时打印一个句点`.`；测试引发错误时打印一个`E`；测试导致断言失败时打印一个`F`。
11. 对于自己编写的函数和类，请编写针对其重要行为的测试，但在项目早期，不要试图去编写全覆盖的测试用途，除非有充分的理由这样做。