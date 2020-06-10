# 第 6 章 物件及資料結構| Clean Code - 手寫筆記 - Medium

[https://medium.com/%E6%89%8B%E5%AF%AB%E7%AD%86%E8%A8%98/objects-and-data-structures-354cf1e302c8](https://medium.com/%E6%89%8B%E5%AF%AB%E7%AD%86%E8%A8%98/objects-and-data-structures-354cf1e302c8)

![1*U8RD8lCnkkhqf2rUZOC1vg.jpeg](6%20Clean%20Code%20Medium%20927c7c2de43741a0af572e5a56ea5349/1U8RD8lCnkkhqf2rUZOC1vg.jpeg)

```
🔖 文章索引1. Getter-Setter and private variables
2. 資料結構與物件的差別
3. 德摩特爾法則 (The Law of Demeter)
4. 小結
```

# **Getter-Setter and private variables**

作者在書中提醒我們這件事，我們使用私有 (private) 變數的理由是「**不希望任何人依賴這些變數**」，但是有許多工程師會自動為它們加上 getter 與 setter，讓私有變數如同公用 (public) 變數一般。

> 延伸閱讀： 🔗 Why do we need private variables?

# **資料結構與物件的差別**

在這個章節中，作者解釋**資料結構 (Data Structure) 與物件 (Object) 為何不同**，而且它們是反對稱性的存在：

- **資料結構**：將資料直接暴露在外，可以直接對資料讀取與寫入。例如：list、dict、set 等。
- **物件**：將資料隱藏起來，提供可以操作這些資料的函式在外面。例如： [🕸 Crawler](https://github.com/scrapy/scrapy/blob/master/scrapy/crawler.py#L28)、[🕸 CrawlerRunner](https://github.com/scrapy/scrapy/blob/master/scrapy/crawler.py#L119)、 [🕸 CrawlerProcess](https://github.com/scrapy/scrapy/blob/master/scrapy/crawler.py#L240)。

> 🔗 這一篇 對於資料結構與物件有很好的解釋，大家不妨可以看看。

# **德摩特爾法則 (The Law of Demeter)**

德摩特爾法則又可以稱作「**最少知識原則**」，引用至 [🔗 維基百科上對於 LoD 的敘述](https://en.wikipedia.org/wiki/Law_of_Demeter)：

```
符合 LoD 的函式要求在物件 O 中的函式 m 只能調用以下幾種類型的函式：
1. O 本身
2. m 的參數
3. 在 m 中建立的物件
4. 宣告在 O 中的物件
5. 被 O 存取的全域變數，並且在 m 的 scope 中
```

LoD 範例使用 Python：

```python
e = E()

class O:
    def __init__(self):
        self.a = A()

    def m(self, b):
        result = self.f()   # O 本身的函式
        b.execute()         # m 的參數
        d = D()             # 在 m 中建立的物件
        d.join()
        self.a.run()        # 宣告在 O 中的物件
        e.jump()            # 被 O 存取的全域變數，並且在 m 的 scope 中

    def f(self):
        return 1+1
```

如果你在網路上搜尋，也許會看到有人說：「LoD 禁止在一行程式碼中使用多個 dot」，這句話是一半對一半錯，使用多個 dot 的前題是「**你知道它們是資料結構還是物件**」。

如果是物件，則破壞了物件封裝；而如果是資料結構，資料結構本來就暴露了資料在外面，所以透過巢狀存取資料是沒有問題的。

```cpp
// 破壞封裝，違反 LoD
class A {
    fun(b) {
        b.getOption().getSelection()
    }
}

// 存取資料結構，沒有違反 LoD 
data = {
    a: {
        b: 2
    }
}
console.log(data.a.b)
```

> 你不應該讓一個函式知道太多事情，否則會破壞物件封裝原本的意義。

# **小結**

在物件與資料結構這個章節，我們首先談到物件的私有變數不應該隨意將它們暴露在外，物件即是要隱藏其內容，只讓外部的人看見它們想公開的行為；因此，開發時必須小心謹慎管理私有變數，否則私有變數將如同公有變數影響整個系統。

此外，我們談到資料結構與物件的不同之處，你經常會在寫程式時用到這兩種技巧，但是它們的意義是不一樣的，為了方便管理與維護，必須時時刻刻在意究竟現在寫的是資料結構還是物件。

最後，德摩特爾法則是一種撰寫程式的規範，我們不應該知道太多物件內的事情，否則就會破壞物件的封裝，讓被使用的物件難以被維護。

# **Reference**

- [Getter-Setter and private variables](https://stackoverflow.com/questions/22008180/getter-setter-and-private-variables/22008228#22008228)
- [封裝與迪米特法則](https://www.ithome.com.tw/voice/98670)
- [Whats the difference between objects and data structures?](https://stackoverflow.com/questions/23406307/whats-the-difference-between-objects-and-data-structures)
- [Objects vs. Data Structures](https://hackernoon.com/objects-vs-data-structures-e380b962c1d2)
- [Object Oriented Tricks: #2 Law of Demeter](https://hackernoon.com/object-oriented-tricks-2-law-of-demeter-4ecc9becad85)

# **✋ 延伸閱讀**

**[第 7 章 錯誤處理 | Clean Code**Uncle Bob：「如果我們不需要處理例外的情況，那不是更好嗎？」medium.com](https://medium.com/%E6%89%8B%E5%AF%AB%E7%AD%86%E8%A8%98/error-handling-6132b22fe5c2)