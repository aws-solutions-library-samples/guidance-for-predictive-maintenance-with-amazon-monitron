## Monitron Predictive Maintenance Solution
Reliability managers and technicians in industrial environments such as manufacturing production lines, warehouses, and industrial plants are keen to improve equipment health and uptime to maximize product output and quality. Machine and process failures are often addressed by reactive activity after incidents happen or by costly preventive maintenance, where you run the risk of over-maintaining the equipment or missing issues that could happen between the periodic maintenance cycles. Predictive condition-based maintenance is a proactive strategy that is better than reactive or preventive ones. Indeed, this approach combines continuous monitoring, predictive analytics, and just-in-time action. This enables maintenance and reliability teams to service equipment only when necessary, based on the actual equipment condition.

There have been common challenges with condition-based monitoring to generate actionable insights for large industrial asset fleets. These challenges include but are not limited to: build and maintain a complex infrastructure of sensors collecting data from the field, obtain a reliable high-level summary of industrial asset fleets, efficiently manage failure alerts, identify possible root causes of anomalies, and effectively visualize the state of industrial assets at scale.

Amazon Monitron is an end-to-end condition monitoring solution that enables you to start monitoring equipment health with the aid of machine learning (ML) in minutes, so you can implement predictive maintenance and reduce unplanned downtime. It includes sensor devices to capture vibration and temperature data, a gateway device to securely transfer data to the AWS Cloud, the Amazon Monitron service that analyzes the data for anomalies with ML, and a companion mobile app to track potential failures in your machinery. Your field engineers and operators can directly use the app to diagnose and plan maintenance for industrial assets.

From the operational technology (OT) team standpoint, using the Amazon Monitron data also opens up new ways to improve how they operate large industrial asset fleets thanks to AI. OT teams can reinforce the predictive maintenance practice from their organization by building a consolidated view across multiple hierarchies (assets, sites, and plants). They can combine actual measurement and ML inference results with unacknowledged alarms, sensors or getaways connectivity status, or asset state transitions to build a high-level summary for the scope (asset, site, project) they are focused on.

With the recently launched Amazon Monitron Kinesis data export v2 feature, your OT team can stream incoming measurement data and inference results from Amazon Monitron via Amazon Kinesis to AWS Simple Storage Service (Amazon S3) to build an Internet of Things (IoT) data lake. By leveraging the latest data export schema, you can obtain sensors connectivity status, gateways connectivity status, measurement classification results, closure reason code and details of asset state transition events.

## Architecture overview
The solution you will build in this post combines Amazon Monitron, Kinesis Data Streams, Amazon Kinesis Data Firehose, Amazon S3, AWS Glue, Athena, and Amazon Managed Grafana.

The following diagram illustrates the solution architecture. Amazon Monitron sensors measure and detect anomalies from equipment. Both measurement data and ML inference outputs are exported at a frequency of once per hour to a Kinesis data stream, and they are delivered to Amazon S3 via Kinesis Data Firehose with a 1-minute buffer. The exported Amazon Monitron data is in JSON format. An AWS Glue crawler analyzes the Amazon Monitron data in Amazon S3 at a chosen frequency of once per hour, builds a metadata schema, and creates tables in Athena. Finally, Amazon Managed Grafana uses Athena to query the Amazon S3 data, allowing dashboards to be built to visualize both measurement data and device health status.

### Automatic setup with CloudFormation
You will deploy a CloudFormation template that will provision an environment for your work going forward. The template will setup the AWS resources to export Amazon Monitron data to Amazon Glue and S3 to your AWS account.

This data pipeline will setup the following resources in your AWS account: 
* S3 Bucket for Monitron data Export;
* Firehose to deliver Monitron data from Kinesis stream;
* Kinesis Stream for Monitron data;
* Amazon Glue database and Table for Monitron data catelog.

## Considerations for Practical Implementations
For demonstration purpose, a Grafana dashboard can be used to visualize the Monitron data available from the Amazon Glue database by following this blog (https://aws.amazon.com/blogs/machine-learning/generate-actionable-insights-for-predictive-maintenance-management-with-amazon-monitron-and-amazon-kinesis/). 

## Clean Up procedures
CloudFormation stacks:
To avoid incurring future charges, navigate to the CloudFormation console and delete the CloudFormation stack created from this blog walkthrough. 

S3 Buckets: 

* Navigate  to the S3 console, 
* Empty and delete the S3 bucket created in the second CloudFormation.

Cloudformation:
* Delete the cloudformation you deployed from this repository. 

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

