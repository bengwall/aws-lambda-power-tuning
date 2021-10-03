# Brents Modified Install

```bash
# setup vars
TARGET_ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
BUCKET_NAME=aws-deploy-$TARGET_ACCOUNT_ID-us-east-1
STACK_NAME=lambda-power-tuning-brent
PERMISSION_BOUNDRY=arn:aws:iam::$TARGET_ACCOUNT_ID:policy/SANDBOX_BOUNDARY


bash scripts/build-layer.sh


PowerValues='128,256,512,1024,1536,3008'
LambdaResource='*'
TotalExecutionTimeout="300"
PermissionsBoundary=arn:aws:iam::$TARGET_ACCOUNT_ID:policy/SANDBOX_BOUNDARY

# package
sam package --s3-bucket $BUCKET_NAME --template-file template.yml --output-template-file packaged.yml

# deploy
sam deploy --template-file packaged.yml --stack-name $STACK_NAME --capabilities CAPABILITY_IAM \
 --parameter-overrides PowerValues=$PowerValues lambdaResource=$LambdaResource totalExecutionTimeout=$TotalExecutionTimeout permissionsBoundary=$PermissionsBoundary
```
