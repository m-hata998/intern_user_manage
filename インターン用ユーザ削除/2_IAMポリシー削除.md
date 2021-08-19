<!-- omit in toc -->
# IAMポリシーの削除

ポリシーを削除します。

## 1. 変数の指定

### 1.1. ポリシー名

    IAM_POLICY_NAME='internship-group-policy'

### 1.2. ポリシーパス

    IAM_POLICY_PATH='/internship-cli/'

## 2. メインの処理

### 2.1. IAMポリシーのARN取得

    IAM_POLICY_ARN=$( \
        aws iam list-policies \
            --path-prefix ${IAM_POLICY_PATH} \
            --query "Policies[?PolicyName==\`${IAM_POLICY_NAME}\`].Arn" \
            --output text
    ) \
        && echo ${IAM_POLICY_ARN}

### 2.2. ポリシーの過去のバージョン削除

デフォルト以外のポリシーのバージョンを削除します。

    aws iam list-policy-versions \
        --policy-arn ${IAM_POLICY_ARN} \
        --query "Versions[?IsDefaultVersion == \`false\`].[VersionId]" \
        --output text |  \
        xargs -I {} -P 4 \
            aws iam delete-policy-version \
                --policy-arn ${IAM_POLICY_ARN} \
                --version-id "{}"

### 2.3. ポリシー削除

    aws iam delete-policy \
        --policy-arn ${IAM_POLICY_ARN}

## 3. 確認

### 3.1. ポリシーの存在確認

出力がないことを確認する。

    aws iam list-policies \
        --scope Local \
        --path-prefix "${IAM_POLICY_PATH}" \
        --max-items 1000 \
        --query "Policies[?PolicyName == \`${IAM_POLICY_NAME}\`].PolicyName" \
        --output text
