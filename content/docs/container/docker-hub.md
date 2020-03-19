# Docker Hubとは
- コンテナイメージをビルドしたり配布したりする場所.
- **公式サイト:** https://hub.docker.com

## 配布方法
### イメージをそのままpush
```bash
# 先ずはDocker Hubにログインする
docker login
# 自分の名前でtag付けしてイメージを作成する
docker image build -t solareenlo/test .
# そして, Docker Hubにpushする
docker push solareenlo/test
```

### GitHubからDockerfileをpush
