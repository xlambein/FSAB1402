Séance 4 : Higher-order programming & Enregistrements
==================================================

Exercices
---------

### 2. Listes, tuples, enregistrements
Voici un ensemble d'enregistrements. Pour chacune, indiquez s'il s'agit d'une liste, d'un tuple ou d'un enregistrement. Etant donné qu'une liste est un cas particulier de tuple, et qu'un tuple est un cas particulier d'enregistrement, donnez le type le plus précis.

```oz
'|'(a b)
'|'(a '|'(b nil))
'|'(2:nil a)
state(1 a 2)
state(1 3:2 2:a)
tree(v:a T1 T2)
```

Vous pouvez vérifier vos réponses en utilisant les fonctions IsList et IsTuple. {IsList X} renvoie true si X est une liste, false sinon. IsTuple fonctionne de manière similaire avec les tuples.

#### Réponse

```oz
'|'(a b)          % Tuple
'|'(a '|'(b nil)) % Liste
'|'(2:nil a)      % Liste
state(1 a 2)      % Tuple
state(1 3:2 2:a)  % Tuple
tree(v:a T1 T2)   % Enregistrement
```

#### Tests

```oz
% Necessary for last test
declare T1 T2

{Browse {And {IsList '|'(a b)} == false {IsTuple '|'(a b)} == true}}
{Browse {IsList '|'(a '|'(b nil))} == true}
{Browse {IsList '|'(2:nil a)} == true}
{Browse {And {IsList state(1 a 2)} == false {IsTuple state(1 a 2)} == true}}
{Browse {And {IsList state(1 3:2 2:a)} == false {IsTuple state(1 3:2 2:a)} == true}}
{Browse {And {IsList tree(v:a T1 T2)} == false {IsTuple tree(v:a T1 T2)} == false}}
```


### 3. Zig zag folding
Ecrivez une fonction `FoldZ` qui prend en argument :

- une fonction `F` ;
- une liste `L` (`= [e_1 e_2 ... e_(n-1) e_n]`) ;
- une valeur quelconque `V` ;

et renvoie `{F e_(⌈n/2⌉) {... {F e_(n-1) {F e_2 {F e_n {F e_1 V}}}}...}}` où ⌈x⌉ correspond à x arrondit à l'entier supérieur.

#### Réponse

*Note: beaucoup de réponses différentes sont possibles. Celle-ci en particulier est en `O(n)`.*

```oz
fun {FoldZ F L V}
    local
        fun {InvertList L1 L2}
            case L1
            of H|T then {InvertList T H|L2}
            [] nil then L2
            end
        end

        fun {FoldHelper F L1 L2 V I}
            if I > 0 then
                {FoldHelper F L2 L1.2 {F L1.1 V} I-1}
            else
                V
            end
        end
    in
        {FoldHelper F L {InvertList L nil} V {List.length L}}
    end
end
```

#### Tests

```oz
declare
fun {MakeList H T}
   '|'(H T)
end

{Browse {FoldZ MakeList [1 2 3 4 5] nil} == [3 4 2 5 1]}
{Browse {FoldZ MakeList [1 2 3 4 5 6] nil} == [4 3 5 2 6 1]}
```


### 4. Fibonacci generator
Ecrivez une fonction F qui renvoie un tuple `fib(1 F1)` tel que

- `{F1}` renvoie un tuple `fib(1 F2)` tel que ...
- `{F2}` renvoie un tuple `fib(2 F3)` tel que ...
- `{F3}` renvoie un tuple `fib(3 F4)` tel que ...
- `{F4}` renvoie un tuple `fib(5 F5)` tel que ...
- `{F5}` renvoie un tuple `fib(8 F6)` tel que ...
- etc.

Autrement dit, les premiers éléments des tuples forment la suite de fibonacci, tandis que le second élément d'un tuple est un fonction qui permet de générer le tuple suivant de la séquence.

#### Réponse

```oz
fun {F}
    local
        fun {FibHelper A B}
            fun {$}
                fib(A+B {FibHelper B A+B})
            end
        end
    in
        fib(1 {FibHelper 0 1})
    end
end
```

#### Tests

```oz
declare
F1 = {F}
F2 = {F1.2}
F3 = {F2.2}
F4 = {F3.2}
F5 = {F4.2}
F6 = {F5.2}

{Browse F1.1 == 1}
{Browse F2.1 == 1}
{Browse F3.1 == 2}
{Browse F4.1 == 3}
{Browse F5.1 == 5}
{Browse F6.1 == 8}
```
