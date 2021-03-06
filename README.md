# cfn-modules: AWS EC2 instance (Amazon Linux)

AWS EC2 instance based on Amazon Linux with a fixed public IP address (Elastic IP), [auto recovery](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-recover.html), [alerting](https://www.npmjs.com/package/@cfn-modules/alerting), [IAM user SSH access](https://github.com/widdix/aws-ec2-ssh), following an mutable infrastructure approach (root volume is reused in case of auto recovery).

## Install

> Install [Node.js and npm](https://nodejs.org/) first!

```
npm i @cfn-modules/ec2-instance-amazon-linux
```

## Usage

```
---
AWSTemplateFormatVersion: '2010-09-09'
Description: 'cfn-modules example'
Resources:
  Instance:
    Type: 'AWS::CloudFormation::Stack'
    Properties:
      Parameters:
        VpcModule: !GetAtt 'Vpc.Outputs.StackName' # required
        AlertingModule: !GetAtt 'Alerting.Outputs.StackName' # optional
        BastionModule: !GetAtt 'Bastion.Outputs.StackName' # optional
        HostedZoneModule: !GetAtt 'HostedZone.Outputs.StackName' # optional
        KeyName: '' # optional
        IAMUserSSHAccess: 'false' # optional
        InstanceType: 't2.micro' # optional
        Name: 'test' # optional
        AZChar: 'A' # optional
        SubnetReach: 'Public' # optional
        LogGroupRetentionInDays: '14' # optional
        SubDomainNameWithDot: 'test.' # optional
        UserData: '' # optional
        IngressTcpPort1: '' # optional
        IngressTcpClientSgModule1: '' # optional
        IngressTcpPort2: '' # optional
        IngressTcpClientSgModule2: '' # optional
        IngressTcpPort3: '' # optional
        IngressTcpClientSgModule3: '' # optional
        ClientSgModule1: '' # optional
        ClientSgModule2: '' # optional
        ClientSgModule3: '' # optional
        FileSystemModule1: '' # optional
        VolumeModule1: '' # optional
        AmazonLinuxVersion: '2018.03.0.20180622' # set this to the latest available version!
        ManagedPolicyArns: '' # optional
      TemplateURL: './node_modules/@cfn-modules/ec2-instance-amazon-linux/module.yml'
```

## Examples

* [ec2-ebs](https://github.com/cfn-modules/docs/tree/master/examples/ec2-ebs)
* [ec2-efs](https://github.com/cfn-modules/docs/tree/master/examples/ec2-efs)
* [ec2-mysql](https://github.com/cfn-modules/docs/tree/master/examples/ec2-mysql)
* [ec2-postgres](https://github.com/cfn-modules/docs/tree/master/examples/ec2-postgres)
* [ec2-ssh-bastion](https://github.com/cfn-modules/docs/tree/master/examples/ec2-ssh-bastion)

## Related modules

* [asg-singleton-amazon-linux2](https://github.com/cfn-modules/asg-singleton-amazon-linux2)
* [ec2-instance-amazon-linux2](https://github.com/cfn-modules/ec2-instance-amazon-linux2)

## Parameters

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Description</th>
      <th>Default</th>
      <th>Required?</th>
      <th>Allowed values</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>VpcModule</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/vpc">vpc module</a></td>
      <td></td>
      <td>yes</td>
      <td></td>
    </tr>
    <tr>
      <td>AlertingModule</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/alerting">alerting module</a></td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>BastionModule</td>
      <td>Stack name of <a href="https://www.npmjs.com/search?q=keywords:cfn-modules:Bastion">module implementing Bastion</a></td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>HostedZoneModule</td>
      <td>Stack name of <a href="https://www.npmjs.com/search?q=keywords:cfn-modules:HostedZone">module implementing HostedZone</a></td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>KeyName</td>
      <td>Key name of the Linux user ec2-user to establish a SSH connection to the EC2 instance (update requires replacement of root volume = data loss!)</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>IAMUserSSHAccess</td>
      <td>Synchronize public keys of IAM users to enable personalized SSH access (https://github.com/widdix/aws-ec2-ssh)?</td>
      <td>false</td>
      <td>no</td>
      <td>[true, false]</td>
    </tr>
    <tr>
      <td>InstanceType</td>
      <td>The instance type for the EC2 instance</td>
      <td>t2.micro</td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>Name</td>
      <td>The name for the EC2 instance</td>
      <td>auto generated value</td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>AZChar</td>
      <td>Availability zone char (update requires replacement of root volume = data loss!)</td>
      <td>A</td>
      <td>no</td>
      <td>[A, B, C]</td>
    </tr>
    <tr>
      <td>SubnetReach</td>
      <td>Subnet reach (update requires replacement of root volume = data loss!)</td>
      <td>Public</td>
      <td>no</td>
      <td>[Public, Private]</td>
    </tr>
    <tr>
      <td>LogGroupRetentionInDays</td>
      <td>Specifies the number of days you want to retain log events</td>
      <td>14</td>
      <td>no</td>
      <td>[1, 3, 5, 7, 14, 30, 60, 90, 120, 150, 180, 365, 400, 545, 731, 1827, 3653]</td>
    </tr>
    <tr>
      <td>SubDomainNameWithDot</td>
      <td>Name that is used to create the DNS entry with trailing dot, e.g. §{SubDomainNameWithDot}§{HostedZoneName}. Leave blank for naked (or apex and bare) domain. Requires HostedZoneModule parameter!</td>
      <td>test.</td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>UserData</td>
      <td>Bash script executed on first instance launch</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>IngressTcpPort1</td>
      <td>Port allowing ingress TCP traffic</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>IngressTcpClientSgModule1</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/client-sg">client-sg module</a> that is required to access IngressTcpPort1 (if you leave this blank, IngressTcpPort1 is open to the world 0.0.0.0/0)</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>IngressTcpPort2</td>
      <td>Port allowing ingress TCP traffic</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>IngressTcpClientSgModule2</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/client-sg">client-sg module</a> that is required to access IngressTcpPort2 (if you leave this blank, IngressTcpPort2 is open to the world 0.0.0.0/0)</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>IngressTcpPort3</td>
      <td>Port allowing ingress TCP traffic</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>IngressTcpClientSgModule3</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/client-sg">client-sg module</a> that is required to access IngressTcpPort3 (if you leave this blank, IngressTcpPort3 is open to the world 0.0.0.0/0)</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>ClientSgModule1</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/client-sg">client-sg module</a> to mark traffic from EC2 instance</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>ClientSgModule2</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/client-sg">client-sg module</a> to mark traffic from EC2 instance</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>ClientSgModule3</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/client-sg">client-sg module</a> to mark traffic from EC2 instance</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>FileSystemModule1</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/efs-file-system">efs-file-system module</a> mounted to /mnt/efs1/ (update is not supported)</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>VolumeModule1</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/ebs-volume">ebs-volume</a> mounted to /mnt/volume1/ (update is not supported)</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>AmazonLinuxVersion</td>
      <td>Version of Amazon Linux (update requires replacement of root volume = data loss!)</td>
      <td>2018.03.0.20180622</td>
      <td>no</td>
      <td>['2018.03.0.20190514', '2018.03.0.20181116', '2018.03.0.20180622']</td>
    </tr>
    <tr>
      <td>ManagedPolicyArns</td>
      <td>Comma-delimited list of IAM managed policy ARNs to attach to the instance's IAM role</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
  </tbody>
</table>

## Limitations

* Highly available: EC2 instances only live in a single AZ by design
* Scalable: EC2 instances capacity (CPU, RAM, network, ...) is limited by design
* Secure: Root volume is not encrypted at-rest (not possible unless the AMI is encrypted)
* Secure: Root volume it not backed up
* Monitoring: Network In+Out is not monitored according to capacity of instance type

## Migration Guides

### Migrate to v2

* If `SystemsManagerAccess` is set to `true`, we no longer attach the AWS managed policy `AmazonEC2RoleforSSM` for security reasons. Instead we only allow the SSM agent to communicate with the backend and we enable Session Manager. If you need more permissions, checkout our [SSM example](https://github.com/cfn-modules/docs/tree/master/examples/ec2-ssm).
