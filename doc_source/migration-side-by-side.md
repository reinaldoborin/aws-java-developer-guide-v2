# Use the SDK for Java 1\.x and 2\.x side\-by\-side<a name="migration-side-by-side"></a>

You can use both versions of the AWS SDK for Java in your projects\.

The following shows an example of the `pom.xml` file for a project that uses Amazon S3 from version 1\.x and DynamoDB from version 2\.16\.1\.

**Example of POM**  
This example shows a `pom.xml` file entry for a project that uses both 1\.x and 2\.x versions of the SDK\.  

```
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.amazonaws</groupId>
            <artifactId>aws-java-sdk-bom</artifactId>
            <version>1.12.1</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <dependency>
          <groupId>software.amazon.awssdk</groupId>
          <artifactId>bom</artifactId>
          <version>2.16.1</version>
          <type>pom</type>
          <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>

<dependencies>
    <dependency>
      <groupId>com.amazonaws</groupId>
      <artifactId>aws-java-sdk-s3</artifactId>
    </dependency>
    <dependency>
      <groupId>software.amazon.awssdk</groupId>
      <artifactId>dynamodb</artifactId>
    </dependency>
</dependencies>
```