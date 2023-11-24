# Лабораторна робота 5 з системного програмування

## Cинтаксичний аналізатор

На базі Lex/YACC (або аналога) розробити синтаксичний
аналізатор (з генерацією коду або обчисленнями - опціонально) для С або іншої
сучасної імперативної (ООП, процедурної, тощо) мови програмування, або для арифметичних виразів (спрощений варіант).

## Приклади коду задані у файлах input.c, inputAlt.c

Для запуску програми треба перейти в кореневу папку проекту та запустити у даному порядку команди

flex program.l
bison -yd program.y
gcc -lm -w y.tab.c
a < input.c

## Програма робить

1) Лексичний аналіз
2) Синтаксичний аналіз
3) Семантичний аналіз
4) Генерацію проміжного коду

## Приклад:

Програма:
#include<stdio.h>
#include<string.h>

int main() {
    int a;
    int x=1;
    int y=2;
    int z=3;
    x=3;
    y=10;
    z=5;
    if(x>5) {
        for(int k=0; k<10; k++) {
            y = x+3;
            printf("Hello!");
        }
    } else {
        int idx = 1;
    }
    for(int i=0; i<10; i++) {
        printf("Hello World!");
        scanf("%d", &x);
        if (x>5) {
            printf("Hi");
        }
        for(int j=0; j<z; j++) {
            a=1;
        }
    } 
    return 1;
}

Результат обробки:

                                                                 PHASE 1: LEXICAL ANALYSIS


SYMBOL   DATATYPE   TYPE   LINE NUMBER
_______________________________________

#include<stdio.h>               Header  0
#include<string.h>              Header  1
main    int     Function        3
a       int     Variable        4
x       int     Variable        5
1       CONST   Constant        5
y       int     Variable        6
2       CONST   Constant        6
z       int     Variable        7
3       CONST   Constant        7
10      CONST   Constant        9
5       CONST   Constant        10
if      N/A     Keyword         11
for     N/A     Keyword         12
k       int     Variable        12
0       CONST   Constant        12
printf  N/A     Keyword         14
else    N/A     Keyword         16
idx     int     Variable        17
i       int     Variable        19
scanf   N/A     Keyword         21
j       int     Variable        25
return  N/A     Keyword         29


                                                                 PHASE 2: SYNTAX ANALYSIS



Inorder traversal of the Parse Tree is:

#include<stdio.h>, headers, #include<string.h>, program, a, declaration, NULL, statements, x, declaration, 1, statements, y, declaration, 2, statements, z, declaration, 3, statements, x, =, 3, statements, y, =, 10, statements, z, =, 5, statements, x, >, 5, if, k, declaration, 0, CONDITION, k, <, 10, CONDITION, k, ITERATOR, ++, for, y, =, x, +, 3, statements, printf, if-else, else, idx, declaration, 1, statements, i, declaration, 0, CONDITION, i, <, 10, CONDITION, i, ITERATOR, ++, for, printf, statements, scanf, statements, x, >, 5, if, printf, if-else, statements, j, declaration, 0, CONDITION, j, <, z, CONDITION, j, ITERATOR, ++, for, a, =, 1, main, return, RETURN, 1,



                                                                 PHASE 3: SEMANTIC ANALYSIS

Semantic analysis completed with no errors

                                                           PHASE 4: INTERMEDIATE CODE GENERATION

a = NULL
x = 1
y = 2
z = 3
x = 3
y = 10
z = 5

if (x > 5) GOTO L0 else GOTO L1

LABEL L0:
k = 0

LABEL L2:

if NOT (k < 10) GOTO L3
t1 = x + 3
y = t1
t1 = k + 1
k = t0
JUMP to L2

LABEL L3:

LABEL L1:
idx = 1
GOTO next
i = 0

LABEL L4:

if NOT (i < 10) GOTO L5

if (x > 5) GOTO L6 else GOTO L7

LABEL L6:

LABEL L7:
GOTO next
j = 0

LABEL L8:

if NOT (j < z) GOTO L9
a = 1
t4 = j + 1
j = t3
JUMP to L8

LABEL L9:
t4 = j + 1
j = t3
JUMP to L4

LABEL L5: