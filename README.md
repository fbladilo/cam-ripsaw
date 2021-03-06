# CAM POC ripsaw role

## Purpose
The goal of this CAM POC is to serve as a base to identify any challenges and feasibility for ripsaw consumption.

### Prerequisites

* OCP source and destination clusters, supported releases OCP 3.7 to 4.x
* Cluster admin privileged account for source and destination clusters
* Amazon AWS ansible cloud modules
* RH subscription credentials
* Podman
* OC client
* k8s ansible modules

### Overview
This project will install CAM on an existing OpenShift cluster pair, deploy a sample application workload with PVs on the source cluster and migrates to the destination cluster. The amount of time exclusively spent on the controller **migration** phase is reported.

### Usage

`$ ansible-playbook cam_ripsaw.yml -e cam_src_cluster_api=<src-cluster-api-url> -e cam_src_cluster_pass=<src-cluster-admin-pass> -e cam_dst_cluster_api=<dst-cluster-api-url> -e cam_dst_cluster_pass=<dst-cluster-admin-pass> -e rh_sub_user=<RH registry user> -e rh_sub_pass=<RH registry pass>`

_**Note:**_  Username for both clusters by default set to _admin_ , AWS_REGION, AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY must be set on shell environment, see this and other variables below.

### Role variables

| Parameter | Purpose | Default |
| --- | --- | --- |
| `cam_src_cluster_api`| Source cluster API URL | none |
| `cam_dst_cluster_api`| Destination cluster API URL | none |
| `cam_src_cluster_user`| Cluster admin user on source cluster | admin |
| `cam_dst_cluster_user`| Cluster admin user on destination cluster | admin |
| `cam_src_cluster_pass`| Cluster admin pass on source cluster | none |
| `cam_dst_cluster_pass`| Cluster admin pass on destination cluster | none |
| `cam_version`| CAM version to deploy on cluster pair | v1.1 |
| `cam_controller_host`| Cluster that will host CAM controller | destination |
| `cam_storage_provider`| Storage provider for CAM intermediate storage | aws |
| `cam_src_storage_class`| Source cluster storage class to allocate PVs | default |
| `cam_dst_storage_class`| Destination cluster storage class to allocate PVs | gp2 |
| `cam_pv_action`| Action to take when migrating PVs, valid options are : copy/move | copy |
| `cam_pv_copy_method`| Copy method used when migrating PVs, valid options are : filesystem/snapshot | filesystem |
| `cam_app_pv_size`| Size of PVs requested | 1Gi |
| `aws_region`| AWS region where s3 volumes will be created | AWS_REGION env |
| `aws_access_key_id`| AWS access key id, used by AWS modules | AWS_ACCESS_KEY_ID env |
| `aws_secret_access_key`| AWS secret access key, used by AWS modules | AWS_SECRET_ACCESS_KEY env |
| `rh_sub_user`| RH subscription username, used to access CAM bits | none |
| `rh_sub_pass`| RH subscription password, used to access CAM bits | none |

### Other notes

* The sample migration application is **nginx** by default deploying with 2 PVs
* Podman is only used for v3 deployments, operator and controller CRs are extracted from GA images
* OC client is used for login purposes and serves other special tasks where k8s has limitations
* Support only AWS s3 intermediate storage at the moment, this can be extended
