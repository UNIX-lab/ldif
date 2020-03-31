# sudoers.ldifの解説

sudoersの設定に用いる

## 準備

### LDAPサーバーの構築

`base.md`を読みながら、`base.ldif`を適用する。`hoge.ldif` , `groups.ldif`も適用する。

### sudoとLDAPの連携

パッケージ`sudo-ldap`をインストールする。

`/usr/share/doc/sudo-ldap/schema.OpenLDAP`が生成されるので、ldifファイルに変換してサーバーに読み込ませる。このままでは分かりにくいので、`/etc/ldap/schema/sudoers.schema`にcpするとよい。するとよい。`/etc/ldap/slapd.d/cn=config/cn=schema/sudoers.ldif`の所有者を`openldap:openldap`に変更すること。

## sudoersの登録

`sudoers.ldif`にはsudoersの情報が記述されている。`dc=uken,dc=local`の下に`ou=sudoers`を作成し、その中に`cn=default`と`cn=admin`を作成する。

### sudoUser

そのルールを適用するユーザーを指定する。`ALL`とすれば全員になる。`%[グループ]`とすればグループで指定できる。

### sudoCommand

そのルールを適用するコマンドを指定する。`cn=default`では`/sbin/shutdown`を指定し全員がシャットダウンできるように、`cn=admin`では`ALL`としグループ`admin`に所属するユーザーが全てのコマンドを実行できるようにしている。

### sudoHost

基本的には`ALL`でよい。

### ldapaddコマンドで登録

```bash
ldapadd -x -D cn=admin,dc=uken,dc=local -W -f sudoers.ldif
```

用例は`base.md`にあるものと同じ。

