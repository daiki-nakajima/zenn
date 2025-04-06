---
title: "Windows で ポートを解放する方法"
emoji: "🌐"
type: "tech"
topics:
  - "windows"
  - "コマンドプロンプト"
published: true
published_at: "2023-11-04 23:00"
---

例えば、Spring Boot を停止した時に、Tomcat のプロセスが落ちないままアプリが終了することがある。

解決方法は単純で、netstat で使用している port を全て確認し、該当のプロセスをキルすればよい。

```sh
netstat -ano | find "8080"
  TCP         0.0.0.0:8080           0.0.0.0:0              LISTENING       59316
  TCP         [::]:8080              [::]:0                 LISTENING       59316

taskkill /F /PID 59316
成功: PID 59316 のプロセスは強制終了されました。
```

これでアプリを起動すれば動いた。
