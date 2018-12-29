---
layout:     post
title:      CS106B Assignment1
subtitle:   stanford cpp code 
date:       2018-12-28
author:     Dzw
header-img: img/home-bg-o.jpg
catalog: 	 true
tags:
    - cpp
    - algorithm
---





# CS106B Assignment1





## problem one: Rosencrantz and Guildenstern Flip Heads

```
flip a coin repeatedly , until three consecutive heads are tossed.
output: total number of flip.
Like below:
Flip: heads
Flip: tails
Flip: heads
Flip: tails
Flip: tails
Flip: heads
Flip: heads
Flip: heads
It took 8 flips to get 3 consecutive heads.
(hint : provide a random number generation library that we can transfer)
```

```c++
code:

void flipHeads() {
    // [TODO: fill in the code]
    int head_cnt=0;
    int total = 0;
    while(head_cnt<3){
        bool coin = randomChance(0.5);
        if (coin==false){
            cout<<"Flip: tails"<<endl;
            head_cnt=0;}
        else{
            cout<<"Flip: heads"<<endl;
            head_cnt++;
        }
        total++;
    }
    cout<<"It took"<<total<<" flips to get 3 consecutive heads.";
}


```





## problem two:combinations and pascal's triangle

```
from a group of n people to choose a group of k of them.
in below part,15 corresponds to 6choose2.
it express solutions in pascal's triangle form
```

![](/img/cs106b/1.png)

```
code:
int nChooseK(int n, int k) {
    // [TODO: fill in the code]
    if (k==0 || k==n) 
        return 1;
    else
        return nChooseK(n-1,k-1)+nChooseK(n-1,k);
}

```



## problem three:stack overflows

```
just imitate a recursive sequence
```





## problem four:Implementing Numeric Conversions

```
use recursion to convert int to string and convert string to int.
hint: 
1.recommend split a number into  pieces by separating the last(the 7 in 137) rather than the first digit(the 1 in 137)
2.char ch = char(digit + '0')       from int tochar
  int val = ch - '0'               from char to int
  string str(1, ch);               form char to string

```

```

int stringToInt(string str) {
	// [TODO: fill in the code]
	int num = 0;
	if (str.empty())
		return num;
	if (str.length() == 1 && str[0] != '-')
		return str[0] - '0';
	if (str.length() == 1 && str[0] == '-')
		return num;
	if (str.length() > 1 && str[0] != '-'){
		int tmp = str[str.length() - 1] - '0';
		return stringToInt(str.substr(0, str.length() - 1))*10 + tmp;
	}
	if (str.length() > 1 && str[0] == '-'){
		int tmp = str[str.length() - 1] - '0';
		return stringToInt(str.substr(0, str.length() - 1)) * 10 - tmp;
	}
}

string intToString(int n) {
	// [TODO: fill in the code]
	string str;
	if (n >= 0 && n<10){
		str = n + '0';
		return str;
	}
	if (n>10){
		char ch = n % 10 + '0';
		return intToString(n / 10) + ch;
	}
	if (n < 0){
		return "-" + intToString(abs(n));
	}
}
```





## problem five: the flesch-kincaid grade-level test