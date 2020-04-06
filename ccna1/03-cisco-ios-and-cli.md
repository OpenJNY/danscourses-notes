# Cisco IOS and CLI
https://danscourses.com/cisco-ios-and-cli/

## 概要

Cisco CCNAのためには、コマンドラインインタフェースまたはCLIを使用してCiscoルータとCiscoスイッチを設定する方法を知っている必要があります。コマンドラインインターフェースは、コマンド駆動型のユーザーシェルで、ユーザーがオペレーティングシステムとのインターフェースを行うことができます。コマンドラインインターフェースまたはCLIは、キーボードだけで操作します。対照的に、グラフィカル・ユーザー・インターフェース（GUI）は、キーボードに加えてマウスを使用することを特徴とするアイコンとメニュー駆動型のユーザー・シェルです。Catalyst スイッチや統合サービスルーターで使用される Cisco オペレーティングシステムは、Cisco IOS（Internetwork Operating System）として知られています。

RAM ( 一時メモリ ) - IOS とコンフィグファイルは、ルータの起動時に RAM に読み込まれて実行されますが、通常は FLASH (IOS) と NVRAM (startup-config) に保存または保存されます。ルーティングテーブルはRAMから実行されます。ルータやスイッチはすべてをRAMで実行するので、非常に高速です。設定変更はすぐに RAM (runing-config) で実行されますが、NVRAM (startup-config) に保存して永久保存することができます。
FLASH (パーマネントメモリ) - これはIOSが保存されている場所です。
NVRAM (パーマネントメモリ) - 起動設定ファイルが保存されている場所です。
ROM (恒久的で変更不可) - BIOS、POST、ROMMONが格納されている場所です。

## IOS と CLI

CLI を使って IOS のコンフィグを変更する方法は 3 つある。

- コンソール: コンソールケーブルをシリアルポートに挿入して使う
- Telnet/SSH: L3 レベルでの接続性と安全性を提供
- Aux: auxiliary port はモデムを接続する為に作られたもの。

## コマンド例

```
enable

configure terminal
show running-config
show startup-config
show version
show flash
copy running-config startup config
```