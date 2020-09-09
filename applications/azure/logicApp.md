# Logic App

## 抓取 body 內容

```txt
# `進入點` 的 body
@{triggerBody()?['object_attributes']?['title']}

# `http1` 的 body
@{body('http1')?['data']?['create_item']?['id']}
```

## schema

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
