<!-- omit in toc -->
# IAMユーザをグループに追加

IAMユーザをグループに追加する。

## 1. 変数の指定

### 1.1. ユーザ名

    IAM_USER_NAME='interntestuser'

### 1.2. グループ名

    IAM_GROUP_NAME='kensyuGroup'

## 2. メインの処理

### 2.1. IAMユーザをグループに追加

    aws iam add-user-to-group \
        --group-name ${IAM_GROUP_NAME} \
        --user-name ${IAM_USER_NAME}

## 3. 確認

### 3.1. グループの設定確認

グループ名[kenshuGroup]が存在することを確認する。

    aws iam list-groups-for-user \
        --user-name ${IAM_USER_NAME} \
        --query "Groups[?GroupName==\`${IAM_GROUP_NAME}\`].GroupName" \
        --output text
