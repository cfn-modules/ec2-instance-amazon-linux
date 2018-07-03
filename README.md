# cfn-modules: AWS EC2 instance (Amazon Linux)

AWS EC2 instance based on Amazon Linux with a fixed public IP address (Elastic IP), auto recovery, [alerting](https://www.npmjs.com/package/@cfn-modules/alerting), [IAM user SSH access](https://github.com/widdix/aws-ec2-ssh).

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
  Function:
    Type: 'AWS::CloudFormation::Stack'
    Properties:
      Parameters:
        VpcModule: !GetAtt 'Vpc.Outputs.StackName' # required
        AlertingModule: !GetAtt 'Alerting.Outputs.StackName' # optional
        BastionModule: !GetAtt 'Bastion.Outputs.StackName' # optional
        HostedZoneModule: !GetAtt 'HostedZone.Outputs.StackName' # optional
        KeyName: '' # optional
        IAMUserSSHAccess: false # optional
        InstanceType: 't2.micro' # optional
        Name: 'test' # optional
        SubnetAZChar: 'A' # optional
        SubnetReach: 'Public' # optional
        LogGroupRetentionInDays: 14 # optional
        SubDomainNameWithDot: 'test.' # optional
        UserData: '' # optional
        IngressTcpPort1: '' # optional
        IngressTcpPort2: '' # optional
        IngressTcpPort3: '' # optional
        ClientSgModule1: '' # optional
        ClientSgModule2: '' # optional
        ClientSgModule3: '' # optional
      TemplateURL: './node_modules/@cfn-modules/ec2-instance-amazon-linux/module.yml'
```

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
      <td>Key name of the Linux user ec2-user to establish a SSH connection to the EC2 instance</td>
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
      <td>test</td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>SubnetAZChar</td>
      <td>Subnet availability zone char</td>
      <td>A</td>
      <td>no</td>
      <td>[A, B, C]</td>
    </tr>
    <tr>
      <td>SubnetReach</td>
      <td>Subnet reach</td>
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
      <td>Name that is used to create the DNS entry with trailing dot, e.g. ${SubDomainNameWithDot}${HostedZoneName}. Leave blank for naked (or apex and bare) domain. Requires HostedZoneModule parameter!</td>
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
      <td>IngressTcpPort2</td>
      <td>Port allowing ingress TCP traffic</td>
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
      <td>ClientSgModule1</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/client-sg">client-sg module</a> module to mark traffic from EC2 instance</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>ClientSgModule2</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/client-sg">client-sg module</a> module to mark traffic from EC2 instance</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>ClientSgModule3</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/client-sg">client-sg module</a> module to mark traffic from EC2 instance</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
  </tbody>
</table>
