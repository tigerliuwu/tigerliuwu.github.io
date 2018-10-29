---
layout: post
title:  "RCP - Field Type"
categories: [rcp]
tags: [rcp]
author: Wu Liu
---

* content
{:toc}
主要讲述字段类型的定义以及在Viewer中是如何展示





# 不同字段类型的定义
In the class:<b>EParameterFieldType</b>, define all the field types which used in the nodes.
```
    TEXT,
    MEMO_SQL,
    MEMO_PERL,
    CLOSED_LIST,
    CHECK,
    MEMO,
    SCHEMA_TYPE,
    PROPERTY_TYPE,
    EXTERNAL,
    FILE,
    VERSION,
    TABLE,
    DIRECTORY,
    PROCESS_TYPE,
    IMAGE
```

参数的类型都在这里进行声明，如果需要添加新的类型，也是在此进行添加。

使用例子：

```
FIELD="TEXT"
```

# 类型展示

The above field type will be displayed in the widget in the following method.
```
DynamicTabbedPropertySection.addComponents() {
......
	switch (listParam.get(i).getField()) {
	case EXTERNAL:
	    lastControl = addExternal(composite, listParam.get(i), numInRow, nbInRow, heightSize, lastControl);
	    break;
	case SCHEMA_TYPE:
	    lastControl = addSchemaType(composite, listParam.get(i), numInRow, nbInRow, heightSize, lastControl);
	    break;
	case TEXT:
	    lastControl = addText(composite, listParam.get(i), numInRow, nbInRow, heightSize, lastControl);
	    break;
	case CLOSED_LIST:
	    lastControl = addCombo(composite, listParam.get(i), numInRow, nbInRow, heightSize, lastControl);
	    break;
......
}

```


# 参考
 - [hive 动态分区](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)
