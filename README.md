# ram-policy-editor

Visual RAM Policy Editor

## Demo

http://gosspublic.alicdn.com/ram-policy-editor/index.html

## Usage

### Resources

Resources有如下几种格式：

- 表示某个bucket: `my-bucket` （此时对bucket下的文件没有权限）
- 表示某个bucket下面所有文件: `my-bucket/*` （此时对bucket本身没有权限，例如ListObjects）
- 表示某个bucket下某个目录: `my-bucket/dir` （此时对dir/下面的文件没有权限）
- 表示某个bucket下某个目录下面所有文件: `my-bucket/dir/*` （此时对dir没有权限，例如ListObjects）
- 填写完整的资源路径：`acs:oss:*:1234:my-bucket/dir`，其中`1234`为用户的User ID（在控制台查看）

### EnablePath

当用户需要对某个目录授权时，往往还需要保证对上一层目录也有List权限，例
如用户对`my-bucket/users/dir/*`赋予读写权限，为了在控制台（或其他工具）
能够查看这个目录，用户还需要以下权限：

```
ListObjects my-bucket
ListObjects my-bucket/users
ListObjects my-bucket/users/dir
```

勾选EnablePath选项时，上面这些权限会自动添加。

### Examples

#### Full access to a bucket

对某个bucket（例如`my-bucket`）完全授权：

添加一条规则：

- Effect设置为`Allow`
- Action选择`oss:*`
- Resource填写为`my-bucket`和`my-bucket/*`
- EnablePath不勾选

注意：

- 如果是只读权限，把`oss:*`换成`oss:Get*`;
- 如果是只读权限，把`oss:*`换成`oss:Put*`;

#### Full access to a dir

对`my-bucket`下的`my-dir`完全授权：

添加一条规则：

- Effect设置为`Allow`
- Action选择`oss:*`
- Resource填写为`my-bucket/my-dir/*`
- EnablePath勾选

注意：

- 如果是只读权限，把`oss:*`换成`oss:Get*`;
- 如果是只读权限，把`oss:*`换成`oss:Put*`;

#### Allow only specified IP

只允许特定的IP来访问`my-bucket`下面的`my-dir`目录。

添加一条规则：

- Effect设置为`Allow`
- Action选择`oss:Get*`
- Resource填写为`my-bucket/my-dir/*`
- EnablePath不勾选
- 添加条件
  - Key选择为`acs:SourceIp`
  - Operator选择为`IpAddress`
  - Value填具体的IP，如`40.32.9.125`

注意：

- 如果是只读权限，把`oss:*`换成`oss:Get*`;
- 如果是只读权限，把`oss:*`换成`oss:Put*`;

#### Web console

如果需要在阿里云的控制台用子账号访问，需要有`oss:ListBuckets`权限。

需要额外添加一条规则：

- Effect设置为`Allow`
- Action选择`oss:ListBuckets`
- Resource填写为`*`
- EnablePath不勾选

### Build

```
npm install -g browserify
npm install
npm run build
```
