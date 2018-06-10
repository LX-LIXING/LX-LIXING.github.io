---
layout: post
title:  "Java反射随笔"
date:   2017-12-06
categories: Java
tags: 反射
excerpt: 记录反射类。
---

* 文档结构
{:toc}

这里记录一些我用到的或者见到的比较好用方便的前端开发相关的工具吧。

---


# 反射

##  1.Java反射机制类
 
    java.lang.Class; //类               
    java.lang.reflect.Constructor;//构造方法 
    java.lang.reflect.Field; //类的成员变量       
    java.lang.reflect.Method;//类的方法
    java.lang.reflect.Modifier;//访问权限
    
## 2.Java反射机制实现

1. class对象的获取

- 第一种方式 通过对象getClass方法

        Person person = new Person();
        
        Class<?> class1 = person.getClass();
    
- 第二种方式 通过类的class属性

         class1 = Person.class;
         
- 第三种方式 通过Class类的静态方法——forName()来实现

        try {
            class1 = Class.forName("com.whoislcj.reflectdemo.Person");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        
## 3.获取class对象的摘要信息

    boolean isPrimitive = class1.isPrimitive();//判断是否是基础类型
    boolean isArray = class1.isArray();//判断是否是集合类
    boolean isAnnotation = class1.isAnnotation();//判断是否是注解类
    boolean isInterface = class1.isInterface();//判断是否是接口类
    boolean isEnum = class1.isEnum();//判断是否是枚举类
    boolean isAnonymousClass = class1.isAnonymousClass();//判断是否是匿名内部类
    boolean isAnnotationPresent = class1.isAnnotationPresent(Deprecated.class);//判断是否被某个注解类修饰

    String className = class1.getName();//获取class名字 包含包名路径
    Package aPackage = class1.getPackage();//获取class的包信息
    String simpleName = class1.getSimpleName();//获取class类名
    int modifiers = class1.getModifiers();//获取class访问权限

    Class<?>[] declaredClasses = class1.getDeclaredClasses();//内部类
    Class<?> declaringClass = class1.getDeclaringClass();//外部类
    
## 3.获取class对象的属性、方法、构造函数等

    Field[] allFields = class1.getDeclaredFields();//获取class对象的所有属性
    Field[] publicFields = class1.getFields();//获取class对象的public属性
    try {
        Field ageField = class1.getDeclaredField("age");//获取class指定属性
        Field desField = class1.getField("des");//获取class指定的public属性
    } catch (NoSuchFieldException e) {
      e.printStackTrace();
    }
    
    ---
    
    Method[] methods = class1.getDeclaredMethods();//获取class对象的所有声明方法
    Method[] allMethods = class1.getMethods();//获取class对象的所有方法 包括父类的方法
     
    ---
    
    Class parentClass = class1.getSuperclass();//获取class对象的父类
    Class<?>[] interfaceClasses = class1.getInterfaces();//获取class对象的所有接口
     
    ---
    
    Constructor<?>[] allConstructors = class1.getDeclaredConstructors();//获取class对象的所有声明构造函数
    Constructor<?>[] publicConstructors = class1.getConstructors();//获取class对象public构造函数
    try {
        Constructor<?> constructor = class1.getDeclaredConstructor(new Class[]{String.class});//获取指定声明构造函数
        Constructor publicConstructor = class1.getConstructor(new Class[]{});//获取指定声明的public构造函数
    } catch (NoSuchMethodException e) {
        e.printStackTrace();
    }
     
    ---
    
    Annotation[] annotations = class1.getAnnotations();//获取class对象的所有注解
    Annotation annotation = class1.getAnnotation(Deprecated.class);//获取class对象指定注解
 
    ---
    
    Type genericSuperclass = class1.getGenericSuperclass();//获取class对象的直接超类的 Type
    Type[] interfaceTypes = class1.getGenericInterfaces();//获取class对象的所有接口的type集合
    
    
## 4.class对象动态生成

    -- 第一种方式 Class对象调用newInstance()方法生成
    Object obj = class1.newInstance();
    
    --第二种方式 对象获得对应的Constructor对象，再通过该Constructor对象的newInstance()方法生成
    Constructor<?> constructor = class1.getDeclaredConstructor(new Class[]{String.class});//获取指定声明构造函数
    obj = constructor.newInstance(new Object[]{"lcj"});
    
## 5.动态调用函数

    try {
        // 生成新的对象：用newInstance()方法
        
        Object obj = class1.newInstance();
        //判断该对象是否是Person的子类
        boolean isInstanceOf = obj instanceof Person;
        
        //首先需要获得与该方法对应的Method对象
        Method method = class1.getDeclaredMethod("setAge", new Class[]{int.class});
        
        //调用指定的函数并传递参数
        method.invoke(obj, 28);
        method = class1.getDeclaredMethod("getAge");
        Object result = method.invoke(obj, new Class[]{});
    } catch (InstantiationException e) {
        e.printStackTrace();
    } catch (IllegalAccessException e) {
        e.printStackTrace();
    } catch (NoSuchMethodException e) {
        e.printStackTrace();
    } catch (InvocationTargetException e) {
        e.printStackTrace();
    }
    
    
## 6.获取泛型类型

    //People类
    public class People<T> {}
    //Person类继承People类
    public class Person<T> extends People<String> implements PersonInterface<Integer> {}
    //PersonInterface接口
    public interface PersonInterface<T> {}

    Person<String> person = new Person<>();
    //第一种方式 通过对象getClass方法
    Class<?> class1 = person.getClass();
    Type genericSuperclass = class1.getGenericSuperclass();//获取class对象的直接超类的 Type
    Type[] interfaceTypes = class1.getGenericInterfaces();//获取class对象的所有接口的Type集合
    
    getComponentType(genericSuperclass);
    getComponentType(interfaceTypes[0]);
    
    private Class<?> getComponentType(Type type) {
        Class<?> componentType = null;
        if (type instanceof ParameterizedType) {
            //getActualTypeArguments()返回表示此类型实际类型参数的 Type 对象的数组。
            Type[] actualTypeArguments = ((ParameterizedType) type).getActualTypeArguments();
            if (actualTypeArguments != null && actualTypeArguments.length > 0) {
            componentType = (Class<?>) actualTypeArguments[0];
            }
        } else if (type instanceof GenericArrayType) {
            // 表示一种元素类型是参数化类型或者类型变量的数组类型
            componentType = (Class<?>) ((GenericArrayType) type).getGenericComponentType();
        } else {
            componentType = (Class<?>) type;
        }
        return componentType;
    }


# Type类

##  1.Type的直接子接口

- ParameterizedType： 表示一种参数化的类型，比如Collection，即普通的泛型。 

        public interface ParameterizedType extends Type {
            //1.获得<>中实际类型
            Type[] getActualTypeArguments();
            //2.获得<>前面实际类型
            Type getRawType();
            //3.如果这个类型是某个类型所属，获得这个所有者类型，否则返回null
            Type getOwnerType();
        }


- TypeVariable：是各种类型变量的公共父接口，就是泛型里面的类似T、E。

        public interface TypeVariable<D extends GenericDeclaration> extends Type {
           //获得泛型的上限，若未明确声明上边界则默认为Object
            Type[] getBounds();
            //获取声明该类型变量实体(即获得类、方法或构造器名)
            D getGenericDeclaration();
            //获得名称，即K、V、E之类名称
            String getName();
        }



- GenericArrayType：表示一种元素类型是参数化类型或者类型变量的数组类型，比如List<>[]，T[]这种。 

        public interface GenericArrayType extends Type {
            //获得这个数组元素类型，即获得：A<T>（A<T>[]）或T（T[]）
            Type getGenericComponentType();
        }


- WildcardType：代表一种通配符类型表达式，类似? super T这样的通配符表达式。
 
        public interface WildcardType extends Type {
            //获得泛型表达式上界（上限）
            Type[] getUpperBounds();
            //获得泛型表达式下界（下限）
            Type[] getLowerBounds();
        }


## 2.Type的直接子类

- Type的直接子类只有一个，也就是Class，代表着类型中的原始类型以及基本类型。