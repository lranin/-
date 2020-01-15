## 注解的学习

* 注解的含义(个人理解): 丰富**元素**的metaData,使**元素**可以携带更多信息(信息意味着价值)
* 注解的意义: 
    1. 含义表达清晰,可读性高
    2. 剥离一些业务无关的通用型逻辑
* 注解的用法: 使用反射解析注解携带的信息,进行对应的操作

### 1. 元注解(meta annotation)
```java
/**
* Indicates how long annotations with the annotated type are to
* be retained.  If no Retention annotation is present on
* an annotation type declaration, the retention policy defaults to
* {@code RetentionPolicy.CLASS}.
* 标记: 该注解携带信息的级别
* EX.: RetentionPolicy.CLASS 意味着这个注解携带的信息会在编译时被记录到class文件中,
* 但不会在虚拟机运行时被加载
*/
@Retention

/**
* Indicates the contexts in which an annotation type is applicable. The
* declaration contexts and type contexts in which an annotation type may be
* applicable are specified in JLS 9.6.4.1, and denoted in source code by enum
* constants of {@link ElementType java.lang.annotation.ElementType}.
* 标记: 能该注解所标记的元素类型
* EX.: ElementType.FIELD 意味着该注解只能在域级别使用
*/
@Target

/**
* Indicates that annotations with a type are to be documented by javadoc
* and similar tools by default.  This type should be used to annotate the
* declarations of types whose annotations affect the use of annotated
* elements by their clients.  If a type declaration is annotated with
* Documented, its annotations become part of the public API
* of the annotated elements.
* 标记: 该注解所携带的信息将会被写入到各种工具所生成的文档中
*/
@Documented

/**
* Indicates that an annotation type is automatically inherited.  If
* an Inherited meta-annotation is present on an annotation type
* declaration, and the user queries the annotation type on a class
* declaration, and the class declaration has no annotation for this type,
* then the class's superclass will automatically be queried for the
* annotation type.  This process will be repeated until an annotation for this
* type is found, or the top of the class hierarchy (Object)
* is reached.  If no superclass has an annotation for this type, then
* the query will indicate that the class in question has no such annotation.
* 标记: 该注解将被自动继承
*/
@Inherited
```

### 2. 使用反射获取注解信息

```java
//注解定义
@Retention(value = RetentionPolicy.RUNTIME)
@Target(value = ElementType.TYPE )
public @interface FirstAnnotation {
    String desc() default "";

    String annotationName = "target is type annotation";
}
```

```java
@FirstAnnotation(desc = "类注解")
public class AnnotationTestClass {
    private String name;

    private void thisIsPrivateMethod() {
        System.out.printf("this is a private method");
    }

    @FirstAnnotation(desc = "public方法的注解")
    public void thisIsPublicMethod() {
        System.out.printf("this is a public method");
    }

    protected void thisIsProtectedMethod() {
        System.out.printf("this is a protected method");
    }
}
```

```java
public class AnnotationParsing {
    public static void main(String[] args) throws ClassNotFoundException {
        Class clazz = Class.forName("annotation.AnnotationTestClass");
        Arrays.stream(clazz.getAnnotations()).forEach(annotation -> {
            //输出annotation的类名
            System.out.println(annotation.annotationType().getName());
            //判断注解是否是FirstAnnotation
            if (clazz.isAnnotationPresent(FirstAnnotation.class)) {
                FirstAnnotation firstAnnotation = (FirstAnnotation) clazz.getAnnotation(FirstAnnotation.class);
                //获取指定字段
                Field testLight = firstAnnotation.getClass().getField("annotationName");
                //获取指定方法
                Method message = firstAnnotation.getClass().getMethod("desc");
            }
        });
    }
}
```