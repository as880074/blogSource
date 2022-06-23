---
title: "C# 開發實戰：非同步程式開發技巧-名詞介紹"
date: "2022-06-23"
categories:
- "技術"
- "非同步程式"
tags:
- "C#"
- "非同步程式"
- "async/await"
- "保哥課程"
toc: true
---

# 名詞介紹
## Concurrent computing vs. Parallel computing
> 建議用英文溝通，因為大陸跟台灣中文翻譯不一樣，大陸的Parallel computing翻譯成並行計算

* 並行計算 (Concurrent computing)
    - 數個運算工作<font color="red">同時</font>在**特定時間區段**內**交替地完成**，而**非依序執行**
    - a form of computing in which several computations are executed during **overlapping time periods** <font color="red">concurrently</font> instead of **sequentially**

![](https://i.imgur.com/tdlarOm.png)


* 平行計算 (Parallel computing)
    - 許多運算過程會<font color="red">同時發生</font>
    - a type of computation in which many calculations or the execution of processes are carried out <font color="red">simultaneously</font>

![](https://i.imgur.com/NI0ZZBE.png)


## IO Bound VS CPU Bound

* **IO Bound**
    - 如果你的程式需要透過 I/O 進行存取，就屬於 IO Bound 的非同步工作
    - 例如：從網路上取得一份資料、呼叫 API、存取資料庫、讀寫檔案
    - 由於存取 I/O 的過程可能會**閒置 CPU 運作**，過程可能會導致 CPU 資源被浪費
    - **這種類型的應用**非常適合採用**非同步開發**來解決<font color="red">資源浪費</font>的問題！
    - 💡 **應多利用 <font color="red">await</font> 來等待非同步執行**
* **CPU Bound**
    - 如果你的程式需要進行相當繁重的 CPU 計算工作，就屬於 CPU Bound 的非同步工作
    - 例如：圖片轉換處理、處理大量數據計算
    - 由於 CPU 大量運算的情況，應用程式可能會處於「**無法回應**」的狀態
    - **這種類型的應用**非常適合採用**非同步開發**來解決<font color="red">響應性</font>的問題！
    - 💡 **應多利用 <font color="red">TPL</font> ([Task Parallel Library](https://docs.microsoft.com/en-us/dotnet/standard/parallel-programming/task-parallel-library-tpl?WT.mc_id=DT-MVP-4015686)) 來實現非同步 ([Parallel.ForEach](https://docs.microsoft.com/zh-tw/dotnet/api/system.threading.tasks.parallel.foreach?view=net-6.0) 或 [Task.Run](https://docs.microsoft.com/zh-tw/dotnet/api/system.threading.tasks.task.run?view=net-6.0))**


## 計算機架構相關名詞
![](https://i.imgur.com/1lt2Qhz.png)
*  [中央處理器](https://zh.wikipedia.org/wiki/%E4%B8%AD%E5%A4%AE%E5%A4%84%E7%90%86%E5%99%A8) ([CPU](https://en.wikipedia.org/wiki/Central_processing_unit))
    - **C**entral **P**rocessing **U**nit
    - Processors (處理器)
    - Sockets (實體插槽)
    - Cores (實體核心)
    - Logical processors (邏輯核心)
* [超執行緒](https://zh.wikipedia.org/wiki/%E8%B6%85%E5%9F%B7%E8%A1%8C%E7%B7%92)[(HT)](https://en.wikipedia.org/wiki/Hyper-threading)
    - **H**yper-**T**hreading **T**echnology (HTT)
    - Intel 專利的 [SMT](https://en.wikipedia.org/wiki/Simultaneous_multithreading) 技術
        - SMT = Simultaneous MultiThreading
    - [Scalar processor](https://en.wikipedia.org/wiki/Scalar_processor) vs. [Superscalar processor](https://en.wikipedia.org/wiki/Superscalar_processor)

## 作業系統相關
![](https://i.imgur.com/a1wkQfU.png)

* 電腦程式 ([Program](https://en.wikipedia.org/wiki/Computer_program))
    - 包含**一系列指令** (a sequence of instructions) 用來讓電腦運作的檔案
* 執行 ([Execution](https://en.wikipedia.org/wiki/Execution_(computing)))
    - 將**電腦程式中**的所有指令**載入到記憶體** (RAM) 並建立**處理程序**的流程
* <font color="red">**處理程序**</font> ([Process](https://en.wikipedia.org/wiki/Process_(computing)))
    - **電腦程式**的**實體** (instance of a computer program) (一台電腦可以同時執行多個相同的電腦程式)
    - **作業系統**可能會使用**一個**到**多個**<font color ="red">執行緒</font>來執行電腦程式中的指令 (instructions)
* <font color="red">**執行緒**</font> ([Thread](https://zh.wikipedia.org/wiki/%E7%BA%BF%E7%A8%8B))
    - 由[作業系統](https://en.wikipedia.org/wiki/Operating_system)透過**排程器**([Scheduler](https://en.wikipedia.org/wiki/Scheduling_(computing)))分配**執行**在電腦程式中**一系列指令**的**最小單位**
    - 區分 **Kernel threads** 與 **User threads** 兩種，負責不同類型的執行任務

* 排程器 ([Scheduling](https://en.wikipedia.org/wiki/Scheduling_(computing)))
    - **排程器**是作業系統的一個重要元件，用來安排「**資源**」來執行「**工作**」
    - **資源**包含處理器 (Processor)、核心(Core)、網路、... 等等。
    - **工作**包含執行緒([Thread](https://zh.wikipedia.org/wiki/%E7%BA%BF%E7%A8%8B))、處理程序([Process](https://en.wikipedia.org/wiki/Process_(computing)))、傳輸網路封包 ([Traffic flow](https://en.wikipedia.org/wiki/Traffic_flow_(computer_networking)))、... 等等。
* 先佔式任務處理 ([Preemption](https://en.wikipedia.org/wiki/Preemption_(computing)))
    - 這是一種**多工**([Multi-Tasking](https://zh.wikipedia.org/wiki/%E5%A4%9A%E4%BB%BB%E5%8A%A1%E5%A4%84%E7%90%86))的實現方式。**多工**是指電腦同時執行多個程式的能力！
    - **先佔式任務處理**會中斷正在執行中的**執行緒**，並會在**未來的一段時間**後**繼續執行**
* 內容切換 ([Context Switching](https://en.wikipedia.org/wiki/Context_switch))
    - 由於**先佔式任務處理**會中斷正在執行中的**執行緒**，在執行緒之間反覆切換的過程叫做**內容切換**
    - Windows / macOS / Linux / AIX / BSD / Solaris 皆採用**先佔式多工** ([Preemptive Multitasking](https://en.wikipedia.org/wiki/Preemption_(computing)##PREEMPTIVE))，一個執行緒執行程式時間用完了，系統就會進行 Context Switch，把 CPU 分配給下一個執行緒，沒有一個程式能獨佔 CPU 時間！

* 多工處理 ([Multi-Tasking](https://en.wikipedia.org/wiki/Computer_multitasking))
    - **多工**是指電腦同時執行多個程式的能力
    - 多工的一般方法是執行第一個程式的一段代碼，儲存工作環境；再執行第二個程式的一段代碼，儲存環境；……恢復第一個程式的工作環境，執行第一個程式的下一段代碼……現代的多工，每個程式的時間分配相對平均。
* 多執行緒 ([Multithreading](https://en.wikipedia.org/wiki/Multithreading_(computer_architecture)))
    - **多執行緒**是一種利用**單一核心**來提供**多工處理**的能力，用來提供**同時執行**多條執行緒的工作方式，而這種能力通常都是由**作業系統**內建提供的。
    - 在多執行緒的應用程式中，執行緒會共用一個或多個核心，而共用的資源包含運算單元、CPU 快取或 [TLB](https://en.wikipedia.org/wiki/Translation_lookaside_buffer) (translation lookaside buffer) 等資源。
    - 新一代的 CPU 基本上都有提供**硬體多執行緒**的支援，能夠在**同一時間執行多於一個執行緒**，進而提升整體處理效能

## Program vs. Process vs. ThreadScheduling, Preemption, Context Switching
![](https://upload.wikimedia.org/wikipedia/commons/thumb/2/25/Concepts-_Program_vs._Process_vs._Thread.jpg/1024px-Concepts-_Program_vs._Process_vs._Thread.jpg)

https://en.wikipedia.org/wiki/Thread_(computing)

## 相關名詞

* 同步 ([Synchronous](https://en.wikipedia.org/wiki/Synchronization))
    - 在計算機的世界裡，沒有什麼程式是真的「同步」的。
    - **同步**通常是指在**一個系統**中所**發生的事件之間**進行**協調** (coordination of events)，讓時間上出現    **一致性**與**統一化**的現象，讓整件事看起來像是**依序執行**的結果。
* 非同步 ([Asynchronous](https://en.wikipedia.org/wiki/Asynchrony_(computer_programming)))
    - **非同步**意味著在**一個系統**中所**發生的事件** (occurrence of events) 都可以**獨立執行**，並且提供**一些方法**來處理這些事件。
    - 這些**發生的事件**不會**封鎖**(Block)**主程式**的**執行緒**，因此可以**多個工作**進行**平行處理**

* [同步化](https://hackmd.io/ssCoAOqVThCEivKXNX88Yw?view) ([Synchronization](https://en.wikipedia.org/wiki/Synchronization))
    - 當**多執行緒**需要**同時存取共用資源**時，所需進行的**管控機制**，確保程式執行時可以得到**預期結果**
    - 跨執行緒的同步化： [lock](https://docs.microsoft.com/zh-tw/dotnet/csharp/language-reference/statements/lock?WT.mc_id=DT-MVP-4015686), [SpinLock](https://docs.microsoft.com/zh-tw/dotnet/standard/threading/spinlock?WT.mc_id=DT-MVP-4015686), [Mutex](https://docs.microsoft.com/zh-tw/dotnet/standard/threading/mutexes?WT.mc_id=DT-MVP-4015686), [ReaderWriterLockSlim](https://docs.microsoft.com/en-us/dotnet/api/system.threading.readerwriterlockslim?view=net-6.0), [Barrier](https://docs.microsoft.com/zh-tw/dotnet/standard/threading/barrier?WT.mc_id=DT-MVP-4015686), [CountdownEvent](https://docs.microsoft.com/en-us/dotnet/standard/threading/countdownevent?WT.mc_id=DT-MVP-4015686), ...
    - 跨處理程序同步化：[Mutex](https://docs.microsoft.com/zh-tw/dotnet/standard/threading/mutexes?WT.mc_id=DT-MVP-4015686), [Semaphore](https://docs.microsoft.com/zh-tw/dotnet/standard/threading/semaphore-and-semaphoreslim?WT.mc_id=DT-MVP-4015686) (Windows), [EventWaitHandle](https://docs.microsoft.com/en-us/dotnet/standard/threading/eventwaithandle?WT.mc_id=DT-MVP-4015686) (Windows)
    - 相關文章：[Overview of synchronization primitives](https://docs.microsoft.com/en-us/dotnet/standard/threading/overview-of-synchronization-primitives?WT.mc_id=DT-MVP-4015686)
* 執行緒安全 ([Thread-safety](https://en.wikipedia.org/wiki/Thread_safety))
    - 在**多執行緒**環境下執行程式，用以確保執行可以得到**預期執行結果**！
    - 實現**執行緒安全**的**必要條件**就是必須能夠提供**同步化**機制！
* 同步內容 ([SynchronizationContext](https://docs.microsoft.com/zh-tw/dotnet/api/system.threading.synchronizationcontext?view=netframework-4.7.2))
    - 允許執行緒透過將**工作單元**(Work of Unit)進行封裝([Marshal](https://zh.wikipedia.org/wiki/Marshalling_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6)))之後，傳遞給其他執行緒

* 競爭狀態 ([Race Condition](https://en.wikipedia.org/wiki/Race_condition))
    - 競爭情形是一種錯誤，這種錯誤是指根據兩個或多個執行緒之中，哪一個先到達程式碼的特定區塊而決定程式的結果。
    - 執行程式多次會產生不同的結果，並且無法預測任何指定的執行結果。
* 關鍵區段 ([Critical Section](https://en.wikipedia.org/wiki/Critical_section)) 
    - 是**一段程式碼<font color="red">不允許</font>多執行緒<font color="red">同時執行</font>**
    - 為了**避免資源競爭**的情況發生 ([Race Condition](https://en.wikipedia.org/wiki/Race_condition))
* 死結 ([Deadlock](https://en.wikipedia.org/wiki/Deadlock))
    - 當**兩個以上**的**執行緒**，**雙方都在等待**對方停止執行，以取得系統資源，但是沒有一方會先結束。
![](https://upload.wikimedia.org/wikipedia/commons/thumb/2/23/Deadlock_at_a_four-way-stop.gif/220px-Deadlock_at_a_four-way-stop.gif)
## .NET程式啟動流程
* .NET **處理程序(Process)**啟動後，僅會有**<font color="red">一個</font>前景執行緒**！
    - 每個**執行程序**都可以**產生**出許多**前景執行緒**或**背景執行緒**
* **前景執行緒 (Foreground Thread)**
    - 所有**前景執行緒**必須**全部結束**執行，否則**處理程序**無法結束執行
    - 無論是否還有**背景執行緒**在執行，沒有**前景執行緒**在跑，該**處理程序**就一定會結束
    - new Thread() 預設就是**前景執行緒**
* **背景執行緒 (Background Thread)**
    - new Task() 預設就是**背景執行緒**
    - 從**執行緒集區**(Thread Pool)取得的**執行緒**，一定是**背景執行緒**
    - <font color="##9F0050">**Task.Run** 從執行緒集區取得一個執行緒，因此是**背景執行緒**</font>
* **主執行緒(main Thread)**
    - 程序啟動時的第一個執行緒






https://www.albahari.com/threading/

https://source.dot.net/
