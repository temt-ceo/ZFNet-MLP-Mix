## Unordered Data Structures in C++

## Hash Tables
・Decompose a real-world problem, such as cache memory, into the appropriate implementation of a hash table,<br>

#### Hashing
1. a hash function: Map our input space into an array index.その代わりO(1)だけで行うので著しく早い。<br>
2. an array: Stores the actual data.(this is going to be indexed in by hash function.)<br>
3. collision handling strategy: Handle the collisions that occur when our hash function maps different values to the same point in the array.<br>

・ SUHA: The simple uniform hashing assumption.(値が同じにならない事。hash functionの要件③。要件①はO(1)である事。要件②は同じキーであれば毎回同じ値になる事。)<br>
・ ３つのhash functionの要件のテストには長い年月がかかるので、既存のhash functionを利用する事がベストである。<br>

（最もシンプルなのはintegerに対するmod function(%)）<br>
シンプルゆえにcollision（arrayの同じ値を指す事）が起こる。
```
Note that in binary, the number 7 is 0000...0111.
(The leading digits are all zero, followed by three 1 digits, because these place values represent 4+2+1.)
When you do "key & 7", the result will have leading zeros, and the rightmost three digits will be the same as those of key.
Because this results in values between 0 and 7, it's similar to taking the remainder of division by 8.
That is, "key & 7" should give the same result as "key % 8".
// '&' は早いが、プラットフォームの制約を受ける(マシンを変えるなどが可能か検証が必要)ので常に'&'がいいとは限らない。
```

#### ReHashing
Arrayがfullになった時に領域を倍に増やす、この時同じkeyをhash functionは指すのか（integerであればmod(割る値が10->20だと同じ値にはならない)）

#### unordered_map
・ std::map ... O(log(n)) だけ時間がかかる<br>
・ std::unordered_map ... 上記の通り早い。<br>

