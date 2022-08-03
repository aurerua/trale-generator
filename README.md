# TRALE Generator

I wrote `generator.pl` for surface realization in HPSG using [TRALE](http://milca.sfs.uni-tuebingen.de/A4/Course/trale/) while working at [the University of Bremen](https://www.uni-bremen.de/).

DISCLAIMER: This code has been written in 2006. It could be run with the latest TRALE version at that time.

## PREREQUISITES

These can be given in `theory.pl`:
  
```prolog
syntactic_object(<type being the mgsat of all syntactic objects>).

ind_path([path,to,index,of,the,mrs]).

cont_path([path,to,mrs]).
liszt_path([path,to,list,of,EPs]).
hcons_path([path,to,handle,constraints]).
```  

Here is what I use. There are chances that it is similar to that:
```prolog
syntactic_object(syntactic_object).

ind_path([synsem,loc,cont,ind]).
cont_path([synsem,loc,cont]).
liszt_path([synsem,loc,cont,rels]).
hcons_path([synsem,loc,cont,h_cons]).
```

## LOADING
Either launch trale with the option `-e "compile('/path/to/generator/directory/generator.p')"` or call this within trale `prolog compile('/path/to/generator/directory/generator.pl').`

## USAGE
- To parse a sentence and generate from its semantics: 

```prolog
p_and_g([sentence,as,list,of,words]).
```

- To parse a phrase of description Desc and generate from its semantics:

```prolog
p_and_g([sentence,as,list,of,words],Desc).
```

- To generate from a description Desc (representing the whole sign, not only the semantics), getting all the outputs in the list Words, and matching Desc to the description Root:
```prolog
g(Desc).
g(Desc,Words).
g_d(Desc,Words,Root).
g_d(Desc,Root).
```

## TEST SUITE HANDLING

- The test suite handling for generation parses a sentence and tries then to generate from its semantics.

- Example lines to be added to `test_items.pl` for generation test suite handling:

```prolog
tg(6,[pierre,donne,une,fée,à`,clochette],@root).
tg(6,"Pierre donne une fée à clochette.",@root).
```

- To test the sentence numbered No in `test_items.pl`:

```prolog
testg(No).
```

To test all sentences in `test_items.pl`:

```prolog
testg(No).
```

- These can also be used to write the test results in file File:


```prolog
testgw(No,File).
testgw(all,File).
```

If File exists, its content is replaced by the output, so in order to append the test results to file File instead of overwriting its content, this should be used:

```
testga(No,File).
testga(all,File).
``` 


## KNOWN BUGS:

At the time of development (February 2006):

- There is a bug in TRALE that leads to failing calls to `alec_rule/6` for some grammars. In that case the generator would not work.
- Indices get mixed sometimes in a way that subject and complements of a verb
get interchanged leading to more output than it should give.
- Does not work yet for difference-lists of EPs in `liszt_path(...)` and `hcons_path(...)`.
