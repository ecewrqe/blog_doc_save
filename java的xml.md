xml解析方式：
dom或dom4j解析
sax解析

dom解析为全部解析成对象，然后再调用。常用dom4j解析，需要按照插件
sax解析为边读边解析。适合大xml文件

### dom4j
思路：
添加解析器SAXReader -> 通过解析器创建对象树Document，再通过Document解析出一个个节点Element


```
SAXReader reader = new SAXReader();
try{
	//解析器读取文件，变成document
	Document document = reader.read("conf/student.xml");

	//获取根节点
	Element rootElement = document.getRootElement();
	System.out.println(rootElement);

	//指定标签名获取标签对象
	Element studentElement = rootElement.element("student");
	//获取属性值
	System.out.println(studentElement.attributeValue("id")); 
	//获取标签名
	System.out.println(studentElement.getName());
	//获取该标签的所有文本内容
	System.out.println(studentElement.getStringValue());


}catch(Exception e){
	e.printStackTrace();
}
```
遍历节点
```
for(Iterator<Element> itr = rootElement.elementIterator();itr.hasNext();){
	Element temp = itr.next();
	for(Iterator<Element> innerIter = temp.elementIterator(); innerIter.hasNext();){
	Element elt = innerIter.next();
	//getStringValue是获取标签的文本内容
	System.out.println(elt.getStringValue());
	}
}
```

### sax解析
四部曲：
创建解析工厂 -> 创建解析器

导入包：
javax.xml.Parsers

SAXParserFactory
SAXParser
提供parser方法：

### 用xpath和dom4j来解析
xpath用路径的方式表示，沿着路径来找。scrapy就是用xpath来找元素的。
```
SAXReader reader = new SAXReader();
try{
	Document document =  reader.read("conf/student.xml");
	Element student_name = (Element)document.selectSingleNode("/students/student[3]/name");
	System.out.print(student_name.getStringValue());

}catch(Exception e){
	e.printStackTrace();
}
```

selectSingleNode()  写xpath表达式，返回的结果是什么就强制转换成什么类型，一般有Element，值的类型是DefaultText

#### xpath基本语法：
```
/xxx  下一级xxx的第一个标签
//xxx  下几级中有xxx的第一个标签
.  当前目录
..  上级目录
@  属性选择器

*  为所有标签
|   选取多个
=======================
/student[@id='stu01']   找根下一级的student标签，属性为id，id的值为stu01的
/student[1]    找根下一级的所有student标签的第一个标签
/student[last()]  找根下一级的所有student标签的最后一个标签

text()    获取值，值的类型是DefaultText
```

### 用xpath解析
创建解析器->解析xml
创建XPath->用xPath的evaluate(传入解析器)

```
//创建解析器
DocumentBuilderFactory documentBuilderFactory = DocumentBuilderFactory.newInstance();
DocumentBuilder documentBuilder = documentBuilderFactory.newDocumentBuilder();
Document document = documentBuilder.parse("conf/student.xml");

//创建XPath
XPathFactory xPathFactory = XPathFactory.newInstance();
XPath xPath = xPathFactory.newXPath();

//用xPath的evaluate解析
/*
public Object evaluate(String expression, Object item, QName returnType)
throws XPathExpressionException;
expression:  xpath表达式
item: 实际上是上下文阅读器，这里用W3C的document
returnType: 返回类型，XPathConstants下面有可用QName的类型
(NUMBER,STRING,BOOLEAN,  都是映射到java类型
NODESET  映射到w3c的Nodelist  节点列表)
 */
String name = (String)xPath.evaluate("/students/student",
        document, XPathConstants.STRING);

System.out.println(name);
```










