https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/  

### 解析器  
安裝lxml 解析器:```pip install lxml```  
python標準庫內有```html.parser```，但 lxml解析器(需安裝C)較快。  
使用:  
```BeautifulSoup(markup, 'html.parser')```  
```BeautifulSoup(markup, 'lxml')```  

### 使用
```
from bs4 import BeautifulSoup
soup = BeautifulSoup(open('index.html'))
soup = BeautifulSoup('<html>data</html>')
```
文檔都會被轉為Unicode，HTML也會被轉為Unicode編碼，  
BeautifulSoup會選擇解析器來解析，也可以照上面指定解析器。  

### 物件種類
BeautifulSoup 會將HTML轉為樹狀結構，每個節點是一個python物件，  
物件分為四種:```Tag```,```NavigableString```,```BeautifulSoup```,```Comment```  

##### Tag
兩個重要屬性:Name和Attributes。  
Attributes操作和dictionary相同，多值的attribute(如css class)則是回傳list，  
但XML文檔，tag不包含多值屬性。

##### NavigableString 可遍歷字串
支持大部分 Navigating the tree和 Searching the tree。  
NavigableString字串與Unicode字串相同，通過```unicode()```可轉變type。  
```unicode_string=unicode(tag.string)```  
tag內字串不能編輯，但能用```replace_with()```替換。

##### BeautifulSoup
BeautifulSoup物件表示一整個文檔，也支持大多 Navigating the tree和 Searching the tree，  
如果是tag時，還支持 Modifying the tree。

##### Comment
其實是NavigableString的變形，當出現在HTML檔時會以特殊格式輸出  
```
print(soup.b.prettify())
# <b>
#  <!--Hey, buddy. Want to buy a used parser?-->
# </b>
```

### Navigating the tree
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
#### 子節點
##### .contents 和 .children
tag 的 ```.contents```可將tag 的子節點以list輸出，字串沒有```.contents```，因為不會有子節點。  
不想取得list，也可用```.children```生成器(generator)來循環(iterate)。  

##### .descendants
```.contents```和```.children```只能取得子節點，  
```.descendants```則能取得所有子孫節點。

##### .string
如果tag只有一個為 NavigableString 的 child 或只含一個tag，而tag下也只有一個為 NavigableString 的 child，  
則能用```.string```直接取得字串。

##### .strings 和 .stripped_strings
如tag包含多個字串，可用```.strings```來循環獲取，而```.stripped_strings```則可取得去除空格和空行後的字串。

#### 父節點
##### .parent
取得父節點

##### .parents
可循環取得所有祖先節點

#### 兄弟節點
##### .next_sibling 和 .previous_sibling
可取的前後兄弟節點，  
但要注意在實際文檔中，
