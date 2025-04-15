---
title: "Javaでファイルを使用中、inodeレベルでは何が起きるか"
emoji: "☕"
type: "tech"
topics:
  - "Java"
published: true
published_at: "2025-04-16 12:00"
---

Java でファイルを開いたまま `mv` で置き換えた場合、Java プログラムは新しい内容を読み取るのか？  
Linux の inode の仕組みと合わせて挙動を確認してみました。

## 結論：Javaはinodeを保持し続ける

Linux では「ファイル名」と「実体（inode）」は別物です。  
Java はファイルを `open()` した時点で inode に紐付けられるため、途中で `mv` によりファイルが入れ替わっても、**開いた時点の inode を参照し続けました。**

## ファイル構成と inode 確認

テキストファイルに「1, 2, 3, 4, 5」と書いて、Java プログラムで読み取ります。

```plaintext
src/main/resources/SampleFile.txt      ← 元ファイル: 1, 2, 3, 4, 5
src/main/resources/NewSampleFile.txt   ← 新ファイル: A, B, C, D, E
```

作成したファイルの inode 番号を確認します。

```bash
# inode番号を確認
ls -i src/main/resources/SampleFile.txt
# → 2053118997

ls -i src/main/resources/NewSampleFile.txt
# → 2053119033
```

## 検証コード

```java
import java.io.BufferedInputStream;
import java.io.FileInputStream;

public class InodeFileMoveSample {
    public static void main(String[] args) {
        String path = "src/main/resources/SampleFile.txt";

        try (BufferedInputStream bis = new BufferedInputStream(new FileInputStream(path))) {
            System.out.println("[初回読み取り]");
            int data;
            while ((data = bis.read()) != -1) {
                System.out.print((char) data);
            }

            System.out.println("\n--- 5秒スリープ中にファイルを mv してください ---");
            Thread.sleep(5000);

            System.out.println("\n[2回目読み取り]");
            while ((data = bis.read()) != -1) {
                System.out.print((char) data);
            }

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## 実行手順

```bash
javac -d out src/main/java/com/example/InodeSample.java
java -cp out com.example.InodeSample
```

1. 「1, 2, 3, 4, 5」が表示された後、5秒スリープに入る  
2. その間に、別ターミナルで以下を実行：

   ```bash
   mv src/main/resources/NewSampleFile.txt src/main/resources/SampleFile.txt
   ls -i src/main/resources/SampleFile.txt
   ```

   → inode が新しくなっているはず（2053119033）

3. しかし、Java プログラムの 2回目の出力は変わらない

## まとめ

- Java はファイルを開いた時点の inode を参照し続ける  
- `mv` によるファイル置き換えでは、新しい内容は読み取られない  
- 内容を更新するには、明示的に再オープンする必要がある

## 参考

Java に限らず、OS レベルでファイルディスクリプタを保持する限り、inode の切替は反映されないようです。

[ファイルやディレクトリの実体の確認方法としての inode 番号](https://zenn.dev/mtmatma/articles/a59b58e33a330f)
