---
layout: post
title: Spring Boot注解之@Component
date: 2018-06-06
tag: Spring注解系列
---

```text
1、@controller 控制器（注入服务）
2、@service 服务（注入dao）
3、@repository dao（实现dao访问）
4、@component （把普通pojo实例化到spring容器中，相当于配置文件中的<bean id="" class=""/>）
```

### @Component

#### **第一种写法：@Component（"xxxx"）**

这种注解带有name属性值，在使用getBean获取bean时，beanId需要填写name，否则报错

#### **第二种写法：@Component**

这种注解相反并不需要填写name，而是more以类名`小驼峰`方式获取

#### **解释**

`@component`（把普通pojo实例化到spring容器中，相当于配置文件中的 `<bean id="" class=""/>` ）
泛指各种组件，就是说当我们的类不属于各种归类的时候（不属于`@Controller`、`@Services`等的时候），我们就可以使用`@Component`来标注这个类。

#### **案例**

```xml
<context:component-scan base-package="com.*"> 
```

上面的这个例子是引入 `Component` 组件的例子，其中 `base-package` 表示为需要扫描的所有子包。 

### 拓展

1.@controller 控制器（注入服务）

```text
用于标注控制层，相当于struts中的action层
```

2.@service 服务（注入dao）

```text
用于标注服务层，主要用来进行业务的逻辑处理
```

3.@repository（实现dao访问）

```text
用于标注数据访问层，也可以说用于标注数据访问组件，即DAO组件
```

### 共同点

被 `@controller` 、 `@service` 、 `@repository` 、 `@component` 注解的类，都会把这些类纳入进spring容器中进行管理。