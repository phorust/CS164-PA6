CYK graph to parse tree
---
* root is e(o, N, S)
* edges that caused insertion of e(i,j,T) are children
![A CYK graph with the resulting parse tree overlaid](parsing_img/CYK_graph_to_tree.png "Converting a CYK graph to a parse tree")



Ambiguous Grammars
---
A grammar:

* defines parse trees, defines syntactically legal programs
* associativity, precedence

An ambiguous grammar: <!-- .element: class="fragment" data-fragment-index="1" -->

* defines multiple trees from single input string <!-- .element: class="fragment" data-fragment-index="1" -->

Fix grammar or have parser pick. <!-- .element: class="fragment" data-fragment-index="1" -->
Ambiguous grammars are bad! <!-- .element: class="fragment" data-fragment-index="1" -->

* ill-defined functionality <!-- .element: class="fragment" data-fragment-index="1" -->
* common in programming languages <!-- .element: class="fragment" data-fragment-index="1" -->

Note: remove the lines between the text and list and the markdown output should still be the same, but it isnt.



Removing ambiguity
---
* declare precedence/associativity, or
* rewrite the grammar

```
precedence: + or * first?
associativity: (a-b)-c or a-(b-c)
```



Disambuguity Declarations
---
```
%left + -
%left * /
```
* `*` and `/` have higher precedence
* `+ - * /` all have left associativity

when OPERATORS have LOWER precedence (high on the actual list), they are HIGHER in the tree.

<br>

In the parser
---
* pick operation with <i>least</i> precedence
* pick tree with largest left subtree



Dangling Else problem
---
`%left` and `%right` only work for binary operators
```
"if e1 then if e2 then e3 then e4" becomes
if e1 then {if e2 then e3} else e4 or
if e1 then {if e2 then e3 else e4}
```
use `%dprec n`
```
E -> if E then E		%dprec 2
 | if E then E else E	%dprec 1
produces
if e1 then {if e2 then e3 else e4}
```
When DPREC is HIGHER, productions are HIGHER in the tree.
<small>here if-then is higher in the tree than if-then-else since it has higher dprec</small>