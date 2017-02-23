# Ax and Nock: Cellular Automatons

### "A universal wave that erases the self-portrait of man drawn in sand,
### inhumanism is a vector of revision." - Reza Negarestani

## Ax
``` 
 0   A noun is either an atom or a cell. An atom is any natural number.   atom/noun|      [1,2,3,4,5,6,7,8,9,10,11..]

 1   A cell is an ordered pair of two nouns.                                   cell|      [1 2] [0 0] [1 1] [3 4] [123 9800923]

 2   n refers to any atom. a, b, c, and d refer to nouns.                         n|      (1..123) (23213245..999999999)

 3  `Ξ` means to perform a rewrite as defined by this specification.              Ξ|      Ξ [0] →  Ξ [0] | Ξ [a 0 n] → n

 4  `→`  shows the steps of such a reduction. All must be completed.              →|      Ξ [a 1 0 n]  → n + 1 

 5  `?`  means the reduction is undefined.                                        ?|      Ξ [a 2 0] →  ?

 6  `:=` indicates a noun is the referent of a symbol.                           :=|      ~(1..256) := *    

 7  `+`  refers to the operation on the natural numbers, whose identity is 0.     +|      Ξ [a 1 0 n] → n + 1 

 8  `~`  requires that n so defined be in the range (n1..n2), inclusive.          ~|      ~(1..100) | ~(8..69)

 9   [a b c] → [a [b c]]

 10  Symbols have no other semantics.                                                     *u*  ^v^ 'w'

 11  The lemmas are reduced in ordinary arithmetic; c and d refer to atoms.               Ξ [a 13 b]  →  Ξ [a b]  →  [c d]  →  c + d 
```
 
## Nock
Remember, Hoon, compiles itself to Nock, so there may be cross-terminology in this 
section. Also Nock is different than Ax, however, Ax helps understand Nock, 
conceptually, syntax not as much so. The following should be considered a 
preamble on operators in Nock.

```
 ?[x] is 0 if x is cell, 1 if atom
 +[x] is x plus 1
 =[x y] is 0 if x and y are the same noun, otherwise 1
 / is (slot).Slot is a |tree| addressing operator. Anything to the left of the statement inside a slot is n, right is 2n+1
                         1
                     2       3
                   4   5   6   7
                             14 15 
                            . . . . 

?[a b]           0                                        0 is a cell, (paired nouns) this is a bottom in formal logic. ? is defined
?a               1                                        1 is a, which is an atom                                        
+[a b]           +[a b]                                   attempting to increment a cell crashes, infinite loop
+a               1 + a                                    +a is 1 + a is 2
=[a a]           0                                        =[a a] same noun, so 0
=[a b]           1                                        =[a b] not same, so 1
=a               =a                                       crash, infinite loop

/[1 a]           a                                        a tree always starts with 1
/[2 a b]         a                                        then 2
/[3 a b]         b                                        then 3
/[(a + a) b]     /[2 /[a b]]                              if slot even take left, divide a by two, keep slotting
/[(a + a + 1) b] /[3 /[a b]]                              if odd take right, subtract one, divide by two
/a               /a                                       crash, infinite loop

*[a [b c] d]     [*[a b c] *[a d]]

*[a 0 b]         /[b a]
*[a 1 b]         b
*[a 2 b c]       *[*[a b] *[a c]]
*[a 3 b]         ?*[a b]
*[a 4 b]         +*[a b]
*[a 5 b]         =*[a b]

*[a 6 b c d]     *[a 2 [0 1] 2 [1 c d] [1 0]
                   2 [1 2 3] [1 0] 4 4 b]
*[a 7 b c]       *[a 2 b 1 c]
*[a 8 b c]       *[a 7 [[7 [0 1] b] 0 1] c]
*[a 9 b c]       *[a 7 c 2 [0 1] 0 b]
*[a 10 [b c] d]  *[a 8 c 7 [0 3] d]
*[a 10 b c]      *[a c]

*a               *a
```
