# Kox Downloader

`Kox Downloader` 是一个简单的 Python 脚本，用于从 [Kox](https://kox.moe/) 网站下载漫画。它支持登录、下载指定书籍及其卷，并支持回调脚本执行。

## 功能

- 以命令行参数登录网站并持久化凭证
- 支持获取用户订阅的漫画供选择
- 多线程下载书籍和卷，失败重试、断点续传
- 自定义下载完成的回调命令

## 安装依赖

在使用本脚本之前，请确保你已经安装了项目所需要的依赖：

```bash
pip install -r requirements.txt
```

## 使用方法

### 1. 登录 `kox.moe`

首先需要登录 `kox.moe` 并保存登录状态（Cookie）。

```bash
python koxdl.py login -u <your_username> -p <your_password>
```

### 2. 下载漫画书籍

你可以通过以下命令下载指定书籍或卷：

```bash
python koxdl.py download -d /path/to/download/destination --book-id 123 --volume 1,2,3
```

#### 参数说明：

- `-d`, `--dest`: 下载目录
- `--book-id`: 指定书籍的 ID
- `--book-name`: 搜索已关注的书籍并按名称下载
- `-v`, `--volume`: 指定卷，多个卷使用逗号分隔，`all` 表示下载所有卷
- `--overwrite`: 如果文件已存在，是否覆盖（默认不覆盖）
- `-p`, `--proxy`: 使用代理服务器下载
- `-r`, `--retry`: 下载失败时的重试次数
- `--call-back`, `-c`: 下载完成后的回调脚本（使用方式详见 [4. 回调函数](# 4. 回调函数)）

### 3. 查看账户状态

查看当前账户信息（例如：账户名和配额等）：

```bash
python koxdl.py status
```

### 4. 回调函数

你可以设置一个回调函数，下载完成后执行。回调可以是任何你想要的命令：

```bash
python koxdl.py download -d /path/to/destination --book-id 123 --volume 1,2 --call-back "echo '{v.name} downloaded!'"
```

> 字符串模板会直接朴素地替换，卷名或者书名可能会包含空格，推荐使用引号包含避免出现错误。

`{v.name}` 会被替换为卷的名称。可以使用的参数：

| 变量名             | 描述                   |
| ------------------ | ---------------------- |
| v.url              | 卷的下载链接           |
| v.name             | 卷的名称               |
| v.size             | 卷的文件大小           |
| v.book.name        | 对应漫画的名字         |
| v.book.book_id     | 对应漫画的标识         |
| v.book.url         | 对应漫画的主页链接     |
| v.book.author      | 对应漫画的作者         |
| v.book.status      | 对应漫画的更新状态     |
| v.book.last_update | 对应漫画的最后更新时间 |


## 参数说明

### login 子命令

用于登录并保存 Cookie：

```bash
python koxdl.py login -u <your_username> -p <your_password>
```

- `-u`, `--username`: `kox.moe` 用户名
- `-p`, `--password`: `kox.moe` 密码

> cookie 等内容存放在 `~/.koxdl` 中，如果删除可能会导致无法获取登录凭证。

### download 子命令

用于下载指定的书籍和卷：

```bash
python koxdl.py download -d /path/to/download/destination --book-id 123 --volume 1,2
```

- `-d`, `--dest`: 下载目标目录
- `--book-id`: 书籍 ID
- `--book-name`: 按书名搜索关注的书籍
- `-v`, `--volume`: 要下载的卷，支持逗号分隔，`all` 表示所有卷
- `--overwrite`: 如果文件存在，是否覆盖
- `-p`, `--proxy`: 使用代理
- `-r`, `--retry`: 最大重试次数（默认 3）
- `--call-back`, `-c`: 下载后的回调脚本

### status 子命令

查看当前账户状态：

```bash
python koxdl.py status
```

- `-p`, `--proxy`: 使用代理（可选）

## 目录结构

- `koxdl.py`: 主脚本文件
- `.koxdl`: 存储用户 Cookie 的文件（登录时自动生成）

## 开发者

- 作者: `@chrisis58`
- GitHub: [https://github.com/chrisis58](https://github.com/chrisis58)
