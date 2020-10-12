# FOMA
� ���� �������� ��� �������� ������������ � ��������������� �������� �� ������� FOMA - ����������� ��� �������� ��������������.

�� ����: https://fomafst.github.io/

## Getting started
��� ������ �������� �������� � �� �����.
### ���������:
��� ��������� �� Debian/Ubuntu ����� ��������������� ��������
~~~
apt instal foma
~~~ 
��� Windows � Mac ������ ��������� � �� �����.

### ������� �������
��� ������� ����������� ��������� ������������ �������: `regex REGULAR-EXPRESSION ;`
����� ����� ������ ����� ���������� ��� ������������ �������������: `def NAME REGEX ;` 
� ��� ����, ����� ������� ��� ����� ����������� ������� ��������� ���������� ������� `words`
������:
```
> def char a|b ;
> regex char char ;
> words
aa
ab
ba
bb
``` 
�����, ���� ���������� *GraphViz*, �� ����� ���������� ���� �������� �������� � ������� ������� `view`. 
������ ��� �������� ������ �� UNIX-�������� ��������. �� ������� ���� ����� ������������ ������� `net`, 
������� ������� ASCII �������:
```
> net
Sigma: a b
Size 2.
Net: 234E2BE34
Flags: deterministic pruned minimized epsilon_free loop_free
Arity: 1
Ss0: a -> s1, b -> s1.
s1: a -> fs2, b -> fs2.
fs2: (no arcs).
```  
������ �������� ��������� ����� ������������ �������� ���������������.
 ���������� ��������� �������� � ����������� ����� `:`, �����������, ��� ���� ���������������.
 ����� ������� ������������ ���� ����-����� ������������ ������� `pairs`, � ����� ������������� ����� ������������ 
������� `down` ��� `up` ��� �������������� � �������� �������. ������:
```
> regex a:b b:a ;
> pairs 
ab ba
> down 
down> ab
ba
> up 
up> ba
ab
``` 

### ���������
������� ��������� ������� ���������� �� ������ � ��� �� ����������� `re`. ���������� ������� �������:
- ��� ���������� ��������� ������������� �� `;`
- ��� ���������� ������������ ���������� ������ � ��� ���������� ������� ���� `[A-Z]`. ����������� ������� �������� �������.
- ������������ �������� ����������� � ������� �������: `a b (c)` ���� ��� ����� `ab` � `abc`.
- "����� ������" ����������� ������ �������, � �� ������
- ������ ����������� `|` ����� ���� ����������� `&` � ����������� `~`. ��������, ��� ������� ����, ������������ � ���������������
�� `a`, ����� ������������ ��������� `a ?* & ?* a`
- ���� ������� ���������� � ������� ��� ���� ���� �������� ���� `%`: `"_"` ��� `%_` 
- ������� ��� �������� ����������������: `:`, `.o.` � `->` 
- ������ ������ �������������� ��� `0`  
- ��������� ������� ����������� ��������, ����� �� ����� �������������� ��� ����������.

### ���������������
� ����������������� �� ��� ������� �������������: ����� ��������� ���� `:` � ���������� ��������� ��� ������. 
�� �������� ��� �� ������ ���������. ��� ������������� ���������� ����� ������������ ��� ���������� rewrite rules:
```
LHS -> RHS || LC _ RC
``` 
��� `LHS` - ����� ������� (��� �������������), `RHS` - ������ ������� (�� ��� �������������),
 `LC` - ����� ��������, `RC` - ������ ��������.
��������, ���������� ���������
```
regex a -> b || c _ d
```
������� ��� `a` �� `b` ������ ����� `c` � `d`. ����� � ������ ��������� ����� ���� �������������. 
����� ����� ������������ ���� ������ `.#.` ��� ����������� ����� ����. ��� ������� "�� ������" (����� `LHS` ������) ������������ 
������ `[..]`. ��� ����� ���������� ��������������� � ������� ����� ���������� `.0.`. ��������:
```
> regex a -> b .o. b -> c ;
> down
down> abc
ccc 
```

### ������� 1
�������� ���������������, ������� ������ ����� `cat` � `bus` � ������������ � 
������������� �����.
```
cat[Sg] -> cat
bus[Sg] -> bus
cat[Pl] -> cats
bus[Pl] -> buses
```
��������� ����� ����� � ���������� 3 � ���������:
  https://github.com/mhulden/foma/blob/master/foma/docs/simpleintro.md

### ����������

��������������� ����� ����� �������� � ���� ����������-��������� ���������.
 ���������� ������ ������ � ��������� ����� � ����������� `*.lexc`. 
� �������� ������� �������� ��� ����� ������ ������ ����� `Lexicon` �� ����������. 
��� ������ ����� ������ ��������, ��������� �� ���������� ��������.
```
Multichar_Symbols +Sg +Pl
```

�����  ����� ����������� ������ �������� ������� � ���� �� ����� ����������:
```
LEXICON ROOT

Noun ;
```
����� ��� �������� �������� � ����
```
LEXICON State

LHS1(:RHS1) NewState1;
LHS2(:RHS2) NewState2;
...
LHSn(:RHSn) NewStateN;
```

��� ������ ������� ��� ����� ��������� ���:
```
LEXICON Noun
cat Ninf;
bus Ninf;

LEXICON Ninf
+Sg:0 #;
+Pl:^s #;
```

����� �� ����� ��������� � ������������� ���������� � ������ �� ���:
```
read lexc lexicon.lexc
define Lexicon;
```
� ������ ������ ���������� � ������� ����������������� ��� � ������.

������� ����� ��������� � ����������� `*.foma` � ��������� � ������� �������
```
source grammar.foma
```

### ������� 2
�������� ��������������� ��� ������� ��������������� �������������� ����� ��� � �����������.
*�������� �� ���������� � FOMA: https://fomafst.github.io/morphtut.html*
### ������� 3*
�������� ��������������� ��� ������� � �������� ����������.