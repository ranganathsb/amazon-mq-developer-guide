# Limits in Amazon MQ<a name="amazon-mq-limits"></a>

This topic lists limits within Amazon MQ\. Many of the following limits can be changed for specific AWS accounts\. To request an increase for a limit, see [AWS Service Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *Amazon Web Services General Reference*\.


+ [Limits Related to Brokers](#broker-limits)
+ [Limits Related to Configurations](#configuration-limits)
+ [Limits Related to Users](#activemq-user-limits)
+ [Limits Related to Data Storage](#data-storage-limits)
+ [Limits Related to API Throttling](#api-throttling-limits)

## Brokers<a name="broker-limits"></a>

The following table lists limits related to Amazon MQ [brokers]()\.


| Limit | Description | 
| --- | --- | 
| Broker name |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/amazon-mq/latest/developer-guide/amazon-mq-limits.html)  | 
| Brokers per [broker instance type](amazon-mq-basic-elements.md#broker-instance-types), per AWS account, per region | 20 | 
| Broker configuration history depth | 10 | 
| Security groups per broker | 5 | 
| Destinations \(queues and topics\) monitored in CloudWatch | CloudWatch monitors only the first 200 destinations\. | 

## Configurations<a name="configuration-limits"></a>

The following table lists limits related to Amazon MQ [configurations]()\.


| Limit | Description | 
| --- | --- | 
| Configuration name |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/amazon-mq/latest/developer-guide/amazon-mq-limits.html)  | 
| Configurations per AWS account | 1,000 | 
| Revisions per configuration | 300 | 

## Users<a name="activemq-user-limits"></a>

The following table lists limits related to Amazon MQ [users](amazon-mq-basic-elements.md#user)\.


| Limit | Description | 
| --- | --- | 
| Username |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/amazon-mq/latest/developer-guide/amazon-mq-limits.html)  | 
| Password |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/amazon-mq/latest/developer-guide/amazon-mq-limits.html)  | 
| Users per broker | 100 | 
| Groups per user | 5 | 

## Data Storage<a name="data-storage-limits"></a>

The following table lists limits related to Amazon MQ data storage\.


| Limit | Description | 
| --- | --- | 
| Storage capacity per broker | 200 GB | 
| Data store | Amazon MQ uses [Apache KahaDB](http://activemq.apache.org/kahadb.html) as its data store\. Other data stores, such as JDBC and LevelDB, aren't supported\. | 

## API Throttling<a name="api-throttling-limits"></a>

The following throttling limits are aggregated per AWS account, *across all [Amazon MQ APIs](http://docs.aws.amazon.com/amazon-mq/latest/api-reference/)* to maintain service bandwidth\.

**Note**  
These limits don't apply to ActiveMQ messaging APIs\.


| Bucket Size | Refill Rate per Second | 
| --- | --- | 
| 100 | 15 | 