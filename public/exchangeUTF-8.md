---
title: Shift-JISのファイルをUTF-8にエンコードする
tags:
  - 'ファイル変換'
  - 'python'
  - 'Google Colab'
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---
# 1. はじめに
この記事では、ファイルの文字コードをshift-jisからUTF-8に変換するコードを紹介します。

# 2. 実行
1. お手持ちのGoogleアカウントで　Google Driveを開く
  - 合わせてフォルダのマウントも行ってください。
    >- 左にあるファイルマークを押してください。
    >- 以下のように4つのアイコンの中の左から3つ目のアイコンを押すことでできます。
    ><img width="342" alt="4つのアイコン" src="https://github.com/user-attachments/assets/3a20f89c-e343-418a-9ff4-7ed2ac8d25d2">
    >- 以下のように斜線が入ると完了しています。
    ><img width="342" alt="4つのアイコン" src="https://github.com/user-attachments/assets/455e72a9-f3d1-4a64-8aa0-4e74f7661d0a">

2. 以下のコードを貼り付る
```python
import csv
import codecs
import sys

def convert_encoding(input_file, output_file):
    try:
        # Shift-JISでファイルを開く
        with codecs.open(input_file, 'r', encoding='shift_jis') as file:
            data = file.read()
        
        # UTF-8でファイルを書き込む
        with codecs.open(output_file, 'w', encoding='utf-8') as file:
            file.write(data)
        
        print(f"変換が完了しました。出力ファイル: {output_file}")
    except UnicodeDecodeError:
        print("エラー: ファイルの読み込み中にデコードエラーが発生しました。入力ファイルがShift-JISでエンコードされていることを確認してください。")
    except IOError as e:
        print(f"エラー: ファイルの操作中にエラーが発生しました: {e}")
    except Exception as e:
        print(f"予期せぬエラーが発生しました: {e}")

if __name__ == "__main__":
    if len(sys.argv) != 3:
        print("使用方法: python script.py <入力ファイル> <出力ファイル>")
        sys.exit(1)
    
    input_file = sys.argv[1]
    output_file = sys.argv[2]
    
    convert_encoding('input-Shift-JIS-file', 'output-UTF-8-file')
```
3. 変更したいファイルをドライブにアップロード
4. アップロードしたファイルのパスをコピーし、貼り付ける
  - 「2」で貼り付けたソースコードの一番したにある`(Shift-JISのファイル)`に貼り付けてください  
  - また、`(UTF-8のファイル)`については、自分で設定することができます。変換がわかるように後ろに`UTF-8`などつけるとわかりやすいかもしれません。
5. Shift+Enterで実行
