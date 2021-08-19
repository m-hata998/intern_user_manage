<!-- omit in toc -->
# ログインプロファイル削除

ユーザのログイン用プロファイルを削除する。

## 1. 変数の指定

### 1.1. ディレクトリ指定

    DIR_USER_LIST_DOC="${HOME}/environment/userlist"

### 1.2. ドキュメント名

    USER_LIST_DOC_NAME='users'

## 2. メインの処理

### 2.1. ユーザリストドキュメントファイル名

    FILE_USER_LIST_DOC="${DIR_USER_LIST_DOC}/${USER_LIST_DOC_NAME}.txt" \
        && echo ${FILE_USER_LIST_DOC}

### 2.2. ユーザのログインプロファイル削除

    cat ${FILE_USER_LIST_DOC} | while read user
    do
        aws iam delete-login-profile \
            --user-name ${user}
    done

## 3. 確認

### 3.1. ユーザのログインプロファイルの存在確認

ログインプロファイルが存在しないエラー[Login Profile for User ユーザ名 cannot be found.]が出力されることを確認する

    cat ${FILE_USER_LIST_DOC} | while read user
    do
        ! aws iam get-login-profile \
            --user-name ${user} \
            --query 'LoginProfile.UserName' \
            --output text
    done
