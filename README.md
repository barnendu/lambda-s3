To create an execution role
Open the roles page in the IAM console.
Choose Create role.
Create a role with the following properties.

Trusted entity – AWS Lambda.

Permissions – AWSLambdaExecute.

Role name – lambda-s3-role.

The AWSLambdaExecute policy has the permissions that the function needs to manage objects in Amazon S3 and write logs to CloudWatch Logs.

Create a Lambda function with the create-function command.

$ aws lambda create-function --function-name CreateThumbnail --zip-file fileb://function.zip --handler index.handler --runtime nodejs8.10 --timeout 10 --memory-size 1024 --role arn:aws:iam::859633958305:role/lambda-s3-role

Now you might get error while executing the following command like :-
An error occurred(InvalidParameterValueException) when calling the CreateFunction operation: The role defined for the function cannot be assumed by Lambda.

To resolve this issue 
Go to 'IAM > Roles > YourRoleName'
(Note: if your role isn't listed, then you need to create it.)
Select the 'Trust Relationships' tab
Select 'Edit Trust Relationship'
{
  "Version": "2012-10-17",
  "Statement": [
    {
      <your other rules>
    },
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}