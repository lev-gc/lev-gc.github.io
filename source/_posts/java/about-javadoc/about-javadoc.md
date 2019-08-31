---
title: Java注释说明
date: 2019-08-22 00:34:00
tags: [Java]
---

## 目的

合理规范的代码注释，一方面可以使代码见注释知其义，有利于对代码的理解、调用、重构、复用等，另一方面可以生成有效并且美观的API文档。关于javadoc的完整说明请参照[官网](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/javadoc.html)，本文将结合不同方面对javadoc的日常使用进行说明。

## 认知和使用

不少开发人员对于javadoc这个东西并没有充分的认知，没有理解其提供的各种标签和注解，因此在撰写javadoc的时候经常是随心所欲地添加描述，甚至还自定义了各种各样的不能被javadoc识别的标签和注解。这样出来的代码注释没有清晰的规范，结构凌乱，可读性低，更不能生成一份有效的API文档，失去了javadoc提供的所有优点。

因此要强调的第一件事是，学会去使用javadoc。举个例子，如果你正在使用IntelliJ IDEA并且使用默认的keymap，把你的光标放在某个类、方法、属性或者变量上，按下`Ctrl+Q`，你就能看到对光标处成员的描述说明，而这个描述说明就是由我们所撰写的javadoc渲染而成（最新的2020版本IDEA甚至可以设置在代码上默认显示渲染后的javadoc）。当你意识到自己写的注释在描述说明中会渲染出一个怎样的效果时，你就下意识地按照规范去维护好这些注释，使之显示成一个美观并且有效的描述说明。

需要强调的第二件事是，javadoc应该是在设计接口和编写代码的同时去撰写，而不应该是事后再去做补救，因为开发的过程中思维会比较缜密，考虑得会更周全，事后可能会忘记很多的细节，描述会有各种的疏漏，这两种情况下编写出来的注释内容质量是不在一个次元上的。而且，参考`TDD`（测试驱动开发）的说法，先编写注释其实也可以起到协助梳理、提供清晰思路的效果。

## javadoc常用标签说明

以下对一些常用的标签进行说明。

### @author name-text

`@author`标注在 __类__ 上，用于标识一个类的作者，`name-text`即该作者的个人标识。一般不需要标注在方法上，我们可以通过Git History来了解各部分更改的作者。

### @version version-text

`@version`指定 __类__ 的版本，`version-text`为具体的版本号。

```java
/**
 * @version v1.0.0
 */
```

### @since since-text

`@since`标记开始引入该特定变化的所处的阶段，一般后面的`since-text`为当前版本号，代表该变化产生于当前这个版本。

```java
/**
 * @since v1.0.0
 */
```

### @deprecated deprecated-text

`@deprecated`用于说明一个类或成员已过期，`deprecated-text`对过期原因、代替的类或成员等进行一个详细的说明。一般被该标签标注的类或成员后续新的代码不再对其进行使用，保留仅作为过渡期兼容旧版本代码，后续一般会进行移除。

### @see reference

`@see`指定了一个到另一个类或成员的链接，其一般的写法为`@see package.class#member label`，其中`package`为成员的完整包名，`class`为类名，`member`为成员名，可以是属性、方法，成员用`#`来标志。最后的`label`用于把前面的一长串字符显示为自定义的文字，相当于昵称。

```java
/**
 * 类
 * @see java.util.HashMap
 */

/**
 * 类内属性
 * @see java.util.HashMap#size
 */

/**
 * 无参方法
 * @see java.util.HashMap#values()
 */

/**
 * 有参方法
 * @see java.util.HashMap#putAll(Map)
 */

/**
 * 使用label代替方法名
 * @see java.util.HashMap#putAll(Map) putAll
 */
```

### {@link reference}

`{@link}`为段中插入到另一个引用的链接，注意需要使用`{}`括起来。写法为`{@link package.class#member label}`，基本与`@see`一致。

```java
/**
 * The {@link #containsKey containsKey} operation may be used to
 * distinguish these two cases.
 */
```

### {@linkplain reference}

`{@linkplain}`的功能和使用都与`{@link}`完全一致，唯一的区别是`{@link}`显示出的样式是代码的样式，而`{@linkplain}`显示的则是普通文本的样式。

### {@value reference}

`{@value}`用于引入某个静态常量，写法为`{@value package.class#field}`，其中`field`为静态常量名，而最后显示在描述中的为静态常量的值。

### {@code text}

`{@code}`用于插入一段特定的字符，样式是以代码模式显示，一般用以强调一些变量名、常量值、特殊意义的字符、枚举量等。

```java
/**
 * Returns the value to which the specified key is mapped,
 * or {@code null} if this map contains no mapping for the key.
 */
```

### {@inheritDoc}

`{@inheritDoc}`用在重写的方法或者子类上，用于继承父类的javadoc，然后子类或者方法可以在这个基础上再加入自己的注释进行拓展。

### @param parameter-name description

`@param`用于说明一个方法的参数，格式为`@param parameter-name description`，其中`parameter-name`为参数名，`description`为对参数的详细描述。

```java
/**
 * @param  initialCapacity the initial capacity
 * @param  loadFactor      the load factor
 */
```

### @return description

`@return`用于说明返回值的类型以及详细描述。

```java
/**
 * @return the number of key-value mappings in this map
 */
```

### @throws/@exception class-name description

`@throws`和`@exception`两个标签是一致的，用来标志一个类抛出的异常。写法为`@throws class-name description`和`@exception class-name description`，其中`class-name`为所抛出异常的class，`description`为对抛出异常的描述。

```java
/**
 * @throws NullPointerException if the specified map is null
 */
```

## 标签

在写注释的时候，如果没有意识的话就会按照普通文本的方式去写，包括换行、进退格这些，但是这样不但显示出来的效果不好，格式还会被代码自动格式化的操作弄乱。首先，javadoc的渲染是以html的方式进行解析，所以对于换行退格等格式，普通文本出来的效果肯定不好，我们需要在文本中适当添加一些常用的html标签，来让代码注释的样式显示得整洁美观。比如换行`</br>`、新段落`<p>`、原样渲染`<pre>`、分点`<ul><li>`等等。（当然，如果你并不擅长用html的方式来写文档，并且没有非常追求原生性，是可以使用`Markdown Doclet for Javadoc`这个插件，使用Markdown替换html的。）

```java
/**
 * First paragraph.
 * <p><ul>
 * <li>the first item
 * <li>the second item
 * <li>the third item
 * </ul><p>
 * Third paragraph.
 */
```

## 样例

### 类

对于类，一般需要对类的作用进行描述，并且提供作者、版本等信息，最简单的写法如下：

```java
/**
 * 现货实例Controller
 *
 * @author xxx
 * @version v1.0.0
 * @since v1.0.0
 */
 public class SpotInstanceController {}
```

### 属性成员

属性成员的注释一般直接进行描述即可：

```java
/**
 * The Spot instance service.
 */
private SpotInstanceService spotInstanceService;
```

### 方法

对于方法，我们需要进行详细的描述，包括功能描述、入参说明、返回结果说明、抛出异常说明。

```java
/**
 * 获取现货实例详情
 *
 * @param tmSceneDateKey 经济场景日期key
 * @param tradeNum       交易序号
 * @return 现货实例详情对象
 * @throws Exception the exception
 */
public Object getSpotInstanceDetail(String tmSceneDateKey,Integer tradeNum) throws Exception {}
```

## 重点说明

上面给出的例子都是简例，但是实际撰写的时候必须要详细描述清楚，尤其是功能描述、入参、返回结果这几个部分。

- 功能描述要表达清楚这个方法的作用、适用场景等；
- `@param`和`@return`应该清楚说明数据的类型和取值范围等，如果是对象的话最好可以说明对象中有没有哪些属性是空的、哪些是不需要填的这种粒度的说明（因为一个对象的类会用在多个地方，并不是完全为某一个方法量身定造的，比如一个对象在add操作时是不需要id的，但是在update的时候则必须填写id）；如果是Map这类数据也最好能说明各种情况会返回的key的意义以及value的范围等。

简而言之，尽量做到看着注释可以理解整个方法的作用、如何去调用、返回结果的真实结构、有那些不可预测的运行结果，这样才能让你的代码更加清晰，更进一步用好设计模式等，把你的代码写得像诗一般优雅（事实上JDK源码的注释可以作为一份教科书般的参考）。
