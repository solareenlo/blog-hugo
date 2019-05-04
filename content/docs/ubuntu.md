# Ubuntuとは
コミュニティにより開発されているオペレーティングシステムのこと.
無料で提供されている.
Linuxディストリビューションの1つ.

## git補完
**Reference:** [「Git補完をしらない」「git statusを1日100回は使う」そんなあなたに朗報【git-completionとgit-prompt】](https://qiita.com/varmil/items/9b0aeafa85975474e9b6)
### `git-completion.bash`をインストール
`git-completion.bash`: gitコマンドの補完スクリプト.
Tabで保管できる.
```bash
wget https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash -O ~/.git-completion.bash
chmod a+x ~/.git-completion.bash
echo "source ~/.git-completion.bash" >> ~/.bashrc
source ~/.bashrc
```

### `git-prompt.sh`のインストール
`git-prompt.sh`: プロンプトに各種追加情報を表示可能にするスクリプト.
```bash
wget https://raw.githubusercontent.com/git/git/master/contrib/completion/git-prompt.sh -O ~/.git-prompt.sh
chmod a+x ~/.git-prompt.sh
```
下記を一度にコピペしてエンターを押す.
```bash
cat << EOF | tee -a ~/.bashrc > /dev/null
source ~/.git-prompt.sh
GIT_PS1_SHOWDIRTYSTATE=true
GIT_PS1_SHOWSTASHSTATE=true
GIT_PS1_SHOWUNTRACKEDFILES=true
GIT_PS1_SHOWUPSTREAM="auto"
GIT_PS1_SHOWCOLORHINTS=true
EOF
```
```bash
source ~/.bashrc
```

## ターミナルの色を変える
`~/.bashrc`の当該部分を以下の様に書き換える.
```bash
if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\[\033[01;31m\]$(__git_ps1)\[\033[00m\]\n\[\033[01;35m\]-> \$\[\033[00m\] '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
```

## 再起動
```bash
sudo shutdown -r now
// or
sudo reboot
```

## mountしているSSDのlabel名を変更する
Ubuntuのディスクプリで当該のSSDをmountする.
```bash
df -Th
> Filesystem     Type      Size  Used Avail Use% Mounted on
> /dev/nvme2n1p1 ext4      234G   61M  222G   1% /media/solareenlo/ssd021
```
でSSDのFilesystem名(この場合は`/dev/nvme2n1p1`)を確認する.

```bash
sudo umount /dev/nvme2n1p1
```
で一度SSDをunmountする.

SSDのTypeがext2/ext3/ext4の場合は,
```bash
sudo e2label /dev/nvme2n1p1 ssd02
```
とし, label名を`ssd021`から`ssd02`に変更する.

そして, Ubuntuのディスクアプリで当該のSSDをmountする.

## snapでのupdate
snapとはLinuxのパッケージ管理システムの1つ.
```bash
snap refresh
```

## バージョン確認
```bash
cat /etc/os-release
```
