<!-- omit in toc -->
# IAMユーザをグループから削除

IAMユーザをグループから削除する。

## 1. 変数の指定

### 1.1. グループ名

    IAM_GROUP_NAME='kensyuGroup'

### 1.2. ディレクトリ指定

    DIR_USER_LIST_DOC="${HOME}/environment/userlist"

### 1.3. ドキュメント名

    USER_LIST_DOC_NAME='users'

## 2. メインの処理

### 2.1. ユーザリストドキュメントファイル名

    FILE_USER_LIST_DOC="${DIR_USER_LIST_DOC}/${USER_LIST_DOC_NAME}.txt" \
        && echo ${FILE_USER_LIST_DOC}

### 2.2. ユーザの削除

    cat ${FILE_USER_LIST_DOC} | while read user
    do
        aws iam remove-user-from-group \
            --group-name ${IAM_GROUP_NAME} \
            --user-name ${user}
    done

## 3. 確認

### 3.1. グループの設定確認確認

出力がないことを確認する。

    cat ${FILE_USER_LIST_DOC} | while read user
    do
        aws iam list-groups-for-user \
            --user-name ${user} \
            --query "Groups[?GroupName==\`${IAM_GROUP_NAME}\`].GroupName" \
            --output text
    done
