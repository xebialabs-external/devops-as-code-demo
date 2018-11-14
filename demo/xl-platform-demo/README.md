# AVIS demo on AWS EC2 Container Service (ECS) with Fargate

This is directory uses your local XL Deploy to deploy the XL Platform to an AWS Fargate cluster. It's a bit inception like... I know.

## Prerequisites
* AWS Command Line Tools
* Python 3.6 or up
* Do everything described in `demo/ecs`. Make sure you've got all of this working.

### Prestep: push the customized XL Platform images to ECR

The latest image we want for AVIS deployed to [ECR](https://console.aws.amazon.com/ecs/home?region=us-east-1#/repositories) (yes, in us-east-1 instead of 2). I basically did this:

```
docker tag devops-as-code-demo_xl-deploy:latest 932770550094.dkr.ecr.us-east-1.amazonaws.com/devops-as-code-demo/xl-deploy:latest
docker push 932770550094.dkr.ecr.us-east-1.amazonaws.com/devops-as-code-demo/xl-deploy:latest

docker tag devops-as-code-demo_xl-release:latest 932770550094.dkr.ecr.us-east-1.amazonaws.com/devops-as-code-demo/xl-release:latest
docker push 932770550094.dkr.ecr.us-east-1.amazonaws.com/devops-as-code-demo/xl-release:latest
```

So the images that were built in the previous ECS step by docker-compose need to be pushed to ECR since ECS doesn't support building on the fly.

SO! As we want to add plugin etc to the docker image for AVIS we probably want to extend those Dockerfiles and push to ECR.

## Step 1 - Import definitions to deploy the XL Platform to AWS.

```
$ xl apply -f demo/xl-platform-demo/xl-platform-ecs-fargate-cluster.yaml
$ xl apply -f demo/xl-platform-demo/xl-platform-devops-as-code-service.yaml
```

Make sure you've already imported the AWSConfig in the `demo/ecs` example

## Step 2 - Create a log group for yourself

- Go to [CloudWatch](https://us-east-2.console.aws.amazon.com/cloudwatch/home?region=us-east-2#logs:)
- Create an `username-devops-as-code` group

## Step 3 - Start the deployment

1. Go to the XL Deploy UI running on http://localhost:4516.

2. Deploy xl-platform-ecs-fargate-cluster 1.0 to AWS

3. Deploy xl-deploy-devops-as-code 1.0 to AWS

4. Go to your [ECS Clusters](https://us-east-2.console.aws.amazon.com/ecs/home?region=us-east-2#/clusters) and find the IP Address by clicking on `username-xl-platform-ecs-cluster`, then `xl-platform-devops-as-code-server`, then `Tasks`, the task, and you'll see public IP. The shell script in this directory which should find the public IP for some reason doesn't work... TBD why.

5. Open XL Deploy and XL Release on the usual ports. The admin password is in the YAML.

## Step 4 - Import your AWS config into the ECS XL Deploy and XL Release

Do all the `xl apply` commands in `ecs/demo` again but add `--config config.yaml` so it posts to the AWS endpoint.

## Work to be done

- AVIS templates as YAML
- AVIS configs as YAML
- Add plugins to the ECR images
