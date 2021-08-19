<!-- omit in toc -->
# ログインプロファイル削除

ユーザのログイン用プロファイルを削除する。

## 1. 変数の指定

### 1.1. ユーザ名

    IAM_USER_NAME='interntestuser'

## 2. メインの処理

### 2.1. ユーザのログインプロファイル削除

    aws iam delete-login-profile \
        --user-name ${IAM_USER_NAME}

## 3. 確認

### 3.1. ユーザのログインプロファイルの存在確認

ログインプロファイルが存在しないエラー[Login Profile for User interntestuser cannot be found.]が出力されることを確認する

    ! aws iam get-login-profile \
        --user-name ${IAM_USER_NAME} \
        --query 'LoginProfile.UserName' \
        --output text
