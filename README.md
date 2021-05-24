# mvc 框架的(已经升级成mvvm)

## 框架设计目的
 该框架最初是制作Mmo所做，后来又应用到个人Horrible game（恐怖fps游戏所作）项目，以及最近与好有协助开发roguelike game，再此基础不断改进而成。框架适合模块化开发，协同工作，此后也会不断改进提升。

<p align="center">
    <img width="400px" src="https://github.com/Drinkwang/drinkwang.github.io/blob/master/img/git.png?raw=true">    
</p>

# 

## 框架的内容

```内容构成
* controller
* model
* view
* appfactory
* ovserver

```
### 使用教程
待补充



## Mvvm升级内容介绍（新）

### 使用教程

本次升级为model直接调用view提供桥梁，在每次修改具体model实例时都会刷新view（界面），使部分功能开发更为高效..

新升级内容完全兼容之前的内容，之前代码无需修改，仅在适宜自己功能需要时使用。

使用方法，在Model模块的单例模式的return instance前调用`instance.ModelToDoView();`这样每次获取model对象进行操作时候都会对View进行刷新

```c#
    public static AllTaskproxy instances()
    {
        if (instance == null)
        {
            instance = new AllTaskproxy();

        }
        instance.ModelToDoView();
        return instance;

    }
```

另外需要在任意unity对象中绑定指定的view，也就是调用

` Proxy.instance.regiestNewComponent（Vmediator）；`

这样桥梁就成功搭建了,此后每次调用Model对象单例时候都会调用View.refresh方法。

### 框架修改（仅配合理解框架）

具体框架修改内容如下：

model模块也就是Proxy的基类当中增加了二个方法，用来刷新“View”模块，也就是具体的Vmediator类

增加方法如下:

```c#
    public void regiestNewComponent(Vmediator t)
    {
        if (IComponentList == null)
        {
            IComponentList = new List<Vmediator>();
        }
        IComponentList.Add(t);
    }

    public void removeNewComponent(Vmediator t)
    {
        if (IComponentList != null && IComponentList.Count > 0)
        {
            IComponentList.Remove(t);
        }

    }
```

以及调用单例的方法

```c#
    public void ModelToDoView()
    {
        if (IComponentList != null && IComponentList.Count > 0)
        {
            foreach (Vmediator t in IComponentList)
            {

                t.refresh();
            }
        }

    }
```

## 数据处理

~~数据处理设想有二套方案，一套走`ScriptObject`，一套走`json`相关，目前将数据处理相关逻辑设计成`json`相关，`scriptObject`相关逻辑仅为学习参考。~~

目前数据文件是excel，但处理走的是json相关的逻辑。需要通过excel2json工具转换成对应`cs类和json`文件

下载链接和资料：https://neil3d.github.io/coding/excel2json.html

将`json文件和cs文件`导入到具体文件夹后，具体数据处理相关使用`ArchiveManager`这个类的单例，使用 `GetSamplelist<T>()`可以获取对应序列化实例对象的链表。直接可以用来使用。

![参考1](https://github.com/Drinkwang/jekyll2/blob/master/assets/cooperation/json1.png?raw=true)

![参考2](https://github.com/Drinkwang/jekyll2/blob/master/assets/cooperation/json2.png?raw=true)

```c#
//源码参考,其他非示例参考代码请自行理解
    public List<T> GetSamplelist<T>()where T:class {
        var t = ArchiveManager.Instance.Retrieve<T>();
        var content = t.ToJson();
        List<T> list = JsonMapper.ToObject<List<T>>(content);
        return list;
    }
    
```




## Authors

* **Drinker·wang** - [Github](https://github.com/Drinkwang)
<br>![B站 Follow](https://space.bilibili.com/13061595)  

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details
