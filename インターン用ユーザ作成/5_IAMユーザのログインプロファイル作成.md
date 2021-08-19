<!-- omit in toc -->
# ログインプロファイル作成

ユーザのログイン用プロファイルを作成する。  
初期パスワードはランダムな文字列を設定し、初回ログイン時に変更が必要なものとする。  
生成した初期パスワードは1.4にて指定するフォルダのユーザ名のファイル内に保存する。

## 1. 変数の指定

### 1.1. リージョンの指定

    export AWS_DEFAULT_REGION='ap-northeast-1'

### 1.2. ユーザ名

    IAM_USER_NAME='interntestuser'

### 1.3. ユーザのデフォルトパスワード長

    IAM_USER_PASSWORD_LENGTH=15

### 1.4. パスワードの保存ディレクトリ

    DIR_IAM_PASSWORD_DOC="${HOME}/environment/conf-iam/temp"

## 2. メインの処理

### 2.1. ディレクトリの存在確認

    ls -d ${DIR_IAM_PASSWORD_DOC}

ディレクトリが存在しない場合次項番を実施する。

#### 2.1.1. ディレクトリ作成

    mkdir -p ${DIR_IAM_PASSWORD_DOC}

### 2.2. ユーザのログインパスワード生成

    USER_PASSWORD=$( \
        aws secretsmanager get-random-password \
            --password-length ${IAM_USER_PASSWORD_LENGTH} \
            --output text \
    ) \
        && echo ${USER_PASSWORD}

### 2.3. ドキュメントファイル名

    FILE_IAM_PASSWORD_DOC="${DIR_IAM_PASSWORD_DOC}/${IAM_USER_NAME}" \
        && echo ${FILE_IAM_PASSWORD_DOC}

### 2.4. パスワードを一時ファイルに保存

    cat << EOF > ${FILE_IAM_PASSWORD_DOC}
    ${USER_PASSWORD}
    EOF

    cat ${FILE_IAM_PASSWORD_DOC}

### 2.5. ユーザのログインプロファイル作成

    aws iam create-login-profile \
        --user-name ${IAM_USER_NAME} \
        --password ${USER_PASSWORD} \
        --password-reset-required

## 3. 確認

### 3.1. ユーザのログインプロファイルの存在確認

ユーザ名[interntestuser]が出力されることを確認する

    aws iam get-login-profile \
        --user-name ${IAM_USER_NAME} \
        --query 'LoginProfile.UserName' \
        --output text
