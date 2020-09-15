# Logic App

## 抓取 body 內容

```txt
# `進入點` 的 body
@{triggerBody()?['object_attributes']?['title']}

# `http1` 的 body
@{body('http1')?['data']?['create_item']?['id']}
```

## Schema

### application/x-www-form-urlencoded

設定 `Content-Type`

`body` 直接使用 `param1=value1&param2=value2...`

```json
"actions": {
  "在monday建立item": {
    "inputs": {
      "body": "pulse[name]=@{triggerBody()?['object_attributes']?['title']}&user_id=@{split(decodeBase64(triggerOutputs()['headers']?['X-Gitlab-Token']),',')[1]}&update[text]=@{triggerBody()?['object_attributes']?['url']}\n\n@{triggerBody()?['object_attributes']?['description']}",
      "headers": {
          "Content-Type": "application/x-www-form-urlencoded"
      },
      "method": "POST",
      "uri": "https://api.monday.com:443/v1/boards/@{split(decodeBase64(triggerOutputs()['headers']?['X-Gitlab-Token']),',')[0]}/pulses.json?api_key=@{split(decodeBase64(triggerOutputs()['headers']?['X-Gitlab-Token']),',')[2]}"
    },
    "runAfter": {},
    "type": "Http"
  }
}
```

## [Outbound IP Addresses](https://docs.microsoft.com/zh-tw/azure/azure-functions/ip-addresses#function-app-outbound-ip-addresses)

每個函式應用程式都有一組可用的輸出 IP 位址。 任何來自應用程式的輸出連線 (例如連往後端資料庫) 都會使用其中一個可用的輸出 IP 位址作為來源 IP 位址。 您事先不知道指定的連線會使用哪個 IP 位址。 基於這個理由，您的後端服務必須對函式應用程式的所有輸出 IP 位址開啟其防火牆。

```bash
az webapp show --resource-group <group_name> --name <app_name> --query outboundIpAddresses --output tsv
az webapp show --resource-group <group_name> --name <app_name> --query possibleOutboundIpAddresses --output tsv
```
