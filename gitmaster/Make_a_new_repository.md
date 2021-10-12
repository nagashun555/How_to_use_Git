# 新しいリポジトリの作り方

参照:[Github に新規 repository を追加](https://qiita.com/sodaihirai/items/caf8d39d314fa53db4db)

- 前提
  - github のアカウントは 作成済み
- 環境
  - windows10

## はじめに

- [キャッシュファイル読み込み無効化](https://note.com/masato1230/n/na63ac4e7ccdd)
- [キャッシュの削除](https://qiita.com/fuwamaki/items/3ed021163e50beab7154)
- github アカウントを作る。
- git のインストールする。

## 1.git init でセットアップ

        git init

## 2.git add -A でインデックスにファイルを追加

        git add -A

## 3.git commit -m で変更を反映

        git commit -m "Initialize repository"

## 4.github で新規レポジトリを作成

Repository name を決めて Create repository

## 5.github のリモートリポジトリ情報を追加する

        git remote add origin [リモートリポジトリ情報(URL)]

## 6.gitpush でローカルリポジトリの変更をリモートリポジトリに反映する

        git push origin master
