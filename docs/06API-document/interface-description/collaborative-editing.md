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
{ label: 'PHP', value: 'php', },
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
{ label: 'PHP', value: 'php', },
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
{ label: 'PHP', value: 'php', },
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
{ label: 'PHP', value: 'php', },
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
{ label: 'PHP', value: 'php', },
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
{ label: 'PHP', value: 'php', },
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
中日韩文字，每个字符都当做一个独立的单词来计数，比如对于“你好”，是两个单词：”你“ 和 ”好“
英文和其它符号，只会由空格来区分，比如对于“Hello world”，也只有两个单词：“Hello” 和 “world”。

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
{ label: 'PHP', value: 'php', },
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
{ label: 'PHP', value: 'php', },
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
{ label: 'PHP', value: 'php', },
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
  - `工作表1`
  - `工作表1!A1`
  - `工作表1!A1:C1`
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
| Query    | range  | string | 工作表1!A1:C3   | 是   | 请参考 [range](#range) 参数说明。                                                                                                                |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'PHP', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request GET 'https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values?range=工作表1!A1:C3' \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values?range=工作表1!A1:C3")
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
  url: "https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values?range=工作表1!A1:C3",
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
$request = new Request('GET', 'https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values?range=工作表1!A1:C3', $headers);
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
| range    | string | 工作表1!A1:C3 | 请参考 [range](#range) 参数说明。       |
| resource | object | -              | 请参考 [resource](#resource) 参数说明。 |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'PHP', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request POST 'https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values' \
--header 'Content-Type: application/json' \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token' \
--data-raw '{
    "range": "工作表1!A1:C3",
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
RequestBody body = RequestBody.create(mediaType, "{\n    \"range\": \"工作表1!A1:C3\",\n    \"resource\": {\n        \"values\": [\n            [\"第一行第一列的值\",\"第一行第二列的值\"],\n            [\"第二行第一列的值\",\"第二行第二列的值\"]\n        ]\n    }\n}");
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
    "range": "工作表1!A1:C3",
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
  range: "工作表1!A1:C3",
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
  "range": "工作表1!A1:C3",
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
a. 不指定单元格范围，例如工作表1：会遍历整个表格，找到最后一个有数据的行
b. 指定单元格范围，例如 工作表1!C5:E8：会找到起始单元格 C5 所在的行，从该行向下遍历，找到最后一个连续的有数据行向下插入 N 行，填充 values，其中 N = values.length
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
| range    | string | 工作表1!A1:C3 | 请参考 [range](#range) 参数说明。       |
| resource | object | -              | 请参考 [resource](#resource) 参数说明。 |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'PHP', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request PUT 'https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values' \
--header 'Content-Type: application/json' \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token' \
--data-raw '{
    "range": "工作表1!A1:C3",
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
RequestBody body = RequestBody.create(mediaType, "{\n    \"range\": \"工作表1!A1:C3\",\n    \"resource\": {\n        \"values\": [\n            [\"第一行第一列追加文本\",\"第一行第二列追加文本\"],\n            [\"第二行第一列追加文本\",\"第二行第二列追加文本\"]\n        ]\n    }\n}");
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
    "range": "工作表1!A1:C3",
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
  range: "工作表1!A1:C3",
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
  "range": "工作表1!A1:C3",
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
| Path     | sheetName | string | 工作表1         | 是   | 表格中工作表的名称                                                                                                                               |
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
{ label: 'PHP', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request DELETE 'https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/工作表1/rows/0?count=2' \
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
  .url("https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/工作表1/rows/0?count=2")
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
  url := "https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/工作表1/rows/0?count=2"
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
  url: "https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/工作表1/rows/0?count=2",
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
$request = new Request('DELETE', 'https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/工作表1/rows/0?count=2', $headers);
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

_POST_ https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/

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
{ label: 'PHP', value: 'php', },
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

## 传统文档

### 读取传统文档书签内容

读取传统文档指定书签内容。

**请求地址**

_GET_ https://shimo-domain/sdk/v2/api/files/{fileId}/documentpro/bookmark_content

**HTTP Request Query**

| 传参方式 | 参数名    | 类型     | 值示例   | 必选 | 说明             |
| :------- | :-------- | :------- | :------- | :--- | :--------------- |
| Query     | bookmarks | []string | ["guid"] | 是   | 数组最大长度 500 |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
{ label: 'PHP', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request GET 'https://shimo-domain/sdk/v2/api/files/{fileId}/documentpro/bookmark_content?bookmarks=1&bookmarks=2' \
--header 'X-Shimo-Signature: your_signature' \
--header 'X-Shimo-Token: your_token'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/files/{fileId}/documentpro/bookmark_content?bookmarks=1&bookmarks=2")
  .method("GET")
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
  url := "https://shimo-domain/sdk/v2/api/files/{fileId}/documentpro/bookmark_content?bookmarks=1&bookmarks=2"
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
  method: "get",
  url: "https://shimo-domain/sdk/v2/api/files/{fileId}/documentpro/bookmark_content?bookmarks=1&bookmarks=2",
  headers: {
    "Content-Type": "application/json",
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
$options = [];
$request = new Request('GET', 'https://shimo-domain/sdk/v2/api/files/{fileId}/documentpro/bookmark_content?bookmarks=1&bookmarks=2', $headers);
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

_PUT_ https://shimo-domain/sdk/v2/api/files/{fileId}/documentpro/bookmark_content

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
{ label: 'PHP', value: 'php', },
]
}>
<TabItem value="curl">

```bash
curl --request GET 'https://shimo-domain/sdk/v2/api/files/{fileId}/documentpro/bookmark_content' \
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
  .url("https://shimo-domain/sdk/v2/api/files/{fileId}/documentpro/bookmark_content")
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
  url := "https://shimo-domain/sdk/v2/api/files/{fileId}/documentpro/bookmark_content"
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
  url: "https://shimo-domain/sdk/v2/api/files/{fileId}/documentpro/bookmark_content",
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
$request = new Request('GET', 'https://shimo-domain/sdk/v2/api/files/{fileId}/documentpro/bookmark_content', $headers);
$res = $client->sendAsync($request, $options)->wait();
echo $res->getBody();

```

</TabItem>
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
|:----| :------- |
| 204 | 返回成功 |
