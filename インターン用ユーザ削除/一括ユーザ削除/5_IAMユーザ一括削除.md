<!-- omit in toc -->
# ユーザ削除

ユーザを作成する。

## 1. 変数の指定

### 1.1. ディレクトリ指定

    DIR_USER_LIST_DOC="${HOME}/environment/userlist"

### 1.2. ドキュメント名

    USER_LIST_DOC_NAME='users'

## 2. メインの処理

### 2.1. ユーザリストドキュメントファイル名

    FILE_USER_LIST_DOC="${DIR_USER_LIST_DOC}/${USER_LIST_DOC_NAME}.txt" \
        && echo ${FILE_USER_LIST_DOC}

### 2.2. ユーザの削除

    cat ${FILE_USER_LIST_DOC} | while read user
    do
        aws iam delete-user \
            --user-name ${user} 
    done

## 3. 確認

### 3.1. ユーザの存在確認

出力がないことを確認する。

    cat ${FILE_USER_LIST_DOC} | while read user
    do
        aws iam list-users \
            --query "Users[?UserName==\`${user}\`].UserName" \
            --output text
    done
