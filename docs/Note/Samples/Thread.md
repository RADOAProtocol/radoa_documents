# Thread

## Thread
　Mbed ではマルチスレッドでプログラムを実行することができます。

- Sample
```cpp
#include "mbed.h"

DigitalOut led1(LED1);
DigitalOut led2(LED2);
Thread thread;

void led2_thread()
{
    while (true) {
        led2 = !led2;
        ThisThread::sleep_for(1000);
    }
}

int main()
{
    thread.start(led2_thread);

    while (true) {
        led1 = !led1;
        ThisThread::sleep_for(500);
    }
}
```

[引用](https://os.mbed.com/docs/mbed-os/v6.13/apis/thread.html)

　プロジェクト中では、Displayというスレッドを立てて、それを実行することで
ディスプレイ表示をすることができるようになっています。

　スタートさせたTheadを止める時には、`therad.join()` (スレッドが終わるまで待つ)や、
`thread.terminate()` を使います。
