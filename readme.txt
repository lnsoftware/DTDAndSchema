******************************************
	DTD 和 Schema
******************************************
参考:http://blog.csdn.net/sunxing007/article/details/5684265

1. DTD
	
	1.1 一个XML文档由以下模块构成
		元素：即一个节点
		属性：提供元素的额外信息
		实体：预定义的普通文本
		PCDATA 会被解析的文本
		CDATA 不会被解析的文本
		
		DTD 分别用ELEMENT/ATTLIST/ENTITY/#PCDATA/#CDATA来描述
	
	1.2 关于ELEMENT
		<!ELEMENT 元素名称 EMPTY>  描述空元素   <!ELEMENT br EMPTY>
		<!ELEMENT 元素名称 (#PCDATA)>  描述只有PCDATA的元素  <!ELEMENT from (#PCDATA)>
		<!ELEMENT 元素名称 ANY>  描述任何可包含可被解析的元素. 不常用
		<!ELEMENT 元素名称 (子元素名称1, 子元素名称2) >  描述元素包含的子元素序列   <!ELEMENT note (to, from, heading, body)>
		可以限制元素出现的次数 "?" 表示一次  "*" 表示多次
		<!ELEMENT web-app (icon?, display-name?, description?, context-param*, filter*, filter-mapping*,role*)>
		也可以描述或关系
		<!ELEMENT note (to, from, header, (message|body)), 表示note元素 必须包含to, from, header但只能包含message或者body
		
	1.3 关于ATTLIST
		<!ATTLIST 元素名称 属性名称 属性类型 默认值>
		<payment type="checked"> 对应的DTD描述<!ATTLIST payment type CDATA "checked">
		
		属性类型列表：
		1) CDATA 值为字符数据
		2) (en1|en2|...)  为枚举列表中的一个值
		3) ID 值为唯一的id
		4) IDREF 值为另一个元素的id
		5) IDREF 值为其他id的列表
		6) NMTOKEN 值为合法的XML名称
		7) NMTOKENS 值为合法的XML名称列表
		8) ENTITY 值为一个实体
		9) ENTITYS 值为一个实体列表
		10) xml 值是一个预定义的xml
		11) NOTATION 次值是符合的名称
		
		值的解释
		1) 具体值  属性的默认值
		2) #REQUIRED 属性值是必须的
		3) #IMPLIED  属性值不是必须的
		4) FIXED value 属性值是固定的
		<!ATTLIST payment fax CDATA #IMPLIED> 则<payment fax="12332"> 是合法的定义
		
		关于实体
		<!ENTITY 实体名称 "实体的值"> 
		<!ENTITY writer "Alan">
		<!ENTITY worker "Java Programmer">
		则可以这样引用
		<author>&writer, &worker</author>
	
	举例：
		1. xml文件
		<?xml version="1.0" standalone="no"?>  
		<!DOCTYPE tvschedule SYSTEM "TVSCHEDULE.dtd">  
		<tvschedule name="ANHUI WEISHI">  
		        <channel chan="123">  
	                <banner>BANNEL 1</banner>  
	                <day>  
                        <date></date>  
                        <holiday>HOLIDAY</holiday>  
	                </day>  
	                <day>  
                        <date></date>  
                        <programslot vtr="">  
                            <time></time>  
                            <title rating="" language="EN">12</title>  
                            <description></description>  
                        </programslot>  
	                </day>  
		        </channel>  
		</tvschedule> 		
		
		2. dtd文件(+号代表多个)
		<!ELEMENT tvschedule (channel+)>  
		<!ELEMENT channel (banner,day+)>  
		<!ELEMENT banner (#PCDATA)>  
		<!ELEMENT day (date,(holiday|programslot+)+)>  
		<!ELEMENT holiday (#PCDATA)>  
		<!ELEMENT date (#PCDATA)>  
		<!ELEMENT programslot (time,title,description?)>  
		<!ELEMENT time (#PCDATA)>  
		<!ELEMENT title (#PCDATA)>   
		<!ELEMENT description (#PCDATA)>  
		  
		<!ATTLIST tvschedule name CDATA #REQUIRED>  
		<!ATTLIST channel chan CDATA #REQUIRED>  
		<!ATTLIST programslot vtr CDATA #IMPLIED>  
		<!ATTLIST title rating CDATA #IMPLIED>  
		<!ATTLIST title language CDATA #IMPLIED>
		
		
		
2. SCHEMA
	
	2.1 SCHEMA 是DTD的替代者
		1) 定义出现在XML文档的元素
		2) 定义出现在XML文档中的属性
		3) 定义哪个元素是子元素
		4) 定义子元素的次序
		5) 定义子元素的数目
		6) 定义元素是否为空，或者是否可包含文本
		7) 定义元素和属性的数据类型
		8) 定义元素和属性的默认值和固定值

	
	举例：
		xml文件
		
		<?xml version="1.0"?>  
		<note  
			xmlns="http://www.w3school.com.cn"  
			xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
			xsi:schemaLocation="http://www.w3school.com.cn/note.xsd">  
			  
			<to>George</to>  
			<from>John</from>  
			<heading>Reminder</heading>  
			<body>Don't forget the meeting!</body>  
		</note>  

		note.xsd文件
				
		<?xml version="1.0"?>  
		<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"  
			targetNamespace="http://www.w3school.com.cn"  
			xmlns="http://www.w3school.com.cn"  
			elementFormDefault="qualified">  
		  
		<xs:element name="note">  
		    <xs:complexType>  
		      <xs:sequence>  
			    <xs:element name="to" type="xs:string"/>  
			    <xs:element name="from" type="xs:string"/>  
			    <xs:element name="heading" type="xs:string"/>  
			    <xs:element name="body" type="xs:string"/>  
		      </xs:sequence>  
		    </xs:complexType>  
		</xs:element>  
		  
		</xs:schema>
		
		
		一个xml文档可以使用多个命名空间, 命名空间需要先声明, 并给它取别名, 然后依次给出每个命名空间的schema文件地址.如下代码解释希望可以帮助你理解上面臭长的命名空间声明:
		[xhtml] view plaincopy
		<?xml version="1.0" encoding="UTF-8"?>  
		<root xmlns="默认命名空间,它不需要取别名"      
		        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"    
		        xmlns:ns1别名="namespace1"    
		        xmlns:ns2别名="namespace2"    
		        xsi:schemaLocation="默认命名空间,它不需要取别名" "默认命名空间的xsd文件地址"     
		                namespace1 namespace1的xsd文件地址  
		                namespace2 namespace2的xsd文件地址>    
		        ......  
		</root>  
		
		需要注意的是xsi命名空间几乎是必要的, 因为需要用xsi:schemaLocation来申明每个命名空间的schema文件地址.
		对于上面给出的note.xsd. 因为它本身也是一个xml, 它要使用schema命名空间, 所以它也要申明命名空间. 				 