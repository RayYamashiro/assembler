
# Лабораторная работа по ЭВМ №5 и №6

## Лабораторная работа по ЭВМ №5

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

###  Контрольные вопросы
> 1. Каким ключевым словом открывается ассемблерная вставка?
>> **asm**
> 2. Где описываются выходные параметры ассемблерных вставок расширенного
>синтаксиса GCC? Что означают символы =, =&, + в начале строки ограничений
>выходного параметра?
>>
> 3. Где описываются входные параметры?
>>
> 4. Где указывается список перезаписываемых [clobbers] во вставке регистров
>(кроме параметров)? Какая строка соответствуют изменению флагов 𝑓𝑙𝑎𝑔𝑠?
>Какая строка соответствуют изменению памяти (кроме параметров)?
>>
> 5. Где описываются метки ЯВУ, на которые может быть передано управление из
>вставки?
>>
> 6. Какое ключевое слово нужно указать после asm, чтобы запретить компилятору
>оптимизировать вставку?
>> **volatile**
> 7. Какое ключевое слово нужно указать после asm, чтобы показать, что управление из вставки может передаваться на ту или иную метку C/C++ (зато у этой
>вставки нет выходных параметров)?
>> **goto**

## Лабораторная работа по ЭВМ №6
### Задание 1
Task:

![task1](https://i.postimg.cc/vBNVSCyz/1.png)
```
void task_1() {
    int x = 4, y = 5, z;
    asm (
    "sub $1, %[X]\n"
    "sub $1, %[Y]\n"
    "mov %[X], %%eax\n"
    "imul %[Y]\n"
    "mov $5, %%ebx\n"
    "sub %%eax, %%ebx\n"
    : "=&b" (z)
    : [X]"r" (x), [Y]"r" (y)
    : "eax", "cc"
    );

    std::cout << "Math: " << (5 - (x - 1) * (y - 1)) << std::endl;
    std::cout << "z: " << z << std::endl;
}
```
output on display:


![output on display](https://i.postimg.cc/nrqqj08G/1.png)

### Задание 2
Task:

![task2](https://i.postimg.cc/bNXK7J37/1.png)
```
void task_2() {
    int x = 1, z;

    asm (
    "lea -12(, %[X], 4), %%eax"
    : "=&a" (z)
    : [X]"r" (x)
    : "cc"
    );

    std::cout << "Math: " << (4 * x - 12) << std::endl;
    std::cout << "z: " << z << std::endl;
}

```
output on display:


![output on display](https://i.postimg.cc/SK8Qp5cd/1.png)

### Задание 3
Task:

![task3](https://i.postimg.cc/GhQqXhNJ/1.png)
```
void cpuid_test(unsigned int *pA, unsigned int *pB, unsigned int *pC, unsigned int *pD)
{
    asm(
    "cpuid\n"
    : "+a"(*pA), "+b"(*pB), "+c"(*pC), "+d"(*pD)
    );

    printf("%08c %08c %08c %08c\n", 'A', 'B', 'C', 'D');
    printf("%08X %08X %08X %08X\n", *pA, *pB, *pC, *pD);
}

#define PRINT_FLAG(tag, r, b) printf("%s[%2d] %9s: %X\n", #r, b, #tag, ( r & (1<<b) ) !=0 )

void task_3() {
    unsigned int A = 1, B = -1, C = -1, D = -1;
    cpuid_test(&A, &B, &C, &D);

    PRINT_FLAG(FPU,    D, 0);
    PRINT_FLAG(SSE,    D, 25);
    PRINT_FLAG(SSE2,   D, 26);
    PRINT_FLAG(SSE3,   C, 0);
    PRINT_FLAG(SSSE3,  C, 9);
    PRINT_FLAG(SSE4.1, C, 19);
    PRINT_FLAG(SSE4.3, C, 20);
    PRINT_FLAG(AVX, C, 28);

}
```
output on display:


![output on display](https://i.postimg.cc/59mBVK3f/1.png)

### Задание 4
Task:

![task4](https://i.postimg.cc/L5SGZQYH/1.png)
```
void task_4() {
    double x = 5.0, y = 5.0;

    asm (
    "vsubsd %[Const], %[Y], %[Y]\n"
    : [Y]"+x"(y)
    : [Const]"x"(23.0)
    );
    std::cout << "Math: " << (x - 23) << std::endl;
    std::cout << "y: " << y << std::endl;
}
```
output on display:


![output on display](https://i.postimg.cc/mgSkcKPr/1.png)

### Задание 5
Task:

![task5](https://i.postimg.cc/TwM4ZNcr/1.png)
```
void task_5() {
    volatile double x = 5.0, y = 5.0, c = 23.0;

    asm (
    "fldl %[C]\n"
    "fldl %[Y]\n"
    "fsub %%st(1), %%st(0)\n"
    "fstpl %[Y]\n"
    "ffree %%st(0)"
    : [Y]"+m"(y)
    : [C]"m"(c)
    );
    std::cout << "Math: " << (x - 23) << std::endl;
    std::cout << "y: " << y << std::endl;
}
```
output on display:


![output on display](https://i.postimg.cc/wTyDmxzj/1.png)

### Задание 6
Task:

![task6](https://i.postimg.cc/VsbjSLmL/1.png)
```
void task_6() {

    asm (
    "vmovupd %[X], %%ymm0\n"
    "vmovupd %[Y], %%ymm1\n"
    "vaddpd %%ymm1, %%ymm0, %%ymm2\n"
    "vsubpd %%ymm1, %%ymm0, %%ymm3\n"
    "vdivpd %%ymm3, %%ymm2, %%ymm4\n"
    "vmovupd %%ymm4, %[Z]\n"
    : [Z]"=m"(z)
    : [X]"m"(x), [Y]"m"(y)
    : "ymm0", "ymm1", "ymm2", "ymm3", "ymm4"
    );

    printf("x %lf, y %lf, z %lf, w %lf\n", calc(0), calc(1), calc(2), calc(3));
    printf("x %lf, y %lf, z %lf, w %lf\n", z[0], z[1], z[2], z[3]);
}
#define calc(i) (x[i] + y[i]) / (x[i] - y[i])

alignas(32) double x[4] = { 1.0, 2.0, 3.0, 4.0 };
alignas(32) double y[4] = { 5.0, 6.0, 7.0, 8.0 };
alignas(32) double z[4] = { 0.0, 0.0, 0.0, 0.0 };

```
output on display:


![output on display](https://i.postimg.cc/LsNJWGSW/1.png)

### Задание 7
Task:

![task7](https://i.postimg.cc/8Cj7qtCP/1.png)
```
void task_7() {
    asm (
    "vmovapd %[X], %%ymm0\n"
    "vmovapd %[Y], %%ymm1\n"
    "vaddpd %%ymm1, %%ymm0, %%ymm2\n"
    "vsubpd %%ymm1, %%ymm0, %%ymm3\n"
    "vdivpd %%ymm3, %%ymm2, %%ymm4\n"
    "vmovapd %%ymm4, %[Z]\n"
    : [Z]"=m"(z)
    : [X]"m"(x), [Y]"m"(y)
    : "ymm0", "ymm1", "ymm2", "ymm3", "ymm4"
    );

    printf("x %lf, y %lf, z %lf, w %lf\n", calc(0), calc(1), calc(2), calc(3));
    printf("x %lf, y %lf, z %lf, w %lf\n", z[0], z[1], z[2], z[3]);
}
```
output on display:


![output on display](https://i.postimg.cc/L5T4q1sb/1.png)

###  Контрольные вопросы
> 1. Какие вы знаете регистры общего назначения x86 и x86-64?
>> Архитектура x86-64 имеет:
>>16 целочисленных 64-битных регистров общего назначения (RAX, RBX, RCX, RDX, RBP, RSI, RDI, RSP, R8 — R15);
>>8 80-битных регистров с плавающей точкой (ST0 — ST7);
>>8 64-битных регистров MMX (MM0 — MM7, имеют общее пространство с регистрами ST0 — ST7);
>>16 128-битных регистров SSE (XMM0 — XMM15);
>>64-битный указатель RIP и 64-битный регистр флагов RFLAGS
>>
>>Архитектура x86
>>IP (англ. Instruction Pointer) — регистр, указывающий на смещение (адрес) инструкций в сегменте кода (1234:0100h сегмент/смещение).
>>
>>IP — 16-битный (младшая часть EIP)
>>
>>EIP — 32-битный аналог (младшая часть RIP)
>>
>>RIP — 64-битный аналог

> 2. Какие вы знаете команды ассемблера x86?
>>-        Команды общего назначения
>>-        Системные команды
>>-        Команды сопроцессора (x87 FPU)
>>-        Команды управления состоянием сопроцессора и SIMD
>>-        Команды технологии MMX
>>-        Команды расширения SSE
>>-        Команды расширения SSE2
>>-        Команды расширения SSE3
>>-        Команды расширения 3DNow
