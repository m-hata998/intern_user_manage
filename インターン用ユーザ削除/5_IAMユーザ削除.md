<!-- omit in toc -->
# ユーザ削除

ユーザを作成する。

## 1. 変数の指定

### 1.1. ユーザ名

    IAM_USER_NAME='interntestuser'

## 2. メインの処理

### 2.1. ユーザの削除

    aws iam delete-user \
        --user-name ${IAM_USER_NAME} 

## 3. 確認

### 3.1. ユーザの存在確認

出力がないことを確認する。

    aws iam list-users \
        --query "Users[?UserName == \`${IAM_USER_NAME}\`].UserName" \
        --output text
