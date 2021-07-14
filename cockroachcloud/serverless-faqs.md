---
title: CockroachCloud Serverless (beta) FAQs
summary: Get answers to frequently asked questions about CockroachCloud Serverless (beta)
toc: true
---

This page answers the frequently asked questions about CockroachCloud Serverless (beta) and CockroachCloud Dedicated.

<div class="filters clearfix">
    <a href="serverless-faqs.html"><button class="filter-button page-level current">CockroachCloud Serverless</button></a>
    <a href="frequently-asked-questions.html"><button class="filter-button page-level">CockroachCloud Dedicated</button></a>
</div>

## General

### What is CockroachCloud Serverless (beta)?

CockroachCloud Serverless (beta) delivers free CockroachDB clusters for you and your organization. It is a managed instance of CockroachDB that removes the friction of initial cluster sizing and auto-scales based on your application traffic.

### How do I start using CockroachCloud Serverless (beta)?

To get started with CockroachCloud Serverless (beta), <a href="https://cockroachlabs.cloud/signup?referralId=docs_serverless_faq" rel="noopener" target="_blank">sign up for a CockroachCloud account</a>, click **Create Cluster**, then click **Create your free cluster**. Your cluster will be ready in 20-30 seconds. For more information, see [**Quickstart**](quickstart.html).

### What are the usage limits of Cockroach Cloud Serverless (beta)?

Free clusters have a limit of 500M Request Units and 5GB of storage, but the performance is heavily throttled. Paid clusters have access to the same resources with no limitations in addition to the amount you pay for.

### Do I have to pay for CockroachCloud Serverless (beta)?

No, you can create a Serverless cluster that is free forever. If you choose to create a paid cluster, you will only be charged for the resources you use up to your spend limit.

### What can I use CockroachCloud Serverless (beta) for?

Free CockroachCloud Serverless (beta) clusters can be used for proofs-of-concept, toy programs, or to use while completing [Cockroach University](https://www.cockroachlabs.com/cockroach-university/).

For examples of applications that use free clusters, check out the following [Hack the North](https://hackthenorth.com/) projects:

- [flock](https://devpost.com/software/flock-figure-out-what-film-to-watch-with-friends)
- [mntr.tech](https://devpost.com/software/mntr-tech)
- [curbshop.online](https://devpost.com/software/curbshop-online)

Paid Serverless clusters have higher performance, additional features, and the ability to scale according to your needs. They can be used for larger applications and projects.

### What are the limitations of CockroachCloud Serverless (beta)?

CockroachCloud Serverless is currently in beta and there are capabilities we are still working on enabling, such as the ability to enable backups, to import data, and no-downtime upgrades to a paid tier. If you want to use any of these capabilities, try a [30-day trial of CockroachCloud](quickstart-trial-cluster.html).

### How do I connect to my cluster?

To connect to a cluster, download the CA certificate, and then generate a connection string or parameters. You can use this information to connect to your cluster through the CockroachDB SQL client or a Postgres-compatible driver or ORM. For more details, see [Connect to Your CockroachCloud Cluster](connect-to-your-cluster.html).

## Beta release

### Why is CockroachCloud Serverless in beta?

CockroachCloud Serverless is in beta while we work on adding core features like [import](../{{site.versions["stable"]}}/import.html) and [backups](backups-page.html).

### Where can I submit feedback or bugs on the beta?

You can submit feedback or log any bugs you find through [this survey](https://forms.gle/jWNgmCFtF4y15ePw5).

## Security

### Is my cluster secure?

Yes, we use separate certificate authorities for each cluster, and all connections to the cluster over the internet use TLS 1.2.

### Is encryption-at-rest enabled on CockroachCloud Serverless (beta)?

Yes. All data on CockroachCloud is encrypted-at-rest using the tools provided by the cloud provider that your cluster is running in.

- Data stored in clusters running in GCP are encrypted-at-rest using [persistent disk encryption](https://cloud.google.com/compute/docs/disks#pd_encryption).
- Data stored in clusters running in AWS are encrypted-at-rest using [EBS encryption-at-rest](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html).

Because we are relying on the cloud provider's encryption implementation (as noted above), we do not enable CockroachDB's [internal implementation of encryption-at-rest](../{{site.versions["stable"]}}/encryption.html#encryption-at-rest-enterprise). This means that encryption will appear to be disabled in the [DB Console](../{{site.versions["stable"]}}/ui-overview.html), since it is unaware of cloud provider encryption.

### Is my cluster isolated? Does it share resources with any other clusters?

CockroachCloud Serverless (beta) is a multi-tenant offering and resources are shared between clusters.

## Cluster maintenance

### How do I add nodes?

You can add nodes to paid CockroachCloud Serverless (beta) clusters, but not to free clusters. If you have a paid cluster, you can add nodes by accessing the **Clusters** page on the [CockroachCloud Console](https://cockroachlabs.cloud/) and clicking the **...** button for the cluster you want to add or delete nodes for. See [Cluster Mangement](cluster-management.html?filters=dedicated#add-or-remove-nodes-from-a-cluster) for more details.

{% include cockroachcloud/nodes-limitation.md %}

### Can I upgrade my free CockroachCloud Serverless (beta) cluster to a paid CockroachCloud Serverless (beta) cluster?

At this time, a free CockroachCloud Serverless cluster cannot be upgraded. In the future, you will have the ability to move from a free CockroachCloud Serverless to a pay-as-you-go Serverless cluster.

### Can I upgrade my cluster from CockroachCloud Serverless (beta) to CockroachCloud Dedicated?

At this time, a CockroachCloud Serverless cluster cannot be upgraded. In the future, you will have the ability to move from CockroachCloud Serverless to CockroachCloud Dedicated.

## Product features

### Are partitioning or change data capture available to me?

No, change data capture and partitioning are not available on CockroachCloud Serverless (beta) clusters, but will be in the future.

### Do you have a UI? How can I see details?

Yes, you can view and your clusters in the [CockroachCloud Console](https://cockroachlabs.cloud/). However, [DB Console](../{{site.versions["stable"]}}/ui-overview.html) pages (e.g., **Statements** or **Database** pages) are not currently available for CockroachCloud Serverless (beta) clusters.

### Can I backup my CockroachCloud Serverless (beta) cluster? Does Cockroach Labs take backups of my cluster?

Cockroach Labs takes full cluster backups of all CockroachCloud Serverless (beta) clusters for our own purposes. Currently, these backups are not available to you and you cannot backup and restore a CockroachCloud Serverless (beta) cluster yourself. We expect to support user-initiated backup and restore of free clusters in the future.

In the meantime, you can run a [`SELECT`](../{{site.versions["stable"]}}/select-clause.html) statement using the [`--format=csv` flag](../{{site.versions["stable"]}}/cockroach-sql.html#general) to print the output into a file. For example:

{% include_cached copy-clipboard.html %}
~~~
$ cockroach sql -e 'SELECT * FROM test_database.table1' --format=csv --url='postgres://username:password@free-tier...' > users.txt
~~~

For an example on how to use this output to migrate to a paid CockroachCloud cluster, see [Migrate from a CockroachCloud Serverless (beta) to CockroachCloud Cluster](migrate-from-free-to-dedicated.html).