# RESTFUL风格 Notes

```properties
spring.mvc.hiddenmethod.filter.enabled=true
```

```html
1. <for action="SpringAnnotation/testRest/1" method="post"> 
2.     <input type="hidden" name="_method" value="PUT"/>
3.     <input type="submit" value="TestRestPut"/>
4. <form>
```