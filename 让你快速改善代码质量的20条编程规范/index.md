# 让你最快速地改善代码质量的20条编程规范


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



# 代码风格（Code Style）

# 编程技巧（Coding Tips）
