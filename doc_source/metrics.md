# Enabling SDK metrics for the AWS SDK for Java<a name="metrics"></a>

With the AWS SDK for Java 2\.x, you can collect metrics about the service clients in your application, analyze the output in Amazon CloudWatch, and then act on it\.

By default, metrics collection is disabled in the SDK\. This topic helps you to enable and configure it\.

## Prerequisites<a name="prerequisitesmetrics"></a>

Before you can enable and use metrics, you must complete the following steps:
+ Complete the steps in [Set up the AWS SDK for Java 2\.x](setup.md)\.
+ Configure your project dependencies \(for example, in your `pom.xml` or `build.gradle` file\) to use version `2.14.0` or later of the AWS SDK for Java\.

  To enabling publishing of metrics to CloudWatch, also include the artifactId `cloudwatch-metric-publisher` with the version number `2.14.0` or later in your project’s dependencies\.

  For example:

  ```
  <project>
    <dependencyManagement>
     <dependencies>
        <dependency>
          <groupId>software.amazon.awssdk</groupId>
          <artifactId>bom</artifactId>
          <version>2.14.0</version>
          <type>pom</type>
          <scope>import</scope>
        </dependency>
     </dependencies>
    </dependencyManagement>
    <dependencies>
     <dependency>
        <groupId>software.amazon.awssdk</groupId>
        <artifactId>cloudwatch-metric-publisher</artifactId>
        <version>2.14.0</version>
     </dependency>
    </dependencies>
  </project>
  ```

## How to enable metrics collection<a name="how-to-enable-metrics-collection"></a>

You can enable metrics in your application for a service client or on individual requests\.

### Enable metrics for a specific request<a name="enable-metrics-for-a-specific-request"></a>

The following code snippet shows how to enable the CloudWatch metrics publisher for a request to Amazon DynamoDB\. It uses the default metrics publisher configuration\.

```
MetricPublisher metricsPub = CloudWatchMetricPublisher.create();
DynamoDbClient ddb = DynamoDbClient.create();
ddb.listTables(ListTablesRequest.builder()
  .overrideConfiguration(c -> c.addMetricPublisher(metricsPub))
  .build());
```

### Enable metrics for a specific service client<a name="enable-metrics-for-a-specific-service-client"></a>

The following code snippet shows how to enable a CloudWatch metrics publisher with default settings for a service client\.

```
MetricPublisher metricsPub = CloudWatchMetricPublisher.create();

DynamoDbClient ddb = DynamoDbClient.builder()
          .overrideConfiguration(c -> c.addMetricPublisher(metricsPub))
          .build();
```

The following snippet demonstrates how to use a custom configuration for the metrics publisher for a specific service client\. The customizations include loading a specific credentials profile, specifying a different region than the service client, and customizing how often the publisher sends metrics to CloudWatch\.

```
MetricPublisher metricsPub = CloudWatchMetricPublisher.builder()
    .cloudWatchClient(CloudWatchAsyncClient.builder()
                                           .region(Region.US_WEST_2)
                                           .credentialsProvider(ProfileCredentialsProvider.create("cloudwatch"))
                                           .build())
    .uploadFrequency(Duration.ofMinutes(5))
    .build();

DynamoDbClient ddb = DynamoDbClient.builder()
          .overrideConfiguration(c -> c.addMetricPublisher(metricsPub))
          .build();
```

## What information is collected?<a name="what-information-is-collected"></a>

Metrics collection includes the following:
+ Number of API requests, including whether they succeed or fail
+ Information about the AWS services you call in your API requests, including exceptions returned
+ The duration for various operations such as Marshalling, Signing, and HTTP requests
+ HTTP client metrics, such as the number of open connections, the number of pending requests, and the name of the HTTP client used

**Note**  
The metrics available vary by HTTP client\.

For a complete list, see [Service client metrics](metrics-list.md)\.

## How can I use this information?<a name="how-can-i-use-this-information"></a>

You can use the metrics the SDK collects to monitor the service clients in your application\. You can look at overall usage trends, identify anomalies, review service client exceptions returned, or to dig in to understand a particular issue\. Using Amazon CloudWatch, you can also create alarms to notify you as soon as your application reaches a condition that you define\.

For more information, see [Using Amazon CloudWatch Metrics](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html) and [Using Amazon CloudWatch Alarms](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the [Amazon CloudWatch User Guide](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

**Topics**