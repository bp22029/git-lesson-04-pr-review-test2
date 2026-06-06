# 04 Pull Request とレビュー

## レッスンのリポジトリ

- 前のレッスン: [git-lesson-03-conflict](https://github.com/bp22029/git-lesson-03-conflict.git)
- このレッスンのテンプレート: [git-lesson-04-pr-review](https://github.com/bp22029/git-lesson-04-pr-review.git)
- 次のレッスン: [git-lesson-05-actions-artifact](https://github.com/bp22029/git-lesson-05-actions-artifact.git)

この演習で学生が clone するのは、教員から指定されたグループ用共有リポジトリです。上のテンプレートリポジトリを直接 clone しないでください。

## 学習目標

この演習では、feature ブランチで変更を行い、GitHub 上で Pull Request を作成し、レビューを受ける流れを体験します。

終了後、次のことを説明できるようになることを目標にします。

- Pull Request の目的を説明できる
- feature ブランチを GitHub に push できる
- PR テンプレートに沿って変更内容を書ける
- レビュー観点に沿って他の人の変更を確認できる
- レビューコメントを受けて、必要な修正を追加 commit できる

## 前提条件

- GitHub に push できる
- `main`、`develop`、`feature` ブランチの役割を学んでいる
- feature ブランチを作成できる
- GitHub の画面で Pull Request を開ける

## この演習だけの重要な前提

この演習では、グループごとに1つの共有リポジトリを使います。

グループ全員が同じ共有リポジトリを clone し、各自が自分用の feature ブランチを作成します。

レビューは GitHub の Pull Request 画面で行います。レビュー担当者が、相手のブランチにローカルで切り替える必要はありません。

## このリポジトリで使うファイル

```text
research_introductions/README.md
.github/pull_request_template.md
REVIEW_GUIDE.md
```

この演習では GitHub Actions は扱いません。Actions と Artifact は演習05で扱います。

## ブランチと PR の流れ

```text
main
  |
  o-------------------------o
   \                       ^
    develop                |
      |                    |
      o-------------o------+
       \           /
        feature/add-research-introduction-yourname
             Pull Request
```

Pull Request は、feature ブランチの変更を `develop` に取り込む前に、内容を説明し、他の人に確認してもらうための場所です。

## 手順

### 1. 共有リポジトリを clone する

`<GitHubリポジトリURL>` は、教員から指定されたグループ用共有リポジトリの URL に置き換えます。

このレッスンでは、01 で作成した授業用の親フォルダを使います。授業用の親フォルダで次を実行します。

```bash
git clone <GitHubリポジトリURL>
```

```bash
cd <リポジトリ名>
```

確認ポイント:

- グループ全員が同じ共有リポジトリを clone しているか確認します。
- ターミナルが clone したリポジトリの中にいる状態で、次に進みます。

### 2. 現在の状態を確認する

```bash
git status
```

確認ポイント:

- commit していない変更がないか確認します。
- 変更が残っている場合は、先に commit するか教員に相談してください。

### 3. develop ブランチに移動する

この演習では、教員が先に `develop` ブランチを用意している想定です。

まず、GitHub 側の branch 情報を取得します。

```bash
git fetch origin
```

次に、`develop` ブランチに移動します。

```bash
git switch develop
```

確認ポイント:

- `Switched to branch 'develop'` と表示されるか確認します。
- `Your branch is up to date with 'origin/develop'.` のように表示されれば、GitHub 上の `develop` とつながっています。

教員から `develop` ブランチを作成するように指示された場合だけ、次を実行します。

```bash
git switch -c develop
```

```bash
git push -u origin develop
```

確認ポイント:

- GitHub の branch 一覧に `develop` が表示されるか確認します。

### 4. feature ブランチを作成する

```bash
git switch -c feature/add-research-introduction-yourname
```

確認ポイント:

- `On branch feature/add-research-introduction-yourname` と表示されるか確認します。
- `yourname` の部分は、自分の名前に置き換えます。

### 5. 自分の研究紹介ファイルを作成する

VS Code で `research_introductions` フォルダを開き、自分用の Markdown ファイルを作成します。

ファイル名の例:

```text
research_introductions/yamada.md
```

公開できない研究内容は書かず、公開してよい範囲にします。必要であれば、架空の研究テーマでもかまいません。

書く内容の例:

```markdown
# 山田の研究紹介

## 研究テーマ

水質データを使った季節変化の分析

## 研究の目的

川の水質が季節によってどのように変化するかを調べます。

## 使うデータ

- 月ごとの水温
- 月ごとの pH
- 月ごとの観測地点

## まだ分からないこと

- 雨が多い時期に値がどのくらい変わるか
```

確認ポイント:

- 専門外の人にも読める短い説明にします。
- 個人情報、未公開データ、研究室の秘密情報は書きません。
- 1つの PR で扱うファイルは、基本的に自分の研究紹介ファイルだけにします。

### 6. 変更を commit する

```bash
git status
```

```bash
git add research_introductions/yamada.md
```

```bash
git commit -m "研究紹介を追加"
```

確認ポイント:

- `git log --oneline` で commit が増えているか確認します。

```bash
git log --oneline
```

### 7. feature ブランチを GitHub に push する

```bash
git push -u origin feature/add-research-introduction-yourname
```

確認ポイント:

- GitHub の画面に Pull Request 作成ボタンが表示されることがあります。
- branch 一覧に `feature/add-research-introduction-yourname` が表示されるか確認します。

### 8. GitHub 上で Pull Request を作成する

GitHub のリポジトリ画面で Pull Request を作成します。

設定例:

```text
base: develop
compare: feature/add-research-introduction-yourname
```

PR 本文には、`.github/pull_request_template.md` に沿って次の内容を書きます。

```text
変更内容:
- 自分の研究紹介ファイルを追加しました。

変更した理由:
- グループのメンバーに研究内容を説明し、レビューを受けるためです。

レビューしてほしい点:
- 専門外の人にも研究目的が分かるか確認してほしいです。
```

確認ポイント:

- `.github/pull_request_template.md` の内容が PR 本文に表示されるか確認します。
- 変更内容、確認したこと、レビューしてほしい点を書きます。
- PR の URL を作業記録に残します。

### 9. グループでレビュー担当を決める

2人組の場合は、お互いの Pull Request をレビューします。

3人組の場合は、時計回りにレビューします。

例:

```text
Aさんの PR は Bさんがレビューする
Bさんの PR は Cさんがレビューする
Cさんの PR は Aさんがレビューする
```

教室で座っている位置を使う場合は、最初に Aさん、Bさん、Cさんを決めてから、上の対応表に合わせます。

```text
Aさん → Bさん → Cさん → Aさん
```

確認ポイント:

- 全員が PR を作成します。
- 全員が1つ以上の PR をレビューします。
- レビュー担当が重ならないようにします。

### 10. レビューを行う

`REVIEW_GUIDE.md` を見ながら、担当する Pull Request を確認します。

GitHub 上で次の画面を開きます。

```text
Pull requests -> 担当する PR -> Files changed
```

レビューコメントの例:

```text
研究の目的が最初に書かれていて分かりやすいです。
```

```text
「使うデータ」に、データの単位や期間が1文あると、さらに読みやすくなりそうです。
```

```text
公開できない情報は含まれていないことを確認しました。
```

確認ポイント:

- 研究テーマが短く説明されているか
- 研究の目的が専門外の人にも分かるか
- 使うデータや方法が簡単に書かれているか
- 公開してはいけない情報が含まれていないか
- PR 本文にレビューしてほしい点が書かれているか

### 11. レビューコメントに対応する

修正が必要な場合は、同じ feature ブランチで追加修正します。

```bash
git status
```

```bash
git add research_introductions/yamada.md
```

```bash
git commit -m "レビュー指摘を反映"
```

```bash
git push
```

確認ポイント:

- 追加 commit が同じ Pull Request に反映されるか確認します。
- レビューコメントに返信して、どのように直したかを書きます。

### 12. Pull Request を merge する

教員の指示がある場合は、レビュー後に Pull Request を `develop` に merge します。

確認ポイント:

- `develop` ブランチに自分の研究紹介ファイルが追加されているか確認します。
- 授業で merge しない指示がある場合は、PR を開いた状態のままにします。

## 演習課題

3人組または2人組で、自分の研究紹介を Pull Request として提出し、相手の研究紹介をレビューします。

課題:

- 全員が新しい feature ブランチを作る
- 全員が `research_introductions/自分の名前.md` を作成する
- 全員が Pull Request を作成する
- 3人組では時計回りにレビューする
- 2人組ではお互いにレビューする
- レビュー担当者は、最低1つのコメントを書く
- PR 作成者は、コメントを受けて必要な修正を1回以上 commit する

ブランチ名の例:

```bash
git switch develop
```

```bash
git switch -c feature/add-research-introduction-yamada
```

ファイル名の例:

```text
research_introductions/yamada.md
```

commit の例:

```bash
git add research_introductions/yamada.md
```

```bash
git commit -m "研究紹介を追加"
```

push の例:

```bash
git push -u origin feature/add-research-introduction-yamada
```

この課題の主目的:

- Pull Request を「提出場所」ではなく、変更内容を説明して確認してもらう場所として体感すること
- レビューを通して、相手に伝わる研究説明に改善すること

## 期待される結果

- feature ブランチから Pull Request が作成される
- PR 本文テンプレートに沿って説明が書かれている
- 2人組または3人組でレビューコメントを付けられる
- 3人組では全員が PR 作成者とレビュー担当者の両方を体験する
- 必要に応じて追加 commit を同じ PR に反映できる

## よくあるエラー

### PR テンプレートが表示されない

確認すること:

```text
.github/pull_request_template.md
```

この場所にファイルがあるか確認します。

### `base` ブランチが分からない

この演習では、基本的に次の設定にします。

```text
base: develop
compare: feature/add-research-introduction-yourname
```

### push 後に PR 作成ボタンが見つからない

確認すること:

- GitHub の branch 一覧に `feature/add-research-introduction-yourname` があるか
- Pull requests タブから `New pull request` を押せるか
- `base` と `compare` の指定が正しいか

### ほかの人と同じファイルを編集してしまった

確認すること:

- 自分のファイル名になっているか確認します。
- 例: `research_introductions/yamada.md`
- ほかの人のファイルを変更した場合は、教員に相談してください。

### 研究内容をどこまで書いてよいか分からない

公開してよい範囲だけを書きます。迷う場合は、架空の研究テーマや授業用の例に置き換えます。

## 提出物

- Pull Request の URL
- PR 本文に書いた変更説明
- レビューコメントの内容
- 追加修正した場合は、その commit の URL
- 授業中の作業記録

## 振り返り質問

- Pull Request を作ると、どのような確認がしやすくなりますか。
- PR 本文には何を書くとレビューしやすくなりますか。
- レビューコメントを書くとき、どのような表現が相手に伝わりやすいですか。
- 自分の研究紹介は、レビュー後にどこが分かりやすくなりましたか。

## 提出前チェックリスト

- [ ] グループ用共有リポジトリを clone した
- [ ] `develop` ブランチに移動した
- [ ] 自分用の feature ブランチを作成した
- [ ] `research_introductions/自分の名前.md` を作成した
- [ ] 変更を commit した
- [ ] feature ブランチを GitHub に push した
- [ ] Pull Request を作成した
- [ ] PR 本文テンプレートを記入した
- [ ] 3人組の場合は時計回りにレビュー担当を決めた
- [ ] レビュー観点に沿って確認した
- [ ] レビューコメントに対応した
- [ ] Pull Request URL を作業記録に書いた
