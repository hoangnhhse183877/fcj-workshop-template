---
title: "Blog 1"
date: 2026-11-27
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Amazon S3 – Cost Optimization, Security, and Automated Data Management on AWS

| Information | Details |
|---|---|
| Author | Nguyen Huu Huy Hoang |
| Submission date | November 27, 2026 |
| Status | Pending approval |
| Platform | [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj) |
| Post URL | To be updated after approval |
| Format | Technical research and knowledge sharing |

## 1. Introduction

While learning about Cloud Computing and AWS services, I found Amazon Simple Storage Service (Amazon S3) to be one of the foundational services with broad applicability in building and operating cloud-based systems.

Amazon S3 not only provides data storage but also supports many features that help businesses manage the data lifecycle, control access, recover data, and automate processing workflows.

In this article, I would like to share how to use Amazon S3 features to address three important challenges in building cloud systems: optimizing costs, securing data, and improving operational efficiency.

## 2. Amazon S3 Overview

Amazon S3 is a scalable object storage service that allows users to store and retrieve data over the internet or through appropriate private network connections.

Data in Amazon S3 is organized around two main components:

- Bucket: A container for storing and managing objects.
- Object: A data object consisting of content, metadata, and an identifying key (Object Key).

Amazon S3 is used in many real-world scenarios, such as:

- Storing images, videos, and documents.
- Data backup and recovery.
- Building data lakes for data analytics.
- Storing data for AI and Machine Learning applications.
- Managing system activity logs.
- Storing resources for web applications.

S3 Standard is designed for 99.999999999% data durability and 99.99% availability over a year.

Durability describes the ability to preserve data over time, whereas availability describes the ability to access and use a service when needed. These are two distinct concepts that should be understood when designing storage systems.

## 3. Real-world Data Management Challenges

Suppose a business is developing an e-commerce system on AWS. The system generates many types of data each day, including:

- Product images.
- Customer invoices.
- Revenue reports.
- Application activity logs.
- Periodic backups.

If all data is stored in the same storage class without lifecycle management policies or appropriate access controls, the business may encounter problems such as:

- Storage costs increasing over time.
- Important data being accidentally deleted or overwritten.
- Risks of unauthorized access to customer information.
- Increased manual data management tasks.
- Difficulties scaling and operating the system.

To address these issues, the business can combine existing Amazon S3 features instead of building every data management mechanism from scratch.

## 4. Cost Optimization with S3 Storage Classes

An important consideration when using Amazon S3 is choosing a storage class suitable for the characteristics of the data.

Amazon S3 offers multiple storage classes for different requirements:

**S3 Standard**

Suitable for frequently accessed data, such as product images and application resources.

**S3 Intelligent-Tiering**

Automatically adjusts data access tiers according to usage frequency. Suitable when future access requirements are difficult to predict.

**S3 Standard-IA (Infrequent Access)**

Suitable for infrequently accessed data that still requires fast retrieval when needed.

**S3 One Zone-IA**

Stores data in a single Availability Zone and is suitable for infrequently accessed data that can be recreated in the event of a failure.

**S3 Glacier Instant Retrieval**

Suitable for long-term, infrequently accessed data requiring low-latency retrieval.

**S3 Glacier Flexible Retrieval**

Suitable for long-term archival data that does not require immediate retrieval.

**S3 Glacier Deep Archive**

Suitable for long-term archival data with very low access frequency and tolerance for longer recovery times.

**S3 Express One Zone**

Designed for applications requiring high performance and low access latency in a single Availability Zone.

Storage class selection should not be based on storage price alone. System designers should also consider retrieval costs, request volume, minimum storage duration, object size, and availability requirements.


![Figure 1. Illustrative classification of data by access frequency and S3 storage class.](/images/blogs/blog1/s3-storage-classes.jpg)

*Figure 1. Illustrative classification of data by access frequency and S3 storage class.*


## 5. Automating Data Management with S3 Lifecycle

In the e-commerce system described earlier, application activity logs are frequently accessed during the first few days for performance monitoring and incident analysis.

Over time, demand for access to this data decreases. Continuing to store all logs in S3 Standard may no longer be cost-effective.

S3 Lifecycle allows rules to be configured to automatically transition storage classes or expire objects based on the data lifecycle.

For example, a business could establish the following policy for application logs:

- Days 0 to 29: Store in S3 Standard.
- After 30 days: Transition to S3 Standard-IA.
- After 90 days: Transition to S3 Glacier Flexible Retrieval.
- After 365 days: Apply expiration if the data is no longer required and retention requirements are satisfied.

Amazon S3 automatically manages this process according to the configured Lifecycle Rule.

However, the following considerations are important during implementation:

- S3 Standard-IA has a minimum billable storage duration of 30 days.
- S3 Glacier Flexible Retrieval has a minimum billable storage duration of 90 days.
- S3 Glacier Deep Archive has a minimum billable storage duration of 180 days.
- Under the current default configuration, objects smaller than 128 KB are not automatically transitioned through Lifecycle Rules unless an appropriate configuration changes that behavior.
- Storage class transitions and data retrieval may incur costs.

A Lifecycle Rule can also apply to existing objects that satisfy its conditions. Therefore, the scope should be checked carefully before configuring data expiration actions.

S3 Lifecycle reduces manual management tasks and supports long-term cost optimization when policies are designed around actual data usage patterns.


![Figure 2. Illustrative S3 Lifecycle storage class transition paths.](/images/blogs/blog1/s3-lifecycle-transitions.jpg)

*Figure 2. Illustrative S3 Lifecycle storage class transition paths.*


## 6. Data Protection with S3 Versioning

During system operations, accidental overwrites or deletions can affect business activities.

S3 Versioning allows multiple versions of the same object to be retained in a bucket.

For example, a revenue report named `monthly-report.pdf` is updated multiple times. When Versioning is enabled, Amazon S3 can retain previous versions instead of storing only the latest content.

If a user accidentally uploads an incorrect report, previous versions can still be retrieved and restored.


![Figure 3. PUT creates a new object version when S3 Versioning is enabled.](/images/blogs/blog1/s3-versioning-put.jpg)

*Figure 3. PUT creates a new object version when S3 Versioning is enabled.*


When a normal deletion is performed on a versioning-enabled bucket without specifying a Version ID, Amazon S3 creates a delete marker instead of permanently deleting previous data versions.


![Figure 4. DELETE creates a delete marker when S3 Versioning is enabled.](/images/blogs/blog1/s3-versioning-delete.jpg)

*Figure 4. DELETE creates a delete marker when S3 Versioning is enabled.*


However, Versioning does not provide absolute data protection. A user with appropriate permissions can still permanently delete a specific version.

Each retained version also incurs storage costs. Businesses should combine S3 Versioning with S3 Lifecycle to manage noncurrent versions.

For data with strict protection requirements, S3 Object Lock, AWS Backup, or other backup mechanisms suitable for the system's requirements may be considered.

## 7. Secure Data Sharing with S3 Presigned URLs

In an e-commerce system, customers may need to download invoices or documents related to their orders.

Making a bucket public to meet this requirement would introduce unnecessary security risks.

S3 presigned URLs provide a solution that allows an application to grant time-limited access to a specific object without making the bucket public.

For example, when a customer requests an invoice download:

1. The application authenticates the customer.
2. The backend checks access permissions for the requested invoice.
3. The backend generates a presigned URL valid for 15 minutes.
4. The customer uses the URL to download the invoice from Amazon S3.

Applications can also use presigned URLs to allow users to upload files directly to Amazon S3 without sending the entire file through the backend server.

This approach reduces backend workload and limits access on a per-object basis.

However, presigned URLs must be protected as access credentials. Anyone possessing a valid URL can use it within the granted scope.

Some principles to follow include:

- Grant permissions only for necessary operations.
- Set an appropriately short validity period.
- Authenticate users before generating a URL.
- Do not publicly share presigned URLs granting access to sensitive data.
- Use HTTPS to protect data in transit.

The actual validity period of a presigned URL also depends on the expiration and status of the credentials used to generate it.

## 8. Amazon S3 Security Best Practices

Data security is an important part of designing architectures on AWS.

For Amazon S3, the following mechanisms can be applied:

**S3 Block Public Access**

Helps prevent unintended public access. For internal and customer data, public access blocking settings should be maintained.

**AWS Identity and Access Management (IAM)**

Use IAM policies and S3 bucket policies to control access according to the principle of least privilege, ensuring that each user or application receives only the permissions required.

**Server-Side Encryption**

Amazon S3 provides default encryption for new objects using Amazon S3-managed keys (SSE-S3).

For systems requiring stricter management and control of encryption keys, SSE-KMS can be used with AWS Key Management Service (AWS KMS).

**AWS CloudTrail**

Supports recording API activities related to Amazon S3. For object access operations such as GetObject, PutObject, and DeleteObject, appropriate CloudTrail Data Events must be configured.

**S3 Object Lock**

Helps protect object versions from deletion or modification within the scope of retention policies, making it suitable for systems with compliance and data preservation requirements.

Combining these mechanisms helps reduce security risks and improve monitoring capabilities during operations.

## 9. Data Processing Automation with Amazon S3 and AWS Lambda

Amazon S3 can integrate with other AWS services through S3 Event Notifications.

Suppose a business needs to automatically create thumbnails whenever an administrator uploads a new product image.

The workflow can be designed as follows:

1. The administrator uploads an image to an input S3 bucket.
2. Amazon S3 generates an ObjectCreated event.
3. The event triggers AWS Lambda.
4. Lambda processes the image and creates a thumbnail.
5. The processed image is stored in an output S3 bucket.
6. The application uses the optimized image.

This is an example of event-driven architecture combined with serverless computing.


![Figure 5. Thumbnail generation architecture with Amazon S3 and AWS Lambda.](/images/blogs/blog1/s3-lambda-thumbnail.jpg)

*Figure 5. Thumbnail generation architecture with Amazon S3 and AWS Lambda.*


The business does not need to maintain an image processing server running continuously; the function can execute when an event occurs.

When designing this architecture, use a separate output bucket or appropriate event filters to prevent Lambda from repeatedly triggering itself.

Additionally, S3 Event Notifications may deliver duplicate events or events out of the expected order. Lambda should therefore be designed to handle repeated events safely.

A similar approach can be applied to:

- Automatically processing CSV files.
- Building data processing workflows.
- Converting file formats.
- Analyzing system logs.
- Preprocessing data for Machine Learning.

## 10. Hands-on Guide to S3 Lifecycle Configuration

To better understand Amazon S3 data lifecycle management, a simple hands-on exercise can be performed in the AWS Management Console.

**Step 1: Create an S3 bucket**

Open Amazon S3 and create a general purpose bucket with a unique name.

Keep Block Public Access settings enabled and check the default encryption configuration.

**Step 2: Upload sample data**

Upload some test files larger than 128 KB with object keys beginning with `logs/`.

Do not use personal or important data for this exercise.

**Step 3: Create a Lifecycle Rule**

Open the newly created bucket, select the Management tab, and choose Create lifecycle rule.

Name the rule `application-logs-lifecycle`.

**Step 4: Define the scope**

Set the prefix to `logs/` so that the rule applies only to the group of test log objects.

**Step 5: Configure Lifecycle Actions**

Configure the following actions:

- Transition to S3 Standard-IA 30 days after object creation.
- Transition to S3 Glacier Flexible Retrieval after 90 days.
- Apply expiration after 365 days if consistent with data retention policies.

**Step 6: Review the configuration**

Review the rule name, scope, storage classes, and time thresholds before saving.

If Versioning is enabled on the bucket, configure additional policies for noncurrent versions if older versions should be permanently deleted automatically.

**Step 7: Monitor the results**

Check the Lifecycle Rule on the Management tab.

When objects become eligible, Amazon S3 schedules the corresponding action. Transition or expiration may not occur immediately.

**Step 8: Clean up resources**

After completing the exercise, disable or delete the Lifecycle Rule if it is no longer needed.

Delete test data, including any versions and delete markers, then delete the bucket to avoid unnecessary costs.

## 11. Benefits and Limitations

Combining Amazon S3 features offers many benefits:

- Optimize costs based on data access characteristics.
- Reduce manual management work.
- Support data recovery when errors occur.
- Control access according to application requirements.
- Integrate flexibly with AWS services.
- Support scalable architecture design.

However, the effectiveness of a solution depends on its design and configuration.

An inappropriate Lifecycle Rule may lead to data loss or unexpected costs. Versioning may increase storage usage. Presigned URLs may introduce security risks if not controlled correctly.

Business requirements, data characteristics, operational costs, and security standards should therefore be evaluated before applying these mechanisms in production.

## 12. Conclusion

Through researching Amazon S3, I realized that the value of a cloud service lies not only in its primary functionality but also in the ability to combine its features to solve real-world problems.

Amazon S3 provides many tools for building secure, flexible, and efficient storage systems. Combining S3 Storage Classes, S3 Lifecycle, S3 Versioning, presigned URLs, and security mechanisms improves architectural quality and reduces operational complexity.

These methods also align with the AWS Well-Architected Framework, especially the Security, Reliability, Operational Excellence, and Cost Optimization pillars.

For those beginning to learn AWS, Amazon S3 is a service worth studying in depth because knowledge of storage, access control, cost management, and automation can be applied to many different cloud architectures.

I hope this article provides useful information for the community. I welcome feedback and shared experiences from everyone so that I can continue improving my AWS knowledge.

## Knowledge and Lessons Learned

Through researching and writing this article about Amazon S3, I had the opportunity to consolidate and broaden my knowledge of data storage services on AWS.

I gained a clearer understanding of selecting suitable storage classes, managing the data lifecycle, protecting data through S3 Versioning, and controlling access according to security principles.

Exploring the integration of Amazon S3 and AWS Lambda also broadened my understanding of event-driven architecture and serverless computing, and helped me recognize the importance of automation in system operations.

Writing the article helped me practice independent learning, technical documentation research, knowledge synthesis, and systematic presentation.

These foundational skills and concepts will help me continue researching and developing projects related to AWS and Cloud Computing.

## Evidence of Article Submission


![Figure 6. Amazon S3 – Cost Optimization, Security, and Automated Data Management on AWS, submitted on November 27, 2026.](/images/blogs/blog1/blog1-submission-proof.png)

*Figure 6. Amazon S3 – Cost Optimization, Security, and Automated Data Management on AWS, submitted on November 27, 2026.*


The author reports that the article is pending approval; the screenshot does not independently establish approval status. The Facebook post URL will be added later.

## References

1. Amazon S3 Overview
https://aws.amazon.com/s3/

2. Amazon S3 Storage Classes
https://aws.amazon.com/s3/storage-classes/

3. Transitioning objects using Amazon S3 Lifecycle
https://docs.aws.amazon.com/AmazonS3/latest/userguide/lifecycle-transition-general-considerations.html

4. Setting an S3 Lifecycle configuration
https://docs.aws.amazon.com/AmazonS3/latest/userguide/how-to-set-lifecycle-configuration-intro.html

5. S3 Versioning
https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html

6. Download and upload objects with presigned URLs
https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-presigned-url.html

7. Security best practices for Amazon S3
https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-best-practices.html

8. Amazon S3 Event Notifications
https://docs.aws.amazon.com/AmazonS3/latest/userguide/EventNotifications.html

9. AWS Well-Architected Framework
https://aws.amazon.com/architecture/well-architected/

10. Amazon S3 Pricing
https://aws.amazon.com/s3/pricing/

#AWS #AmazonS3 #CloudComputing #CloudSecurity #CostOptimization #AWSLambda #Serverless #AWSStudyGroup #FirstCloudAIJourney
