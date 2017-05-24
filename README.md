# ApiWay

> Cloud based API Test Runner
## Build 

### Vue

``` bash
# Change directory
cd vue

# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# localhost 환경에서 github API를 사용시 apiway.io/login에서 Github Authentication을 완료한 후 localstorage에 저장된 token을 아래 파일에 추가해야 함
- ${TOP}/vue/config/local.env.js -
exports.token = {
  github: 'xxxxxxxxxxxx',
  bitbucket: ''
}

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report

# run unit tests
npm run unit

# run e2e tests
npm run e2e

# run all tests
npm test
```

