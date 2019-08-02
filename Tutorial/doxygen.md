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

$$
\begin{dot2tex}[styleonly,codeonly,neato]
digraph G {
d2ttikzedgelabels = true;
node [style="state"];
edge [lblstyle="auto",topath="bend left"];
A [style="state, initial"];
A -> B [label=2];
A -> D [label=7];
B -> A [label=1];
B -> B [label=3,topath="loop above"];
B -> C [label=4];
C -> F [label=5];
F -> B [label=8];
F -> D [label=7];
D -> E [label=2];
E -> A [label="1,6"];
F [style="state,accepting"];
}
\end{dot2tex}
$$ 

# References

#. https://caiorss.github.io/C-Cpp-Notes/Doxygen-documentation.html

#. http://www.doxygen.nl/manual/preprocessing.html
