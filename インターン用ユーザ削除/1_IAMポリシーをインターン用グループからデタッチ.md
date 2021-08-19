<!-- omit in toc -->
# IAMポリシーのグループへの付与

ポリシーを作成します。

## 1. 変数の指定

### 1.1. ポリシー名

    IAM_POLICY_NAME='internship-group-policy'

### 1.2. ポリシーパス

    IAM_POLICY_PATH='/internship-cli/'

### 1.3. グループ名

    IAM_GROUP_NAME='kensyuGroup'

## 2. メインの処理

### 2.1. IAMポリシーのARN取得

    IAM_POLICY_ARN=$( \
        aws iam list-policies \
            --path-prefix ${IAM_POLICY_PATH} \
            --query "Policies[?PolicyName==\`${IAM_POLICY_NAME}\`].Arn" \
            --output text
    ) \
        && echo ${IAM_POLICY_ARN}

### 2.2. ポリシーデタッチ

    aws iam detach-group-policy \
        --group-name ${IAM_GROUP_NAME} \
        --policy-arn ${IAM_POLICY_ARN}

## 3. 確認

### 3.1. ポリシーの存在確認

出力がないことを確認する。

    aws iam list-attached-group-policies \
        --group-name ${IAM_GROUP_NAME} \
        --query "AttachedPolicies[?PolicyName == \`${IAM_POLICY_NAME}\`].PolicyName" \
        --output text
