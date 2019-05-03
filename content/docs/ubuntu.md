# Ubuntuとは
コミュニティにより開発されているオペレーティングシステムのこと.
無料で提供されている.
Linuxディストリビューションの1つ.

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
