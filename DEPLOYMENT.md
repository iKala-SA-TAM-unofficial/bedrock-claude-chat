
## When to bootstrap your environment

If you found while you tried to execute `cdk bootstrap` and it would have `SSM parameter /cdk-bootstrap/hnb659fds/version not found` error return.[1]
You would like to clarify what could be the root cause of the issue. Please correct me if misunderstood your query.

Firstly, I tried to figure out when the IAM Roles `cdk-hnb659fds-{{ROLE_NAME}}-role-{{ACCOUNT_ID}}-{{REGION_CODE}}` removed.
From CloudTrail history, I can see an IAM User tried to update `CDKToolkit` stack, but it might be accident, the IAM Roles below were removed at the time,

    arn:aws:iam::435028209039:role/cdk-hnb659fds-cfn-exec-role-435028209039-us-east-1
    arn:aws:iam::435028209039:role/cdk-hnb659fds-lookup-role-435028209039-us-east-1
    arn:aws:iam::435028209039:role/cdk-hnb659fds-file-publishing-role-435028209039-us-east-1
    arn:aws:iam::435028209039:role/cdk-hnb659fds-image-publishing-role-435028209039-us-east-1
    arn:aws:iam::435028209039:role/cdk-hnb659fds-deploy-role-435028209039-us-east-1

    CloudTrail history:
    - https://us-east-1.console.aws.amazon.com/cloudtrail/home?region=us-east-1#/events?StartTime=2024-12-05T08:00:00.000Z&EndTime=2024-12-05T09:00:00.000Z&Username=richie

Follow the clues, I also found under your account, there have multiple users/roles tried to update the same stack - `CDKToolkit`, causing `CDKToolkit` stack been removed/created repeatedly.

To avoid this from happening again, kindly try to add default qualifier `hnb659fds` while deploy, by setting `--qualifier` flag whenever `cdk` command execute,

    $ cdk bootstrap --qualifier myproject
    $ cdk deploy --qualifier myproject

    https://docs.aws.amazon.com/cdk/v2/guide/bootstrapping-customizing.html#bootstrapping-customizing-cli-qualifier

Even more, you may try to enable termination protection for the bootstrap stack (CDKToolkit) to prevent it from termination by accident,

    $ cdk bootstrap --termination-protection
    $ aws cloudformation describe-stacks --stack-name CDKToolkit --query "Stacks[0]" | grep 'EnableTerminationProtection'

    https://docs.aws.amazon.com/cdk/v2/guide/bootstrapping-customizing.html#bootstrapping-customizing-cli-protection

To learn more about CDK troubleshooting, kindly spare some time to go through the links provided, and bookmark the link below,

    https://docs.aws.amazon.com/cdk/v2/guide/bootstrapping-troubleshoot.html
    https://docs.aws.amazon.com/cdk/v2/guide/bootstrapping-env.html#bootstrapping-env-when
    https://docs.aws.amazon.com/cdk/v2/guide/bootstrapping-env.html#bootstrapping-env-default-id

I believe after you add `--qualifier` and termination protection enabled, the issue should not happen again.

[1] https://docs.aws.amazon.com/cdk/v2/guide/bootstrapping-env.html#bootstrapping-env-when
