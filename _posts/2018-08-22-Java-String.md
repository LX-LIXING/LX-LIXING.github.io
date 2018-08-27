---
layout: post
title:  "String 源码学习"
date:   2018-08-22
categories: Java源码
tags: 源码
excerpt: 对String类进行学习
---

---

* 文档结构
{:toc}



---

2018-08-22

---

# String 源码

## 主要成员变量

```

	//将字符串存储在字符数组中
	private final char value[];
	//存储字符串的hash值
	private int hash; 

```

## 构造函数

### 无参

* public String() 

```

	public String() {
        this.value = "".value;
    }


	空餐构造函数是讲空字符串复制给成员变量 value

```

### 参数为String类型

* public String(String original)

```

	public String(String original) {
        this.value = original.value;
        this.hash = original.hash;
    }

	构造函数参数为一个字符串时，是将参数成员变量复制给新的字符串的成员变量

```

### 参数为char[]类型

* public String(char value[]) 

```

	public String(char value[]) {
        this.value = Arrays.copyOf(value, value.length);
    }

	public static char[] copyOf(char[] original, int newLength) {
        char[] copy = new char[newLength];
        System.arraycopy(original, 0, copy, 0,
                         Math.min(original.length, newLength));
        return copy;
    }

	构造函数参数为字符数组时，将参数值复制给一个新的字符数组，最后复制给字符串的成员变量	value
```

* public String(char value[], int offset, int count)

```

	public String(char value[], int offset, int count) {
        if (offset < 0) {
            throw new StringIndexOutOfBoundsException(offset);
        }
        if (count <= 0) {
            if (count < 0) {
                throw new StringIndexOutOfBoundsException(count);
            }
            if (offset <= value.length) {
                this.value = "".value;
                return;
            }
        }
        
        if (offset > value.length - count) {
            throw new StringIndexOutOfBoundsException(offset + count);
        }
        this.value = Arrays.copyOfRange(value, offset, offset+count);
    }

	public static char[] copyOfRange(char[] original, int from, int to) {
        int newLength = to - from;
        if (newLength < 0)
            throw new IllegalArgumentException(from + " > " + to);
        char[] copy = new char[newLength];
        System.arraycopy(original, from, copy, 0,
                         Math.min(original.length - from, newLength));
        return copy;
    }

	此构造函数首先检查数组下表是否越界，如果下标不越界，生成一个新的数组，并赋值给成员变量
```

### 参数为int[]类型

* public String(int[] codePoints, int offset, int count)

```

	public String(int[] codePoints, int offset, int count) {
		//一、检查数组下标是否越界
        if (offset < 0) {
            throw new StringIndexOutOfBoundsException(offset);
        }
        if (count <= 0) {
            if (count < 0) {
                throw new StringIndexOutOfBoundsException(count);
            }
            if (offset <= codePoints.length) {
                this.value = "".value;
                return;
            }
        }
        
        if (offset > codePoints.length - count) {
            throw new StringIndexOutOfBoundsException(offset + count);
        }

        final int end = offset + count;

        // 精确的计算字符数组的大小
        int n = count;
        for (int i = offset; i < end; i++) {
            int c = codePoints[i];
			//判断codePoint是否是基本字符，是的话一个char可以放下，codePoint包含基本字符和增补字符时，需要两个char的空间存放codePoint，否则抛出不合法的参数异常
            if (Character.isBmpCodePoint(c))
                continue;
            else if (Character.isValidCodePoint(c))
                n++;
            else throw new IllegalArgumentException(Integer.toString(c));
        }

        // 定义char类型数组，将codePoints数组内容转换为char数组
        final char[] v = new char[n];

        for (int i = offset, j = 0; i < end; i++, j++) {
            int c = codePoints[i];
            if (Character.isBmpCodePoint(c))
                v[j] = (char)c;
            else
                Character.toSurrogates(c, v, j++);
        }

		//将char[]赋值给成员变量value[]
        this.value = v;
    }

	char：Java中，char类型为16个二进制位，原本用于表示一个字符。但后来发现，16位已经不够表示所有的字符，所以后来发展出了代码点表示字符的方法。

	代码点(code point)：是指编码字符集中，字符所对应的数字。有效范围从U+0000到U+10FFFF。其中U+0000到U+FFFF为基本字符，U+10000到U+10FFFF为增补字符。

	代码单元(code unit)：对代码点进行编码得到的1或2个16位序列。其中基本字符的代码点直接用一个相同值的代码单元表示，增补字符的代码点用两个代码单元的进行编码，这个范围内没有数字用于表示字符，因此程序可以识别出当前字符是单单元的基本字符，还是双单元的增补字符。

```

### 参数为byte[]类型
	
* public String(byte bytes[], int offset, int length, String charsetName)

* public String(byte bytes[], int offset, int length, Charset charset)


* String(byte bytes[], String charsetName)

```

	String(byte bytes[], String charsetName)
            throws UnsupportedEncodingException {
        this(bytes, 0, bytes.length, charsetName);
    }
```

* public String(byte bytes[], Charset charset) 

```

	public String(byte bytes[], Charset charset)  {
        this(bytes, 0, bytes.length, charset);
    }
```

* String(byte bytes[], int offset, int length)

```

	public String(byte bytes[], int offset, int length) {
        checkBounds(bytes, offset, length);
        this.value = StringCoding.decode(bytes, offset, length);
    }

```

* public String(byte bytes[])

```

	public String(byte bytes[]) {
        this(bytes, 0, bytes.length);
    }
```

	以某种方式编码转换成字符串，后续详细介绍

## 成员函数



