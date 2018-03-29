# RAM Policy Editor

## [README of Chinese](https://github.com/aliyun/ram-policy-editor/blob/master/README-CN.md)

## About

Visual RAM Policy Editor for OSS.

## Demo

[http://gosspublic.alicdn.com/ram-policy-editor/index.html](http://gosspublic.alicdn.com/ram-policy-editor/index.html)

## Usage

### Resources

Resources can be represented in the following formats:

- A bucket: `my-bucket` (with no permission on objects in the bucket)
- All objects in a bucket: `my-bucket/*` (with no permission on the bucket itself, such as ListObjects)
- A directory in a bucket: `my-bucket/dir` (with no permission on objects under dir/)
- All objects under a directory in a bucket: `my-bucket/dir/*` (with no permission on dir, such as ListObjects)
- Complete resource path: `acs:oss:*:1234:my-bucket/dir`, where `1234` is the User ID (view the User ID in the console).

### EnablePath

When you want to grant permissions to a directory, you also need to grant the List permission on its upper level directory. For example, if you want to grant read and write permissions to `my-bucket/users/dir/*`, you also need to grant the following permissions to
view this directory in the console (or in other tools):

```
ListObjects my-bucket
ListObjects my-bucket/users
ListObjects my-bucket/users/dir
```

When the **EnablePath** option is selected, these permissions are granted automatically.

### Examples

#### Full access to a bucket

To grant all permissions to a bucket (such as `my-bucket`), add a rule as follows:

- **Effect**: Select `Allow`.
- **Actions**: Select `oss:*`.
- **Resources**: Enter `my-bucket` and `my-bucket/*`.
- **EnablePath**: Unselected.

> **Note:**
- For read-only permission, replace `oss:*` with `oss:Get*`.
- For write-only permission, replace `oss:*` with `oss:Put*`.

#### Full access to a directory

To grant all permissions to `my-dir` in `my-bucket`, add a rule as follows:

- **Effect**: Select `Allow`.
- **Actions**: Select `oss:*`. 
- **Resources**: Enter `my-bucket/my-dir/*`.
- **EnablePath**: Selected.

> **Note:**
- For read-only permission, replace `oss:*` with `oss:Get*`.
- For write-only permission, replace `oss:*` with `oss:Put*`.

#### Allow only specified IP

To allow only specified IP addresses to access the `my-dir` directory in `my-bucket`, add a rule as follows:

- **Effect**: Select `Allow`.
- **Actions**: Select `oss:Get*`.
- **Resources**: Enter `my-bucket/my-dir/*`.
- **EnablePath**: Unselected.
- **Conditions**: Click **Show** and add conditions as follows:
  - **Key**: Select `acs:SourceIp`. 
  - **Operator**: Select `IpAddress`.
  - **Value**: Enter the IP address, such as `40.32.9.125`.

> **Note:**
- For read-only permission, replace `oss:*` with `oss:Get*`.
- For write-only permission, replace `oss:*` with `oss:Put*`.

#### Web Console

To allow a RAM user to access the Alibaba Cloud console, you must grant the `oss:ListBuckets` permission by adding a rule as follows:

- **Effect**: Select `Allow`.
- **Actions**: Select `oss:ListBuckets`.
- **Resources**: Enter `*`.
- **EnablePath**: Unselected.

### Build

```
npm install -g browserify
npm install
npm run build
```
