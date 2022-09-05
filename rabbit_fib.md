# 关于兔子繁殖算法总结
## 通过推理得出
方式一：

**思考过程**

1.第一个月1对幼年兔子，0对成年兔子

2.第二个月把幼年兔子加入成年兔子(因为第3个月生产出的幼年兔子等于当月的成年兔子)，0对幼年，1对成年

3.第三个月幼年兔子对数 = 前月成年兔子对数1，成年兔对数 = 前月幼兔对数+前月成年对数=0+1=1

总结公式：  
兔子总对数 = 成年兔子对数+幼年兔子对数  
成年兔子对数 = 前月成年兔子对数+前月幼年兔子对数 = 前月总对数  
幼年兔子对数 = 前月成年兔子对数 = 大前月兔子总对数（由上条公式得到）  
最终推导公式：兔子总对数 = 前月兔子总对数+大前月兔子总对数！ 这个公式正好符合Fibonacci数列

**范围更广的普遍推导**：  
先看表，再推理公式。  
![](/Users/wuyu/mynote/image/rabbit.png)
![](/Users/wuyu/mynote/image/rabbit1.png)

## 代码推理
### 面向过程
有了斐波那契数列概念后，计算兔子数量也就转化成了计算数列中第 n 项的问题。相应地，我们的思路也是如何通过代码计算该数列中的第 n 项，明显的是以计算过程为中心，也就是所谓的“面向过程”

```
def get_result(n):
    if n<3:
        result = 1
    else:
        x = 1
        y = 1
        for i in range(n-2):
            x,y = y,x+y
            
            return x# 获取第 10 个月的兔子数量；获取兔子数列的第 10 项result = get_result(10)print(result)
```

### 面向对象
面向对象是相对于面向过程来讲的，面向对象方法，把相关的数据和方法组织为一个整体来看待，从更高的层次来进行系统建模，更贴近事物的自然运行模式

#### 伪面向对象
继续拿兔子说事，如果尝试按照时间轴去计算兔子数量其实比较难整理，不妨换个角度，我们把一对兔子封装成一个“类”，这样题目中的每对兔子就都成了这个兔子“类”的实例，而且每一对兔子的属性和功能都是相同的：它们都可以拥有数量的属性，也都可以具有繁殖的功能，唯一的区别就是其“出生时间”不同，也就是对应的时间 n 不同。

将兔子对转化成具体对象后，我们想获取的是兔子数目，那么就可以给兔子实例添加一个变量 count 用来统计数目。对每对兔子这个对象来说，它所关联的数量是它自身 1 对 和所有兔宝宝数量的总和。而其兔宝宝这个对象是每个月都会产生，且该兔宝宝数量又和兔孙孙的数量有关……这样，我们通过定义“类”，变相地规定好了所有兔子对象的行为和属性

```
# 面向对象编程解决兔子问题# python 中通过 class 这个类来定义对象，我们给定义的对象取名 rabbit_pair (兔子对)
class rabbit_pair:
    # def 是用来定义这个对象能力的，首先通过 __init__ 给兔子定义数量标签 count 和 繁殖能力 product 
    def __init__(self,month):
        self.count = 1
        self.product(month)
        # 繁殖能力要具体表明，是和时间 month 相关的

    def product(self,month):
        # 当 月数大于 2 时，就会繁殖
        while month>2:
            # 繁殖出的小兔子也是同样的兔子对象，区别是它所经历的时间比父母要少 2 个月，所以时间 -2
            child = rabbit_pair(month-2)
            # 繁殖后，兔子对象数量标签要把繁殖出的小兔子数量标签数目也给加进来
            self.count += child.count
            # 通过月数控制，来模拟整个时间轴上兔子对象的繁殖与数量变化
            month -= 1

result = rabbit_pair(10).count
```

它就是套了个类的壳子，最终解决还是用到了递归。这里面的count和month两个标签都不属于兔子的真实属性。
####真面向对象
仍旧是定义兔子“类”，每一对兔子都是该兔子类的实例。兔子本身并不知道其它兔子数量，所以我们不再引入数量属性。每个兔子随着时间变化自己会年龄增长，年龄达到 3 个月就会繁殖兔崽子。所以，我们要为兔子“类”定义“年龄”属性和“繁殖”的方法；同时，也要定义一个“生长”的方法来控制年龄。（当然，将生长和繁殖定义到同一个方法中更省事）

这样，我们的兔子类便定义好了。接下来要做的是，随着时间变量的变化，我们来来让兔子对象们来生长和繁殖。

至于如何统计数量，我们可以为其建立个“族谱”，也就是所有兔子的列表，只要生成了新的兔子实例，便将其纳入列表中，最终便可以根据该列表长度获取兔子家族的数量了

```
class Rabbit:
    def __init__(self, period):
        self.age = 0 #兔子的兔年，代表是几月兔
        self.period = period #兔子的性成熟期

    def grow(self):
        #过一个月age就加1
        self.age += 1

    def product(self):
        if self.age > self.period:
            return Rabbit(self.period)
        else:
            return None

def rabbit_count(month, period):
    rabbits = []
    origin_rabbit = Rabbit(period)
    rabbits.append(origin_rabbit)
    for i in range(month):
        for rabbit in rabbits:
            rabbit.grow()
            child_rabbit = rabbit.product()
            if child_rabbit:
                rabbits.append(child_rabbit)

    return len(rabbits)
```



