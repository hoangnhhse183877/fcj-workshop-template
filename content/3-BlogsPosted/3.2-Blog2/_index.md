---
title: "Blog 2"
date: 2026-11-27
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# AWS EC2-Other: Understanding Hidden Costs and How to Optimize Costs on AWS

| Information | Details |
|---|---|
| Author | Khánh Vương (team member) |
| Submission date | November 27, 2026 |
| Publication date | November 27, 2026 |
| Status | Approved and published (as reported) |
| Platform | [AWS Study Group VN](https://www.facebook.com/groups/awsstudygroupfcj) |
| Post URL | [View the original post](https://www.facebook.com/groups/awsstudygroupfcj/posts/2351052395659719/) |

This article was written and published by Khánh Vương, a member of my team. It is included in this report as a contribution from our team. The article below retains the original author's perspective.

## 1. Introduction

When using Amazon EC2, many people focus only on the cost of running EC2 instances. However, even after a server has been stopped, related resources and services can continue to incur costs.

While checking AWS Billing and Cost Management, I noticed that the November cost was $4.13 and was forecast to reach $4.77 by the end of the month (Figure 1).

When analyzing costs by service, the entire October cost of $4.80 came from EC2-Other (Figure 2). This is a category of costs that can easily be overlooked, particularly by people who are new to AWS or frequently complete hands-on labs in cloud environments.


![Figure 1. AWS Billing and Cost Management overview, including Cost Summary and Cost Monitor.](/images/blogs/blog2/aws-billing-cost-summary.png)

*Figure 1. AWS Billing and Cost Management overview, including Cost Summary and Cost Monitor.*



![Figure 2. Cost breakdown by service: EC2-Other accounts for the entire USD 4.80 cost in October 2026.](/images/blogs/blog2/ec2-other-cost-breakdown.png)

*Figure 2. Cost breakdown by service: EC2-Other accounts for the entire USD 4.80 cost in October 2026.*


## 2. What is EC2-Other?

EC2-Other is a category of costs associated with Amazon EC2 that does not include the direct cost of running EC2 instances.

Some common sources of EC2-Other costs include:

- EBS Volume: Disk storage costs, even when an EC2 instance has been stopped.
- EBS Snapshot: Costs of storing disk backups.
- NAT Gateway: Costs of maintaining the service and processing network traffic.
- Data Transfer: Data transfer costs in certain cases.
- EBS IOPS and Throughput: Costs of additional provisioned storage performance.

Public IPv4 addresses and Elastic IPs should also be reviewed when optimizing AWS costs, although they may be categorized separately under Amazon VPC.

## 3. How to Check EC2-Other Costs

AWS Cost Explorer allows users to analyze costs in detail through the following steps:

- Open AWS Billing and Cost Management → Cost Explorer.
- Select the time period to analyze.
- Under Filters → Service, select EC2-Other.
- Under Group by → Dimension, select Usage type.
- Review each usage type to identify the source of the costs.

In my case, although EC2-Other was identified as the main cost category in October, the specific cause of the $4.80 charge has not yet been verified.

## 4. AWS Free Tier and AWS Credits

An important point is that AWS Free Tier does not mean every resource incurs no cost.

Resources can still generate usage costs and consume AWS Credits.

In my case (Figure 3):

- Initial AWS Credits: $100.00.
- Estimated remaining AWS Credits: $89.48.
- Total credits used and expected to be used: $10.52.
- Percentage of credits used and expected to be used: 10.52%.
- Amazon EC2 accounts for most of the credits consumed.

This demonstrates that monitoring and managing costs is necessary even when no out-of-pocket payment has been required.


![Figure 3. AWS Free Tier credit information and credit usage history.](/images/blogs/blog2/aws-free-tier-credits.png)

*Figure 3. AWS Free Tier credit information and credit usage history.*


## 5. AWS Cost Optimization Methods

To limit unwanted costs, the following approaches can be applied:

- Review and delete unused EBS volumes after confirming that their data is no longer needed.
- Delete EBS snapshots that are no longer required.
- Release unused Elastic IPs.
- Review and delete NAT gateways that no longer serve a workload.
- Review resources across all AWS Regions.
- Configure AWS Budgets to receive alerts when costs exceed the desired threshold.
- Regularly monitor AWS Cost Explorer and the AWS Credits balance.

For people learning AWS or working on personal projects in particular, reviewing and cleaning up resources after each lab should become a habit.

## 6. Conclusion

Stopping an EC2 instance does not mean that all AWS costs stop accruing.

Understanding EC2-Other helps users proactively control costs, reduce resource waste, and use AWS Credits more effectively.

This is also a practical application of Cost Optimization, one of the six pillars of the AWS Well-Architected Framework.

Effective cloud infrastructure management is not only about deploying and operating systems successfully, but also about controlling and optimizing costs throughout the resource lifecycle.

## 7. References

1. [AWS Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/)
2. [AWS Cost Explorer – Tips and Tricks](https://aws.amazon.com/blogs/aws-cloud-financial-management/tips-and-tricks-for-exploring-your-data-in-aws-cost-explorer-part-2/)
3. [Amazon EBS Pricing](https://aws.amazon.com/ebs/pricing/)
4. [Amazon VPC Pricing](https://aws.amazon.com/vpc/pricing/)
5. [AWS Free Tier](https://aws.amazon.com/free/)
6. [AWS Budgets](https://aws.amazon.com/aws-cost-management/aws-budgets/)
7. [AWS Well-Architected Framework – Cost Optimization Pillar](https://docs.aws.amazon.com/wellarchitected/latest/cost-optimization-pillar/welcome.html)

#AWS #AmazonEC2 #EC2Other #AWSFreeTier #CostOptimization #FinOps #CloudComputing #DevOps #AWSCommunity #AWSStudyGroup #FirstCloudAIJourney

## Published Article Evidence

![Figure 4. AWS EC2-Other article in the AWS Study Group VN community.](/images/blogs/blog2/blog2-publication-proof.png)

*Figure 4. AWS EC2-Other article in the AWS Study Group VN community.*

The article was approved according to the supplied information. [View the original post](https://www.facebook.com/groups/awsstudygroupfcj/posts/2351052395659719/).
