---

title: Doxygen文档生成

date: 2019-07-31 18:53:08
tags: [How]
categories: [Tutorial]

---

<!-- vim-markdown-toc GFM -->

* [Tags](#tags)
* [References](#references)

<!-- vim-markdown-toc -->

<!-- more -->

# Tags

|Tag | Description |
|--- | ------ |
| @brief |   Brief description of class or function (fits a single line)
| @details |   about class or function
| @author <AUTHOR NAME> | Insert author name
| @param <PARAM> <DESCR> |  Function or method parameter description
| @param[in] <PARAM> <DESCR>  | Input parameter (C-function)
| @param[out] <PARAM> <DESCR> |  Output paramter of C-style function that returns multiple values
| @param[in, out] <PARAM> <DESCR> |  Parameter used for both input and output in a C-style function.
| @tparam <PARAM> <DESCR> |  Template type parameter
| @trhow <EXCEP-DESCR>  | Specify exceptions that a function can throw
| @pre <DESCR>  | Pre conditions
| @post <DESCR>  | Post conditions
| @return <DESCR>  | Description of return value or type.
| @code … <C++-Code>… @encode  | C++ code example.
| @remark  | Additional side-notes
| @note  | Insert additional note
| @warning |   
| @see SomeClass::Method |  Reference to some class, method, or web site
| @li  | Bullet point

# References

#. https://caiorss.github.io/C-Cpp-Notes/Doxygen-documentation.html

#. http://www.doxygen.nl/manual/preprocessing.html
