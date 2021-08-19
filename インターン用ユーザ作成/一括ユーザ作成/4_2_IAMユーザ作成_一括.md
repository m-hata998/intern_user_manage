<!-- omit in toc -->
# ユーザ作成

ユーザを一括で作成する。

## 1. 変数の指定

### 1.1. ユーザパス

    IAM_USER_PATH='/internship-user/'

### 1.2. ディレクトリ指定

    DIR_USER_LIST_DOC="${HOME}/environment/userlist"

### 1.3. ドキュメント名

    USER_LIST_DOC_NAME='users'

## 2. メインの処理

### 2.1. ドキュメントファイル名

    FILE_USER_LIST_DOC="${DIR_USER_LIST_DOC}/${USER_LIST_DOC_NAME}.txt" \
        && echo ${FILE_USER_LIST_DOC}

### 2.2. ユーザの作成

    cat ${FILE_USER_LIST_DOC} | while read user
    do
        aws iam create-user \
            --user-name ${user} \
            --path ${IAM_USER_PATH}
    done

## 3. 確認

### 3.1. ユーザの存在確認

ユーザリストに記載した全ユーザが出力されることを確認する

    cat ${FILE_USER_LIST_DOC} | while read user
    do
        aws iam list-users \
            --query "Users[?contains(UserName,\`${user}\`)].[UserName]" \
            --output text
    done
