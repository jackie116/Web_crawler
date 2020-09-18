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
