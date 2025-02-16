---

copyright:
  years: 2020
lastupdated: "2020-12-08"

keywords: data encryption, data storage, bring your own keys, BYOK, key management, key encryption, personal data, data deletion, data security

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:term: .term}

# Securing your data in VPC
{: #mng-data}

To ensure that you can securely manage your data when you use {{site.data.keyword.vpc_full}}, it's important to know exactly what data is stored and encrypted and how you can delete any stored personal data. Data encryption using your own root keys is available by using a supported key management service (KMS). 
{: shortdesc}

VPN for VPC does not store any customer data other than what is required to configure VPN gateways, connections, and policies. Data transmitted through a VPN gateway is not encrypted by IBM. Data about your specific VPN and policy configurations are encrypted in transit and at rest. VPN configuration data is deleted upon your request through API or User Interface.  

## How your data is stored and encrypted in VPC
{: #data-storage}

All block storage volumes are encrypted by default with IBM-managed encryption. {{site.data.keyword.IBM}}-managed keys are generated and securely stored in a block storage vault that is backed by Consul and maintained by {{site.data.keyword.cloud}} operations. 

For more security and control, you can protect your data by using your own root keys (also called a customer root key or CRK). This feature is commonly called Bring Your Own Key, or BYOK. Root keys encrypt the keys that safeguard your data. You can import your root keys to {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}, or have either key management service create one for you. 

The KMS stores your key and makes it available during volume and custom image encryption. Key Protect provides FIPS 140-2 Level 3 compliance. Hyper Protect Crypto Services offers the highest level of security with FIPS 140-2 Level 4 compliance. Your key material is protected in transit (when it's transported) and at rest (when it is stored).

Customer-managed encryption is available for custom images, boot volumes, and data volumes. When an instance is provisioned from an encrypted custom image, its boot volume is encrypted by using the image’s root key. You can also choose a different root key. Data volumes are encrypted by using root keys when you provision a virtual server instance or when you create a stand-alone volume.

Images and volumes are often referred to as being encrypted with a root key when, in fact, [envelope encryption](#x9860393){: term} is used. Internally, each image or volume is encrypted with a [data encryption key (DEK)](#x4791827){: term}, which is an open source QEMU technology that is used by the {{site.data.keyword.vpc_short}} Generation 2 infrastructure. A LUKS passphrase, also called a _key encryption key_, encrypts the DEK. The LUKS passphrase is then encrypted with a root key, creating what is called a wrapped DEK (WDEK). For more information about {{site.data.keyword.vpc_short}} key encryption technology, see [IBM Cloud VPC Generation 2 encryption technology](/docs/vpc?topic=vpc-vpc-encryption-about#byok-technologies).

All interaction with VPN for VPC from clients is encrypted. For example, when a client uses an API or interacts with the service via User Interface to configure VPN gateways and VPN connections, all such interactions are encrypted end-to-end. Likewise, data elements related to the clients' configuration are encrypted in transit and at rest. No personal or sensitive data is stored, processed, or transmitted. Data at rest is stored in an encrypted database.

After the VPN for VPC is provisioned and the network connections are created, the encryption of data that clients choose to transmit across the network is the client's responsibility.

For example, if you provision two volumes by using the same root key, unique passphrases are generated for each volume, which are then encrypted with the root key. Envelope encryption provides more protection for your data, and ensures that the root key can be rotated without having to reencrypt the data. For more information about envelope encryption, see the following section.

## Protecting your sensitive data in VPC
{: #data-encryption}

{{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} provide a higher level of protection called envelope encryption.

[Envelope encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-envelope-ecryption-byok) encrypts one encryption key with another encryption key. A DEK encrypts your actual data. The DEK is never stored. Rather, it's encrypted by a key encryption key. The LUKS passphrase is then encrypted by a root key, which creates a WDEK. To decrypt data, the WDEK is unwrapped so you can access the data that's stored on the volume. This process is possible only by accessing the root key that is stored in your KMS instance. Root keys in HPCS service instances are also protected by a [hardware security module (HSM)](#x6704988){: term} master key.

You control access to your root keys stored in KMS instances within {{site.data.keyword.cloud}} by using {{site.data.keyword.iamlong}} (IAM). You grant access to a service to use your keys. You can also revoke access at any time, for example, if you suspect your keys might be compromised, or [delete your root keys](#delete-root-keys).

VPN for VPC: Customer-provided preshared keys are encrypted before being stored in database. All other data VPN gateway and VPN policy configuration is encrypted at rest at the database level.

### About customer-managed keys
{: #about-encryption}

For block storage volumes and encrypted images, you can rotate the root keys for more security. When you rotate a root key by schedule or on demand, the original key material is replaced. The old key remains active to decrypt existing resources but can't be used to encrypt new ones. For more information, see [Key rotation for VPC resources](/docs/vpc?topic=vpc-vpc-key-rotation).

There are also regional and cross-regional considerations to take into account when using customer-managed encryption. For more information, see [Regional and cross regional considerations](/docs/vpc?topic=vpc-vpc-encryption-about#byok-cross-region-keys).

With Key Protect or HPCS you can create, import, and manage your root keys. You can assign access policies to the keys, assign users or service IDs to the keys, or give the key access only to a specific service. The first 20 keys are free.

### About customer-managed encrypted volumes and images
{: #about-encryption}

With customer-managed encryption, you provision root keys to protect your encrypted resources in the cloud. Root keys serve as key-wrapping keys that encrypt the encryption keys that protect your data. You decide whether to import your existing root keys, or have a supported KMS create one for you.

Block storage volumes are assigned a unique data encryption key that is generated by the instance's host hypervisor. The data encryption key for each volume is encrypted with a unique KMS-generated LUKS passphrase, which is then encrypted by the root key and stored in the KMS.

Custom images are encrypted by your own LUKS passphrase you create by using QEMU. After the image is encrypted, you wrap the passphrase with your CRK stored in the KMS. For more information, see [Encrypted custom images](/docs/vpc?topic=vpc-vpc-encryption-about#byok-about-encrypted-images).

### Enabling customer-managed keys for VPC
{: #using-byok}

See the following procedures for creating block storage volumes with customer-managed encryption and virtual server instances with customer-managed encryption volumes:

* [Creating block storage volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption)
* [Creating virtual server instances with customer-managed encryption volumes](/docs/vpc?topic=vpc-creating-instances-byok)

### Working with customer-managed keys for VPC
{: #working-with-keys}

For information about managing data encryption, see [Managing data encryption](/docs/vpc?topic=vpc-vpc-encryption-managing), and the section on temporarily revoking access by [removing service authorization to a root key](/docs/vpc?topic=vpc-vpc-encryption-managing#instance-byok-inaccessible-data).

Encryption of the link between a customer's workload that's outside of {{site.data.keyword.cloud_notm}}, and a workload that's inside {{site.data.keyword.cloud_notm}}, is the customer’s responsibility. See _Encryption_ in [Security and regulation compliance](/docs/vpc?topic=vpc-responsibilities-vpc#security-compliance).
{: note}

## Deleting data in VPC
{: #data-delete}

### Deleting root keys
{: #delete-root-keys}

For information about deleting root keys, see [Deleting root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-delete-root-keys).

### Deleting a block storage volume
{: #data-delete-volume}

For information about deleting block storage volumes, see this FAQ: [What happens to my data when I delete a block storage data volume?](/docs/vpc?topic=vpc-block-storage-vpc-faq#faq-block-storage-16).

### Deleting VPC instances
{: #service-delete}

For information about deleting a VPC and its associated resources, see [Deleting a VPC](/docs/vpc?topic=vpc-deleting).

The VPC data retention policy describes how long your data is stored after you delete the service. The data retention policy is included in the {{site.data.keyword.vpc_full}} service description, which you can find in the [{{site.data.keyword.cloud_notm}} Terms and Notices](/docs/overview?topic=overview-terms).

The VPN and VPN policy configurations are deleted on request through the API or user interface.

### Restoring deleted data for VPC
{: #data-restore}

You can restore deleted root keys that you imported to the KMS within 30 days of deletion. For more information, see
[Restoring deleted root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-restore-root-key).

### Deleting all VPC data
{: #delete-all-data}
To delete all persisted data that {{site.data.keyword.vpc_short}} stores, choose one of the following options.

Removing your personal and sensitive information requires all of your {{site.data.keyword.vpc_short}} resources to be deleted as well. Make sure that you back up your data before you proceed.
{: note}

VPN for VPC does not support the restoration of deleted data.

* Open an {{site.data.keyword.cloud_notm}} support case. Contact IBM Support to remove your personal and sensitive information from {{site.data.keyword.vpc_short}}. For more information, see [Getting support](https://cloud.ibm.com/docs/get-support?topic=get-support-getting-customer-support).
* End your {{site.data.keyword.cloud_notm}} subscription. After you end your {{site.data.keyword.cloud_notm}} subscription, {{site.data.keyword.vpc_short}} deletes all service resources that you created, which includes all persisted data that is associated with those resources.
