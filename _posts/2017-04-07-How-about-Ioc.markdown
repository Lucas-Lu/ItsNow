---
layout:     post
title:      "Haste does not bring success."
subtitle:   "依赖注入学习小结"
date:       2017-04-07 23:00:00
author:     "Lucas Lu"
header-img: "img/post-bg-11.jpg"
---

<p>很抱歉没能继续努力学习python。最近因为面试的原因嘛，恶补了一些C#的基础知识，加上之前在学习和工作的时候也慢慢有些面对对象编程的知识积累。
	正好最近看到设计模式提到的依赖注入，想到之前工作的时候用的一些框架当中有一部分依赖注入的思想还不是特别理解，因此在这里对最近学习的依赖注入做一个总结。</p>

<p>首先码一下概念，由于客户类只依赖于服务类的一个接口，而不依赖于具体的服务类，所以客户类只需要定一个注入点。而在程序运行的过程中，客户类不直接实例化具体服务类，
而是客户运行上下文环境或者其他组件比如说工厂类，然后将其注入到客户类中，保证客户类的正常运行。</p>

<p>依赖注入的主要类型有Setter注入、构造器注入、依赖获取等。嗯，就着代码解释：</p>

<font style="color:green">//定义注入客户类的接口</font>
<pre>
public interface IEat{
    string eat();
}
</pre>

<font style="color:green">//定义接口的实现类</font>
<pre>
internal sealed class CarnivoresEat:IEat{
    public string eat(){
        return "eat meat";
    }
}

internal sealed class HerbivoreEat:IEat{
    public string eat(){
        return "eat vegetable";
    }
}
</pre>


<font style="color:green">//定义客户类</font>
<pre>
public class Animal{
    public Animal(string name){
        this.name = name;
    }
    public IEat eat;
    protected string name;
    public void haveMeal(){
        console.write(name + eat.eat());
    }
}
</pre>

<font style="color:green">//主函数通过給客户类的接口赋值，实现客户类接口的注入，这种注入叫Setter注入。</font>
<pre>
void main(){
    var tiger = new Animal("tiger");
    tiger.eat = new CarnivoresEat();
    tiger.haveMeal();
}
</pre>

<p>至于构造器注入其实就是换一个注入点，通过构造函数的时候就直接注入接口实现类。如下：</p>

<font style="color:green">//修改客户类</font>
<pre>
public class Animal{
    public Animal(string name，IEat eat){
        this.name = name;
        this._eat = eat;
    }
    public IEat _eat;
    protected string name;
    public void haveMeal(){
        console.write(name + _eat.eat());
    }
}

void main(){
    var = new CarnivoresEat();
    var tiger = new Animal("tiger",eat);
    tiger.haveMeal();
}
</pre>

<p></p>

