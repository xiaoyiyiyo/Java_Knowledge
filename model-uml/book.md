# (建模)UML

## 一. 前序

### 1. 怎么做到的

> *如何来描绘现实世界并解决面向对象的困难*

#### 1.1 从现实世界到业务模型

映射人、事、物、规则-

* 参与者(actor)
* 用例(use case)  
表示参与者的业务目标，也就是参与者想要做什么并且获得什么。对应着现实中的“事”  
* 业务场景(business scenario) 和 用例场景(use case scenario)  
用其UML的视图来描绘事怎么做，依据什么规则。对应着“规则”

* 业务对象模型  
用其UML的视图来说明在达成这些业务目标的过程中涉及到的事物。对应着“物”

![png1.6](https://github.com/xiaoyiyiyo/Java_Knowledge/blob/master/model-uml/png/png1.6.png

#### 1.2 从业务模型到概念模型

> 通过称之为概念化的过程来建立适合计算机理解和实现的模型，这个模型称为**分析模型**  
> 介于原始需求和计算机实现之间，向上映射原始需求，向下为计算机实现规定一种高层次的抽象和指导

分析模型的元模型：  

* 边界类  
边界决定了外面能对里面做什么“事”

* 实体类  
业务实体的实例化结果

* 控制类  
表述原始需求中的动态信息，即业务或用例场景中步骤和活动  
(UML观点：实体类、边界类、以及自身之间不能够直接相互访问，需要通过控制类来代理访问 )

![png1.7](https://github.com/xiaoyiyiyo/Java_Knowledge/blob/master/model-uml/png/png1.7.png

#### 1.3 从概念模型到设计模型

概念模型会因为软件架构、框架等选择不同而得到不同的设计模型

![png1.8](https://github.com/xiaoyiyiyo/Java_Knowledge/blob/master/model-uml/png/png1.8.png

#### 1.4 总结

要解决面向对象的困难我们需要一些方法：

* 一种把现实世界映射到对象世界的方法

* 一种用对象世界描述现实世界的方法

* 一种验证对象世界行为正确反映了现实世界的方法

![png1.9](https://github.com/xiaoyiyiyo/Java_Knowledge/blob/master/model-uml/png/png1.9.png

## 二. 建模基础

### 2.1 建模  
> 通过对客观事物建立一种抽象的方法用以表征事物并获得对事物本身的理解，同时将这种理解概念化，将这些逻辑概念组织起来，构成一种对所观察的对象的内部结构和工作原理的便于理解的表达

问题：  

* 怎么建
  * 抽象角度  
  * 做需求的时候，首要目标不是弄清楚如何一步一步完成，而是要弄清楚有多少业务参与者，每个参与者的目标。实际上为**用例**

* 模是什么
  * 确定了抽象角度(目标)后，之后要找出满足这一目标的事物
  * 找寻过程不是面向对象而是过程化的。  
  (谁发出动作，作用于什么事物，产生了什么后果) 过程化的描述方式
  * 目的是找出事物。我们不试图控制场景，场景会因条件变化而改变

![png2.1](https://github.com/xiaoyiyiyo/Java_Knowledge/blob/master/model-uml/png/png2.1.png