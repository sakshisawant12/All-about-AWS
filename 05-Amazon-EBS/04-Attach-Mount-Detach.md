# Attach, Mount, and Detach an Amazon EBS Volume

An Amazon EBS volume must be attached to an EC2 instance before it can be used.

After attaching the volume, you must:

1. Detect the new disk.
2. Create a file system.
3. Mount the volume.
4. (Optional) Configure automatic mounting after reboot.

---

# Workflow

```text
Create EBS Volume
        │
        ▼
Attach to EC2
        │
        ▼
Connect to EC2 (SSH)
        │
        ▼
Detect Disk
        │
        ▼
Create File System
        │
        ▼
Create Mount Point
        │
        ▼
Mount Volume
        │
        ▼
Verify Mount
        │
        ▼
(Optional) Configure /etc/fstab
```

---

# Step 1: Create an EBS Volume

1. Open the AWS Management Console.
2. Navigate to **EC2**.
3. In the left menu, select **Volumes**.
4. Click **Create Volume**.
5. Configure:
   - Volume Type: **gp3**
   - Size: **10 GiB** (Example)
   - Availability Zone: Same AZ as your EC2 instance
6. Click **Create Volume**.

---

# Step 2: Attach the Volume

1. Select the newly created volume.
2. Click **Actions** → **Attach Volume**.
3. Choose your EC2 instance.
4. Leave the default device name (for example, `/dev/xvdf`) or specify another valid device.
5. Click **Attach Volume**.

---

# Step 3: Connect to EC2

SSH into your EC2 instance.

```bash
ssh -i mykey.pem ec2-user@<Public-IP>
```

For Ubuntu:

```bash
ssh -i mykey.pem ubuntu@<Public-IP>
```

---

# Step 4: Check Available Disks

Use the following command:

```bash
lsblk
```

Example output:

```text
NAME    MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
xvda      202:0   0   8G  0 disk
└─xvda1   202:1   0   8G  0 part /

xvdf      202:80  0  10G  0 disk
```

The new disk (`xvdf`) is attached but not yet formatted or mounted.

---

# Step 5: Create a File System

Format the new volume using the ext4 file system.

```bash
sudo mkfs -t ext4 /dev/xvdf
```

or

```bash
sudo mkfs.ext4 /dev/xvdf
```

⚠️ **Warning:** Formatting deletes all existing data on the volume.

---

# Step 6: Create a Mount Point

A mount point is a directory where the volume will be accessible.

Example:

```bash
sudo mkdir /data
```

You can choose any directory name.

Examples:

```text
/data
/backup
/storage
/app-data
```

---

# Step 7: Mount the Volume

```bash
sudo mount /dev/xvdf /data
```

Now the EBS volume is accessible through `/data`.

---

# Step 8: Verify the Mount

### Check mounted file systems

```bash
df -h
```

Example:

```text
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda1       8G   2G    6G   25% /
/dev/xvdf       10G   24M   10G    1% /data
```

---

### Verify block devices

```bash
lsblk
```

Example:

```text
xvdf
└── /data
```

---

# Step 9: Test the Volume

Create a file:

```bash
cd /data

touch test.txt

echo "Hello AWS" > test.txt
```

Verify:

```bash
cat test.txt
```

Output:

```text
Hello AWS
```

---

# Step 10: Make the Mount Persistent

Without configuration, the volume will not mount automatically after a reboot.

Find the UUID:

```bash
sudo blkid
```

Example:

```text
/dev/xvdf: UUID="6f3d6d7d-xxxx-xxxx-xxxx-xxxxxxxx" TYPE="ext4"
```

Edit `/etc/fstab`:

```bash
sudo nano /etc/fstab
```

Add the following line:

```text
UUID=6f3d6d7d-xxxx-xxxx-xxxx-xxxxxxxx   /data   ext4   defaults,nofail   0   2
```

Save the file.

Test the configuration:

```bash
sudo mount -a
```

If there is no error, the configuration is correct.

---

# Detaching an EBS Volume

Before detaching a volume, unmount it.

```bash
sudo umount /data
```

Verify:

```bash
df -h
```

The `/data` mount should no longer appear.

Now detach the volume from the AWS Console:

1. Go to **EC2** → **Volumes**.
2. Select the volume.
3. Click **Actions** → **Detach Volume**.

---

# Reattaching the Volume

Attach the same volume to another EC2 instance in the **same Availability Zone**.

After attaching:

```bash
sudo mkdir /data

sudo mount /dev/xvdf /data
```

Your existing files will still be available because EBS is persistent.

---

# Important Linux Commands

| Command | Purpose |
|----------|---------|
| `lsblk` | List block devices |
| `blkid` | Display UUID information |
| `df -h` | Show mounted file systems |
| `mkfs.ext4` | Create an ext4 file system |
| `mkdir` | Create a mount point |
| `mount` | Mount a volume |
| `umount` | Unmount a volume |
| `nano /etc/fstab` | Configure automatic mounting |

---

# Common Mistakes

### Wrong Availability Zone

An EBS volume can only be attached to an EC2 instance in the **same Availability Zone**.

---

### Forgetting to Format

A new EBS volume must be formatted before use.

---

### Forgetting `/etc/fstab`

Without updating `/etc/fstab`, the volume will not automatically mount after a reboot.

---

### Detaching Without Unmounting

Always unmount the file system before detaching the volume to help avoid data corruption.

---

# Real-World Example

```text
AWS Console

↓

Create gp3 Volume

↓

Attach to EC2

↓

SSH into EC2

↓

lsblk

↓

mkfs.ext4

↓

mkdir /data

↓

mount /dev/xvdf /data

↓

Store Files

↓

Update /etc/fstab

↓

Automatic Mount After Reboot
```

---

# Interview Questions

## 1. Which command lists all block devices?

```bash
lsblk
```

---

## 2. Which command creates an ext4 file system?

```bash
mkfs.ext4 /dev/xvdf
```

---

## 3. Which command mounts a volume?

```bash
mount /dev/xvdf /data
```

---

## 4. Which file controls automatic mounting during boot?

```text
/etc/fstab
```

---

## 5. Which command shows mounted file systems?

```bash
df -h
```

---

## 6. Which command displays the UUID of a volume?

```bash
blkid
```

---

## Quick Revision

✅ Create EBS Volume

✅ Attach to EC2

✅ Check with `lsblk`

✅ Format using `mkfs.ext4`

✅ Create mount point

✅ Mount using `mount`

✅ Verify using `df -h`

✅ Find UUID using `blkid`

✅ Configure `/etc/fstab`

✅ Unmount before detaching
