# Amazon S3

S3 is an object storage service from Amazon Web Services that offers industry-leading scalability, data availability, security, and performance. This means customers of all sizes and industries can use it to store and protect any amount of data for a range of use cases, such as websites, mobile applications, backup and restore, archive, enterprise applications, IoT devices, and big data analytics.

## Object Structure

An object consists of the following:

- **Key** – The name that you assign to an object. You use the object key to retrieve the object.
- **Version ID** – Within a bucket, a key and version ID uniquely identify an object.
- **Value** – The content that you are storing.
- **Metadata** – A set of name-value pairs that you can store with the object. Metadata is not encrypted while stored in Amazon S3.
- **Subresources** – Used to store object-specific information such as Access Control Information and Storage Class.
- **Access Control Information** – Information that controls access to the object.

## Amazon S3 Transfer Acceleration (S3TA)

Amazon S3 Transfer Acceleration (S3TA) can speed up content transfers to and from Amazon S3 by as much as 50-500 for long-distance transfer of larger objects. Customers who have either web or mobile applications with widespread users or applications hosted far away from their S3 bucket can experience long and variable upload and download speeds over the Internet. S3 Transfer Acceleration (S3TA) reduces the variability in Internet routing, congestion, and speeds that can affect transfers, and logically shortens the distance to S3 for remote applications.

S3TA improves transfer performance by routing traffic through Amazon CloudFront globally distributed Edge Locations and over AWS backbone networks, and by using network protocol optimizations. For applications interacting with your Amazon S3 buckets through the S3 API from outside of your bucket’s region, S3TA helps avoid the variability in Internet routing and congestion. It does this by routing your uploads and downloads over the AWS global network infrastructure, so you get the benefit of AWS network optimizations.

There are no S3 data transfer charges when data is transferred in from the internet. Also with S3TA, you pay only for transfers that are accelerated.

## Notifications

The Amazon S3 notification feature enables you to receive notifications when certain events happen in your bucket. To enable notifications, you must first add a notification configuration that identifies the events you want Amazon S3 to publish and the destinations where you want Amazon S3 to send the notifications (can be SNS, SQS (standard, not FIFO), Lambda).

Amazon S3 event notifications can be leveraged to automatically trigger **AWS Glue DataBrew jobs** when new data is uploaded to an S3 bucket. This integration enables **real-time data preparation workflows**, ensuring datasets are always up-to-date for analysis. By automatically initiating data processing tasks when new files arrive, the process can be streamlined for greater efficiency in data management and analytics.

## Compliance and Security

- **Bucket Policies**: resource-based access control policy that you attach directly to an Amazon S3 bucket. It defines the permissions that are granted to specific actions (like `GET`, `PUT`, `DELETE`) on the objects within the bucket. These policies use JSON syntax and can grant or deny permissions to AWS IAM users, groups, or roles, as well as public access or access from specific AWS accounts.

- **Access Control Lists (ACLs)**: provide a simple way to control access to objects or buckets. They allow you to specify which users or groups can perform specific actions on S3 resources. While effective for simpler use cases, **bucket policies** or **IAM policies** are usually preferred for more complex or flexible access control.

- **S3 Object Lock**: Amazon S3 Object Lock is an Amazon S3 feature that allows you to store objects using a write once, read many (WORM) model. You can use WORM protection for scenarios where it is imperative that data is not changed or deleted after it has been written. Whether your business has a requirement to satisfy compliance regulations in the financial or healthcare sector, or you simply want to capture a golden copy of business records for later auditing and reconciliation, Amazon S3 Object Lock is the right tool for you. Object Lock can help prevent objects from being deleted or overwritten for a fixed amount of time or indefinitely.
- **Encryption**:
    - **Server-Side Encryption**: Request Amazon S3 to encrypt your object before saving it on disks in its data centers and then decrypt it when you download the objects.
    - SSE-S3 (S3-Managed Keys): S3 manages all keys and handles encryption and decryption automatically. Uses AES-256 encryption.
    - SSE-KMS (AWS Key Management Service-Managed Keys): Uses AWS KMS to manage the encryption keys, allowing for more control and auditing. Supports key rotation and additional access controls through AWS KMS.
    - SSE-C (Customer-Provided Keys): You provide your own encryption keys for S3 to use when encrypting and decrypting objects. S3 does not store the keys; they must be provided with each request.
    - **Client-Side Encryption**: Client-side encryption is the act of encrypting your data locally to help ensure its security in transit and at rest. To encrypt your objects before you send them to Amazon S3, use the Amazon S3 Encryption Client. Amazon S3 receives your objects already encrypted; Amazon S3 does not play a role in encrypting or decrypting your objects.
    - **AWS Key Management Service (AWS KMS)**: Allows you to manage your server-side encryption keys for encryption.
- **AWS CloudTrail**: log access requests to the S3 bucket for full visibility into who is accessing the data and what actions they are taking.

To securely access data in Amazon S3 with **AWS Glue** while maintaining strict access control and auditing, it's critical to implement a combination of:

- **Server-Side Encryption (SSE)** with **AWS KMS-managed keys** for data encryption.
- Configure the **AWS Glue service role** with the necessary permissions to use the KMS key.

> Note: You can enforce that all objects are encrypted by adding a condition to the bucket policy that denies any PUT request that does not include the `x-amz-server-side-encryption` header.

## Amazon S3 Storage Classes

Amazon S3 offers a range of storage classes to meet different data storage needs, providing options to optimize cost, performance, and durability based on access frequency and retention requirements. Here are the key storage classes:

- **S3 Standard**:
    - **Use Case**: Frequently accessed data such as active content, mobile apps, and gaming files.
    - **Features**: High durability (99.999999999% or 11 9’s), low latency, and high throughput.
    - **Cost**: Higher storage cost for low-latency access and high performance.

- **S3 Intelligent-Tiering**:
    - **Use Case**: Data with unpredictable access patterns; ideal for cases where access frequency varies over time.
    - **Features**: designed to optimize costs by automatically moving data to the most cost-effective access tier, without performance impact or operational overhead. It works by storing objects in two access tiers: one tier that is optimized for frequent access and another lower-cost tier that is optimized for infrequent access.
    - **Minimum Storage Duration**: 30 days.
    - **Cost**: Small monitoring fee per object, but no additional charges for tier transitions.

- **S3 Standard-IA (Infrequent Access)**:
    - **Use Case**: Infrequently accessed data requiring immediate availability when needed, such as backups and disaster recovery files.
    - **Features**: High durability and availability, but optimized for lower storage costs with a retrieval charge.
    - **Minimum Storage Duration**: 30 days.
    - **Cost**: Lower storage cost than S3 Standard; retrieval fees apply.

- **S3 One Zone-IA**:
    - **Use Case**: Data that does not require multi-AZ resilience and can be re-created if lost, such as temporary or easily reproducible datasets.
    - **Features**: Lower-cost option than S3 Standard-IA, storing data in a single Availability Zone.
    - **Minimum Storage Duration**: 30 days.
    - **Cost**: 20% lower than S3 Standard-IA, with a minimum storage duration of 30 days.

- **S3 Glacier**:
    - **Use Case**: Long-term archival storage where retrieval speed can be flexible, such as compliance and regulatory data.
    - **Features**: Ultra-low storage cost with three retrieval options—**Expedited**, **Standard**, and **Bulk**—each with different retrieval times.
    - **Minimum Storage Duration**: 90 days.
    - **Cost**: Lowest cost for archival data with retrieval charges that vary by retrieval speed.

- **S3 Glacier Deep Archive**:
    - **Use Case**: Data that is rarely accessed, typically once or twice a year, and can afford retrieval times of 12 to 48 hours.
    - **Features**: Lowest-cost storage class in S3, offering extremely low storage rates.
    - **Minimum Storage Duration**: 180 days.
    - **Cost**: Significantly cheaper than S3 Glacier, ideal for data meant to be stored for extended durations.

### Amazon S3 Analytics

Amazon S3 Analytics helps you better understand your storage usage and access patterns, allowing you to make informed decisions about the most cost-effective storage solutions. The service works by analyzing your S3 data and generating recommendations based on the frequency of access and other metrics.

S3 Analytics examines access patterns to determine whether transitioning objects from S3 Standard to S3 Standard-IA (or vice versa) would result in cost savings. It doesn't work for One Zone-IA or Glacier.

## Lifecycle Management

Amazon S3’s **Lifecycle Management** feature enables automated transitioning and deletion of objects over time based on specific rules. This is especially useful for data archiving, cost optimization, and compliance with data retention policies.

- **S3 Lifecycle Configuration**: You can define rules to manage data storage costs and organize your data based on its access needs. For example:
    - **Transition Rules**: Move objects to a lower-cost storage class after a specified number of days. For instance, transition objects to S3 Standard-IA after 30 days or to S3 Glacier after 365 days.
    - **Expiration Rules**: Automatically delete objects after a specified time period, which can help with compliance and data hygiene.
    - **Use Case**: Efficiently manage data across its lifecycle to reduce storage costs by moving data to the most cost-effective storage class based on access frequency and retention requirements.

Amazon S3 Storage Classes can be configured at the object level and a single bucket can contain objects stored in multiple storage classes. You can upload objects directly to a class storage, or use S3 Lifecycle policies to transfer objects.

## S3 Features for Developers

To host a static website on Amazon S3, you configure an Amazon S3 bucket for website hosting and then upload your website content to the bucket. When you configure a bucket as a static website, you enable static website hosting, set permissions, and add an index document. Depending on your website requirements, you can also configure other options, including redirects, web traffic logging, and custom error documents.

When you configure your bucket as a static website, the website is available at the AWS Region-specific website endpoint of the bucket. Depending on your Region, your Amazon S3 website endpoints follow one of these two formats: s3-website dash (-) Region <http://bucket-name.s3-website-Region.amazonaws.com>, s3-website dot (.) Region <http://bucket-name.s3-website.Region.amazonaws.com> These URLs return the default index document that you configure for the website.

All Amazon S3 GET, PUT, and LIST operations, as well as operations that change object tags, ACLs, or metadata, are strongly consistent. What you write is what you will read, and the results of a LIST will be an accurate reflection of what is in the bucket.

- **S3 Access Points**: Amazon S3 Access Points, a feature of S3, simplify data access for any AWS service or customer application that stores data in S3. With S3 Access Points, customers can create unique access control policies for each access point to easily control access to shared datasets. You can also control access point usage using AWS Organizations support for AWS SCPs.

- **S3 Object Lambda**: With S3 Object Lambda you can add your own code to S3 GET requests to modify and process data as it is returned to an application. For the first time, you can use custom code to modify the data returned by standard S3 GET requests to filter rows, dynamically resize images, redact confidential data, and much more. Powered by AWS Lambda functions, your code runs on infrastructure that is fully managed by AWS, eliminating the need to create and store derivative copies of your data or to run expensive proxies, all with no changes required to your applications.

> Tip: S3 Object Lambda enables dynamic data transformation in response to S3 GET requests, making it possible to redact PII on-the-fly for specific user groups without needing to maintain multiple data copies.

## Optimizing Data Access and Retrieval

### Amazon S3 Select

Amazon S3 Select is a new Amazon S3 capability designed to pull out only the data you need from an object, which can dramatically improve the performance and reduce the cost of applications that need to access data in Amazon S3. You cannot use Byte Range Fetch parameter with S3 Select to traverse the Amazon S3 bucket and get the first bytes of a file.

Please note that with Amazon S3 Select, you can scan a subset of an object by specifying a range of bytes to query using the ScanRange parameter. This capability lets you parallelize scanning the whole object by splitting the work into separate Amazon S3 Select requests for a series of non-overlapping scan ranges. Use the Amazon S3 Select ScanRange parameter and Start at (Byte) and End at (Byte).

### Multipart upload

S3 Multipart upload feature with MD5 checksum validation for each part. This method allows for the breaking down of large files into smaller parts, each uploaded separately, with the integrity of each part verified via MD5 checksums. This ensures that each segment of the file is uploaded successfully and that the content matches exactly between the on-premises version and the one stored in S3. Multipart upload not only provides a robust solution for preventing file corruption during upload but also offers an efficient and cost-effective way to transfer large files by allowing parallel uploads of parts and reducing the impact of failures. This method aligns with the company's need for reliability in file uploads while maintaining minimal costs.

## Data Replication

- **Batch Replication**:  This method allows you to replicate existing objects that were created before replication was enabled, as well as recover from previous replication failures. You use Batch Operations to replicate these objects in bulk, allowing you to fix any replication gaps.
- **Cross-Region Replication (CRR)**: This feature automatically replicates data from an Amazon S3 bucket in one AWS region to another region. Note: By default, CRR only applies to new objects created after replication is enabled. Configuring replication for existing objects may require assistance from AWS Support.
- **Live Replication**: This provides continuous, real-time replication of data between buckets. Any changes made to objects in the source bucket are automatically and immediately replicated to the destination bucket. To set up live replication for existing objects, you need to contact AWS Support for assistance.

> Note: Versioning must be enable for replication.

While batch replication deals with replicating existing objects and fixing past replication issues, live replication focuses on ongoing, real-time replication of new objects as they are created or modified. The main difference is that live replication operates continuously, while batch replication is a one-time fix.

You cannot directly use the AWS S3 console to configure cross-Region replication for existing objects. By default, replication **only supports copying new Amazon S3 objects after it is enabled using the AWS S3 console**. If you want to enable live replication for existing objects for your bucket, you must contact AWS Support and raise a support ticket. This is required to ensure that replication is configured correctly.

## Cross-Origin Resource Sharing (CORS)

CORS is needed when a web application on one domain (e.g., <www.example.com>) tries to access resources stored in an S3 bucket on another domain (e.g., storage.example.com). By setting up CORS on the S3 bucket, you allow cross-domain access to the resources, enabling your client-side web application to interact with the objects in the bucket.

## Versioning and Delete Markers

Versioning is a feature in Amazon S3 that allows you to preserve, retrieve, and restore every version of every object stored in a bucket. When versioning is enabled for a bucket, S3 keeps multiple versions of an object, even if the object is overwritten or deleted. It is applied at the bucket level.

Once you enable versioning on a bucket, S3 begins to keep track of all versions of every object uploaded to that bucket. This includes new uploads, modified versions, and deletions. Each time you upload an object with the same name as an existing object in a versioned bucket, S3 creates a new version. Every object version has a unique version ID. This way you can restore an earlier version of an object, even if it was overwritten or deleted. This gives you the ability to recover from accidental deletions or overwrites.

When versioning is enabled on a bucket, deleting an object does not permanently remove it. Instead, S3 creates a delete marker, which is a special version that acts as a placeholder indicating the object has been deleted.

## Extra

### S3 Sync Command

The aws S3 sync command uses the CopyObject APIs to **copy objects between Amazon S3 buckets**. The sync command lists the source and target buckets to identify objects that are in the source bucket but that aren’t in the target bucket. The command also identifies objects in the source bucket that have different LastModified dates than the objects that are in the target bucket. The sync command on a versioned bucket copies only the current version of the object, previous versions aren’t copied. If the operation fails, you can run the sync command again without duplicating previously copied objects.

### File Transfer Optimization

- **Range HTTP Header**: This allows you to retrieve specific byte ranges of an object to improve throughput and retry speeds. Using the Range HTTP header in a GET Object request, you can fetch a byte-range from an object, transferring only the specified portion. You can use concurrent connections to Amazon S3 to fetch different byte ranges from within the same object. This helps you achieve higher aggregate throughput versus a single whole-object request. Fetching smaller ranges of a large object also allows your application to improve retry times when requests are interrupted.

### Amazon S3 Best Practices

- Having a staging area in Amazon S3 allows for the preprocessing of data before it moves into other stages of the pipeline. For example, AWS Glue can be used to handle transformations and schema modifications in this staging area. This approach provides a layer of abstraction and automation, handling schema changes more fluidly and ensuring that data is prepared in the correct format for downstream processing.
- Partitioning data in Amazon S3 by common query dimensions—such as date, source, or region—can significantly improve query performance and reduce the cost of queries in Amazon Athena. Converting the data into an efficient columnar format, such as Apache Parquet, further optimizes it for querying. This approach ensures that queries are faster, more efficient, and cost-effective by minimizing the amount of data read from S3.
- Separating sensitive data into its own S3 bucket is part of AWS best practices for security and data governance.
- When structuring large datasets in Amazon S3, consider using open-source formats like **Apache Iceberg** or **Delta Lake**. These formats offer transactional capabilities, allowing you to apply **Change Data Capture (CDC)** updates without needing to rewrite entire files.

> Did you know? **Apache Iceberg** is a modern open table format designed for very large analytic datasets. It manages collections of files as tables, providing support for record-level insert, update, delete, and time travel queries. It integrates well with AWS Glue for schema evolution and CDC updates, making it an ideal choice for data lakes stored on S3.

# S3 Glacier

Amazon S3 Glacier is an archival storage class designed for long-term data storage. It offers various retrieval options, each with different performance and cost trade-offs. S3 Glacier provides a variety of storage classes for retrieval depending on how quickly you need to access the data.

### Retrieval Methods for S3 Glacier

- **Expedited Retrievals**: Ideal for urgent data access needs. Expedited retrievals offer the fastest way to access archived data in Glacier, but they incur higher costs than other retrieval methods. This is suitable when you have immediate data requirements.

- **S3 Glacier Select**: This feature allows you to run SQL queries directly on your archived data in Glacier, enabling you to retrieve only the specific records you need without the need to restore the entire dataset.

### S3 Glacier Storage Classes

- **Amazon S3 Glacier Instant Retrieval**: Provides millisecond retrieval for data that is accessed infrequently, such as quarterly data access. This class has a minimum storage duration of 90 days.

- **Amazon S3 Glacier Flexible Retrieval**: Formerly known as Amazon S3 Glacier, this class supports:
    - **Expedited** (1 to 5 minutes),
    - **Standard** (3 to 5 hours),
    - **Bulk** (5 to 12 hours), all at no additional retrieval cost.

    The minimum storage duration is 90 days.

- **Amazon S3 Glacier Deep Archive**: The lowest-cost storage class for long-term storage. It supports:
    - **Standard Retrieval** (12 hours), and
    - **Bulk Retrieval** (48 hours).

    It has a minimum storage duration of 180 days.
