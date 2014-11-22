title=catch-exception library has new home
date=2014-11-08
type=post
tags=blog, catch-exception, java, assertions
status=published
~~~~~~

Rod Woo, the former author of [catch-exception](https://code.google.com/p/catch-exception/) library decided to not
maintenance his project. This is related to Java 8’s lambda expressions, which highly simplified exception handling.

> +++ 2014-05-02: Java 8's lambda expressions will make catch-exception redundant. Therefore, this project won't be maintained any longer +++

Because many projects and companies are still using Java 7, we wanted to keep this project up to date, and
 fix some problems, like compatibility with AssertJ 1.7.0 and Mockito 1.10.0.
We only wanted to maintain catch-exception and fix existing problems. Supported version was reborn at new home
[https://github.com/Codearte/catch-exception](https://github.com/Codearte/catch-exception)! We hope that the project will be as good as the original version.

Remember that for new projects based on Java 8, you can use lambda expression instead of this library. There is a good
article about testing exception with Java8: [jUnit: Testing exception with java 8 and lambda expressions](http://blog.codeleak.pl/2014/07/junit-testing-exception-with-java-8-and-lambda-expressions.html) by [Rafał Bobrowiec](http://blog.codeleak.pl/p/author.html)

Changes
---

* New project home [github.com/Codearte/catch-exception](https://github.com/Codearte/catch-exception)

* [FEST Fluent Assertions](https://github.com/alexruiz/fest-assert-2.x) was replaced by [AssertJ](http://joel-costigliola.github.io/assertj/). [FEST Fluent Assertions](https://github.com/alexruiz/fest-assert-2.x) is dead project.

* New class [`BDDCatchException`](http://codearte.github.io/catch-exception/apidocs/com/googlecode/catchexception/apis/BDDCatchException.html)
 was introduced for BDD style exception handling. Naming convention is consistent with [`BDDAssertions`](http://joel-costigliola.github.io/assertj/core/api/org/assertj/core/api/BDDAssertions.html)
  and [`BDDMockito`](http://mockito.github.io/mockito/docs/current/org/mockito/BDDMockito.html).

* Class [`CatchExceptionAssertJ`](http://codearte.github.io/catch-exception/apidocs/com/googlecode/catchexception/apis/CatchExceptionAssertJ.html)
was declared deprecated, use [`BDDCatchException`](http://codearte.github.io/catch-exception/apidocs/com/googlecode/catchexception/apis/CatchExceptionBdd.html) instead.

* Class [`CatchExceptionBdd`](http://codearte.github.io/catch-exception/apidocs/com/googlecode/catchexception/apis/CatchExceptionBdd.html) was deprecated, because [FEST Fluent Assertions](https://github.com/alexruiz/fest-assert-2.x) is dead.

* Added support for AssertJ 1.7.0

* Added support for Mockito versions from 1.8.1 to 1.10.8

* [Discussion group](http://groups.google.com/group/catch-exception-discuss) and [old issues page](http://code.google.com/p/catch-exception/issues/list) was replaced by new [Github Issues Page](https://github.com/Codearte/catch-exception/issues)

* Maven coordinates was changed from [com.googlecode.catch-exception:catch-exception](https://code.google.com/p/catch-exception/wiki/Installation) to [eu.codearte.catch-exception:catch-exception](https://maven-badges.herokuapp.com/maven-central/eu.codearte.catch-exception/catch-exception)

* Releasing process was simplified and automated.