<!-- omit in toc -->
# IAMポリシーの作成

ポリシーを作成する。

## 1. 変数の指定

### 1.1. ポリシー名

    IAM_POLICY_NAME='internship-group-policy'

### 1.2. ポリシーパス

    IAM_POLICY_PATH='/internship-cli/'

### 1.3. ポリシーの説明

    IAM_POLICY_DESCRIPTION='Policy for internship user group.'

### 1.4. ディレクトリ指定

    DIR_IAM_POLICY_DOC="${HOME}/environment/conf-iam"

### 1.5. ドキュメント名

    IAM_POLICY_DOC_NAME='internship-user-policy'

## 2. メインの処理

### 2.1. ポリシードキュメントパス

    FILE_IAM_POLICY_DOC="${DIR_IAM_POLICY_DOC}/${IAM_POLICY_DOC_NAME}.json" \
        && echo ${FILE_IAM_POLICY_DOC}

### 2.2. ポリシー作成

    aws iam create-policy \
        --policy-name ${IAM_POLICY_NAME} \
        --path "${IAM_POLICY_PATH}" \
        --policy-document file://${FILE_IAM_POLICY_DOC} \
        --description "${IAM_POLICY_DESCRIPTION}"

## 3. 確認

### 3.1. ポリシーの存在確認

ポリシー名[internship-group-policy]が表示されることを確認する。

    aws iam list-policies \
        --scope Local \
        --path-prefix "${IAM_POLICY_PATH}" \
        --max-items 1000 \
        --query "Policies[?PolicyName == \`${IAM_POLICY_NAME}\`].PolicyName" \
        --output text
