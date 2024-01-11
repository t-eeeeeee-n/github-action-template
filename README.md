# GitHub Actionsのテンプレート

### 前提条件

- OIDCを使用して、AWS認証ができること。
- 参照：[GitHub Actionsを使ったOIDCによるAWS認証の方法](https://zenn.dev/kou_pg_0131/articles/gh-actions-oidc-aws)

### 設定手順

1. **GitHubのSecretに環境変数の情報を登録**
   - `AWS_ACCOUNT_ID`: 747280103911
   - `AWS_REGION`: ap-northeast-1
   - `AWS_ROLE_TO_ASSUME`: arn:aws:iam::747280103911:oidc-provider/token.actions.githubusercontent.com
   - `AWS_ACCESS_KEY_ID`: [アクセスキーID]
   - `AWS_SECRET_ACCESS_KEY`: [シークレットアクセスキー]

2. **ECRのリポジトリに許可を追加**
   - IAMポリシー例:
     ```json
     {
       "Version": "2008-10-17",
       "Statement": [
         {
           "Sid": "LambdaECRImageRetrievalPolicy",
           "Effect": "Allow",
           "Principal": {
             "Service": "lambda.amazonaws.com"
           },
           "Action": [
             "ecr:BatchGetImage",
             "ecr:DeleteRepositoryPolicy",
             "ecr:GetDownloadUrlForLayer",
             "ecr:GetImage",
             "ecr:GetRepositoryPolicy",
             "ecr:SetRepositoryPolicy"
           ],
           "Condition": {
             "StringLike": {
               "aws:sourceArn": "arn:aws:lambda:ap-northeast-1:747280103911:function
