# 文档

如何为项目写文档、用什么写文档、如何方便的管理/更新文档，这是很多技术人员都要经常面对的一些问题。根据个人经验，程序开发类工作中我们涉及到的文档可能主要包括下面几类，
* 函数/类的接口说明文档， 比如 XXX Framework API Documentation
* 服务端接口说明文档，比如 Web Service API, Restful API 等
* 使用指南、手册等说明文档，比如 Getting Start XXX, XXX Tutorial, XXX Guideline, XXX Manual 等
* 设计相关文档，比如 数据库 ER 图、类图、时序图等（Enterprise Architect）

## 相关工具/平台

下面，我们将分别介绍一些常用的相关工具/平台

### 使用指南、手册等说明文档

此类文档内容多以大段文字说明为主，一般按章节进行内容组织，随着项目的不断成熟、演进，很多成熟项目的文档都会有相当篇幅；于此同时，出于格式、排版等需求，逐步形成了多种标记语言标准，常见的比如 TeX, AsciiDoc, DocBook, HTML, Markdown, reStructuredText 等。
扩展阅读：
* [Comparison of document markup languages](https://en.wikipedia.org/wiki/Comparison_of_document_markup_languages)
* [Lightweight Markup: Markdown, reStructuredText, MediaWiki, AsciiDoc, Org-mode](http://hyperpolyglot.org/lightweight-markup)

#### ReST（reStructuredText）
* [Sphinx Documentation](http://sphinx-doc.org/)，可能是 ReST（reStructuredText） 系的代表项目了吧，Python 开发，由于语法简单、功能强大，非常多的项目都使用 Sphinx 来生成项目文档（[感受下?](http://sphinx-doc.org/examples.html)），跟他一起出现的还有 [Read The Docs](https://readthedocs.org/)（开源文档生成的托管平台和社区，同时支持 reStructuredText 格式以及 Markdown 格式） 这位兄弟，关于他们之间的关系，可以[看下这里](https://coderwall.com/p/vemncg/what-is-the-difference-rest-docutils-sphinx-readthedocs)；相关 PHP 项目示例，
    * [Guzzle](http://guzzle3.readthedocs.org/)，[源码](https://github.com/guzzle/guzzle/tree/v3.8.1/docs)
    * [CakePHP Cookbook 3.x](http://book.cakephp.org/3.0/en/index.html)

### 函数/类的接口说明文档

* [PHPDoc](http://www.phpdoc.org/)
* [Doxygen](http://www.stack.nl/~dimitri/doxygen/)

### 服务端接口说明文档

* [Apidoc](http://apidocjs.com/), [Demo](http://apidocjs.com/example/) 
* [Slate](https://github.com/tripit/slate) - 创建好看的 API 文档，Travis-CI, Appium 等公司使用中，更多例子参考：[Examples of Slate in the Wild](https://github.com/tripit/slate#examples-of-slate-in-the-wild)
* [Apigen](http://www.apigen.org/)
* [Swagger](http://swagger.io/), [Demo](http://petstore.swagger.wordnik.com/)
* [JSON-Schema](http://json-schema.org/)

### 设计相关文档
