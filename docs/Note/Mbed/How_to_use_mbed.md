# Mbed OSの基本的な使い方
Mbed OSは[ホームページ](https://os.mbed.com)から、ダウンロードできます。
プロジェクトを進めていく上では、補完機能の豊富なMbed Studioを使うのがお勧めです。
Homebrewが使えるのなら

```zsh
brew install mbed-studio
```

でMbed Studioをインストールすることができます。
Mbed Studioをインストールするには、アカウントが必要となるので、
ホームページからアカウントを作っておきましょう。

Mbed cliを用いる場合は、最新のpythonのある環境で

```
pip intall mbed-tools
```

からインストールできます。

## コマンド
```sh
mbed-tools detect
```

から、接続されているマイコンのリストを表示することができる。
上のリストのbuild_targetをコピーして

```
mbed-tools configure -m <build_target> -t GCC_ARM
```
