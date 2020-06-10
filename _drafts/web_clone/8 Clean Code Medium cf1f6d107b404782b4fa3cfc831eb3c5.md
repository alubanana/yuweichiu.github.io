# 第 8 章 邊界| Clean Code - 手寫筆記 - Medium

[https://medium.com/%E6%89%8B%E5%AF%AB%E7%AD%86%E8%A8%98/boundaries-774cae00dfbd](https://medium.com/%E6%89%8B%E5%AF%AB%E7%AD%86%E8%A8%98/boundaries-774cae00dfbd)

![1*Cbj_BcK5ePD4Mc4TK4sfvQ.jpeg](8%20Clean%20Code%20Medium%20cf1f6d107b404782b4fa3cfc831eb3c5/1Cbj_BcK5ePD4Mc4TK4sfvQ.jpeg)

```
🔖 文章索引1. 與第三方軟體的邊界 - 學習式測試
2. 已知與未知的邊界 - 使用尚未存在的程式
3. 小結
```

在書中講的有兩種**邊界 (Boundaries)**，第一種是**與第三方軟體的邊界**，第二種是**已知與未知的邊界**。更白話一點，邊界就是「程式與程式的邊界」。

# **與第三方軟體的邊界 — 學習式測試**

我們在軟體中經常會依賴第三方的軟體，如果沒有好的設計或是一些好的習慣，則不容易保持系統**邊界**的整捷。

而我們經常需要幾天的時間學習第三方軟體，在摸索的過程中通常是花大量的時間看第三方軟體的文件，以及實作官方提供的範例程式，思考第三方軟體是如何使用及運作。

但是，作者說道，與其沒有目標的看官方文件學習，不如撰寫**學習式測試**來探索與了解第三方的軟體，Jim Newkirk 稱這種方法為**學習式測試 (Learning tests)**。

# **學習式測試比不花功夫更好**

![https://miro.medium.com/max/750/0*3TwvCAKpNspUdy09.png](https://miro.medium.com/max/750/0*3TwvCAKpNspUdy09.png)

取自 [https://pandas.pydata.org](https://pandas.pydata.org/)

假設我們想要在系統中加入 **[🔗 交通部觀光局觀光統計資料庫](https://stat.taiwan.net.tw/)** 中的資料，並且使用 pandas 幫助我們解析 csv 中部份的欄位。所以，我們必須閱讀 **[📖 pandas 的官方文件](http://pandas.pydata.org/pandas-docs/stable/)** 學習怎麼使用第三方軟體 pandas。

> 📊 資料：來臺旅客按居住地統計.csv

```python
class PandasLearningTest(unittest.TestCase):
    def setUp(self):
        self.csv = pd.read_csv("./來臺旅客按居住地統計.csv")

    def test_output_csv(self):
        self.csv.to_csv("output.csv")

    def test_output_csv_without_index(self):
        self.csv.to_csv("output.csv", index=False)

    def test_calculate_number_of_tourists(self):
        csv = self.csv.iloc[1:-1]
        region = ['亞洲地區', '非洲地區', '美洲地區', '大洋洲地區', '歐洲地區', '其他未列明']
        number_of_tourists = csv.astype('int').sum()[region]

    def test_calculate_number_of_tourists_in_asia(self):
        csv = self.csv.iloc[1:-1]
        number_of_tourists = csv.astype('int').sum()
        region = ['亞洲地區']
        number_of_asia_tourists = number_of_tourists[region]

    def test_calculate_number_of_tourists_after_105(self):
        csv = self.csv.iloc[1:-1]
        number_of_tourists_after_105 = csv[csv.loc[:, '年別'].astype('int') > 105]
```

在經歷了一番苦戰之後，我們發現了一些 pandas 使用的技巧，並且將這些知識建立成一些簡單的單元測試。

**這些被封裝的知識就是邊界**，現階段，它們應用到我們的系統中時，因為通過了單元測試，不會引發任何的問題。

但是，如果未來為了滿足更多的需求進行升級了 pandas 的版本，或是資料庫被修改了一些欄位，舊有的程式碼不一定會相容於現階段的系統。因此，假設未來升級時，無法通過我們先前撰寫的單元測試，我們就能夠立即發現問題。

> 而這些測試，作者稱為邊界測試，可以減輕升級整合所造成的負擔。

---

# **已知與未知的邊界 — 使用尚未存在的程式**

假設，你的團隊要寫一個統計台灣觀光數據的網站，而團隊只有兩名成員；你負責開發介接 API 的程式，而你的隊友負責撰寫 API。但是，你的隊友正在*學習如何使用 pandas 與交通部觀光局觀光統計資料庫*，所以你還不知道 API 長什麼樣子。

![https://miro.medium.com/max/589/1*fsQ20oEhZ5xoxu9ItbPIFw.png](https://miro.medium.com/max/589/1*fsQ20oEhZ5xoxu9ItbPIFw.png)

> Uncle Bob：「我們並不想因此被困住，所以我們在離這塊未知的程式碼比較遠的地方，展開我們的工作。」

雖然你還不知道 Statistic API 的模樣，但是仍然能夠**想像那個邊界介面長 (Statistic Interface)** 什麼樣子。我們希望那個介面可以：

> 統計每個區域或是州的觀光人數，以及統計蔡總統上任後的觀光人數。

所以，我們將已知與未知的邊界使用介面隔離開來，這時候你就可以繼續撰寫 Controller 介接 API 的程式。在 Statistic API 還沒開發完成前，先使用一個假的 Fake Statistic，能夠讓 Controller 順利地動起來。

因此，你甚至可以撰寫 Controller 的單元測試，不必等待 Statistic API 開發完成。

![https://miro.medium.com/max/8676/1*hRtNC4lqOBCCHOGnXgqFNw.png](https://miro.medium.com/max/8676/1*hRtNC4lqOBCCHOGnXgqFNw.png)

在 Statistic API 開發完成後，你就可以撰寫 Statistic Adapter，而它與 Fake Statistic 繼承同樣的介面，所以 Controller 也不必修改。此時的你也能夠專心撰寫 Statistic Adapter 的程式碼，也能同時產生**邊界測試**，卻保你們正確使用 API。

最後只要把在 Controller 呼叫的 Fake Statistic 物件替換成 Statistic Adapter 物件，Controller 就可以順利接上 Statistic API。

> 這種做法可以參考 🔗 [Design Patterns — Adapter Pattern](http://design%20patterns%20-%20adapter%20pattern/)

採用這種設計，Adapter 封裝了與 API 的互動，當 API 升級時，Adapter 是唯一需要被修改的地方。

```python
import abc

class Statistic(metaclass=abc.ABCMeta):
    @abc.abstractmethod
    def calculate_tourist_summary(self):
        pass

    @abc.abstractmethod
    def calculate_tourist_summary_after_105(self):
        pass

class FakeStatistic(Statistic):
    def calculate_tourist_summary(self):
        do something...

    def calculate_tourist_summary_after_105(self):
        do something...

class StatisticAdapter(Statistic):
    def calculate_tourist_summary(self):
        to do...
    
    def calculate_tourist_summary_after_105(self):
        to do...
```

# **小結**

在這個章節，我們學到 1️⃣ **邊界測試** 與 2️⃣ **Adapter**，它們都是為了讓我們能夠有效地掌控邊界的區域。

作者告訴我們，邊界是一個交戰非常猛烈的區域。不論是依賴第三方的軟體，或是面對未知的邊界，一不小心，就有可能讓戰火一發不可收拾。

我們必須控管這些邊界，所以我們需要使用最少量的程式碼將它們封裝起來，把邊界隔離出來，把**主導權放在我們能夠控制的程式上**。

# **✋ 延伸閱讀**

**[第 9 章 單元測試| Clean Code**Uncle Bob:「什麼是一個整潔的測試？三件事，可讀性，可讀性，還是可讀性。」medium.com](https://medium.com/%E6%89%8B%E5%AF%AB%E7%AD%86%E8%A8%98/unit-tests-ecc8ed18d583)