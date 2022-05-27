<!-- omit in toc -->
# インターンユーザ用ポリシードキュメント作成

インターンユーザに付与するポリシードキュメントを作成します。

## 1. 変数の指定

### 1.1. ディレクトリ指定

    DIR_IAM_POLICY_DOC="${HOME}/environment/conf-iam"

### 1.2. ドキュメント名

    IAM_POLICY_DOC_NAME='internship-user-policy'

### 1.3. Lambda用IAMロール名

    IAM_LAMBDA_ROLE_NAME='internship-lambda-role-template'

### 1.4. Lambda用IAMロールPATH

    IAM_LAMBDA_ROLE_PATH='/internship-cli/'

### 1.5. Lambda関数配置リージョン

    LAMBDA_FUNCTION_REGION='ap-northeast-1'

### 1.6. Lambda関数名

    LAMBDA_FUNCTION_PREFIX='internship-yolo5-'

## 2. メインの処理

### 2.1. ディレクトリの存在確認

    ls -d ${DIR_IAM_POLICY_DOC} | mkdir -p ${DIR_IAM_POLICY_DOC}

### 2.2. ドキュメントファイル名

    FILE_IAM_POLICY_DOC="${DIR_IAM_POLICY_DOC}/${IAM_POLICY_DOC_NAME}.json" \
        && echo ${FILE_IAM_POLICY_DOC}

### 2.3. AWSID取得

    AWS_ID=$( \
        aws sts get-caller-identity \
            --query 'Account' \
            --output text \
    ) \
    && echo ${AWS_ID}

### 2.5. Lambda用IAMロールのARN取得

    IAM_LAMBDA_ROLE_ARN=$( \
        aws iam list-roles \
            --path-prefix ${IAM_LAMBDA_ROLE_PATH} \
            --query "Roles[?RoleName==\`${IAM_LAMBDA_ROLE_NAME}\`].Arn" \
            --output text
    ) \
        && echo ${IAM_LAMBDA_ROLE_ARN}

### 2.6. ポリシードキュメント作成

    cat << EOF > ${FILE_IAM_POLICY_DOC}
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "lambda:AddPermission",
                    "lambda:CreateFunction",
                    "lambda:GetPolicy",
                    "lambda:UpdateFunctionConfiguration",
                    "lambda:InvokeFunction",
                    "lambda:GetAccountSettings",
                    "lambda:ListFunctions",
                    "lambda:GetFunctionConfiguration",
                    "lambda:GetFunction"
                ],
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "s3:CreateBucket",
                    "s3:ListBucket",
                    "s3:ListAllMyBuckets",
                    "s3:GetBucketPolicyStatus",
                    "s3:GetAccountPublicAccessBlock",
                    "s3:GetBucketPublicAccessBlock",
                    "s3:GetBucketAcl",
                    "s3:ListAccessPoints"
                ],
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "s3:GetObject",
                    "s3:PutObject",
                    "s3:PutObjectACL",
                    "s3:DeleteObject",
                    "s3:GetBucketLocation",
                    "s3:GetBucketNotification",
                    "s3:PutBucketNotification",
                    "s3:CreateBucket",
                    "s3:ListBucket",
                    "s3:ListAllMyBuckets",
                    "s3:GetBucketCORS",
                    "s3:PutBucketCORS",
                    "s3:PutBucketPublicAccessBlock",
                    "s3:PutBucketAcl",
                    "s3:GetObjectAcl",
                    "s3:GetBucketWebsite",
                    "s3:PutBucketWebsite",
                    "s3:ListBucketVersions",
                    "s3:GetBucketVersioning"
                ],
                "Resource": [
                    "arn:aws:s3:::internship-web-*",
                    "arn:aws:s3:::raspberrypi-*"
                ]
            },
            {
                "Effect": "Allow",
                "Action": [
                    "ecr:ListImages",
                    "ecr:DescribeRepositories",
                    "ecr:DescribeImages"
                ],
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "glue:CreateTable",
                    "glue:GetDatabases",
                    "glue:GetTables",
                    "glue:GetTable"
                ],
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "athena:ListDatabases",
                    "athena:ListTableMetadata",
                    "athena:ListWorkGroups",
                    "athena:GetQueryResults",
                    "athena:StartQueryExecution",
                    "athena:GetWorkGroup",
                    "athena:GetQueryExecution"
                ],
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "logs:DescribeLogGroups",
                    "logs:CreateLogGroup"
                ],
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "logs:DescribeLogStreams",
                    "logs:GetLogEvents"
                ],
                "Resource": "arn:aws:logs:${LAMBDA_FUNCTION_REGION}:${AWS_ID}:log-*:/aws/lambda/${LAMBDA_FUNCTION_PREFIX}*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "quicksight:CreateUser",
                    "quicksight:CreateAdmin"
                ],
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "iam:ListRoles",
                    "iam:GetRole",
                    "iam:ChangePassword"
                ],
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "iam:PassRole"
                ],
                "Resource": "${IAM_LAMBDA_ROLE_ARN}"
            }
        ]
    }
    EOF

    cat ${FILE_IAM_POLICY_DOC}

## 3. 確認

### 3.1. JSONフォーマットの確認

出力がないことを確認する。

    cat ${FILE_IAM_POLICY_DOC} \
        |  python3 -m json.tool \
        > /dev/null

### 3.2. ファイルの存在確認

ファイルのパスが表示されることを確認する

    ls ${FILE_IAM_POLICY_DOC}
