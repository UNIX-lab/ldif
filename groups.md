# groups.ldifの解説
グループ作成に用いる。

## 準備
### LDAPサーバーを構築
`base.md`を読みながら、`base.ldif`を適用する。

## グループの登録
`group.ldif`にはグループの情報が記述されている。
`ou=*th`の下に各学年のグループ`cn=*th`,`ou=teacher`の下に教員用のグループ`cn=teacher`,`ou=groups`直下に全員が所属する`cn=uken`とroot権限を持つ(sudoできる)`cn=admin`を作成するldifファイルの例である。

### ldapaddコマンドで登録
```
ldapadd -x -D cn=admin,dc=uken,dc=local -W -f groups.ldif
```
用例は`base.md`にあるものと同じ。
