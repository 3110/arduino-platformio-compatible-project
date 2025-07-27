# Arduino IDEとPlatformIO IDEとで共有可能なプロジェクト構造

Arduino IDEとPlatformIO IDEの両方で使用できるプロジェクト構造を提供します。詳細は[1]をご覧ください。

## プロジェクト構造

```
arduino-platformio-compatible-project/
├── examples/          # Arduino IDEとPlatformIO IDE共通のサンプルコード
│   ├── Sample1/
│   │   └── Sample1.ino
│   └── Sample2/
│       └── Sample2.ino
├── include/           # ライブラリのヘッダーファイル
│   └── SampleLib.h
├── src/               # ライブラリのソースコード
│   └── SampleLib.cpp
├── platformio.ini     # PlatformIO設定ファイル
└── README.md
```

## Arduino IDEでの使用方法

1. `examples/`フォルダ内の任意のサンプル（例：`Sample1/`）を開く
2. Arduino IDEでそのまま編集・コンパイル・アップロードが可能

## PlatformIO IDEでの使用方法

1. プロジェクトルートでPlatformIO IDEを開く
2. `platformio.ini`の`default_envs`を変更してサンプルを切り替え：
   ```ini
   default_envs = Sample1  # Sample1を使用する場合
   ; default_envs = Sample2  # Sample2を使用する場合
   ```
3. ビルド・アップロードを実行

## 共有のキーポイント

PlatformIO IDEでArduino IDEとプロジェクト構造を共有するためのキーポイントは以下の通りです。

### 動的なサンプルフォルダ指定

```ini
src_dir = examples/${platformio.default_envs}
```

この設定により、`default_envs`で指定した環境名に対応するサンプルフォルダがソースディレクトリとして使用されます。

PlatformIO IDEでは`src_dir`に指定されたフォルダの直下にある`.ino`ファイルを`.ino.cpp`に変換してコンパイルできるため[1]，この設定によりArduino IDEのソース構造をそのまま適用できるようになります。

### ライブラリパスの設定によるワークアラウンド

```ini
lib_extra_dirs = ../../src,../../include
```

ライブラリのソースコードを`src`の直下にまとめる方法もありますが，このようにソースファイルとヘッダファイルを分けておかないと，`lib_deps`で指定したライブラリのコンパイル時にヘッダが見つからないという問題が出ます。

## 参考

* [1] [Arduino IDEとPlatformIO for VSCodeでM5Stack用ライブラリのソースツリーを共有する](https://zenn.dev/saitotetsuya/articles/share_source_tree_with_arudino_ide_and_platformio)

* [2] [3.1. PlatformIOでinoファイルをコンパイルする](https://zenn.dev/saitotetsuya/articles/share_source_tree_with_arudino_ide_and_platformio)（[PlatformIO豆知識](https://zenn.dev/saitotetsuya/articles/platformio_tips#3.1.-platformio%E3%81%A7ino%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%82%92%E3%82%B3%E3%83%B3%E3%83%91%E3%82%A4%E3%83%AB%E3%81%99%E3%82%8B)）