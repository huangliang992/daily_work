# 约束驱动
文章提出了一种约束驱动的模型，在这个模型里面转换做的事是自动化产生模型约束，而非是产生完整的模型。

## 内容

传统模型转化面临的挑战：
![](http://i.imgur.com/Mhtoun7.png)

本文的做法：当不能得到精准的模型时，提供的是模型的约束来引导设计者，而非是生成模型。它认为一个消息过来，对应的类中就应该有一个同名的方法来满足它，如果没有就应该检查。

传统的模型转化公式：    
![](http://i.imgur.com/YrHdEg6.png)    
A是源模型（可以包含很多元素），Tm是转化模型（转化成模型的规则），Bg是目标模型
本文中的模型转化公式：    
![](http://i.imgur.com/fxED4v7.png)    
Tc是转化模型（是转化成约束的规则），C是约束模型，包含了一系列对Br的一致性检查规则。Br是受到限制的模型，由c限制。初始的Br=Bg。  
  
### 应用：

![](http://i.imgur.com/gIMTXco.png)

两条由实例或者消触发的规则（顺序图中）使用的是ATL语法，也可以用其他模型转化语言     
![](http://i.imgur.com/tkc4qUW.png)   
 
C包含的约束如下（用的是OCL作为约束语言）：

	c1 context Package inv: self.classes->exists(c|c.name='LightSwitch')
	c2 context Class inv:
	self.name='LightSwitch' implies
	self.providedMethods->exists(m|m.name='activate')
	c3 context Class inv:
	self.name='LightSwitch' implies
	self.providedMethods->exists(m|m.name='turnOff')

在上面例子中，Bg中就没有包含c2约束

### 增量约束模型管理
源模型，修改了，修改的部分就是增量，按理说要重新对源模型重新生成C，但不需要    
![](http://i.imgur.com/7JtGlXf.png)

![](http://i.imgur.com/3KgT98J.png)

![](http://i.imgur.com/KP50059.png)


### 约束验证和解空间

val :(m，c)->{ffalse; true} m是要验证的模型，c是约束模型，如果m违背c则返回false，反之返回true。

## 提供指导（给开发设计者）

1. 指出哪里不一致，比如要加什么
2. 哪些是验证过的，哪些是修改过的，并提供高亮显示

## 对验证可能造成的威胁

1. 没有验证能帮助快速检测不一致性到什么程度
2. 没有自动化消除不一致