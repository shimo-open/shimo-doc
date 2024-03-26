---
sidebar_position: 5
description: >-
  The Graphite SDK server domain name refers to the Graphite SDK server, which is used to obtain the detailed information of the app, including the name, the type of file that can be used, the number of activated users, the total number of users, the number of seats, and the license
  Valid time and callback address information. Update the app callback adres, create a collaboration document of a specified type, specify the language information when creating the file, and access the unique file of the service provider
  ID, delete the collaboration document, delete the specified type of collaboration document.
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# List of graphite APIs

:::caution Note
⚠️ Unless otherwise specified, all requests are  , `HTTP`and both the request header and the return header `Content-Type` are `application/json` . 

⚠️ `shimo-domain` Refers to the domain name of the Graphite SDK server, please contact Graphite to obtain this domain name. 

⚠️ If there is a special symbol in the parameter value in the route, it is recommended to escape the value, please refer to JavaScript escaping method [`MDN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent?fileGuid=Wr3DVn8lO4HE2kJQ).
:::

## Account and application management

### get app detail {#get-app-detail}

Obtain app details, including app name, available file types, number of activated users, total number of users, number of seats, license validity period, and callback address information.

:::caution warning

Due to the special nature of the app API, for security reasons, you need to[ attach the field when ](./resources.md#签名) `"scope": "license"`generating the signature

:::

**The address of the request**

_GET_ https://shimo-domain/sdk/v2/api/license/apps/{appId}

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Examples of values                               | Required | illustrate                                                                                                      |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :-------------------------------------------------------------------------------------------------------- |
| Path     | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                              |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                              |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The token provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                   |

**Sample request**
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

**HTTP status code**

| Status code | illustrate     |
| :----- | :------- |
| 200    | The return is successful |

**HTTP Response Body**

| The name of the field             | type   | Examples of values                                                          | illustrate                                                                            |
| :----------------- | :----- | :-------------------------------------------------------------- | :------------------------------------------------------------------------------ |
| appName            | string | your app name                                                   | The name of the app                                                                          |
| availableFileTypes | Array  | ["document","documentPro","spreadsheet","presentation","table"] | List of available graphite kits                                                                |
| activatedUserCount | number | 43                                                              | The number of users who have activated seats                                                                |
| userCount          | number | 50                                                              | The total number of users, including`已激活` , , and`已禁用` the `未使用` total number of users, and only `已激活` the number of occupied seats |
| memberLimit        | number | 99                                                              | The license seat limits the number of users, that is, the `已激活` maximum number of users                            |
| validFrom          | string | 2020-12-31T16:00:00Z                                            | The effective time of the license                                                                |
| validUntil         | string | 2022-12-30T16:00:00Z                                            | The expiration date of the license term                                                          |
| endpointUrl        | string | http://your-domain                                              | The callback address of the service provider                                                                  |

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

### update-app-endpoint-url {#update-app-endpoint-url}

Used to update the callback address of the corresponding service provider of the app

:::caution warning

Due to the special nature of the app API, for security reasons, you need to[ attach the field when ](./resources.md#签名) `"scope": "license"`generating the signature

:::

**The address of the request**

_PUT_ https://shimo-domain/sdk/v2/api/license/apps/{appId}/endpoint-url

| Parameter transfer method | Several participants    | type   | Examples of values                               | Required | illustrate                                                                                                      |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :-------------------------------------------------------------------------------------------------------- |
| Path     | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                              |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                              |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The token provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                   |

**HTTP Request Body**

| The name of the field | type   | Examples of values             | Required | illustrate                     |
| :----- | :----- | :----------------- | :--- | :----------------------- |
| url    | string | http://your-domain | be   | The callback address of the service provider that needs to be updated |

**Sample request**
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

**HTTP status code**

| Status code | illustrate     |
| :----- | :------- |
| 204    | The creation is successful |

**HTTP Response Body**

empty

## Collaborative documentation

### create-collab-file {#create-collab-file}

Create a collaborative document of the specified type

**The address of the request**

_POST_ https://shimo-domain/sdk/v2/api/files

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Examples of values                               | Required | illustrate                                                                                                                               |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :--------------------------------------------------------------------------------------------------------------------------------- |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                                                       |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The token provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider                          |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                                            |
| Query    | lang      | string | in                                   | not   | Optional. The default value is the server-side default language setting, which usually defaults to  .`zh-CN` Specify the language information when creating the file, and the following values are optional:`zh-CN` (Chinese Simplified), `en`(English), (Japanese`ja`). |

**HTTP Request Headers**

| Header 名       | value                      | Required | illustrate                                                                                                                                                                                                     |
| :-------------- | :---------------------- | :--- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Accept-Language | Z-Sun,Z; K=0.9,N; Q=0.8 | not   | If there is no `lang` parameter by default, try to identify this header, if this header is not passed, the server default language is used. The Accept-Languge format refers to [MDN Accept-Language](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept-Language) |

**HTTP Request Body**

| The name of the field | type   | Examples of values     | Required | illustrate                                                    |
| :----- | :----- | :--------- | :--- | :------------------------------------------------------ |
| type   | string | document   | be   | For graphite file types, please refer to the "Supported File Types" section for optional values        |
| fileId | string | filid1001 | be   | The unique ID of the file that connects to the service provider must be a string no longer than 64 in length |

**Sample request**
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

**HTTP status code**

| Status code | illustrate     |
| :----- | :------- |
| 204    | The creation is successful |

**HTTP Response Body**

empty

**Supported document types**

| file type | Type value       |
| :------- | :----------- |
| Light documents   | document     |
| form     | spreadsheet  |
| Traditional documentation | documentPro  |
| slide   | presentation |
| Application tables | table        |

:::caution 说明

In addition to `轻文档`, ,`传统文档``表格` , and  , `幻灯片`other types are non-standard functions (such as `应用表格`), please contact Graphite customer service for details. 

:::

:::info Notes

With the exception `轻文档` of , all `应用表格` file types are highly compatible with Office formats

:::

**Load the editor after creation**

Refer to the steps in "[Collaborative Documentation - Access Process](./roll.md#接入流程)" to initialize the use of graphite JSSDK and follow the parameter instructions. 

### Create a collaborative copy of the document

Creates a copy of the specified collaboration document

**The address of the request**

_POST_ https://shimo-domain/sdk/v2/shimo-files/{fileId}/copy

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Examples of values                               | Required | illustrate                                                                                                      |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :-------------------------------------------------------------------------------------------------------- |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                              |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The token provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                   |

**HTTP Request Body**

| The name of the field | type   | Examples of values     | Required | illustrate                                                               |
| :----- | :----- | :--------- | :--- | :----------------------------------------------------------------- |
| fileId | string | filid1001 | be   | The new file ID, the unique ID of the file accessing the service provider, must be a string no larger than 64 in length |

**Sample request**
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

**HTTP status code**

| Status code | illustrate                                                                                  |
| :----- | :------------------------------------------------------------------------------------ |
| 204    | The creation is successful                                                                              |
| 200    | When a request is repeated, a copy of the file is being executed, responding to the data `{"code":70019}`                               |
| 400    | When the request is repeated, the target file association data exists, but the information of the Create Replica task cannot be found, and the data `{"code":70016}` is responded|
| 400    | If the request is repeated, the data associated with the target file exists, and the replica fails to be created, and the data is responded `{"code":70017}`         |
| 500    | When repeating the request, an error was encountered when creating a copy of the source file content, responding to the data `{"code":70015}`               |
| 500    | When repeating the request, an error was encountered while getting the Create Replica task in response to data `{"code":70018}`                     |
| 500    | Duplicate requests are repeated when the status of the Create Replica task is unknown and the response data is unknown `{"code":70020}`                         |

**HTTP Response Body**

empty

### Delete the collaboration document

Deletes a collaboration document of the specified type, requiring the user to have permission on the document `manageable` . 

**The address of the request**

_DELETE_ https://shimo-domain/sdk/v2/api/files/{fileId}

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Examples of values                               | Required | illustrate                                                                                                      |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :-------------------------------------------------------------------------------------------------------- |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                              |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The token provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                   |

**HTTP Request Body**

not

**Sample request**
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

**HTTP status code**

| Status code | illustrate     |
| :----- | :------- |
| 204    | The operation was successful |

**HTTP Response Body**

empty

### Get the history list {#doc-sidebar-info}

Get the file sidebar history list information

**The address of the request**

_getttps_  ://shimo-domain/sdk/v2/smo-files/{filed}/doc-sidebar-info

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter      | type   | Examples of values                               | Required | illustrate                                                                                                                                             |
| :------- | :---------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId      | string | qeK4Xdxvxg8jF5gz                     | be   | The file ID is located in the URL path of the API to create the preview API, and this file ID is a unique ID in the file list of the access service provider, and Graphite will request the file interface of the access service provider to obtain the file information and download address based on this ID |
| Query    | appId       | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                                                                     |
| Query    | token       | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The token provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider                                        |
| Query    | signature   | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                                                          |
| Query    | pageSize    | number | 10                                   | not   |The  number of entries returned per page, recommended value`10`, and too `20` large will affect the response time. The default value is `10`                                                                    |
| Query    | count       | number | 0                                    | not   | The number of records that need to be skipped on the current page can `count = (page - 1) * pageSize` be obtained by . The default value is `0`                                                                |
| Query    | historyType | number | 1                                    | not   | Optional values:`1` Operation history (such as the history of operations resulting from modifications that did not actually write such as locked cells),`2` edit history. When not passed, all types are returned by default                              |

**Sample request**
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

**HTTP status code**

| Status code | illustrate     |
| :----- | :------- |
| 200    | The return is successful |

**HTTP Response Body**

| The name of the field                   | type    | Examples of values                   | illustrate                                           |
| :----------------------- | :------ | :----------------------- | :--------------------------------------------- |
| histories                | array   |                          | Sidebar history array                                 |
| histories[i].content     | string  |                          | Collaborate on file format data                               |
| histories[i].createdAt   | string  | 2021-06-07T06:12:24Z     | The history of the sidebar creation time of this article                         |
| histories[i].historyType | number  | 2                        | The sidebar history type, 1 is the operation history and 2 is the edit generated     |
| histories[i].id          | string  | 60bdb8c847a7850006bf12c1 | Sidebar history ID                                  |
| histories[i].name        | string  | 历史 1                   | Sidebar historical name                                 |
| histories[i].updatedAt   | string  | 2021-06-07T06:12:24Z     | The sidebar history was last updated                         |
| histories[i].userId      | string  | user123,user134          | Vendor user IDs, which may be several, separated by a comma "," |
| isLastPage               | boolean |                          | Whether it's the last page                                   |
| limit                    | number  | 100                      | The size of the pagination                                       |
| users                    | object  | { "user123": "User A" }  | The service provider user ID corresponds to the username mapping                   |

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

#### Historical type description

##### 操作历史 (historyType = 1)

For records that are manually operated and do not usually cause changes in the text of the file, the type and context information of such history is stored in the content field in the form of JSON strings.

The known operations are as follows, and the JSON data is `histories[i].content` the result of the field JSON.parse

###### createRevision 创建版本

```json
{
  "action": "createRevision"
}
```

###### renameRevision Rename the version

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

###### lock_cell Lock the cell

```json
{
  "action": "lock_cell",
  "range": "E14:E14",
  "name": "工作表1"
}
```

###### unlock_cell Unlock the cell

```json
{
  "action": "unlock_cell",
  "range": ["E14:E14"],
  "name": "工作表1"
}
```

###### lock_sheet Lock the worksheet

```json
{
  "action": "lock_sheet",
  "name": "工作表1"
}
```

###### unlock_sheet Unlock the worksheet

```json
{
  "action": "unlock_sheet",
  "name": "工作表1"
}
```

###### update_lock_cell Update cell locking

```json
{
  "action": "update_lock_cell",
  "range": ["E17:E17"],
  "name": "工作表1"
}
```

##### 编辑历史 (historyType = 2)

Indicates that one or more users have made edits to a file over a period of time

### Get a list of versions

Obtain the file version list information

**The address of the request**

_GET_ https://shimo-domain/sdk/v2/shimo-files/{fileId}/revisions

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Examples of values                               | Required | illustrate                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | be   | The file ID is located in the URL path of the API to create the preview API, and this file ID is a unique ID in the file list of the access service provider, and Graphite will request the file interface of the access service provider to obtain the file information and download address based on this ID |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                                                                     |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The token provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider                                        |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                                                          |

**Sample request**
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

**HTTP status code**

| Status code | illustrate     |
| :----- | :------- |
| 200    | The return is successful |

**HTTP Response Body**

| The name of the field       | type   | Examples of values                   | illustrate               |
| :----------- | :----- | :----------------------- | :----------------- |
| id           | string | 11089                    | Version ID            |
| label        | string | Monday, 6/7/2021 14:33    | Version Label         |
| title        | string | 123                      | title               |
| docHistoryId | string | 60bdbda2f0a8af000664d719 | Corresponding sidebar history ID  |
| createdAt    | string | 2021-06-07T06:33:13Z     | Sidebar history created time |
| updatedAt    | string | 2021-06-07T06:33:13Z     | Sidebar history update time |
| user.id      | string | user123                  | Vendor User ID      |
| user.name    | string | testuser                 | Username             |

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

### Get the plain text content of the file

Gets the plain text content of a specified file

**The address of the request**

_GET_ https://shimo-domain/sdk/v2/shimo-files/{fileId}/plain-text

**HTTP Request Parameters**

| Parameter transfer method | Several participants    | type   | Examples of values                               | Required | illustrate                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | be   | The file ID is located in the URL path of the API to create the preview API, and this file ID is a unique ID in the file list of the access service provider, and Graphite will request the file interface of the access service provider to obtain the file information and download address based on this ID |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                                                                     |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The token provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider                                        |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                                                          |

**Sample request**
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

**HTTP status code**

| Status code | illustrate     |
| :----- | :------- |
| 200    | The return is successful |

**HTTP Response Body**

| The name of the field  | type   | Examples of values         | illustrate                                     |
| :------ | :----- | :------------- | :--------------------------------------- |
| content | string | The plain text content of the file | The plain text content of a graphite file obtained based on the specified file ID |

Example

```json
{
  "content": "文件纯文本内容"
}
```

### Get a list of all at people in the file contents

Get a list of all at people in the file contents

Supported file types:

- `document` Light documents
- `documentPro` Traditional documentation
- `spreadsheet` form

**The address of the request**

_GET_ https://shimo-domain/sdk/v2/shimo-files/{fileId}/mention-at-list

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Examples of values                               | Required | illustrate                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | be   | The file ID is located in the URL path of the API to create the preview API, and this file ID is a unique ID in the file list of the access service provider, and Graphite will request the file interface of the access service provider to obtain the file information and download address based on this ID |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                                                                     |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The token provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider                                        |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                                                          |

**Sample request**
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

**HTTP status code**

| Status code | illustrate     |
| :----- | :------- |
| 200    | The return is successful |

**HTTP Response Body**

| The name of the field                  | type   | Examples of values           | illustrate                                               |
| :---------------------- | :----- | :--------------- | :------------------------------------------------- |
| mentionAtList           | Array  |                  | A list of all at information in a graphite file obtained based on the specified file ID |
| mentionAtList.userId[0] | string | user1            | at the user ID of the mentions                                   |
| mentionAtList.atGuid[0] | string | MODOCnb4HPu04G8f | at mentions the corresponding location tag in the file                      |

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

### Get the number of comments in a file {#get-comment-count}

Get the total number of comments in a file

Supported file types:

- `spreadsheet` form

**The address of the request**

_GET_ https://shimo-domain/sdk/v2/shimo-files/{fileId}/comment-count

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Examples of values                               | Required | illustrate                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | be   | The file ID is located in the URL path of the API to create the preview API, and this file ID is a unique ID in the file list of the access service provider, and Graphite will request the file interface of the access service provider to obtain the file information and download address based on this ID |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                                                                     |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The token provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider                                        |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                                                          |

**Sample request**
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

**HTTP status code**

| Status code | illustrate     |
| :----- | :------- |
| 200    | The return is successful |

**HTTP Response Body**

| The name of the field | type   | Examples of values | illustrate     |
| :----- | :----- | :----- | :------- |
| count  | number | 2      | Number of comments |

Example

```json
{
  "count": 2
}
```

## File preview

:::caution Note
⚠️ This feature does not support editing documents, if you want to edit documents, please refer to [`协同文档`](#协同文档).
:::

### Create a preview

Create a preview task synchronously, and if it fails, return the reason for the failure

**The address of the request**

_POST_ https://shimo-domain/sdk/v2/api/cloud-files/{fileID}/create

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Example values                               | Required | illustrate                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | be   | The file ID is located in the URL path of the API to create the preview API, and this file ID is a unique ID in the file list of the access service provider, and Graphite will request the file interface of the access service provider to obtain the file information and download address based on this ID |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The token provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider                                        |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                                                          |

**Sample request**
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

| The name of the field  | type   | Examples of values             | illustrate                                                        |
| :------ | :----- | :----------------- | :---------------------------------------------------------- |
| code    | string | 90028              | Create a preview result status code, with an empty string indicating that the creation is successful, and a non-empty string indicating that the creation fails |
| message | string | file is converting | Create a failed error message                                            |

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

### Access the preview

The page used to render the preview on the browser needs to be used with an embedded iframe

> For the same file, when you call this API for the first time, the backend will asynchronously call[ the "Create Preview](#创建预览)" API to create a task, and the preview page will be rendered only after the creation is successful. If you need to speed up the first time you open a file, you can call[ the Create Preview](#创建预览) API on the server to create a preview in advance

**The address of the request**

_GET_ https://shimo-domain/sdk/v2/api/cloud-files/{fileID}/page

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Example values                               | Required | illustrate                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | be   | The file ID is located in the URL path of the API to create the preview API, and this file ID is a unique ID in the file list of the access service provider, and Graphite will request the file interface of the access service provider to obtain the file information and download address based on this ID |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The token provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider                                        |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                                                          |
| Query    | lang      | string | zh-CN                                | not   | The language settings in the page, default zh-CN, optional en-US, ja-JP                                                                                                |

Embed iframes to use

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

## Import and export {#import-export}

### Import file {#import-v1}

:::caution
⚠️ This API is a new version of the API for importing files, and it is recommended that you use it first. The new version of the import file API requires the accessing party to parse the taskId in the result, and[ to bring the taskId parameter when calling "(New) Get Import Progress](#import-progress-v1)". The maximum time for a task to be executed is 10 minutes.
:::

> The process of importing files is an asynchronous process, and the successful interface request only means that the import service receives and starts processing the import request.[](#import-progress-v1) 

**The address of the request**

_POST_ https://shimo-domain/sdk/v2/api/files/v1/import

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Examples of values                               | Required | illustrate                                                                                                        |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :---------------------------------------------------------------------------------------------------------- |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                                |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The user credential provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                     |

**HTTP Request Body**

| Parameter transfer method | The name of the parameter   | type   | Examples of values                                        | Required | illustrate                                                         |
| :------- | :------- | :----- | :-------------------------------------------- | :--- | :----------------------------------------------------------- |
| Form     | fileId   | string | file1001                                      | be   | The unique ID of the service provider's file                                          |
| Form     | type     | string | documentPro                                   | be   | The type of file that needs to be imported                                           |
| Form     | file     | file   |                                               | not   | Files that need to be imported (fileUrl, fileName must be passed if the file is not sent) |
| Form     | fileUrl  | string | <https://domain.com/files/download/test.docx> | not   | The download address of the file to be imported (there is a parameter file, you don't need to pass it)            |
| Form     | fileName | string | test.docx                                     | not   | The name of the file to be imported (there is a parameter file, you don't need to pass it)                |

:::caution
⚠️ Unless otherwise specified, the file name of the imported file needs to be accompanied by an extension, otherwise it may fail to parse, such as "`测试文件.doc`" or the  file name with the extension in the `FormData`"" or use `filename` the time  field, refer to [MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData/append#%E7%A4%BA%E4%BE%8B).
:::

**Sample request**
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

| The name of the field      | type   | Examples of values           | illustrate                                                                                |
| :---------- | :----- | :--------------- | :---------------------------------------------------------------------------------- |
| status      | number | 0                | The import status, with a non-zero value indicating an exception                                                            |
| message     | string |                  | A message will be displayed when an exception is imported                                                                |
| data.taskId | string | TnJdQS8Wk70wNHuB | The ID of the import task, and bring it when you call the import progress API. When the import fails, provide this ID for debugging |

**Import a supported file format**

| Graphite file type | Supported file types for import   |
| :----------- | :------------------- |
| document     | docx，doc，md，txt   |
| documentPro  | docx,doc,wps       |
| spreadsheet  | xlsx，xls，csv, xlsm |
| presentation | pptx，ppt            |

### Get the import progress {#import-progress-v1}

:::caution
⚠️ This API is the new version of the API for obtaining import progress, and it is recommended that you use it first. When the task is completed, the progress result will be cached for 5 minutes by default (configurable), and the progress result will fail to be called again after the expiration date.
:::
**The address of the request**

_POST_ https://shimo-domain/sdk/v2/api/files/v1/import/progress

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Examples of values                               | Required | illustrate                                                                                                        |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :---------------------------------------------------------------------------------------------------------- |
| Query    | taskId    | string | TnJdQS8Wk70wNHuB                     | be   | The taskId returned by the import file interface                                                                                   |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                                |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The user credential provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                     |

**Sample request**
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

| The name of the field        | type   | Examples of values | illustrate                                  |
| :------------ | :----- | :----- | :------------------------------------ |
| status        | number | 0      | The import status, with a non-zero value indicating an exception              |
| message       | string |        | A message will be displayed when an exception is imported                  |
| data.progress | number | 100    | The import progress percentage, which is 100 for import completion |

**illustrate**

By polling the interface until the return is `data.progress` `100` a time representation`导入完成`, you can view or edit it on the page by docking with the front-end js-SDK. 

### Export File {#export-v1}

:::caution
⚠️ This API is a new version of the export file API and is recommended to be used first. The new version of the export file API requires the accessing party to parse the taskId in the result and call "[ ( New ) to get the export progress](#export-progress-v1)taskId parameter. This interface allows you to export the same graphite file to different file types at the same time, for example, to export a light document to both PDF and Word formats at the same time. The maximum time for a task to be executed is 10 minutes.
:::
Export online editing files to Office files, PDFs, and more, which are supported by different suites, as shown below. 

> The export process is an asynchronous process, the success of the interface request only means that the export service receives and starts to process the export request, whether it is successful or not, you need to poll the "[(new version) get export progress](#export-progress-v1)" interface to check whether the export task is completed, when the task is completed, use the link (field) returned by "get export progress" `data.downloadUrl` to  download the file. 

**The address of the request**

_POST_ https://shimo-domain/sdk/v2/api/files/v1/export/{fileId}

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Examples of values                               | Required | illustrate                                                                                                        |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :---------------------------------------------------------------------------------------------------------- |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                                |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The user credential provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                     |

**HTTP Request Body**

| Parameter transfer method | The name of the parameter | type   | Examples of values | Required | illustrate                                       |
| :------- | :----- | :----- | :----- | :--- | :----------------------------------------- |
| Body     | type   | string | docx   | not   | The file type that needs to be exported is exported according to the default type when it is not transmitted |

**Export supported file formats**

| Graphite file type | The default exported file type | Supported file types for export |
| :----------- | :----------------- | :----------------- |
| document     | docx               | Docs, MD, JPG, PDF |
| documentPro  | docx               | docx, pdf, wps     |
| spreadsheet  | xlsx               | xlsx               |
| presentation | pptx               | PPTX, PDF          |

**Sample request**
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

| The name of the field      | type   | Examples of values                       | illustrate                                                                                |
| :---------- | :----- | :--------------------------- | :---------------------------------------------------------------------------------- |
| status      | number | 0                            | The export status, with a non-zero value indicating an anomaly                                                            |
| data.taskId | string | 3oo4vnBJgcG5HxMm:1:603:xmind | The ID of the export task, please bring this parameter when calling the export progress API, and provide this ID for debugging when the export fails |
| message     | string |                              | Notification information when an exception is exported                                                                |

### Get export progress {#export-progress-v1}

:::caution
⚠️ This API is the new version of the API for obtaining import progress, and it is recommended that you use it first. Progress results will be cached for 5 minutes (configurable) by default, and recalling progress results will fail after expiration.
:::
**The address of the request**

_POST_ https://shimo-domain/sdk/v2/api/files/v1/export/progress

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Examples of values                               | Required | illustrate                                                                                                        |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :---------------------------------------------------------------------------------------------------------- |
| Query    | taskId    | string | 3oo4vnBJgcG5HxMm:1:603:xmind         | be   | The taskId returned by the export file interface                                                                                   |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                                |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The user credential provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                     |

**Sample request**
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

| The name of the field           | type   | Examples of values | illustrate                                  |
| :--------------- | :----- | :----- | :------------------------------------ |
| status           | number | 0      | The export status, with a non-zero value indicating an anomaly              |
| message          | string |        | Notification information when an exception is exported                  |
| data.progress    | number | 100    | The export progress percentage, which is 100 for import completion |
| data.downloadUrl | string |        | The download address of the exported file                    |

**illustrate**

The export is complete by polling the interface until the returned data.progress is 100

### Export table to Excel {#export-table-sheets}

The export application table returns the download address of the .xlsx file

**The address of the request**

_Post _ Hatpas://shimo-domain/SDK/v2/API/Files/Export/Table-sheets/{filed}

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Examples of values                               | Required | illustrate                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | be   | The file ID is located in the URL path of the API to create the preview API, and this file ID is a unique ID in the file list of the access service provider, and Graphite will request the file interface of the access service provider to obtain the file information and download address based on this ID |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                                                                     |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The user credential provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider                                      |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                                                          |

**Sample request**
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

| The name of the field      | type   | Examples of values | illustrate                     |
| :---------- | :----- | :----- | :----------------------- |
| status      | number | 0      | The export status, with a non-zero value indicating an anomaly |
| message     | string |        | Notification information when an exception is exported     |
| downloadUrl | string |        | .xlsx The address where the file was downloaded       |

## Table content operations

### Get the contents of the form

Gets the table content based on the incoming range

**The address of the request**

_GET_ https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Examples of values                               | Required | illustrate                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | be   | The file ID is located in the URL path of the API to create the preview API, and this file ID is a unique ID in the file list of the access service provider, and Graphite will request the file interface of the access service provider to obtain the file information and download address based on this ID |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                                                                     |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The token provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider                                        |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                                                          |
| Query    | range     | string | Worksheet 1! A1:C3                       | be   | Refer to the following parameter description [range](#range)                                                                                                                 |

**Sample request**
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

**HTTP status code**

| Status code | illustrate     |
| :----- | :------- |
| 200    | The return is successful |

**HTTP Response Body**

| The name of the field | type  | Examples of values                                    | illustrate         |
| :----- | :---- | :---------------------------------------- | :----------- |
| values | array | [["Name", "Age", "Gender"],["Xiao Ming", 29, "Male"]] | Table cell values |

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

**Supported file types**

| file type | Type value      |
| :------- | :---------- |
| form     | spreadsheet |

### Update the content of the table

Updates the table content based on the incoming range

**The address of the request**

_POST_ https://shimo-domain/sdk/v2/api/files/{fileId}/sheets/values

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Examples of values                               | Required | illustrate                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | be   | The file ID is located in the URL path of the API to create the preview API, and this file ID is a unique ID in the file list of the access service provider, and Graphite will request the file interface of the access service provider to obtain the file information and download address based on this ID |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                                                                     |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The token provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider                                        |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                                                          |

**HTTP Request Body**

| The name of the field   | type   | Examples of values         | illustrate                                   |
| :------- | :----- | :------------- | :------------------------------------- |
| range    | string | Worksheet 1! A1:C3 | Refer to the following parameter description [range](#range)       |
| resource | object |                | Refer to the following parameters to describe [resource](#resource) |

**Sample request**
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

**HTTP status code**

| Status code | illustrate     |
| :----- | :------- |
| 204    | The return is successful |

**HTTP Response Body**

empty

**Supported file types**

| file type | Type value      |
| :------- | :---------- |
| form     | spreadsheet |

### The contents of the table are appended

Append the table content based on the incoming range (only plain text and numbers are supported). The logic for finding append locations based on range is as follows:
Find the row position that needs to be appended:
a. Don't specify a range of cells, e.g. Sheet 1: will go through the whole table to find the last row with data
b. Specify a range of cells, e.g. Sheet 1! C5:E8: will find the row where the starting cell C5 is located, traverse down from that row, find the last consecutive row with data and insert N rows downward, populating values, where N = values.length
If a non-existent column is modified, it is automatically added to the column

**The address of the request**

_Put _ Hotpas://shimo-domain/SDK/V2/API/Files/{filed}/sheets/values

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Examples of values                               | Required | illustrate                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | be   | The file ID is located in the URL path of the API to create the preview API, and this file ID is a unique ID in the file list of the access service provider, and Graphite will request the file interface of the access service provider to obtain the file information and download address based on this ID |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                                                                     |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The token provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider                                        |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                                                          |

**HTTP Request Body**

| The name of the field   | type   | Examples of values         | illustrate                                   |
| :------- | :----- | :------------- | :------------------------------------- |
| range    | string | Worksheet 1! A1:C3 | Refer to the following parameter description [range](#range)       |
| resource | object |                | Refer to the following parameters to describe [resource](#resource) |

**Sample request**
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

**HTTP status code**

| Status code | illustrate     |
| :----- | :------- |
| 204    | The return is successful |

**HTTP Response Body**

empty

**Supported file types**

| file type | Type value      |
| :------- | :---------- |
| form     | spreadsheet |

### Delete table rows

Description: Starting with index, delete the number of rows with the quantity count

**The address of the request**

_delete_  hatps://shimo-domain/sdk/v2/api/files/{filed}/sheets/{sheetname}/rose/{index}

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Examples of values                               | Required | illustrate                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | be   | The file ID is located in the URL path of the API to create the preview API, and this file ID is a unique ID in the file list of the access service provider, and Graphite will request the file interface of the access service provider to obtain the file information and download address based on this ID |
| Path     | sheetName | string | Worksheet 1                             | be   | The name of the worksheet in the table                                                                                                                               |
| Path     | index     | number | 0                                    | be   | Delete from the first few lines                                                                                                                                 |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                                                                     |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The token provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider                                        |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                                                          |
| Query    | count     | number | 1                                    | not   | Delete a few rows, default to 1                                                                                                                               |

**HTTP Request Body**

empty

**Sample request**
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

**HTTP status code**

| Status code | illustrate     |
| :----- | :------- |
| 204    | The return is successful |

**HTTP Response Body**

empty

**Supported file types**

| file type | Type value      |
| :------- | :---------- |
| form     | spreadsheet |

### Added tabular worksheets

Note: A worksheet has been added

**The address of the request**

_Post_  Hatps://shimo-domain/sdk/v2/api/files/{filed}/sheets/

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Examples of values                               | Required | illustrate                                                                                                                                             |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Path     | fileId    | string | qeK4Xdxvxg8jF5gz                     | be   | The file ID is located in the URL path of the API to create the preview API, and this file ID is a unique ID in the file list of the access service provider, and Graphite will request the file interface of the access service provider to obtain the file information and download address based on this ID |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                                                                     |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The token provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider                                        |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                                                          |

**HTTP Request Body**

| The name of the field | type   | Examples of values   | illustrate               |
| :----- | :----- | :------- | :----------------- |
| name   | string | Worksheet 2 | Added table worksheet name |

**Sample request**
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

**HTTP status code**

| Status code | illustrate     |
| :----- | :------- |
| 204    | The return is successful |

**HTTP Response Body**

empty

**Supported file types**

| file type | Type value      |
| :------- | :---------- |
| form     | spreadsheet |

### Parameter description

#### range

The format is ${worksheet}!${cell range}, and the specification is as follows:

- The format of the cell range is A1:C10 or A1, and the cell start position cannot exceed the largest row and column of the table
- If the worksheet name contains the 3 featured characters !:', you must wrap the worksheet name in single quotation marks.
  Example: Worksheet 1 | Worksheet 1! A1 | Worksheet 1! A1:C1 | 'Work!: Table'! A2:C3

#### resource

The type is object, which must contain the key value "values".
values is a two-dimensional array that represents appended/updated table contents, with the first dimension representing rows and the second dimension representing columns. An empty string means that the current cell is empty.
Example:

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

### Date type OADate numeric conversion {#oadate-conversion-example}

The return value of the date type is a value of the OADate type, for example, the date in the cell is `2022/7/27` ,  and the returned value is `44769` . 

Refer to the following JavaScript code to convert:

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

## **Seat management**

Due to the peculiarities of the seat management API, for security reasons, you need to[ append  the field when ](./resources.md#签名)generating the signature`"scope": "license"`. 

### Get a list of users and seat status

**The address of the request**

_GET_ http(s)://shimo-domain/sdk/v2/api/license/users

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Examples of values                               | Required | illustrate                                                                                                      |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :-------------------------------------------------------------------------------------------------------- |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                              |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The token provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                   |
| Query    | page      | number | 1                                    | be   | Request page number                                                                                                  |
| Query    | size      | number | 30                                   | be   | The number of users returned in a single page                                                                                          |

**Sample request**
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

Example of a response body:

```json
[
  {
    "userId": "1",
    "createdAt": "2021-05-20T14:56:53+08:00",
    "status": 1
  }
]
```

| The name of the field    | type   | Examples of values                    | illustrate                                            |
| :-------- | :----- | :------------------------ | :---------------------------------------------- |
| userId    | string | 1                         | User ID                                         |
| createdAt | string | 2021-05-20T14 56:53+08:00 | Creation time                                        |
| status    | number | 1                         | Seat status<br/>: 1, Active<br/>, 0, <br/> Disabled, -1, Not enabled |

#### User Status Description {#user-status-desc}

:::tip status description is not enabled
**`未启用` When will the state exist?**

If the accessor has integrated and implemented [`at 人`](./impl#feature-mention-at)related functions, when calling the accessor's search interface, the users who are not associated in the Graphite SDK may be returned, and because such users need to be displayed in the at drop-down list in the graphite editor, the Graphite SDK will first establish a user association, and the user's status will be initialized as`未启用` the state, which  is the **`不会占用`** number of seats. 

**`未启用` The status is automatically activated**

When a user is not enabled, it will be automatically activated and become a state when the following operations are performed through this user identity, `激活` and the corresponding seat will be occupied at this time, and if the seat is insufficient, access will be denied

- Create a file
- Create a copy
- Visit the edit page
- Import and export files
- Preview the file
- Invoke the API for manipulating the contents of a table
- Get the historical information and version information of the file sidebar
- Get the plain text information of the file
- Get a list of files at people
- Get file comment data
  :::

### Activate user seats

**The address of the request**

_POST_ http(s)://shimo-domain/sdk/v2/api/license/users/activate

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Examples of values                               | Required | illustrate                                                                                                      |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :-------------------------------------------------------------------------------------------------------- |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                              |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The token provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                   |

**HTTP Request Body**

Example of a request body:

| The name of the field  | type     | Examples of values     | Required | illustrate                       |
| :------ | :------- | :--------- | :--- | :------------------------- |
| userIds | []string | ["1", "2"] | be   | A list of user IDs that need to be activated for the seat |

**Sample request**
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

empty

**HTTP status code**

| Status code | illustrate     |
| :----- | :------- |
| 204    | The operation was successful |

### Cancel the user's seat

**The address of the request**

_POST_ http(s)://shimo-domain/sdk/v2/api/license/users/deactivate

**HTTP Request Parameters**

| Parameter transfer method | The name of the parameter    | type   | Examples of values                               | Required | illustrate                                                                                                     |
| :------- | :-------- | :----- | :----------------------------------- | :--- | :------------------------------------------------------------------------------------------------------- |
| Query    | appId     | string | ebc1cde3-9b57-4962-883d-54302d428600 | be   | Access the AppId obtained by the service provider from graphite                                                                             |
| Query    | token     | string | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | be   | The token provided by the access service provider will be passed as a value in the HTTP Headers X-Shimo-Token when Graphite requests the interface of the access service provider |
| Query    | signature | string | Refer to the section on how to sign them                     | be   | Access the service provider, and use the AppId and AppSecret obtained from graphite to generate the string according to the signature method                                  |

**HTTP Request Body**

Example of a request body:

| The name of the field  | type     | Examples of values     | Required | illustrate                       |
| :------ | :------- | :--------- | :--- | :------------------------- |
| userIds | []string | ["1", "2"] | be   | A list of user IDs that need to be deseated |

**Sample request**
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

empty

**HTTP status code**

| Status code | illustrate     |
| :----- | :------- |
| 204    | The operation was successful |
