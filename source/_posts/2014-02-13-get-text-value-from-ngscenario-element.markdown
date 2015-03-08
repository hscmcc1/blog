---
author: hesicong
comments: true
date: 2014-02-13 12:14:36+00:00
layout: post
slug: get-text-value-from-ngscenario-element
title: 如何获得ngScenario中的element的text的值
wordpress_id: 3291
categories:
- 其他
---

在AngularJS的ngScenario测试中， 我们有时需要对返回的值做一些特殊的处理，然后再进行比较。

例如我们验证select的值为abcd

``` js
expect(element("select").text()).toBe("abcd")
```

但用jquery发现这个select的值为

```
\n   abc   \n
```

前后会多出一些空白字符。我们要做的是先对其进行trim操作，然后与abcd对比即可。

``` js
expect(_.str.trim(element("select").text())).toBe("abcd")
```

注意_.str.trim是Underscore.String提供的。

但这样是行不通的，_.str.trim函数接收到的是一个promise对象，所以里面没有text的值。

要正确的获得值，需要做一些改进。在[这里](http://stackoverflow.com/questions/20691139/why-is-the-text-function-returning-object-object-in-ngscenario-for-angularj)有一些讨论，可以参考：

``` js
var textPromise = element('select').query(function (nameElement, done) {
    var text = _.str.trim(nameElement.text()); // Can finally access this guy!

    // The first param null indicates a nominal execution, the second param is a return of sorts
    done(null, text);
  });

  // Passes
  expect(textPromise).toBe('abcd')
```

这样，通过query这个函数，即可获得真正的text的值了。

[这里](http://stackoverflow.com/questions/20691139/why-is-the-text-function-returning-object-object-in-ngscenario-for-angularj)还有一些别的例子，请参考原文。
