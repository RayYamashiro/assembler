
# Лабораторная работа по ЭВМ №5

### Задание 1
Task:

![task1](https://i.postimg.cc/NFJPnJnk/1.png)
```
void task1(){
    unsigned int x,y,z,w;
    x = 0xFFFFFF89-34;
    y = 2;
    asm(
    "movl %[X], %%eax\n\t"
    "movl %[Y], %%ebx\n\t"
    "addl $34, %%eax\n\t"
    "XOR %%edx, %%edx\n\t" //обнуление регистра
    "divl %%ebx\t"
    :"=a"(z), "=d"(w)
    : [X] "m"(x), [Y] "m"(y)
    :"cc", "ebx");
    cout << hex << z << ' ' << hex << w << endl;
    // проверка
    z = (x+34)/y;
    w = (x+34)%y;
    cout << hex << z << ' ' << hex << w << endl;
}
```
output on display:


![output on display](https://i.postimg.cc/W3tNKKW5/1.png)
### Задание 2
Task:

![task2](https://i.postimg.cc/P5Z2b5d4/2.png)
```
void task2(){
    unsigned int x,y,z,w;
    x = 0xFFFFFF89-34;
    y = 2;
    asm(
    "XOR %%edx, %%edx\n\t"
    "addl $34, %%eax\n\t"
    "divl %[Y]\t"
    :"=a"(z), "=&d"(w)
    : [X] "a"(x), [Y] "r"(y)
    :"cc"
    );
    cout << z << ' ' << w << endl;
    // проверка
    z = (x+34)/y;
    w = (x+34)%y;
    cout << z << ' ' << w << endl;
}
```
output on display:


![output on display](https://i.postimg.cc/bdfqZvz5/2.png)
### Задание 3
Task:

![task3](https://i.postimg.cc/T3rJLNcN/3.png)
```
void task3(){
    unsigned int x,y,z,w;
    x = 0xFFFFFF89-34;
    y = 2;
    unsigned int* p = &x;
    unsigned int* q = &y;
    asm (
    "XOR %%edx, %%edx\n\t"
    "movl (%[pX]), %%eax\n\t"
    "addl $34, %%eax\n\t"
    "divl (%[pY])"
    :"=&a"(z), "=&d"(w)
    :[pX] "r"(p), [pY]"r"(q)
    :"cc"
    );
    cout << z << ' ' << w << endl;
    // проверка
    z = (x+34)/y;
    w = (x+34)%y;
    cout << z << ' ' << w << endl;
}
```
output on display:


![output on display](https://i.postimg.cc/1RkFGcXk/3.png)
### Задание 4
Task:

![task4](https://i.postimg.cc/5y24ct4d/4.png)
```
void task4(){
    unsigned short x,y,z,w;
    x = 0xFF89-34;
    y = 2;
    asm (
    "XOR %%dx, %%dx\n\t"
    "mov %[X], %%ax\n\t"
    "mov %[Y], %%bx\n\t"
    "add $34, %%ax\n\t"
    "div %%bx\t"
    :"=a"(z), "=d"(w)
    : [X] "m"(x), [Y] "m"(y)
    :"cc", "ebx");
    cout << z << ' ' << w << endl;
    // проверка
    z = (x+34)/y;
    w = (x+34)%y;
    cout << z << ' ' << w << endl;
}
```
output on display:


![output on display](https://i.postimg.cc/HWc58PY0/4.png)
### Задание 5
Task:

![task5](https://i.postimg.cc/ydXx4509/5.png)
```
void task5(){
    int const N = 5;
    int M[N];
    for (int i = 0; i < N; i++)
        M[i] = 0x44332211;
    cout << "Data: ";
    for (int i = 0; i < N; i++)
        printf("%#10X ", M[i]);
    cout << endl;
    size_t k = 3; // позиция
    int x = 7; // число
    asm (
    "movl %0, (%1, %2, 4)\n"
    :
    :"r"(x), "r"(M) ,"r"(k)
    : "memory"
    );
    cout << "Data: ";
    for (int i = 0; i < N; i++)
        printf("%#10X ", M[i]);
}
```
output on display:


![output on display](https://i.postimg.cc/4y9J4cTt/5.png)
### Задание 6
Task:

![task6](https://i.postimg.cc/0jYp4cyH/6.png)
```
void task6(){
    int const N = 5;
    unsigned int M[N];
    for (int i = 0; i < N; i++)
        M[i] = 0x44332211;
    size_t j = 4;
    cout << "Data: ";
    for (int i = 0; i < N; i++)
        printf("%#10X ", M[i]);
    cout << endl;
    asm(
    "movb $0xFF, 3(%0, %1, 4)\n"
    :
    : "r"(M) ,"r"(j)
    : "memory"
    );
    cout << "Data: ";
    for (int i = 0; i < N; i++)
        printf("%#10X ", M[i]);
}
```
output on display:


![output on display](https://i.postimg.cc/Y9Vrw3TM/6.png)
