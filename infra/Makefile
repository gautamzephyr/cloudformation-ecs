AWS_PROFILE:=team0infra

ecr:
	aws cloudformation create-stack --stack-name books-api --template-body file://01_ecr.yaml --tags Key=owner,Value=sameer Key=service,Value=ecr --parameters file://parameters.json --profile $(AWS_PROFILE)
image:
	@eval $$\(aws ecr get-login --no-include-email --profile $(AWS_PROFILE)\)
	docker build -t books-api ../app
	@$(eval REPOSITORY_URI := $(shell aws ecr describe-repositories --repository-name books-api --query 'repositories[*].repositoryUri'  --output text --profile $(AWS_PROFILE)))
	docker tag books-api:latest $(REPOSITORY_URI):latest
	docker push $(REPOSITORY_URI):latest
iam:
	aws cloudformation create-stack --stack-name books-api-iam --template-body file://02_iam.yaml --tags Key=owner,Value=sameer Key=service,Value=ecr --capabilities CAPABILITY_IAM --profile $(AWS_PROFILE)
loadbalancer:
	aws cloudformation create-stack --stack-name books-api-lb --template-body file://03_loadbalancer.yaml --tags Key=owner,Value=sameer Key=service,Value=ecr --profile $(AWS_PROFILE)
cluster:
	aws cloudformation create-stack --stack-name books-api-cluster --template-body file://04_ecs_cluster.yaml --tags Key=owner,Value=sameer Key=service,Value=ecr --profile $(AWS_PROFILE)