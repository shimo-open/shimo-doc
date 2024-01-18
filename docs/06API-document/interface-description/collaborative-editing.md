---
title: 协同编辑
description: 如何使用API接口创建、处理文档
keywords:
  [
    石墨文档,
    文档中台,
    协同办公,
    在线文档,
    文件共享,
    元素,
    单元格,
    格式,
    应用,
    文件,
  ]
sidebar_position: 2
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

## 基础功能

### 创建协同文档 {#create-collab-file}

创建指定类型的协同文档，创建后加载编辑器。

| 支持的文件类型 | 类型值       |
| :------------- | :----------- |
| 轻文档         | document     |
| 表格           | spreadsheet  |
| 传统文档       | documentPro  |
| 幻灯片         | presentation |
| 应用表格       | table        |

:::caution 说明

- 除 `轻文档`、`传统文档`、`表格` 和 `幻灯片`，其他类型为非标准功能 (如 `应用表格`) ，详询石墨客服。
- 除`轻文档`、`应用表格`之外，其他文件类型均高度兼容 Office 格式

:::

**请求地址**

_POST_ https://shimo-domain/sdk/v2/api/files

**HTTP Request Parameters**

| 传参方式 | 参数名 | 类型   | 值示例 | 必选 | 说明                                                                                                                               |
| :------- | :----- | :----- | :----- | :--- | :--------------------------------------------------------------------------------------------------------------------------------- |
| Query    | lang   | string | en     | 否   | 可选。默认值为服务端默认语言设置，通常默认为 `zh-CN` 。指定创建文件时的语言信息，可选值：`zh-CN`(简体中文)、`en`(英文)、`ja`(日文) |

**HTTP Request Headers**

| Header 名       | 值                      | 必选 | 说明                                                                                                                                                                                                     |
| :-------------- | :---------------------- | :--- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Accept-Language | zh-CN,zh;q=0.9,en;q=0.8 | 否   | 若无 `lang` 参数默认尝试从此 Header 识别，若未传此 Header 则使用服务器默认语言。Accept-Languge 格式参考 [MDN Accept-Language](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept-Language) |

**HTTP Request Body**

| 字段名 | 类型   | 值示例     | 必选 | 说明                                                    |
| :----- | :----- | :--------- | :--- | :------------------------------------------------------ |
| type   | string | document   | 是   | 石墨文件类型，可选值参考 「支持的文件类型」 部分        |
| fileId | string | fileid1001 | 是   | 接入服务商的文件的唯一 ID，必须为不超过 64 长度的字符串 |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'php', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request POST 'https://shimo-domain/sdk/v2/api/files' \
--header 'Content-Type: application/json' \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token' \
--data-raw '{"type": "document","fileId": "83478944-8d4c-4936-baff-20ab180eb712"}'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\"type\": \"document\",\"fileId\": \"1847cf35-a7aa-4cfe-b7fb-e1fc47d76838\"}");
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/files")
  .method("POST", body)
  .addHeader("Content-Type", "application/json")
  .addHeader("X-Shimo-Signature", "your_signature")
  .addHeader("X-Shimo-Token", "your_token")
  .build();
Response response = client.newCall(request).execute();
```

</TabItem>
<TabItem value="go">

```go
package main

import (
  "fmt"
  "strings"
  "net/http"
  "io/ioutil"
)

func main() {
  url := "https://shimo-domain/sdk/v2/api/files"
  method := "POST"

  payload := strings.NewReader(`{
    "type": "document",
    "fileId": "9f4af4e6-f118-4d4b-b485-48d5ead34c89"
}`)

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
    return
  }
  req.Header.Set("Content-Type", "application/json")
  req.Header.Set("X-Shimo-Signature", "your_signature")
  req.Header.Set("X-Shimo-Token", "your_token")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");
var data = JSON.stringify({
  type: "document",
  fileId: "8ae0186b-6ee5-4c45-82d1-1bd149cd94b5",
});

var config = {
  method: "post",
  url: "https://shimo-domain/sdk/v2/api/files",
  headers: {
    "Content-Type": "application/json",
    "X-Shimo-Signature": "your_signature",
    "X-Shimo-Token": "your_token",
  },
  data: data,
};

axios(config)
  .then(function (response) {
    console.log(JSON.stringify(response.data));
  })
  .catch(function (error) {
    console.log(error);
  });
```

</TabItem>
<TabItem value="php">

```php
<?php
require 'vendor/autoload.php';
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Request;
$client = new Client();
$headers = [
  'Content-Type' => 'application/json',
  'X-Shimo-Signature' => 'your_signature',
  'X-Shimo-Token' => 'your_token'
];
$body = '{
  "type": "document",
  "fileId": "83478944-8d4c-4936-baff-20ab180eb712"
}';
$request = new Request('POST', 'https://shimo-domain/sdk/v2/api/files', $headers, $body);
$res = $client->sendAsync($request)->wait();
echo $res->getBody();



```

</TabItem>
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 204    | 创建成功 |

**HTTP Response Body**

空

### 创建协同文档副本

创建指定协同文档的副本。

**请求地址**

_POST_ https://shimo-domain/sdk/v2/shimo-files/{fileId}/copy

**HTTP Request Body**

| 字段名 | 类型   | 值示例     | 必选 | 说明                                                   |
| :----- | :----- | :--------- | :--- | :----------------------------------------------------- |
| fileId | string | fileid1001 | 是   | 新文件 ID，接入方的文件唯一 ID，字符串长度不超过 64 。 |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'php', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request POST 'https://shimo-domain/sdk/v2/api/files/e0f238d1-8c10-448c-bf69-cd315051c095/copy' \
--header 'Content-Type: application/json' \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token' \
--data-raw '{"fileId": "83478944-8d4c-4936-baff-20ab180eb712"}'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\"fileId\": \"1847cf35-a7aa-4cfe-b7fb-e1fc47d76838\"}");
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/files/e0f238d1-8c10-448c-bf69-cd315051c095/copy")
  .method("POST", body)
  .addHeader("Content-Type", "application/json")
  .addHeader("X-Shimo-Signature", "your_signature")
  .addHeader("X-Shimo-Token", "your_token")
  .build();
Response response = client.newCall(request).execute();
```

</TabItem>
<TabItem value="go">

```go
package main

import (
  "fmt"
  "strings"
  "net/http"
  "io/ioutil"
)

func main() {
  url := "https://shimo-domain/sdk/v2/api/files/e0f238d1-8c10-448c-bf69-cd315051c095/copy"
  method := "POST"

  payload := strings.NewReader(`{
    "type": "document",
    "fileId": "9f4af4e6-f118-4d4b-b485-48d5ead34c89"
}`)

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
    return
  }
  req.Header.Set("Content-Type", "application/json")
  req.Header.Set("X-Shimo-Signature", "your_signature")
  req.Header.Set("X-Shimo-Token", "your_token")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");
var data = JSON.stringify({
  fileId: "8ae0186b-6ee5-4c45-82d1-1bd149cd94b5",
});

var config = {
  method: "post",
  url: "https://shimo-domain/sdk/v2/api/files/e0f238d1-8c10-448c-bf69-cd315051c095/copy",
  headers: {
    "Content-Type": "application/json",
    "X-Shimo-Signature": "your_signature",
    "X-Shimo-Token": "your_token",
  },
  data: data,
};

axios(config)
  .then(function (response) {
    console.log(JSON.stringify(response.data));
  })
  .catch(function (error) {
    console.log(error);
  });
```

</TabItem>
<TabItem value="php">

```php
<?php
require 'vendor/autoload.php';
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Request;
$client = new Client();
$headers = [
  'Content-Type' => 'application/json',
  'X-Shimo-Signature' => 'your_signature',
  'X-Shimo-Token' => 'your_token'
];
$body = '{
  "fileId": "83478944-8d4c-4936-baff-20ab180eb712"
}';
$request = new Request('POST', 'https://shimo-domain/sdk/v2/api/files/e0f238d1-8c10-448c-bf69-cd315051c095/copy', $headers, $body);
$res = $client->sendAsync($request)->wait();
echo $res->getBody();



```

</TabItem>
</Tabs>

**HTTP 状态码**

| 状态码 | 说明                                                                                  |
| :----- | :------------------------------------------------------------------------------------ |
| 204    | 创建成功                                                                              |
| 200    | 重复请求时，文件拷贝正在执行，响应数据 `{"code":70019}`                               |
| 400    | 重复请求时，目标文件关联数据存在但无法找到创建副本任务信息，响应数据 `{"code":70016}` |
| 400    | 重复请求时，目标文件关联数据存在，创建副本执行失败，响应数据 `{"code":70017}`         |
| 500    | 重复请求时，创建副本时获取源文件内容遇到错误，响应数据 `{"code":70015}`               |
| 500    | 重复请求时，获取创建副本任务时遇到错误，响应数据 `{"code":70018}`                     |
| 500    | 重复请求时，未知的创建副本任务状态, 响应数据 `{"code":70020}`                         |

**HTTP Response Body**

空

### 删除协同文档

删除指定类型的协同文档，要求用户对文档有 `manageable` 权限。

**请求地址**

_DELETE_ https://shimo-domain/sdk/v2/api/files/{fileId}

**HTTP Request Body**

无

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'php', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request DELETE 'https://shimo-domain/sdk/v2/api/files' \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "");
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/files/5c80a323-47d3-4e69-9842-1682d0ca604c")
  .method("DELETE", body)
  .addHeader("X-Shimo-Signature", "your_signature")
  .addHeader("X-Shimo-Token", "your_token")
  .build();
Response response = client.newCall(request).execute();
```

</TabItem>
<TabItem value="go">

```go
package main

import (
  "fmt"
  "net/http"
  "io/ioutil"
)

func main() {
  url := "https://shimo-domain/sdk/v2/api/files/1785c0b3-6157-499c-a493-60a034fe03c2"
  method := "DELETE"

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, nil)

  if err != nil {
    fmt.Println(err)
    return
  }

  req.Header.Set("X-Shimo-Signature", "your_signature")
  req.Header.Set("X-Shimo-Token", "your_token")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");

var config = {
  method: "DELETE",
  url: "https://shimo-domain/sdk/v2/api/files/2ef0f99b-6e1b-48c1-bb61-b7c2eac0914c",
  headers: {
    "X-Shimo-Signature": "your_signature",
    "X-Shimo-Token": "your_token",
  },
};

axios(config)
  .then(function (response) {
    console.log(JSON.stringify(response.data));
  })
  .catch(function (error) {
    console.log(error);
  });
```

</TabItem>
<TabItem value="php">

```php
<?php
require 'vendor/autoload.php';
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Request;
$client = new Client();
$headers = [
  'X-Shimo-Signature' => 'your_signature',
  'X-Shimo-Token' => 'your_token'
];
$request = new Request('DELETE', 'https://shimo-domain/sdk/v2/api/files', $headers);
$res = $client->sendAsync($request)->wait();
echo $res->getBody();



```

</TabItem>
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 204    | 操作成功 |

**HTTP Response Body**

空

### 获取历史列表 {#doc-sidebar-info}

获取文件侧边栏历史列表信息，历史类型分为操作历史和编辑历史。

**请求地址**

_GET_ https://shimo-domain/sdk/v2/shimo-files/{fileId}/doc-sidebar-info

**HTTP Request Parameters**

| 传参方式 | 参数名      | 类型   | 值示例           | 必选 | 说明                                                                                                                                             |
| :------- | :---------- | :----- | :--------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId      | string | qeK4Xdxvxg8jF5gz | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |
| Query    | pageSize    | number | 10               | 否   | 每一页返回的条数，推荐值 `10`、`20` ，设的过大将会影响响应时间。默认值为 `10`                                                                    |
| Query    | count       | number | 0                | 否   | 当前页需要跳过的记录数，可通过 `count = (page - 1) * pageSize` 求得。默认值为 `0`                                                                |
| Query    | historyType | number | 1                | 否   | 可选值：`1` 操作历史 （如锁定单元格此类未实际编写内容的修改产生的操作历史），`2` 编辑历史。未传时，默认返回所有类型                              |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'php', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl "https://shimo-domain/sdk/v2/shimo-files/{fileId}/doc-sidebar-info" \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/shimo-files/{fileId}/doc-sidebar-info")
  .method("GET", null)
  .addHeader("X-Shimo-Signature", "your_signature")
  .addHeader("X-Shimo-Token", "your_token")
  .build();
Response response = client.newCall(request).execute();
```

</TabItem>
<TabItem value="go">

```go
package main

import (
  "fmt"
  "net/http"
  "io/ioutil"
)

func main() {
  url := "https://shimo-domain/sdk/v2/shimo-files/%3CfileId%3E/doc-sidebar-info"
  method := "GET"

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, nil)

  if err != nil {
    fmt.Println(err)
    return
  }

  req.Header.Set("X-Shimo-Signature", "your_signature")
  req.Header.Set("X-Shimo-Token", "your_token")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");

var config = {
  method: "GET",
  url: "https://shimo-domain/sdk/v2/shimo-files/{fileId}/doc-sidebar-info",
  headers: {
    "X-Shimo-Signature": "your_signature",
    "X-Shimo-Token": "your_token",
  },
};

axios(config)
  .then(function (response) {
    console.log(JSON.stringify(response.data));
  })
  .catch(function (error) {
    console.log(error);
  });
```

</TabItem>
<TabItem value="php">

```php
<?php
require 'vendor/autoload.php';
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Request;
$client = new Client();
$headers = [
  'X-Shimo-Signature' => 'your_signature',
  'X-Shimo-Token' => 'your_token'
];
$request = new Request('GET', 'https://shimo-domain/sdk/v2/shimo-files/{fileId}/doc-sidebar-info', $headers);
$res = $client->sendAsync($request)->wait();
echo $res->getBody();

```

</TabItem>
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 200    | 返回成功 |

**HTTP Response Body**

| 字段名                   | 类型    | 值示例                   | 说明                                           |
| :----------------------- | :------ | :----------------------- | :--------------------------------------------- |
| histories                | array   | -                        | 侧边栏历史数组                                 |
| histories[i].content     | string  | -                        | 协作文件格式数据                               |
| histories[i].createdAt   | string  | 2021-06-07T06:12:24Z     | 本条侧边栏历史创建时间                         |
| histories[i].historyType | number  | 2                        | 侧边栏历史类型，1 为操作历史，2 为编辑产生     |
| histories[i].id          | string  | 60bdb8c847a7850006bf12c1 | 侧边栏历史 ID                                  |
| histories[i].name        | string  | 历史 1                   | 侧边栏历史名称                                 |
| histories[i].updatedAt   | string  | 2021-06-07T06:12:24Z     | 侧边栏历史最后更新时间                         |
| histories[i].userId      | string  | user123,user134          | 服务商用户 ID，可能有多个，以英文逗号 "," 分隔 |
| isLastPage               | boolean |                          | 是否最后一页                                   |
| limit                    | number  | 100                      | 分页大小                                       |
| users                    | object  | { "user123": "用户 A" }  | 接入方用户 ID 对应的用户名映射                 |

Example

```json
{
  "histories": [
    {
      "content": "{\"range\":[1,1],\"changeset\":\"[[10, \\\"\\\\n\\\", \\\"line:\\\\\\\"init\\\\\\\"\\\"]]\",\"frozen\":true}",
      "createdAt": "2021-06-07T06:12:24Z",
      "historyType": 2,
      "id": "60bdb8c847a7850006bf12c1",
      "name": "",
      "updatedAt": "2021-06-07T06:12:24Z",
      "userId": "user123,user134"
    },
    {
      "content": "{\"range\":[2,3],\"changeset\":\"省略若干内容 ...\"}",
      "createdAt": "2021-06-07T06:12:39Z",
      "historyType": 2,
      "id": "60bdb8d747a7850006bf12c2",
      "name": "",
      "updatedAt": "2021-06-07T06:12:39Z",
      "userId": "user123,user134"
    },
    {
      "content": "{\"action\":\"unlock_cell\",\"range\":[\"E14:E14\"],\"name\":\"工作表1\"}",
      "createdAt": "2022-03-11T03:58:59Z",
      "historyType": 1,
      "id": "622ac9034079aa0006d54f3b",
      "name": "",
      "updatedAt": "2022-03-11T03:58:59Z",
      "userId": "user123"
    },
    {
      "content": "{\"action\":\"lock_sheet\",\"name\":\"工作表1\"}",
      "createdAt": "2022-03-11T03:59:04Z",
      "fileGuid": "Vi9rVPy7v6RJvLZK",
      "historyType": 1,
      "id": "622ac9084079aa0006d54f3c",
      "name": "",
      "updatedAt": "2022-03-11T03:59:04Z",
      "userId": "user123"
    }
  ],
  "isLastPage": true,
  "limit": null,
  "users": {
    "user123": "testuser",
    "user134": "testuser2"
  }
}
```

#### 操作历史说明 (historyType = 1)

表示手动操作而通常不造成文件内容等发生变化的记录，例如创建版本、锁定单元格等操作，此类历史的类型及上下文信息存于 content 字段中，格式为 JSON 字符串形式。

JSON 数据为 `histories[i].content` 字段，经过 JSON.parse()解析后的结果如下：

##### createRevision 创建版本

```json
{
  "action": "createRevision"
}
```

##### renameRevision 重命名版本

```json
{
  "action": "renameRevision",
  "before": "修改前的版本名称",
  "after": "修改后的版本名称"
}
```

##### deleteRevision 删除版本

```json
{
  "action": "deleteRevision",
  "label": "被删除的版本名称"
}
```

##### lock_cell 锁定单元格

```json
{
  "action": "lock_cell",
  "range": "E14:E14",
  "name": "工作表1"
}
```

##### unlock_cell 解锁单元格

```json
{
  "action": "unlock_cell",
  "range": ["E14:E14"],
  "name": "工作表1"
}
```

##### lock_sheet 锁定工作表

```json
{
  "action": "lock_sheet",
  "name": "工作表1"
}
```

##### unlock_sheet 解锁工作表

```json
{
  "action": "unlock_sheet",
  "name": "工作表1"
}
```

##### update_lock_cell 更新单元格锁定

```json
{
  "action": "update_lock_cell",
  "range": ["E17:E17"],
  "name": "工作表1"
}
```

#### 编辑历史说明 (historyType = 2)

表示一个或多个用户在一段时间内对文件内容进行了编辑。

### 获取版本列表

获取文件版本列表信息。

**请求地址**

_GET_ https://shimo-domain/sdk/v2/shimo-files/{fileId}/revisions

**HTTP Request Parameters**

| 传参方式 | 参数名 | 类型   | 值示例           | 必选 | 说明                                                                                                                                             |
| :------- | :----- | :----- | :--------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId | string | qeK4Xdxvxg8jF5gz | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'php', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request GET 'https://shimo-domain/sdk/v2/shimo-files/{fileId}/revisions' \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/shimo-files/{fileId}/revisions")
  .method("GET", null)
  .addHeader("X-Shimo-Signature", "your_signature")
  .addHeader("X-Shimo-Token", "your_token")
  .build();
Response response = client.newCall(request).execute();
```

</TabItem>
<TabItem value="go">

```go
package main

import (
  "fmt"
  "net/http"
  "io/ioutil"
)

func main() {
  url := "https://shimo-domain/sdk/v2/shimo-files/%3CfileId%3E/revisions"
  method := "GET"

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, nil)
  if err != nil {
    fmt.Println(err)
    return
  }

  req.Header.Set("X-Shimo-Signature", "your_signature")
  req.Header.Set("X-Shimo-Token", "your_token")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");

var config = {
  method: "GET",
  url: "https://shimo-domain/sdk/v2/shimo-files/{fileId}/revisions",
  headers: {
    "X-Shimo-Signature": "your_signature",
    "X-Shimo-Token": "your_token",
  },
};

axios(config)
  .then(function (response) {
    console.log(JSON.stringify(response.data));
  })
  .catch(function (error) {
    console.log(error);
  });
```

</TabItem>
<TabItem value="php">

```php
<?php
require 'vendor/autoload.php';
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Request;
$client = new Client();
$headers = [
  'X-Shimo-Signature' => 'your_signature',
  'X-Shimo-Token' => 'your_token'
];
$request = new Request('GET', 'https://shimo-domain/sdk/v2/shimo-files/{fileId}/revisions', $headers);
$res = $client->sendAsync($request)->wait();
echo $res->getBody();


```

</TabItem>
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 200    | 返回成功 |

**HTTP Response Body**

| 字段名       | 类型   | 值示例                   | 说明               |
| :----------- | :----- | :----------------------- | :----------------- |
| id           | string | 11089                    | 版本 ID            |
| label        | string | 2021/6/7 星期一 14:33    | 版本 Label         |
| title        | string | 123                      | 标题               |
| docHistoryId | string | 60bdbda2f0a8af000664d719 | 对应侧边栏历史 ID  |
| createdAt    | string | 2021-06-07T06:33:13Z     | 侧边栏历史创建时间 |
| updatedAt    | string | 2021-06-07T06:33:13Z     | 侧边栏历史更新时间 |
| user.id      | string | user123                  | 服务商用户 ID      |
| user.name    | string | testuser                 | 用户名             |

Example

```json
[
  {
    "id": 11089,
    "label": "2021/6/7 星期一 14:33",
    "title": "123",
    "docHistoryId": "60bdbda2f0a8af000664d719",
    "createdAt": "2021-06-07T06:33:13.000Z",
    "updatedAt": "2021-06-07T06:33:13.000Z",
    "user": {
      "id": "user123",
      "name": "testuser"
    }
  },
  {
    "id": 11090,
    "label": "2021/6/7 星期一 14:33",
    "title": "123",
    "docHistoryId": "60bdbdd5f0a8af000664d71d",
    "createdAt": "2021-06-07T06:33:59.000Z",
    "updatedAt": "2021-06-07T06:33:59.000Z",
    "user": {
      "id": "user123",
      "name": "testuser"
    }
  }
]
```

### 获取文件纯文本内容

获取指定文件的纯文本形式内容。

**请求地址**

_GET_ https://shimo-domain/sdk/v2/shimo-files/{fileId}/plain-text

**HTTP Request Parameters**

| 传参方式 | 参数名 | 类型   | 值示例           | 必选 | 说明                                                                                                                                             |
| :------- | :----- | :----- | :--------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId | string | qeK4Xdxvxg8jF5gz | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'php', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request GET 'https://shimo-domain/sdk/v2/shimo-files/{fileId}/plain-text' \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/shimo-files/{fileId}/plain-text?signature=your_signature&token=your_token")
  .method("GET", null)
  .addHeader("X-Shimo-Signature", "your_signature")
  .addHeader("X-Shimo-Token", "your_token")
  .build();
Response response = client.newCall(request).execute();
```

</TabItem>
<TabItem value="go">

```go
package main

import (
  "fmt"
  "net/http"
  "io/ioutil"
)

func main() {
  url := "https://shimo-domain/sdk/v2/shimo-files/%3CfileId%3E/plain-text"
  method := "GET"

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, nil)
  if err != nil {
    fmt.Println(err)
    return
  }

  req.Header.Set("X-Shimo-Signature", "your_signature")
  req.Header.Set("X-Shimo-Token", "your_token")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");

var config = {
  method: "GET",
  url: "https://shimo-domain/sdk/v2/shimo-files/{fileId}/plain-text",
  headers: {
    "X-Shimo-Signature": "your_signature",
    "X-Shimo-Token": "your_token",
  },
};

axios(config)
  .then(function (response) {
    console.log(JSON.stringify(response.data));
  })
  .catch(function (error) {
    console.log(error);
  });
```

</TabItem>
<TabItem value="php">

```php
<?php
require 'vendor/autoload.php';
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Request;
$client = new Client();
$headers = [
  'X-Shimo-Signature' => 'your_signature',
  'X-Shimo-Token' => 'your_token'
];
$request = new Request('GET', 'https://shimo-domain/sdk/v2/shimo-files/{fileId}/plain-text', $headers);
$res = $client->sendAsync($request)->wait();
echo $res->getBody();


```

</TabItem>
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 200    | 返回成功 |

**HTTP Response Body**

| 字段名  | 类型   | 值示例         | 说明                                     |
| :------ | :----- | :------------- | :--------------------------------------- |
| content | string | 文件纯文本内容 | 根据指定文件 ID 获取的石墨文件纯文本内容 |

Example

```json
{
  "content": "文件纯文本内容"
}
```

### 文件纯文本字数统计

获取指定文件的纯文本字数以及给定关键词出现的次数。

**请求地址**

_POST_ https://shimo-domain/sdk/v2/shimo-files/{fileId}/plain-text/wc

**HTTP Request Parameters**

| 传参方式 | 参数名 | 类型   | 值示例           | 必选 | 说明                                                                                                                                             |
| :------- | :----- | :----- | :--------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId | string | qeK4Xdxvxg8jF5gz | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |

**HTTP Request Body**

| 字段名   | 类型     | 值示例         | 必选 | 说明           |
| :------- | :------- | :------------- | :--- | :------------- |
| keywords | []string | ["foo", "bar"] | 否   | 要查询的关键词 |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'php', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request POST 'https://shimo-domain/sdk/v2/shimo-files/{fileId}/plain-text/wc' \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token' \
--data-raw '{
    "keywords": ["foo", "bar"]
}'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\"keywords\":[\"foo\",\"bar\"]}");
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/shimo-files/{fileId}/plain-text/wc")
  .method("POST", body)
  .addHeader("X-Shimo-Signature", "your_signature")
  .addHeader("X-Shimo-Token", "your_token")
  .build();
Response response = client.newCall(request).execute();
```

</TabItem>
<TabItem value="go">

```go
package main

import (
  "fmt"
  "net/http"
  "io/ioutil"
)

func main() {
  url := "https://shimo-domain/sdk/v2/shimo-files/%3CfileId%3E/plain-text/wc"
  method := "POST"

    payload := strings.NewReader(`{
    "keywords": ["foo", "bar"]
}`)

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)
  if err != nil {
    fmt.Println(err)
    return
  }

  req.Header.Set("X-Shimo-Signature", "your_signature")
  req.Header.Set("X-Shimo-Token", "your_token")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");
var data = JSON.stringify({
  keywords: ["foo", "bar"],
});

var config = {
  method: "POST",
  url: "https://shimo-domain/sdk/v2/shimo-files/{fileId}/plain-text/wc",
  headers: {
    "X-Shimo-Signature": "your_signature",
    "X-Shimo-Token": "your_token",
  },
};

axios(config)
  .then(function (response) {
    console.log(JSON.stringify(response.data));
  })
  .catch(function (error) {
    console.log(error);
  });
```

</TabItem>
<TabItem value="php">

```php
<?php
require 'vendor/autoload.php';
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Request;
$client = new Client();
$headers = [
  'X-Shimo-Signature' => 'your_signature',
  'X-Shimo-Token' => 'your_token',
  'Content-Type' => 'application/x-www-form-urlencoded'
];
$options = [
'form_params' => [
  '{
    "keywords": ["foo", "bar"]
}' => ''
]];
$request = new Request('POST', 'https://shimo-domain/sdk/v2/shimo-files/{fileId}/plain-text/wc', $headers);
$res = $client->sendAsync($request, $options)->wait();
echo $res->getBody();


```

</TabItem>
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 200    | 返回成功 |

**HTTP Response Body**

| 字段名    | 类型   | 值示例         | 说明                                         |
| :-------- | :----- | :------------- | :------------------------------------------- |
| wordCount | number | 纯文本字数     | 根据指定文件 ID 获取的石墨文件纯文本字数     |
| keywords  | object | 关键词出现次数 | map[keyword]count，示例： {"foo":1,"bar":10} |

**Example**

```json
{
  "wordCount": 10,
  "keywords": {
    "foo": 1,
    "bar": 2
  }
}
```

### 获取文件内容中所有的 at 人信息列表

获取文件内容中所有的 at 人信息列表。

支持的文件类型：

- `document` 轻文档
- `documentPro` 传统文档
- `spreadsheet` 表格

**请求地址**

_GET_ https://shimo-domain/sdk/v2/shimo-files/{fileId}/mention-at-list

**HTTP Request Parameters**

| 传参方式 | 参数名 | 类型   | 值示例           | 必选 | 说明                                                                                                                                             |
| :------- | :----- | :----- | :--------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId | string | qeK4Xdxvxg8jF5gz | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'php', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl "https://shimo-domain/sdk/v2/shimo-files/ad8ed1afa8172d91/mention-at-list" \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/shimo-files/ad8ed1afa8172d91/mention-at-list")
  .addHeader("X-Shimo-Signature", "your_signature")
  .addHeader("X-Shimo-Token", "your_token")
  .build();
Response response = client.newCall(request).execute();
```

</TabItem>
<TabItem value="go">

```go
package main

import (
  "fmt"
  "strings"
  "net/http"
  "io/ioutil"
)

func main() {
  url := "https://shimo-domain/sdk/v2/shimo-files/ad8ed1afa8172d91/mention-at-list"
  method := "GET"

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, nil)
  if err != nil {
    fmt.Println(err)
    return
  }

  req.Header.Set("X-Shimo-Signature", "your_signature")
  req.Header.Set("X-Shimo-Token", "your_token")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");

axios({
  method: "GET",
  url: "https://shimo-domain/sdk/v2/shimo-files/ad8ed1afa8172d91/mention-at-list",
  header: {
    "X-Shimo-Signature": "your_signature",
    "X-Shimo-Token": "your_token",
  },
})
  .then(function (response) {
    console.log(JSON.stringify(response.data));
  })
  .catch(function (error) {
    console.log(error);
  });
```

</TabItem>
<TabItem value="php">

```php
<?php
require 'vendor/autoload.php';
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Request;
$client = new Client();
$headers = [
  'X-Shimo-Signature' => 'your_signature',
  'X-Shimo-Token' => 'your_token'
];
$request = new Request('GET', 'https://shimo-domain/sdk/v2/shimo-files/ad8ed1afa8172d91/mention-at-list', $headers);
$res = $client->sendAsync($request)->wait();
echo $res->getBody();


```

</TabItem>
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 200    | 返回成功 |

**HTTP Response Body**

| 字段名                  | 类型   | 值示例           | 说明                                               |
| :---------------------- | :----- | :--------------- | :------------------------------------------------- |
| mentionAtList           | Array  | -                | 根据指定文件 ID 获取的石墨文件中的所有 at 信息列表 |
| mentionAtList[0].userId | string | user1            | at 提及的用户 ID                                   |
| mentionAtList[0].atGuid | string | MODOCnb4HPu04G8f | at 提及在文件中对应的位置标记                      |

Example

```json
{
  "mentionAtList": [
    {
      "userId": "1",
      "atGuid": "MODOCnb4HPu04G8f"
    },
    {
      "userId": "4",
      "atGuid": "MODOCPryKBQpdLVS"
    },
    {
      "userId": "3",
      "atGuid": "MODOC6IQPpxpi5RV"
    },
    {
      "userId": "1", // 同个用户在不同位置被 at
      "atGuid": "MODOCSxgFvWiIupK"
    }
  ]
}
```

### 获取文件中的评论数 {#get-comment-count}

获取文件内中的评论总数。

支持文件类型：

- `spreadsheet` 表格

**请求地址**

_GET_ https://shimo-domain/sdk/v2/shimo-files/{fileId}/comment-count

**HTTP Request Parameters**

| 传参方式 | 参数名 | 类型   | 值示例           | 必选 | 说明                                                                                                                                             |
| :------- | :----- | :----- | :--------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId | string | qeK4Xdxvxg8jF5gz | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'php', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl "https://shimo-domain/sdk/v2/shimo-files/fe143ca1a08e9976/comment-count" \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/shimo-files/fe143ca1a08e9976/comment-count")
  .addHeader("X-Shimo-Signature", "your_signature")
  .addHeader("X-Shimo-Token", "your_token")
  .build();
Response response = client.newCall(request).execute();
```

</TabItem>
<TabItem value="go">

```go
package main

import (
  "fmt"
  "strings"
  "net/http"
  "io/ioutil"
)

func main() {
  url := "https://shimo-domain/sdk/v2/shimo-files/fe143ca1a08e9976/comment-count"
  method := "GET"

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, nil)
  if err != nil {
    fmt.Println(err)
    return
  }

  req.Header.Set("Content-Type", "application/json")
  req.Header.Set("X-Shimo-Signature", "your_signature")
  req.Header.Set("X-Shimo-Token", "your_token")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");
var config = {
  method: "GET",
  url: "https://shimo-domain/sdk/v2/shimo-files/fe143ca1a08e9976/comment-count",
  headers: {
    "X-Shimo-Signature": "your_signature",
    "X-Shimo-Token": "your_token",
  },
  data: data,
};

axios(config)
  .then(function (response) {
    console.log(JSON.stringify(response.data));
  })
  .catch(function (error) {
    console.log(error);
  });
```

</TabItem>
<TabItem value="php">

```php
<?php
require 'vendor/autoload.php';
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Request;
$client = new Client();
$headers = [
  'X-Shimo-Signature' => 'your_signature',
  'X-Shimo-Token' => 'your_token'
];
$request = new Request('GET', 'https://shimo-domain/sdk/v2/shimo-files/fe143ca1a08e9976/comment-count', $headers);
$res = $client->sendAsync($request)->wait();
echo $res->getBody();


```

</TabItem>
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 200    | 返回成功 |

**HTTP Response Body**

| 字段名 | 类型   | 值示例 | 说明     |
| :----- | :----- | :----- | :------- |
| count  | number | 2      | 评论条数 |

Example

```json
{
  "count": 2
}
```

## 表格

### 表格参数说明

#### range

格式为 `{工作表}!{单元格范围}`，规范如下：

- 单元格范围的格式为 `A1:C10` or `A1`，单元格起始位置不能超过表格最大的行列
- 如果工作表名称中含有 `!:'` 这 3 种特殊字符，必须以单引号包裹工作表名称。示例:
  - `工作表 1`
  - `工作表 1!A1`
  - `工作表 1!A1:C1`
  - `'工作!:表'!A2:C3`

#### resource

类型为 `object`，里面必须包含 key 值 `"values"`。
`values` 为一个二维数组，表示追加/更新的表格内容，第一维表示行，第二维表示列。空字符串表示当前单元格为空。
示例：

```json title="返回一行数据"
{
  "resource": {
    "values": [[1, "", "a"]]
  }
}
```

```json title="返回多行数据"
{
  "resource": {
    "values": [
      [1, "", "a"],
      ["b", "c", 2]
    ]
  }
}
```

#### 日期类型 OADate 数值转换 {#oadate-conversion-example}

日期类型返回值为 OADate 类型数值，如单元格中日期为 `2022/7/27` ，返回的值为 `44769`。

参考以下 JavaScript 代码进行转换：

```javascript title="convertOADateToLocaleDateString.js"
function _getTimezoneOffset(date) {
  let offset = date.getTimezoneOffset();
  if (offset === -485) {
    offset = -485 - 43 / 60;
  }
  return offset;
}

function _fromOADate(oadate) {
  const offsetDay = oadate - 25569;
  const date = new Date(offsetDay * 86400000);

  const adjustValue = offsetDay >= 0 ? 1 : -1;
  const oldDateTimezoneOffset = _getTimezoneOffset(date);
  const ms =
    (oadate * 86400000 * 1440 +
      adjustValue -
      25569 * 86400000 * 1440 +
      oldDateTimezoneOffset * 86400000) /
    1440;
  let firstResult = new Date(ms);

  const fixHourSign = oldDateTimezoneOffset >= 0 ? 1 : -1;
  const nextHour = new Date(ms + fixHourSign * 3600000);
  const nextHourTimezoneOffset = _getTimezoneOffset(nextHour);
  if (oldDateTimezoneOffset !== nextHourTimezoneOffset) {
    let newResult = new Date(
      ms + (nextHourTimezoneOffset - oldDateTimezoneOffset) * 60 * 1000
    );
    if (oldDateTimezoneOffset > nextHourTimezoneOffset) {
      if (
        fixHourSign === -1 ||
        nextHourTimezoneOffset === _getTimezoneOffset(firstResult)
      ) {
        newResult =
          newResult.getMilliseconds() === 999
            ? new Date(newResult.valueOf() + 1)
            : newResult;
        return newResult;
      }
    } else if (oldDateTimezoneOffset < nextHourTimezoneOffset) {
      if (
        fixHourSign === 1 ||
        nextHourTimezoneOffset === _getTimezoneOffset(firstResult)
      ) {
        newResult =
          newResult.getMilliseconds() === 999
            ? new Date(newResult.valueOf() + 1)
            : newResult;
        return newResult;
      }
    }
  }

  firstResult =
    firstResult.getMilliseconds() === 999
      ? new Date(firstResult.valueOf() + 1)
      : firstResult;
  return firstResult;
}

function convertOADate(OAdate) {
  return _fromOADate(OAdate).toLocaleDateString();
}

convertOADate(44769);
// '2022/7/27'
```

### 获取表格内容{#get-table-content}

根据传入的表格范围获取表格内容。

支持文件类型：

- `spreadsheet` 表格

**请求地址**

_GET_ https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values

**HTTP Request Parameters**

| 传参方式 | 参数名 | 类型   | 值示例           | 必选 | 说明                                                                                                                                             |
| :------- | :----- | :----- | :--------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId | string | qeK4Xdxvxg8jF5gz | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |
| Query    | range  | string | 工作表 1!A1:C3   | 是   | 请参考 [range](#range) 参数说明。                                                                                                                |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'php', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request GET 'https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values?range=工作表 1!A1:C3' \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values?range=工作表 1!A1:C3")
  .method("GET", null)
  .addHeader("X-Shimo-Signature", "your_signature")
  .addHeader("X-Shimo-Token", "your_token")
  .build();
Response response = client.newCall(request).execute();
```

</TabItem>
<TabItem value="go">

```go
package main

import (
  "fmt"
  "net/http"
  "io/ioutil"
)

func main() {
  url := "https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values?range=%E5%B7%A5%E4%BD%9C%E8%A1%A8%201!A1:C3"
  method := "GET"

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, nil)
  if err != nil {
    fmt.Println(err)
    return
  }

  req.Header.Set("X-Shimo-Signature", "your_signature")
  req.Header.Set("X-Shimo-Token", "your_token")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");

var config = {
  method: "GET",
  url: "https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values?range=工作表 1!A1:C3",
  headers: {
    "X-Shimo-Signature": "your_signature",
    "X-Shimo-Token": "your_token",
  },
};

axios(config)
  .then(function (response) {
    console.log(JSON.stringify(response.data));
  })
  .catch(function (error) {
    console.log(error);
  });
```

</TabItem>
<TabItem value="php">

```php
<?php
require 'vendor/autoload.php';
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Request;
$client = new Client();
$headers = [
  'X-Shimo-Signature' => 'your_signature',
  'X-Shimo-Token' => 'your_token'
];
$request = new Request('GET', 'https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values?range=工作表 1!A1:C3', $headers);
$res = $client->sendAsync($request)->wait();
echo $res->getBody();


```

</TabItem>
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 200    | 返回成功 |

**HTTP Response Body**

| 字段名 | 类型  | 值示例                                    | 说明         |
| :----- | :---- | :---------------------------------------- | :----------- |
| values | array | [["姓名","年龄","性别"],["小明",29,"男"]] | 表格单元格值 |

Example

```json
{
  "values": [
    ["姓名", "年龄", "性别"],
    ["小明", 29, "男"],
    ["小强", 18, "男"],
    ["小兰", 20, "女"],
    [null, null, "a"]
  ]
}
```

### 更新表格内容

根据传入范围更新表格内容。

支持文件类型：

- `spreadsheet` 表格

**请求地址**

_POST_ https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values

**HTTP Request Parameters**

| 传参方式 | 参数名 | 类型   | 值示例           | 必选 | 说明                                                                                                                                             |
| :------- | :----- | :----- | :--------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId | string | qeK4Xdxvxg8jF5gz | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |

**HTTP Request Body**

| 字段名   | 类型   | 值示例         | 说明                                    |
| :------- | :----- | :------------- | :-------------------------------------- |
| range    | string | 工作表 1!A1:C3 | 请参考 [range](#range) 参数说明。       |
| resource | object | -              | 请参考 [resource](#resource) 参数说明。 |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'php', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request POST 'https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values' \
--header 'Content-Type: application/json' \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token' \
--data-raw '{
    "range": "工作表 1!A1:C3",
    "resource": {
        "values": [
            ["第一行第一列的值","第一行第二列的值"],
            ["第二行第一列的值","第二行第二列的值"]
        ]
    }
}'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\n    \"range\": \"工作表 1!A1:C3\",\n    \"resource\": {\n        \"values\": [\n            [\"第一行第一列的值\",\"第一行第二列的值\"],\n            [\"第二行第一列的值\",\"第二行第二列的值\"]\n        ]\n    }\n}");
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values")
  .method("POST", body)
  .addHeader("Content-Type", "application/json")
  .addHeader("X-Shimo-Signature", "your_signature")
  .addHeader("X-Shimo-Token", "your_token")
  .build();
Response response = client.newCall(request).execute();
```

</TabItem>
<TabItem value="go">

```go
package main

import (
  "fmt"
  "strings"
  "net/http"
  "io/ioutil"
)

func main() {
  url := "https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values"
  method := "POST"

  payload := strings.NewReader(`{
    "range": "工作表 1!A1:C3",
    "resource": {
        "values": [
            ["第一行第一列的值","第一行第二列的值"],
            ["第二行第一列的值","第二行第二列的值"]
        ]
    }
}`)

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
    return
  }
  req.Header.Set("Content-Type", "application/json")
  req.Header.Set("X-Shimo-Signature", "your_signature")
  req.Header.Set("X-Shimo-Token", "your_token")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");
var data = JSON.stringify({
  range: "工作表 1!A1:C3",
  resource: {
    values: [
      ["第一行第一列的值", "第一行第二列的值"],
      ["第二行第一列的值", "第二行第二列的值"],
    ],
  },
});

var config = {
  method: "post",
  url: "https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values",
  headers: {
    "Content-Type": "application/json",
    "X-Shimo-Signature": "your_signature",
    "X-Shimo-Token": "your_token",
  },
  data: data,
};

axios(config)
  .then(function (response) {
    console.log(JSON.stringify(response.data));
  })
  .catch(function (error) {
    console.log(error);
  });
```

</TabItem>
<TabItem value="php">

```php
<?php
require 'vendor/autoload.php';
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Request;
$client = new Client();
$headers = [
  'Content-Type' => 'application/json',
  'X-Shimo-Signature' => 'your_signature',
  'X-Shimo-Token' => 'your_token'
];
$body = '{
  "range": "工作表 1!A1:C3",
  "resource": {
    "values": [
      [
        "第一行第一列的值",
        "第一行第二列的值"
      ],
      [
        "第二行第一列的值",
        "第二行第二列的值"
      ]
    ]
  }
}';
$request = new Request('POST', 'https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values', $headers, $body);
$res = $client->sendAsync($request)->wait();
echo $res->getBody();


```

</TabItem>
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 204    | 返回成功 |

**HTTP Response Body**

空

### 追加表格内容

根据传入的表格范围追加表格内容（只支持纯文本和数字）。

支持文件类型：

- `spreadsheet` 表格

根据 range 查找追加位置的逻辑是定位到需要追加的行位置：
a. 不指定单元格范围，例如工作表 1：会遍历整个表格，找到最后一个有数据的行
b. 指定单元格范围，例如 工作表 1!C5:E8：会找到起始单元格 C5 所在的行，从该行向下遍历，找到最后一个连续的有数据行向下插入 N 行，填充 values，其中 N = values.length
如果修改了不存在的列，会自动新增到该列。

**请求地址**

_PUT_ https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values

**HTTP Request Parameters**

| 传参方式 | 参数名 | 类型   | 值示例           | 必选 | 说明                                                                                                                                             |
| :------- | :----- | :----- | :--------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId | string | qeK4Xdxvxg8jF5gz | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |

**HTTP Request Body**

| 字段名   | 类型   | 值示例         | 说明                                    |
| :------- | :----- | :------------- | :-------------------------------------- |
| range    | string | 工作表 1!A1:C3 | 请参考 [range](#range) 参数说明。       |
| resource | object | -              | 请参考 [resource](#resource) 参数说明。 |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'php', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request PUT 'https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values' \
--header 'Content-Type: application/json' \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token' \
--data-raw '{
    "range": "工作表 1!A1:C3",
    "resource": {
        "values": [
            ["第一行第一列追加文本","第一行第二列追加文本"],
            ["第二行第一列追加文本","第二行第二列追加文本"]
        ]
    }
}'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\n    \"range\": \"工作表 1!A1:C3\",\n    \"resource\": {\n        \"values\": [\n            [\"第一行第一列追加文本\",\"第一行第二列追加文本\"],\n            [\"第二行第一列追加文本\",\"第二行第二列追加文本\"]\n        ]\n    }\n}");
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values")
  .method("PUT", body)
  .addHeader("Content-Type", "application/json")
  .addHeader("X-Shimo-Signature", "your_signature")
  .addHeader("X-Shimo-Token", "your_token")
  .build();
Response response = client.newCall(request).execute();
```

</TabItem>
<TabItem value="go">

```go
package main

import (
  "fmt"
  "strings"
  "net/http"
  "io/ioutil"
)

func main() {
  url := "https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values"
  method := "PUT"

  payload := strings.NewReader(`{
    "range": "工作表 1!A1:C3",
    "resource": {
        "values": [
            ["第一行第一列追加文本","第一行第二列追加文本"],
            ["第二行第一列追加文本","第二行第二列追加文本"]
        ]
    }
}`)

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)
  if err != nil {
    fmt.Println(err)
    return
  }

  req.Header.Set("Content-Type", "application/json")
  req.Header.Set("X-Shimo-Signature", "your_signature")
  req.Header.Set("X-Shimo-Token", "your_token")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");
var data = JSON.stringify({
  range: "工作表 1!A1:C3",
  resource: {
    values: [
      ["第一行第一列追加文本", "第一行第二列追加文本"],
      ["第二行第一列追加文本", "第二行第二列追加文本"],
    ],
  },
});

var config = {
  method: "put",
  url: "https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values",
  headers: {
    "Content-Type": "application/json",
    "X-Shimo-Signature": "your_signature",
    "X-Shimo-Token": "your_token",
  },
  data: data,
};

axios(config)
  .then(function (response) {
    console.log(JSON.stringify(response.data));
  })
  .catch(function (error) {
    console.log(error);
  });
```

</TabItem>
<TabItem value="php">

```php
<?php
require 'vendor/autoload.php';
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Request;
$client = new Client();
$headers = [
  'Content-Type' => 'application/json',
  'X-Shimo-Signature' => 'your_signature',
  'X-Shimo-Token' => 'your_token'
];
$body = '{
  "range": "工作表 1!A1:C3",
  "resource": {
    "values": [
      [
        "第一行第一列追加文本",
        "第一行第二列追加文本"
      ],
      [
        "第二行第一列追加文本",
        "第二行第二列追加文本"
      ]
    ]
  }
}';
$request = new Request('PUT', 'https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values', $headers, $body);
$res = $client->sendAsync($request)->wait();
echo $res->getBody();


```

</TabItem>
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 204    | 返回成功 |

**HTTP Response Body**

空

### 删除表格行

删除表格中的一行和多行。从 index 开始，删除数量为 count 的行数。

支持文件类型：

- `spreadsheet` 表格

**请求地址**

_DELETE_ https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/{sheetName}/rows/{index}

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例           | 必选 | 说明                                                                                                                                             |
| :------- | :-------- | :----- | :--------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |
| Path     | sheetName | string | 工作表 1         | 是   | 表格中工作表的名称                                                                                                                               |
| Path     | index     | number | 0                | 是   | 从第几行开始删除                                                                                                                                 |
| Query    | count     | number | 1                | 否   | 删除几行，默认为 1                                                                                                                               |

**HTTP Request Body**

空

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'php', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request DELETE 'https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/工作表 1/rows/0?count=2' \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("text/plain");
RequestBody body = RequestBody.create(mediaType, "");
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/工作表 1/rows/0?count=2")
  .method("DELETE", body)
  .addHeader("X-Shimo-Signature", "your_signature")
  .addHeader("X-Shimo-Token", "your_token")
  .build();
Response response = client.newCall(request).execute();
```

</TabItem>
<TabItem value="go">

```go
package main

import (
  "fmt"
  "net/http"
  "io/ioutil"
)

func main() {
  url := "https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/工作表 1/rows/0?count=2"
  method := "DELETE"

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, nil)
  if err != nil {
    fmt.Println(err)
    return
  }

  req.Header.Set("X-Shimo-Signature", "your_signature")
  req.Header.Set("X-Shimo-Token", "your_token")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");

var config = {
  method: "delete",
  url: "https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/工作表 1/rows/0?count=2",
  headers: {
    "X-Shimo-Signature": "your_signature",
    "X-Shimo-Token": "your_token",
  },
};

axios(config)
  .then(function (response) {
    console.log(JSON.stringify(response.data));
  })
  .catch(function (error) {
    console.log(error);
  });
```

</TabItem>
<TabItem value="php">

```php
<?php
require 'vendor/autoload.php';
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Request;
$client = new Client();
$headers = [
  'X-Shimo-Signature' => 'your_signature',
  'X-Shimo-Token' => 'your_token'
];
$request = new Request('DELETE', 'https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/工作表 1/rows/0?count=2', $headers);
$res = $client->sendAsync($request)->wait();
echo $res->getBody();



```

</TabItem>
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 204    | 返回成功 |

**HTTP Response Body**

空

### 新增表格工作表

在表格中新增工作表。

支持文件类型：

- `spreadsheet` 表格

**请求地址**

_POST_ https://shimo-domain/sdk/v2/api/files/{fileId}/sheets

**HTTP Request Parameters**

| 传参方式 | 参数名 | 类型   | 值示例           | 必选 | 说明                                                                                                                                             |
| :------- | :----- | :----- | :--------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId | string | qeK4Xdxvxg8jF5gz | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |

**HTTP Request Body**

| 字段名 | 类型   | 值示例   | 说明               |
| :----- | :----- | :------- | :----------------- |
| name   | string | 工作表 2 | 新增表格工作表名称 |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'php', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request POST 'https://shimo-domain/sdk/v2/api/files/{fileId}/sheets' \
--header 'Content-Type: application/json' \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token' \
--data-raw '{"name": "工作表2"}'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\"name\": \"工作表2\"}");
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/files/{fileId}/sheets")
  .method("POST", body)
  .addHeader("Content-Type", "application/json")
  .addHeader("X-Shimo-Signature", "your_signature")
  .addHeader("X-Shimo-Token", "your_token")
  .build();
Response response = client.newCall(request).execute();
```

</TabItem>
<TabItem value="go">

```go
package main

import (
  "fmt"
  "strings"
  "net/http"
  "io/ioutil"
)

func main() {
  url := "https://shimo-domain/sdk/v2/api/files/%3CfileId%3E/sheets"
  method := "POST"

  payload := strings.NewReader(`{"name": "工作表2"}`)

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
    return
  }
  req.Header.Set("Content-Type", "application/json")
  req.Header.Set("X-Shimo-Signature", "your_signature")
  req.Header.Set("X-Shimo-Token", "your_token")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");
var data = JSON.stringify({
  name: "工作表2",
});

var config = {
  method: "post",
  url: "https://shimo-domain/sdk/v2/api/files/{fileId}/sheets",
  headers: {
    "Content-Type": "application/json",
    "X-Shimo-Signature": "your_signature",
    "X-Shimo-Token": "your_token",
  },
  data: data,
};

axios(config)
  .then(function (response) {
    console.log(JSON.stringify(response.data));
  })
  .catch(function (error) {
    console.log(error);
  });
```

</TabItem>
<TabItem value="php">

```php
<?php
require 'vendor/autoload.php';
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Request;
$client = new Client();
$headers = [
  'Content-Type' => 'application/json',
  'X-Shimo-Signature' => 'your_signature',
  'X-Shimo-Token' => 'your_token'
];
$body = '{
  "name": "工作表2"
}';
$request = new Request('POST', 'https://shimo-domain/sdk/v2/api/files/{fileId}/sheets', $headers, $body);
$res = $client->sendAsync($request)->wait();
echo $res->getBody();



```

</TabItem>
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 204    | 返回成功 |

**HTTP Response Body**

空

### 设置单元格链接

PUT /sdk/v2/api/files/{fileId}/sheets/{sheetName}/cells/{cell}/link

创建/修改单元格链接

> Body 请求参数

```json
{
  "text": "foo.pdf",
  "link": "https://bar.com/f/foo.pdf",
  "type": "attach"
}
```

**请求参数**

| 名称      | 位置 | 类型        | 必选 | 说明                                                                   |
| --------- | ---- | ----------- | ---- | ---------------------------------------------------------------------- |
| fileId    | path | string      | 是   | 接入方的文件 ID                                                        |
| sheetName | path | string      | 是   | Sheet 名称                                                             |
| cell      | path | string      | 是   | 单元格位置                                                             |
| body      | body | object      | 否   | none                                                                   |
| text      | body | string      | 是   | 链接显示的文本                                                         |
| link      | body | string      | 是   | 链接。如果需要设置附件链接，请先按照上传文件流程上传后获取附件链接     |
| type      | body | string¦null | 是   | 链接类型，不指定时为普通链接。指定时可选值为 "attach" ，代表附件链接。 |

**返回结果**

| 状态码 | 状态码含义                                                      | 说明 | 数据模型 |
| ------ | --------------------------------------------------------------- | ---- | -------- |
| 204    | [No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5) | 成功 | Inline   |

**返回数据结构**

无

### 设置单元格格式

PUT /sdk/v2/api/files/{fileId}/sheets/{sheetName}/cells/{cell}

#### **类型定义**

```typescript
/**
  * 本接口的 Body 为 JSON 格式，JSON 类型需要按照以下 TS 格式传递。
  **/
interface CellAttr {
  // 单元格图片属性，需要设置 3 个属性
  backgroundImage?: string
  backgroundImageLayout?: 4
  backgorundImageSize?: JSON.stringify([width, height])
  // 单元格颜色
  background?: string
  // 加粗
  bold?: boolean
  // 斜体
  italic?: boolean
  // 中划线
  strike?: boolean
  // 下划线
  underline?: boolean
  // 字体颜色
  color?: string
  // 格式
  format?: string
  // 单元格内容，内容也是单元格的一个属性之一
  cellValue?: string | number | boolean
  // 单元格富文本内容
  richText?: JSON.stringify(RichTextItem[])
  // 字体大小
  'font-size'?: number
  // 字体
  'font-family'?: string
  // 合并单元格行数
  rowspan?: number
  // 合并单元格列数
  colspan?: number
  // 上边框
  'border-top'?: JSON.stringify([color, BorderType])
  // 右边框
  'border-right'?: JSON.stringify([color, BorderType])
  // 下边框
  'border-bottom'?: JSON.stringify([color, BorderType])
  // 左边框
  'border-left'?: JSON.stringify([color, BorderType])
  // 斜下边框
  'diagonal-down'?: JSON.stringify([color, BorderType])
  // 斜上边框
  'diagonal-up'?: JSON.stringify([color, BorderType])

}

enum BorderType {
  /**
   *  Indicates a border line with a solid thin line.
   */
  thin = 1,
  /**
   *  Indicates a medium border line with a solid line.
   */
  medium = 2,
  /**
   *  Indicates a border line with dots.
   */
  dotted = 4,
  /**
   *  Indicates a border line with all dots.
   */
  hair = 7,
}
interface RichLink {
  type: 'link'
  data: {
    link: string
    icon?: 'attach' | 'cloud'
  }
}

interface RichTextItem {
  data: number // 偏移量
  attributes?: {
    bold?: true
    italic?: true
    strike?: true
    underline?: true
    'font-size'?: number
    'font-family'?: string
    color?: string
    extension?: RichLink
  }
}

```

以下对 `format` 和 `richText` 属性进行详细说明。

**富文本内容：richText**

如果单元格内容包含富文本，写入单元格内容到时候就需要用到 richText 属性。

richText 属性的使用需要结合 cellValue 属性一起使用。单元格富文本的本质是将单元格的内容（纯文本字符串）拆分成多段来渲染展示，我们可以分别针对每一段的文本设置 BIUS、字体大小、字体、字体颜色、富文本链接等等。

举例说明：

![富文本内容：richText](./../../image/edit-1.png)

从上图可以看出，这个单元格的内容为“abcdefg”，其中包含了 3 段富文本。第 1 段的长度是 3 个字符，样式包含加粗、斜体、下划线、中划线、字号 24、红色字体颜色；第 2 段长度 2 个字符，没有任何样式；第 3 段长度为 2 个字符，这两个字符包含了一个链接，链接对应的 url 为：http://aaa.cc。

所以，这个单元格的富文本可以表示为：

```typescript
const richTexts = [
  {
    // 表示偏移量，也就是这一小段富文本的长度
    data: 3,
    // 这一段富文本的样式(如果有的话)
    attributes: {
      bold: true,
      italic: true,
      "font-size": 24,
      "font-family": "STKaiti",
      color: "#ff0000",
      underline: true,
      strike: true,
    },
  },
  {
    data: 2,
    // 如果没有额外的样式，就不需要 attributes 属性
  },
  {
    data: 2,
    attributes: {
      // 富文本链接
      extension: {
        type: "link",
        data: {
          link: "http://aaa.cc",
        },
      },
    },
  },
];
```

将这个单元格的内容及其富文本样式写入单元格的接口调用的 body 部分为：

```typescript
{
  cellValue: 'abcdefg',
  richText: JSON.stringify(richTexts)
}
```

**单元格格式：format**

这里说的格式指的是给单元格设置成文本、数值、日期、百分比等等的格式。包含下面这些分类：

![edit-2](./../../image/edit-2.png)

下面就每种分类支持的格式类型的取值做详细说明。

常规

```typescript
["General", "", "normal"];
```

文本

```typescript
["@"];
```

数值

```typescript
[
  "#,##0.0_);[Red](#,##0.0)",
  "#,##0.0_);(#,##0.0)",
  "#,##0.0;[Red]#,##0.0",
  "#,##0.0_ ",
  "#,##0.0_ ;[Red]-#,##0.0 ",
  "#,##0_);[Red](#,##0)",
  "#,##0_);(#,##0)",
  "#,##0;[Red]#,##0",
  "#,##0_ ",
  "#,##0_ ;[Red]-#,##0 ",
  "0.0_);[Red](0.0)",
  "0.0_);(0.0)",
  "0.0;[Red]0.0",
  "0.0_ ",
  "0.0_ ;[Red]-0.0 ",
  "0_);[Red](0)",
  "0_);(0)",
  "0;[Red]0",
  "0_ ",
  "0_ ;[Red]-0 ",
];
```

百分比

```typescript
["0%", "0.0%"];
```

货币

```typescript
[
  '""#,##0.0_);[Red](""#,##0.0)',
  '"¥"#,##0.0_);[Red]("¥"#,##0.0)',
  '"$"#,##0.0_);[Red]("$"#,##0.0)',
  '"€"#,##0.0_);[Red]("€"#,##0.0)',
  '"£"#,##0.0_);[Red]("£"#,##0.0)',
  '"₩"#,##0.0_);[Red]("₩"#,##0.0)',
  '"NT$"#,##0.0_);[Red]("NT$"#,##0.0)',
  '"Rp"#,##0.0_);[Red]("Rp"#,##0.0)',
  '"IDR "#,##0.0_);[Red]("IDR "#,##0.0)',
  '"JPY "#,##0.0_);[Red]("JPY "#,##0.0)',
  '"KRW "#,##0.0_);[Red]("KRW "#,##0.0)',
  '"KPW "#,##0.0_);[Red]("KPW "#,##0.0)',
  '"NTD "#,##0.0_);[Red]("NTD "#,##0.0)',
  '"RUB "#,##0.0_);[Red]("RUB "#,##0.0)',
  '"SAR "#,##0.0_);[Red]("SAR "#,##0.0)',
  '"TWD "#,##0.0_);[Red]("TWD "#,##0.0)',
  '""#,##0.0_);(""#,##0.0)',
  '"¥"#,##0.0_);("¥"#,##0.0)',
  '"$"#,##0.0_);("$"#,##0.0)',
  '"€"#,##0.0_);("€"#,##0.0)',
  '"£"#,##0.0_);("£"#,##0.0)',
  '"₩"#,##0.0_);("₩"#,##0.0)',
  '"NT$"#,##0.0_);("NT$"#,##0.0)',
  '"Rp"#,##0.0_);("Rp"#,##0.0)',
  '"IDR "#,##0.0_);("IDR "#,##0.0)',
  '"JPY "#,##0.0_);("JPY "#,##0.0)',
  '"KRW "#,##0.0_);("KRW "#,##0.0)',
  '"KPW "#,##0.0_);("KPW "#,##0.0)',
  '"NTD "#,##0.0_);("NTD "#,##0.0)',
  '"RUB "#,##0.0_);("RUB "#,##0.0)',
  '"SAR "#,##0.0_);("SAR "#,##0.0)',
  '"TWD "#,##0.0_);("TWD "#,##0.0)',
  '""#,##0.0;[Red]""#,##0.0',
  '"¥"#,##0.0;[Red]"¥"#,##0.0',
  '"$"#,##0.0;[Red]"$"#,##0.0',
  '"€"#,##0.0;[Red]"€"#,##0.0',
  '"£"#,##0.0;[Red]"£"#,##0.0',
  '"₩"#,##0.0;[Red]"₩"#,##0.0',
  '"NT$"#,##0.0;[Red]"NT$"#,##0.0',
  '"Rp"#,##0.0;[Red]"Rp"#,##0.0',
  '"IDR "#,##0.0;[Red]"IDR "#,##0.0',
  '"JPY "#,##0.0;[Red]"JPY "#,##0.0',
  '"KRW "#,##0.0;[Red]"KRW "#,##0.0',
  '"KPW "#,##0.0;[Red]"KPW "#,##0.0',
  '"NTD "#,##0.0;[Red]"NTD "#,##0.0',
  '"RUB "#,##0.0;[Red]"RUB "#,##0.0',
  '"SAR "#,##0.0;[Red]"SAR "#,##0.0',
  '"TWD "#,##0.0;[Red]"TWD "#,##0.0',
  '""#,##0.0',
  '"¥"#,##0.0',
  '"$"#,##0.0',
  '"€"#,##0.0',
  '"£"#,##0.0',
  '"₩"#,##0.0',
  '"NT$"#,##0.0',
  '"Rp"#,##0.0',
  '"IDR "#,##0.0',
  '"JPY "#,##0.0',
  '"KRW "#,##0.0',
  '"KPW "#,##0.0',
  '"NTD "#,##0.0',
  '"RUB "#,##0.0',
  '"SAR "#,##0.0',
  '"TWD "#,##0.0',
  '""#,##0.0;[Red]-""#,##0.0',
  '"¥"#,##0.0;[Red]-"¥"#,##0.0',
  '"$"#,##0.0;[Red]-"$"#,##0.0',
  '"€"#,##0.0;[Red]-"€"#,##0.0',
  '"£"#,##0.0;[Red]-"£"#,##0.0',
  '"₩"#,##0.0;[Red]-"₩"#,##0.0',
  '"NT$"#,##0.0;[Red]-"NT$"#,##0.0',
  '"Rp"#,##0.0;[Red]-"Rp"#,##0.0',
  '"IDR "#,##0.0;[Red]-"IDR "#,##0.0',
  '"JPY "#,##0.0;[Red]-"JPY "#,##0.0',
  '"KRW "#,##0.0;[Red]-"KRW "#,##0.0',
  '"KPW "#,##0.0;[Red]-"KPW "#,##0.0',
  '"NTD "#,##0.0;[Red]-"NTD "#,##0.0',
  '"RUB "#,##0.0;[Red]-"RUB "#,##0.0',
  '"SAR "#,##0.0;[Red]-"SAR "#,##0.0',
  '"TWD "#,##0.0;[Red]-"TWD "#,##0.0',
  '""#,##0_);[Red](""#,##0)',
  '"¥"#,##0_);[Red]("¥"#,##0)',
  '"$"#,##0_);[Red]("$"#,##0)',
  '"€"#,##0_);[Red]("€"#,##0)',
  '"£"#,##0_);[Red]("£"#,##0)',
  '"₩"#,##0_);[Red]("₩"#,##0)',
  '"NT$"#,##0_);[Red]("NT$"#,##0)',
  '"Rp"#,##0_);[Red]("Rp"#,##0)',
  '"IDR "#,##0_);[Red]("IDR "#,##0)',
  '"JPY "#,##0_);[Red]("JPY "#,##0)',
  '"KRW "#,##0_);[Red]("KRW "#,##0)',
  '"KPW "#,##0_);[Red]("KPW "#,##0)',
  '"NTD "#,##0_);[Red]("NTD "#,##0)',
  '"RUB "#,##0_);[Red]("RUB "#,##0)',
  '"SAR "#,##0_);[Red]("SAR "#,##0)',
  '"TWD "#,##0_);[Red]("TWD "#,##0)',
  '""#,##0_);(""#,##0)',
  '"¥"#,##0_);("¥"#,##0)',
  '"$"#,##0_);("$"#,##0)',
  '"€"#,##0_);("€"#,##0)',
  '"£"#,##0_);("£"#,##0)',
  '"₩"#,##0_);("₩"#,##0)',
  '"NT$"#,##0_);("NT$"#,##0)',
  '"Rp"#,##0_);("Rp"#,##0)',
  '"IDR "#,##0_);("IDR "#,##0)',
  '"JPY "#,##0_);("JPY "#,##0)',
  '"KRW "#,##0_);("KRW "#,##0)',
  '"KPW "#,##0_);("KPW "#,##0)',
  '"NTD "#,##0_);("NTD "#,##0)',
  '"RUB "#,##0_);("RUB "#,##0)',
  '"SAR "#,##0_);("SAR "#,##0)',
  '"TWD "#,##0_);("TWD "#,##0)',
  '""#,##0;[Red]""#,##0',
  '"¥"#,##0;[Red]"¥"#,##0',
  '"$"#,##0;[Red]"$"#,##0',
  '"€"#,##0;[Red]"€"#,##0',
  '"£"#,##0;[Red]"£"#,##0',
  '"₩"#,##0;[Red]"₩"#,##0',
  '"NT$"#,##0;[Red]"NT$"#,##0',
  '"Rp"#,##0;[Red]"Rp"#,##0',
  '"IDR "#,##0;[Red]"IDR "#,##0',
  '"JPY "#,##0;[Red]"JPY "#,##0',
  '"KRW "#,##0;[Red]"KRW "#,##0',
  '"KPW "#,##0;[Red]"KPW "#,##0',
  '"NTD "#,##0;[Red]"NTD "#,##0',
  '"RUB "#,##0;[Red]"RUB "#,##0',
  '"SAR "#,##0;[Red]"SAR "#,##0',
  '"TWD "#,##0;[Red]"TWD "#,##0',
  '""#,##0',
  '"¥"#,##0',
  '"$"#,##0',
  '"€"#,##0',
  '"£"#,##0',
  '"₩"#,##0',
  '"NT$"#,##0',
  '"Rp"#,##0',
  '"IDR "#,##0',
  '"JPY "#,##0',
  '"KRW "#,##0',
  '"KPW "#,##0',
  '"NTD "#,##0',
  '"RUB "#,##0',
  '"SAR "#,##0',
  '"TWD "#,##0',
  '""#,##0;[Red]-""#,##0',
  '"¥"#,##0;[Red]-"¥"#,##0',
  '"$"#,##0;[Red]-"$"#,##0',
  '"€"#,##0;[Red]-"€"#,##0',
  '"£"#,##0;[Red]-"£"#,##0',
  '"₩"#,##0;[Red]-"₩"#,##0',
  '"NT$"#,##0;[Red]-"NT$"#,##0',
  '"Rp"#,##0;[Red]-"Rp"#,##0',
  '"IDR "#,##0;[Red]-"IDR "#,##0',
  '"JPY "#,##0;[Red]-"JPY "#,##0',
  '"KRW "#,##0;[Red]-"KRW "#,##0',
  '"KPW "#,##0;[Red]-"KPW "#,##0',
  '"NTD "#,##0;[Red]-"NTD "#,##0',
  '"RUB "#,##0;[Red]-"RUB "#,##0',
  '"SAR "#,##0;[Red]-"SAR "#,##0',
  '"TWD "#,##0;[Red]-"TWD "#,##0',
  '#,##0.0"₽"_);[Red](#,##0.0"₽")',
  '#,##0.0"₽"_);(#,##0.0"₽")',
  '#,##0.0"₽";[Red]#,##0.0"₽"',
  '#,##0.0"₽"',
  '#,##0.0"₽";[Red]-#,##0.0"₽"',
  '#,##0"₽"_);[Red](#,##0"₽")',
  '#,##0"₽"_);(#,##0"₽")',
  '#,##0"₽";[Red]#,##0"₽"',
  '#,##0"₽"',
  '#,##0"₽";[Red]-#,##0"₽"',
];
```

会计专用

```typescript
[
  '_(""* #,##0.0_);_(""* (#,##0.0);_(""* "-"??_);_(@_)',
  '_("¥"* #,##0.0_);_("¥"* (#,##0.0);_("¥"* "-"??_);_(@_)',
  '_("$"* #,##0.0_);_("$"* (#,##0.0);_("$"* "-"??_);_(@_)',
  '_("€"* #,##0.0_);_("€"* (#,##0.0);_("€"* "-"??_);_(@_)',
  '_("£"* #,##0.0_);_("£"* (#,##0.0);_("£"* "-"??_);_(@_)',
  '_("₩"* #,##0.0_);_("₩"* (#,##0.0);_("₩"* "-"??_);_(@_)',
  '_("NT$"* #,##0.0_);_("NT$"* (#,##0.0);_("NT$"* "-"??_);_(@_)',
  '_("Rp"* #,##0.0_);_("Rp"* (#,##0.0);_("Rp"* "-"??_);_(@_)',
  '_("IDR "* #,##0.0_);_("IDR "* (#,##0.0);_("IDR "* "-"??_);_(@_)',
  '_("JPY "* #,##0.0_);_("JPY "* (#,##0.0);_("JPY "* "-"??_);_(@_)',
  '_("KRW "* #,##0.0_);_("KRW "* (#,##0.0);_("KRW "* "-"??_);_(@_)',
  '_("KPW "* #,##0.0_);_("KPW "* (#,##0.0);_("KPW "* "-"??_);_(@_)',
  '_("NTD "* #,##0.0_);_("NTD "* (#,##0.0);_("NTD "* "-"??_);_(@_)',
  '_("RUB "* #,##0.0_);_("RUB "* (#,##0.0);_("RUB "* "-"??_);_(@_)',
  '_("SAR "* #,##0.0_);_("SAR "* (#,##0.0);_("SAR "* "-"??_);_(@_)',
  '_("TWD "* #,##0.0_);_("TWD "* (#,##0.0);_("TWD "* "-"??_);_(@_)',
  '_(""* #,##0_);_(""* (#,##0);_(""* "-"_);_(@_)',
  '_("¥"* #,##0_);_("¥"* (#,##0);_("¥"* "-"_);_(@_)',
  '_("$"* #,##0_);_("$"* (#,##0);_("$"* "-"_);_(@_)',
  '_("€"* #,##0_);_("€"* (#,##0);_("€"* "-"_);_(@_)',
  '_("£"* #,##0_);_("£"* (#,##0);_("£"* "-"_);_(@_)',
  '_("₩"* #,##0_);_("₩"* (#,##0);_("₩"* "-"_);_(@_)',
  '_("NT$"* #,##0_);_("NT$"* (#,##0);_("NT$"* "-"_);_(@_)',
  '_("Rp"* #,##0_);_("Rp"* (#,##0);_("Rp"* "-"_);_(@_)',
  '_("IDR "* #,##0_);_("IDR "* (#,##0);_("IDR "* "-"_);_(@_)',
  '_("JPY "* #,##0_);_("JPY "* (#,##0);_("JPY "* "-"_);_(@_)',
  '_("KRW "* #,##0_);_("KRW "* (#,##0);_("KRW "* "-"_);_(@_)',
  '_("KPW "* #,##0_);_("KPW "* (#,##0);_("KPW "* "-"_);_(@_)',
  '_("NTD "* #,##0_);_("NTD "* (#,##0);_("NTD "* "-"_);_(@_)',
  '_("RUB "* #,##0_);_("RUB "* (#,##0);_("RUB "* "-"_);_(@_)',
  '_("SAR "* #,##0_);_("SAR "* (#,##0);_("SAR "* "-"_);_(@_)',
  '_("TWD "* #,##0_);_("TWD "* (#,##0);_("TWD "* "-"_);_(@_)',
  '_(* #,##0.0"₽"_);_(* (#,##0.0"₽");_("₽"* "-"??_);_(@_)',
  '_(* #,##0"₽"_);_(* (#,##0"₽");_("₽"* "-"_);_(@_)',
];
```

日期

```typescript
[
  "yyyy/m/d",
  "yyyy/mm/dd;@",
  "yyyy-mm-dd;@",
  "yy/mm/dd;@",
  "yy-mm-dd;@",
  "mm/dd;@",
  "mm-dd;@",
  "yyyy-m-d;@",
  "yy/m/d;@",
  "yy-m-d;@",
  "m/d;@",
  "m-d;@",
  "m/d/yy;@",
  "mm/dd/yy;@",
  "yyyy/m;@",
  "yyyy/mm;@",
  "yyyy-m;@",
  "yyyy-mm;@",
  "d;@",
  "dd;@",
  'yyyy"年"m"月"d"日" aaaa',
  'yyyy"年"m"月"d"日";@',
  'yyyy"年"m"月";@',
  'm"月"d"日";@',
  '[$-804]yyyy"年"mm"月"dd"日" aaaa;@',
  'yyyy"年"mm"月"dd"日";@',
  'yyyy"年"mm"月";@',
  'mm"月"dd"日";@',
  'm"月";@',
  'mm"月";@',
  '[DBNum1][$-804]m"月";@',
  "aaaa;@",
  "aaa;@",
  '[DBNum1][$-804]yyyy"年"m"月"d"日";@',
  '[DBNum1][$-804]yyyy"年"m"月";@',
  '[DBNum1][$-804]m"月"d"日";@',
  "[$-409]d-mmm;@",
  "[$-409]d-mmm-yy;@",
  "[$-409]dd-mmm-yy;@",
  "[$-409]mmm-yy;@",
  "[$-409]mmmm-yy;@",
  "[$-409]mmmmm;@",
  "[$-409]mmmmm-yy;@",
  "[$-409]yyyy/mm/dd h:mm AM/PM;@",
  "yyyy/mm/dd hh:mm;@",
  "[$-409]yyyy/m/d h:mm AM/PM;@",
  "yyyy/m/d h:mm;@",
  "[$-409]yyyy/mm/dd h:mm:ss AM/PM;@",
  "yyyy/mm/dd hh:mm:ss;@",
  "[$-409]yyyy/m/d h:mm:ss AM/PM;@",
  "yyyy/m/d h:mm:ss;@",
  "d/m/yyyy",
  "d-m-yyyy;@",
  "d/m/yyyy h:mm:ss;@",
];
```

时间

```typescript
[
  "h:mm:ss;@",
  "h:mm;@",
  "AM/PM h:mm:ss",
  `${s18n("上午")}/${s18n("下午")}h"${s18n("时")}"mm"分"ss"秒";@`,
  `${s18n("上午")}/${s18n("下午")}h"${s18n("时")}"mm"分";@`,
  `[DBNum1]h"${s18n("时")}"mm"分";@`,
  `[DBNum1]${s18n("上午")}/${s18n("下午")}h"${s18n("时")}"mm"分";@`,
  "[$-409]h:mm AM/PM;@",
  "[$-409]h:mm:ss AM/PM;@",
  `h"${s18n("时")}"mm"分";@`,
  `h"${s18n("时")}"mm"分"ss"秒";@`,
];
```

分数

```typescript
[
  "# ?/?",
  "# ??/??",
  "# ???/???",
  "# ?/2",
  "# ?/4",
  "# ?/8",
  "# ??/16",
  "# ?/10",
  "# ??/100",
];
```

科学计数

```typescript
["0.0E+00", "0.E+00", "0E+00"];
```

特殊

```typescript
["000000", "[DBNum1][$-804]General", "[DBNum2][$-804]General"];
```

自定义

```typescript
// 无论传什么格式，直接保存，至于渲染的时候能不能解析出来，取决于该格式是否合法
```

需要注意一点的是，对于「数值」「百分比」「货币」「会计」「科学计数」这几种类型，如果设置不同的小数位数（即精度），那么对应的格式可能就需要在上面列出的这些格式的基础上加上对精度的表示。举个例子：

```typescript
"0_);[Red](0)";
```

这个数值格式表示没有小数位；不使用千分位；如果是正数，右侧缩进一位；如果是负数，字体颜色为红色且用括号括起来（而不是显示负号）
如果想要在上面这个格式的基础上带上 3 位小数位，则格式为：

```typescript
"0.000_);[Red](0.000)";
```

Excel 格式设置相关文档可参考：

[https://www.ablebits.com/office-addins-blog/custom-excel-number-format/](https://www.ablebits.com/office-addins-blog/custom-excel-number-format/)

#### 请求参数

| 名称      | 位置 | 类型   | 必选 | 说明       |
| --------- | ---- | ------ | ---- | ---------- |
| fileId    | path | string | 是   | 文件 ID    |
| sheetName | path | string | 是   | Sheet 名称 |
| cell      | path | string | 是   | 单元格位置 |
| body      | body | object | 否   | none       |

#### 返回结果

| 状态码 | 状态码含义                                                      | 说明 | 数据模型 |
| ------ | --------------------------------------------------------------- | ---- | -------- |
| 204    | [No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5) | 成功 | Inline   |

#### 返回数据结构

无

### 设置数据验证

POST /sdk/v2/api/files/{providerFileID}/sheets/{sheetName}/data-validations

#### 类型定义

```typescript
// 本接口的 Body 数据类型（In JSON）
export declare type DataValidationRule =
  | DataValidationList
  | DataValidationCustomList
  | DataValidationText
  | DataValidationDate
  | DataValidationNumber
  | DataValidationCustomFormula;

export enum ValidationType {
  List = 0, //列表
  Number, //数字
  Date, //日期
  Text, // 文本
  CustomFormula, // 自定义公式
}

// 文本验证规则索引
export enum TextConditionIndex {
  Equal = 0,
  Contain,
  NoContain,
  Email,
  Link,
  Phone,
  Telephone,
  IdCard,
}

// 文本验证规则
export enum TextCompareType {
  Equal = 0,
  Contain = 6,
  NoContain = 7,
  Email = 8,
  Link = 9,
  Phone = 10,
  Telephone = 11,
  IdCard = 12,
}

// 数字验证规则索引
export enum NumberConditionIndex {
  Equal = 0,
  NotEqual,
  GreaterThan,
  GreaterOrEqual,
  LessThan,
  LessOrEqual,
  Between,
  NoBetween,
}

//数字验证规则
export enum NumberCompareType {
  Equal = 0,
  NotEqual = 1,
  GreaterThan = 2,
  GreaterOrEqual = 3,
  LessThan = 4,
  LessOrEqual = 5,
  Between = 6,
  NoBetween = 7,
}

//日期验证规则索引
export enum DateConditionIndex {
  Equal = 0,
  NotEqual,
  GreaterThan,
  LessThan,
  Between,
  NoBetween,
}
//日期验证规则
export enum DateCompareType {
  Equal = 0,
  NotEqual = 1,
  GreaterThan = 2,
  LessThan = 4,
  Between = 6,
  NoBetween = 7,
}

// 数据无效时
export enum InvalidAction {
  ShowWarning = 0, // 显示警告
  RejectInput, // 拒绝输入
}

type Nullable<T> = T | null;

export interface DataValidationBase {
  curTypeIndex: ValidationType; // 验证类型
  curValiIndex: InvalidAction; // 数据无效时操作：显示警告 | 拒绝输入
  ignoreEmpty: boolean; // 是否忽略空值
  isTipFromUser: boolean; //是否是自户自定义提示，可默认传false
  showTip: boolean; // 是否显示提示
  tipText: string; // 提示文本
  errorTip: string; // 错误提示文案
}

// 列表验证-手动添加下拉选项
interface DataValidationList extends DataValidationBase {
  customList: string; //列表选项，用逗号分隔
  multipleSelect: boolean; // 是否支持多选
  showArrow: boolean; // 是否是示箭头
  showColor: boolean; // 是否显示选项背景色
  colors: string[]; // 背景色列表
}

// 列表验证 - 从单元格选择下拉选项
interface DataValidationCustomList extends DataValidationBase {
  baseCol: number; //验证范围的左上角单元格所在的列
  baseRow: number; //验证范围的左上角单元格所在的行
  showColor: boolean; // 是否显示选项背景色
  colors: string[]; // 背景色列表
  customList: string; // 引用范围，如：=工作表3!$G$17:$G$30
  multipleSelect: boolean; // 是否支持多选
  showArrow: boolean; // 是否是示箭头，可默认传true
}

// 文本验证
interface DataValidationText extends DataValidationBase {
  conditionValue1: Nullable<string>;
  conditionValue2: Nullable<string>;
  curConditionIndex: TextConditionIndex;
  curConditionTypeValue: TextCompareType;
}

//日期验证
interface DataValidationDate extends DataValidationBase {
  conditionValue1: Nullable<string>;
  conditionValue2: Nullable<string>;
  curConditionIndex: DateConditionIndex;
  curConditionTypeValue: DateCompareType;
}

//数字验证
interface DataValidationNumber extends DataValidationBase {
  conditionValue1: Nullable<number>;
  conditionValue2: Nullable<number>;
  curConditionIndex: NumberConditionIndex;
  curConditionTypeValue: NumberCompareType;
}

// 自定义公式验证
interface DataValidationCustomFormula extends DataValidationBase {
  baseCol: number; //验证范围的左上角单元格所在的列
  baseRow: number; //验证范围的左上角单元格所在的行
  condition: string; //公式 如：=ISTEXT(A1)
}
```

> Body 请求参数示例

```json
{
  "range": "A1:A20",
  "rule": {
    "curTypeIndex": 0,
    "ignoreEmpty": true,
    "curValiIndex": 0,
    "showTip": false,
    "tipText": "输入一个列表中的值",
    "isTipFromUser": false,
    "showColor": true,
    "colors": ["#dad5e9", "#cce0f1", "#c7e6ea"],
    "multipleSelect": false,
    "customList": "11,22,33",
    "showArrow": true,
    "errorTip": "输入一个列表中的值"
  }
}
```

#### 请求参数

| 名称           | 位置 | 类型   | 必选 | 说明                                             |
| -------------- | ---- | ------ | ---- | ------------------------------------------------ |
| providerFileID | path | string | 是   | 文件 ID                                          |
| sheetName      | path | string | 是   | Sheet 名称                                       |
| body           | body | object | 否   | none                                             |
| range          | body | string | 是   | none                                             |
| rule           | body | object | 是   | 参照描述部分中的 **DataValidationRule** 类型声明 |

#### 返回结果

| 状态码 | 状态码含义                                                      | 说明 | 数据模型 |
| ------ | --------------------------------------------------------------- | ---- | -------- |
| 204    | [No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5) | 成功 | Inline   |

#### 返回数据结构

无

### 设置条件格式

POST /sdk/v2/api/files/{providerFileID}/sheets/{sheetName}/conditional-formats

#### 类型定义

```typescript
// 本接口的 Body 类型描述
export interface CFRuleChanges {
  // RuleId 为 16 位长度的字符串，对应标识每条具体的条件格式规则
  [RuleId: string]: ConditionalFormatRule | null;
}

interface RangeRef {
  row: number;
  col: number;
  rowCount: number;
  colCount: number;
}

export enum RuleTypes {
  CellValue = "cellValue",
  Text = "text",
  Duplicate = "duplicate",
  Unique = "unique",
  Custom = "custom",
  Top10 = "top10",
  Average = "average",
  DataBar = "dataBar",
  TwoScale = "twoScale",
  ThreeScale = "threeScale",
}

export type RuleOperators = -1 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9;

export interface ConditionalFormatRule {
  // 应用条件格式的选区范围，可多个
  ranges: RangeRef[];
  // 规则类型，目前我们支持的有：
  // 突出显示单元格、最前/最后规则、自定义公式、数据条、双色色阶、三色色阶
  ruleType: RuleTypes;
  // 具体条件类型，需要结合 ruleType 来理解，
  // 比如选择了「突出显示单元格」之后，可以选择「等于」「大于」「小于」等等选项……
  operator: RuleOperators;
  // 根据 ruleType 和 operator 的不同，params 数组的个数和值也有所不同，
  // 具体用法下面会详细列出每种组合下 params 对应的值
  params: (string | number | null)[];
  // 满足条件格式时的样式，
  // 大多数场景下都是[foreColor, backColor, bold, italic, underline, strike],
  // 也有少数几种情况(如数据条、双色/三色色阶)会有点区别，下面也会细说
  style: (string | number | null)[];
}
```

#### 突出显示单元格

##### 等于

```typescript
// 举例：
const rule = {
  ranges: [{ row: 0, rowCount: 1, col: 0, colCount: 1 }],
  ruleType: "cellValue",
  operator: 0,
  // 必须为包含2个元素的数组: [string, null]
  // 第一个元素为需要比较的值，类型为 string，第二个元素的值为 null
  params: ["1", null],
  // 必须为包含6个元素的数组: [string, string, 0|1|null, 0|1|null, 0|1|null, 0|1|null]
  // 第1个元素：应用条件格式的单元格字体颜色，类型为 string，
  // 第2个元素：应用条件格式的单元格背景颜色，类型为 string，
  // 第3个元素：应用条件格式的单元格是否加粗，没设置过为 null，需要加粗为 1，不需要为 0，
  // 第4个元素：应用条件格式的单元格是否斜体，没设置过为 null，需要斜体为 1，不需要为 0，
  // 第5个元素：应用条件格式的单元格是否显示下划线，没设置过为 null，需要下划线为 1，不需要为 0，
  // 第6个元素：应用条件格式的单元格是否显示中划线，没设置过为 null，需要中划线为 1，不需要为 0，
  style: ["#2B2B2B", "#FBD4D0", 1, null, 1, 0],
};
```

##### 大于

```typescript
// 举例：
const rule = {
  ranges: [{ row: 0, rowCount: 1, col: 0, colCount: 1 }],
  ruleType: "cellValue",
  operator: 2,
  // 必须为包含2个元素的数组: [string, null]
  // 第一个元素为需要比较的值，类型为 string，第二个元素的值为 null
  params: ["1", null],
  // 必须为包含6个元素的数组: [string, string, 0|1|null, 0|1|null, 0|1|null, 0|1|null]
  // 第1个元素：应用条件格式的单元格字体颜色，类型为 string，
  // 第2个元素：应用条件格式的单元格背景颜色，类型为 string，
  // 第3个元素：应用条件格式的单元格是否加粗，没设置过为 null，需要加粗为 1，不需要为 0，
  // 第4个元素：应用条件格式的单元格是否斜体，没设置过为 null，需要斜体为 1，不需要为 0，
  // 第5个元素：应用条件格式的单元格是否显示下划线，没设置过为 null，需要下划线为 1，不需要为 0，
  // 第6个元素：应用条件格式的单元格是否显示中划线，没设置过为 null，需要中划线为 1，不需要为 0，
  style: ["#2B2B2B", "#FBD4D0", 1, null, 1, 0],
};
```

##### 大于等于

```typescript
// 举例：
const rule = {
  ranges: [{ row: 0, rowCount: 1, col: 0, colCount: 1 }],
  ruleType: "cellValue",
  operator: 3,
  // 必须为包含2个元素的数组: [string, null]
  // 第一个元素为需要比较的值，类型为 string，第二个元素的值为 null
  params: ["1", null],
  // 必须为包含6个元素的数组: [string, string, 0|1|null, 0|1|null, 0|1|null, 0|1|null]
  // 第1个元素：应用条件格式的单元格字体颜色，类型为 string，
  // 第2个元素：应用条件格式的单元格背景颜色，类型为 string，
  // 第3个元素：应用条件格式的单元格是否加粗，没设置过为 null，需要加粗为 1，不需要为 0，
  // 第4个元素：应用条件格式的单元格是否斜体，没设置过为 null，需要斜体为 1，不需要为 0，
  // 第5个元素：应用条件格式的单元格是否显示下划线，没设置过为 null，需要下划线为 1，不需要为 0，
  // 第6个元素：应用条件格式的单元格是否显示中划线，没设置过为 null，需要中划线为 1，不需要为 0，
  style: ["#2B2B2B", "#FBD4D0", 1, null, 1, 0],
};
```

##### 小于

```typescript
// 举例：
const rule = {
  ranges: [{ row: 0, rowCount: 1, col: 0, colCount: 1 }],
  ruleType: "cellValue",
  operator: 4,
  // 必须为包含2个元素的数组: [string, null]
  // 第一个元素为需要比较的值，类型为 string，第二个元素的值为 null
  params: ["1", null],
  // 必须为包含6个元素的数组: [string, string, 0|1|null, 0|1|null, 0|1|null, 0|1|null]
  // 第1个元素：应用条件格式的单元格字体颜色，类型为 string，
  // 第2个元素：应用条件格式的单元格背景颜色，类型为 string，
  // 第3个元素：应用条件格式的单元格是否加粗，没设置过为 null，需要加粗为 1，不需要为 0，
  // 第4个元素：应用条件格式的单元格是否斜体，没设置过为 null，需要斜体为 1，不需要为 0，
  // 第5个元素：应用条件格式的单元格是否显示下划线，没设置过为 null，需要下划线为 1，不需要为 0，
  // 第6个元素：应用条件格式的单元格是否显示中划线，没设置过为 null，需要中划线为 1，不需要为 0，
  style: ["#2B2B2B", "#FBD4D0", 1, null, 1, 0],
};
```

##### 小于等于

```typescript
// 举例：
const rule = {
  ranges: [{ row: 0, rowCount: 1, col: 0, colCount: 1 }],
  ruleType: "cellValue",
  operator: 5,
  // 必须为包含2个元素的数组: [string, null]
  // 第一个元素为需要比较的值，类型为 string，第二个元素的值为 null
  params: ["1", null],
  // 必须为包含6个元素的数组: [string, string, 0|1|null, 0|1|null, 0|1|null, 0|1|null]
  // 第1个元素：应用条件格式的单元格字体颜色，类型为 string，
  // 第2个元素：应用条件格式的单元格背景颜色，类型为 string，
  // 第3个元素：应用条件格式的单元格是否加粗，没设置过为 null，需要加粗为 1，不需要为 0，
  // 第4个元素：应用条件格式的单元格是否斜体，没设置过为 null，需要斜体为 1，不需要为 0，
  // 第5个元素：应用条件格式的单元格是否显示下划线，没设置过为 null，需要下划线为 1，不需要为 0，
  // 第6个元素：应用条件格式的单元格是否显示中划线，没设置过为 null，需要中划线为 1，不需要为 0，
  style: ["#2B2B2B", "#FBD4D0", 1, null, 1, 0],
};
```

##### 介于

```typescript
// 举例：
const rule = {
  ranges: [{ row: 0, rowCount: 1, col: 0, colCount: 1 }],
  ruleType: "cellValue",
  operator: 6,
  // 必须为包含2个元素的数组: [string, string]
  // 分别为设置的「介于」之间的两个值，类型都为 string
  params: ["1", "2"],
  // 必须为包含6个元素的数组: [string, string, 0|1|null, 0|1|null, 0|1|null, 0|1|null]
  // 第1个元素：应用条件格式的单元格字体颜色，类型为 string，
  // 第2个元素：应用条件格式的单元格背景颜色，类型为 string，
  // 第3个元素：应用条件格式的单元格是否加粗，没设置过为 null，需要加粗为 1，不需要为 0，
  // 第4个元素：应用条件格式的单元格是否斜体，没设置过为 null，需要斜体为 1，不需要为 0，
  // 第5个元素：应用条件格式的单元格是否显示下划线，没设置过为 null，需要下划线为 1，不需要为 0，
  // 第6个元素：应用条件格式的单元格是否显示中划线，没设置过为 null，需要中划线为 1，不需要为 0，
  style: ["#2B2B2B", "#FBD4D0", 1, null, 1, 0],
};
```

##### 包含

```typescript
// 举例：
const rule = {
  ranges: [{ row: 0, rowCount: 1, col: 0, colCount: 1 }],
  ruleType: "text",
  operator: 0,
  // 必须为包含2个元素的数组: [string, null]
  // 第一个元素为需要包含的值，类型为 string，第二个元素的值为 null
  params: ["1", null],
  // 必须为包含6个元素的数组: [string, string, 0|1|null, 0|1|null, 0|1|null, 0|1|null]
  // 第1个元素：应用条件格式的单元格字体颜色，类型为 string，
  // 第2个元素：应用条件格式的单元格背景颜色，类型为 string，
  // 第3个元素：应用条件格式的单元格是否加粗，没设置过为 null，需要加粗为 1，不需要为 0，
  // 第4个元素：应用条件格式的单元格是否斜体，没设置过为 null，需要斜体为 1，不需要为 0，
  // 第5个元素：应用条件格式的单元格是否显示下划线，没设置过为 null，需要下划线为 1，不需要为 0，
  // 第6个元素：应用条件格式的单元格是否显示中划线，没设置过为 null，需要中划线为 1，不需要为 0，
  style: ["#2B2B2B", "#FBD4D0", 1, null, 1, 0],
};
```

##### 重复值

```typescript
// 举例：
const rule = {
  ranges: [{ row: 0, rowCount: 1, col: 0, colCount: 1 }],
  ruleType: "duplicate",
  operator: -1,
  // 必须为空数组: []
  params: [],
  // 必须为包含6个元素的数组: [string, string, 0|1|null, 0|1|null, 0|1|null, 0|1|null]
  // 第1个元素：应用条件格式的单元格字体颜色，类型为 string，
  // 第2个元素：应用条件格式的单元格背景颜色，类型为 string，
  // 第3个元素：应用条件格式的单元格是否加粗，没设置过为 null，需要加粗为 1，不需要为 0，
  // 第4个元素：应用条件格式的单元格是否斜体，没设置过为 null，需要斜体为 1，不需要为 0，
  // 第5个元素：应用条件格式的单元格是否显示下划线，没设置过为 null，需要下划线为 1，不需要为 0，
  // 第6个元素：应用条件格式的单元格是否显示中划线，没设置过为 null，需要中划线为 1，不需要为 0，
  style: ["#2B2B2B", "#FBD4D0", 1, null, 1, 0],
};
```

##### 唯一值

```typescript
// 举例：
const rule = {
  ranges: [{ row: 0, rowCount: 1, col: 0, colCount: 1 }],
  ruleType: "unique",
  operator: -1,
  // 必须为空数组: []
  params: [],
  // 必须为包含6个元素的数组: [string, string, 0|1|null, 0|1|null, 0|1|null, 0|1|null]
  // 第1个元素：应用条件格式的单元格字体颜色，类型为 string，
  // 第2个元素：应用条件格式的单元格背景颜色，类型为 string，
  // 第3个元素：应用条件格式的单元格是否加粗，没设置过为 null，需要加粗为 1，不需要为 0，
  // 第4个元素：应用条件格式的单元格是否斜体，没设置过为 null，需要斜体为 1，不需要为 0，
  // 第5个元素：应用条件格式的单元格是否显示下划线，没设置过为 null，需要下划线为 1，不需要为 0，
  // 第6个元素：应用条件格式的单元格是否显示中划线，没设置过为 null，需要中划线为 1，不需要为 0，
  style: ["#2B2B2B", "#FBD4D0", 1, null, 1, 0],
};
```

##### 空值

```typescript
// 举例：
const rule = {
  ranges: [{ row: 0, rowCount: 1, col: 0, colCount: 1 }],
  ruleType: "custom",
  operator: 0,
  // 必须为空数组: []
  params: [],
  // 必须为包含6个元素的数组: [string, string, 0|1|null, 0|1|null, 0|1|null, 0|1|null]
  // 第1个元素：应用条件格式的单元格字体颜色，类型为 string，
  // 第2个元素：应用条件格式的单元格背景颜色，类型为 string，
  // 第3个元素：应用条件格式的单元格是否加粗，没设置过为 null，需要加粗为 1，不需要为 0，
  // 第4个元素：应用条件格式的单元格是否斜体，没设置过为 null，需要斜体为 1，不需要为 0，
  // 第5个元素：应用条件格式的单元格是否显示下划线，没设置过为 null，需要下划线为 1，不需要为 0，
  // 第6个元素：应用条件格式的单元格是否显示中划线，没设置过为 null，需要中划线为 1，不需要为 0，
  style: ["#2B2B2B", "#FBD4D0", 1, null, 1, 0],
};
```

##### 非空值

```typescript
// 举例：
const rule = {
  ranges: [{ row: 0, rowCount: 1, col: 0, colCount: 1 }],
  ruleType: "custom",
  operator: 1,
  // 必须为空数组: []
  params: [],
  // 必须为包含6个元素的数组: [string, string, 0|1|null, 0|1|null, 0|1|null, 0|1|null]
  // 第1个元素：应用条件格式的单元格字体颜色，类型为 string，
  // 第2个元素：应用条件格式的单元格背景颜色，类型为 string，
  // 第3个元素：应用条件格式的单元格是否加粗，没设置过为 null，需要加粗为 1，不需要为 0，
  // 第4个元素：应用条件格式的单元格是否斜体，没设置过为 null，需要斜体为 1，不需要为 0，
  // 第5个元素：应用条件格式的单元格是否显示下划线，没设置过为 null，需要下划线为 1，不需要为 0，
  // 第6个元素：应用条件格式的单元格是否显示中划线，没设置过为 null，需要中划线为 1，不需要为 0，
  style: ["#2B2B2B", "#FBD4D0", 1, null, 1, 0],
};
```

##### 错误

```typescript
// 举例：
const rule = {
  ranges: [{ row: 0, rowCount: 1, col: 0, colCount: 1 }],
  ruleType: "custom",
  operator: 2,
  // 必须为空数组: []
  params: [],
  // 必须为包含6个元素的数组: [string, string, 0|1|null, 0|1|null, 0|1|null, 0|1|null]
  // 第1个元素：应用条件格式的单元格字体颜色，类型为 string，
  // 第2个元素：应用条件格式的单元格背景颜色，类型为 string，
  // 第3个元素：应用条件格式的单元格是否加粗，没设置过为 null，需要加粗为 1，不需要为 0，
  // 第4个元素：应用条件格式的单元格是否斜体，没设置过为 null，需要斜体为 1，不需要为 0，
  // 第5个元素：应用条件格式的单元格是否显示下划线，没设置过为 null，需要下划线为 1，不需要为 0，
  // 第6个元素：应用条件格式的单元格是否显示中划线，没设置过为 null，需要中划线为 1，不需要为 0，
  style: ["#2B2B2B", "#FBD4D0", 1, null, 1, 0],
};
```

##### 无错误

```typescript
// 举例：
const rule = {
  ranges: [{ row: 0, rowCount: 1, col: 0, colCount: 1 }],
  ruleType: "custom",
  operator: 3,
  // 必须为空数组: []
  params: [],
  // 必须为包含6个元素的数组: [string, string, 0|1|null, 0|1|null, 0|1|null, 0|1|null]
  // 第1个元素：应用条件格式的单元格字体颜色，类型为 string，
  // 第2个元素：应用条件格式的单元格背景颜色，类型为 string，
  // 第3个元素：应用条件格式的单元格是否加粗，没设置过为 null，需要加粗为 1，不需要为 0，
  // 第4个元素：应用条件格式的单元格是否斜体，没设置过为 null，需要斜体为 1，不需要为 0，
  // 第5个元素：应用条件格式的单元格是否显示下划线，没设置过为 null，需要下划线为 1，不需要为 0，
  // 第6个元素：应用条件格式的单元格是否显示中划线，没设置过为 null，需要中划线为 1，不需要为 0，
  style: ["#2B2B2B", "#FBD4D0", 1, null, 1, 0],
};
```

#### 最前/最后规则

##### 最前

```typescript
// 举例：
const rule = {
  ranges: [{row: 0, rowCount: 1, col: 0, colCount: 1}],
  ruleType: 'top10',
  operator: 0,
  // 必须为包含2个元素的数组: [number, 0|1]
  // 第一个元素为最前多少位的数值，类型为 number，第二个元素表示是否为最前百分之多少，是为 1，否为 0
  // 如：[10, 1] 表示前 10%；[20, 0] 表示前 20 位
  params: [10， 1],
  // 必须为包含6个元素的数组: [string, string, 0|1|null, 0|1|null, 0|1|null, 0|1|null]
  // 第1个元素：应用条件格式的单元格字体颜色，类型为 string，
  // 第2个元素：应用条件格式的单元格背景颜色，类型为 string，
  // 第3个元素：应用条件格式的单元格是否加粗，没设置过为 null，需要加粗为 1，不需要为 0，
  // 第4个元素：应用条件格式的单元格是否斜体，没设置过为 null，需要斜体为 1，不需要为 0，
  // 第5个元素：应用条件格式的单元格是否显示下划线，没设置过为 null，需要下划线为 1，不需要为 0，
  // 第6个元素：应用条件格式的单元格是否显示中划线，没设置过为 null，需要中划线为 1，不需要为 0，
  style: ['#2B2B2B', '#FBD4D0', 1, null, 1, 0]
}
```

##### 最后

```typescript
// 举例：
const rule = {
  ranges: [{row: 0, rowCount: 1, col: 0, colCount: 1}],
  ruleType: 'top10',
  operator: 1,
  // 必须为包含2个元素的数组: [number, 0|1]
  // 第一个元素为最后多少位的数值，类型为 number，第二个元素表示是否为最后百分之多少，是为 1，否为 0
  // 如：[10, 1] 表示后 10%；[20, 0] 表示后 20 位
  params: [20， 0],
  // 必须为包含6个元素的数组: [string, string, 0|1|null, 0|1|null, 0|1|null, 0|1|null]
  // 第1个元素：应用条件格式的单元格字体颜色，类型为 string，
  // 第2个元素：应用条件格式的单元格背景颜色，类型为 string，
  // 第3个元素：应用条件格式的单元格是否加粗，没设置过为 null，需要加粗为 1，不需要为 0，
  // 第4个元素：应用条件格式的单元格是否斜体，没设置过为 null，需要斜体为 1，不需要为 0，
  // 第5个元素：应用条件格式的单元格是否显示下划线，没设置过为 null，需要下划线为 1，不需要为 0，
  // 第6个元素：应用条件格式的单元格是否显示中划线，没设置过为 null，需要中划线为 1，不需要为 0，
  style: ['#2B2B2B', '#FBD4D0', 1, null, 1, 0]
}
```

##### 高于平均值

```typescript
// 举例：
const rule = {
  ranges: [{ row: 0, rowCount: 1, col: 0, colCount: 1 }],
  ruleType: "average",
  operator: 0,
  // 必须为空数组: []
  params: [],
  // 必须为包含6个元素的数组: [string, string, 0|1|null, 0|1|null, 0|1|null, 0|1|null]
  // 第1个元素：应用条件格式的单元格字体颜色，类型为 string，
  // 第2个元素：应用条件格式的单元格背景颜色，类型为 string，
  // 第3个元素：应用条件格式的单元格是否加粗，没设置过为 null，需要加粗为 1，不需要为 0，
  // 第4个元素：应用条件格式的单元格是否斜体，没设置过为 null，需要斜体为 1，不需要为 0，
  // 第5个元素：应用条件格式的单元格是否显示下划线，没设置过为 null，需要下划线为 1，不需要为 0，
  // 第6个元素：应用条件格式的单元格是否显示中划线，没设置过为 null，需要中划线为 1，不需要为 0，
  style: ["#2B2B2B", "#FBD4D0", 1, null, 1, 0],
};
```

##### 低于平均值

```typescript
// 举例：
const rule = {
  ranges: [{ row: 0, rowCount: 1, col: 0, colCount: 1 }],
  ruleType: "average",
  operator: 1,
  // 必须为空数组: []
  params: [],
  // 必须为包含6个元素的数组: [string, string, 0|1|null, 0|1|null, 0|1|null, 0|1|null]
  // 第1个元素：应用条件格式的单元格字体颜色，类型为 string，
  // 第2个元素：应用条件格式的单元格背景颜色，类型为 string，
  // 第3个元素：应用条件格式的单元格是否加粗，没设置过为 null，需要加粗为 1，不需要为 0，
  // 第4个元素：应用条件格式的单元格是否斜体，没设置过为 null，需要斜体为 1，不需要为 0，
  // 第5个元素：应用条件格式的单元格是否显示下划线，没设置过为 null，需要下划线为 1，不需要为 0，
  // 第6个元素：应用条件格式的单元格是否显示中划线，没设置过为 null，需要中划线为 1，不需要为 0，
  style: ["#2B2B2B", "#FBD4D0", 1, null, 1, 0],
};
```

#### 自定义公式

```typescript
// 举例：
const rule = {
  ranges: [{ row: 0, rowCount: 1, col: 0, colCount: 1 }],
  ruleType: "custom",
  operator: 4,
  // 必须为包含2个元素的数组: [string, null]
  // 第一个元素为输入的公式，**注意不需要以=开头**，类型为 string，第二个元素的值为 null
  params: ["a", null],
  // 必须为包含6个元素的数组: [string, string, 0|1|null, 0|1|null, 0|1|null, 0|1|null]
  // 第1个元素：应用条件格式的单元格字体颜色，类型为 string，
  // 第2个元素：应用条件格式的单元格背景颜色，类型为 string，
  // 第3个元素：应用条件格式的单元格是否加粗，没设置过为 null，需要加粗为 1，不需要为 0，
  // 第4个元素：应用条件格式的单元格是否斜体，没设置过为 null，需要斜体为 1，不需要为 0，
  // 第5个元素：应用条件格式的单元格是否显示下划线，没设置过为 null，需要下划线为 1，不需要为 0，
  // 第6个元素：应用条件格式的单元格是否显示中划线，没设置过为 null，需要中划线为 1，不需要为 0，
  style: ["#2B2B2B", "#FBD4D0", 1, null, 1, 0],
};
```

##### 数据条

```typescript
// 举例：
const rule = {
  ranges: [{ row: 0, rowCount: 1, col: 0, colCount: 1 }],
  ruleType: "dataBar",
  operator: -1,
  // 必须为包含5个元素的数组: [number, string, number, string, 0|1]
  // 分别解释如下：
  // 第一个元素为最小值的类型，取值为：
  // 1: 最小值
  // 0: 数值
  // 3: 百分比
  // 6: 公式
  // 4: 百分点值
  // 5: 自动
  // 第二个元素为最小值的输入值，类型为 string，如果是「自动」或「最小值」，那这里就是空字符串""
  // 第三个元素为最大值的类型，取值为：
  // 2: 最大值
  // 0: 数值
  // 3: 百分比
  // 6: 公式
  // 4: 百分点值
  // 7: 自动
  // 第四个元素为最大值的输入值，类型为 string，如果是「自动」或「最大值」，那这里就是空字符串""
  // 第五个元素表示是否勾选「隐藏数据」选项，勾选了为 1，未勾选为 0
  params: [4, "10", 4, "100", 0],
  // 必须为包含3个元素的数组: [string, string, 0|1]
  // 第1个元素：数据条样式「负值」的单元格背景颜色，类型为 string，
  // 第2个元素：数据条样式「正值」的单元格背景颜色，类型为 string，
  // 第3个元素：数据条样式为纯色或渐变，纯色为 0，渐变为 1，
  style: ["#2B2B2B", "#FBD4D0", 1],
};
```

#### 双色色阶

```typescript
// 举例：
const rule = {
  ranges: [{ row: 0, rowCount: 1, col: 0, colCount: 1 }],
  ruleType: "twoScale",
  operator: -1,
  // 必须为包含4个元素的数组: [number, string, number, string]
  // 分别解释如下：
  // 第一个元素为最小值的类型，取值为：
  // 1: 最小值
  // 0: 数值
  // 3: 百分比
  // 6: 公式
  // 4: 百分点值
  // 第二个元素为最小值的输入值，类型为 string，如果是「最小值」，那这里就是空字符串""
  // 第三个元素为最大值的类型，取值为：
  // 2: 最大值
  // 0: 数值
  // 3: 百分比
  // 6: 公式
  // 4: 百分点值
  // 第四个元素为最大值的输入值，类型为 string，如果是「最大值」，那这里就是空字符串""
  params: [4, "10", 4, "100"],
  // 必须为包含2个元素的数组: [string, string]
  // 第1个元素：最小值对应的单元格背景颜色，类型为 string，
  // 第2个元素：最大值对应的单元格背景颜色，类型为 string，
  style: ["#2B2B2B", "#FBD4D0"],
};
```

#### 三色色阶

```typescript
// 举例：
const rule = {
  ranges: [{ row: 0, rowCount: 1, col: 0, colCount: 1 }],
  ruleType: "threeScale",
  operator: -1,
  // 必须为包含6个元素的数组: [number, string, number, number, number, string]
  // 分别解释如下：
  // 第一个元素为最小值的类型，取值为：
  // 1: 最小值
  // 0: 数值
  // 3: 百分比
  // 6: 公式
  // 4: 百分点值
  // 第二个元素为最小值的输入值，类型为 string，如果是「最小值」，那这里就是空字符串""
  // 第三个元素为中间值的类型，取值为：
  // 0: 数值
  // 3: 百分比
  // 6: 公式
  // 4: 百分点值
  // 第四个元素为中间值的输入值，类型为 number，
  // 第五个元素为最大值的类型，取值为：
  // 2: 最大值
  // 0: 数值
  // 3: 百分比
  // 6: 公式
  // 4: 百分点值
  // 第六个元素为最大值的输入值，类型为 string，如果是「最大值」，那这里就是空字符串""
  params: [4, "10", 4, 50, 4, "100"],
  // 必须为包含3个元素的数组: [string, string, string]
  // 第1个元素：最小值对应的单元格背景颜色，类型为 string，
  // 第2个元素：中间值对应的单元格背景颜色，类型为 string，
  // 第3个元素：最大值对应的单元格背景颜色，类型为 string，
  style: ["#2B2B2B", "#FFFFFF", "#FBD4D0"],
};
```

#### 请求参数

| 名称           | 位置 | 类型   | 必选 | 说明       |
| -------------- | ---- | ------ | ---- | ---------- |
| providerFileID | path | string | 是   | 文件 ID    |
| sheetName      | path | string | 是   | Sheet 名称 |
| body           | body | object | 否   | none       |

#### 返回结果

| 状态码 | 状态码含义                                                      | 说明 | 数据模型 |
| ------ | --------------------------------------------------------------- | ---- | -------- |
| 204    | [No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5) | 成功 | Inline   |

#### 返回数据结构

无

### 设置单元格锁定

POST /sdk/v2/api/files/{fileID}/sheets/{sheetName}/cell-locks

#### 类型定义

```typescript
// Body 类型
export declare type CreateLockData = CreateSheetLockData | CreateRangeLockData;

export declare type CreateSheetLockData = {
  /** 工作表锁定 */
  type: "sheet";
  /** 用户权限 */
  users?: PermissionDict;
  /** 部门权限 */
  departments?: PermissionDict;
  /** 群组权限 */
  groups?: PermissionDict;
  /** 对该锁定的描述 */
  description?: string;
  /** 其他访问者权限，不传默认仅查看 */
  visitorPermission?: PermissionLevel;
  /** 可编辑的细分权限，不传默认无限制 */
  editablePermission?: EditablePermission;
  /** 可查看的细分权限，不传默认无限制 */
  readablePermission?: ReadablePermission;
};

export declare type CreateRangeLockData = Omit<CreateSheetLockData, "type"> & {
  /** 范围锁定 */
  type: "range";
  /** 锁定的范围, [A1:B2, C2:C2] */
  ranges: string[];
};

export declare enum PermissionLevel {
  /** 可以编辑 */
  editable = 0,
  /** 仅查看 */
  readonly = 1,
  /** 禁止查看 */
  forbid = 2,
}

export declare type PermissionDict = {
  [id: number]: PermissionLevel;
};

export declare enum EditablePermission {
  /** 只能录入 */
  InputOnly = 0,
  /** 修改表结构 */
  ModifyStructure = 1,
  /** 应用格式和样式 */
  ModifyStyle = 2,
  /** 全部 */
  All = 3,
}

export declare enum ReadablePermission {
  ReadContentOnly = 0,
  All = 1,
}

/**
 * 权限规则：
 * a. 优先按协作者指定的锁定权限继承
 * b. 设置了部门、群组权限 → 继承最高权限(可编辑> 仅查看 > 禁止查看)
 * c. 只设置了部门或群组权限其中一个时 → 继承部门或群组权限(其中一个)
 * d. 部门、群组权限都没设置时继承「其他访问者的权限」
 * e. 范围锁定优先级高于工作表权限
 * 特殊：表格管理员无视任意非创建副本而生成的锁定，权限均为可编辑。若该锁定是创建副本而来，则管理员权限还是由上述规则计算得到(由于用户创建副本后就默认成了副本表格的管理员，为防止数据泄露，特设定此规则)
 */
export declare type LockData<T extends "sheet" | "range"> = {
  /** 工作表锁定或者范围锁定 */
  type: T;
  /** 创建锁定的用户的id */
  lockerId: number;
  /** 其他访问者权限 */
  visitorPermission: PermissionLevel;
  /** 用户权限 */
  users: PermissionDict;
  /** 部门权限 */
  departments: PermissionDict;
  /** 群组权限 */
  groups: PermissionDict;
  /** 是否来源于创建副本 */
  isFromCreateCopy: boolean;
} & (T extends "range"
  ? {
      /** 锁定的范围 */ ranges: string[];
    }
  : {});
```

#### 请求参数

| 名称      | 位置 | 类型   | 必选 | 说明       |
| --------- | ---- | ------ | ---- | ---------- |
| fileID    | path | string | 是   | 文件名     |
| sheetName | path | string | 是   | Sheet 名称 |
| body      | body | object | 否   | none       |

#### 返回结果

| 状态码 | 状态码含义                                                      | 说明 | 数据模型 |
| ------ | --------------------------------------------------------------- | ---- | -------- |
| 204    | [No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5) | 成功 | Inline   |

## 应用表格

### 查看应用表格历史 {#table-api-base-histories}

查看指定应用表格的历史列表。

**请求地址**

_GET_ https://shimo-domain/sdk/v2/table-api/bases/{fileId}/baseHistories

**HTTP Request Parameters**

| 传参方式 | 参数名        | 类型    | 值示例        | 必选          | 说明                 |
| :------- | :------------ | :------ | :------------ | :------------ | :------------------- |
| Query    | pageSize      | integer | 100           | 否            | 默认 100 最大 100 条 |
| Query    | lastUpdatedAt | integer | 1648622594000 | ms 级别时间戳 |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'php', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request GET 'https://shimo-domain/sdk/v2/table-api/bases/fe143ca1a08e9976/baseHistories?pageSize=100' \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/table-api/bases/fe143ca1a08e9976/baseHistories?pageSize=100")
  .addHeader("X-Shimo-Signature", "your_signature")
  .addHeader("X-Shimo-Token", "your_token")
  .build();
Response response = client.newCall(request).execute();
```

</TabItem>
<TabItem value="go">

```go
package main

import (
  "fmt"
  "strings"
  "net/http"
  "io/ioutil"
)

func main() {
  url := "https://shimo-domain/sdk/v2/table-api/bases/fe143ca1a08e9976/baseHistories?pageSize=100"
  method := "GET"

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, nil)

  if err != nil {
    fmt.Println(err)
    return
  }
  req.Header.Set("X-Shimo-Signature", "your_signature")
  req.Header.Set("X-Shimo-Token", "your_token")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");

var config = {
  method: "get",
  url: "https://shimo-domain/sdk/v2/table-api/bases/fe143ca1a08e9976/baseHistories?pageSize=100",
  headers: {
    "X-Shimo-Signature": "your_signature",
    "X-Shimo-Token": "your_token",
  },
};

axios(config)
  .then(function (response) {
    console.log(JSON.stringify(response.data));
  })
  .catch(function (error) {
    console.log(error);
  });
```

</TabItem>
<TabItem value="php">

```php
<?php
require 'vendor/autoload.php';
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Request;
$client = new Client();
$headers = [
  'X-Shimo-Signature' => 'your_signature',
  'X-Shimo-Token' => 'your_token'
];
$request = new Request('GET', 'https://shimo-domain/sdk/v2/table-api/bases/fe143ca1a08e9976/baseHistories?pageSize=100', $headers);
$res = $client->sendAsync($request)->wait();
echo $res->getBody();



```

</TabItem>
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 200    | 返回成功 |

**HTTP Response Body**

| 字段名                            | 类型    | 值示例                                                                                                                       | 说明                                                         |
| :-------------------------------- | :------ | :--------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------- |
| histories                         | Array   | -                                                                                                                            | 历史列表                                                     |
| histories[0].changes              | Array   | -                                                                                                                            | 变更                                                         |
| histories[0].changes[0].baseRev   | integer | 15                                                                                                                           | 变更初始版本                                                 |
| histories[0].changes[0].rev       | integer | 16                                                                                                                           | 变更版本                                                     |
| histories[0].changes[0].createdAt | string  | 2023-10-17T06:25:30.445Z                                                                                                     | 时间                                                         |
| histories[0].changes[0].data      | string  | [{\"action\":\"DeleteTable\",\"data\":{\"tableGuid\":\"475lZmLk3PL\"},\"extraData\":\"{\\\"tableName\\\":\\\"Sheet3\\\"}\"}] | 变更内容                                                     |
| histories[0].changes[0].userId    | integer | 1000000001                                                                                                                   | 用户 id                                                      |
| histories[0].createdAt            | string  | 2023-10-17T06:25:23.480Z                                                                                                     | 创建时间                                                     |
| histories[0].historyType          | integer | 2                                                                                                                            | 历史类型 1 操作历史，2 编辑历史                              |
| histories[0].historyType          | integer | 2                                                                                                                            | 历史类型 1 操作历史，2 编辑历史                              |
| histories[0].range                | Array   | [14, 16]                                                                                                                     | 表示聚合的历史版本范围 rev 14-16 的历史聚合在这条 history 里 |
| histories[0].updatedAt            | string  | 2023-10-17T06:25:30.446Z                                                                                                     | 更新时间                                                     |
| histories[0].userIds              | Array   | [10000000001]                                                                                                                | 用户 id                                                      |
| users                             | Object  | -                                                                                                                            | 用户信息，key 为 用户 id，value 为 {id,name} object          |

### 查看应用表格版本 {#table-api-base-snapshots}

查看指定应用表格的版本列表。

**请求地址**

_GET_ https://shimo-domain/sdk/v2/table-api/bases/{fileId}/snapshots

**HTTP Request Parameters**

| 传参方式 | 参数名   | 类型    | 值示例                   | 必选 | 说明    |
| :------- | :------- | :------ | :----------------------- | :--- | :------ |
| Query    | pageSize | integer | 10                       | 否   | 默认 10 |
| Query    | lastId   | string  | 652e28e013ed24350da036ad |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'php', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request GET 'https://shimo-domain/sdk/v2/table-api/bases/fe143ca1a08e9976/snapshots?pageSize=10' \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/table-api/bases/fe143ca1a08e9976/snapshots?pageSize=10")
  .addHeader("X-Shimo-Signature", "your_signature")
  .addHeader("X-Shimo-Token", "your_token")
  .build();
Response response = client.newCall(request).execute();
```

</TabItem>
<TabItem value="go">

```go
package main

import (
  "fmt"
  "strings"
  "net/http"
  "io/ioutil"
)

func main() {
  url := "https://shimo-domain/sdk/v2/table-api/bases/fe143ca1a08e9976/snapshots?pageSize=10"
  method := "GET"

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, nil)

  if err != nil {
    fmt.Println(err)
    return
  }
  req.Header.Set("X-Shimo-Signature", "your_signature")
  req.Header.Set("X-Shimo-Token", "your_token")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");

var config = {
  method: "get",
  url: "https://shimo-domain/sdk/v2/table-api/bases/fe143ca1a08e9976/snapshots?pageSize=10",
  headers: {
    "X-Shimo-Signature": "your_signature",
    "X-Shimo-Token": "your_token",
  },
};

axios(config)
  .then(function (response) {
    console.log(JSON.stringify(response.data));
  })
  .catch(function (error) {
    console.log(error);
  });
```

</TabItem>
<TabItem value="php">

```php
<?php
require 'vendor/autoload.php';
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Request;
$client = new Client();
$headers = [
  'X-Shimo-Signature' => 'your_signature',
  'X-Shimo-Token' => 'your_token'
];
$request = new Request('GET', 'https://shimo-domain/sdk/v2/table-api/bases/fe143ca1a08e9976/snapshots?pageSize=10', $headers);
$res = $client->sendAsync($request)->wait();
echo $res->getBody();

```

</TabItem>
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 200    | 返回成功 |

**HTTP Response Body**

| 字段名                           | 类型    | 值示例                   | 说明                                                |
| :------------------------------- | :------ | :----------------------- | :-------------------------------------------------- |
| snapshots                        | Array   |                          | 历史列表                                            |
| snapshots[0].createdAt           | string  | 2023-10-17T06:25:30.445Z | 创建时间                                            |
| histories[0].createdBy           | integer | 1000000001               | 用户 id                                             |
| histories[0].id                  | string  | 652e28e013ed24350da036ad | 版本 id                                             |
| histories[0].name                | string  | 2023/10/17 星期二 14:25  | 版本名称                                            |
| histories[0].explanation         | Object  |                          | 说明页                                              |
| histories[0].explanation.content |         |                          | 说明页内容                                          |
| users                            | Object  |                          | 用户信息，key 为 用户 id，value 为 {id,name} object |

## 传统文档

### 读取传统文档书签内容

读取传统文档指定书签内容。

**请求地址**

_GET_ https://shimo-domain/sdk/v2/shimo-files/{fileId}/documentpro/bookmark_content

**HTTP Request Body**

| 传参方式 | 参数名    | 类型     | 值示例   | 必选 | 说明             |
| :------- | :-------- | :------- | :------- | :--- | :--------------- |
| Body     | bookmarks | []string | ["guid"] | 是   | 数组最大长度 500 |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'php', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request GET 'https://shimo-domain/sdk/v2/shimo-files/{fileId}/documentpro/bookmark_content' \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token'
--data-raw '{"bookmarks":["guid"]}'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\"bookmarks\":[\"guid\"]}");
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/shimo-files/{fileId}/documentpro/bookmark_content")
  .method("GET", body)
  .addHeader("Content-Type", "application/json")
  .addHeader("X-Shimo-Signature", "your_signature")
  .addHeader("X-Shimo-Token", "your_token")
  .build();
Response response = client.newCall(request).execute();
```

</TabItem>
<TabItem value="go">

```go
package main

import (
  "fmt"
  "strings"
  "net/http"
  "io/ioutil"
)

func main() {
  url := "https://shimo-domain/sdk/v2/shimo-files/{fileId}/documentpro/bookmark_content"
  method := "GET"

  payload := strings.NewReader(`{
    "bookmarks": ["guid"]
  }`)

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
    return
  }
  req.Header.Set("Content-Type", "application/json")
  req.Header.Set("X-Shimo-Signature", "your_signature")
  req.Header.Set("X-Shimo-Token", "your_token")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");
var data = JSON.stringify({
  bookmarks: ["guid"],
});

var config = {
  method: "get",
  url: "https://shimo-domain/sdk/v2/shimo-files/{fileId}/documentpro/bookmark_content",
  headers: {
    "Content-Type": "application/json",
    "X-Shimo-Signature": "your_signature",
    "X-Shimo-Token": "your_token",
  },
  data: data,
};

axios(config)
  .then(function (response) {
    console.log(JSON.stringify(response.data));
  })
  .catch(function (error) {
    console.log(error);
  });
```

</TabItem>
<TabItem value="php">

```php
<?php
require 'vendor/autoload.php';
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Request;
$client = new Client();
$headers = [
  'X-Shimo-Signature' => 'your_signature',
  'X-Shimo-Token' => 'your_token',
  'Content-Type' => 'application/x-www-form-urlencoded'
];
$options = [
'form_params' => [
  '{"bookmarks":["guid"]}' => ''
]];
$request = new Request('GET', 'https://shimo-domain/sdk/v2/shimo-files/{fileId}/documentpro/bookmark_content', $headers);
$res = $client->sendAsync($request, $options)->wait();
echo $res->getBody();

```

</TabItem>
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 200    | 返回成功 |

**HTTP Response Body**

| 字段名           | 类型   | 值示例   | 说明     |
| :--------------- | :----- | :------- | :------- |
| data             | Array  | -        | 列表     |
| data[0].bookmark | string | 测试     | 书签名称 |
| data[0].content  | string | 学习网站 | 书签内容 |

### 替换传统文档书签内容{#replace-bookmark}

替换传统文档指定书签部分内容。

**请求地址**

_PUT_ https://shimo-domain/sdk/v2/shimo-files/{fileId}/documentpro/bookmark_content

**HTTP Request Body**

| 传参方式 | 参数名                   | 类型   | 值示例 | 必选 | 说明                     |
| :------- | :----------------------- | :----- | :----- | :--- | :----------------------- |
| Body     | replacements             | Array  | -      | 是   | 数组最大长度 50          |
| Body     | replacements[0].bookmark | string | -      | 是   | 字符串最大长度 500       |
| Body     | replacements[0].type     | string | text   | 是   | 书签类型 text / document |
| Body     | replacements[0].value    | string | -      | 是   | 字符串最大长度 5242880   |

:::caution 说明

- 书签类型 text 则 value 为替换的文本内容
- 书签类型 document 则 value 为 {fileId}

:::

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'php', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request GET 'https://shimo-domain/sdk/v2/shimo-files/{fileId}/documentpro/bookmark_content' \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token'
--data-raw '{"replacements": [{"bookmark":"bookmark","type":"text","value":"bookmark value"}]}'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\"replacements\": [{\"bookmark\":\"bookmark\",\"type\":\"text\",\"value\":\"bookmark value\"}]}");
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/shimo-files/{fileId}/documentpro/bookmark_content")
  .method("PUT", body)
  .addHeader("Content-Type", "application/json")
  .addHeader("X-Shimo-Signature", "your_signature")
  .addHeader("X-Shimo-Token", "your_token")
  .build();
Response response = client.newCall(request).execute();
```

</TabItem>
<TabItem value="go">

```go
package main

import (
  "fmt"
  "strings"
  "net/http"
  "io/ioutil"
)

func main() {
  url := "https://shimo-domain/sdk/v2/shimo-files/{fileId}/documentpro/bookmark_content"
  method := "PUT"

  payload := strings.NewReader(`{
    "bookmarks": {"replacements": [{"bookmark":"bookmark","type":"text","value":"bookmark value"}]}
  }`)

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
    return
  }
  req.Header.Set("Content-Type", "application/json")
  req.Header.Set("X-Shimo-Signature", "your_signature")
  req.Header.Set("X-Shimo-Token", "your_token")

  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := ioutil.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");
var data = JSON.stringify({
  bookmarks: {
    replacements: [
      { bookmark: "bookmark", type: "text", value: "bookmark value" },
    ],
  },
});

var config = {
  method: "put",
  url: "https://shimo-domain/sdk/v2/shimo-files/{fileId}/documentpro/bookmark_content",
  headers: {
    "Content-Type": "application/json",
    "X-Shimo-Signature": "your_signature",
    "X-Shimo-Token": "your_token",
  },
  data: data,
};

axios(config)
  .then(function (response) {
    console.log(JSON.stringify(response.data));
  })
  .catch(function (error) {
    console.log(error);
  });
```

</TabItem>
<TabItem value="php">

```php
<?php
require 'vendor/autoload.php';
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Request;
$client = new Client();
$headers = [
  'X-Shimo-Signature' => 'your_signature',
  'X-Shimo-Token' => 'your_token',
  'Content-Type' => 'application/x-www-form-urlencoded'
];
$options = [
'form_params' => [
  '{"replacements": [{"bookmark":"bookmark","type":"text","value":"bookmark value"}]}' => ''
]];
$request = new Request('GET', 'https://shimo-domain/sdk/v2/shimo-files/{fileId}/documentpro/bookmark_content', $headers);
$res = $client->sendAsync($request, $options)->wait();
echo $res->getBody();

```

</TabItem>
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 200    | 返回成功 |
