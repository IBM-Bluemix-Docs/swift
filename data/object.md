---

copyright:
  years: 2018, 2020
lastupdated: "2020-06-05"

keywords: object storage swift, static storage swift, file services swift, swift storage class, cos swift, swift data encryption, static swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Using Object Storage for static content
{: #object-storage}

Object Storage is a fundamental component of cloud computing, and provides powerful features to Apple Developers and their applications. Unlike storing information in a file hierarchy (such as Block or File storage), an object store consists only of the files and their metadata, stored in collections known as buckets. By definition, these objects are immutable, which makes them perfect for data such as images, videos, and other static documents. For data that changes often or is relational, you can use [Cloudant](/docs/swift/data?topic=swift-cloudant#cloudant), and [SQL](/docs/swift/data?topic=swift-sql_data#sql_data) database services.

{{site.data.keyword.cos_full_notm}} (COS) is a storage system that can be used to store unstructured data that is flexible, cost-effective, and scalable. The data is accessible through SDKs or by using the IBM user interface. You can use {{site.data.keyword.cos_full_notm}} to access your unstructured data through a self-service portal that is backed by RESTful APIs and SDKs. 

Depending on your needs, you can use {{site.data.keyword.cos_short}} for the following services:

* Content repository
* Backup and recovery
* Data archival
* File services
* Data transfer

When you create a bucket, you must select a resiliency level (cross-region or regional). Based on your selection, your data is dispersed, and stored across more than one geographic locations.

## API
{: #api-cos}

The {{site.data.keyword.cos_full}} API is a REST-based API for reading and writing objects. It supports a subset of the S3 API for easy migration of applications to {{site.data.keyword.cloud_notm}}. Any S3 SDK can be used to use {{site.data.keyword.cos_full}}. For more information, see the full [{{site.data.keyword.cos_short}} API Reference](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api).

## Securing object storage
{: #security-cos}

{{site.data.keyword.cos_full_notm}} is highly secure. Initially, only the bucket and object owners originally have access to {{site.data.keyword.cos_full_notm}} service they create. It also supports user authentication to access to data. Use access control mechanisms such as bucket policies to selectively grant permissions to users, and applications. You can securely transfer your data to {{site.data.keyword.cos_short}} through SSL endpoints by using the HTTPS protocol. If you need extra security, you can use the Server-Side Encryption (SSE) option or the Server-Side Encryption with Customer-Provided Keys (SSE-C) option to encrypt data stored-at-rest. {{site.data.keyword.cos_full_notm}} provides the encryption technology for both SSE and SSE-C. Alternatively, you can use your own encryption libraries to encrypt data before storing it in Cloud Object Storage.

You can use the following access control mechanisms to secure your data in {{site.data.keyword.cos_full_notm}}.

### Identity and Access Management (IAM) policies
{: #iam-cos}

IAM enables organizations with multiple employees to create and manage multiple users under a single account. With IAM policies, companies can grant IAM users control to their {{site.data.keyword.cos_short}} buckets.

### Access Control Lists (ACLs)
{: #acls-cos}

With ACLs, you can grant specific permissions (for example, READ, WRITE), to specific users for an individual bucket.

Data at rest is encrypted with automatic provider side Advanced Encryption Standard (AES) 256-bit encryption and Secure Hash Algorithm (SHA)-256 hash. Data in motion is secured by using built-in carrier grade Transport Layer Security (TLS), Secure Sockets Layer (SSL), or SNMPv3 with AES encryption.

### Encryption
{: #encryption-cos}

Data at rest is encrypted with automatic provider side Advanced Encryption Standard (AES) 256-bit encryption and Secure Hash Algorithm (SHA)-256 hash. Data in motion is secured by using built-in carrier grade Transport Layer Security/Secure Sockets Layer (TLS/ SSL) or SNMPv3 with AES encryption.

Server-side encryption is always ON for customer data. Compared to the hashing that is required in authentication, and the erasure coding, encryption isn't a significant part of the processing cost of Cloud Object Storage.

You can provide your own key for encryption since SSE-C is supported in {{site.data.keyword.cos_full_notm}}. However, the responsibility is on you to manage and provide the key for storing, and retrieve the data.

## Resiliency
{: #resiliency-cos}

When you create a bucket, you must select a resiliency level (cross-region or regional). Based on your selection, your data is dispersed, and stored across more than one geographic locations.

Regional resiliency is for low latency and your data is distributed in three centers within a single region. Use cross region resiliency for critical availability, as your data is stored in 3 or more different regions. Cross region offers geographic resiliency and is available across more than one endpoint. Consider your application needs to decide between the two.

### Geographies
{: #geographies-cos}

You can use {{site.data.keyword.cos_full_notm}} from anywhere in the world. You can choose where you want to store your data at the time of bucket creation. Information that is stored in IBM COS is encrypted, and dispersed across more than one geographic location by using distributed storage technologies that are provided by IBM Object Storage System. 

Consider the following factors to select the geographic location of your object store when you're deciding between regional and cross-region options.

Geographic location considerations:
* A location that is remote from your operations for redundancy.
* A location to address legal, and regulatory requirements.
* Reduce data access latencies.
* Consider pricing models.

## Use Cases and Storage Classes
{: #usecases-cos}

Depending on your use case, you can reduce costs by selecting a service plan that meets your needs. Archival operations that involve minimal access to the object store, do not need the speed and durability of a frequently accessed object, and this distinction is reflected in the Storage Class support and pricing plan for your applications. Storage classes are defined at the bucket level, so you can use a combination of plans to suit your needs. Create a bucket that is set to the storage class that you want to use.

More information about the pricing is available from the [{{site.data.keyword.cos_short}} Storage pricing](/docs/cloud-object-storage/help?topic=cloud-object-storage-billing#billing-pricing) documentation.

### Sample Storage Classes
{: #samples-cos}

- Standard
  
  This service is for unstructured data that requires frequent access, such as DevOps, collaboration and action content repositories.

- Vault
  
  This service is for workloads with infrequently accessed data, such as backup, archive, and compliance workloads.

- Cold Vault
  
  This deployment option is ideal for minimum access requirements, historical records compliance, and long-term backup.

- Flex

  Deploy for varying data access requirements and protect your budget from unexpected cost fluctuations. Storage classes are defined at the bucket level. Create a bucket that is set to the storage class that you want to use.


