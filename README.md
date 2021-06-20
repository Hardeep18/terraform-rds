# terraform-rds

Terraform modules to manage RDS resources

## rds

Creates a RDS instance, security_group, subnet_group and parameter_group

### Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| rds\_password | RDS root password | string | n/a | yes |
| security\_groups | Security groups that are allowed to access the RDS | list(string) | n/a | yes |
| security\_groups\_count | Number of security groups provided in `security\_groups` variable | string | n/a | yes |
| subnets | Subnets to deploy in | list(string) | n/a | yes |
| vpc\_id | ID of the VPC where to deploy in | string | n/a | yes |
| allowed\_cidr\_blocks | CIDR blocks that are allowed to access the RDS | list(string) | `[]` | no |
| apply\_immediately | Apply changes immediately | string | `"true"` | no |
| auto\_minor\_version\_upgrade | Indicates that minor engine upgrades will be applied automatically to the DB instance during the maintenance window. | string | `"true"` | no |
| availability\_zone | The availability zone where you want to launch your instance in | string | `""` | no |
| backup\_retention\_period | How long do you want to keep RDS backups | string | `"14"` | no |
| default\_parameter\_group\_family | Parameter group family for the default parameter group, according to the chosen engine and engine version. Defaults to mysql5.7 | string | `"mysql5.7"` | no |
| enabled\_cloudwatch\_logs\_exports | List of log types to enable for exporting to CloudWatch logs. You can check the available log types per engine in the \[AWS RDS documentation\]\(https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER\_LogAccess.html#USER\_LogAccess.Procedural.UploadtoCloudWatch\). | list(string) | `[]` | no |
| engine | RDS engine: mysql, oracle, postgres. Defaults to mysql | string | `"mysql"` | no |
| engine\_version | Engine version to use, according to the chosen engine. You can check the available engine versions using the \[AWS CLI\]\(http://docs.aws.amazon.com/cli/latest/reference/rds/describe-db-engine-versions.html\). Defaults to 5.7.17 for MySQL. | string | `"5.7.25"` | no |
| environment | How do you want to call your environment, this is helpful if you have more than 1 VPC. | string | `"production"` | no |
| maintenance\_window | The window to perform maintenance in. Syntax: 'ddd:hh24:mi-ddd:hh24:mi'. See RDS Maintenance Window docs for more information. | string | `"Mon:00:00-Mon:01:00"` | no |
| max\_allocated\_storage | When configured, the upper limit to which Amazon RDS can automatically scale the storage of the DB instance. Configuring this will automatically ignore differences to allocated\_storage. Must be greater than or equal to allocated\_storage or 0 to disable Storage Autoscaling. | string | `"0"` | no |
| monitoring\_interval | The interval, in seconds, between points when Enhanced Monitoring metrics are collected for the DB instance. To disable collecting Enhanced Monitoring metrics, specify 0. Valid Values: 0, 1, 5, 10, 15, 30, 60. | string | `"0"` | no |
| multi\_az | Multi AZ true or false | string | `"true"` | no |
| name | The name of the RDS instance | string | `""` | no |
| number | number of the database default 01 | string | `"01"` | no |
| performance\_insights\_enabled | Specifies whether Performance Insights is enabled or not. | bool | `"false"` | no |
| project | The current project | string | `""` | no |
| rds\_custom\_parameter\_group\_name | A custom parameter group name to attach to the RDS instance. If not provided a default one will be used | string | `""` | no |
| rds\_username | RDS root user | string | `"root"` | no |
| size | Instance size | string | `"db.t2.small"` | no |
| skip\_final\_snapshot | Skip final snapshot when destroying RDS | string | `"false"` | no |
| snapshot\_identifier | Specifies whether or not to create this database from a snapshot. This correlates to the snapshot ID you'd find in the RDS console, e.g: rds:production-2015-06-26-06-05. | string | `""` | no |
| storage | How many GBs of space does your database need? | string | `"10"` | no |
| storage\_encrypted | Encrypt RDS storage | string | `"true"` | no |
| storage\_type | Type of storage you want to use | string | `"gp2"` | no |
| tag | A tag used to identify an RDS in a project that has more than one RDS | string | `""` | no |

### Outputs

| Name | Description |
|------|-------------|
| aws\_db\_subnet\_group\_id | The subnet group id of the RDS instance |
| rds\_address | The hostname of the RDS instance |
| rds\_arn | The arn of the RDS instance |
| rds\_id | The id of the RDS instance |
| rds\_port | The port of the RDS instance |
| rds\_sg\_id | The security group id of the RDS instance |

### Example

```tf
module "rds" {
  source                          = "github.com/Hardeep18/terraform-rds"
  vpc_id                          = "vpc-12345"
  subnets                         = ["subnet-12345d67", "subnet-12345d68", "subnet-12345d69"]
  project                         = "project1"
  environment                     = "production"
  size                            = "db.t2.small"
  security_groups                 = ["sg-12be345678905ebf1", "sg-1234567890aef"]
  enabled_cloudwatch_logs_exports = ["audit", "error", "slowquery"]
  security_groups_count           = 2
  rds_password                    = "supersecurepassword"
  multi_az                        = "false"
}
```

## Aurora

Creates a Aurora cluster + instances, security_group, subnet_group and parameter_group
