# 星球秀场 移动团队 Objective-C 规范指南


## 目录

* [点语法](#点语法)
* [间距](#间距)
* [条件判断](#条件判断)
* [三目运算符](#三目运算符)
* [方法](#方法)
* [方法调用](#方法调用)
* [Init方法](#Init方法)
* [类构造方法](#类构造方法)
* [Getter & Setter](#Getter & Setter)
* [变量](#变量)
* [命名](#命名)
* [代理命名](#代理命名)
* [注释](#注释)
* [字面量](#字面量)
* [CGRect 函数](#CGRect-函数)
* [常量](#常量)
* [枚举类型](#枚举类型)
* [位掩码](#位掩码)
* [私有属性](#私有属性)
* [图片命名](#图片命名)
* [布尔](#布尔)
* [单例](#单例)
* [导入](#导入)
* [Xcode 工程](#Xcode-工程)

## 点语法

应该 **始终** 使用点语法来访问或者修改属性，访问其他实例时首选括号。

**推荐：**
```objc
view.backgroundColor = UIColor.orangeColor;
UIApplication.sharedApplication.delegate;
```

**反对：**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
[UIApplication sharedApplication].delegate;
```

## 间距

* 使用Xcode默认缩进，即（tab=4空格）缩进。
*  可以使用Xcode中的 re-indent 定期对代码格式进行自动整理。

## 方法大括号
* 方法的大括号和其他的大括号（`if`/`else`/`switch`/`while` 等等）始终和声明在同一行开始，在新的一行结束。

**推荐：**
```objc
if (condition) {
    [obj doSomething];
}
else {
    [obj doSomethingElse];
}
```
* 方法之间应该正好空一行，这有助于视觉清晰度和代码组织性。
* `@synthesize` 和 `@dynamic` 在实现中每个都应该占一个新行。


## 条件判断

条件判断主体部分应该始终使用大括号括住，即使它可以不用大括号（例如它只需要一行），以此来防止出错：
* 在添加第二行（代码）并希望它是 if 语句的一部分时造成逻辑错误。
* 当 if 语句里面的一行被注释掉，下一行就会在不经意间成为了这个 if 语句的一部分，造成错误。

此外，这种风格也更符合所有其他的条件判断，因此也更容易检查。

**推荐：**
```objc
if (!error) {
    return success;
}
```
或

```objc
if (!error) { return success };
```

**反对：**
```objc
if (!error)
    return success;
```

或

```objc
if (!error) return success;
```

### 三目运算符

三目运算符，? ，只有当它可以增加代码清晰度或整洁时才使用。单一的条件都应该优先考虑使用。多条件时通常使用 if 语句会更易懂，或者重构为实例变量。

**推荐：**
```objc
result = a > b ? x : y;
```

**反对：**
```objc
result = a > b ? x = c > d ? c : d : y;
```


## 方法

方法应在 -/+ 符号后应该有一个空格。方法片段之间也应该有一个空格。

**推荐：**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```

## 方法调用

**推荐：**

方法调用时所有参数应该在同一行：
```objc
[myObject doFooWith:arg1 name:arg2 error:arg3];
```
或者每行一个参数，以冒号对齐：
```objc
[myObject doFooWith:arg1 
               name:arg2 
              error:arg3];
```

**反对：**
```objc
[myObject doFooWith:arg1 name:arg2 // some lines with >1 arg 
              error:arg3];

[myObject doFooWith:arg1 
               name:arg2 error:arg3];

[myObject doFooWith:arg1 
          name:arg2 // aligning keywords instead of colons 
          error:arg3];
```

## Init方法
Init方法应该遵循Apple生成代码模板的命名规则，返回类型应该使用instancetype而不是id。
```objc
- (instancetype)init {  
  self = [super init];  
  if (self) {  
      // ...  
  }  
  return self;  
}  
```

## 类构造方法
当类构造方法被使用时，它应该返回类型是instancetype而不是id。这样确保编译器正确地推断结果类型。
```objc
@interface Airplane  
+ (instancetype)airplaneWithType:(RWTAirplaneType)type;  
@end 
```

## Getter & Setter
当我们需要重写Getter和Setter的时候，需要把Getter和Setter放在代码的最底部，避免过多的Getter和Setter拥挤在头部而把主逻辑代码被放在下方，不利于读者阅读代码。

## 变量

变量名应该尽可能命名为描述性的。除了 `for()` 循环外，其他情况都应该避免使用单字母的变量名。
星号表示指针属于变量，例如：`NSString *text` 不要写成 `NSString* text` 或者 `NSString * text` ，常量除外。

尽量定义属性来代替直接使用实例变量。除了初始化方法（`init`， `initWithCoder:`，等）， `dealloc` 方法和自定义的 setters 和 getters 内部，应避免直接访问实例变量。


**推荐：**

```objc
@interface JWSection : NSObject

@property (nonatomic) NSString *name;

@end
```

**反对：**

```objc
@interface JWSection : NSObject {
    NSString *name;
}
```

#### 变量限定符

当涉及到在 ARC 中被引入变量限定符时，
限定符 (`__strong`, `__weak`, `__unsafe_unretained`, `__autoreleasing`) 应该位于星号和变量名之间，如：`NSString * __weak text`。

## 命名

尽可能遵守苹果的命名约定，尤其那些涉及到内存管理规则的。

长的和描述性的方法名和变量名都不错。

**推荐：**

```objc
UIButton *settingsButton;
```

**反对：**

```objc
UIButton *setBut;
```
为了代码清晰，常量应该使用相关类的名字作为前缀并使用驼峰命名法。

**推荐：**

```objc
static const NSTimeInterval JWHomeViewControllerNavigationFadeAnimationDuration = 0.3;
```

**反对：**

```objc
static const NSTimeInterval fadetime = 1.7;
```

属性和局部变量应该使用驼峰命名法并且首字母小写。

为了保持一致，实例变量应该使用驼峰命名法命名，并且首字母小写，以下划线为前缀。这与 LLVM 自动合成的实例变量相一致。
**如果 LLVM 可以自动合成变量，那就让它自动合成。**

**推荐：**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**反对：**

```objc
id varnm;
```

宏命名部分分为几种情况
* 不带参数的通用常量宏，全部大写，单词间用` _` 分隔。
```objc
#define THIS_IS_AN_MACRO @"THIS_IS_AN_MACRO"
```
* 不带参数的私有常量宏，以字母 k 开头，后面遵循大驼峰命名。
```objc
#define kWidth self.frame.size.width
```
* 带参数的，以小驼峰命名。
```objc
#define getImageUrl(url) [NSURL URLWithString:[NSString stringWithFormat:@"%@%@",kBaseUrl,url]]
```

## 代理命名
* 回调方法的参数只有类自己的情况，方法名要符合实际含义, 如:
```objc
-(NSInteger)numberOfSectionsInTableView:(UITableView*)tableView
```

* 类的实例必须为回调方法的参数之一, 如：
```objc
-(NSInteger)tableView:(UITableView*)tableView numberOfRowsInSection:(NSInteger)section
```

* 以类的名字开头(回调方法存在两个以上参数的情况)以表明此方法是属于哪个类的, 如:
```objc
-(UITableViewCell*)tableView:(UITableView*)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
```

* 使用did和will具有状态标识的词通知Delegate已经发生的变化或将要发生的变化, 如:
```objc
-(NSIndexPath*)tableView:(UITableView*)tableView willSelectRowAtIndexPath:(NSIndexPath*)indexPath;
-(void)tableView:(UITableView*)tableView didSelectRowAtIndexPath:(NSIndexPath*)indexPath;
```

## 注释

当需要的时候，注释应该被用来解释 **为什么** 特定代码做了某些事情。所使用的任何注释必须保持最新否则就删除掉。

如果做不到命名尽量的见名知意的话，就可以适当的添加一些注释或者mark。

通常应该避免一大块注释，代码就应该尽量作为自身的文档，只需要隔几行写几句说明。这并不适用于那些用来生成文档的注释。

* 属性注释
```objc
/// 学生
@property (nonatomic, strong) Student *student;
```
* 方法注释
Xcode8以后可以使用快捷键`[cmd + alt +/]`，系统会根据方法的参数和返回值类型生成对应的多行注释
```objc
/** 
* @brief 登录方法
*
* @param name 姓名
* @param number 学号
*
* @return
*/
- (void)loginWithStudentName:(NSString *)name number:(NSNumber *)number;
```
* 方法集注释
使用pragma注释：
```objc
#pragma mark 显示文字，无分割线
- (void)test1;

#pragma mark - 显示分割线，并在分割线下方显示文字
- (void)test2;

可通过将注释格式保存为代码块的方式，提高注释的效率和代码可读性
```
## 字面量

每当创建 `NSString`， `NSDictionary`， `NSArray`，和 `NSNumber` 类的不可变实例时，都应该使用字面量。要注意 `nil` 值不能传给 `NSArray` 和 `NSDictionary` 字面量，这样做会导致崩溃。

**推荐：**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone": @"Kate", @"iPad": @"Kamal", @"Mobile Web": @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**反对：**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

## CGRect 函数

当访问一个 `CGRect` 的 `x`， `y`， `width`， `height` 时，应该使用[`CGGeometry` 函数][CGRect-Functions_1]代替直接访问结构体成员。苹果的 `CGGeometry` 参考中说到：

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**推荐：**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**反对：**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

## 常量

常量首选内联字符串字面量或数字，因为常量可以轻易重用并且可以快速改变而不需要查找和替换。常量应该声明为 `static` 常量而不是 `#define` ，除非非常明确地要当做宏来使用。

**推荐：**

```objc
static NSString * const JWAboutViewControllerCompanyName = @"XXX Company";

static const CGFloat JWImageThumbnailHeight = 50.0;
```

**反对：**

```objc
#define CompanyName @"XXX Company"

#define thumbnailHeight 2
```

## 枚举类型

当使用 `enum` 时，建议使用新的基础类型规范，因为它具有更强的类型检查和代码补全功能。现在 SDK 包含了一个宏来鼓励使用使用新的基础类型 - `NS_ENUM()`

**推荐：**

```objc
typedef NS_ENUM(NSInteger, NYTAdRequestState) {
    NYTAdRequestStateInactive,
    NYTAdRequestStateLoading
};
```

## 位掩码

当用到位掩码时，使用 `NS_OPTIONS` 宏。

**举例：**

```objc
typedef NS_OPTIONS(NSUInteger, UISwipGestureRecognizerDirection) {
    UISwipGestureRecognizerDirectionUp    = 1 << 0,
    UISwipGestureRecognizerDirectionLeft  = 1 << 1,
    UISwipGestureRecognizerDirectionDown  = 1 << 2,
    UISwipGestureRecognizerDirectionRight = 1 << 3
};
```


## 私有属性

私有属性应该声明在类实现文件的延展（匿名的类目）中。

**推荐：**

```objc
@interface JWAdvertisement ()

@property (nonatomic, strong) UIView *companyAdView;
@property (nonatomic, strong) UIView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```

## 图片命名

图片名称应该被统一命名以保持组织的完整。它们应该被命名为一个说明它们用途的驼峰式字符串，其次是自定义类或属性的无前缀名字（如果有的话），然后进一步说明颜色 和/或 展示位置，最后是它们的状态。

**推荐：**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` 和 `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` 和 `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

图片目录中被用于类似目的的图片应归入各自的组中。


## 布尔

因为 `nil` 解析为 `NO`，所以没有必要在条件中与它进行比较。不要直接和 `YES` 进行比较，因为 `YES` 被定义为 1，而 `BOOL` 可以多达 8 位。

这使得整个文件有更多的一致性和更大的视觉清晰度。

**推荐：**

```objc
if (!someObject) {
}
```

**反对：**

```objc
if (someObject == nil) {
}
```

-----

**对于 `BOOL` 来说, 这有两种用法:**

**推荐：**

```objc
if (isAwesome)
if (![someObject boolValue])
```

**反对：**

```objc
if ([someObject boolValue] == NO)
if (isAwesome == YES) // 永远别这么做
```

-----

如果一个 `BOOL` 属性名称是一个形容词，属性可以省略 “is” 前缀，但为 get 访问器指定一个惯用的名字，例如：

```objc
@property (assign, getter=isEditable) BOOL editable;
```


## 单例

单例对象应该使用线程安全的模式创建共享的实例。

```objc
+ (instancetype)sharedInstance {
    static id sharedInstance = nil;

    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        sharedInstance = [[self alloc] init];
    });

    return sharedInstance;
}
```

## 导入   

如果有一个以上的 import 语句，就对这些语句进行分组。每个分组的注释是可选的。   
注：对于模块使用语法。   

```objc   
// Frameworks
@import QuartzCore;

// Models
#import "JWUser.h"

// Views
#import "JWButton.h"
#import "JWUserView.h"
```  

## Xcode 工程

为了避免文件杂乱，物理文件应该保持和 Xcode 项目文件同步。Xcode 创建的任何组（group）都必须在文件系统有相应的映射。为了更清晰，代码不仅应该按照类型进行分组，也可以根据功能进行分组。


如果可以的话，打开 target Build Settings 中 "Treat Warnings as Errors" 以及一些额外的警告。如果你需要忽略指定的警告,使用 Clang 的编译特性。
