---
layout: post
title : JSP - FreeMarker
author: Mégane Vilain
category:  Programming
subcategory: Java
---

## Definition

Apache FreeMarker™ is a template engine: a Java library to generate text output (HTML web pages, e-mails, configuration files, source code, etc.) based on templates and changing data. Templates are written in the FreeMarker Template Language (FTL). Usually, a general-purpose programming language (like Java) is used to prepare the data. Then, Apache FreeMarker displays that prepared data using templates. In the template you are focusing on how to present the data, and outside the template you are focusing on what data to present.

![FreeMarker_Image](https://freemarker.apache.org/images/overview.png)


## Interpolations

FreeMarker will replace ${...} in the output with the actual value of the expression inside the curly brackets. 

## Comments

Comments are similar to HTML comments, but they are delimited by **<#--** and **-->**. Unlike HTML comments, FTL comments won't get into the output (won't be visible in the page source for the visitor), because FreeMarker skips them.

## Directives

### If Else

```ftl
<#if animals.python.price < animals.elephant.price>
  Pythons are cheaper than elephants today.
<#elseif animals.elephant.price < animals.python.price>
  Elephants are cheaper than pythons today.
<#else>
  Elephants and pythons cost the same today.
</#if>
```

### List

The generic form of the list directive is: <#list sequence as loopVariable>repeatThis</#list>. The repeatThis part will be repeated for each item in the sequence that you have specified with sequence, one after the other, starting from the first item. In all repetitions loopVariable will hold the value of the current item. This variable exists only between the <#list ...> and </#list> tags.


```ftl
<ul>
<#list misc.fruits as fruit>
  <li>${fruit}
</#list>
</ul>
```

A problem with the above example is that if we happen to have 0 fruits, it will still print an empty **< ul ></>** instead of just nothing.

```ftl
<#list misc.fruits>
  <ul>
    <#items as fruit>
      <li>${fruit}
    </#items>
  </ul>
</#list>
```

#### Sep

The section covered by sep will be only executed when there will be a next item. Hence there's no comma after the last fruit.

```ftl
<p>Fruits: <#list misc.fruits as fruit>${fruit}<#sep>, </#list>
```

#### Else

A list, just like an if, can have an else, which is executed if there were 0 list items

```ftl
<p>Fruits: <#list misc.fruits as fruit>${fruit}<#sep>, <#else>None</#list>
```

### Include

With the include directive you can insert the content of another file into the template

```ftl
<#include "/copyright_footer.html">
```

### t, lt, rt

* <#t> - Ignore all leading and trailing white-space in this line.
* <#lt> - Ignore all leading white-space in this line.
* <#rt> - Ignore all trailing white-space in this line.

### Built-ins

The so-called built-ins are like methods  that aren't coming from the data-model, but added by FreeMarker to the values. In order to make it clear where subvariables comes from, you have to use **?**  instead of **.**  to access them. 


|Function|Description|
|---|---|
|**upper_case**|Returns the upper case version of the value of the variable|
|**cap_first**|Returns the variable with its first letter converted to upper case|
|**length**|Returns the number of characters in the variable |
|**size**|Returns the number of items in a list|
|**join("separator")**|Returns the list to a string by concatenating items, and inserting the parameter separator between each items|
|**starts_with("value")**|Returns true if variable start with the value specified|
|**filter(it -> it.protected)**|Returns the list of protected animals|

#### Between list tag

|Function|Description|
|---|---|
|**index**|Returns the 0-based index of the item|
|**counter**|Returns the 1-based index of the item|
|**item_parity**|Returns **odd** or **even**  depending on the current counter parity|


### Missing variables

Wherever you refer to a variable, you can specify a default value for the case the variable is missing by following the variable name with a ! and the default value

```ftl
<h1>Welcome ${user!"visitor"}!</h1>
```

You can ask whether a variable isn't missing by putting ?? after its name

```ftl
<#if user??><h1>Welcome ${user}!</h1></#if>
```







