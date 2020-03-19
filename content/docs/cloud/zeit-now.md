# ZEIT Nowとは
- サーバーレスデプロイのためのクラウドプラットフォームサービスのこと.

## サブドメインの割り当て方
1. まずはメインドメインをNowに登録する.
  ```bash
  now domains add iotajapan.com
  ```

2. サブドメインのエイリアスを登録しようとする.
  ```bash
  # 以下を実行すると,
  now alias https://my-docs.test.now.sh docs.iotajapan.com
  # 以下のようなエラーが返ってくる.
  Error! We could not alias since the domain iotajapan.com could not be verified due to the following reasons:
 a) Nameservers verification failed since we see a different set than the intended set:
      Intended Nameservers    Current Nameservers
      c.zeit-world.co.uk      01.dnsv.jp             ☓
      d.zeit-world.org        02.dnsv.jp             ☓
      e.zeit-world.com        03.dnsv.jp             ☓
      f.zeit-world.net        04.dnsv.jp             ☓
 b) DNS TXT verification failed since found no matching records.
      name        type        value
      _now        TXT         abc...xyz
 Once your domain uses either the nameservers or the TXT DNS record from above, run again `now domains verify <domain>`.
   We will also periodically run a verification check for you and you will receive an email once your domain is verified.
   Read more: https://err.sh/now-cli/domain-verification
   ```

3. お名前.comのNameserverを使用するとして, 返ってきたエラーの内容でDNS TXTレコードを登録する.

    | ホスト名 | TYPE | TTL | VALUE |
    | -------- | ---- | --- | ----- |
    | _now.iotajapan.com | TXT | 3600 | abc...xyz |

4. ドメインの所有者認証を確かめる.
  ```bash
  # 以下を実行し,
  now domains veryfy iotajapan.com
  # 以下のように返ってきたら認証されてる.
  Success! Domain iotajapan.com was verified using DNS TXT record. [500ms]
  You can verify with nameservers too. Run `now domains inspect iotajapan.com` to find out the intended set.
  ```

5. DNS CNAMEレコードを登録する.

    | ホスト名 | TYPE | TTL | VALUE |
    | -------- | ---- | --- | ----- |
    | docs.iotajapan.com | CNAME | 3600 | alias.zeit.co |

6. `now.json`を以下のように設定する.
  ```json
  {
    "version": 2,
    "alias": "docs.iotajapan.com"
  }
  ```

7. nowにデプロイする.
  ```bash
  now --target production
  ```

8. References
  - [Now インスタンスに独自ドメインを設定する](https://blog.mktia.com/how-to-add-a-custom-domain-to-now/)
  - [ZEITのnow(v2)でデプロイしたらドメインaliasを最新のデプロイに追従させる](https://tech-1natsu.hatenablog.com/entry/2019/04/24/121251?utm_source=feed)
