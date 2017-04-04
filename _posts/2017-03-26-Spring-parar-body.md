---
layout: post
title: Spring  @RequestParamer & @RequestBody 区别
date: 2017-03-26
tag: Spring
---

### @RequestParamer

常用来处理简单类型的绑定，通过 `Request.getParameter()` 方式获取参数，可以处理  `get` 方式中 `queryString` 的值，也可以处理 `post` 方式中 `body data` 的值.

用来处理Content-Type: 为 application/x-www-form-urlencoded编码的内容，提交方式GET、POST；

该注解有两个经常使用的属性： value、required； value用来指定要传入值的id名称，required用来指示参数是否必须绑定；

```java
@Controller  
@RequestMapping("/pets")  
@SessionAttributes("pet")  
public class EditPetForm {  
  
    // ...  
  
    @RequestMapping(method = RequestMethod.GET)  
    public String setupForm(@RequestParam("petId") int petId, ModelMap model) {  
        Pet pet = this.clinic.loadPet(petId);  
        model.addAttribute("pet", pet);  
        return "petForm";  
    }  
  
    // ... 
```

### @RequestBody

该注解常用来处理Content-Type: 不是application/x-www-form-urlencoded编码的内容，例如application/json, application/xml等；

它是通过使用HandlerAdapter 配置的HttpMessageConverters来解析post data body，然后绑定到相应的bean上的。

因为配置有FormHttpMessageConverter，所以也可以用来处理 application/x-www-form-urlencoded的内容，处理完的结果放在一个MultiValueMap<String, String>里，这种情况在某些特殊需求下使用，详情查看FormHttpMessageConverter api;

```java
@RequestMapping(value = "/something", method = RequestMethod.PUT)  
public void handle(@RequestBody String body, Writer writer) throws IOException {  
  writer.write(body);  
}  
```