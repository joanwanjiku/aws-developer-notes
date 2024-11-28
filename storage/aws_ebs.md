# EBS
## Notes
- EBS volumes are AZ specific
- Block-level storage that can be attached to an EC2 instance
- Only attached to one instance at a type. 
- Attach multiple EBS volumes to a single instance, volume and instance must be in same AZ
- Can use multi-attach to mount a volume to multiple instances at the same time
- multi-attach use cases:- achieve higher application availability in clustered linux applications, applications must manage concurrent write ops. Up to 16 instances in the same AZ, must use file system that is cluster-aware.
- Flexible:- can dynamically increase size, modify the provisioned IOPS capacity and change volume type on production volumes.
- Use for throughput intensive applications that perform continous disk scans.
- 

## EBS Encyrption
- Uses 256-bit advanced ecryption standard algorithms(AES-256).
- Amazon EBS uses AWS KMS master keys when creating the encyrpted volumes and any snapshots created from those.

## EBS Snapshots
- Data on volume can be backed via snapshots(stored in s3 for redundancy).
- Snapshots can be taken of running instances, encrypted volumes create encrypted snapshots, volumes restored from encrypted snapshots are encyrpted.
- Incremental backups, only blocks that have changed since the previous snapshots are backed up.
- Lazy loading:- when volume s created not all data is loaded from S3 when the instance requests not loaded data, it is downloaded immediately, and continues loading the rest of data in the background.
- A volume restored from a snapshot is created in the same region as the snapshot.
- Snapshots support cross-region copy.
- When you copy an unencyrpted snapshot that you own, you can encyrpt it during the copy process.
- EBS snapshot archive tier:- move an EBS snapshot to an archive tier that is 75% cheaper. takes 24-72hrs to restore the archive.
- EBS snapshots recycle bins
## EBS Raid Options
- Use raid configuration as you would on a standard bare-metal server
- Faster I/O performance than a single volume.
- *Raid 0*:- stripes multiple volumes together giving on-instance redundacy.
            1. Use when I/O performance is more important than fault tolerance. Performance of the stripe is limited to the least performing volume. Loss of a single volume results to loss of whole array, hence loss of data. Resulting size volume is sum of sizes of volume in the array.
- *Raid 1*:- mirrors two volumes together.
            2. When fault tolerance is important than I/O performance. Does not improve write performance. Needs more EC2 to bandwidth; data written to multiple volumes simultaneously.
- EBS volume data is replicated across multiple servers in one AZ, this prevents loss of data when a component fails, making EBS volumes more reliable than typical disk.
- Do not use RAID in boot volume. GRUB(bootloader) is only installed in one device, if one of the mirrored divices fails, OS may not boot.

## Volume types
[EBS volume types](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ebs-volume-types.html)
- **General Purpose SSD (gp2, gp3)**:- provides a balance of price and performance, recommended for most workloads. Max IOPS: 16,000. Max throughput per volume gp3(1000MiBs), gp2(250MiBs). can be used as boot volumes
- **Provisioned IOPS SSD (io1 and io2) block express**:- provides high performance for mission-critical, low latency or high-throughput workloads. max IOPS(64,000), Max throughput per volume io2 block express(4000MiBs), io2, io1(1000MiBs). can be used as boot volumes
- **Throughput Optimized HDD(st 1)**:- low cost HDD designed for frequently accessed, throughput-intensive workloads. 
- **Cold HDD(sc 1)**:- Lowest-cost HDD designed for less frequently accessed workloads.
- **Magnetic**:- Workloads where data is infrequently accessed.

## EBS snapshot encryption
- EBS Encryption uses KMS keys when creating encrypted volumes and snapshots; encrypts volume with a data key using the industry-standard AES-256 algorithm.
- Encryption happens on servers that host EC2 instances, securing both data at rest and data-in-transit
- By default, only you can create volumes from snapshots that you own. However, you can share your unencrypted snapshots with specific AWS accounts, or you can share them with the entire AWS community by making them public.
- You can share an encrypted snapshot only with specific AWS accounts. For others to use your shared, encrypted snapshot, you must also share the CMK key that was used to encrypt it.
- Users with access to your encrypted snapshot must create their own personal copy of it and then use that copy. Your copy of a shared, encrypted snapshot can also be re-encrypted using a different key.
[EBS volume encryption considerations](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html#EBSEncryption_c%20onsiderations)

## Amazon data lifecycle manager
You can use Amazon Data Lifecycle Manager to automate the creation, retention, and deletion of EBS snapshots and EBS-backed AMIs. When you automate snapshot and AMI management, it helps you to:-
    - Protect valuable data by enforcing a regular backup schedule.
    - Create standardized AMIs that can be refreshed at regular intervals.
    - Retain backups as required by auditors or internal compliance.
    - Reduce storage costs by deleting outdated backups.
    - Create disaster recovery backup policies that back up data to isolated accounts.

## EBS-optimized instances
EBS-optimized instances enable EC2 instances to fully use the IOPS provisioned on an EBS volume.

EBS-optimized instances deliver dedicated throughput between Amazon EC2 and Amazon EBS, with options between 500 and 60,000 Megabits per second (Mbps) depending on the instance type used. The dedicated throughput minimizes contention between Amazon EBS I/O and other traffic from your EC2 instance, providing the best performance for your EBS volumes.

EBS-optimized instances are designed for use with all Amazon EBS volume types.

    
- You can create point-in-time snapshots of EBS volumes, which are persisted to amazon s3.
- you can't create an unencrypted volume from an encrypted snapshot. Similarly, you can't create an unencrypted snapshot from an encrypted volume. 
- Encryption uses kms keys, both data at rest and data-in-transit. 
- You can attach multiple ebs volumes to a single instance, the volume and instance must be in the same AZ.
- Types of data encrypted is:
    1. Data at rest inside the volume.
    2. Data moving between the intance and the volume.
    3. All snapshots created from the volume.
    4. All volumes created from those snapshot.

- Create a volume, in AZ, select same AZ as the instance that you are attaching to, if creating volume from snapshot, select the snapshop ID, select encryption(best practise), tag your resource.
- Attach the volume to the EC2 instance, on instance's storage tab, delete on termination is false, this means volume persists when the instance is deleted, volume can be detached and attached to another instance.

## Create, configure file system on an attched ebs volume
- SSH into the instance or use the session manager
1. `df -h`  to see amount of free disk space
2. `sudo mkfs -t ext3 /dev/sdf` creates an ext3 filesystem on the /dev/sdf device, -t specify the type of the filesystem.
3. `sudo mkdir /mnt/<dir_name>` create a directory name for mounting the instance storage volume.
4. `sudo mount /dev/sdf /mnt/<dir_name>` mount allows you to attach the file systems to the storage device.
5. `echo "/dev/sdf /mnt/<dir_name> ext3 defaults,noatime 1 2" | sudo tee -a /etc/fstab ` this appends the details about the /dev/sdf device and its mount point towards the end of the /etc/fstab file. 
6. `sudo sh -c "echo some text has been written > /mnt/<dir_name>/<file_name>"` creates a new text file with some sample data on the newly mounted volume.
7. You can modify the size of your attached volume, after this, extend the filesystem to the available space using `sudo resize2fs /dev/nvme1n1`

## Modifying EBS volume type and provisioned performance for an existing app.
- You can modify(add, remove) the volume type and add IOPS and throughtput to a volume attached to an instance. select volume > actions > modify volume

## Snapshot for EBS volume
- Its is good practise to create snapshots of your volumes, select volume, then actions > create snapshot. Add the appropriate tags

## Volume from a snapshot
- you can create snapshots to clone or restore volumes, when you create an EBS volume based on a snapshot, the new volume beh=gins as an exact replica of the original volume that was used to create the snapshot. 
1. on snapshots tab, select the snapshot > create volume > create volume. Tag it properly.
2. On volumes, select restored volume created > attach volume to the instance > attach
3. In the instance, create a directory for mounting the attached volume, 
4. mount the new volume using the mount command.