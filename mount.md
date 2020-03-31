# mount.ldifの解説

マウント情報の設定に用いる。



## 準備

### LDAPサーバーを構築

`base.md`を読みながら、`base.ldif`を適用する。

### NFSサーバーを構築

パッケージ`nfs-kernel-server`をインストールする。

`/etc/exports`を書き換え、共有の設定をする。この例では、`/home/ldap`と`/home/share`を起用有するように設定した。

### autofsとLDAPの連携

パッケージ`autofs-ldap`をインストールする。

`/etc/ldap/schema/autofs.schema`が生成されるので、ldifファイルに変換してサーバーに読ませる。`/etc/ldap/slapd.d/cn=config/cn=schema/autofs.ldif`の所有者を`openldap:openldap`に変更すること。

## マウント情報の登録

`mount.ldif`にはマウント情報が記述されている。`dc=uken,dc=local`の下に`ou=automount`を作成し、その中に`ou=auto.master`,`ou=auto.home`,`ou=auto.share`を作成する。

`ou=auto.master`において、`cn`はマウントポイントになり、`automountInformation`はマウントマップの在り処を表す。

`ou=auto.home`及び`ou=auto.share`において、`cn=*`,`automountInformation`の最後を`/hoge/&`にすると、`/hoge`内の全てのサブディレクトリがマウント対象になる。

### ldapaddコマンドで登録

```bash
ldapadd -x -D cn=admin,dc=uken,dc=local -W -f mount.ldif
```

用例は`base.md`にあるものと同じ。