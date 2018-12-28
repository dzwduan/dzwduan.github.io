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

![](/img/cs106b/%5CUsers%5C10184%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5C1545991699538.png)

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

```

