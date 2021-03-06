# Working with DB Security Groups \(EC2\-Classic Platform\)<a name="USER_WorkingWithSecurityGroups"></a>

By default, network access is turned off to a DB instance\. You can specify rules in a *security group* that allows access from an IP address range, port, or EC2 security group\. Once ingress rules are configured, the same rules apply to all DB instances that are associated with that security group\. You can specify up to 20 rules in a security group\.

Amazon RDS supports two different kinds of security groups\. The one you use depends on which Amazon RDS platform you are on: 
+ **VPC security groups** – for the EC2\-VPC platform\.
+ **DB security groups** – for the EC2\-Classic platform\.

You are most likely on the EC2\-VPC platform \(and must use VPC security groups\) if any of the following are true:
+ If you are a new Amazon RDS customer\.
+ If you have never created a DB instance before\.
+ If you are creating a DB instance in an AWS Region you have not used before\.

Otherwise, if you are on the EC2\-Classic platform, you use DB security groups to manage access to your Amazon RDS DB instances\. For more information about the differences between DB security groups and VPC security groups, see [Amazon RDS Security Groups](Overview.RDSSecurityGroups.md)\.

**Note**  
To determine which platform you are on, see [Determining Whether You Are Using the EC2\-VPC or EC2\-Classic Platform](USER_VPC.FindDefaultVPC.md)\.  
If you are on the EC2\-VPC platform, you must use VPC security groups instead of DB security groups\. For more information about using a VPC, see [Amazon Virtual Private Cloud \(VPCs\) and Amazon RDS](USER_VPC.md)\.

**Topics**
+ [Creating a DB Security Group](#USER_WorkingWithSecurityGroups.Creating)
+ [Listing Available DB Security Groups](#USER_WorkingWithSecurityGroups.Listing)
+ [Viewing a DB security group](#USER_WorkingWithSecurityGroups.Viewing)
+ [Associating a DB Security Group with a DB Instance](#USER_WorkingWithSecurityGroups.Associate)
+ [Authorizing Network Access to a DB Security Group from an IP Range](#USER_WorkingWithSecurityGroups.Authorizing)
+ [Authorizing Network Access to a DB Instance from an Amazon EC2 Instance](#USER_WorkingWithSecurityGroups.AuthorizingEC2)
+ [Revoking Network Access to a DB Instance from an IP Range](#USER_WorkingWithSecurityGroups.Revoking)

## Creating a DB Security Group<a name="USER_WorkingWithSecurityGroups.Creating"></a>

To create a DB security group, you need to provide a name and a description\.

### AWS Management Console<a name="USER_WorkingWithSecurityGroups.Creating.CON"></a>

**To create a DB security group**

1. Open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. Choose **Security Groups** in the navigation pane on the left side of the window\.
**Note**  
If you are on the EC2\-VPC platform, the **Security Groups** option does not appear in the navigation pane\. In this case, you must use VPC security groups instead of DB security groups\. For more information about using a VPC, see [Amazon Virtual Private Cloud \(VPCs\) and Amazon RDS](USER_VPC.md)\.

1. Choose **Create DB Security Group**\.

1. Type the name and description of the new DB security group in the **Name** and **Description** text boxes\. The security group name can't contain spaces and can't start with a number\.

1. Choose **Yes, Create**\. 

   The DB security group is created\. 

   A newly created DB security group doesn't provide access to a DB instance by default\. You must specify a range of IP addresses or an Amazon EC2 security group that can have access to the DB instance\. To specify IP addresses or an Amazon EC2 security group for a DB security group, see [Authorizing Network Access to a DB Security Group from an IP Range](#USER_WorkingWithSecurityGroups.Authorizing)\.

### CLI<a name="USER_WorkingWithSecurityGroups.Creating.CLI"></a>

To create a DB security group, use the AWS CLI command [http://docs.aws.amazon.com/cli/latest/reference/rds/create-db-security-group.html](http://docs.aws.amazon.com/cli/latest/reference/rds/create-db-security-group.html)\.

**Example**  
For Linux, OS X, or Unix:  

```
1. aws rds create-db-security-group \
2.     --db-security-group-name mydbsecuritygroup \
3.     --db-security-group-description "My new security group"
```
For Windows:  

```
1. aws rds create-db-security-group ^
2.     --db-security-group-name mydbsecuritygroup ^
3.     --db-security-group-description "My new security group"
```

A newly created DB security group doesn't provide access to a DB instance by default\. You must specify a range of IP addresses or an Amazon EC2 security group that can have access to the DB instance\. To specify IP addresses or an Amazon EC2 security group for a DB security group, see [Authorizing Network Access to a DB Security Group from an IP Range](#USER_WorkingWithSecurityGroups.Authorizing)\.

### API<a name="USER_WorkingWithSecurityGroups.Creating.API"></a>

To create a DB security group, call the Amazon RDS function [http://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_CreateDBSecurityGroup.html](http://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_CreateDBSecurityGroup.html) with the following parameters:
+ `DBSecurityGroupName = mydbsecuritygroup`
+ `Description = "My new security group"`

**Example**  

```
 1. https://rds.amazonaws.com/
 2.     ?Action=CreateDBSecurityGroup
 3.     &DBSecurityGroupName=mydbsecuritygroup
 4.     &Description=My%20new%20db%20security%20group
 5.     &Version=2012-01-15						
 6.     &SignatureVersion=2
 7.     &SignatureMethod=HmacSHA256
 8.     &Timestamp=2012-01-20T22%3A06%3A23.624Z
 9.     &AWSAccessKeyId=<AWS Access Key ID>
10.     &Signature=<Signature>
```

A newly created DB security group doesn't provide access to a DB instance by default\. You must specify a range of IP addresses or an Amazon EC2 security group that can have access to the DB instance\. To specify IP addresses or an Amazon EC2 security group for a DB security group, see [Authorizing Network Access to a DB Security Group from an IP Range](#USER_WorkingWithSecurityGroups.Authorizing)\.

## Listing Available DB Security Groups<a name="USER_WorkingWithSecurityGroups.Listing"></a>

You can list which DB security groups have been created for your AWS account\.

### AWS Management Console<a name="USER_WorkingWithSecurityGroups.Listing.CON"></a>

**To list all available DB security groups for an AWS account**

1. Open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. Choose **Security Groups** in the navigation pane on the left side of the window\.

   The available DB security groups appear in the **DB Security Groups** list\.

### CLI<a name="USER_WorkingWithSecurityGroups.Listing.CLI"></a>

To list all available DB security groups for an AWS account, Use the AWS CLI command [http://docs.aws.amazon.com/cli/latest/reference/rds/describe-db-security-groups.html](http://docs.aws.amazon.com/cli/latest/reference/rds/describe-db-security-groups.html) with no parameters\.

**Example**  

```
1. aws rds describe-db-security-groups
```

### API<a name="USER_WorkingWithSecurityGroups.Listing.API"></a>

To list all available DB security groups for an AWS account, call [http://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DescribeDBSecurityGroups.html](http://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DescribeDBSecurityGroups.html) with no parameters\.

**Example**  

```
1. https://rds.amazonaws.com/
2.     ?Action=DescribeDBSecurityGroups
3.     &MaxRecords=100
4.     &Version=2009-10-16
5.     &SignatureVersion=2
6.     &SignatureMethod=HmacSHA256
7.     &AWSAccessKeyId=<AWS Access Key ID>
8.     &Signature=<Signature>
```

## Viewing a DB security group<a name="USER_WorkingWithSecurityGroups.Viewing"></a>

You can view detailed information about your DB security group to see what IP ranges have been authorized\.

### AWS Management Console<a name="USER_WorkingWithSecurityGroups.Viewing.CON"></a>

**To view properties of a specific DB security group**

1. Open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. Choose **Security Groups** in the navigation pane on the left side of the window\.

1. Select the details icon for the DB security group you want to view\. The detailed information for the DB security group is displayed\. 

### CLI<a name="USER_WorkingWithSecurityGroups.Viewing.CLI"></a>

To view the properties of a specific DB security group use the AWS CLI [http://docs.aws.amazon.com/cli/latest/reference/rds/describe-db-security-groups.html](http://docs.aws.amazon.com/cli/latest/reference/rds/describe-db-security-groups.html)\. Specify the DB security group you want to view\.

**Example**  
For Linux, OS X, or Unix:  

```
1. aws rds describe-db-security-groups \
2.     --db-security-group-name mydbsecuritygroup
```
For Windows:  

```
1. aws rds describe-db-security-groups ^
2.     --db-security-group-name mydbsecuritygroup
```

### API<a name="USER_WorkingWithSecurityGroups.Viewing.API"></a>

To view properties of a specific DB security group, call [http://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DescribeDBSecurityGroups.html](http://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DescribeDBSecurityGroups.html) with the following parameters:
+ `DBSecurityGroupName`=*mydbsecuritygroup*

**Example**  

```
1. https://rds.amazonaws.com/
2.     ?Action=DescribeDBSecurityGroups
3.     &DBSecurityGroupName=mydbsecuritygroup
4.     &Version=2009-10-16
5.     &SignatureVersion=2
6.     &SignatureMethod=HmacSHA256
7.     &Timestamp=2009-10-16T22%3A23%3A07.107Z
8.     &AWSAccessKeyId=<AWS Access Key ID>
9.     &Signature=<Signature>
```

## Associating a DB Security Group with a DB Instance<a name="USER_WorkingWithSecurityGroups.Associate"></a>

You can associate a DB security group with a DB instance using the RDS console's **Modify** option, the `ModifyDBInstance` Amazon RDS API, or the AWS CLI `modify-db-instance` command\. For information about modifying a DB instance, see [Modifying an Amazon RDS DB Instance and Using the Apply Immediately Parameter](Overview.DBInstance.Modifying.md)\. 

## Authorizing Network Access to a DB Security Group from an IP Range<a name="USER_WorkingWithSecurityGroups.Authorizing"></a>

 By default, network access is turned off to a DB instance\. If you want to access a DB instance that is not in a VPC, you must set access rules for a DB security group to allow access from specific EC2 security groups or CIDR IP ranges\. You then must associate that DB instance with that DB security group\. This process is called ingress\. Once ingress is configured for a DB security group, the same ingress rules apply to all DB instances associated with that DB security group\. 

**Warning**  
Talk with your network administrator if you are intending to access a DB instance behind a firewall to determine the IP addresses you should use\.

In following example, you configure a DB security group with an ingress rule for a CIDR IP range\.

### AWS Management Console<a name="USER_WorkingWithSecurityGroups.Authorizing.CON"></a>

**To configure a DB security group with an ingress rule for a CIDR IP range**

1. Open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. Select **Security Groups** from the navigation pane on the left side of the console window\. 

1. Select the details icon for the DB security group you want to authorize\.

1. In the details page for your security group, select *CIDR/IP* from the **Connection Type** drop\-down list, type the CIDR range for the ingress rule you want to add to this DB security group into the **CIDR** text box, and choose **Authorize**\. 
**Tip**  
The AWS Management Console displays a CIDR IP based on your connection below the CIDR text field\. If you are not accessing the DB instance from behind a firewall, you can use this CIDR IP\. 

1. The status of the ingress rule is **authorizing** until the new ingress rule has been applied to all DB instances that are associated with the DB security group that you modified\. After the ingress rule has been successfully applied, the status changes to **authorized**\. 

### CLI<a name="USER_WorkingWithSecurityGroups.Authorizing.CLI"></a>

To configure a DB security group with an ingress rule for a CIDR IP range, use the AWS CLI command [http://docs.aws.amazon.com/cli/latest/reference/rds/authorize-db-security-group-ingress.html](http://docs.aws.amazon.com/cli/latest/reference/rds/authorize-db-security-group-ingress.html)\.

**Example**  
For Linux, OS X, or Unix:  

```
1. aws rds authorize-db-security-group-ingress \
2.     --db-security-group-name mydbsecuritygroup \
3.     --cidrip 192.168.1.10/27
```
For Windows:  

```
1. aws rds authorize-db-security-group-ingress ^
2.     --db-security-group-name mydbsecuritygroup ^
3.     --cidrip 192.168.1.10/27
```
The command should produce output similar to the following\.  

```
1. SECGROUP  mydbsecuritygroup  My new DBSecurityGroup
2. IP-RANGE  192.168.1.10/27  authorizing
```

### API<a name="USER_WorkingWithSecurityGroups.Authorizing.API"></a>

To configure a DB security group with an ingress rule for a CIDR IP range, call the Amazon RDS API [http://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_AuthorizeDBSecurityGroupIngress.html](http://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_AuthorizeDBSecurityGroupIngress.html) with the following parameters:
+ `DBSecurityGroupName = mydbsecuritygroup`
+ `CIDRIP = 192.168.1.10/27`

**Example**  

```
 1. https://rds.amazonaws.com/
 2. 	?Action=AuthorizeDBSecurityGroupIngress
 3. 	&CIDRIP=192.168.1.10%2F27
 4. 	&DBSecurityGroupName=mydbsecuritygroup
 5. 	&Version=2009-10-16
 6. 	&Action=AuthorizeDBSecurityGroupIngress
 7. 	&SignatureVersion=2
 8. 	&SignatureMethod=HmacSHA256
 9. 	&Timestamp=2009-10-22T17%3A10%3A50.274Z
10. 	&AWSAccessKeyId=<AWS Access Key ID>
11. 	&Signature=<Signature>
```

## Authorizing Network Access to a DB Instance from an Amazon EC2 Instance<a name="USER_WorkingWithSecurityGroups.AuthorizingEC2"></a>

If you want to access your DB instance from an Amazon EC2 instance, you must first determine if your EC2 instance and DB instance are in a VPC\. If you are using a default VPC, you can assign the same EC2 or VPC security group that you used for your EC2 instance when you create or modify the DB instance that the EC2 instance accesses\. 

If your DB instance and EC2 instance are not in a VPC, you must configure the DB instance's security group with an ingress rule that allows traffic from the Amazon EC2 instance\. You do this by adding the Amazon EC2 security group for the EC2 instance to the DB security group for the DB instance\. In this example, you add an ingress rule to a DB security group for an Amazon EC2 security group\. 

**Important**  
Adding an ingress rule to a DB security group for an Amazon EC2 security group only grants access to your DB instances from Amazon EC2 instances associated with that Amazon EC2 security group\. 
You can't authorize an Amazon EC2 security group that is in a different AWS Region than your DB instance\. You can authorize an IP range, or specify an Amazon EC2 security group in the same AWS Region that refers to IP address in another AWS Region\. If you specify an IP range, we recommend that you use the private IP address of your Amazon EC2 instance, which provides a more direct network route from your Amazon EC2 instance to your Amazon RDS DB instance, and doesn't incur network charges for data sent outside of the Amazon network\.

### AWS Management Console<a name="USER_WorkingWithSecurityGroups.AuthorizingEC2.CON"></a>

**To add an EC2 security group to a DB security group**

1. Open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. From the navigation pane, choose **Security Groups**\.

1. Select the details icon for the DB security group you want to grant access\.

1. In the details page for your security group, choose **EC2 Security Group** for **Connection Type**, and then select the Amazon EC2 security group you want to use\. Then choose **Authorize**\. 

1. The status of the ingress rule is **authorizing** until the new ingress rule has been applied to all DB instances that are associated with the DB security group that you modified\. After the ingress rule has been successfully applied, the status changes to **authorized**\. 

### CLI<a name="USER_WorkingWithSecurityGroups.AuthorizingEC2.CLI"></a>

To grant access to an Amazon EC2 security group, use the AWS CLI command [http://docs.aws.amazon.com/cli/latest/reference/rds/authorize-db-security-group-ingress.html](http://docs.aws.amazon.com/cli/latest/reference/rds/authorize-db-security-group-ingress.html)\.

**Example**  
For Linux, OS X, or Unix:  

```
1. aws rds authorize-db-security-group-ingress \
2.     --db-security-group-name default  \
3.     --ec2-security-group-name myec2group \
4.     --ec2-security-group-owner-id 987654321021
```
For Windows:  

```
1. aws rds authorize-db-security-group-ingress ^
2.     --db-security-group-name default  ^
3.     --ec2-security-group-name myec2group ^
4.     --ec2-security-group-owner-id 987654321021
```
The command should produce output similar to the following:  

```
1. SECGROUP  Name     Description 
2. SECGROUP  default  default
3.       EC2-SECGROUP  myec2group  987654321021  authorizing
```

### API<a name="USER_WorkingWithSecurityGroups.AuthorizingEC2.API"></a>

To authorize network access to an Amazon EC2 security group, call that Amazon RDS API function, [http://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_AuthorizeDBSecurityGroupIngress.html](http://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_AuthorizeDBSecurityGroupIngress.html)`AuthorizeDBSecurityGroupIngress` with the following parameters:
+ `EC2Security­GroupName = myec2group`
+ `EC2SecurityGroupOwnerId = 987654321021`

**Example**  

```
 1. https://rds.amazonaws.com/
 2.     ?Action=AuthorizeDBSecurityGroupIngress
 3.     &EC2SecurityGroupOwnerId=987654321021
 4.     &EC2Security­GroupName=myec2group
 5.     &Version=2009-10-16
 6.     &SignatureVersion=2
 7.     &SignatureMethod=HmacSHA256
 8.     &Timestamp=2009-10-22T17%3A10%3A50.274Z
 9.     &AWSAccessKeyId=<AWS Access Key ID>
10.     &Signature=<Signature>
```

## Revoking Network Access to a DB Instance from an IP Range<a name="USER_WorkingWithSecurityGroups.Revoking"></a>

You can easily revoke network access from a CIDR IP range to DB Instances belonging to a DB security group by revoking the associated CIDR IP ingress rule\.

In this example, you revoke an ingress rule for a CIDR IP on a DB Security Group\.

### AWS Management Console<a name="USER_WorkingWithSecurityGroups.Revoking.CON"></a>

**To revoke an ingress rule for a CIDR IP range on a DB Security Group**

1. Open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. From the navigation pane, choose **Security Groups**\. 

1. Select the details icon for the DB security group that has the ingress rule you want to revoke\.

1. In the details page for your security group, choose **Remove** next to the ingress rule you want to revoke\.

1. The status of the ingress rule is **revoking** until the ingress rule has been removed from all DB instances that are associated with the DB security group that you modified\. After the ingress rule has been successfully removed, the ingress rule is removed from the DB security group\. 

### CLI<a name="USER_WorkingWithSecurityGroups.Revoking.CLI"></a>

To revoke an ingress rule for a CIDR IP range on a DB security group, use the AWS CLI command [http://docs.aws.amazon.com/cli/latest/reference/rds/revoke-db-security-group-ingress.html](http://docs.aws.amazon.com/cli/latest/reference/rds/revoke-db-security-group-ingress.html)\.

**Example**  
For Linux, OS X, or Unix:  

```
1. aws rds revoke-db-security-group-ingress \ 
2.     --db-security-group-name mydbsecuritygroup \
3.     --cidrip 192.168.1.1/27
```
For Windows:  

```
1. aws rds revoke-db-security-group-ingress ^
2.     --db-security-group-name mydbsecuritygroup ^
3.     --cidrip 192.168.1.1/27
```
The command should produce output similar to the following\.  

```
1. SECGROUP  mydbsecuritygroup  My new DBSecurityGroup
2.   IP-RANGE  192.168.1.1/27  revoking
```

### API<a name="USER_WorkingWithSecurityGroups.Revoking.API"></a>

To revoke an ingress rule for a CIDR IP range on a DB security group, call the Amazon RDS API action [http://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_RevokeDBSecurityGroupIngress.html](http://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_RevokeDBSecurityGroupIngress.html)`RevokeDBSecurityGroupIngress` with the following parameters:
+ `DBSecurityGroupName = mydbsecuritygroup`
+ `CIDRIP = 192.168.1.10/27`

**Example**  

```
1. https://rds.amazonaws.com/
2.     ?Action=RevokeDBSecurityGroupIngress
3.     &DBSecurityGroupName=mydbsecuritygroup
4.     &CIDRIP=192.168.1.10%2F27
5.     &Version=2009-10-16
6.     &SignatureVersion=2&SignatureMethod=HmacSHA256
7.     &Timestamp=2009-10-22T22%3A32%3A12.515Z
8.     &AWSAccessKeyId=<AWS Access Key ID>
9.     &Signature=<Signature>
```