---
layout: post
title : Custom Tags
author: Mégane Vilain
category: JSP
---

## Definition

JSP tag extensions lets you create new tags that you can insert directly into a JavaServer Page

To create a custom JSP tag, you must first create a Java class that acts as a tag handler.

```java
public class HelloTag extends SimpleTagSupport {
   public void doTag() throws JspException, IOException {
      JspWriter out = getJspContext().getOut();
      out.println("Hello Custom Tag!");
   }
}
```

## TLD

If you want to redistribute your tag files or implement your custom tags with tag handlers written in Java, you must declare the tags in a tag library descriptor (TLD). A tag library descriptor is an XML document that contains information about a library as a whole and about each tag contained in the library. TLDs are used by a web container to validate the tags and by JSP page development tools.

**Tld**
```xml
<taglib>
   <tlib-version>1.0</tlib-version>
   <jsp-version>2.0</jsp-version>
   <short-name>Example TLD</short-name>
   
   <tag>
      <name>Hello</name>
      <tag-class>com.tutorialspoint.HelloTag</tag-class>
      <body-content>empty</body-content>
   </tag>
</taglib>
```

## Accessing the Tag Body

You can include a message in the body of the tag as you have seen with standard tags. Consider you want to define a custom tag named <ex:Hello> and you want to use it in the following fashion with a body

```jsp
<ex:Hello>
   This is message body
</ex:Hello>
```

**Java class**
```java
public class HelloTag extends SimpleTagSupport {
   StringWriter sw = new StringWriter();
   public void doTag()
   
   throws JspException, IOException {
      getJspBody().invoke(sw);
      getJspContext().getOut().println(sw.toString());
   }
}
```

**Tld**
```xml
<taglib>
   <tlib-version>1.0</tlib-version>
   <jsp-version>2.0</jsp-version>
   <short-name>Example TLD with Body</short-name>
   
   <tag>
      <name>Hello</name>
      <tag-class>com.tutorialspoint.HelloTag</tag-class>
      <body-content>scriptless</body-content>
   </tag>
</taglib>
```

**JSP**
```jsp
<%@ taglib prefix = "ex" uri = "WEB-INF/custom.tld"%>

<html>
   <head>
      <title>A sample custom tag</title>
   </head>
   
   <body>
      <ex:Hello>
         This is message body
      </ex:Hello>
   </body>
</html>
```

### Result

```
This is message body
```

## Custom Tag Attributes

You can use various attributes along with your custom tags. To accept an attribute value, a custom tag class needs to implement the setter methods

### Example

**Java class**
```java
public class HelloTag extends SimpleTagSupport {
   private String message;

   public void setMessage(String msg) {
      this.message = msg;
   }
   StringWriter sw = new StringWriter();
   public void doTag()
   
   throws JspException, IOException {
      if (message != null) {
         /* Use message from attribute */
         JspWriter out = getJspContext().getOut();
         out.println( message );
      } else {
         /* use message from the body */
         getJspBody().invoke(sw);
         getJspContext().getOut().println(sw.toString());
      }
   }
}
```

**Tld**
```xml
<taglib>
   <tlib-version>1.0</tlib-version>
   <jsp-version>2.0</jsp-version>
   <short-name>Example TLD with Body</short-name>
   
   <tag>
      <name>Hello</name>
      <tag-class>com.tutorialspoint.HelloTag</tag-class>
      <body-content>scriptless</body-content>
      
      <attribute>
         <name>message</name>
      </attribute>
   
   </tag>
</taglib>

```

### Taglib properties

|Property|Description|Required|
|---|---|---|
|**tlibversion**|Tag version number|Yes|
|**jspversion**|Jsp version number|No|
|**shortname**|Tags prefix|Yes|
|**uri**|Uri of the taglibrary used in jsp pages|No|
|**info**|Tag description|No|

### Tag properties

|Property|Description|Required|
|---|---|---|
|**name**|The name of the tag|Yes
|**tag-class**|Java class complete name (With pacakge)|Yes|
|**body-content**|Type of content (empty, JSP, tagdependant)|No|
|**info**|Description of the tag|No|

### Attribute properties

|Property|Description|Required|
|---|---|---|
|**name**|The name element defines the name of an attribute. Each attribute name must be unique for a particular tag.|Yes|
|**required**|This specifies if this attribute is required or is an optional one. It would be false for optional.|No|
|**rtexprvalue**|Declares if a runtime expression value for a tag attribute is valid (Ex: El values)|No|
|**type**|Defines the Java class-type of this attribute. By default it is assumed as String|No|
|**description**|Informational description can be provided.|No|
|**fragment**|Declares if this attribute value should be treated as a JspFragment.|No|


**JSP**
```jsp
<%@ taglib prefix = "ex" uri = "WEB-INF/custom.tld"%>

<html>
   <head>
      <title>A sample custom tag</title>
   </head>
   
   <body>
      <ex:Hello message = "This is custom tag" />
   </body>
</html>
```


### Result


```
This is a custom tag
```

