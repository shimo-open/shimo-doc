---
sidebar_position: 5
description: >-
  石墨SDK服务器域名指的是石墨 SDK 服务器,用于获取app的详细信息,包括名称、可使用文件类型、已激活用户数、用户总数、席位数、 license
  有效时间和回调地址信息。 更新app回调 adres,创建指定类型的协同文档,指定创建文件时的语言信息,接入服务商的文件的唯一
  ID,删除协同文檔,删除指定类型の协同文稿。
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# 石墨 API 列表

:::caution 注意
⚠️ 如果没有特别说明，所有请求均为 `HTTP`，请求头和返回头的 `Content-Type` 均为 `application/json`。

⚠️ `shimo-domain` 指石墨 SDK 服务器域名，请联系石墨获取此域名。

⚠️ 路由中的参数值如果有特殊符号，建议将值进行转义，JavaScript 转义方法请参考 [`MDN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent?fileGuid=Wr3DVn8lO4HE2kJQ)。
:::

## 账号与应用管理

### 获取 app 详情 {#get-app-detail}

获取 app 详细信息，包括 app 名称、可使用文件类型、已激活用户数、用户总数、席位数、license 有效时间、回调地址信息。

:::caution 提醒

由于 app API 的特殊性，为了安全考虑，需要您在[生成签名](./resources.md#签名)时附加上 `"scope": "license"` 字段

:::

**请求地址**

_GET_ https://shimo-domain/sdk/v2/api/license/apps/{appId}

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                      |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :-------------------------------------------------------------------------------------------------------- |
| Path     | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                              |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                              |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的 token ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递 |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                   |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
]
}>
<TabItem value="curl">

```bash
curl "https://shimo-domain/sdk/v2/api/license/apps/yourShimoAppId?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString"
```

</TabItem>
<TabItem value="java">

```java
import java.io.IOException;
import org.apache.http.client.fluent.*;

public class SendRequest
{
  public static void main(String[] args) {
    sendRequest();
  }

  private static void sendRequest() {

    // 获取 app 详情 (GET )

    try {

      // Create request
      Content content = Request.Get("https://shimo-domain/sdk/v2/api/license/apps/yourShimoAppId?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString")

      // Fetch request and return content
      .execute().returnContent();

      // Print content
      System.out.println(content);
    }
    catch (IOException e) { System.out.println(e); }
  }
}
```

</TabItem>
<TabItem value="go">

```go
package main

import (
	"fmt"
	"io/ioutil"
	"net/http"
)

func sendApp() {
	// 获取 app 详情 (GET https://shimo-domain/sdk/v2/api/license/apps/yourShimoAppId?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString)

	// Create client
	client := &http.Client{}

	// Create request
	req, err := http.NewRequest("GET", "https://shimo-domain/sdk/v2/api/license/apps/yourShimoAppId?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString", nil)

	parseFormErr := req.ParseForm()
	if parseFormErr != nil {
	  fmt.Println(parseFormErr)
	}

	// Fetch Request
	resp, err := client.Do(req)

	if err != nil {
		fmt.Println("Failure : ", err)
	}

	// Read Response Body
	respBody, _ := ioutil.ReadAll(resp.Body)

	// Display Results
	fmt.Println("response Status : ", resp.Status)
	fmt.Println("response Headers : ", resp.Header)
	fmt.Println("response Body : ", string(respBody))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");

axios({
  method: "GET",
  url: "https://shimo-domain/sdk/v2/api/license/apps/yourShimoAppId",
  params: {
    appId: "yourShimoAppId",
    signature: "yourSignatureString",
    token: "yourTokenString",
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
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 200    | 返回成功 |

**HTTP Response Body**

| 字段名             | 类型   | 值示例                                                          | 说明                                                                            |
| :----------------- | :----- | :-------------------------------------------------------------- | :------------------------------------------------------------------------------ |
| appName            | string | your app name                                                   | 应用名                                                                          |
| availableFileTypes | Array  | ["document","documentPro","spreadsheet","presentation","table"] | 可用石墨套件列表                                                                |
| activatedUserCount | number | 43                                                              | 已激活席位用户数                                                                |
| userCount          | number | 50                                                              | 用户数总数，包含`已激活`、`已禁用`、`未使用` 用户总数，仅 `已激活` 数量占用席位 |
| memberLimit        | number | 99                                                              | license 席位限制用户数，即 `已激活` 用户数量最大限制                            |
| validFrom          | string | 2020-12-31T16:00:00Z                                            | license 生效时间                                                                |
| validUntil         | string | 2022-12-30T16:00:00Z                                            | license 有效期截止时间                                                          |
| endpointUrl        | string | http://your-domain                                              | 服务商回调地址                                                                  |

Example

```json
{
  "appName": "your app name",
  "availableFileTypes": [
    "document",
    "documentPro",
    "spreadsheet",
    "presentation",
    "table"
  ],
  "activatedUserCount": 43,
  "userCount": 50,
  "memberLimit": 99,
  "validFrom": "2020-12-31T16:00:00Z",
  "validUntil": "2022-12-30T16:00:00Z",
  "endpointUrl": "http://your-domain"
}
```

### 更新 app 回调地址 {#update-app-endpoint-url}

用于更新 app 对应服务商回调地址

:::caution 提醒

由于 app API 的特殊性，为了安全考虑，需要您在[生成签名](./resources.md#签名)时附加上 `"scope": "license"` 字段

:::

**请求地址**

_PUT_ https://shimo-domain/sdk/v2/api/license/apps/{appId}/endpoint-url

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                      |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :-------------------------------------------------------------------------------------------------------- |
| Path     | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                              |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                              |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的 token ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递 |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                   |

**HTTP Request Body**

| 字段名 | 类型   | 值示例             | 必选 | 说明                     |
| :----- | :----- | :----------------- | :--- | :----------------------- |
| url    | string | http://your-domain | 是   | 需要更新的服务商回调地址 |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
]
}>
<TabItem value="curl">

```bash
curl -X "PUT" "https://shimo-domain/sdk/v2/api/license/apps/yourShimoAppId/endpoint-url?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString" \
     -H 'Content-Type: application/json' \
     -d $'{
  "url": "http://your-domain"
}'
```

</TabItem>
<TabItem value="java">

```java
import java.io.IOException;
import org.apache.http.client.fluent.*;
import org.apache.http.entity.ContentType;

public class SendRequest
{
  public static void main(String[] args) {
    sendRequest();
  }

  private static void sendRequest() {

    // 更新 app 回调地址 (PUT )

    try {

      // Create request
      Content content = Request.Put("https://shimo-domain/sdk/v2/api/license/apps/yourShimoAppId/endpoint-url?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString")

      // Add headers
      .addHeader("Content-Type", "application/json")

      // Add body
      .bodyString("{\"url\": \"http://your-domain\"}", ContentType.APPLICATION_JSON)

      // Fetch request and return content
      .execute().returnContent();

      // Print content
      System.out.println(content);
    }
    catch (IOException e) { System.out.println(e); }
  }
}
```

</TabItem>
<TabItem value="go">

```go
package main

import (
	"fmt"
	"io/ioutil"
	"net/http"
	"bytes"
)

func sendApp() {
	// 更新 app 回调地址 (PUT https://shimo-domain/sdk/v2/api/license/apps/yourShimoAppId/endpoint-url?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString)

	json := []byte(`{"url": "http://your-domain"}`)
	body := bytes.NewBuffer(json)

	// Create client
	client := &http.Client{}

	// Create request
	req, err := http.NewRequest("PUT", "https://shimo-domain/sdk/v2/api/license/apps/yourShimoAppId/endpoint-url?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString", body)

	// Headers
	req.Header.Add("Content-Type", "application/json")

	parseFormErr := req.ParseForm()
	if parseFormErr != nil {
	  fmt.Println(parseFormErr)
	}

	// Fetch Request
	resp, err := client.Do(req)

	if err != nil {
		fmt.Println("Failure : ", err)
	}

	// Read Response Body
	respBody, _ := ioutil.ReadAll(resp.Body)

	// Display Results
	fmt.Println("response Status : ", resp.Status)
	fmt.Println("response Headers : ", resp.Header)
	fmt.Println("response Body : ", string(respBody))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");
axios({
  method: "PUT",
  url: "https://shimo-domain/sdk/v2/api/license/apps/yourShimoAppId/endpoint-url",
  params: {
    appId: "yourShimoAppId",
    signature: "yourSignatureString",
    token: "yourTokenString",
  },
  headers: {
    "Content-Type": "application/json",
  },
  data: {
    url: "http://your-domain",
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
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 204    | 创建成功 |

**HTTP Response Body**

空

## 协同文档

### 创建协同文档 {#create-collab-file}

创建指定类型的协同文档

**请求地址**

_POST_ https://shimo-domain/sdk/v2/api/files

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                                               |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :--------------------------------------------------------------------------------------------------------------------------------- |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                                                       |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的 token ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递                          |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                                            |
| Query    | lang      | string | en                                   | 否   | 可选。默认值为服务端默认语言设置，通常默认为 `zh-CN` 。指定创建文件时的语言信息，可选值：`zh-CN`(简体中文)、`en`(英文)、`ja`(日文) |

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
]
}>
<TabItem value="curl">

```bash
curl --location --request POST 'https://shimo-domain/sdk/v2/api/files?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString' \
--header 'Content-Type: application/json' \
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
  .url("https://shimo-domain/sdk/v2/api/files?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString")
  .method("POST", body)
  .addHeader("Content-Type", "application/json")
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

  url := "https://shimo-domain/sdk/v2/api/files?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString"
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
  req.Header.Add("Content-Type", "application/json")

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
  url: "https://shimo-domain/sdk/v2/api/files?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString",
  headers: {
    "Content-Type": "application/json",
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
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 204    | 创建成功 |

**HTTP Response Body**

空

**支持的文档类型**

| 文件类型 | 类型值       |
| :------- | :----------- |
| 轻文档   | document     |
| 表格     | spreadsheet  |
| 传统文档 | documentPro  |
| 幻灯片   | presentation |
| 应用表格 | table        |

:::caution 说明

除 `轻文档`、`传统文档`、`表格` 和 `幻灯片`，其他类型为非标准功能 (如 `应用表格`) ，详询石墨客服。

:::

:::info 备注

除 `轻文档`、`应用表格` 之外，其他文件类型均高度兼容 Office 格式

:::

**创建后加载编辑器**

参考「[协同文档-接入流程](./roll.md#接入流程)」步骤进行使用石墨 JSSDK 按照参数说明进行初始化。

### 创建协同文档副本

创建指定协同文档的副本

**请求地址**

_POST_ https://shimo-domain/sdk/v2/shimo-files/{fileId}/copy

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                      |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :-------------------------------------------------------------------------------------------------------- |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                              |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的 token ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递 |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                   |

**HTTP Request Body**

| 字段名 | 类型   | 值示例     | 必选 | 说明                                                               |
| :----- | :----- | :--------- | :--- | :----------------------------------------------------------------- |
| fileId | string | fileid1001 | 是   | 新文件 ID，接入服务商的文件的唯一 ID，必须为不超过 64 长度的字符串 |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
]
}>
<TabItem value="curl">

```bash
curl --location --request POST 'https://shimo-domain/sdk/v2/api/files/e0f238d1-8c10-448c-bf69-cd315051c095/copy?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString' \
--header 'Content-Type: application/json' \
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
  .url("https://shimo-domain/sdk/v2/api/files/e0f238d1-8c10-448c-bf69-cd315051c095/copy?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString")
  .method("POST", body)
  .addHeader("Content-Type", "application/json")
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

  url := "https://shimo-domain/sdk/v2/api/files/e0f238d1-8c10-448c-bf69-cd315051c095/copy?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString"
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
  req.Header.Add("Content-Type", "application/json")

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
  url: "https://shimo-domain/sdk/v2/api/files/e0f238d1-8c10-448c-bf69-cd315051c095/copy?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString",
  headers: {
    "Content-Type": "application/json",
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

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                      |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :-------------------------------------------------------------------------------------------------------- |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                              |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的 token ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递 |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                   |

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
]
}>
<TabItem value="curl">

```bash
curl --location --request DELETE 'https://shimo-domain/sdk/v2/api/files/e0f238d1-8c10-448c-bf69-cd315051c095?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "");
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/files/5c80a323-47d3-4e69-9842-1682d0ca604c?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString")
  .method("DELETE", body)
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

  url := "https://shimo-domain/sdk/v2/api/files/1785c0b3-6157-499c-a493-60a034fe03c2?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString"
  method := "DELETE"

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, nil)

  if err != nil {
    fmt.Println(err)
    return
  }
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
  url: "https://shimo-domain/sdk/v2/api/files/2ef0f99b-6e1b-48c1-bb61-b7c2eac0914c?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString",
  headers: {},
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
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 204    | 操作成功 |

**HTTP Response Body**

空

### 获取历史列表 {#doc-sidebar-info}

获取文件侧边栏历史列表信息

**请求地址**

_GET_ https://shimo-domain/sdk/v2/shimo-files/{fileId}/doc-sidebar-info

**HTTP Request Parameters**

| 传参方式 | 参数名      | 类型   | 值示例                               | 必选 | 说明                                                                                                                                             |
| :------- | :---------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId      | string | qeK4Xdxvxg8jF5gz                     | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |
| Query    | appId       | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                                                                     |
| Query    | token       | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的 token ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递                                        |
| Query    | signature   | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                                                          |
| Query    | pageSize    | number | 10                                   | 否   | 每一页返回的条数，推荐值 `10`、`20` ，设的过大将会影响响应时间。默认值为 `10`                                                                    |
| Query    | count       | number | 0                                    | 否   | 当前页需要跳过的记录数，可通过 `count = (page - 1) * pageSize` 求得。默认值为 `0`                                                                |
| Query    | historyType | number | 1                                    | 否   | 可选值：`1` 操作历史 （如锁定单元格此类未实际编写内容的修改产生的操作历史），`2` 编辑历史。未传时，默认返回所有类型                              |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
]
}>
<TabItem value="curl">

```bash
curl --location --request GET 'https://shimo-domain/sdk/v2/shimo-files/<fileId>/doc-sidebar-info?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/shimo-files/<fileId>/doc-sidebar-info?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString")
  .method("GET", null)
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

  url := "https://shimo-domain/sdk/v2/shimo-files/%3CfileId%3E/doc-sidebar-info?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString"
  method := "GET"

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, nil)

  if err != nil {
    fmt.Println(err)
    return
  }
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
  url: "https://shimo-domain/sdk/v2/shimo-files/<fileId>/doc-sidebar-info?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString",
  headers: {},
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
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 200    | 返回成功 |

**HTTP Response Body**

| 字段名                   | 类型    | 值示例                   | 说明                                           |
| :----------------------- | :------ | :----------------------- | :--------------------------------------------- |
| histories                | array   |                          | 侧边栏历史数组                                 |
| histories[i].content     | string  |                          | 协作文件格式数据                               |
| histories[i].createdAt   | string  | 2021-06-07T06:12:24Z     | 本条侧边栏历史创建时间                         |
| histories[i].historyType | number  | 2                        | 侧边栏历史类型，1 为操作历史，2 为编辑产生     |
| histories[i].id          | string  | 60bdb8c847a7850006bf12c1 | 侧边栏历史 ID                                  |
| histories[i].name        | string  | 历史 1                   | 侧边栏历史名称                                 |
| histories[i].updatedAt   | string  | 2021-06-07T06:12:24Z     | 侧边栏历史最后更新时间                         |
| histories[i].userId      | string  | user123,user134          | 服务商用户 ID，可能有多个，以英文逗号 "," 分隔 |
| isLastPage               | boolean |                          | 是否最后一页                                   |
| limit                    | number  | 100                      | 分页大小                                       |
| users                    | object  | { "user123": "用户 A" }  | 服务商用户 ID 对应用户名映射                   |

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

#### 历史类型说明

##### 操作历史 (historyType = 1)

为手动操作而通常不造成文件内容文字等发生变化的记录，此类历史的类型及上下文信息存于 content 字段中，格式为 JSON 字符串形式。

已知的操作如下, JSON 数据为 `histories[i].content` 字段 JSON.parse 后的结果：

###### createRevision 创建版本

```json
{
  "action": "createRevision"
}
```

###### renameRevision 重命名版本

```json
{
  "action": "renameRevision",
  "before": "修改前的版本名称",
  "after": "修改后的版本名称"
}
```

###### deleteRevision 删除版本

```json
{
  "action": "deleteRevision",
  "label": "被删除的版本名称"
}
```

###### lock_cell 锁定单元格

```json
{
  "action": "lock_cell",
  "range": "E14:E14",
  "name": "工作表1"
}
```

###### unlock_cell 解锁单元格

```json
{
  "action": "unlock_cell",
  "range": ["E14:E14"],
  "name": "工作表1"
}
```

###### lock_sheet 锁定工作表

```json
{
  "action": "lock_sheet",
  "name": "工作表1"
}
```

###### unlock_sheet 解锁工作表

```json
{
  "action": "unlock_sheet",
  "name": "工作表1"
}
```

###### update_lock_cell 更新单元格锁定

```json
{
  "action": "update_lock_cell",
  "range": ["E17:E17"],
  "name": "工作表1"
}
```

##### 编辑历史 (historyType = 2)

表示一个或多个用户在一段时间内对文件进行了编辑

### 获取版本列表

获取文件版本列表信息

**请求地址**

_GET_ https://shimo-domain/sdk/v2/shimo-files/{fileId}/revisions

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                                                                     |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的 token ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递                                        |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                                                          |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
]
}>
<TabItem value="curl">

```bash
curl --location --request GET 'https://shimo-domain/sdk/v2/shimo-files/<fileId>/revisions?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/shimo-files/<fileId>/revisions?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString")
  .method("GET", null)
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

  url := "https://shimo-domain/sdk/v2/shimo-files/%3CfileId%3E/revisions?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString"
  method := "GET"

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, nil)

  if err != nil {
    fmt.Println(err)
    return
  }
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
  url: "https://shimo-domain/sdk/v2/shimo-files/<fileId>/revisions?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString",
  headers: {},
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

获取指定文件的纯文本形式内容

**请求地址**

_GET_ https://shimo-domain/sdk/v2/shimo-files/{fileId}/plain-text

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                                                                     |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的 token ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递                                        |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                                                          |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
]
}>
<TabItem value="curl">

```bash
curl --location --request GET 'https://shimo-domain/sdk/v2/shimo-files/<fileId>/plain-text?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/shimo-files/<fileId>/plain-text?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString")
  .method("GET", null)
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

  url := "https://shimo-domain/sdk/v2/shimo-files/%3CfileId%3E/plain-text?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString"
  method := "GET"

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, nil)

  if err != nil {
    fmt.Println(err)
    return
  }
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
  url: "https://shimo-domain/sdk/v2/shimo-files/<fileId>/plain-text?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString",
  headers: {},
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

### 获取文件内容中所有的 at 人信息列表

获取文件内容中所有的 at 人信息列表

支持文件类型：

- `document` 轻文档
- `documentPro` 传统文档
- `spreadsheet` 表格

**请求地址**

_GET_ https://shimo-domain/sdk/v2/shimo-files/{fileId}/mention-at-list

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                                                                     |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的 token ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递                                        |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                                                          |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
]
}>
<TabItem value="curl">

```bash
curl "https://shimo-domain/sdk/v2/shimo-files/ad8ed1afa8172d91/mention-at-list?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString"
```

</TabItem>
<TabItem value="java">

```java
import java.io.IOException;
import org.apache.http.client.fluent.*;

public class SendRequest
{
  public static void main(String[] args) {
    sendRequest();
  }

  private static void sendRequest() {
    try {

      // Create request
      Content content = Request.Get("https://shimo-domain/sdk/v2/shimo-files/ad8ed1afa8172d91/mention-at-list?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString")

      // Fetch request and return content
      .execute().returnContent();

      // Print content
      System.out.println(content);
    }
    catch (IOException e) { System.out.println(e); }
  }
}
```

</TabItem>
<TabItem value="go">

```go
package main

import (
	"fmt"
	"io/ioutil"
	"net/http"
)

func sendAt() {
	// Create client
	client := &http.Client{}

	// Create request
	req, err := http.NewRequest("GET", "https://shimo-domain/sdk/v2/shimo-files/ad8ed1afa8172d91/mention-at-list?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString", nil)

	parseFormErr := req.ParseForm()
	if parseFormErr != nil {
	  fmt.Println(parseFormErr)
	}

	// Fetch Request
	resp, err := client.Do(req)

	if err != nil {
		fmt.Println("Failure : ", err)
	}

	// Read Response Body
	respBody, _ := ioutil.ReadAll(resp.Body)

	// Display Results
	fmt.Println("response Status : ", resp.Status)
	fmt.Println("response Headers : ", resp.Header)
	fmt.Println("response Body : ", string(respBody))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");

axios({
  method: "GET",
  url: "https://shimo-domain/sdk/v2/shimo-files/ad8ed1afa8172d91/mention-at-list",
  params: {
    appId: "yourShimoAppId",
    signature: "yourSignatureString",
    token: "yourTokenString",
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
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 200    | 返回成功 |

**HTTP Response Body**

| 字段名                  | 类型   | 值示例           | 说明                                               |
| :---------------------- | :----- | :--------------- | :------------------------------------------------- |
| mentionAtList           | Array  |                  | 根据指定文件 ID 获取的石墨文件中的所有 at 信息列表 |
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

获取文件内中的评论总数

支持文件类型：

- `spreadsheet` 表格

**请求地址**

_GET_ https://shimo-domain/sdk/v2/shimo-files/{fileId}/comment-count

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                                                                     |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的 token ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递                                        |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                                                          |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
]
}>
<TabItem value="curl">

```bash
curl "https://shimo-domain/sdk/v2/shimo-files/fe143ca1a08e9976/comment-count?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString"
```

</TabItem>
<TabItem value="java">

```java
import java.io.IOException;
import org.apache.http.client.fluent.*;

public class SendRequest
{
  public static void main(String[] args) {
    sendRequest();
  }

  private static void sendRequest() {

    // 获取文件内容中的评论数 (GET )

    try {

      // Create request
      Content content = Request.Get("https://shimo-domain/sdk/v2/shimo-files/fe143ca1a08e9976/comment-count?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString")

      // Fetch request and return content
      .execute().returnContent();

      // Print content
      System.out.println(content);
    }
    catch (IOException e) { System.out.println(e); }
  }
}
```

</TabItem>
<TabItem value="go">

```go
package main

import (
	"fmt"
	"io/ioutil"
	"net/http"
)

func send() {
	// 获取文件内容中的评论数 (GET https://shimo-domain/sdk/v2/shimo-files/fe143ca1a08e9976/comment-count?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString)

	// Create client
	client := &http.Client{}

	// Create request
	req, err := http.NewRequest("GET", "https://shimo-domain/sdk/v2/shimo-files/fe143ca1a08e9976/comment-count?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString", nil)

	parseFormErr := req.ParseForm()
	if parseFormErr != nil {
	  fmt.Println(parseFormErr)
	}

	// Fetch Request
	resp, err := client.Do(req)

	if err != nil {
		fmt.Println("Failure : ", err)
	}

	// Read Response Body
	respBody, _ := ioutil.ReadAll(resp.Body)

	// Display Results
	fmt.Println("response Status : ", resp.Status)
	fmt.Println("response Headers : ", resp.Header)
	fmt.Println("response Body : ", string(respBody))
}
```

</TabItem>
<TabItem value="js">

```js
var axios = require("axios");

axios({
  method: "GET",
  url: "https://shimo-domain/sdk/v2/shimo-files/fe143ca1a08e9976/comment-count",
  params: {
    appId: "yourShimoAppId",
    signature: "yourSignatureString",
    token: "yourTokenString",
  },
});
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

## 文件预览

:::caution 注意
⚠️ 此功能不支持编辑文档，如果要编辑文档，请参考 [`协同文档`](#协同文档)。
:::

### 创建预览

同步创建预览任务, 如果失败返回失败的原因

**请求地址**

_POST_ https://shimo-domain/sdk/v2/api/cloud-files/{fileID}/create

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 示例值                               | 必选 | 说明                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的 token ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递                                        |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                                                          |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
]
}>
<TabItem value="curl">

```bash
curl --location -g --request POST 'https://shimo-domain/sdk/v2/api/cloud-files/{{fileID}}/create?signature=yourSignatureString&token=yourTokenString'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("text/plain");
RequestBody body = RequestBody.create(mediaType, "");
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/cloud-files/{{fileID}}/create?signature=yourSignatureString&token=yourTokenString")
  .method("POST", body)
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

  url := "https://shimo-domain/sdk/v2/api/cloud-files/{{fileID}}/create?signature=yourSignatureString&token=yourTokenString"
  method := "POST"

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, nil)

  if err != nil {
    fmt.Println(err)
    return
  }
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
  method: "post",
  url: "https://shimo-domain/sdk/v2/api/cloud-files/{{fileID}}/create?signature=yourSignatureString&token=yourTokenString",
  headers: {},
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
</Tabs>

**HTTP Response Body**

| 字段名  | 类型   | 值示例             | 说明                                                        |
| :------ | :----- | :----------------- | :---------------------------------------------------------- |
| code    | string | 90028              | 创建预览结果状态码，空字符串代表创建成功， 非空代表创建失败 |
| message | string | file is converting | 创建失败错误信息                                            |

**Example**

```json
// 创建失败
{
  "code": "90042",
  "message": "文件格式不正确"
}
// 创建成功
{
  "code": "",
  "message": ""
}
```

### 访问预览

用于在浏览器上渲染出预览的页面， 需要嵌入 Iframe 使用

> 对于同一个文件， 第一次调用该接口时， 后台会异步调用「[创建预览](#创建预览)」接口创建任务， 创建成功后才会渲染出预览页面。 如果需要加快第一次打开文件的速度， 可在服务端提前调用「[创建预览](#创建预览)」接口创建预览

**请求地址**

_GET_ https://shimo-domain/sdk/v2/api/cloud-files/{fileID}/page

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 示例值                               | 必选 | 说明                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的 token ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递                                        |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                                                          |
| Query    | lang      | string | zh-CN                                | 否   | 页面中的语言设置， 默认 zh-CN， 可选 en-US，ja-JP                                                                                                |

嵌入 iframe 使用

```html
<html>
  <head>
    <!-- some code -->
  </head>
  <body>
    <!-- your HTML elements -->
    <div id="shimo-sdk-preview" class="shimo-sdk-preview">
      <iframe
        src="https://shimo-domain/sdk/v2/api/cloud-files/{{fileID}}/page?signature=yourSignatureString&token=yourTokenString&lang=zh-CN"
        style="border: none; overflow: hidden;"
      ></iframe>
    </div>
  </body>
</html>
```

## 导入导出 {#import-export}

### 导入文件 {#import-v1}

:::caution
⚠️ 此接口为新版导入文件接口，建议优先使用。新版导入文件接口要求接入方必须解析结果中的 taskId，并且在调用「[( 新版 )获取导入进度](#import-progress-v1)」时带上 taskId 参数。任务执行的最长时间为 10 分钟。
:::

> 导入文件过程是一个异步处理过程，接口请求成功仅代表导入服务收到并开始处理导入请求，实际是否成功需要轮询「[( 新版 )获取导入进度](#import-progress-v1)」接口查询导入任务是否完成。

**请求地址**

_POST_ https://shimo-domain/sdk/v2/api/files/v1/import

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                        |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :---------------------------------------------------------------------------------------------------------- |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                                |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的用户凭证 ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递 |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                     |

**HTTP Request Body**

| 传参方式 | 参数名   | 类型   | 值示例                                        | 必选 | 说明                                                         |
| :------- | :------- | :----- | :-------------------------------------------- | :--- | :----------------------------------------------------------- |
| Form     | fileId   | string | file1001                                      | 是   | 服务商文件的唯一 ID                                          |
| Form     | type     | string | documentPro                                   | 是   | 需要导入的文件类型                                           |
| Form     | file     | file   |                                               | 否   | 需要导入的文件（如果不发送文件，就必须传 fileUrl、fileName） |
| Form     | fileUrl  | string | <https://domain.com/files/download/test.docx> | 否   | 需要导入的文件下载地址（有参数 file，可以不用传）            |
| Form     | fileName | string | test.docx                                     | 否   | 需要导入的文件名称（有参数 file，可以不用传）                |

:::caution
⚠️ 如无特别说明，导入文件 file 文件名需要附带扩展名，否则可能解析失败，如「`测试文件.doc`」或使用 `FormData` 时 `filename` 字段中传入带扩展名的文件名，参考 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData/append#%E7%A4%BA%E4%BE%8B)。
:::

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
]
}>
<TabItem value="curl">

```bash
curl --location --request POST 'https://shimo-domain/sdk/v2/api/files/v1/import?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString' \
--form 'fileId="{{importFileId}}"' \
--form 'type="document"' \
--form 'file=@"/your/local/path/测试.docx"'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("multipart/form-data");
RequestBody body = new MultipartBody.Builder().setType(MultipartBody.FORM)
  .addFormDataPart("fileId","{{importFileId}}")
  .addFormDataPart("type","document")
  .addFormDataPart("file","测试.docx",
    RequestBody.create(MediaType.parse("application/vnd.openxmlformats-officedocument.wordprocessingml.document"),
    new File("/your/local/path/测试.docx")))
  .build();
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/files/v1/import?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString")
  .method("POST", body)
  .build();
Response response = client.newCall(request).execute();
```

</TabItem>
<TabItem value="go">

```go
package main

import (
  "fmt"
  "bytes"
  "mime/multipart"
  "os"
  "path/filepath"
  "io"
  "net/http"
  "io/ioutil"
)

func main() {

  url := "https://shimo-domain/sdk/v2/api/files/v1/import?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString"
  method := "POST"

  payload := &bytes.Buffer{}
  writer := multipart.NewWriter(payload)
  _ = writer.WriteField("fileId", "{{importFileId}}")
  _ = writer.WriteField("type", "document")
  file, errFile3 := os.Open("/your/local/path/测试.docx")
  defer file.Close()
  part3,
         errFile3 := writer.CreateFormFile("file",filepath.Base("/Users/yanjixiong/Downloads/Office文件/测试.docx"))
  _, errFile3 = io.Copy(part3, file)
  if errFile3 != nil {
    fmt.Println(errFile3)
    return
  }
  err := writer.Close()
  if err != nil {
    fmt.Println(err)
    return
  }


  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
    return
  }
  req.Header.Set("Content-Type", writer.FormDataContentType())
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
var FormData = require("form-data");
var fs = require("fs");
var data = new FormData();
data.append("fileId", "{{importFileId}}");
data.append("type", "document");
data.append("file", fs.createReadStream("/your/local/path/测试.docx"));

var config = {
  method: "post",
  url: "https://shimo-domain/sdk/v2/api/files/v1/import?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString",
  headers: {
    ...data.getHeaders(),
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
</Tabs>

**HTTP Response Body**

| 字段名      | 类型   | 值示例           | 说明                                                                                |
| :---------- | :----- | :--------------- | :---------------------------------------------------------------------------------- |
| status      | number | 0                | 导入状态，非零值表示异常                                                            |
| message     | string |                  | 导入异常时的提示信息                                                                |
| data.taskId | string | TnJdQS8Wk70wNHuB | 导入任务的标识 ID，调用导入进度接口时，请带上该参数。导入失败时请提供此 ID 用于调试 |

**导入支持的文件格式**

| 石墨文件类型 | 支持导入的文件类型   |
| :----------- | :------------------- |
| document     | docx，doc，md，txt   |
| documentPro  | docx，doc，wps       |
| spreadsheet  | xlsx，xls，csv, xlsm |
| presentation | pptx，ppt            |

### 获取导入进度 {#import-progress-v1}

:::caution
⚠️ 此接口为新版获取导入进度接口，建议优先使用。当任务完成之后，进度结果将会默认缓存 5 分钟（可配置），过期之后再次调用进度结果将会失败。
:::
**请求地址**

_POST_ https://shimo-domain/sdk/v2/api/files/v1/import/progress

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                        |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :---------------------------------------------------------------------------------------------------------- |
| Query    | taskId    | string | TnJdQS8Wk70wNHuB                     | 是   | 导入文件接口返回的 taskId                                                                                   |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                                |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的用户凭证 ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递 |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                     |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
]
}>
<TabItem value="curl">

```bash
curl --location -g --request POST 'https://shimo-domain/sdk/v2/api/files/v1/import/progress?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString&taskId={{taskId}}' \
--data-raw ''
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "");
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/files/v1/import/progress?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString&taskId={{taskId}}")
  .method("POST", body)
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

  url := "https://shimo-domain/sdk/v2/api/files/v1/import/progress?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString&taskId=%7B%7BtaskId%7D%7D"
  method := "POST"

  payload := strings.NewReader(``)

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
    return
  }
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
var data = "";

var config = {
  method: "post",
  url: "https://shimo-domain/sdk/v2/api/files/v1/import/progress?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString&taskId={{taskId}}",
  headers: {},
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
</Tabs>

**HTTP Response Body**

| 字段名        | 类型   | 值示例 | 说明                                  |
| :------------ | :----- | :----- | :------------------------------------ |
| status        | number | 0      | 导入状态，非零值表示异常              |
| message       | string |        | 导入异常时的提示信息                  |
| data.progress | number | 100    | 导入进度百分比，为 100 时表示导入完成 |

**说明**

通过轮询该接口，直到返回的 `data.progress` 为 `100` 时表示`导入完成`，即可通过对接前端 js-sdk，在页面上进行查看或编辑。

### 导出文件 {#export-v1}

:::caution
⚠️ 此接口为新版导出文件接口，建议优先使用。新版导出文件接口要求接入方必须解析结果中的 taskId，并且在调用「[( 新版 )获取导出进度](#export-progress-v1)」时带上 taskId 参数。此接口允许同时将同一个石墨文件导出为不同的文件类型，例如：同时将轻文档导出为 PDF 和 Word 两个格式。任务执行的最长时间为 10 分钟。
:::
将在线编辑文件导出为 Office 文件、PDF 等格式，不同套件支持的导出格式有所不同，下文有具体列表。

> 导出文件过程是一个异步处理过程，接口请求成功仅代表导出服务收到并开始处理导出请求，实际是否成功需要轮询「[( 新版 )获取导出进度](#export-progress-v1)」接口查询导出任务是否完成，当任务完成后，用「获取导出进度」返回的链接 ( `data.downloadUrl` 字段) 下载文件。

**请求地址**

_POST_ https://shimo-domain/sdk/v2/api/files/v1/export/{fileId}

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                        |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :---------------------------------------------------------------------------------------------------------- |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                                |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的用户凭证 ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递 |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                     |

**HTTP Request Body**

| 传参方式 | 参数名 | 类型   | 值示例 | 必选 | 说明                                       |
| :------- | :----- | :----- | :----- | :--- | :----------------------------------------- |
| Body     | type   | string | docx   | 否   | 需要导出的文件类型，不传时按照默认类型导出 |

**导出支持的文件格式**

| 石墨文件类型 | 默认导出的文件类型 | 支持导出的文件类型 |
| :----------- | :----------------- | :----------------- |
| document     | docx               | docx, md, jpg, pdf |
| documentPro  | docx               | docx, pdf, wps     |
| spreadsheet  | xlsx               | xlsx               |
| presentation | pptx               | pptx, pdf          |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
]
}>
<TabItem value="curl">

```bash
curl --location -g --request POST 'https://shimo-domain/sdk/v2/api/files/v1/export/{{exportFileId}}?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString' \
--header 'Content-Type: application/json' \
--data-raw '{"type": "docx"}'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\"type\": \"docx\"}");
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/files/v1/export/{{exportFileId}}?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString")
  .method("POST", body)
  .addHeader("Content-Type", "application/json")
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

  url := "https://shimo-domain/sdk/v2/api/files/v1/export/%7B%7BexportFileId%7D%7D?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString"
  method := "POST"

  payload := strings.NewReader(`{"type": "docx"}`)

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
    return
  }
  req.Header.Add("Content-Type", "application/json")

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
  type: "docx",
});

var config = {
  method: "post",
  url: "https://shimo-domain/sdk/v2/api/files/v1/export/{{exportFileId}}?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString",
  headers: {
    "Content-Type": "application/json",
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
</Tabs>

**HTTP Response Body**

| 字段名      | 类型   | 值示例                       | 说明                                                                                |
| :---------- | :----- | :--------------------------- | :---------------------------------------------------------------------------------- |
| status      | number | 0                            | 导出状态，非零值表示异常                                                            |
| data.taskId | string | 3oo4vnBJgcG5HxMm:1:603:xmind | 导出任务的标识 ID，调用导出进度接口时，请带上该参数，导出失败时请提供此 ID 用于调试 |
| message     | string |                              | 导出异常时的提示信息                                                                |

### 获取导出进度 {#export-progress-v1}

:::caution
⚠️ 此接口为新版获取导入进度接口，建议优先使用。进度结果将会默认缓存 5 分钟（可配置），过期之后再次调用进度结果将会失败。
:::
**请求地址**

_POST_ https://shimo-domain/sdk/v2/api/files/v1/export/progress

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                        |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :---------------------------------------------------------------------------------------------------------- |
| Query    | taskId    | string | 3oo4vnBJgcG5HxMm:1:603:xmind         | 是   | 导出文件接口返回的 taskId                                                                                   |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                                |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的用户凭证 ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递 |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                     |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
]
}>
<TabItem value="curl">

```bash
curl --location -g --request POST 'https://shimo-domain/sdk/v2/api/files/v1/export/progress?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString&taskId={{taskId}}' \
--data-raw ''
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "");
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/files/v1/export/progress?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString&taskId={{taskId}}")
  .method("POST", body)
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

  url := "https://shimo-domain/sdk/v2/api/files/v1/export/progress?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString&taskId=%7B%7BtaskId%7D%7D"
  method := "POST"

  payload := strings.NewReader(``)

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
    return
  }
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
var data = "";

var config = {
  method: "post",
  url: "https://shimo-domain/sdk/v2/api/files/v1/export/progress?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString&taskId={{taskId}}",
  headers: {},
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
</Tabs>

**HTTP Response Body**

| 字段名           | 类型   | 值示例 | 说明                                  |
| :--------------- | :----- | :----- | :------------------------------------ |
| status           | number | 0      | 导出状态，非零值表示异常              |
| message          | string |        | 导出异常时的提示信息                  |
| data.progress    | number | 100    | 导出进度百分比，为 100 时表示导入完成 |
| data.downloadUrl | string |        | 导出文件的下载地址                    |

**说明**

通过轮询该接口，直到返回的 data.progress 为 100 时表示导出完成

### 导出应用表格为 Excel {#export-table-sheets}

导出应用表格为 .xlsx 文件返回该文件的下载地址

**请求地址**

_POST_ https://shimo-domain/sdk/v2/api/files/export/table-sheets/{fileId}

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                                                                     |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的用户凭证 ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递                                      |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                                                          |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
]
}>
<TabItem value="curl">

```bash
curl -X "POST" "https://shimo-domain/sdk/v2/api/files/export/table-sheets/:fileId?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString"
```

</TabItem>
<TabItem value="java">

```java
import java.io.IOException;
import org.apache.http.client.fluent.*;

public class SendRequest
{
  public static void main(String[] args) {
    sendRequest();
  }

  private static void sendRequest() {

    // 导出应用表格为 Excel (POST )

    try {

      // Create request
      Content content = Request.Post("https://shimo-domain/sdk/v2/api/files/export/table-sheets/:fileId?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString")

      // Fetch request and return content
      .execute().returnContent();

      // Print content
      System.out.println(content);
    }
    catch (IOException e) { System.out.println(e); }
  }
}
```

</TabItem>
<TabItem value="go">

```go
package main

import (
	"fmt"
	"io"
	"net/http"
)

func sendExcel() {
	// 导出应用表格为 Excel (POST https://shimo-domain/sdk/v2/api/files/export/table-sheets/:fileId?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString)

	// Create client
	client := &http.Client{}

	// Create request
	req, err := http.NewRequest("POST", "https://shimo-domain/sdk/v2/api/files/export/table-sheets/:fileId?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString", nil)

	parseFormErr := req.ParseForm()
	if parseFormErr != nil {
	  fmt.Println(parseFormErr)
	}

	// Fetch Request
	resp, err := client.Do(req)

	if err != nil {
		fmt.Println("Failure : ", err)
	}

	// Read Response Body
	respBody, _ := io.ReadAll(resp.Body)

	// Display Results
	fmt.Println("response Status : ", resp.Status)
	fmt.Println("response Headers : ", resp.Header)
	fmt.Println("response Body : ", string(respBody))
}
```

</TabItem>
<TabItem value="js">

```js
axios({
  method: "POST",
  url: "https://shimo-domain/sdk/v2/api/files/export/table-sheets/:fileId",
  params: {
    appId: "yourShimoAppId",
    signature: "yourSignatureString",
    token: "yourTokenString",
  },
});
```

</TabItem>
</Tabs>

**HTTP Response Body**

| 字段名      | 类型   | 值示例 | 说明                     |
| :---------- | :----- | :----- | :----------------------- |
| status      | number | 0      | 导出状态，非零值表示异常 |
| message     | string |        | 导出异常时的提示信息     |
| downloadUrl | string |        | .xlsx 文件下载地址       |

## 表格内容操作

### 获取表格内容

根据传入范围获取表格内容

**请求地址**

_GET_ https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                                                                     |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的 token ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递                                        |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                                                          |
| Query    | range     | string | 工作表1!A1:C3                       | 是   | 参照下方参数说明 [range](#range)                                                                                                                 |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
]
}>
<TabItem value="curl">

```bash
curl --location --request GET 'https://shimo-domain/sdk/v2/api/files/<fileId>/sheets/values?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString&range=工作表1!A1:C3'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/files/<fileId>/sheets/values?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString&range=工作表1!A1:C3")
  .method("GET", null)
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

  url := "https://shimo-domain/sdk/v2/api/files/%3CfileId%3E/sheets/values?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString&range=%E5%B7%A5%E4%BD%9C%E8%A1%A8%201!A1:C3"
  method := "GET"

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, nil)

  if err != nil {
    fmt.Println(err)
    return
  }
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
  url: "https://shimo-domain/sdk/v2/api/files/<fileId>/sheets/values?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString&range=工作表1!A1:C3",
  headers: {},
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

**支持的文件类型**

| 文件类型 | 类型值      |
| :------- | :---------- |
| 表格     | spreadsheet |

### 更新表格内容

根据传入范围更新表格内容

**请求地址**

_POST_ https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                                                                     |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的 token ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递                                        |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                                                          |

**HTTP Request Body**

| 字段名   | 类型   | 值示例         | 说明                                   |
| :------- | :----- | :------------- | :------------------------------------- |
| range    | string | 工作表1!A1:C3 | 参照下方参数说明 [range](#range)       |
| resource | object |                | 参照下方参数说明 [resource](#resource) |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
]
}>
<TabItem value="curl">

```bash
curl --location --request POST 'https://shimo-domain/sdk/v2/api/files/<fileId>/sheets/values?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString' \
--header 'Content-Type: application/json' \
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
  .url("https://shimo-domain/sdk/v2/api/files/<fileId>/sheets/values?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString")
  .method("POST", body)
  .addHeader("Content-Type", "application/json")
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

  url := "https://shimo-domain/sdk/v2/api/files/%3CfileId%3E/sheets/values?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString"
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
  req.Header.Add("Content-Type", "application/json")

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
  url: "https://shimo-domain/sdk/v2/api/files/<fileId>/sheets/values?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString",
  headers: {
    "Content-Type": "application/json",
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
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 204    | 返回成功 |

**HTTP Response Body**

空

**支持的文件类型**

| 文件类型 | 类型值      |
| :------- | :---------- |
| 表格     | spreadsheet |

### 追加表格内容

根据传入范围追加表格内容（只支持纯文本和数字）。 根据 range 查找追加位置的逻辑如下：
找到需要追加的行位置:
a. 不指定单元格范围，例如 工作表1：会遍历整个表格，找到最后一个有数据的行
b. 指定单元格范围，例如 工作表1!C5:E8：会找到起始单元格 C5 所在的行，从该行向下遍历，找到最后一个连续的有数据行向下插入 N 行，填充 values，其中 N = values.length
如果修改了不存在的列，会自动新增到该列

**请求地址**

_PUT_ https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                                                                     |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的 token ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递                                        |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                                                          |

**HTTP Request Body**

| 字段名   | 类型   | 值示例         | 说明                                   |
| :------- | :----- | :------------- | :------------------------------------- |
| range    | string | 工作表1!A1:C3 | 参照下方参数说明 [range](#range)       |
| resource | object |                | 参照下方参数说明 [resource](#resource) |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
]
}>
<TabItem value="curl">

```bash
curl --location --request PUT 'https://shimo-domain/sdk/v2/api/files/<fileId>/sheets/values?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString' \
--header 'Content-Type: application/json' \
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
  .url("https://shimo-domain/sdk/v2/api/files/<fileId>/sheets/values?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString")
  .method("PUT", body)
  .addHeader("Content-Type", "application/json")
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

  url := "https://shimo-domain/sdk/v2/api/files/%3CfileId%3E/sheets/values?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString"
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
  req.Header.Add("Content-Type", "application/json")

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
  url: "https://shimo-domain/sdk/v2/api/files/<fileId>/sheets/values?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString",
  headers: {
    "Content-Type": "application/json",
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
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 204    | 返回成功 |

**HTTP Response Body**

空

**支持的文件类型**

| 文件类型 | 类型值      |
| :------- | :---------- |
| 表格     | spreadsheet |

### 删除表格行

说明：从 index 开始，删除数量为 count 的行数

**请求地址**

_DELETE_ https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/{sheetName}/rows/{index}

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |
| Path     | sheetName | string | 工作表1                             | 是   | 表格中工作表的名称                                                                                                                               |
| Path     | index     | number | 0                                    | 是   | 从第几行开始删除                                                                                                                                 |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                                                                     |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的 token ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递                                        |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                                                          |
| Query    | count     | number | 1                                    | 否   | 删除几行，默认为 1                                                                                                                               |

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
]
}>
<TabItem value="curl">

```bash
curl --location --request DELETE 'https://shimo-domain/sdk/v2/api/files/<fileId>/sheets/工作表1/rows/0?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString&=2'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("text/plain");
RequestBody body = RequestBody.create(mediaType, "");
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/files/<fileId>/sheets/工作表1/rows/0?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString&=2")
  .method("DELETE", body)
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

  url := "https://shimo-domain/sdk/v2/api/files/%3CfileId%3E/sheets/%E5%B7%A5%E4%BD%9C%E8%A1%A8%201/rows/0?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString&=2"
  method := "DELETE"

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, nil)

  if err != nil {
    fmt.Println(err)
    return
  }
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
  url: "https://shimo-domain/sdk/v2/api/files/<fileId>/sheets/工作表1/rows/0?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString&=2",
  headers: {},
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
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 204    | 返回成功 |

**HTTP Response Body**

空

**支持的文件类型**

| 文件类型 | 类型值      |
| :------- | :---------- |
| 表格     | spreadsheet |

### 新增表格工作表

说明：新增工作表

**请求地址**

_POST_ https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | 是   | 文件 ID 位于创建预览接口的 URL 路径中，此文件 ID 为接入服务商文件列表中的唯一 ID，石墨会根据此 ID 请求接入服务商的文件接口获取文件信息和下载地址 |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                                                                     |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的 token ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递                                        |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                                                          |

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
]
}>
<TabItem value="curl">

```bash
curl --location --request POST 'https://shimo-domain/sdk/v2/api/files/<fileId>/sheets?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString' \
--header 'Content-Type: application/json' \
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
  .url("https://shimo-domain/sdk/v2/api/files/<fileId>/sheets?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString")
  .method("POST", body)
  .addHeader("Content-Type", "application/json")
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

  url := "https://shimo-domain/sdk/v2/api/files/%3CfileId%3E/sheets?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString"
  method := "POST"

  payload := strings.NewReader(`{"name": "工作表2"}`)

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
    return
  }
  req.Header.Add("Content-Type", "application/json")

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
  url: "https://shimo-domain/sdk/v2/api/files/<fileId>/sheets?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString",
  headers: {
    "Content-Type": "application/json",
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
</Tabs>

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 204    | 返回成功 |

**HTTP Response Body**

空

**支持的文件类型**

| 文件类型 | 类型值      |
| :------- | :---------- |
| 表格     | spreadsheet |

### 参数说明

#### range

格式为 ${工作表}!${单元格范围}，规范如下：

- 单元格范围的格式为 A1:C10 or A1，单元格起始位置不能超过表格最大的行列
- 如果工作表名称中含有 !:' 这 3 种特色字符，必须以单引号包裹工作表名称。
  示例: 工作表1 | 工作表1!A1 | 工作表1!A1:C1 | '工作!:表'!A2:C3

#### resource

类型为 object，里面必须包含 key 值 "values"。
values 为一个二维数组，表示追加/更新的表格内容，第一维表示行，第二维表示列。空字符串表示当前单元格为空。
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

### 日期类型 OADate 数值转换 {#oadate-conversion-example}

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

## **席位管理**

由于席位管理 API 的特殊性，为了安全考虑，需要您在[生成签名](./resources.md#签名)时附加上 `"scope": "license"` 字段。

### 获取用户列表和席位状态

**请求地址**

_GET_ http(s)://shimo-domain/sdk/v2/api/license/users

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                      |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :-------------------------------------------------------------------------------------------------------- |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                              |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的 token ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递 |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                   |
| Query    | page      | number | 1                                    | 是   | 请求页码                                                                                                  |
| Query    | size      | number | 30                                   | 是   | 单页返回用户数量                                                                                          |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
]
}>
<TabItem value="curl">

```bash
curl --location --request GET 'https://shimo-domain/sdk/v2/api/license/users?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString&page=1&size=30'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/license/users?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString&page=1&size=30")
  .method("GET", null)
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

  url := "https://shimo-domain/sdk/v2/api/license/users?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString&page=1&size=30"
  method := "GET"

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, nil)

  if err != nil {
    fmt.Println(err)
    return
  }
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
  url: "https://shimo-domain/sdk/v2/api/license/users?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString&page=1&size=30",
  headers: {},
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
</Tabs>

**HTTP Response Body**

响应体示例：

```json
[
  {
    "userId": "1",
    "createdAt": "2021-05-20T14:56:53+08:00",
    "status": 1
  }
]
```

| 字段名    | 类型   | 值示例                    | 说明                                            |
| :-------- | :----- | :------------------------ | :---------------------------------------------- |
| userId    | string | 1                         | 用户 ID                                         |
| createdAt | string | 2021-05-20T14:56:53+08:00 | 创建时间                                        |
| status    | number | 1                         | 席位状态<br/>1 激活<br/>0 已禁用<br/> -1 未启用 |

#### 用户状态说明 {#user-status-desc}

:::tip 未启用状态说明
**`未启用` 状态何时会存在？**

若接入方已集成实现[`at 人`](./impl#feature-mention-at)相关功能时，在调用接入方搜索接口时，可能会返回石墨 SDK 中未关联的用户，由于需要在石墨编辑器中的 at 下拉列表中展示此类用户，石墨 SDK 会首先进行建立用户关联操作，此时用户的状态将会初始化为`未启用`状态，此状态 **`不会占用`** 席位数量。

**`未启用` 状态自动激活**

当用户处于未启用状态时，当通过此用户身份进行如下操作时，将会自动激活，成为 `激活` 状态，此时占用相应席位，若席位不足，则会被拒绝访问:

- 创建文件
- 创建副本
- 访问编辑页
- 导入、导出文件
- 预览文件
- 调用表格内容操作接口
- 获取文件侧边栏历史信息、版本信息
- 获取文件纯文本信息
- 获取文件 at 人列表
- 获取文件评论数据
  :::

### 激活用户席位

**请求地址**

_POST_ http(s)://shimo-domain/sdk/v2/api/license/users/activate

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                      |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :-------------------------------------------------------------------------------------------------------- |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                              |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的 token ，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递 |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                   |

**HTTP Request Body**

请求体示例：

| 字段名  | 类型     | 值示例     | 必选 | 说明                       |
| :------ | :------- | :--------- | :--- | :------------------------- |
| userIds | []string | ["1", "2"] | 是   | 需要激活席位的用户 ID 列表 |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
]
}>
<TabItem value="curl">

```bash
curl --location --request POST 'https://shimo-domain/sdk/v2/api/license/users/activate?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString' \
--header 'Content-Type: application/json' \
--data-raw '{"userIds":["1","2"]}'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\"userIds\":[\"1\",\"2\"]}");
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/license/users/activate?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString")
  .method("POST", body)
  .addHeader("Content-Type", "application/json")
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

  url := "https://shimo-domain/sdk/v2/api/license/users/activate?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString"
  method := "POST"

  payload := strings.NewReader(`{"userIds":["1","2"]}`)

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
    return
  }
  req.Header.Add("Content-Type", "application/json")

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
  userIds: ["1", "2"],
});

var config = {
  method: "post",
  url: "https://shimo-domain/sdk/v2/api/license/users/activate?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString",
  headers: {
    "Content-Type": "application/json",
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
</Tabs>

**HTTP Response Body**

空

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 204    | 操作成功 |

### 取消用户席位

**请求地址**

_POST_ http(s)://shimo-domain/sdk/v2/api/license/users/deactivate

**HTTP Request Parameters**

| 传参方式 | 参数名    | 类型   | 值示例                               | 必选 | 说明                                                                                                     |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :------------------------------------------------------------------------------------------------------- |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | 是   | 接入服务商从石墨获取的 AppId                                                                             |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | 是   | 接入服务商提供的 token，在石墨请求接入服务商的接口时，会放到 HTTP Headers X-Shimo-Token 中作为值进行传递 |
| Query    | signature | string | 参考签名方法一节                     | 是   | 接入服务商，使用从石墨获取的 AppId 、AppSecret 根据签名方法生成的字符串                                  |

**HTTP Request Body**

请求体示例：

| 字段名  | 类型     | 值示例     | 必选 | 说明                       |
| :------ | :------- | :--------- | :--- | :------------------------- |
| userIds | []string | ["1", "2"] | 是   | 需要取消席位的用户 ID 列表 |

**请求示例**
<Tabs
defaultValue="curl"
values={[
{ label: 'cURL', value: 'curl', },
{ label: 'Java', value: 'java', },
{ label: 'Golang', value: 'go', },
{ label: 'Node.js', value: 'js', },
]
}>
<TabItem value="curl">

```bash
curl --location --request POST 'https://shimo-domain/sdk/v2/api/license/users/deactivate?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString' \
--header 'Content-Type: application/json' \
--data-raw '{"userIds":["1","2"]}'
```

</TabItem>
<TabItem value="java">

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\"userIds\":[\"1\",\"2\"]}");
Request request = new Request.Builder()
  .url("https://shimo-domain/sdk/v2/api/license/users/deactivate?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString")
  .method("POST", body)
  .addHeader("Content-Type", "application/json")
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

  url := "https://shimo-domain/sdk/v2/api/license/users/deactivate?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString"
  method := "POST"

  payload := strings.NewReader(`{"userIds":["1","2"]}`)

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
    return
  }
  req.Header.Add("Content-Type", "application/json")

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
  userIds: ["1", "2"],
});

var config = {
  method: "post",
  url: "https://shimo-domain/sdk/v2/api/license/users/deactivate?appId=yourShimoAppId&signature=yourSignatureString&token=yourTokenString",
  headers: {
    "Content-Type": "application/json",
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
</Tabs>

**HTTP Response Body**

空

**HTTP 状态码**

| 状态码 | 说明     |
| :----- | :------- |
| 204    | 操作成功 |
