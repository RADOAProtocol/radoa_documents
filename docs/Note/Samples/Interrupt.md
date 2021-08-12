# 割り込み

## 割り込みとは
　割り込みとは、メインのプロセス(ループなど)が実行されている時、別の処理をするための機能です。

　割り込みでは、割り込みのトリガーとなるイベント(クロック、ピン変化、データ受信時等）
を合図に、割り込みベクターに記録されている処理を実行します。

　 **割り込みベクター**とは、割り込み処理用の処理の関数のアドレスを格納したメモリ上の
配列のことを指しています。今回の例では、

```
button.rise(&counter_reset);
```

　としている場所で、割り込みハンドラーを割り込みベクターに格納しています。

　割り込みベクターやハンドラーなどの細かな仕様は使っているツールやライブラリによ ります。
基本的に、マイコンではベクターテーブルなどメモリの確保が必要なので、
割り込みが可能かどうかはマイコンの仕様によります。

　割り込みと似たような機能として、スレッド機能があります。スレッド機能は、
割り込みと同じようにメインの処理をしている時に別の処理も同時に行いたい時に使うものです。

　このスレッドと、割り込みの大きな違いは処理が同時に行われるか、並行して行われるかの違いです。

　つまり、割り込み処理で長い時間がかかる処理をする場合、その時間だけメインの処理が
止まることになります。


## 割り込みの種類
- タイマー割り込み
- ピン変化割り込み
- 


## タイマー割り込み
マイコンに組み込みこまれたタイマー、またはクロックに伴って割り込みを決める。

## ピン変化割り込み
　Mbed OSではピン変化割り込みに対応しています。

```
InterruptIn button(A2);
```

　からA2ピンにピン変化割り込みの初期化をすることができます。

　InterruptInに対して`.rise`または、`.fall`で割り込みハンドラーを追加することができます。
ピンがLOW（GLOUND）からHIGHになった時に割り込みが発生する。riseと、ピンがHIGHからLOW
になった時に割り込みが発生するfallです。

　注意することは、ピンのチャタリングによって割り込みが複数回入ることがあります。
コンデンサー等を用いて、電気的にフィルターをかけて防止するか、短時間で複数回処理が
入った時に処理をキャンセルするようなプラグラムを作る必要があります。

- 最小構成のサンプル 
```cpp
#include "mbed.h"
 
InterruptIn button(p5);
DigitalOut led(LED1);
DigitalOut flash(LED4);
 
void flip() {
    led = !led;
}
 
int main() {
    button.rise(&flip);  // attach the address of the flip function to the rising edge
    while(1) {           // wait around, interrupts will interrupt this!
        flash = !flash;
        wait(0.25);
    }
}
```

[引用元](https://os.mbed.com/users/okini3939/notebook/interruptin_jp/)

## タイマー割り込み
　タイマー割り込みは、クロックによって割り込みを発生させる機能です。

　クロックによって割り込みが発生するため、この割り込みでは一定時間ごとの周期実行
しかすることができません。

```cpp
#include "mbed.h"

Ticker flipper;

DigitalOut led1(PC_8);
DigitalOut led2(PC_6);

void flip() { led2 = !led2; }

int main() {
    led2 = 1;
    flipper.attach(&flip, 100ms);
    while (1) {
        led1 = !led1;
        wait_us(200'000);
    }
    return 0;
}
```

## その他割り込み
　mbed os ではその他の割り込みにも対応しています。シリアル通信の通信時に発生させる
[Serial Interrupt](https://os.mbed.com/cookbook/Serial-Interrupts)などがあります。

