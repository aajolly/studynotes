# IoT on AWS

#### Question to ask for an IoT engagement
Q1. If you knew **the state of every thing** and could **reason on top of that data** ... what **problems** would you solve?

AWS IoT Use Cases
![aws_iot_use_cases](/images/what_customers_do_with_aws_iot.png)

- Operational Efficiency: IoT data decreases OpEx
- Revenue Growth: IoT data drives business growth

### Fundamentals of AWS IoT
Business Scenario: An ice cream company that manufactures ice cream and trucks them to stores throughout Seattle area. One of the biggest challenges they face is food spoilage (30%) while ice cream is in transit. The problem could be with the refrigeration units turning off, high summer temperatures, truck doors being left open, electrical issues, or something they simply haven't thought of yet. They just don't know. That's why the customer wants out help.

To solve the listed problems, we'll use a micro controller to record measurements along with timestamp for the following scenarios:
1. The freezer door with a digital I/O switch so that you can tell how long the door is open. 
2. The on-board freezer’s internal temperature so that we see how well the trailer maintains its temperature. 
3. The voltage going to the freezer unit so that we know if the freezer is being starved for power. And finally, a time and date stamp, which will tell you exactly when these problems occur so that you can correlate them with specific drivers and weather patterns.
![micro_controller](/images/micro_controller.png)

As we collect this information, use the following IoT best practices:

1. Take readings every 5 seconds to know when different events occur. This way, we will have the resolution we need without taking up too much bandwidth or data storage.
2. With each reading, also take measurements from all sensors and assemble them into a single string to send to AWS IoT. Combining measurements into a single message is an efficient way of getting time series data to the AWS Cloud.
3. Finally, because there may be an opportunity to expand this solution in the future, it makes sense to track this in UTC and not worry about timezone conversions.

**Additionall data measurements?**
Apart from the data gathered from the 3 sensors and track more information about the truck, what other kinds of information should be considered?
Communication can be bi-directional. You can collect information from the truck, and you can also send commands to the truck and perform actions based on the information received.

What data-driven actions do you think might be useful in a food-service distrubution truck?
- Send alerts to the driver via a console in the truck that provides track light overview of the health. Is the door closed or not?

Additional sensors can also provide useful data and help answer the following questions:
1. Does the issue happen when the truck is stuck in traffic, loading area at a customer site where it takes longer than usual?
2. Is it specific days/routes when the spoilage happens?
3. Does the issue only arise on hot days?

Consider a GPS chip to provide location data, an external temperature sensor located under the chassis can give you reliable outside temperature data.

**Business Outcome**:
The goal is to achieve a viable business outcome for the small ice cream distributor scenario. You need to collect data from a sensor and format the data for consumption by the non-technical business manager. The business manager reviews the data to make process improvement decisions. To gather the data required by the customer, the following tasks must be completed:

1. Establish communication with the IoT sensor.
2. Collect and transform the data.
3. Summarize and format the data for the business manager.

High Level Solution within AWS to achieve the business outcome:
1. Register a sensor within AWS by adding a device using the AWS IoT console.
2. Secure the sensor by using roles, policies, and certificates, in the AWS Identity and Access Management (IAM).
3. Establish communication with the sensor by modifying an existing policy to suit the business objective.
4. Collect data from the sensor by establishing communication to the device and collecting data.
5. Transform and visualize data by using AWS IoT rules engine to transfer the data into AWS IoT Analytics and Amazon QuickSight. This enables you to transform and visualize the data into an easily readable format for the business decision-maker.

Review best practices by identifying your IoT accomplishments and discussing AWS best practices for telemetry.

Now that the high level business solution is layed out, lets dive into the services that would help us build the solution and solve the customer's problems.

#### AWS IoT Components
##### AWS IoT Core
AWS IoT Core provides secure, bidirectional communication between internet-connected devices, such as sensors, actuators, embedded microcontrollers, or smart appliances, and the AWS Cloud. This enables you to collect, store, and analyze telemetry data from multiple devices. You can also create applications that enable your users to control these devices from their phones or tablets. AWS IoT Core is composed of six main components: 

1. **Identity & Access Management**
AWS IoT Core provides a secure communication channel for devices to communicate with each other and other services. AWS IoT provides authentication by offering the following options: 

Certificates for mutual authentication by using Message Queuing Telemetry Transport (MQTT) over Transport Layer Security (TLS) v1.2 
SigV4 over HTTP 
MQTT over WebSockets, which is similar to other AWS services. 
You can also use custom authentication tokens that are provided by your authentication or authorization service. AWS IoT also provides flexible authorization options and fine-grained access control through JavaScript Object Notation (JSON) policies.
![iam_iot](/image/iot_iam.png)

2. **Device Gateway**
Securely connects IP-connected devices and edge gateways to the AWS Cloud and other devices at scale.

3. **Message Broker**
Procsses and routes data messages to the AWS Cloud. It processes and routes data from your devices into AWS IoT Core. Message broker is scalable, has low-latency, and provides reliable message routing. It also uses a publish and subscribe model to decouple devices and applications.
The message broker allows two-way message streaming between devices and applications and provides an opportunity for data transformation, rerouting, and enhancement with external data sources.

4. **Rules Engine**
Triggers actions in the AWS Cloud, such as writing data to an S3 bucket, invoking a lambda function, or sending a message to an SNS topic. 

5. **Device Shadow**
Often referred to as a thing shadow or a digital twin. Use it to get and set the state of a device over MQTT or HTTP, regardless of whether the device is connected to the Internet. Each device's shadow is uniquely identified by the name of the corresponding thing.
Device Shadow Lifecycle
![device_shadow_lifecycle](/images/device_shadow_lifecycle.png)

6. **The registry**
It's database of devices. Using the registry for your devices is optional; however, the registry helps you manage your device ecosystem effectively and acts as a repository for device certificates. The registry also enables you to search for your things based on attributes and tags.
Information about a thing is stored in the registry as JSON data.
```json
"version": 3,
"thingName": "truckSensor01",
"defaultClientId": "truckSensor01",
"thingTypeName": "sensor_DorAndPower",
"attributes": {
    "deviceID": "T001",
    "powerRequirements": "12v"
}
```

#### Device Security
AWS IoT Core uses certificates to grant access from the device to the core components. After the devices have been authenticated, AWS IoT Core policies control which topics the device can publish and subscribe. Then AWS Identity and Access Management (IAM) roles allow AWS IoT to access other AWS resources in your account on your behalf. For example, if you want to have a device publish its state to a DynamoDB table, IAM roles allow AWS IoT to interact with Amazon DynamoDB. 

All communication is encrypted with Transport Layer Security (TLS) version 1.2 to ensure that your application protocols remain secure and confidential. Not all devices support TLSv1.2, so when adding devices to your fleet, ensure that they support TLSv1.2.

**A note on IoT Policies**
AWS IoT policies primarily control an identity’s access to the IoT data plane. In AWS IoT, the data plane enables you to send data to and receive data from AWS IoT. This means that the data plane defines whether you can connect to the message broker, send or receive MQTT messages, or publish or subscribe to a specific topic. 

#### Message Broker
AWS IoT message broker allows clients to communicate with AWS IoT Core and vice versa.
The message broker uses the publish/subscribe model and leverages MQTT for publish/subscribe and the HTTPS protocol to publish. Both protocols are supported via IPv4 & IPv6.
MQTT supports WebSockets for browser-based applications as well.

#### Rules Engine
By definition, a sensor will "sense" as long as it has power. Accumulating every bit of data from a sensor means that you will have a large amount of received data that is not useful for your business objectives. Because of the amount of data collected, you can filter or direct data to different locations in the cloud.

To begin transforming the data, start by learning about the AWS IoT rules engine. The AWS IoT rules engine is one of the primary methods of filtering and directing communication from AWS IoT Core to other AWS services.
![rules_engine_overview](/images/rules_engine_overview.png)

**Rules Engine Language**
The rules engine uses SQL-like statements that are built within a structured development environment to filter and route MQTT messages.
__Example__:
The SQL statement filters the messages, and the roleARN gives AWS IoT permission to write to the Amazon S3 bucket.
```json
{
    "awsIotSqlVersion": "2016-03-23",
    "sql": "SELECT * FROM 'iot/test'",
    "ruleDisabled": false,
    "actions": [
        {
            "s3": {
                "roleArn": "arn:aws:iam::012345678912:role/aws_iot_s3",
                "bucketName": "my-bucket",
                "key": "myS3Key"
            }
        }
    ]
}
```

#### AWS IoT Analytics
To achieve the objective around food spoilage, we must analyze the data to determine where the spoilage occurs and then present it to the business leaders at the ice cream company in a format that is easy for them to read and understand. AWS IoT Analytics is the tool that helps with formatting and analyze the data.
AWS IoT Terminology
**Channel**: Collects & archives raw, unprocessed message data before publishing this data to a pipeline.
**Pipeline**: A pipeline consumes messages from a channel & enables you to process and filter the messages before storing them in a data store.
**Data store**: A data store is not a database, but is a scalable and queryable repository of your messages. You can have multiple data stores for messages that from came from different devices or locations.
**Dataset**: You retrieve data from a data store by creating a dataset. AWS IoT Analytics enables you to create a SQL dataset or a container dataset. Here, the simple SQL dataset is similar to a materialized view from a SQL database. In fact, you create a SQL dataset by applying a SQL action.
**Dataset contents**: The results. After you create dataset contents, you can view them from the console.

#### AWS IoT Device Management
AWS Device Management allows you to onboard devices at scale.
AWS Device Management Pillars
**Onboard**: Onboard devices at scale.
**Organize**: Organize your devices into logical hierarchies with Thing Groups. Using thing groups allow you to set policies for your devices at an org level, instead of at the device level. AWS IoT Device also provides indexing, which can make searching the AWS IoT Device Registry more efficient and effective. In addition, you can receive notifications when devices are created, updated, or changed.
**Monitor**: Monitoring device activities informs you about changes made to your devices and who made them. You can define the level of granularity to monitor devices and, if needed, focus your monitoring services to be device-specific.
**Update**: Organize and schedule device updates through jobs. You may not want to update all devices at the same time because of bandwidth constraints. AWS IoT jobs help you ensure updates roll out in a controlled manner.
![aws_device_mgmt_summary](/images/aws_device_mgmt_summary.png)

Device Provisioning with an API
![device_provisioning_with_api](/images/device_provisioning_with_api.png)

Whether provisioning devices in advance or on-demand, you need a provisioning template. It's a JSON document that uses parameters to describe the resources your device must use to interact with AWS IoT.