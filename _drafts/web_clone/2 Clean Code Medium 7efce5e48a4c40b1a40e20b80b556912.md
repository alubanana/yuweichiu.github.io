# 第 2 章 有意義的命名 | Clean Code - 手寫筆記 - Medium

[https://medium.com/%E6%89%8B%E5%AF%AB%E7%AD%86%E8%A8%98/meaningful-names-893d3ae05ec](https://medium.com/%E6%89%8B%E5%AF%AB%E7%AD%86%E8%A8%98/meaningful-names-893d3ae05ec)

![1*aIIRntdw2stBWukYPGJg8Q.jpeg](2%20Clean%20Code%20Medium%207efce5e48a4c40b1a40e20b80b556912/1aIIRntdw2stBWukYPGJg8Q.jpeg)

# 讓名稱代表意圖 — 使之名符其實

> 如果一個名稱還需要註解的輔助，那麼這個名稱就不具備展現意圖的能力。

我們看看以下的命名，變數 `s` 沒有傳達出任何的資訊，如果你看到這種命名應該會昏倒，尤其是整支程式充滿許多變數 `s` 時。

```python
s = settings # 設定 (settings)
```

我們應該選擇能夠**具體描述事件**，或者是**數量單位**的命名：

```python
elapsed_time_in_days = None
days_sinces_creation = None
days_since_modification = None
file_age_in_days = None
```

接下來，想想看，如果你要為這段程式碼寫下註解，你會寫什麼呢？

```python
def get_them():
    list1 = []
    for x in the_list:
        if x[0] == 4:
            list1.append(x)
    return list1
```

上述程式碼隱含著要求我們能夠回答下列問題：

- `the_list` 存放著什麼型態 (type) 的東西？
- `the_list` 裡 index 等於 0 的元素，代表的意義是什麼？
- 數值 4 的意義是什麼？
- 我們該如何使用回傳的 `list1`？

如果你發現很難以回答這些問題時，代表這段程式碼並沒有使用好的命名，它們應該要能夠展現這段程式碼想表達的意思。

實際上，現在正在開發的是一款踩地雷的遊戲，這個 `get_them()` 要做的事情是回傳已經被「**插旗**」的位置，所以我們可以這樣修改程式碼：

```python
def get_flagged_cells():
    flagged_cells = []
    for cell in gameBoard:
        if cell.is_flagged():
            flagged_cells.append(cell)
    return flagged_cells
```

是不是看起來可讀性提高非常多呢？只要適當的封裝，並且使用好的命名就能讓程式碼展現它們應該有的樣子。

> 在寫程式時，不妨時常問問自己：「這段程式碼能夠展現它們的意圖嗎？」

# 避免誤導

看了這張圖，你大概就知道作者想傳達什麼。

![2%20Clean%20Code%20Medium%207efce5e48a4c40b1a40e20b80b556912/Untitled.png](2%20Clean%20Code%20Medium%207efce5e48a4c40b1a40e20b80b556912/Untitled.png)

# 產生有意義的區別

作者也特別提出來表示「**相似的變數命名會造成開發困難**」。之前在 trace 別人的程式碼時，看到了以下的變數命名方式：

```python
reports
report_list
the_reports
```

以上程式碼似乎無法第一時間了解該變數想表達的意思，

> 在寫程式時，不妨時常問問自己：「這段程式碼能夠區別它們彼此的意圖嗎？」

# 類別的命名

類別和物件應該使用**名詞**或**名詞片語**來命名，不應該是**動詞**，我們用 Scrapy 這個 Python 的爬蟲框架舉例，在 [🔗 Crawler.py](https://github.com/scrapy/scrapy/blob/master/scrapy/crawler.py) 這個檔案中，包含了以下幾個類別：

![2%20Clean%20Code%20Medium%207efce5e48a4c40b1a40e20b80b556912/Untitled%201.png](2%20Clean%20Code%20Medium%207efce5e48a4c40b1a40e20b80b556912/Untitled%201.png)

可以看到，在類別命名時都是**名詞**。回想一下，過去寫程式時在引入物件時應該很少看到類別的名稱是動詞吧！

# 函式的命名

函式則應該使用**動詞**或**動詞片語**來命名，我們同樣用 [🔗 Crawler.py](https://github.com/scrapy/scrapy/blob/master/scrapy/crawler.py) 這個檔案來舉例，檔案裡面包含了以下幾個方法：

![2%20Clean%20Code%20Medium%207efce5e48a4c40b1a40e20b80b556912/Untitled%202.png](2%20Clean%20Code%20Medium%207efce5e48a4c40b1a40e20b80b556912/Untitled%202.png)

從 Cralwer 這個類別中，我們可以看大部分函式在命名時都使**以動詞為優先**，以適當的動詞命名，我們便不用透過註解就能清楚地明白這個函式究竟表達的意義是什麼。

有趣的是，在書中作者特地提到：「畢竟你不會想看到一個名為 `web_page()` 的函式，第一眼看到時我們無法確定它是一個 getter 還是 setter，或者有其他的功用。」

> 但是，在 Python 中因為 decorator 的存在，以及特別設計作為 getter 與 setter 的 decorator，所以時常會看到名詞的函式。不曉得有沒有沒有人為此感到頭痛 😅

在這個章節，我們知道變數命名應該要能夠充分描述它想表達的意圖，而且不應該使用會影響觀看的字母，例如 0 跟 O 幾乎難以分辨，或是 l 以及 1，不仔細看幾乎看不出其中的差異。

此外，盡量不要使用相似的名稱，像是 reports、the_reports，我們無法輕易分辨兩者的差異。

最後，則是函式及類別名稱的差異，函式可以使用**動詞**表達其行為，類別使用**名詞**描述其內部封裝的資料結構。

> 有意義的命名就像是高樓大廈的地基一樣，地基打穩，在樓層越來越高時，才不會容易因為意外讓高樓倒塌。

# ✋ 延伸閱讀