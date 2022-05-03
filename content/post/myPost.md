---
title: "Post"
date: "2018-06-02"
categories:
- "技术"
tags:
- "Rust"
- "Programing"
keywords:
  - "關鍵詞1"
  - "關鍵詞2"
toc: false
---

# Net 基礎觀念

## .Net 三種區塊記憶體位置 Heap , Stack , Globle??
棧(Stack)：由編譯器自動分配、釋放。
在函數體中定義的變數通常在棧上。
堆(Heap)：一般由程序員分配釋放。用new、malloc等分配內存函數分配得到的就是在堆上。
存放在棧(Stack)中時要管存儲順序，保持著先進後出的原則，他是一片連續的內存域，有系統自動分配和維護；

堆(Heap)：是無序的，他是一片不連續的記憶體空間，有用戶自己來控制和釋放，如果用戶自己不釋放的話，當記憶體達到一定的特定值時，通過垃圾回收器(GC)來回收。
棧(Stack)內存無需我們管理，也不受GC管理。當棧(Stack)頂元素使用完畢，立馬釋放。而堆(Heap)則需要GC清理。
使用引用類型的時候，一般是對指針進行的操作而非引用類型對象本身。但是值類型則操作其本身

Stack ：
1. 存放值類型的值(int,double,float,byte .....)
2. 存放參考類型記憶體位置(Pointer指針)
3. 編譯期間就知道運行時的記憶體位置

Globle：
1. 全域記憶體
2. 主要存放全域變數或宣告為static的靜態變數

Heap ：
1. 存放參考類型(可動態產生的空間)
2. 運行期間分配記憶體位置(這就是為什麼參考類型的類別要new)
3. 基本是Class關鍵字

https://dotblogs.com.tw/daniel/2017/10/20/174725

## C#中的委派是什麼？事件是不是一種委派？

委派(delegate)的本質是一個類，委派(delegate)是將一種方法作為參數代入到另一種方法。
事件(Event)是委派的實例，事件是一種特殊的委派。 
譬如:onclick事件中的參數就是一種方法。

https://hackmd.io/@BKLiang/csharp_delegate_event

## C#靜態建構子特點是什麼？

最先被執行的建構函式，且在一個類別(Class)裡只允許有一個無參的靜態建構函式
執行順序：靜態變數>靜態建構函式>實例變數>實例建構函式

## CTS、CLS、CLR分別作何解釋

CTS：通用語言系統。 CLS：通用語言規範。 CLR：公共語言運行庫。
CTS：Common Type System 通用類型系統。 Int32、Int16→int、String→string、Boolean→bool。
每種語言都定義了自己的類型，.NET通過CTS提供了公共的類型，然後翻譯生成對應的.NET類型。
CLS：Common Language Specification 通用語言規範。不同語言語法的不同。每種語言都有自己的語
法，.NET通過CLS提供了公共的語法，然後不同語言翻譯生成對應的.NET語法。
CLR：Common Language Runtime 公共語言運行時，就是GC、JIT等這些。有不同的CLR，比如服務器
CLR、Linux CLR（Mono）、Silverlight CLR(CoreCLR)。相當於一個發動機，負責執行IL。

https://dotblogs.com.tw/plus/2020/05/21/212036

## C#中什麼是實值型別(Value Type)與參考型別(Reference Type)？
實值型：struct、enum、int、float、char、bool、decimal
參考型別：class、delegate、interface、array、object、string

## 請詳述在C#中類別(class)與結構(struct)的異同？
class可以被實例化,屬於參考類型,
class可以實現介面(interface)和單繼承其他類,還可以作為基類型,是分配在內存的堆(Heap)上的
struct屬於值類型,不能作為基類型,但是可以實現接口,是分配在內存的棧(Stack)上的.


| 不同之處 | Struct | Class |
| -------- | -------- | -------- |
| 型態     | 值類型	     | 參考類型     |
| 記憶體位置     | Stack上     | Heap上     |
| 繼承     | 只能實現Interface     | 可繼承也可實現Interface     |
| NULL     | 不能NULL     | 可為NULL     |

https://dotblogs.com.tw/daniel/2018/02/22/135011


## new關鍵字的作用

運算符：創建對象實例
修飾符：在派生類定義一個重名的方法，隱藏掉基類方法
約束：泛型約束定義，約束可使用的泛型類型

## int?和int有什麼區別
int？為可空類型，默認值可以是null
int默認值是0
int?是通過int裝箱為參考類型實現

## C#中值值類型遞與參考類型傳遞的區別是什麼？

值類型傳遞時，系統會先分配一個記憶體空間，並將值類型的值複製到新分配的記憶體空間，此後，被調用方法中變數的數值被修改，都不會引想到原本的變數的數值；
![](https://i.imgur.com/azonaqP.png)

參考類型傳遞時，系統不是將實參本身的值複製後傳遞給形參，而是將其引用值（即地址值）傳遞給形參，
因此，形參所引用的該地址上的變量與傳遞的實參相同，方法體內相應形參值得任何改變都將影響到作為引用傳遞的實參。
簡而言之，按值傳遞不是值參數是值類型，而是指形參變量會復制實參變量，也就是會在棧上多創建一個相同的變量。而按引用傳遞則不會。可以通過 ref 和 out 來決定參數是否按照引用傳遞。
![](https://i.imgur.com/0lOga9g.png)

https://ithelp.ithome.com.tw/articles/10213241


## C#中參數傳遞 ref 與 out 的區別？

（1）ref指定的參數在函數調用時必須先初始化，而out不用
（2）out指定的參數在進入函數時會清空自己，因此必須在函數內部進行初始化賦值操作，而ref不用

總結：ref可以把值傳到方法裡，也可以把值傳到方法外；out只可以把值傳到方法外
注意：string作為特殊的引用類型，其操作是與值類型看齊的，若要將方法內對形參賦值後的結果傳遞
出來，需要加上ref或out關鍵字。

## C#中什麼是裝箱(Boxing)和拆箱(UnBoxing)？
裝箱：把實值型別轉換成參考型別
拆箱：把參考型別轉換成實值型別

裝箱：對實值型別在堆中分配一個對象實例，並將該值複製到新的對像中。
（1）第一步：新分配託管堆內存(大小為值類型實例大小加上一個方法表指針。
（2）第二步：將實值型別的實例字段拷貝到新分配的內存中。
（3）第三步：返回託管堆中新分配對象的地址。這個地址就是一個指向對象的引用了。
拆箱：檢查對象實例，確保它是給定值類型的一個裝箱值。將該值從實例複製到值類型變量中。
在裝箱時是不需要顯式的類型轉換的，不過拆箱需要顯式的類型轉換。
int i=0;
System.Object obj=i; //這個過程就是裝箱！就是將 i 裝箱！
int j=(int)obj;//這個過程 obj 拆箱！
裝箱
![](https://i.imgur.com/epvBJux.png)
拆箱
![](https://i.imgur.com/OcikpXv.png)

https://www.itread01.com/content/1548893704.html

## C#實現多態的過程中 overload 重載 與override 重寫的區別？

override 重寫與 overload 重載的區別。
重載(overload)是方法的名稱相同。參數或參數類型不同，進行多次重載以適應不同的需要
override 是進行基類中函數的重寫。實現多態。
重載：是方法的名稱相同，參數或參數類型不同；重載是物件導向過程的概念。
重寫：是對基類中的虛方法進行重寫。重寫是物件導向的概念。

https://wayne265265.pixnet.net/blog/post/115533452-%E3%80%90%E6%95%99%E5%AD%B8%E3%80%91override-%E8%88%87-overload-%E7%9A%84%E5%B7%AE%E5%88%A5

## C#中static關鍵字的作用？

對類有意義的字段和方法使用static關鍵字修飾，稱為靜態成員，通過類名加訪問操作符“.”進行訪問; 對類的實例有意義的字段和方法不加static關鍵字，稱為非靜態成員或實例成員。
注: 靜態字段在內存中只有一個拷貝，非靜態字段則是在每個實例對像中擁有一個拷貝。而方法無論是否
為靜態，在內存中只會有一份拷貝，區別只是通過類名來訪問還是通過實例名來訪問。

缺點:
* 無謂地佔住記憶體過久
* 直接耦合造成無法獨立進行單元測試
* 無法享用物件導向設計的好處（繼承的重用與擴充、介面的可抽換性、多型的擴充性）

https://dotblogs.com.tw/im_sqz777/2017/11/14/235408

## C# 成員變量和成員函數前加static的作用？
它們被稱為常成員變量和常成員函數，又稱為類成員變量和類成員函數。
分別用來反映類的狀態。
比如類成員變量可以用來統計類實例的數量，類成員函數
負責這種統計的動作。不用new

https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/628644/

## C#中索引器的實現過程，是否只能根據數字進行索引，請描述一下

C#通過提供索引器，可以像處理數組一樣處理對象。特別是屬性，每一個元素都以一個get或set方法暴露。索引器不單能索引數字（數組下標），還能索引一些HASHMAP的字符串，所以，通常來說，C#中類的索引器通常只有一個，就是THIS，但也可以有無數個，只要你的參數列表不同就可以了索引器和返
回值無關, 索引器最大的好處是使代碼看上去更自然，更符合實際的思考模式。
微軟官方一個示例：
索引器允許類或結構的實例按照與數組相同的方式進行索引。索引器類似於屬性，不同之處在於它們的
訪問器採用參數。在下面的示例中，定義了一個泛型類（class SampleCollection ），並為其提供了簡
單的 get 和 set訪問器 方法（作為分配和檢索值的方法）。 Program 類為存儲字符串創建了此類的一個
實例。

https://www.runoob.com/csharp/csharp-indexer.html

## C#中 abstract class和interface有什麼區別?

abstract class abstract 聲明抽像類抽象方法，一個類中有抽象方法，那麼這個類就是抽像類了。所謂的抽象方法，就是不含主體（不提供實現方法），必須由繼承者重寫。因此，抽像類不可實例化，只能通
過繼承被子類重寫。
interface 宣告介面，只提供一些方法規約，在C#8之前的版本中不提供任何實現，在C#9版本也可以支持介面的實體；不能用public、abstract等修飾，無字段、常量，無構造函數
兩者區別：
1.interface中不能有字段，而abstract class可以有; 
2.interface中不能有public等修飾符，而abstract class 可以有。 
3.interface 可以實現多繼承

## C#中用sealed修飾的類有什麼特點？
密封，不能繼承

## 字串中string str=null和string str=""和string str=string.Empty的區別

string.Empty相當於“”,Empty是一個靜態只讀的字段。 string str="" ,初始化對象，並分配一個空字符串
的內存空間 string str=null,初始化對象，不會分配內存空間

## byte b = 'a'; byte c = 1; byte d = 'ab'; byte e = '啊'; byte g = 256; 这些变量有些错误是错在哪里?

本題考查的是數據類型能承載數據的大小。
1byte =8bit，1個漢字=2個byte，1個英文=1個byte=8bit
所以bc是對的，deg是錯的。 'a'是char類型，a錯誤
java byte取值範圍是-128~127, 而C#裡一個byte是0~255

## string和StringBuilder的區別,兩者性能的比較

都是引用類型，分配再堆上StringBuilder默認容量是16，可以允許擴充它所封裝的字符串中字符的數量.每個StringBuffer對像都有一定的緩衝區容量，當字符串大小沒有超過容量時，不會分配新的容量，當字符串大小超過容量時，會自動增加容量。
對於簡單的字符串連接操作，在性能上stringbuilder不一定總是優於strin因為stringbulider對象的創建
也消耗大量的性能，在字符串連接比較少的情況下，過度濫用stringbuilder會導致性能的浪費而非節
約，只有大量無法預知次數的字符串操作才考慮stringbuilder的使用。從最後分析可以看出如果是相對
較少的字符串拼接根本看不出太大差別。
Stringbulider的使用，最好制定合適的容量值，否則優於默認值容量不足而頻繁的進行內存分

## 什麼是擴充方法？

一句話解釋，擴充方法使你能夠向現有類別“添加”方法，無需修改類別
條件：按擴充方法必須滿足的條件，
1.必須要靜態類中的靜態方法
2.第一個參數的類型是要擴展的類型，並且需要添加this關鍵字以標識其為擴展方法

建議：通常，只在不得已的情況下才實現擴展方法，並謹慎的實現
使用：不能通過類名調用，直接使用類型來調用

## 特性是什麼？如何使用？

特性與屬性是完全不相同的兩個概念，只是在名稱上比較相近。 Attribute特性就是關聯了一個目標對象
的一段配置信息，本質上是一個類，其為目標元素提供關聯附加信息，這段附加信息存儲在dll內的元數
據，它本身沒什麼意義。運行期以反射的方式來獲取附加信息

## 什麼叫應用程序域(AppDomain)

一種邊界，它由公共語言運行庫圍繞同一應用程序範圍內創建的對象建立（即，從應用程序入口點開始，沿著對象激活的序列的任何位置）。
應用程序域有助於將在一個應用程序中創建的對象與在其他應用程序中創建的對象隔離，以使運行時行為可以預知。
在一個單獨的進程中可以存在多個應用程序域。應用程序域可以理解為一種輕量級進程。起到安全的作用。佔用資源小
![](https://i.imgur.com/7uoAekV.png)
http://vito-note.blogspot.com/2012/03/appdomain.html

## byte a =255;a+=5;a的值是多少？

byte的取值範圍是-2的8次方至2的8次方-1，-256至258，a+=1時，a的值時0，a+=5時，a的值是0，所
以a+=5時，值是4

## const和readonly有什麼區別？

都可以標識一個常量。主要有以下區別：
1、初始化位置不同。 const必須在聲明的同時賦值；readonly即可以在聲明處賦值;
2、修飾對像不同。 const即可以修飾類的字段，也可以修飾局部變量；readonly只能修飾類的字段
3、const是編譯時常量，在編譯時確定該值；readonly是運行時常量，在運行時確定該值。
4、const默認是靜態的；而readonly如果設置成靜態需要顯示聲明
5、修飾引用類型時不同，const只能修飾string或值為null的其他引用類型；readonly可以是任何類型。

## 分析下面代碼，a、b的值是多少？
```csharp=
string strTmp = "a1某某某";
int a = System.Text.Encoding.Default.GetBytes(strTmp).Length;
int b = strTmp.Length
```
分析：一個字母、數字佔一個byte，一個中文佔佔兩個byte，所以a=8,b=5

## String s = new String(“xyz”);創建了幾個String Object?

兩個對象，一個是“xyz”,一個是指向“xyz”的引用對象s。

## c#可否對內存直接操作

C#在unsafe 模式下可以使用指針對內存進行操作, 但在託管模式下不可以使用指針，C#NET默認不運行帶指針的，需要設置下，選擇項目右鍵->屬性->選擇生成->“允許不安全代碼”打勾->保存

## 什麼是強型別，什麼是弱型別？哪種更好些？為什麼?

強型別是在編譯的時候就確定了資料型別，在執行時資料型別不能更改，而弱型別在執行的時候才會確定型別。沒有好不好，二者各有好處，強型別安全，因為它事先已經確定好了，而且效率高。一般用於編譯型編程語言，如c++,java,c#,pascal等,弱型別相比而言不安全，在運行的時候容易出現錯誤，但它靈活，多用於解釋型編程語言，如javascript等

## Math.Round(11.5)等於多少? Math.Round(-11.5)等於多少?

使用指定的舍入慣例，將雙精確度浮點值四捨五入為指定的小數位數。
```csharp=
Math.Round(11.5)=12
Math.Round(-11.5)=-12
```

https://docs.microsoft.com/zh-tw/dotnet/api/system.math.round?view=net-6.0

## &和&&的區別

相同點&和&&都可作邏輯與的運算符，表示邏輯與（and），當運算符兩邊的表達式的結果都為true時，其結果才為true，否則，只要有一方為false，則結果為false。 
（ps：當要用到邏輯與的時候&是毫無意義，&本身就不是乾這個的）

```csharp=
string strTmp = "a1某某某";
int a = System.Text.Encoding.Default.GetBytes(strTmp).Length;
int b = strTmp.Length;
//不同點
if(loginUser != null && string.IsnullOrEmpty(loginUser.UserName))
```

&&具有短路的功能，即如果第一個表達式為false，則不再計算第二個表達式，對於上面的表達式，當
loginUser為null時，後面的表達式不會執行，所以不會出現NullPointerException如果將&&改為&，則
會拋出NullPointerException異常。 
（ps：所以說當要用到邏輯與的時候&是毫無意義的）
& 是用作位運算的。

**總結**
> &是位運算，返回結果是int類型 
> &&是邏輯運算，返回結果是bool類型

## i++和++i有什麼區別？

1. i++是先賦值，然後再自增；++i是先自增，後賦值。
2. i=0，i++=0，++i=1； 
```csharp=
Console.WriteLine(++i==i++); 
```
結果位true

## as和is的區別

as在轉換的同時判斷兼容性，如果無法進行轉換，返回null（沒有產生新的對象），as轉換是否成功
判斷的依據是是否位null is只是做類型兼容性判斷，並不執行真正的類型轉換，返回true或false，對象為null也會返回false。
as比is效率更高，as只需要做一次類型兼容檢查

## 簡述C#成員修飾符

abstract:指示該方法或屬性沒有實現。
const:指定域或局部變量的值不能被改動。
event:聲明一個事件。
extern:指示方法在外部實現。
override:對由基類繼承成員的新實現。
readonly:指示一個域只能在聲明時以及相同類的內部被賦值。
static:指示一個成員屬於類型本身,而不是屬於特定的對象。
virtual:指示一個方法或存取器的實現可以在繼承類中被覆蓋。

https://docs.microsoft.com/zh-tw/dotnet/csharp/language-reference/keywords/sealed
## 什麼是匿名類，有什麼好處？

不用定義、沒有名字的類，使用一次便可丟棄。
好處是簡單、隨意、臨時的。

```csharp=
var v = new { Amount = 108, Message = "Hello" };
```

https://docs.microsoft.com/zh-tw/dotnet/csharp/fundamentals/types/anonymous-types


## 說說什麼是逐字字符串

在普通字符串中，反斜杠字符是轉義字符。而在逐字字符串（Verbatim Strings）中，字符將被編程器按照原義進行解釋。使用逐字字符串只需在字符串前面加上 @ 符號。

```csharp=
// 逐字字符串：轉義符
var filename = @"c:\temp\newfile.txt";
Console.WriteLine(filenaame);
// 逐字字符串：多行文本
var multiLine = @"This is a
multiline paragraph.";
Console.WriteLine(multiLine);
// 非逐字字符串
var escapedFilename = "c:\temp\newfile.txt";
Console.WriteLine(escapedFilename);
```
OutPut:

```csharp=
c:\temp\newfile.txt
This is a
multiline paragraph.
c:   emp
ewfile.txt
```
逐字字符串中唯一不被原樣解釋的字符是雙引號。由於雙引號是定義字符串的關鍵字符，所以在逐字字
符串中要表達雙引號需要用雙引號進行轉義。

```csharp=
var str = @"""I don't think so"", he said.";
Console.WriteLine(str);
```

```csharp=
"I don't think so", he said.
```

在逐字字符串中也可以 $ 符號實現字符串內插值。

```csharp=
Testing \n 1 2 3
```

output:
```csharp=
Testing \n 1 2 3
```



## 列舉你知道的數字格式化轉換

可使用“0”和“#”佔位符進行補位。 “0” 表示位數不夠位數就補充“0”，小數部分如果位數多了則會四捨五
入；“#”表示佔位，用於輔助“0”進行補位。如下例：

```csharp=
 // “0”描述：占位符，如果可能，填充位
           Console.WriteLine(string.Format("{0:000000}", 1234)); // 结果：001234 
           // “#”描述：占位符，如果可能，填充位
           Console.WriteLine(string.Format("{0:######}", 1234)); // 结果：1234
           Console.WriteLine(string.Format("{0:#0####}", 1234)); // 结果：01234
           Console.WriteLine(string.Format("{0:0#0####}", 1234)); // 结果：
0001234 
内置快捷字母格式化用法
ToString 也可以自定义补零格式化：
           // "."描述：小数点
           Console.WriteLine(string.Format("{0:000.000}", 1234));       // 结果：
1234.000
           Console.WriteLine(string.Format("{0:000.000}", 4321.12543)); // 结果：
4321.125
           // ","描述：千分表示
           Console.WriteLine(string.Format("{0:0,0}", 1234567));   //结果：
1,234,567
           // "%"描述：格式化为百分数
           Console.WriteLine(string.Format("{0:0%}", 1234));       // 结果：
123400%
           Console.WriteLine(string.Format("{0:#%}", 1234.125));   // 结果：
123413%
           Console.WriteLine(string.Format("{0:0.00%}", 1234));     // 结果：
123400.00%
           Console.WriteLine(string.Format("{0:#.00%}", 1234.125)); // 结果：
123412.50%
           // E-科学计数法表
```

內置快捷字母格式化用法

```csharp=
// E-科學計數法表示
           Console.WriteLine((25000).ToString("E")); // 結果：2.500000E+004
           // C-貨幣表示，帶有逗號分隔符，默認小數點後保留兩位，四捨五入
           Console.WriteLine((2.5).ToString("C")); // 結果：￥2.50
           // D[length]-十進制數
           Console.WriteLine((25).ToString("D5")); // 結果：00025
           // F[precision]-浮點數，保留小數位數(四捨五入)
           Console.WriteLine((25).ToString("F2")); // 結果：25.00
           // G[digits]-常規，保留指定位數的有效數字，四捨五入
           Console.WriteLine((2.52).ToString("G2")); // 結果：2.5
           // N-帶有逗號分隔符，默認小數點後保留兩位，四捨五入
           Console.WriteLine((2500000).ToString("N")); // 結果：2,500,000.00
           // X-十六進制，非整型將產生格式異常
           Console.WriteLine((255).ToString("X")); // 結果
```

ToString 也可以自定義補零格式化：

```csharp=
Console.WriteLine((15).ToString("000"));             // 結果：015
           Console.WriteLine((15).ToString("value is 0"));       // 結果：value 
is 15
           Console.WriteLine((10.456).ToString("0.00"));         // 結果：10.46
           Console.WriteLine((10.456).ToString("00"));           // 結果：10
           Console.WriteLine((10.456).ToString("value is 0.0")); // 結果：value 
is 10.5
```

## 說說字符串拼接、字符串內插法

將數組中的字符串拼接成一個字符串：

```csharp=
 var parts = new[] { "Foo", "Bar", "Fizz", "Buzz"};
 var joined = string.Join(", ", parts);
 // joined = "Foo, Bar, Fizz, Buzz"

```

以下四種方式都可以達到相同的字符串拼接的目的：

```csharp=
string first = "Hello";
 stringsecond = "World";
 string foo = first + " " + second;
 string foo = string.Concat(first, " ", second);
 string foo = string.Format("{0} {1}", firstname, lastname);
 string foo = $"{firstname} {lastname}";
```
字符串內插法簡單用法：

```csharp=
var name = "World";
         var str =$"Hello, {name}!";
 // str = "Hello, World!"
```

帶日期格式化
```csharp=
var date = DateTime.Now;
 var str = $"Today is {date:yyyy-MM-dd}！";
```

補齊格式化（Padding）：

```csharp=
var number = 42; 
           // 向左補齊
           var str = $"The answer to life, the universe and everything is 
{number,5}.";
           // str = "The answer to life, the universe and everything is ___42." 
('_'表示空格) 
           // 向右補齊
           var str = $"The answer to life, the universe and everything is 
${number,-5}.";
           // str = "The answer to life, the universe and everything is 42___."
```

結合內置快捷字母格式化：
```csharp=
var amount = 2.5;
           var str = $"It costs {amount:C}";
           // str = "￥2.50" 
           var number = 42;
           var str = $"The answer to life, the universe and everything is 
{number,5:f1}.";
           // str = "The answer to life, the universe and everything is 
___42.1"
```

## 什麼是虛函數？什麼是抽象函數？

虛函數：沒有實現的，可以由子類繼承並重寫的函數。
抽象函數：規定其非虛子類必須實現的函數，必須被重寫。

## 什麼是WebService?

答：Web Service是基於網絡的、分佈式的模塊化組件，它執行特定的任務，遵守具體的技術規範，這
些
規範使得Web Service能與其他兼容的組件進行互操作。

## ADO.NET常用對像有哪些？

Connection：主要是開啟程序和數據庫之間的連接。沒有利用連接對象將數據庫打開，是無法從數據
庫中取得數據的。 Close和Dispose的區別，Close以後還可以Open，Dispose以後則不能再用。
Command：主要可以用來對數據庫發出一些指令，例如可以對數據庫下達查詢、新增、修改、刪除數
據等指令，以及調用存在數據庫中的存儲過程等。這個對像是架構在Connection 對像上，也就是
Command： 對像是通過在Connection對象連接到數據源。
DataAdapter：主要是在數據源以及DataSet 之間執行數據傳輸的工作，它可以透過Command 對像下
達命令後，並將取得的數據放入DataSet 對像中。這個對像是架構在Command對像上，並提供了許多
配合DataSet 使用的功能。
DataSet：這個對象可以視為一個暫存區（Cache），可以把從數據庫中所查詢到的數據保留起來，甚
至可以將整個數據庫顯示出來，DataSet是放在內存中的。 DataSet 的能力不只是可以儲存多個Table 而
已，還可以透過DataAdapter對象取得一些例如主鍵等的數據表結構，並可以記錄數據表間的關聯。
DataSet 對象可以說是ADO.NET 中重量級的對象，這個對象架構在DataAdapter對像上，本身不具備和
數據源溝通的能力；也就是說我們是將DataAdapter對象當做DataSet 對像以及數據源間傳輸數據的橋
梁。 DataSet包含若干DataTable、DataTableTable包含若干DataRow。
DataReader：當我們只需要循序的讀取數據而不需要其它操作時，可以使用DataReader 對象。
DataReader對像只是一次一次向下循序的讀取數據源中的數據，這些數據是存在數據庫服務器中的，而
不是一次性加載到程序的內存中的，只能（通過游標）讀取當前行的數據，而且這些數據是只讀的，並
不允許作其它的操作。因為DataReader 在讀取數據的時候限制了每次只讀取一條，而且只能只讀，所
以使用起來不但節省資源而且效率很好。使用DataReader 對象除了效率較好之外，因為不用把數據全
部傳回，故可以降低網絡的負載。

## 在ASP.NET中所有的自定義用戶控件都必須繼承自？

Control类

## 在.NET託管代碼總我們不必擔心內存洩漏，這是因為有了？

GC 垃圾收集器。

## 什麼是MVC模式

MVC(Model View Controller)模型－視圖－控制器
aspx就是View，視圖；Model：DataSet、Reader、對象；Controller：cs代碼。
MVC是典型的平行關係，沒有說誰在上誰在下的關係，模型負責業務領域的事情，視圖負責顯示的事
情，控制器把數據讀取出來填充模型後把模型交給視圖去處理。而各種驗證什麼的應該是在模型里處理
了。它強制性的使應用程序的輸入、處理和輸出分開。 MVC最大的好處是將邏輯和頁面分離。

## 能用foreach遍歷訪問的對象的要求

需要實現IEnumerable接口或聲明GetEnumerator方法的類型。

## 什麼是反射?

程序集包含模塊，而模塊又包括類型，類型下有成員，反射就是管理程序集，模塊，類型的對象，它能
夠動態的創建類型的實例，設置現有對象的類型或者獲取現有對象的類型，能調用類型的方法和訪問類
型的字段屬性。它是在運行時創建和使用類型實例。

## ORM中的延遲加載與直接加載有什麼異同？

延遲加載（Lazy Loading）只在真正需要進行數據操作的時候再進行加載數據，可以減少不必要的開
銷。

## 簡述Func與Action的區別？

Func是有返回值的委託，Action是沒有返回值的委託。

## 23種設計模式分別叫什麼名稱，如何分類？

分三類：
創建型，行為型，結構型；
創建型包含：
1.單例模式
2.工廠模式
3.建造者模式
4.原型模式
5.工廠方法模式
行為型包含：
1.策略模式
2.模板方法模式
3.觀察者模式
4.迭代子模式
5.責任鏈模式
6.命令模式
7.備忘錄模式
8.狀態模式
9.訪問者模式
10.中介者模式
11.解釋器模式
結構型設計模式包含：
1.適配器模式
2.裝飾器模式
3.代理模式
4.外觀模式
5.橋接模式
6.組合模式
7.享元模式

https://codingnote.cc/zh-tw/p/83149/