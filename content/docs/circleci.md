# CircleCIとは
- クラウド上でCI/CDを行なってくれるサービスの1つ.
- **公式サイト:** https://circleci.com

## docs
- [CircleCI Documentation](https://github.com/circleci/circleci-docs)

## AWSのS3へデプロイ
- [Nuxt.js+CircleCIで静的ページをAWSのS3へデプロイする](https://qiita.com/noplan1989/items/21af46b4101392ffc666)

## AWSのElastic Beanstalkへデプロイ
1. 以下で`CircleCI`で`Elastic Beanstalk CLI`を起動して, デプロイする.

    ```yaml
    # .circleci/config.yml
    version: 2
    jobs:
      deploy:
        working_directory: ~/app
        docker:
          - image: circleci/ruby:2.6.3
        steps:
          - checkout
          - run:
              name: Installing deployment dependencies
              working_directory: /
              command: |
                sudo apt-get -y -qq update
                sudo apt-get install python-pip python-dev build-essential
                sudo pip install --upgrade setuptools
                sudo pip install awsebcli --upgrade
          - run:
              name: Deploying
              command: eb deploy master-TaxPlusAws01-env

    workflows:
      version: 2
      build:
        jobs:
          - deploy:
              filters:
                branches:
                  only:
                    - master
    ```
- 以下は`Elastci Beanstalk CLI`の設定.

    ```yaml
    # .elasticbeanstalk/config.yml
    branch-defaults:
      master:
        environment: master-TaxPlusAws01-env
    global:
      application_name: tax-plus-aws-01
      default_ec2_keyname: tax-plus
      default_platform: 64bit Amazon Linux 2018.03 v4.8.2 running Node.js
      default_region: ap-northeast-1
      sc: git
    ```
- そして, `CircleCI`の`Project Settings > Environment Variables`で`AWS_ACCESS_KEY_ID`と`AWS_SECRET_ACCESS_KEY`を設定する.
- **Reference:**
  - [Deploying to Elastic Beanstalk via CircleCi 2.0](https://gist.github.com/ryansimms/808214137d219be649e010a07af44bad)
  - https://github.com/solareenlo/tax-plus-aws

## 参考サイト
- [CircleCI 2.1の機能を使ってインフラの"ほぼ"全自動構成管理をやってみた話](https://speakerdeck.com/inductor/circleci-terraform)
- [CircleCI 2.1 の新機能を使って冗長な config.yml をすっきりさせよう！](https://tech.recruit-mp.co.jp/dev-tools/post-14868/)
