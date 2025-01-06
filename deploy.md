#### TRY 1: <qualifier_name> must same as you define in the [cdk.json](./cdk/cdk.json)
```shell
cdk bootstrap aws://435028209039/us-east-1 --qualifier <qualifier_name>
cdk bootstrap aws://435028209039/ap-northeast-1 --qualifier <qualifier_name>
cdk deploy --require-approval never --all
```

### TRY 2: execute the command below since you have a qualifier "veckboot":
```shell
cdk deploy --context @aws-cdk/core:bootstrapQualifier=veckboot --no-previous-parameters --all --require-approval never
```

### TRY 3: execute the command below if the command above failed:
```shell
cdk bootstrap --qualifier mybootstrap
cdk deploy --context @aws-cdk/core:bootstrapQualifier=mybootstrap --no-previous-parameters --all --require-approval never
cat cdk.json
```
