# [Git](https://git-scm.com)とは
プログラムのソースコードなどの変更履歴を記録・追跡するための分散型バージョン管理システムのこと.
とっても便利.
慣れてしまうともう後戻りはできない.

## 新しいブランチの作成
### ローカルブランチからブランチ作成
```bash
cd ディレクトリ // 作業ディレクトリに移動
git branch -a // branchの一覧を表示
git checkout master // master branchに切り替え
git checkout -b 作成するbranch名 // masterを元にして新しいbranchを作成
git branch -a
git push -u origin 作成したbranch名 // branchをremoteに登録
```

### リモートブランチからブランチ作成
```bash
cd ディレクトリ
git checkout -b ローカルに作成するブランチ名 origin/再生元のリモートブランチ名
git branch -a
```


## ブランチの削除
```bash
cd 作業ティレクトり
git checkout -b master // 削除したいブランチとは違うブランチにまずは移動する
git checkout -d 削除したいブランチ名
```
