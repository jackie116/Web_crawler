[BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)  

* [解析器](#1)

<span id='1'>
# 解析器 
安裝lxml 解析器:```pip install lxml```  
python標準庫內有```html.parser```，但 lxml解析器(需安裝C)較快。  
使用: 
```BeautifulSoup(markup, 'html.parser')```  
```BeautifulSoup(markup, 'lxml')```  

# 使用
```
from bs4 import BeautifulSoup
soup = BeautifulSoup(open('index.html'))
soup = BeautifulSoup('<html>data</html>')
```
文檔都會被轉為Unicode，HTML也會被轉為Unicode編碼，  
BeautifulSoup會選擇解析器來解析，也可以照上面指定解析器。  

# 物件種類
BeautifulSoup 會將HTML轉為樹狀結構，每個節點是一個python物件，  
物件分為四種:```Tag```,```NavigableString```,```BeautifulSoup```,```Comment```  

### Tag
兩個重要屬性:Name和Attributes。  
Attributes操作和dictionary相同，多值的attribute(如css class)則是回傳list，  
但XML文檔，tag不包含多值屬性。

### NavigableString 可遍歷字串
支持大部分 Navigating the tree和 Searching the tree。  
NavigableString字串與Unicode字串相同，通過```unicode()```可轉變type。  
```unicode_string=unicode(tag.string)```  
tag內字串不能編輯，但能用```replace_with()```替換。

### BeautifulSoup
BeautifulSoup物件表示一整個文檔，也支持大多 Navigating the tree和 Searching the tree，  
如果是tag時，還支持 Modifying the tree。

### Comment
其實是NavigableString的變形，當出現在HTML檔時會以特殊格式輸出  
```
print(soup.b.prettify())
# <b>
#  <!--Hey, buddy. Want to buy a used parser?-->
# </b>
```

# Navigating the tree
```
soup.head
# <head><title>The Dormouse's story</title></head>

soup.title
# <title>The Dormouse's story</title>

soup.body.b
# <b>The Dormouse's story</b>

soup.a
# <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>
```  
上面方法都只能取得第一個找的值，要取得全部要用 Seaching the tree，像 ```find_all()```  
## 子節點
### .contents 和 .children
tag 的 ```.contents```可將tag 的子節點以list輸出，字串沒有```.contents```，因為不會有子節點。  
不想取得list，也可用```.children```生成器(generator)來循環(iterate)。  

### .descendants
```.contents```和```.children```只能取得子節點，  
```.descendants```則能取得所有子孫節點。

### .string
如果tag只有一個為 NavigableString 的 child 或只含一個tag，而tag下也只有一個為 NavigableString 的 child，  
則能用```.string```直接取得字串。

### .strings 和 .stripped_strings
如tag包含多個字串，可用```.strings```來循環獲取，而```.stripped_strings```則可取得去除空格和空行後的字串。

## 父節點
### .parent
取得父節點

### .parents
可循環取得所有祖先節點

## 兄弟節點
### .next_sibling 和 .previous_sibling
可取得前後兄弟節點，  
但要注意在實際文檔中，```.next_sibling```和```.previous_sibling```，通常都是字串,空白或換行符。  

### .next_siblings 和 .previous_siblings
可循環取得前後所有兄弟節點

# 回退和前進
### .next_element 和 .previous_element
指向解析過程中下一個或前一個被解析對象(字串或tag)。  

### .next_elements 和 .previous_elements
可循環取得前後被解析對象。

# Searching the tree
### filters過濾器
可使用string/regular expression/list  
使用 ```True```取得所有。  
也可以自己寫function

## find_all()
```find_all(name, attrs, recursive, string, limit, **kwargs)```  
##### name參數
##### keyword參數
有些tag屬性在搜索中不能用，如html5中的 data-* 屬性:  
```
data_soup = BeautifulSoup('<div data-foo="value">foo!</div>')
data_soup.find_all(data-foo="value")
# SyntaxError: keyword can't be an expression
```
須改用 ```find_all()``` 的 ```attrs```參數定義一個dictionary參數來搜尋包含特殊屬性的tag:  
```
data_soup.find_all(attrs={"data-foo": "value"})
# [<div data-foo="value">foo!</div>]
```
*find_all()有name屬性，因此也不能當成keyword來使用，需用attrs方式搜尋*

##### css搜尋
*class在python中視保留字，因此要用class_*
```soup.find_all("a", class_="sister")```  
```class_``` 同樣接受不同filter

當你要搜尋多值的CSS class時，也能用CSS selector:
```
css_soup.select("p.strikeout.body")
# [<p class="body strikeout"></p>]
```

##### string參數
搜尋字串用

##### limit參數
可以設定符合條件的返回數量

##### recursive參數
find_all()會返回所有descendants，如果你只想獲得direct child，可以設定```recursive=False```  

### find_all()簡寫
```
soup.find_all("a")
soup("a")

soup.title.find_all(string=True)
soup.title(string=True)
```

## find()
```find( name , attrs , recursive , string , **kwargs )```  
```find_all()```會找查所有符合的tag，如果只要一個時，能設```limit=1```或用 ```find()```。  
差別是```find_all()```返回list，```find()```只返回結果。  
```find_all()```找不到值時返回空字串，```find()```返回```None```。  
上面提到的```find_all()```簡寫，靠的就是多次呼叫```find()```

### find_parents 和 find_parent
### find_next_siblings() 和 find_next_sibling()
### find_previous_siblings() 和 find_previous_sibling()
### find_all_next() 和 find_next()
### find_all_previous() 和 find_previous()
### CSS selectors
逐層找查   
```
soup.select("body a")
soup.select("html head title")
```  
直接子標籤    
```
soup.select("head > title")
soup.select("p > #link1")
soup.select("body > a")
```  
兄弟節點標籤  
```
soup.select("#link1 ~ .sister")
# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie"  id="link3">Tillie</a>]

soup.select("#link1 + .sister")
# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
```  
class名找查  
```
soup.select(".sister")
soup.select("[class~=sister]")
```  
tag的id找查  
```
soup.select("#link1")
soup.select("a#link2")
```  
多CSS元素查詢  
```
soup.select("#link1,#link2")
```  
通過是否存在某個屬性找查
```
soup.select('a[href]')
```  
通過屬性值查詢  
```
soup.select('a[href="http://example.com/elsie"]')
soup.select('a[href^="http://example.com/"]')
soup.select('a[href$="tillie"]')
soup.select('a[href*=".com/el"]')
```  
返回找到的第一個元素  
```
soup.select_one(".sister")
```

# Modifying the tree 
