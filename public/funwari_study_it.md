---
title: 初めての勉強会 若手エンジニアふんわりLT Night！感想戦
tags:
  - 'funwari_study_it'
  - '新卒1年目'
  - '新卒2年目'
  - '新卒3年目'
  - '大学生'

private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: ture
---

先日、春和の候！ 若手エンジニアふんわりLT　Night！に参加したので、記録として残すため、ブログ書きました。

https://wakate-funwari-study.connpass.com/event/308966/

## どんなイベント？
  > このイベントは、新卒1〜3年目ぐらいまでの若手エンジニア同士の交流を主目的としています。
  >前半はテーマフリーのLT（ライトニングトーク）会、後半は参加者同士が交流ができる懇親会を開催します！
  >LT会と懇親会を通して、エンジニア同士の横の繋がりを作りましょう！

  *イベント概要 (Connpass)[^1]* 
  
  ということで、社会人1~3年目といった若手向けの交流会です。実際自分は大学生ということで、この枠には入りませんが、若手社会人の先輩方がどのようなことに挑戦していて、働かれているのか解像度を上げるため、参加しました。

  会場は、AKKODiSコンサルティング株式会社さん。大学の友人が就活していた企業さんで、どんなオフィスなのか探検に行くような気分でした。かっこいいエントランスがあり、とても綺麗で快適でした。
  ![akkodis_office](https://github.com/ShotaArima/qiita/assets/130956497/594e96fb-8095-44c8-85f6-227745ee511b)




## 1. 個人開発でAWSを使いたい
発表者:javadogさん X:[@javadog_](https://x.com/Javadog_)
https://speakerdeck.com/javadog/hunwarilt
  最初から全てAWSを使用してのアプリケーション開発することは金銭的に高くなりやすい...
  スケールに合わせて激安サーバとAWSを拡使い分けていこう
  | 選択肢 | サービス | 月額料金 |
  | --- | --- | --- |
  | 1 | 激安サーバー＆Firebase無料枠 | 1,600円/月 |
  | 2 | 激安サーバー＆有料DBaaS | 4,000円/月 |
  | 3 | ECS on Fargate + RDS | 20,000円/月 |

  サービスの成長に合わせてDBの運用を柔軟に対応するためにも
  **DBの乗り換えをスムーズにする必要がある**

  - DBサービスの切り替え
    repositoryパターンで具体的な実装を隠しておく
    マイグレーションもしやすくなるはず

  - サーバの乗り換え
    オンプレサーバでもdocker-composeによるコンテナ運用にしておく

**感想**
  - オンプレサーバとのAWSとの併用について初心者でも理解しやすかった
  - 自分自身で開発したシステムでは、S3上にSQLiteのDBファイルを作成し、運用しているので、別のアプローチを知るいい機会になった
  ※SQLiteをS3上で運用した話についての記事は近日公開予定です。

## 2. 友達にコード送ったら１行にされた
発表者:mi111025さん X:[@mi111025](https://x.com/mi111025)
https://speakerdeck.com/mi111025/0425
  ポートフォリオサイトで色を表現するスライダーを作成
  RGB色空間での表現のためコードが煩雑に...
  HSL色空間で表現することで色相を変化させることで簡単に表現できるように！

**感想**
  - コードに詳しい人が友人にいてお互いにレビューできるいい関係性が素晴らしい
  - 自分のコードを作りっぱなしにしていないことがすごい！
  - 周りにいる詳しい人にどんどん質問していこうと思った

## 3. Let's learn code review
発表者:RioFujimonさん X:[@RioFujimon](https://x.com/RioFujimon)
https://speakerdeck.com/riofujimon/lets-learn-code-review

  コードレビューの課題点
  - 他人の書いたコードレベルが高くて、理解が難しい
  - どのような観点でレビューすればいいのかわからない
  - コードレビューで気をつける点はあるのか
  - 雰囲気でコードレビュー
  
  エンジニアはコードを読む時間のほうが長いので、勉強していこう！
  コードレビューについての参考文献
  - *Google's Code Review Guidelines [^2]*
  - *Google Style Guides [^3]* 
  
  > <コードレビューの目的>
  >- コードベース全体の健全性が時間と共に向上することを確認すること


  ><レビュー原則>
  > - 意見や個人の好みよりも技術的な事実とデータを優先
  > - コードスタイルはスタイルガイドに従う
  > - ソフトウェア設計は、原則の基づいて評価されるべき


**感想**
  - レビューの原則は、改めて全プログラマの共通認識として理解しないといけない内容だとわかった
  - ソースコード以外の全てのレビューにも同じことが言えそうな内容だった

## 4. Result型の次のエラーハンドリング
発表者:aka sousanさん X:[@moso_midnight](https://x.com/moso_midnight)
https://speakerdeck.com/riku_kuramoto/resultxing-noci-noerahandoringu

  jsでのtry-catchのエラーハンドリングについて以下のような問題がある
  - 処理が増えたら`if -else`が増える
  - エラーの網羅ができているか判断が大変

  **Railway Oriented**で、TS開発でより型安全なエラーハンドリングを実現する

**感想**
  - ネストが深くなり、どこでエラー処理を行なっているのか見失いやすいから、この処理はいずれか導入していきたいと思った
  - 導入するまでカロリーがいるとのことだったので、体力をつけようと思った
  - 懇親会でお話した際に、これから社内に浸透させていくということを伺い、熱量溢れている姿がかっこいいと思った


## 5. 記事の一歩目は業務内容から
発表者:yamatai12さん X:[@taiyama1212](https://x.com/taiyama1212)
https://speakerdeck.com/yamatai1212/ji-shi-no-bu-mu-haye-wu-nei-rong-kara

  1年で100本の記事投稿をしているとのこと
  何書けばいいかわからない人むけの発表でした
  
  > 業務派生で考えるきのネタ
  > - 詰まったこと
  > - メンバーが困っていること
  > - 深掘りしたいこと

  上記の内容で記事を書くと、自分が同じ部分で困ったことや他の人が困った時に役立つ記事になるとのこと
  社内で記事を共有すると強い領域の人からフィードバックがもらえることも！

**感想**
  - まず投稿頻度がレベチ
  - 自分はイベントごとの内容しか書いていないので、日頃のことを書こうと思った
  - 執筆活動は自分だけでなく、相手が助かることにもつながる！
  
## 6. Laravelのサービスコンテナを知ろう
発表者:たくみんさん X:[@Ota_Rg_Blog](https://x.com/Ota_Rg_Blog)
https://speakerdeck.com/takumin1234/laravelnosabisukontenawozhi-rou

  >DI(Depending Injection):依存性注入
  > 他のオブジェクトを使用する時に、自身の中ではく外から設定することで、よりスムーズなソース管理や処理の流れを再現するデザインパターンのこと

  例) 
  ```php
    class MyClass
    {
      public $email;

      public function __construct() {
        $this->email = new Email();
      }

      public function run() {
        $this->email->send();
      }
    }
    $myClass = MyClass();
    $myClass->run();
  ```
  *発表資料より[^4]*
  
  上記のようなクラスの場合、emailについてGmailを使用するとMyClassを修正する必要があり、柔軟性に欠ける
  これを依存関係を切り離すことで、より柔軟で、単体テストがやりやすくなるとのこと


**感想**
  - クラスの柔軟性について考えてこなかったので、今回を機にクラス設計について詳しく考えていこうと思った
  - 紹介されていた書籍[^5]を買ってみようと思った


## 7. 
発表者:さん X:[@](https://x.com/)
https://speakerdeck.com/


**感想**



## 8. 
発表者:さん X:[@](https://x.com/)
https://speakerdeck.com/


**感想**


## 9.
発表者:さん X:[@](https://x.com/)
https://speakerdeck.com/


**感想**


## 10. 
発表者:さん X:[@](https://x.com/)
https://speakerdeck.com/


**感想**


## 11. 
発表者:さん X:[@](https://x.com/)
https://speakerdeck.com/

**感想**


## 12.
発表者:さん X:[@](https://x.com/)
https://speakerdeck.com/


**感想**


## 懇親会＆まとめ


[^1]: [春和の候！若手エンジニアふんわりLT Night！](https://wakate-funwari-study.connpass.com/event/308966/)
[^2]: [Google's Code Review Guidelines](https://google.github.io/eng-practices/review/reviewer/standard.html)
[^3]: [Google Style Guides](https://google.github.io/styleguide/)
[^4]: [依存関係がある例](https://speakerdeck.com/takumin1234/laravelnosabisukontenawozhi-rou?slide=5)
[^5]: [なぜ依存を注入するのか DIの原理・原則とパターン](https://www.amazon.co.jp/%E3%81%AA%E3%81%9C%E4%BE%9D%E5%AD%98%E3%82%92%E6%B3%A8%E5%85%A5%E3%81%99%E3%82%8B%E3%81%AE%E3%81%8B-DI%E3%81%AE%E5%8E%9F%E7%90%86%E3%83%BB%E5%8E%9F%E5%89%87%E3%81%A8%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3-Compass-Books%E3%82%B7%E3%83%AA%E3%83%BC%E3%82%BA-Deursen%E3%80%81Mark-Seemann-ebook/dp/B0D1FNH7XB/ref=sr_1_1?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&crid=15UNXYFBX305K&dib=eyJ2IjoiMSJ9.YPpu-h_nygNPc22b1dnbyg.AnwtXLcGr-lWS30FGgxgrhde5-TAetxU0rcW1StWtuY&dib_tag=se&keywords=%E3%81%AA%E3%81%9C%E4%BE%9D%E5%AD%98%E3%82%92%E6%B3%A8%E5%85%A5%E3%81%99%E3%82%8B%E3%81%AE%E3%81%8B+DI%E3%81%AE%E5%8E%9F%E7%90%86%E3%83%BB%E5%8E%9F%E5%89%87%E3%81%A8%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3&qid=1714214897&sprefix=%E3%81%AA%E3%81%9C%E4%BE%9D%E5%AD%98%E3%82%92%E6%B3%A8%E5%85%A5%E3%81%99%E3%82%8B%E3%81%AE%E3%81%8B+di%E3%81%AE%E5%8E%9F%E7%90%86+%E5%8E%9F%E5%89%87%E3%81%A8%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%2Caps%2C308&sr=8-1)
