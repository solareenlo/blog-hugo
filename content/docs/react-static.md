# React Staticとは
- React Staticは, Reactとそのエコシステムをベースにした, 高速, 軽量, そして強力なプログレッシブ静的サイトジェネレータ.
- これは, Create React Appなどのツールで慣れていたシンプルさと開発者の経験に似ており, パフォーマンス, 柔軟性, およびユーザー/開発者の経験のために慎重に設計されている.
- **GitHubリポジトリ:** [nozzle/react-static](https://github.com/nozzle/react-static/tree/master/)

## エラー処理
###  `Error: ENOSPC: System limit for number of file watchers reached, watch`と出たら
```bash
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```
