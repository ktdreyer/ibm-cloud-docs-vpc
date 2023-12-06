---

copyright:
  years: 2022, 2023
lastupdated: "2023-12-05"

keywords: consistency group, snapshots, backups, instance snapshot, instance backup,

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing snapshot consistency groups
{: #snapshots-vpc-manage-consistency-groups}

A snapshot consistency group contains snapshots of multiple volumes that are attached to the same virtual server instance. The snapshots are loosely coupled. So, you can manage the snapshots within a consistency group in the same way that you manage any other snapshot. You can rename or delete individual snapshots from the consistency group if you want to. Or you can keep individual snapshots after you decide to delete the consistency group.
{: shortdesc}

If you update the backup consistency group to keep the individual snapshots after the consistency group is deleted, a backup job is not created when the consistency group is deleted. The system creates an event in the Activity Tracker when the consistency group is deleted.
{: note}

## Updating a consistency group in the UI
{: #update-consistencygroup-ui}
{: ui}

1. Go to the list of snapshot consistency groups. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure ![VPC icon](../../icons/vpc.svg) > Storage > Block Storage Snapshots**.
1. On the Consistency groups tab, click the name of a group on the list to display the consistency group details.
1. You can change the following attributes:
   * To change the name on the group, click the pencil icon and enter the new name.
   * Switch the toggle to enable or disable the deletion of the snapshots when the consistency group is deleted.

## Deleting a consistency group in the UI
{: #delete-consistencygroup-ui}
{: ui}

1. Go to the list of snapshot consistency groups. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure ![VPC icon](../../icons/vpc.svg) > Storage > Block Storage Snapshots**.
1. On the Consistency groups tab, click the name of a group on the list.
1. Click the Actions > Delete.
1. Type `Delete`.
1. Click **Delete**.

## Updating a consistency group from the CLI
{: #update-consistencygroup-cli}
{: cli}

You can update a consistency group from the CLI by running the following command. You can use the CLI to update the name of the resource and switch the `delete_snapshots_on_delete` property to keep the snapshots after the snapshot consistency group is deleted.

```sh
ibmcloud is snapshot-consistency-group-update CONSISTENCY_GROUP_ID
```
{: pre}

The following example changes the consistency group name. It identifies the consistency group by name. You can also use the ID of the consistency group with this command.

```sh
ibmcloud is snapshot-consistency-group-update multiple-snapshots-consistency-group-1 --name my-consistency-group               

Updating snapshot consistency group multiple-snapshots-consistency-group-1 under account Test Account as user test.user@ibm.com...
                               
ID                          r174-ed7c034e-9bd1-4474-83d0-f5b050f1490a   
Name                        my-consistency-group   
CRN                         crn:v1:bluemix:public:is:us-south:a/a123456::snapshot-consistency-group:r174-ed7c034e-9bd1-4474-83d0-f5b050f1490a   
Href                        https://us-south.iaas.cloud.ibm.com/v1/snapshot_consistency_groups/r174-ed7c034e-9bd1-4474-83d0-f5b050f1490a   
Status                      stable   
Backup policy plan          -   
Delete snapshot on delete   true   
Source Snapshot             -   
Resource group              ID                                 Name      
                            11caaa983d9c4beb82690daab08717e9   Default      
                               
Created                     2023-08-25T10:55:37+05:30   
Service Tags                -   
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshot-consistency-group-update`](/docs/vpc?topic=vpc-vpc-reference#snapshot-consistency-group-update).

## Deleting a consistency group from the CLI
{: #delete-consistencygroup-cli}
{: cli}

You can delete a consistency group from the CLI by running the following command.

```sh
ibmcloud is snapshot-consistency-group-delete CONSISTENCY_GROUP_ID
```
{: pre}

The following example deletes two consistency groups that are identified by their names.

```sh
$ ibmcloud is snapshot-consistency-group-delete snapshot-consistency-group-1 snapshot-consistency-group-2

This will delete snapshot consistency group snapshot-consistency-group-1, snapshot-consistency-group-2 and cannot be undone. Continue [y/N] ?> y

Deleting snapshot consistency group snapshot-consistency-group-1, snapshot-consistency-group-2 under account Test account as user test.user@ibm.com...
OK

Deletion request for snapshot consistency groups snapshot-consistency-group-1, snapshot-consistency-group-2 has been accepted.
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshot-consistency-group-delete`](/docs/vpc?topic=vpc-vpc-reference#snapshot-consistency-group-delete).

## Updating a consistency group with the API
{: #update-consistencygroup-api}
{: api}

You can programmatically update a consistency group by calling the `/snapshot_consistency_groups/{id}` method in the [VPC API](/apidocs/vpc-scoped#update-snapshot-consistency-group){: external} as shown in the following sample request. You can use the API to update the name of the resource and switch the `delete_snapshots_on_delete` property to keep the snapshots after the snapshot consistency group is deleted.

```sh
curl -X PATCH\
"$vpc_api_endpoint/v1/snapshot_consistency_groups/$consistency_group_id?version=2023-12-05&generation=2" 
-H "Authorization: $iam_token"
-d "{\
   "delete_snapshot_on_delete_snapshot_consistency_group":false,\
   "name":"my-snapshot-consistency-group"}"
```
{: pre}

## Deleting a consistency group with the API
{: #delete-consistencygroup-api}
{: api}

You can programmatically delete a consistency group by calling the `/snapshot_consistency_groups/{id}` method in the [VPC API](/apidocs/vpc-scoped#delete-snapshot-consistency-group){: external} as shown in the following sample request.

```sh
curl -X DELETE\
"$vpc_api_endpoint/v1/snapshot_consistency_groups/$consistency_group_id?version=2023-12-05&generation=2" 
-H "Authorization: $iam_token"
```
{: pre}

## Activity Tracker events
{: #consistency-groups-at-events}

All multi-volume snapshot operations generate events in {{site.data.keyword.at_full_notm}} regardless if the consistency group was created manually or by the Backup service. The following table lists the actions that relate to snapshot consistency group resources.

| Resource                     |  Action                                                           | Description  |
|------------------------------|-------------------------------------------------------------------|--------------|
| `snapshot-consistency-group` | `is.snapshot-consistency-group.snapshot-consistency-group.create` | The snapshot consistency group was created. |
| `snapshot-consistency-group` | `is.snapshot-consistency-group.snapshot-consistency-group.update` | The snapshot consistency group was updated. |
| `snapshot-consistency-group` | `is.snapshot-consistency-group.snapshot-consistency-group.delete` | The snapshot consistency group was deleted. |
| `snapshot-consistency-group` | `is.snapshot-consistency-group.snapshot-consistency-group.read`   | One or more snapshot consistency groups were retrieved.|
{: caption="Table 1. IAM roles for snapshots" caption-side="bottom"}