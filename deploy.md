#### <qualifier_name> must same as you define in the [cdk.json](./cdk/cdk.json)
```
cdk bootstrap aws://435028209039/us-east-1 --qualifier <qualifier_name>
cdk bootstrap aws://435028209039/ap-northeast-1 --qualifier <qualifier_name>
cdk deploy --require-approval never --all
```