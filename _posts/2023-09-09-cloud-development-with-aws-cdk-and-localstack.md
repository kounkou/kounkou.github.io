# Cloud development

## Introduction

Over the past two weeks, I've had the opportunity to travel across three different continents! While gaining access to the internet turned out to be less challenging than I initially anticipated, I still couldn't shake off my concerns about the security of my online activities when connecting to unsecure networks like those in hotels and cafes.

Recently, I've delved into the realm of Cloud development, and my thirst for knowledge in this area persists, even during my vacation time. That's why I've decided to dedicate this article to individuals who, like me, are eager to engage in Cloud development while being offline.

To make the content easily understandable, I will utilize one of my projects as an example and provide a comprehensive list of the commands you can use to work on a Cloud development project without the need for an internet connection.

## PreSteps

Prior to disconnecting from the internet, please ensure that you have Docker and LocalStack installed. You can locate installation instructions tailored to your particular operating system and proceed with the installation process.

## Steps to follow

Clone (and please like) my project

```bash
git clone git@github.com:kounkou/cloud-hasher.git
```

After successfully cloning the project to your local machine, navigate to the "cloud-hasher" directory and proceed to install all the required dependencies.

```bash
npm install -g aws-cdk
```

Now, here's where the magic happens: Ensure that your Docker desktop is up and running. Afterward, open another terminal and execute the following command:

```bash
docker run --rm -it -p 4566:4566 -p 4571:4571 -v /var/run/docker.sock:/var/run/docker.sock localstack/localstack
```

Afterward, return to the terminal where you initially cloned the cloud-hasher repository and initiate the following steps:

Navigate to the cloud-hasher/src/processorlambda/ directory to zip the lambda code :

```bash
go mod tidy
GOOS=linux GOARCH=amd64 go build -o main main.go
zip main.zip main
```

Then go back to the root directory to launch the command to deploy the CDK application `locally`

```bash
cdklocal bootstrap aws://000000000000/us-east-1 && cdklocal synth && cdklocal deploy
```

This will trigger the deployment of the project to your localstack. Please confirm if prompted with the question, "Do you wish to deploy the changes?"

If you receive a message similar to the one below, congratulations, you have successfully completed the process.

```bash
Do you wish to deploy these changes (y/n)? y
CloudHasherStack: deploying... [1/1]
CloudHasherStack: creating CloudFormation changeset...

 ✅  CloudHasherStack

✨  Deployment time: 10.45s

Outputs:
CloudHasherStack.CloudHasherRestAPIEndpointC81C50C4 = https://f2hexauzp1.execute-api.localhost.localstack.cloud:4566/prod/
Stack ARN:
arn:aws:cloudformation:us-east-1:000000000000:stack/CloudHasherStack/c8de31ae

✨  Total time: 17.17s
```


## Final thoughts

Localstack is an incredible tool for simulating AWS services locally, and the company is sure to achieve great success in this endeavor. While this brief article primarily discusses AWS, it would be logical to include Azure and Google Cloud in Localstack as well. I am eager to see how promising the future will be. I hope you found this short article enjoyable!

## References 

- https://docs.aws.amazon.com/cdk/v2/guide/serverless_example.html
- https://localstack.cloud
