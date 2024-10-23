---

copyright:
  years: 2024
lastupdated: "2024-10-15"

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# FAQs for cluster networks
{: #faqs-cluster-network}

Contact your IBM Support representative if you are interested in getting early access to this offering. It is currently available for early access users of the `gx3d-160x1792x8h100` [virtual server instance profiles](/docs/vpc?topic=vpc-profiles#gpu) in the `us-east` region.
{: beta}

This section contains answers to some frequently asked questions about {{site.data.keyword.cloud}} cluster networks.
{: shortdesc}

## Can my cluster networks communicate across subnets?
{: #can-my-cluster-networks-communicate-acrosssubnets}
{: faq}

Yes, as long as your cluster network virtual network interfaces configure the route correctly. Both virtual network interfaces must configure the route tables to send traffic through the same interface that it would receive traffic from the other virtual server (subnet). These route tables are needed only if the virtual server instances have at least one cluster network that is not present on the other virtual server instance.