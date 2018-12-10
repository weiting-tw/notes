# NPM

## 建立私人倉儲 private registry - [verdaccio](https://github.com/verdaccio/verdaccio)

1. using [docker image](https://hub.docker.com/r/verdaccio/verdaccio/)

2. set registry
    > npm set registry <http://localhost:4873>

3. add user
    > npm adduser --registry <http://localhost:4873>

    example:
    ```sh
    Username: test
    Password: test
    Email: test@test.com
    ```
4. publish
    > npm publish --registry <http://localhost:4873>

## NPM install downgrading resolved packages from https to http registry in package-lock.json

```sh
sed -i -e 's/http:\/\//https:\/\//g' package-lock.json
```
