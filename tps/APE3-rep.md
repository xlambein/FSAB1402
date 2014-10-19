Séance 3 : Programmation récursive avec des listes
==================================================

Exercices
---------

### Exercice 1

```oz
L1=a|nil
L2=a|(b|c|nil)|d|nil
L3=proc {$} {Browse oui} end | proc {$} {Browse non} end |nil
L4=est|une|liste|nil
L5=(a|p|nil)|nil
```

```oz
L6=ceci|L4
```

```oz
{L3.1}
```

```oz
L7=L2.2
```

```oz
proc {TestPattern L}
    case L
    of M then {Browse 1}
    [] H|T then {Browse 2}
    [] nil then {Browse 3}
    end
end
```


### Exercice 2

```oz
% head(l): returns the head of list 'l'
% head([a1, ..., an]) = a1
fun {Head L}
    if L==nil then nil
    else L.1 end
end

% tail(l): returns the tail of list 'l'
% tail([a1, ..., an]) = an
fun {Tail L}
    case L
    of H|T then
        if T==nil then H
        else {Tail T} end
    else L end
end
```


### Exercice 3

```oz
% length(l): returns the number of elements in list 'l'
% length([a1, ..., an]) = n
fun {Length L}
    local
        fun {LengthHelper L A}
            case L
            of H|T then {LengthHelper L.2 A+1}
            else A end
        end
    in
        {LengthHelper L 0}
    end
end
```


### Exercice 5

```oz
fun {TestList L}
    case L
    of nil then {Browse empty}
    [] H|T then {Browse nonEmpty}
    else {Browse other}
    end
end
```

```oz
% Head, Tail, Length are already done

fun {Append L R}
    case L
    of nil then R
    [] H|T then H|{Append T R}
    end
end
```


### Exercice 6

```oz
fun {Take Xs N}
    if N =< 0 then nil
    else
        case Xs
        of H|T then H|{Take T N-1}
        [] nil then nil
        end
    end
end
```

```oz
fun {Drop Xs N}
    if N =< 0 then Xs
    else
        case Xs
        of H|T then {Drop T N-1}
        [] nil then nil
        end
    end
end
```

```oz
fun {Interval Xs N M}
    {Take {Drop Xs N-1} M-N}
end
```


### Exercice 7

```oz
fun {MultList L}
local
    fun {MultListHelper L A}
        case L
        of H|T then {MultListHelper T A*H}
        else A
        end
    end
in
    {MultListHelper L 1}
end
end
```


### Exercice 9

```oz
declare
L1=[[1 2 3] [4 5 6]]
L2=(1|2|3|nil)|(4|5|6|nil)|nil
```


### Exercice 10

```oz
declare
fun {Average L}
    fun {AverageAcc L S N}
        case L
        of nil then S div N
        [] H|T then {AverageAcc T H+S 1+N}
        end
    end
in
   {AverageAcc L 0 0}
end

declare
fun {SquareAverage L}
    fun {SquareAverageAcc L S N}
        case L
        of nil then S div N
        [] H|T then {SquareAverageAcc T H*H+S 1+N}
        end
    end
in
   {SquareAverageAcc L 0 0}
end

fun {Variance L}
local Tmp in
    Tmp={Average L}
    {SquareAverage L}-Tmp*Tmp
end
end
```


Exercices supplémentaires
-------------------------

### Exercice 3

Note: I didn't find a way to do it with an invariant, in a tail-recursive fashion.

```oz
fun {Add B1 B2}
local
    fun {AddDigits D1 D2 CI}
    local Tmp in
        Tmp = D1+D2+CI
        [(Tmp div 2) (Tmp mod 2)]
    end
    end

    Tmp1 Tmp2
in
    case B1
    of H|T then
        Tmp1={Add T B2.2}
        Tmp2={AddDigits H B2.1 Tmp1.1}
        Tmp2.1|Tmp2.2.1|Tmp1.2
    else
        [0]
    end
end
end
```
