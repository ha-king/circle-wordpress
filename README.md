# CircleCI - Grace Hopper Jeopardy

## Run locally - Host

Run the following:

    npm install
    npm run webpack

Then open index.html in your browser.

## Run locally - Docker

Run the following:

    docker build -t ghc:1.0 .
    docker run -itd -p 8000:80 ghc:1.0

In your browser visit: ```http://localhost:8000```

## CircleCI deploy on AWS

### Note: 
CircleCI requires IAM user access key id + secret key. Create environmental variables for these values in CircleCI web console under project system gear icon:

    AWS_ACCESS_KEY_ID
    AWS_SECRET_ACCESS_KEY

This solution uses CircleCI to deploy EC2 resources using AWS CloudFormation.  It can be used as an example application in a workshop, with multiple stacks in the same account.

## Deploy on AWS via CloudFormation service with AWS CLI

Create shared resources (create once):
```
aws cloudformation deploy --stack-name ghc-workshop-shared-resources --template-file cfn/shared_resources.yml --capabilities CAPABILITY_NAMED_IAM --parameter-overrides WorkshopName="ghc-workshop"
```

Create website resources (can create multiple stacks for a workshop):
```
aws cloudformation deploy --stack-name ghc-workshop-application-1 --template-file cfn/application.yml --parameter-overrides SharedResourceStack="ghc-workshop-shared-resources"
```

Once the deployment completes, go to the application URL:
```
aws cloudformation describe-stacks --stack-name ghc-workshop-application-1 --query 'Stacks[0].Outputs[?OutputKey==`Url`].OutputValue' --output text
```

Cleanup:
```
Delete Stacks:

aws cloudformation delete-stack --stack-name ghc-workshop-shared-resources

aws cloudformation delete-stack --stack-name ghc-workshop-application-1
```

## Credits
* Adapted from [Grace Hopper Jeopardy](https://github.com/clareliguori/grace-hopper-jeopardy)
* Based on [React Trivia](https://github.com/ccoenraets/react-trivia)
* Grace Hopper clip art by [gingercoons](https://openclipart.org/detail/137533/grace-hopper)

