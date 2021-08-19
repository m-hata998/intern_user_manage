<!-- omit in toc -->
# ユーザ作成

ユーザを作成する。

## 1. 変数の指定

### 1.1. ユーザ名

    IAM_USER_NAME='interntestuser'

### 1.2. ユーザパス

    IAM_USER_PATH='/internship-user/'

## 2. メインの処理

### 2.1. ユーザの作成

    aws iam create-user \
        --user-name ${IAM_USER_NAME} \
        --path ${IAM_USER_PATH}

## 3. 確認

### 3.1. ユーザの存在確認

ユーザ名[interntestuser]が出力されることを確認する

    aws iam list-users \
        --query "Users[?UserName == \`${IAM_USER_NAME}\`].UserName" \
        --output text
