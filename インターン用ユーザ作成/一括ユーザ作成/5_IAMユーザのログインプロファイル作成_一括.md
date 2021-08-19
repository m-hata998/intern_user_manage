<!-- omit in toc -->
# ログインプロファイル作成

ユーザのログイン用プロファイルを作成する。
初期パスワードはランダムな文字列を設定し、初回ログイン時に変更が必要なものとする。

## 1. 変数の指定

### 1.1. リージョンの指定

    export AWS_DEFAULT_REGION='ap-northeast-1'

### 1.2. ディレクトリ指定

    DIR_USER_LIST_DOC="${HOME}/environment/userlist"

### 1.3. ドキュメント名

    USER_LIST_DOC_NAME='users'

### 1.4. ユーザのデフォルトパスワード長

    IAM_USER_PASSWORD_LENGTH=15

### 1.5. パスワードの保存ディレクトリ

    DIR_IAM_PASSWORD_DOC="${HOME}/environment/userlist/temp"

## 2. メインの処理

### 2.1. パスワード保存ディレクトリの存在確認

    ls -d ${DIR_IAM_PASSWORD_DOC}

ディレクトリが存在しない場合次項番を実施する。

#### 2.1.1. ディレクトリ作成

    mkdir -p ${DIR_IAM_PASSWORD_DOC}

### 2.2. ユーザリストドキュメントファイル名

    FILE_USER_LIST_DOC="${DIR_USER_LIST_DOC}/${USER_LIST_DOC_NAME}.txt" \
        && echo ${FILE_USER_LIST_DOC}

### 2.3. ユーザのログインパスワード生成

    cat ${FILE_USER_LIST_DOC} | while read user
    do
        USER_PASSWORD=$( \
            aws secretsmanager get-random-password \
                --password-length ${IAM_USER_PASSWORD_LENGTH} \
                --output text
        )
        FILE_IAM_PASSWORD_DOC="${DIR_IAM_PASSWORD_DOC}/${user}"
        echo ${USER_PASSWORD} > ${FILE_IAM_PASSWORD_DOC}
    done

### 2.4. ユーザのログインプロファイル作成

    cat ${FILE_USER_LIST_DOC} | while read user
    do
        FILE_IAM_PASSWORD_DOC="${DIR_IAM_PASSWORD_DOC}/${user}"
        USER_PASSWORD=$( \
            cat ${FILE_IAM_PASSWORD_DOC}
        )
        aws iam create-login-profile \
            --user-name ${user} \
            --password ${USER_PASSWORD} \
            --password-reset-required
    done

## 3. 確認

### 3.1. ユーザのログインプロファイルの存在確認

ユーザ名[interntestuser]がユーザの人数分出力されることを確認する

    cat ${FILE_USER_LIST_DOC} | while read user
    do
        aws iam get-login-profile \
            --user-name ${user} \
            --query 'LoginProfile.UserName' \
            --output text
    done
