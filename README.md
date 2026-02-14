# playwright-sandbox

playwrightで遊んでみたい

- https://playwright.dev/docs/intro

## セットアップ記録

```bash
sudo apt install libavif13
```

依存が足りてなくてエラーが出て、以下のコマンドの実行を推奨された。  
aptでインストールしても、npxでインストールしても、あまり変わらない問題ではあるが...  
なんとなく心象が悪かったので、aptでインストールした。

```bash
sudo npx playwright install-deps
```

## テストをレコーディングしたい

`npx playwright test --ui` で、テストの各ステップをWeb UIで確認できる。  
が、バグったときのエビデンスを追いたいとか、取引先に共有するときにラクできるなって思っている。  

- https://playwright.dev/docs/videos
- https://playwright.dev/docs/screenshots

このあたりを設定しておくと良いっぽい。  

- https://github.com/stoneream/playwright-sandbox/commit/3fb5dfb2c1c63588f7c59094ea358d2145fabd3d#diff-f679bf1e58e8dddfc6cff0fa37c8e755c8d2cfc9e6b5dc5520a5800beba59a92R53-R58

画面のサイズを指定することができるので、レスポンシブの表示崩れも確認できるなーって思っている。

- https://github.com/stoneream/playwright-sandbox/commit/4ca5ed34136bb81cd5942db76de88bda911501f7

## locator

- CSSセレクタ
- XPath
- HTMLのrole & name

とかで指定できる。  
Seleniumと比較した強みで言えば、HTMLのrole & nameで指定できることと思った。  
DOM構造の変化に強い & テストの意図がコード上からわかりやすい。  

- https://playwright.dev/docs/locators

## Test Hooks

アクセス先がテスト毎に同じ場合に、いちいち `page.goto()` を書かなくても良くなる。  

- https://github.com/stoneream/playwright-sandbox/commit/25a6e226f2079a2a7aed21345c256792d8de8dff#diff-693fac4ef68a358712eaea54434547b7cdd2eca27ce87c64f7943352f4571f74R3-R6

その他のHookは以下参照。  

- https://playwright.dev/docs/api/class-test

## テストジェネレータ

いちいちコード書くの面倒臭いって問題は絶対に発生するし、実際書きたくないかも。  
ブラウザ上から操作を記録して、テストコードを自動生成してくれる機能がある。  

- https://playwright.dev/docs/codegen-intro

## ヴィジュアルリグレッションテスト

掲題の通り、やりたい。  

- https://playwright.dev/docs/test-snapshots

とりあえずこう書いてみると良い。  

- https://github.com/stoneream/playwright-sandbox/commit/5361edc32448f7d6ee34301ea354da2606d6ef45

初手はdiffが無いから失敗するが、意図した変更であれば、`npx playwright test --update-snapshots` でスナップショットを更新できる。  
次の実行では差分が現れない（はず）なので、テストの成功は確認できる。

サイトでアニメーションを使っていたりすると、意図しない差分が出る可能性がある。  

スナップショットを永続化しておかないと、CI/CDで差分がチェックできない。  
一方でリポジトリに画像データを大量に置きたくないので、どうしようかなぁって思っている。  

一方でGitHub Actionsのアーティファクト機能のストレージ枠ってFreeプランだと割と少なめなので現況はローカルホストで動かして置くのが良さそうか...?（チーム利用を想定しない場合。）

## ログインセッションの永続化

tbd

ログインしているユーザーとしていないユーザーの挙動をチェックしたい場合に毎回ログインするのは流石に面倒臭いので、 Test Hookあたりで一発目ログインしたら、そのセッション情報を保存しておいて、以降のテストで使い回すと良さそう、みたいなことを考えているが実現ができるか不明。

- https://playwright.dev/docs/auth?utm_source=chatgpt.com

## todo

- linter/formatterの導入
