# Steps to take

- install AWSCLI:
- Create the ```trust-policy.json``` file.
- create an IAM role for our lambda function.
```console
aws iam create-role --role-name go-lambda-function --assume-role-policy-document '{"Version": "2012-10-17","Statement": [{ "Effect": "Allow", "Principal": {"Service": "lambda.amazonaws.com"}, "Action": "sts:AssumeRole"}]}'
```
- Assume role policy document:
```console
aws iam create-role --role-name go-lambda-function --assume-role-policy-document file://trust-policy.json
```
- Attach policy to the role: 
```console
aws iam attach-role-policy --role-name go-lambda-function --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
```
- Create a deployment package:
```console
zip function.zip main.go
```
- Create lambda function:
```console
aws lambda create-function --function-name go-lambda-function \
--zip-file fileb://function.zip --handler main --runtime go1.x \
--role arn:aws:iam::<account_ID>:role/go-lambda-function
```
- Invoke the lambda function:
```console
aws lambda invoke --function-name go-lambda-function --cli-binary-format raw-in-base64-out --payload '{"What is your name?": "Larry", "How old are you?", "26"}' output.txt
```