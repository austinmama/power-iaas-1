﻿---

copyright:
  years: 2019, 2020

lastupdated: "2021-03-25"

keywords: migration strategies, cos, mass data migration, mdm, pwoervc, backup and restore, replication, aspera, mksysb, aws cli, pip, yum

subcollection: power-iaas

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}

# Migration strategies for IBM Power Systems Virtual Servers
{: #migration-strategies-power}

Learn how to migrate your data and workloads to a IBM&reg; Power Systems&trade; Virtual Server.
{: shortdesc}

## IBM Cloud Object Storage (COS)
{: #migration-icos}

COS can be used as an intermediary location to store files from your on-premises environment. You can retrieve and send your files to the {{site.data.keyword.powerSys_notm}} environment from this location. You must create COS buckets to transfer data over the public internet and or privately secured links. For more information, see [IBM Cloud Object Storage: FAQ](https://www.ibm.com/cloud/object-storage/faq){: external}.

To copy data from COS to your AIX virtual machine (VM), you must install the [Amazon Web Services (AWS) CLI](/docs/cloud-object-storage?topic=cloud-object-storage-aws-cli) by using either the **Yellowdog Updater, Modified (yum)** or **Pip installs Python pip (pip)** package managers. If you use the yum package manager, you can install the AWS CLI with the `yum install aws-cli` command. For the pip package manager, you can use `pip install awscli` to install the AWS CLI. After the installation, you can use the universal S3 commands that are supported by AWS to copy objects.

You can also find a script that ships (as a sample command) with AIX 7.2 TL3, or later, that simplifies the installation of yum, pip, and the AWS CLI. You can find the script at: `/usr/samples/nim/cloud_setup`.

- [Configuring YUM and creating local repositories on IBM AIX](https://developer.ibm.com/technologies/systems/articles/configure-yum-on-aix/){: external}
- [Python for AIX](http://www.aixtools.net/index.php/python){: external}

For an IBM i VM, you must use the [Cloud Storage Solution for IBM i product (5733-ICC)](https://www.ibm.com/support/pages/ibm-cloud-storage-solutions-i){: external} to communicate with COS and transfer data.

## Mass Data Migration (MDM)
{: #migration-mdm}

MDM provides a simple and secure way to physically transfer data (terabytes to petabytes) to the IBM Cloud Object Storage to be used by {{site.data.keyword.powerSys_notm}}. As part of the MDM process, IBM sends the client an MDM-approved device, who uploads their on-premises data to the device and sends it back. IBM then transfers and stores the content in COS for later retrieval from within the {{site.data.keyword.powerSys_notm}} environment. To learn more, see [Mass Data Migration: FAQ](https://www.ibm.com/cloud/mass-data-migration/faq){: external}.

The data transfer rate for MDM on an IBM i system is roughly 110-120 MB/sec. It takes between 2.5 and 3 hours to successfully transfer 1 TB of data.
{: note}

For steps to configure MDM on AIX and IBM i VMs, see the following tutorials:
- [Configuring IBM Cloud Mass Data Migration (MDM) on AIX VM](/docs/power-iaas?topic=power-iaas-configuring-mdm)
- [Configuring Mass Data Migration (MDM) on IBM i VM](/docs/power-iaas?topic=power-iaas-configuring-mass-data-migration-mdm-on-ibm-i-vm)

## PowerVC images and COS
{: #migration-powervc-icos}

If you have an environment with access to PowerVC, you can capture OVA images to easily migrate your data. The {{site.data.keyword.powerSys_notm}} offering allows you to provision a new virtual server based on an OVA image. To accomplish this, regardless of the operating system (OS), you must complete the following steps:

1. Create an OVA image on your local system.
2. Copy the OVA image to your **Cloud Object Storage** account.
3. [Deploy the OVA image and provision a new Power Systems Virtual Server](/docs/power-iaas?topic=power-iaas-deploy-custom-image).

## Aspera Technologies
{: #aspera-technologies}

IBM Aspera&reg; on Cloud is a hosted service that quickly and reliably moves and shares your data sets across a hybrid cloud environment. IBM Aspera can help transfer data to the IBM Cloud for later retrieval from the {{site.data.keyword.powerSys_notm}} environment. For more information, see [IBM Aspera on Cloud](https://www.ibm.com/products/aspera){: external}.

## Replication
{: #replication}

You can use various replication mechanisms to migrate and sync environments between your on-premises environment and the {{site.data.keyword.powerSys_notm}}.

Storage area network (SAN) and hardware-based replication is not currently a migration option.
{: note}

### Host and OS-based logical replication
{: #replication-host}

IBM recommends that you use PowerHA SystemMirror (Enterprise Edition) with Geographic Mirroring (Geomirroring) and Geographic Logical Volume Manager (GLVM). The following host and os-based logical replication options exist depending on your OS:

- **AIX** - GLVM
- **IBM i** - Geomirroring

### Application-specific replication
{: #replication-app}

Applications might have replication mechanisms that can sync multiple environments. These options are commonly used for application-specific replication:

- *Db2 HADR*
- *Oracle Data Guard*
- *Oracle Goldengate*
- *iCluster*
- *MIMIX from Syncsort*

## Backup and restore
{: #backup-restore}

You can back up your on-premises environment and restore it to {{site.data.keyword.powerSys_notm}}. In most cases, COS, and NFS servers serve as an intermediary to back up and restore data. The [AIX migration strategies](#migration-aix) and [IBM i migration strategies](#migration-ibmi) sections provide information on OS-specific migration strategies.

## Third-party vendors and tools
{: #third-party-vendors}

Customers can use third-party tools to perform data migration. The following third-party tools are commonly used for data migration:

- *iCluster*
- *MIMIX from Syncsort*
- *Double-Take® MoveTM for AIX*

## IBM teams and managed services
{: #teams-managed-services}

You can engage IBM teams and services to assist you throughout the migration lifecycle.

- [IBM Systems Lab Services](https://www.ibm.com/it-infrastructure/services/lab-services){: external}
- [IBM Services for Cloud Migration](https://www.ibm.com/services/cloud/migration){: external}

## AIX migration strategies
{: #migration-aix}

Learn about migration strategies that are specific to AIX systems.

### Migration by using MKSYSB
{: #migration-mksysb}

You can migrate your data by using the `mksysb` command:

1. Back up the on-premises OS with the `alt_disk_mksysb` command and transfer it to COS.
2. Create a {{site.data.keyword.powerSys_notm}} and import the image.
3. Restore the on-premises image.

For more information, see [Restoring an AIX mksysb image onto a Power Systems Virtual Server instance](/docs/power-iaas?topic=power-iaas-restoring-aix-mksysb-image).

### Logical replication with GLVM and PowerHA SystemMirror for AIX Enterprise Edition
{: #logical-replication}

GLVM is an OS-based IP replication strategy. It is based on the AIX LVM and enables data and logical volume mirroring across geographically distant locations. GLVM supports both synchronous and asynchronous modes. You can integrate PowerHA SystemMirror (Enterprise Edition) for network monitoring and automated failover support.

- [Geographic Logical Volume Manager (GLVM)](https://www.ibm.com/support/knowledgecenter/en/SSPHQG_7.2/glvm/ha_glvm_glvm.html){: external}
- [Configuring geographically mirrored volume groups](https://www.ibm.com/support/knowledgecenter/en/SSPHQG_7.2/glvm/ha_glvm_config_glvm.html){: external}
- [Exploiting IBM PowerHA SystemMirror V6.1 for AIX Enterprise Edition](http://www.redbooks.ibm.com/redbooks/pdfs/sg247841.pdf){: external}
- [PowerHA SystemMirror for AIX 7.2](https://www.ibm.com/support/knowledgecenter/en/SSPHQG_7.2/navigation/welcome.html){: external}
- [IBM PowerHA SystemMirror for AIX Cookbook](http://www.redbooks.ibm.com/abstracts/sg247739.html){: external}
- [PowerHA SystemMirror for AIX on Fix Central](https://www-945.ibm.com/support/fixcentral/options?selectionBean.selectedTab=find&selection=Cluster+software%3bibm%2fOther+software%2fPowerHAClusterManager){: external}

### Alternative AIX migration strategies
{: #migration-alternative-aix}

Some alternative AIX migration strategies include:

- `rsync` for non-database files
- The `savevg` and `restvg` AIX commands
- Log shipping for databases

For a complete tutorial on migrating your AIX workloads to Power Systems Virtual Servers, see [Migrating AIX to IBM Power Systems Virtual Servers](https://cloud.ibm.com/media/docs/downloads/power-iaas-tutorials/PowerVS_AIX_Migration_Tutorial_v1.pdf){: external}.

## IBM i migration strategies
{: #migration-ibmi}

Learn about migration strategies that are specific to IBM i systems.

### Backup Recovery and Media Services (BRMS) and Cloud Storage (ICC)
{: #ibmi-brms-icc}

Image catalogs are created out of objects that are backed up by using optical devices. These catalogs must be restored on the Power Systems Virtual Server instance by using some of the migration strategies, such as MDM, COS, Aspera, and NFS server. BRMS is an IBM i product that can be used to automate activities that help define and process your backup, recovery, and media management operations. The ICC product can be integrated with BRMS to move and retrieve objects from remote locations, including COS.

The following steps detail how to migrate your OS and data from an on-premises system to the {{site.data.keyword.powerSys_notm}} environment. Keep in mind that most of these steps can be automated by using BRMS and ICC.

1. Create your IBM i VM in the {{site.data.keyword.powerSys_notm}}.
2. Create a virtual tape and *IMGCLG* on the deployed VM.
3. Create a virtual tape and *IMGCLG* on the on-premises system and perform an operating system save by entering, `GO LICPGM Option 40`.
4. Transfer or FTP the images to the {{site.data.keyword.powerSys_notm}}.

    You can accomplish the transfer by using some of the listed migration strategies (MDM, COS, Aspera, etc.).
    {: important}

5. Restore or slip the installation of the OS to get the base OS to the same level as it was on your on-premises system.
6. Migrate your remaining data.

For more information, see [Data backup and recovery by using BRMS and IBM Cloud Storage Solutions for i](https://www.ibm.com/support/knowledgecenter/en/ssw_ibm_i_74/rzai8/rzai8backupandrecoveryusingBRMSandICC.htm){: external} and [BRMS with Cloud Storage Solutions for i considerations and requirements](https://www.ibm.com/support/knowledgecenter/en/ssw_ibm_i_74/rzai8/rzai8brmscloudrequireandconsider.htm){: external}.

For a complete tutorial on migrating your IBM i workloads to Power Systems Virtual Servers, see [Migrating IBM i to IBM Power Systems Virtual Servers](https://cloud.ibm.com/media/docs/downloads/power-iaas-tutorials/PowerVS_IBMi_Migration_Tutorial_v1.pdf){: external}.

### Logical Replication with Geographic Mirroring and PowerHA SystemMirror for AIX Enterprise Edition
{: #logical-rep-glvm}

GeoMirroring enables IBM i disk mirroring technology on multiple system environments and supports host-based and logical replication across geographically distant sites. Geomirroring supports synchronous and asynchronous modes. You can integrate PowerHA SystemMirror (Enterprise Edition) for network monitoring and automated failover support.

- [Geographic mirroring](https://www.ibm.com/support/knowledgecenter/en/ssw_ibm_i_73/rzaue/rzalygeographicmirror.htm){: external}
- [IBM PowerHA SystemMirror for i: Using Geographic Mirroring](https://www.redbooks.ibm.com/redbooks/pdfs/sg248401.pdf){: external}
