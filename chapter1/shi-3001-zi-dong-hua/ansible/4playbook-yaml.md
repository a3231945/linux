**1、YAML介绍**

	YAML是一个可读性高的用来表达资料序列的格式。YAML参考了其他多种语言，包括：XML,C语言、Python、Perl以及电子邮件格式RFC2822等，
	Clark Evans在2001年首次发表了这种语言，另外ingy dot Net与Oren Ben-Kiki也是这语言的共同设计者
	
	YAML Ain t  Markup Language,即yaml不是xml，
	不过，在开发的这种语言时，YAML的意思是： Yet Anther Markup Language("仍是一种语言")，
	其特性：
		YAML的可读性好
		YAML和脚本语言的交互性好
		YAML使用实现语言的数据类型
		YAML有一个一致性的信息模型
		YAML易于实现
		YAML可以基于流来处理
		YAML表达能力强，扩展性好
		更多内容及规范参见：http://www.yaml.org
	
**2、YAML语法**

	YAML的语法和其他高阶语言类似，并且可以简单表达清单、散列表、标量等数据结果，
	其结构（Structure）通过空格来展示，序列（Sequence）里的项用“”来代表，map里的键值对用“:”分割。
	实例{
		name: John Smith
		age: 41
		gender: Male
		spose:
		    name： Jane Smith
			age：37
			gender: Female
		children:
		    - name: Jimmy Smith 
			  age: 17
			  gender: Male
			- name: Jenny Smith
			  age: 13
			  gender: Female
	YAML文件扩展名通常为".yaml" 例如：example.yaml
	}
**3、list**

		列表的所有元素均用“-” 打头
		例如：
		# A list of tasty fruits
		- Apple
		- Orange
		- Strawberry
		- Mango
	
**4、dictionary**

		字典通过key 与value进行标识
		例如：
		#An employee record
		name: Example Developer
		job: Developer
		skill: Elite
		也可以将key:value放置于{}中进行表示，例如：
		---
		#An employer record
		{name: Example Developer, job: Developer, skill: Elite}
