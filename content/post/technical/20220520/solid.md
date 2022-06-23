---
title: "SOLID"
date: "2022-05-20"
categories:
- "技術"
- "相依性注入"
tags:
- "C#"
- "相依性注入"
- "SOLID"
- "保哥課程"
toc: true
---

# SOLID

# SOLID

**開發情境:**
* 開發的時間長?還是維護的時間較長?
* 有團隊一起開發?還是一個人寫Code
* 一個長期維護的專案，需求變更頻繁度?
* 你如何讓程式碼具備可讀性與擴充性
* 如何避免在修改程式的過程中引發連鎖反應?(改A壞B)

# OOP的四個特性

## 抽象(Abstraction)
* 將真實世界的需求轉換成為OOP中的類別
* **類別**可以包含**狀態**(屬性)與**行為**(方法)。

## 封裝(Encapsulation)
* 隱藏/保護內部實作的細節，並可以對屬性或方法設定存取層級(Public,Private,Protected)

## 繼承(Inheritance)
* 可以讓您建立新類別以重複使用、擴充和修改其他類別中定義的行為。

## 多型(Polymorphism)
* 在相同介面下，可以用不同的型別來實現。
* 多型有分成好幾種不同類型。

第一步:
從需求或規格中進行"抽象化"的過程
透過"抽象化"過程定義出類別
第二步:
對實作的細節進行"封裝"(隱藏、保護)

第三步:
透過"繼承"來重複利用、擴充和修改基底類別的定義

---
**透過"繼承"來重複利用、擴充和修改基底類別的定義**
![](https://i.imgur.com/yfgmyAW.png)
```csharp=
class BaseClass
{
  public string Name{get;set;}
  public int Age {get;set;}
  public virtual void Output()=>Console.WriteLine("Hello");
  public BaseClass(string name) => Name =name;
  public BaseClass()=> Name ="";
}
class DerivedClass : BaseClass
{
	public string Department { get; set; }
	public DerivedClass() : base("Default") 
	{
		base.Age = 18;
		//base.Department = "IT";
		base.Output();
		this.Age = 19;
		this.Department ="IT";
		this.Output();
	}
	public override void Output()=>Console.WriteLine("Hello !!");
}
```




**在C#中所有類都是"多型"**
* 在設計時期(Design Time)
    * **基底類別**可以定義和實作【虛擬】屬性或方法(<font color ="blue">virtual</font>)
    * **衍生類別**可以【覆寫】這些虛擬的屬性或方法(<font color ="blue">override</font>)
* 在執行時期(Runtime)
    * 當呼叫基底類別的虛擬方法時，會改呼叫子類別覆蓋的方法
* 在C#中，所有類型都是**多型**類型
    * 因為所有類型(包誇使用者定義的類型)都是繼承自Object
* 如果再C#中設計防止衍生類別覆蓋虛擬成員
    * `public sealed override void Dowork(){}`
* 多載(Overloading)比較有點爭議(**有些人認為這不算多型**)




# 內聚力與耦合力(Cohesion & Coupling)

## 何謂"模組"(Module)
* 一個抽象的概念
* 以C#舉例
    * 可能是一個**類別**(Class)
    * 可能是一個**方法**(method)
    * 可能是一個**組件**(assembly)

## 內聚力 Cohesion
:::info
什麼是內聚力?一次專注做一件事情，這件事情做得越好他的內聚力就越高
:::

一個**模組**內完成**一件工作**的度量指標


<font color="blue">**高內聚力**</font>
* 在一個**模組**內只完成一件工作
* 內聚力高，意味者該模組可以獨立運作，也意味者更容易重複利用
* 範例:一個Class只負責一件事情(例如寄送郵件)

<font color="blue">**低內聚力**</font>
* 在一個**模組**內完成多份工作
* 內聚力低，意味者這個模組會造成難以維護/測試/重用/理解
* 範例:所有功能寫在一個class裡面或一個method有500行程式碼

<font color="blue">**最佳實務**</font>
* 在設計模組的時候，要盡量設計出<font color="red">高內聚力</font>的程式碼。
* 若要在一個模組內完成多項工作，建議拆成多個不同的類別
* 實現<font color="red">SRP</font>就是實現<font color="red">**提高內聚力**</font>的一種表現

## 耦合力 Coupling

模組跟模組之間的關聯強度
* 模組之間互相依賴的程度
* 衡量兩個模組的緊密連接程度
* 範例:在ClassB裡面，直接建立了ClassA的物件實體，就會**建立**ClassA與ClassB之間的**耦合關係**。

<font color="blue">**高耦合力**</font>
* 意味者當改了A模組時，相關聯的B模組就會容易被影響(改A壞B)

<font color="blue">**低耦合力**</font>
* 當修改模組的時候，有越少的模組被影響，就意味者耦合力較低

<font color="blue">**最佳實務**</font>
* 在設計不同模組的時候，要盡量設計出<font color="red">低耦合力</font>的程式碼。
* 實現<font color="red">DIP</font>就是實現<font color="red">降低耦合力</font>的一個原則


## :notebook: **隨堂測驗**

**跟多少型別發生耦合?**
```csharp=
public  class InvitaionService
{
    public void SendInvite(string email,string firstName,string lastName)
    {
        if(String.IsNullOrWhiteSpace(firstName))||(String.IsNullOrWhiteSpace(lastName))
        {
            throw new Exception("Name is not vaild");
        }
    }
    if(!email.Contain("@")||email.Contains("."))
    {
        throw new Exception("Email is not vaild!");
    }
    SmtpClient client = new SmtpClient();
    client.Send(new MailMessage("mysite@google.com,email"))
    {
        Subject ="Please join me at my party!"
    }
    
}
```

<font color="red">**Ans:** string、Exception、SmtpClient、MailMessage</font>



> 設計出一個好的程式
> 內聚力越高越好 耦合度越低越好
> 高內聚、低耦合

## :notebook: **隨堂測驗**
**串聯的耦合關係**
```csharp=
public class Order
{
    private ShoppingCartContents cart;
    private float salesTax;
    public Order(ShoppingCartContents cart,float salesTax)
    {
        this.cart=cart;
        this.salesTax=salesTax;
    }
    public float OrderTotal()
    {
        float cartTotal = 0;
        for(int i=0;i<cart.item.length;i++)
        {
            cart.item+=cart.item[i].price;
        }
        return cart.item;
    }
}
public class ShoppingCartContents
{
    public ShoppingCart[] items;
}
public class ShoppingCart
{
    public float Price;
    public int Quanity;
}
```


> 完美上來看要寫出**低耦合高內聚**的程式碼，但現實沒有這麼簡單，所以我們才需要一些**原則**來幫助我們釐清什麼樣的程式才是好的

# 介紹SOLID物件導向設計原則

<font color="blue">**何謂原則(Principle)**</font>
* A <font color="red">principle</font> is a concept or value that is a guide for <font color="blue">behavior</font> or <font color="blue">evaluation</font>
* 所謂【<font color="red">原則</font>】(Principle)就是一種【概念】或【價值】，用來導引你產生<font color="blue">適切的行為</font>與<font color="blue">價值評量方法</font>

**白話文解釋**
* 依循SOLID原則，可以<font color="blue">寫出比較好的程式碼</font>
* 依循SOLID原則，能夠<font color="blue">判斷程式碼的好壞</font>


**背起來**
:::info
* 單一責任原則SRP（Single Responsibility Principle）
* 開放封閉原則OCP(Open Closed Principle)
* 里氏替換原則LSP（Liskov Substitution Principle）
* 介面隔離原則ISP（Interface Segregation Principle）
* 相依反轉原則DIP（Dependence Inversion Principle）
:::


**學習SOLID物件導向設計原則的好處**
* 降低程式碼複雜程度
* 具有較佳程式碼可讀性
* 提升模組可重複利用性
* 讓模組具有高內聚，低耦合力
* 面臨變更需求時可減少破壞現有模組的風險

# 單一責任原則SRP

**何謂<font color="blue">責任</font>(Responsibility)**
* 責任= reason to change (改變的理由)
* 當一個類別擁有多個不同的責任，意味者一個類別責任多項不同的工作，當需求變更時，更動一個類別的理由也可能不只一個

<font color="red">**以下類別有多少責任?**</font>
```csharp=
public class OrderManger
{
    public bool LoadOrder()
    {
        //1.建立資料庫連線(包含寫死的連線字串)
        //2.執行ADO.NET資料存取(包含資料塞選)
        //3.跑回圈取得資料(包含資料格式轉換)
        //4.回傳資料
    }
    
}
```
:thinking_face: **思考:有什麼理由會需要改動到這個class**
<font color="red">Ans:資料格式變了，連線字串改變，塞選資料條件變了</font>

## 關於SRP的基本精神

* 一個類別負擔太多責任時，意味者該<font color="blue">類別</font>可以被切割
    * 可以透過定義一個全新的類別輕鬆做到
    * 對類別進行適度的切割，方便日後管理與維護

* <font color="red">SRP</font>主要精神就是<font color="red">提高內聚力</font>
    * <font color="red">高內聚力</font>意味者可以想到<font color="blue">一個清楚的理由</font>去改它!

## 低內聚力的示意圖
![](https://i.imgur.com/3QKHn8S.png)
> **SRP主要精神就是在提高內聚力**

## 常見的設計問題
* 將所有功能寫在一個類別中
* 類別複雜度過高
* 維護時經常找不到應該要改哪裡
* 發生邏輯問題時找不到BUG在哪裡
* 使用類別時不知道應該呼叫哪個方法

## 關於SRP的使用時機

* **兩個責任**會在**不同時間點**產生變更需求
    * 當你想**改資料庫查詢語法**與修改**系統紀錄的邏輯**時，都會改到同一個類別，那就需要拆開!
* 類別中有一段程式碼有<font color="blue">重複利用</font>的需求
    * 這段程式碼在其他類別也用的到
* 系統中有個**非必要**的功能(<font color="blue">未來需求</font>)，老闆又逼你要實作時
    * 責任會直接依附在類別中，但對維護造成困難



    
修改前:
**請問他有SRP問題嗎，有的話 要如何重構**
```csharp=
void Main()
{
    DataAccess.InsertData();
}
class DataAccess
{
    public static void InsertData()
    {
        Console.WriteLine("Data inserted into database successfully");
        Console.WriteLine("Logged Time" + DateTime.Now.ToLongTimeString()+"Log Data insertion completed successfully");
    }
}
```
修改後:
**將寫入資料、寫LOG拆開**
```csharp=
class DataAccess
{
    public static void InsertData()
    {
        Console.WriteLine("Data inserted into database successfully");
        Logger.Writelog();
    }
}
class Logger
{
    public static void WriteLog()
    {
        Console.WriteLine("Logged Time" + DateTime.Now.ToLongTimeString()+"Log Data insertion completed successfully");
    }
}
```


## SRP討論事項
* 你怎樣確認一個類別被賦予了過多的責任?
* 套用SRP可能有副作用，因為類別變多導致耦合力增加

![](https://i.imgur.com/XLECseE.png)

> <font color="red">提高耦合力，意味者"改B壞A"的機會大幅增加!</font>



## 關於SRP還需要注意的事
* 參考YAGNI(You Ain't Gonna Need It)原則
    * 不用急於在第一時間就專注於分離責任
    * 尚未出現的需求(未來的需求)不需要預先分離責任
    * 當需求變更的時候，再進行類別分割即可!
* SRP是SOLID中簡單的，但卻是最難做到的
    * 需要不斷提升你的**開發經驗**與**重構技術**
    * 如果沒有足夠的經驗去定義一個物件的Reponsibility那麼建議你不要過早進行SRP規劃!


## :notebook: **練習情境**

請試者找出OrderMannger類別，不符合 單一責任原則地方
請指出這個類別是否違反了SRP原則?
請說明理由與如何改善

```csharp=
public class OrderManaer
{
    public List<Product> products=new List<Product>();
    public void Processing()
    {
        //1.檢查商品庫存數量是否足夠
        //2.進行付款處理程序
        //3.進行送貨處理程序
    }
}
```

**請試者修正該OrderManager類別，使其符合 單一責任原則**
將多個責任使用新類別分離出來，但還有什麼問題?
```csharp=
public class Product{}
public class Cstomer{}
public class Stock
{
    public void checkAvailability(
        IEnumerable<Product> products){}
}
public class Payment
{
    public void Processing(
        Customer customer,
        IEnumerable<Product> product){}
}
public class Shipment
{
    public void SendProducts(
        Customer customer,
        IEnumerable<Product> products){}
}
public class OrderManger
{
    public List<Product> Products=new List<Product>();
    public Customer Customer{get;set;}
    public OrderManger()
    {
        
    }
    public void Processing()
    {
        new Stock().CheckAvailability(Products);
        new Payment().Processing(Customer,Products);
        new Shipment().SendProducts(Customer,Products);
    }
}
```
<font color="red">若客戶想要增加Lie Pay 付款方法，要改多少Code?</font>
**目前會有高耦合的問題**

# 開放封閉原則OCP

* Software entities(classes,modules,functions,etc.) should be <font color="red">open for extension</font> but <font color="red">closed for modification</font>
* 軟體實體(類別、模組、函式等)應能<font color="red">開放擴充</font>但<font color="red">封閉修改</font>
* 藉由<font color="blue">增加新的程式碼</font>來擴充系統的功能，而不是藉由<font color="blue">修改原本已經存在的程式碼</font>來擴充系統

## 關於OCP的基本精神
* 一個**類別**需要<font color="red">開放</font>，意味者該類別可以被擴充!
    * 可以透過**繼承**輕鬆做到
    * C#還有**擴充方法**可以輕鬆擴充既有類別
* 一個**類別**需要<font color="red">封閉</font>，意味者有其他人正在使用這個類別!
    * 如果程式已經編譯，但又已經有人在使用原本的類別
    * 封閉修改可以有效避免未知的問題發生

## 常見的設計問題
**耦合力過高，擴充不易**
![](https://i.imgur.com/NG8r1ND.png)



## 關於OCP的實作方式
* 採用分離與相依的技巧(<font color="blue">相依於抽象</font>)
    * 缺點:**需要針對原有程式碼進行重構**
![](https://i.imgur.com/EO2wAh7.png)



**關於OCP的C#範例**
透過**抽象類別**限制其修改，並透過繼承開放擴充不同實作


## 關於OCP的使用時機
* 你既有的類別<font color="blue">已經被清楚定義</font>，處於一個<font color="red">強調穩定</font>的狀態
* 你<font color="blue">需要擴充</font>現有類別，<font color="red">加入新需求</font>的屬性或方法
* 你<font color="blue">擔心</font>修改現有程式碼會<font color="red">破壞現有系統</font>的運作
* 系統剛開始設計時就決定採用OCP模式
    * 可以透過<font color="blue">介面</font>或<font color="blue">抽象類別</font>進行實作

## OCP討論事項
* 當您剛接受維護一份2年前的程式碼，你會怎樣做?
    * 修改之前寫過的類別?
    * 擴充之前寫過的類別?
    * 直接修改舊有原始碼，會有哪些風險存在呢?
* 如何讓系統在**擴充需求**時更簡單、更容易、更安全?
* C#可以透過<font color="blue">interface</font>實踐OCP原則嗎?如何做到?
* 如何進行**抽象化設計**?多少人用過C#**抽象類別**?
---
**Q&A:**
**抽象類別跟Interface差別**
<font color="red">抽象類別 =>可以包含實作(耦合度增加)</font>

## :notebook: **練習情境**
**試者透過OCP原則重構程式碼**
**若客戶想要增加Log輸出到檔案的功能，你會如何改寫程式碼?**
```csharp=
public class AppEvent
{
    public void GenerateEvent(string message)
    {
        Logger fooLogger  =new Logger();
        fooLogger.Log(message);
    }
}
public class Logger
{
    public void Log(string message)
    {
        Console.WriteLine(message);
    }
}
```
**沒學過SOLID的開發者，可能會這樣寫**
```csharp=
public class AppEvent
{
    public void GenerateEvent(string message)
    {
        Logger fooLogger  =new Logger();
        fooLogger.Log(message);
    }
}
public class Logger
{
    private readonly string _Target;
    public Logger(string target){_Target =target;}
    public void Log(string message)
    {
        if(_Targer == "Console")
            Console.WriteLine(message);
        else if(_Target == "File")
            File.WriteAllText("MyLog",message);
        else
            throw new NotImplementedException();
    }
}
```
**此時，若又想要增加訊息傳送到遠端Web API或Storage呢?**

---
* **採用分離與相依的技巧**
```csharp=
public interface ILogger
{
    void Log(string message);
}
public class ConsoleLogger:ILogger
{
    public void Log(string message)
    {
        Console.WriteLine(message);
    }
}
public class FileLogger:ILogger
{
    public void Log(string message)
    {
        File.WriteAllText("MyLog",message)
    }
}
public class AppEvent
{
    private readlony ILogger _Logger;
    public AppEvent(string loggerType)
    {
        this._logger = LoggerFactory.CreateLogger(loggerType);
    }
    public void GenerateEvent(string message)
    {
        _Logger.Log(message);
    }
}
public class LoggerFactory
{
    public static ILogger CreateLogger(string loggerType)
    {
        if(loggerType =="Console")
            return new ConsoleLogger();
        else if(loggerType=="File")
            return new FileLogger();
        else throw new NotImplementedException();
        
    }
}
```

如果要新增一個log，要新增一個class繼承Ilogger 在LoggerFactory增加 

# 里氏替換原則LSP

* <font color="blue">Subtypes</font> <font color="red">**must**</font> be substitutable for their <font color="blue">base types.</font>
    * <font color="blue">subtypes</font>(衍生類別) = 類別
    * <font color="blue">base types</font>(基底類別) =介面、抽象類別、基底類別
* 子型別必須可替換為他的基底型別
* 如果你的程式有採用**繼承**或**介面**，然後建立出幾個不同的**衍生型別**(Subtype)。在你的系統中只要是**基底型別**出現的地方，都可以用**子型別**來取代，而不會破壞程式原有的行為。


## 關於LSP的基本精神
* 當實作繼承時，必須確保型別轉換後還能得到正確的結果
    * 當每個**衍生類別**都可以正確地替換為**基底類別**，且程式在執行時不會有異常的情況(如發生執行時期例外)
    * 必須正確的實作<font color="blue">繼承</font>與<font color="blue">多型</font>


## 常見的設計問題
* 不正確的實作繼承與多型
    * 第一版：沒有繼承，單純的計算矩形面積
    * 第二版：新增需求，增加Square類別(套用OCP原則)
    * 第三版：重構程式，正確套用LSP原則

* 實作繼承時，在特定情況下發生執行時期錯誤(Runtime Error)
    * 範例程式
    * 違反LSP原則有時候較難發現

**第一版:**
```csharp=
void Main()
{
    Rectangle o = new Rectangle();
    o.Width = 40;
    o.Height =50;
    LSPBehavior.GetArea(o).Dump();
}
public class Rectangle
{
    public int Height{get;set;}
    public int Width{get;set;}
}
public class LSPBehavior
{
    public static int GetArea(Rectangle s)
    {
        if(s.Width>20)
        {
            s.Width = 20;
        }
        return s.Width * s.Height;
    }
}
```

**第二版(OCP原則):**
```csharp=
void Main()
{
    Square o = new Square();
    o.Width = 40;
    //o.Height =40;
    LSPBehavior.GetArea(o).Dump();
}
public class Rectangle
{
    public int Height{get;set;}
    public int Width{get;set;}
}
public class Square:Rectangle
{
    private int _height;
    private int _width;
    public int Height
    {
        get{return _height;}
        set{_height = _width =value;}
    }
    public int Width
    {
        get{return _width;}
        set{_width = _height =value;}
    }
}
public class LSPBehavior
{
    public static int GetArea(Rectangle s)
    {
        if(s.Width>20)
        {
            s.Width = 20;
        }
        return s.Width * s.Height;
    }
}
```
<font color ="red">**答案會是多少?**</font>

**第二版不符合LSP**

**第三版(LSP):**
```csharp=
void Main()
{
    Square o = new Square();
    o.Width = 40;
    //o.Height =40;
    LSPBehavior.GetArea(o).Dump();
}
public class Rectangle
{
    public virtual int Height{get;set;}
    public virtual int Width{get;set;}
}
public class Square:Rectangle
{
    private int _height;
    private int _width;
    public override int Height
    {
        get{return _height;}
        set{_height = _width =value;}
    }
    public override int Width
    {
        get{return _width;}
        set{_width = _height =value;}
    }
}
public class LSPBehavior
{
    public static int GetArea(Rectangle s)
    {
        if(s.Width>20)
        {
            s.Width = 20;
        }
        return s.Width * s.Height;
    }
}
```
**善用virtual & override**

---

**第二個例子(不符合LSP)**
```csharp=
class Customer
{
    public virtual double getDiscount(double TotalSales)
    {
        return TotalSales
    }
    public virtual void Add()
    {
        
    }
}
class GoldCustomer : Customer
{
    public override double getDiscount(double TotalSales)
    {
        return base.getDiscount(TotalSales) - 5;
    }
    public override void Add()
    {
        Console.WriteLine("GoldCustomer:Add");
    }
}
class SilverCustomer:Customer
{
    public overide double getDiscount(double TotalSales)
    {
        return base.getDiscount(TotalSales) - 5;
    }
    public override void Add()
    {
        Console.WriteLine("SilverCustomer:Add");
    }
}
class Enquiry:Customer
{
        public overide double getDiscount(double TotalSales)
    {
        return base.getDiscount(TotalSales) - 5;
    }
    public override void Add()
    {
        Console.WriteLine("Not allowed");
    }
}
void Main()
{
    List<Customer> Customers = new List<Customer)();
    Customer.Add(new SilverCustomer());
    Customer.Add(new GoldCustomer());
    Customer.Add(new Enquiry());
    
    foreach(Customer o in Customers)
    {
        o.Add();
    }
}
```

**有沒有人這樣解決?**
```csharp=
class Enquiry:Customer
{
        public overide double getDiscount(double TotalSales)
    {
        return base.getDiscount(TotalSales) - 5;
    }
    public override void Add()
    {
        
    }
}
```


**修正(符合LSP)**
```csharp=
interface IDiscount
{
    double getDiscount(double TotalSales);
}
interface IDatabase
{
    
}
class Customer:IDatabase,IDiscount
{
    public virtual double getDiscount(double TotalSales)
    {
        return TotalSales
    }
    public virtual void Add()
    {
        
    }
}
class GoldCustomer : Customer
{
    public override double getDiscount(double TotalSales)
    {
        return base.getDiscount(TotalSales) - 5;
    }
    public override void Add()
    {
        Console.WriteLine("GoldCustomer:Add");
    }
}
class SilverCustomer:Customer
{
    public overide double getDiscount(double TotalSales)
    {
        return base.getDiscount(TotalSales) - 5;
    }
    public override void Add()
    {
        Console.WriteLine("SilverCustomer:Add");
    }
}
class Enquiry:IDiscount
{
        public overide double getDiscount(double TotalSales)
    {
        return base.getDiscount(TotalSales) - 5;
    }
    public override void Add()
    {
        Console.WriteLine("Not allowed");
    }
}
void Main()
{
    List<Customer> Customers = new List<Customer)();
    Customer.Add(new SilverCustomer());
    Customer.Add(new GoldCustomer());
    Customer.Add(new Enquiry());
    
    foreach(Customer o in Customers)
    {
        o.Add();
    }
}
```
**優點:這樣在編譯時期就可以看出錯誤**

## 關於LSP的實作
* 採用<font color="blue">類別繼承方式</font>來進行開發
    * 須注意繼承的實作方式
* 採用<font color="blue">合約設計方式</font>來進行開發
    * 利用<font color="red">介面</font>(<font color="greed">interface</font>)來定義基底型別(base type)


## 關於LSP的使用時機
* 當你需要透過<font color="blue">基底型別</font>對<font color="blue">多型</font>物件進行操作時

## LSP討論事項
* 在教導新人時，如何有效的避免繼承的錯誤實作?
* 你會用<font color="blue">抽象類別、類別</font>或<font color="blue">介面</font> 來實現LSP原則? 為什麼?



# 介面隔離原則ISP
* A:Many <font color="#FF1493">client specific interfaces</font> are <font color="red">better than</font>  <font color="blue">one general purpose interface.</font>
* B:Clients <font color="red">should not</font> be <font color="red">forced</font> to depend upon <font color="blue">interface</font> that <font color="blue">they don't use.</font>

* A:多個<font color="#FF1493">用戶端專用的介面</font>優於一個<font color="blue">通用需求介面</font>
* B:<font color="#FF1493">用戶端</font>不應該強迫相依於<font color="blue">沒用到的介面</font>

* 針對不同需求的**用戶端**，僅開放其對應需求的介面就好

## 關於ISP的基本精神
* 把不同需求的屬性與方法，放在不同的介面中
    * 不要讓你的<font color="blue">interface</font>包山包海
    * 特定需求沒用到的地方，不要加到介面中，另外建一個
    * 可以拿<font color="blue">interface</font>當成群組來用(屬性與方法)
* 使得系統可以更容易的達成<font color="red">鬆散耦合、安全重構、功能擴充</font>

## 常見的設計問題
* 將所有**API**需求都定義在一個超大介面中
* **用戶端**相依於一堆**用不到**得介面方法
    * 如果多個類別已經實作同一個**胖介面**
    * 就會導致某些別實作出**用戶端用不到**的方法
    * 這時應該可以拆分多的用戶端專用的介面進行實作
    * 所以一個實作介面的類別，不應該強迫去實作出這個類別<font color="red">不需要</font>的方法(備註:這裡的**不需要**是指**用戶端不需要**)

## 關於ISP的使用時機
* 當介面需要被分割的時候
* 類別的使用時機可以被切割的時候
    * 假設類別有20個方法，並實作一個15個方法的介面
    * 有某個**用戶端**只會使用該類別中的10個方法
    * 你就可以為這類別的10個方法定義介面並設定實作介面
    * 你的**用戶端**就可以改用**介面**操作
    * 這個過程也可以用來降低主程式與這個類別的耦合力

---

**有沒有符合ISP精神?**
```csharp=
void Main()
{
    Console.Writenline("\n\nOpen Close Principle Demo ");
    DataProvider DataProviderObject = new SqlDataProvider();
    DataProviderObject.OpenProviderObject();
    DataProviderObject.ExcuteCommand();
    DataProviderObject.CloseConnection();
}
interface DataProvider
{
    int OpenConnection();
    int CloseConnection();
    int ExcuteCommand();
    int BeginTransation();
}
class SqlDataProvider:DataProvider
{
    public int OpenConnection()
    {
        Console.WriteLine("
        \nSql Connection opened successfully");
    }
    public int CloseConnection()
    {
        Console.WriteLine("
        Sql Connection Close successfully");
    }
    public int ExecuteCommand()
    {
        Console.WriteLine("
        Sql Command Executed successfully");
    }
    public int BeginTransaction()
    {
        Console.WriteLine("
        Sql BeginTransaction successfully");
    }
}
```
<font color="red">**Q&A:沒有 BeginTransaction沒用到**</font>

---
**修正版:**
```csharp=
void Main()
{
    Console.Writenline("\n\nOpen Close Principle Demo ");
    DataProviderWithoutTransaction DataProviderObject = new SqlDataProvider();
    DataProviderObject.OpenConnection();
    DataProviderObject.ExcuteCommand();
    DataProviderObject.CloseConnection();
}
interface DataProviderWithoutTransaction
{
    int OpenConnection();
    int CloseConnection();
    int ExcuteCommand();
}
interface DataProvider:DataProviderWithoutTransaction
{
    int BeginTransaction();
}
class SqlDataProvider:DataProvider
{
    public int OpenConnection()
    {
        Console.WriteLine("
        \nSql Connection opened successfully");
    }
    public int CloseConnection()
    {
        Console.WriteLine("
        Sql Connection Close successfully");
    }
    public int ExecuteCommand()
    {
        Console.WriteLine("
        Sql Command Executed successfully");
    }
    public int BeginTransaction()
    {
        Console.WriteLine("
        Sql BeginTransaction successfully");
    }
}
```

## ISP 討論事項
* 你工作中是否有設計過**超大介面**的經驗?
* 設計介面的時候，介面的大小應該如何判斷?如何群組?
* 介面可以實作介面，使用的時機為何?

**例子2**
```csharp=
void Main()
{
    Console.WriteLine("\n\nOpen Close Principle Demo");
    IReadAndWrite Logger = new Logger();
    Logger.Write();
    Logger.Read();
}
interface IReadAndWrite
{
    string Read();
    void Write();
}
class Logger:IReadAndWrite
{
    public string Read(){return "";}
    public void Write(){}
}
```
**需求變了
由用戶端來選擇要用的interface**
```csharp=
void Main()
{
    Console.WriteLine("\n\nOpen Close Principle Demo");
    IWritable Logger = new Logger();
    Logger.Write();
    Logger.Read();
}
interface IReadable
{
     string Read();
}
interface IWritable
{
      void Write();
}
interface IReadAndWrite:IReadable,IWritable
{
   
}
class Logger:IReadAndWrite,IReadable,IWritable
{
    public string Read(){return "";}
    public void Write(){}
}
```


例子3
main 跟多少型別發生相依
Q&A:兩個
```csharp=
void Main()
{
    IDatabase cust = new Customer();
    cust.Add();
}
interface IDatebase
{
    void Add();
}
class Customer:IDatebase
{
    public void Add()
    {
        Console.WrtieLine("Add something");
    }
}
```
例子4
**試者找出該IAllnOnceCar介面 不符合介面隔離則地方**
```csharp=
public void Main()
{
    Driver o = new Driver();
    o.StartEngine();
    o.Drive();
    o.StopEngine();
}
public interface IAllInOneCar
{
    void StartEngine();
    void Drive();
    void StopEngine();
    void ChangeEngine();
}
public class Driver:IAllInOneCar
{
    public void ChangeEngine(){
    throw new NotImplementedException();}
    public void Drive(){}
    public void StartEngine(){}
    public void StopEngine(){}
}
```
**修正後結果**
```csharp=
public void Main()
{
    IDriver o = new Driver();
    o.StartEngine();
    o.Drive();
    o.StopEngine();
}
public interface IDriver
{
    void StartEngine();
    void Drive();
    void StopEngine();
}
public class Driver:IDriver
{
    public void Drive(){}
    public void StartEngine(){}
    public void StopEngine(){}
}
public interface IMachanic
{
    void ChangeEngine();
}
public Machanic:IMachanic
{
    public void ChangeEngine()
    {
        throw new NotImplementedException();
    }
}
```

# 相依反轉原則DIP
* A.<font color="blue">High-level modules</font> should <font color="red">not </font>depend on <font color="blue">low-level modules</font>.Both should <font color="pink">depend on abstractions.</font>
* B.<font color="blue">Abstractions</font> should <font color="red">not</font> depend on <font color="blue">details.Details</font> should <font color="pink">depend on abstractions.</font>

* A.<font color="blue">高階模組</font>不應該依賴於<font color="blue">低階模組</font>，兩者都應<font color="pink">相依於抽象</font>
    * 高階模組=>Caller(呼叫端)
    * 低階模組=>Callee(被呼叫端)
* B.<font color="blue">抽象</font>不應該相依於<font color="blue">細節</font>，而<font color="blue"></font>細節則應該<font color="pink">相依於抽象</font>

## 關於DIP的基本精神
* 所有類別都要相依於抽象，而不是具體實作
    * 可透過<font color="red">DI Container</font>達到目的
* 為了要達到類別間鬆散耦合的目的
    * 開發過程中，所有**類別之間**的**耦合關係**一律透過**抽象介面**

## 常見的設計問題
* 類別與類別之間緊密耦合，改A壞B的狀況層出不窮
```csharp=
public class Client
{
    Service _Service;
    public void Client()
    {
        Service fooObj= new Service();
    }
}
public class Service();
```
![](https://i.imgur.com/olgv8VG.png)

**什麼是相依反轉?**
```csharp=
public class Client
{
    IService _Service;
    public void Client(IService service)
    {
        _Service = service;
    }
}
public interface IService{}
public class MyService:IService{}
public class YourService:IService{}
public class TheirService:IService{}
```
![](https://i.imgur.com/3QXr2Xj.png)

## 關於DIP的實作方式
* 型別全部都相依於抽象，而不是具體實作
* 經過套用DIP之後，原來有相依於類別的程式碼
    * 都改成相依於**抽象型別**
    * 從**緊密耦合**關係變成**鬆散偶合**關係
    * 可以依據需求，隨時**抽換具體實作類別**
## 關於DIP的使用時機
* 像要降低耦合的時候
* 希望類別都相依於抽象，讓團隊可以**更有效率**的開發系統
* 想要可以替換具體實作，讓系統變得更有彈性
    * 符合DIP通常也意味者符合OCP與LSP原則
    * 只要再多考量SRP與ISP就很棒了!
* 想要導入<font color="blue">TDD(測試驅動開發)</font>或<font color="blue">單元測試</font>的時候

## DIP討論事項
* 你在工作中是否有遇過類似的設計方式?(相依注入)
* 如果一個類別非常穩定，也沒有變更需求，需要套用DIP嗎?
* 大量套用DIP有缺點嗎?

## :notebook: **練習情境**
**請試者找出底下程式碼不符合 相依反轉原則地方**
```csharp=
public class SecurityService
{
    public bool LoginUser(string userName,string password)
    {
        LoginService service = new LoginService();
        return service.ValidateUser(userName,password);
    }
}
public class LoginService
{
    public bool ValidateUser(string userName,string password)
    {
        throw new NotImplementedException();
    }
}
```
**修正相依於抽象，使用建構式傳入具體實作物件**
```csharp=
public class SecurityService
{
    private readoly ILoginService _LoginService;
    public SecurityService(ILoginService loginService)
    {
        this._LoginService =loginService;
    }
        public bool LoginUser(string userName,string password)
    {
        return _LoginService.ValidateUser(userName,password);
    }
}
public void Main()
{
    new SecurityService(new LoginService());
}
```
