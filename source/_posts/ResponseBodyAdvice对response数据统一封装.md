---
title: ResponseBodyAdvice对response数据统一封装
tags:
  - Java
date: 2021-11-23 23:29:39
---

## 使用背景
SpringBoot开发web应用时，只返回请求结果往往是不够的，需要补充返回消息、封装响应代码等，我们会封装成类似如下代码的结构。
```java
@Getter
@Setter
@ToString
public class ResultDTO<T> {
    private String message;
    private Integer code;
    private T data;
}
```
用于返回下面这种数据:
```json
{
    "message": "获取成功",
    "code": 200,
    "data": [
        {
            "id": "d47ebc6e-5d30-4c16-9707-57fd6e1fa0f2",
            "name": "test",
            "age": 18,
            "address": "测试地址"
        }
    ]
}
```
但是如果应用对外提供feign的调用形式，如果依旧采用这种形式，客户端就需要解析ResultDTO，再从中获取data数据。
当然我们可以单独再写一个直接返回data的rest接口，但这样有点low。
## 解决方案
增加逻辑，对supports中的判断做区分, 如果是feign调用，不执行beforeBodyWrite。例如：在feign调用中增加特殊的请求头，检测到该头返回false
```java
// basePackages可以设置ResponseBodyAdvice的生效范围
@RestControllerAdvice(basePackages = "cn.qiudev.demo")
public class ResponseAdvice implements ResponseBodyAdvice<Object> {
    @Override
    public boolean supports(MethodParameter returnType, Class<? extends HttpMessageConverter<?>> converterType) {
        //todo 通过MethodParameter其中的参数确定是否为feign调用,feign调用直接返回原结构
        return true;
    }

    @Override
    public Object beforeBodyWrite(Object body, MethodParameter returnType, MediaType selectedContentType, Class<? extends HttpMessageConverter<?>> selectedConverterType, ServerHttpRequest request, ServerHttpResponse response) {
        if (body instanceof ResultDTO) {
            return body;
        } else {
            ResultDTO<Object> resultDTO = new ResultDTO<>();
            resultDTO.setData(body);
            resultDTO.setMessage("获取成功");
            resultDTO.setCode(200);
            return resultDTO;
        }
    }
}
```
这样restController中就不用再返回我们统一封装的ResultDTO了。