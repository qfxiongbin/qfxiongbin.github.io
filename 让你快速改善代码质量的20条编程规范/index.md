# 让你最快速地改善代码质量的10条编程规范


<!--more-->

重复的事儿做久了，我就会让自己停下来，学习一下，只有站在巨人的肩膀上，才能省点劲儿。而不是一味地说，我以前做过xxx,因为以前的经验也可能是错的。

这一篇是小争哥（前google工程师）在极客时间专栏《设计模式之美》专栏中根据他的工作经验给出的20条编程规范，整理出来自检。


# 命名与注释（Naming and Comments）

完事开头难，做项目比较难的是起名字，项目名、类名、函数名、变量名、接口名都是我们平时开发时要考虑如何定义的。命名的好坏决定着代码的可读性。

## 1、命名多长合适？

在足够表达意图的情况下，命名越短越好；对于一些默认，大家比较熟悉的词，推荐使用缩写。比如 sec 表示second、str表示string、num表示number。除此之外，对于作用域比较小的变量，我们可以使用想对短的命名；对于类名这种作用域比较打的，更推荐使用长命名。

命名的原则就是准确表达意图。命名时，假设自己不熟悉这块代码，从代码阅读者的角度去考量命名是否够直观。

## 2、利用上下文简化命名

利用对象上下文可简化为

```

public class User {
  private String userName;
  private String userPassword;
  private String userAvatarUrl;
  //...
}

//简化为

public class User {
  private String name;
  private String password;
  private String avatarUrl;
  //...
}

```


利用函数上下文简化命名

```

public void uploadUserAvatarImageToAliyun(String userAvatarImageUri);
//简化为
public void uploadUserAvatarImageToAliyun(String imageUri);
```

## 3、命名可读、可搜索

可读是指，不要用特别生僻、难发音的英文单词来命名；反例：plateaux、eyrie

可搜索指，通过IDEA经常用“关键词联想”的方法自动补全和搜索，例如键入“.get”希望能找到某个对象所有get开头的方法。键入Array，搜索JDK中数组相关的类，命名时需要符合项目的规范，一起用selectXXX，insertXXX等，不要自己用queryXXX,addXXX

## 4、接口和抽象类命名

对于接口一般有两种：
* 加前缀“I”，表示Interface，比如IUserservice,对应的实现类为UserService
* 不加前缀“I”,比如Userservice,对应实现类为UserServiceImpl

抽象类也有两种：
* 带Abstract 前缀,比如 AbstractConfiguration
* 不带Abstract

## 5、注释

### 注释怎么写，写什么
注释到底该怎么写，注释内容包含三个方面：做什么，为什么，怎么做

```

/**
* (what) Bean factory to create beans. 
* 
* (why) The class likes Spring IOC framework, but is more lightweight. 
*
* (how) Create objects from different sources sequentially:
* user specified object > SPI > configuration > default object.
*/
public class BeansFactory {
  // ...
}
```
* 注释比代码承载的信息更多
* 注释起到总结性作用、文档的作用
* 总结性的注释能让代码更清晰

### 注释是不是越多越好？

要写得尽可能全面、详细，而函数内部的注释要相对少一些。
一般都是靠好的命名、提炼函数、解释性的变量、总结性注释来提高代码的可读性。

# 代码风格（Code Style）

## 6、类、函数多大才合适

* 函数的代码行数最大限制，不要超过一屏的垂直高度。
* 一行代码的最大长度，不要超过IDEA的宽度。
* 类的成员变量与函数之间、静态成员变量与普通变量之间、各函数之间、甚至各成员变量之间，都可以添加空行，让结构更清晰

## 7、 四格锁进还是两格？成员排序？
与团队的规范保持一致，不要用tab缩进
* 大括号是否另起一行？
  推荐跟代码在同一行，同上，与团队风格保持一致即可。

* 类中成员的排列顺序
  
  Google编码规范中，依赖类按照字母顺序从小到大排列
  类中，成员变量排在函数前、成员变量之间或函数之间，按照 先静态、后普通的方式排列
  成员变量之间或函数之间，按照作用域范围的大小排列，先public 后protected 最后 private
  把有调用关系的函数放一起


# 编程技巧（Coding Tips）

## 8、把代码分割成更小的单元

阅读代码的习惯是，先读整体，再看细节。要善于将复杂逻辑提炼成类或者函数，屏蔽细节。提高代码的可读性。

## 9、避免参数过多

参数大于5就没法接受，需要重构。两种处理方法：
* 考虑函数是否指责单一，是否能查分成多个函数的方式来减少参数。
* 将参数封装成对象
* 不要用函数的参数来控制逻辑，拆分成多个函数处理
* 函数设计要职责单一
* 移除过深的嵌套层次，建议不要超过两层，解决这个问题的四个思路：
  * 去掉多余的if/else
  * 使用编程语言提供的continue、break、return等关键字提前退出嵌套
  * 调整执行顺序来减少嵌套
  * 将嵌套逻辑封装成函数（个人比较建议这一条）
## 10、 使用解释性变量
  * 用常量取代魔法数字
  * 使用解释性变量代替复杂表达式
  
