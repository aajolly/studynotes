# Amazon QuickSight (QS)

## Implementing QuickSight
### Sign-up for QuickSight
Two ways to sign-up for QuickSight:
1. Via AWS Account >> search >> QuickSight >> Sign-Up
2. Directly from [https://aws.amazon.com/quicksight](https://aws.amazon.com/quicksight)

Once you click sign-up, need to select an edition. Recommended to use Enterprise Edition for all feature/functionality.

### Authentication
AuthN options:
- Role Based Federation (SSO)
  - Federation via SAML
  - Federation via IAM
  - Direct via email invitation
- Active Directory

### VPC Connectivity
Refer to the diagram below
![qs_vpc_architecture](/images/qs_vpc_architecture.png)

- QS sits in its own VPC and when you enable VPC connection, it automatically creates an ENI in the target VPC

### Row Level Security (RLS)
- RLS allows you to have a different view of the same dashboard based on who logs into it.
- RLS works by leveraging a map between users and a column within the dataset, this acts as a filter. Consider the below example:

|  username | businessunit      |
| :----:    | :----:            |
| aashish   | Sales             |
| mansi     | Marketing         |
| bumblebee | Sales, Marketing  |
| optimus   |                   |

`username` matches the user within QS
`businessunit` matches a column in the dataset

So based on who logs in, the data will be filtered, in above example aashish will see data for Sales only. 
Q. What if a user needs access to both Sales & Marketing data?
A. This is solved by either mentioning both Sales & Marketing in the same row or separate rows for the same user. Example: see bumblebee configuration.

Q. What if a user needs access to all data?
A. For users who should have access to everything, leave the column name blank. Example: see optimus

Q. Where is this mapping stored?
A. Store this within the same database or object store like S3.

For production use cases, it makes sense to group users and use group level rules. Example:

| groupname | businessunit |
| :----:    | :----:       |
| Sales     | Sales        |
| Marketing | Marketing    |
| HR        | HR           |
| VPs       |              |

##### Configuration
1. Import the RLS as a dataset as you'd do for any other dataset.
2. Configure RLS using the `Permissions` button on
![rls](/images/rls.png)

#### Common Architectures
Serverless data lake & analytics
![serverless_data_lake_analytics](/images/serverless_data_lake_analytics.png)

ETL, Data Warehousing & Analytics
![etl_datawarehouse](/images/etl_datawarehouse.png)

Auditing & Logging
![qs_audit_logging](/images/qs_audit_logging.png)

## Dashboard Embedding
### Steps to embed a QS dashboard in your application
![steps_embed_dashboard](/images/steps_embed_dashboard.png)

#### Step 1
- Configure & publish dashboards
- Whitelist domain as QS Admin
![whitelist_domain](/images/whitelist_domain.png)

#### Step 2
- Add an IAM role with below policy that will allow viewing of dashboards with reader permissions
```json
{
    "version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "quicksight:RegisterUser",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "quicksight:GetDashboardEmbedUrl",
            "Resource": [
                "arn:aws:quicksight:us-west-2:012345678912:dashboard/3b577ce8-819a-43a1-ade8-0402699b61a2",
                "arn:aws:quicksight:us-west-2:012345678912:dashboard/f7a1c1c0-84df-4e77-86bb-9a95714c6d2e"
            ]
        }
    ]
}
```
#### Step 3
- Based on the authN scheme setyp within AWS account, assume the role setup in Step 2 for each of the users. This ensures that every user is authenticated for access, and can also be provided distinct views of the data (row level security) or personalized defaults (dynamic defaults) if needed. Below is the AWS CLI example, however within the application use the SDK

```bash
aws sts assume-role --role-arn "arn:aws:iam::AWSacctnumber:role/QuickSightEmbed" --role-session-name QSViewer
```

**Note**: Every user should be registered in QS, use the below CLI command or use the QS Admin page for registering users
```bash
aws quicksight register-user \
--aws-account-id AWSacctnumber \
--namespace default \
--identity-type IAM \
--iam-arn "arn:aws:iam::AWSacctnumber:role/QuickSightEmbed" \
--user-role READER \
--session-name "QSViewer" \
--email clark.kent@superman.com \
--region us-west-2
```

Next, call the QS `get-dashboard-embed-url` API to get a signed dashboard URL specific to the user who is being served this dashboard. Below is a CLI example, the application can use the SDK instead
```bash
aws quicksight get-dashboard-embed-url \
--aws-account-id AWSacctnumber \
--dashboard-id 3b577ce8-819a-43a1-ade8-0402699b61a2 \
--identity-type IAM
```
#### Step 4
Use the QS [JavaScript SDK](https://www.npmjs.com/package/amazon-quicksight-embedding-sdk) to specify the div tag where the dashboard should be embedded, and the size allocated for the dashboard. You can pass parameters into the dashboard via the SDK, and also capture loading and error states of the dashboard in order to provide customized messages in your application.

Example for using the javascript sdk to embed the signed url you obtain from AWS
```
<script type="text/javascript" src="quicksight-embedding-js-dsk.min.js"></script>
    function embedDashboard() {
        var containerDiv = document.getElementById("dashboardContainer");
        var params = {
            url: <signed URL obtained from AWS>,
            container: containerDiv,
            parameters: {
                State: 'CA'
            },
            height: "800px",
            width: "1200px"
        }
    }
```

[Sample Project](https://github.com/aws-samples/amazon-quicksight-embedding-sample)