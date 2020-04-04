# blog-hugo
- Hugoで作ったblogの元ファイル.
- 出来上がり: https://github.com/solareenlo/solareenlo.github.io
- デプロイしたサイト: https://solareenlo.com
- Dockerfile: https://github.com/solareenlo/docker-alpine-hugo
- Hugo Theme: https://github.com/alex-shpak/hugo-book

## Usage

### ローカルで走らせる(docker編)
```bash
docker run --rm -it -v $(PWD):/src -p 1313:1313 peaceiris/hugo server --bind=0.0.0.0
docker run --rm -it -v $(PWD):/src -p 1313:1313 peaceiris/hugo new site test
```

Reference: https://github.com/peaceiris/hugo-extended-docker

### ローカルで走らせる(docker-compose編)
```bash
git clone --recurse-submodules git@github.com:solareenlo/blog-hugo.git
cd blog-hugo
# docker-composeで走らせる.
docker-compose up -d
# localhost:1313を任意のブラウザで確認する.
# docker-composeを止める.
docker-compose down
```

### ローカルで走らせる(hugo server編)
```bash
git clone --recurse-submodules git@github.com:solareenlo/blog-hugo.git
cd blog-hugo
hugo server -D --cleanDestinationDir
# localhost:1313を任意のブラウザで確認する.
```

### 記事を追加する
- ドキュメントを追加するには`content`->`docs`
- 作成したドキュメントをメニューに追加するには`content`->`menu`
- blogを追加するには`content`->`posts`

### github pages にデプロイする
```bash
# For Mac
brew update
brew upgrade
brew install hugo
bash deploy.sh
```
