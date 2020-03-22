# hoge.ldifの解説
ユーザー`hoge`の作成に用いる。

## 準備
### LDAPサーバーを構築
`base.md`を読みながら、`base.ldif`を適用する。

## グループの登録
`hoge.ldif`にはユーザー`hoge`の情報が記述されている。`hoge`は`5th`に所属している設定で、`ou=5th,ou=people`の下に`uid=hoge`を作成する例である。ユーザープライベートグループとして`ou=5th,ou=groups`の下に`cn=hoge`も作成する。`5th`の1番なので、`uid`,`gid`はともに10501としている。(LDAPユーザーは10000以降としてローカルユーザーと差別化)

### cn
Common Name:ログイン画面に表示される

### sn
Sir Name:姓

### gn
Given Name:名

### loginShell
`loginShell`を指定しないと、`SHELL=/bin/sh`となってしまう。

### homeDirectory
`homeDirectory`を指定しないと、GUIではログインできずshellからしかログインできない。NFSマウントしつつローカルユーザーと差別化する都合上、`/homes/hoge`としている。

### userPassword
`userPassword`を設定しないと、ログインできない。平文で記述して`ldapadd`すると、自動でハッシュ化されて`slapcat`しても見えなくなる。初回ログイン時に必ず`passwd`で変更する。

### memberUid
グループに所属するメンバーを指定する。

### ldapaddコマンドで登録
```
ldapadd -x -D cn=admin,dc=uken,dc=local -W -f hoge.ldif
```
用例は`base.md`にあるものと同じ。
