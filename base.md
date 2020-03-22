# base.ldifの解説
ユーザーとグループ用の組織単位の新規作成に用いる。

## 準備
### OpenLDAPサーバーを構築する
Debianなら`slapd`,`ldap-utils`,`nscd`,`nslcd`をインストールする。
```
sudo apt install slapd ldap-utils nscd nslcd
```

configureが走るので、設定する。
ここでは`dc=uken,dc=local`とした。
管理者パスワードを忘れないようにすること。

```
slapcat
```
で設定確認。

## 組織単位の登録
`base.ldif`には組織単位の情報が記述されている。
ユーザーを格納する`ou=people`とグループを格納する`ou=groups`の下に学年別の`ou=*th`と教員用`ou=teacher`,ゲスト用`ou=guest`を作成するldifファイルの例である。

### ldapaddコマンドで登録
```
ldapadd -x -D cn=admin,dc=uken,dc=local -W -f base.ldif
```
-Wでパスワードを要求、-fでldifファイルを指定。大文字小文字は区別すること。パスワードを尋ねられるので、設定したパスワードを入力する。
