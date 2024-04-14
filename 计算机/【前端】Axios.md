# Axios - 最常用的网络请求库

> 本站作者：[微雨星晗](https://github.com/WeiYuXingHan)
>
> 本站地址：[https://wyssixsixsix.top](https://wyssixsixsix.top)

## Axios 是什么?


Axios 是一个基于 `Promise` 的 `HTTP` 客户端。它由 `Matt Zabriskie` 开发，因其**简单易用、功能强大**而广受欢迎。axios 能够处理各种类型的 `HTTP` 请求，包括 `GET、POST、PUT、DELETE` 等，并支持拦截请求和响应、转换请求和响应数据、自动转换 `JSON` 数据等特性。

Axios 作用于 `node.js` 和浏览器中。 它是 `isomorphic` 的 (即同一套代码可以运行在浏览器和node.js中)。在服务端它使用原生 `node.js http` 模块, 而在客户端 (浏览端) 则使用 `XMLHttpRequests`。

## 特性
- 从浏览器创建 `XMLHttpRequests` (XHR)
- 从 node.js 创建 `http` 请求
- 支持 `Promise API`
- **拦截**请求和响应
- **转换**请求和响应数据
- **取消**请求
- 超时处理
- 查询参数序列化支持嵌套项处理
- 自动将请求体序列化为：
  - JSON (`application/json`)
  - Multipart / FormData (`multipart/form-data`)
  - URL encoded form (`application/x-www-form-urlencoded`)
- 将 HTML Form 转换成 JSON 进行请求
- 自动转换JSON数据
- 获取浏览器和 node.js 的请求进度，并提供额外的信息（速度、剩余时间）
- 为 node.js 设置带宽限制
- 兼容符合规范的 FormData 和 Blob（包括 node.js）
- 客户端支持防御XSRF
  
## 安装

在此仅展示使用 `npm` 和 `yarn` 安装:

```bash
$ npm install axios
$ yarn add axios
```

## GET请求用例(CommonJS语法)
```js
const axios = require('axios');
axios.get('user/?id=12345')
  .then(function (response) {
    // 处理成功情况
    console.log(response);
  })
  .catch(function (error) {
    // 处理错误情况
    console.log(error);
  })
  .then(function () {
    // 总是会执行
  });
```

## POST请求用例(CommonJS语法)
```js
const axios = require('axios');
axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

## Axios的请求API及别名
Axios库提供了多种别名方法，用于发起不同的HTTP请求。这些别名方法可以简化代码，提高开发效率。
```js
axios.request(config) // 接受一个配置对象作为参数，配置对象可以包含请求的所有细节，如 URL、HTTP 方法、请求头、数据、超时等
axios.get(url[, config]) // GET 请求，传递 URL 作为第一个参数，可选的配置对象作为第二个参数
axios.delete(url[, config]) // HTTP DELETE 请求
axios.head(url[, config]) // HTTP HEAD 请求，通常用于获取资源的元数据，而不获取资源本身
axios.options(url[, config]) // HTTP OPTIONS 请求，可以用来描述 HTTP 接口的能力
axios.post(url[, data[, config]]) // HTTP POST 请求，并可以传递数据作为请求体
axios.put(url[, data[, config]]) // HTTP PUT 请求，通常用于更新资源
axios.patch(url[, data[, config]]) // HTTP PATCH 请求，用于部分更新资源
axios.postForm(url[, data[, config]]) // HTTP POST 请求，data 参数将被转换为 application/x-www-form-urlencoded 格式的字符串
axios.putForm(url[, data[, config]]) // HTTP PUT 请求，data 参数将被转换为 application/x-www-form-urlencoded 格式的字符串
axios.patchForm(url[, data[, config]]) // HTTP PATCH 请求，data 参数将被转换为 application/x-www-form-urlencoded 格式的字符串
```
在实际开发中，可以根据需要选择使用哪种方式。如果你只需要发起一个简单的 GET 请求，使用简写形式会更简洁。如果你需要设置额外的请求选项，如请求头或请求体，那么使用 axios 函数会更合适。

## Axios实例
#### 创建一个实例
您可以使用自定义配置新建一个实例。
```js
axios.create([config])
const instance = axios.create({
  baseURL: 'https://some-domain.com/api/',
  timeout: 1000,
  headers: {'X-Custom-Header': 'foobar'}
});
```

## Axios请求与响应
#### 请求配置
🔑注意：这些是创建请求时可以用的配置选项。只有 url 是必需的。如果没有指定 method，请求将默认使用 GET 方法
```ts
{
  // `url` 是用于请求的服务器 URL
  url: '/user',

  // `method` 是创建请求时使用的方法
  method: 'get', // 默认值

  // `baseURL` 将自动加在 `url` 前面，除非 `url` 是一个绝对 URL。
  // 它可以通过设置一个 `baseURL` 便于为 axios 实例的方法传递相对 URL
  baseURL: 'https://some-domain.com/api/',

  // `transformRequest` 允许在向服务器发送前，修改请求数据
  // 它只能用于 'PUT', 'POST' 和 'PATCH' 这几个请求方法
  // 数组中最后一个函数必须返回一个字符串， 一个Buffer实例，ArrayBuffer，FormData，或 Stream
  // 你可以修改请求头。
  transformRequest: [function (data, headers) {
    // 对发送的 data 进行任意转换处理

    return data;
  }],

  // `transformResponse` 在传递给 then/catch 前，允许修改响应数据
  transformResponse: [function (data) {
    // 对接收的 data 进行任意转换处理

    return data;
  }],

  // 自定义请求头
  headers: {'X-Requested-With': 'XMLHttpRequest'},

  // `params` 是与请求一起发送的 URL 参数
  // 必须是一个简单对象或 URLSearchParams 对象
  params: {
    ID: 12345
  },

  // `paramsSerializer`是可选方法，主要用于序列化`params`
  // (e.g. https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
  paramsSerializer: function (params) {
    return Qs.stringify(params, {arrayFormat: 'brackets'})
  },

  // `data` 是作为请求体被发送的数据
  // 仅适用 'PUT', 'POST', 'DELETE 和 'PATCH' 请求方法
  // 在没有设置 `transformRequest` 时，则必须是以下类型之一:
  // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
  // - 浏览器专属: FormData, File, Blob
  // - Node 专属: Stream, Buffer
  data: {
    firstName: 'Fred'
  },
  
  // 发送请求体数据的可选语法
  // 请求方式 post
  // 只有 value 会被发送，key 则不会
  data: 'Country=Brasil&City=Belo Horizonte',

  // `timeout` 指定请求超时的毫秒数。
  // 如果请求时间超过 `timeout` 的值，则请求会被中断
  timeout: 1000, // 默认值是 `0` (永不超时)

  // `withCredentials` 表示跨域请求时是否需要使用凭证
  withCredentials: false, // default

  // `adapter` 允许自定义处理请求，这使测试更加容易。
  // 返回一个 promise 并提供一个有效的响应 （参见 lib/adapters/README.md）。
  adapter: function (config) {
    /* ... */
  },

  // `auth` HTTP Basic Auth
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },

  // `responseType` 表示浏览器将要响应的数据类型
  // 选项包括: 'arraybuffer', 'document', 'json', 'text', 'stream'
  // 浏览器专属：'blob'
  responseType: 'json', // 默认值

  // `responseEncoding` 表示用于解码响应的编码 (Node.js 专属)
  // 注意：忽略 `responseType` 的值为 'stream'，或者是客户端请求
  // Note: Ignored for `responseType` of 'stream' or client-side requests
  responseEncoding: 'utf8', // 默认值

  // `xsrfCookieName` 是 xsrf token 的值，被用作 cookie 的名称
  xsrfCookieName: 'XSRF-TOKEN', // 默认值

  // `xsrfHeaderName` 是带有 xsrf token 值的http 请求头名称
  xsrfHeaderName: 'X-XSRF-TOKEN', // 默认值

  // `onUploadProgress` 允许为上传处理进度事件
  // 浏览器专属
  onUploadProgress: function (progressEvent) {
    // 处理原生进度事件
  },

  // `onDownloadProgress` 允许为下载处理进度事件
  // 浏览器专属
  onDownloadProgress: function (progressEvent) {
    // 处理原生进度事件
  },

  // `maxContentLength` 定义了node.js中允许的HTTP响应内容的最大字节数
  maxContentLength: 2000,

  // `maxBodyLength`（仅Node）定义允许的http请求内容的最大字节数
  maxBodyLength: 2000,

  // `validateStatus` 定义了对于给定的 HTTP状态码是 resolve 还是 reject promise。
  // 如果 `validateStatus` 返回 `true` (或者设置为 `null` 或 `undefined`)，
  // 则promise 将会 resolved，否则是 rejected。
  validateStatus: function (status) {
    return status >= 200 && status < 300; // 默认值
  },

  // `maxRedirects` 定义了在node.js中要遵循的最大重定向数。
  // 如果设置为0，则不会进行重定向
  maxRedirects: 5, // 默认值

  // `socketPath` 定义了在node.js中使用的UNIX套接字。
  // e.g. '/var/run/docker.sock' 发送请求到 docker 守护进程。
  // 只能指定 `socketPath` 或 `proxy` 。
  // 若都指定，这使用 `socketPath` 。
  socketPath: null, // default

  // `httpAgent` and `httpsAgent` define a custom agent to be used when performing http
  // and https requests, respectively, in node.js. This allows options to be added like
  // `keepAlive` that are not enabled by default.
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),

  // `proxy` 定义了代理服务器的主机名，端口和协议。
  // 您可以使用常规的`http_proxy` 和 `https_proxy` 环境变量。
  // 使用 `false` 可以禁用代理功能，同时环境变量也会被忽略。
  // `auth`表示应使用HTTP Basic auth连接到代理，并且提供凭据。
  // 这将设置一个 `Proxy-Authorization` 请求头，它会覆盖 `headers` 中已存在的自定义 `Proxy-Authorization` 请求头。
  // 如果代理服务器使用 HTTPS，则必须设置 protocol 为`https`
  proxy: {
    protocol: 'https',
    host: '127.0.0.1',
    port: 9000,
    auth: {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  },

  // see https://axios-http.com/zh/docs/cancellation
  cancelToken: new CancelToken(function (cancel) {
  }),

  // `decompress` indicates whether or not the response body should be decompressed 
  // automatically. If set to `true` will also remove the 'content-encoding' header 
  // from the responses objects of all decompressed responses
  // - Node only (XHR cannot turn off decompression)
  decompress: true // 默认值

}
```

#### 响应结构
一个请求的响应包含以下信息：
```js
{
  // `data` 由服务器提供的响应
  data: {},

  // `status` 来自服务器响应的 HTTP 状态码
  status: 200,

  // `statusText` 来自服务器响应的 HTTP 状态信息
  statusText: 'OK',

  // `headers` 是服务器响应头
  // 所有的 header 名称都是小写，而且可以使用方括号语法访问
  // 例如: `response.headers['content-type']`
  headers: {},

  // `config` 是 `axios` 请求的配置信息
  config: {},

  // `request` 是生成此响应的请求
  // 在node.js中它是最后一个ClientRequest实例 (in redirects)，
  // 在浏览器中则是 XMLHttpRequest 实例
  request: {}
}
```
当使用 `then` 时，你可以接收如下响应:
```js
axios.get('/user/12345')
  .then(function (response) {
    console.log(response.data); 
    console.log(response.status);
    console.log(response.statusText);
    console.log(response.headers);
    console.log(response.config);
  });
```

## Axios拦截器
Axios 允许你添加请求和响应拦截器，可以在**请求发送前**或**响应返回后**进行自定义处理，如设置认证头、处理错误等。
我们可以在请求或响应被 `then` 或 `catch` 处理前拦截它们。例如：
#### 添加请求拦截器
```js
axios.interceptors.request.use(function (config) {
  // 在发送请求之前可以修改 config

  // 例如，添加一个 Authorization 头
  config.headers.Authorization = 'Bearer ' + localStorage.getItem('token');

  return config;
}, function (error) {
  // 对请求错误做些什么

  return Promise.reject(error);
});
```
#### 添加响应拦截器
```js
axios.interceptors.response.use(function (response) {
  // 对响应数据做点什么,例如，根据响应状态码进行不同的处理
  switch (response.status) {
    case 200: // 处理成功的响应
      break;
    case 401: // 处理未授权的错误，可能需要重新登录
      break;
    default: // 处理其他错误
  }
  return response;
}, function (error) {
  // 对响应错误做点什么,例如，如果服务器返回 500 错误，你可以在这里处理
  if (error.response && error.response.status === 500) 
    console.error('服务器错误');
  return Promise.reject(error);
});
```
#### 拦截器的返回值
注意，在**请求拦截器**中，你必须返回 `config` 对象，这样 axios 才会使用你的配置发送请求。在**响应拦截器**中，你应该返回 `response` 对象，或者返回一个处理后的响应。
#### 取消拦截器
如果你需要取消之前添加的拦截器，可以使用 `axios.interceptors.request.eject` 和 `axios.interceptors.response.eject` 方法。
```js
// 取消请求拦截器
axios.interceptors.request.eject(yourInterceptorId);
// 取消响应拦截器
axios.interceptors.response.eject(yourInterceptorId);
```
## 可以给自定义的 axios 实例添加拦截器。
```js
const instance = axios.create();
instance.interceptors.request.use(function () {/*...*/});
```

## 取消请求
在 axios 中，取消请求是一个常见的需求，尤其是在处理用户输入或页面导航时，你可能需要取消一个尚未完成的请求以避免潜在的问题。axios 提供了几种方法来取消请求：
#### 使用 CancelToken
axios 提供了 `CancelToken` 类，允许你创建一个取消令牌并在请求时传递它。你可以在请求配置中使用这个令牌，然后在需要时调用取消函数。
> 🔑此 API 从 v0.22.0 开始已被弃用，不应在新项目中使用。
```js
// 创建一个 CancelToken
const CancelToken = axios.CancelToken;
const source = CancelToken.source();

// 发起请求时传递 cancel 函数
axios.get('/some-url', {
  cancelToken: source.token
}).then(response => {
  // 处理响应
}).catch(error => {
  if (axios.isCancel(error)) {
    console.log('Request canceled:', error.message);
  } else {
    // 处理其他错误
  }
});

// 取消请求
source.cancel('Operation canceled by the user.');
```
#### 使用 AbortController
在浏览器环境中，axios 也支持 `AbortController API`，它允许你通过发送一个 `AbortSignal` 来取消请求。
> 从 v0.22.0 开始，Axios 支持以 `fetch API` 方式—— `AbortController` 取消请求：
```js
// 创建一个 AbortController
const controller = new AbortController();

// 发起请求时传递 signal
axios.get('/some-url', {
  signal: controller.signal
}).then(response => {
  // 处理响应
});

controller.abort(); // 取消请求
```

## 请求体编码
默认情况下，axios将 JavaScript 对象序列化为 `JSON` 。 要以 `application/x-www-form-urlencoded` 格式发送数据，您可以使用以下选项之一。

#### 浏览器环境
在浏览器中，可以使用URLSearchParams API，如下所示：

```js
const params = new URLSearchParams();
params.append('param1', 'value1');
params.append('param2', 'value2');
axios.post('/foo', params);
```
请注意，不是所有的浏览器(参见 [caniuse.com](https://caniuse.com/))都支持 `URLSearchParams` ，但是可以使用 `polyfill` (确保 `polyfill` 全局环境)

或者, 您可以使用 `qs` 库编码数据:

```js
const qs = require('qs');
axios.post('/foo', qs.stringify({ 'bar': 123 }));
```

或者用另一种方式 **(ES6)**,
```js
import qs from 'qs';
const data = { 'bar': 123 };
const options = {
  method: 'POST',
  headers: { 'content-type': 'application/x-www-form-urlencoded' },
  data: qs.stringify(data),
  url,
};
axios(options);
```

#### Node.js环境
🎨在 Node.js 中，可以使用 `querystring` 模块，如下所示：

```js
const querystring = require('querystring');
axios.post('http://something.com/', querystring.stringify({ foo: 'bar' }));
```

或者从 `url module` 中使用 `URLSearchParams` ，如下所示:
```js
const url = require('url');
const params = new url.URLSearchParams({ foo: 'bar' });
axios.post('http://something.com/', params.toString());
```

🎨您也可以使用 `qs` 库。

🔑注意
如果需要对嵌套对象进行字符串化处理，则最好使用 `qs` 库，因为 `querystring` 方法在该用例中存在[已知问题](https://github.com/nodejs/node-v0.x-archive/issues/1665)。

🎨您可以使用 `form-data` 库，如下所示:
```js
const FormData = require('form-data');
 
const form = new FormData();
form.append('my_field', 'my value');
form.append('my_buffer', new Buffer(10));
form.append('my_file', fs.createReadStream('/foo/bar.jpg'));

axios.post('https://example.com', form, { headers: form.getHeaders() })
```

#### 自动序列化
当请求头中的 `content-type` 是 `application/x-www-form-urlencoded` 时，Axios 将自动地将普通对象序列化成 `urlencoded` 的格式。

在浏览器和 node.js 环境中都适用：
```js
const data = {
  x: xxx;
  y: yyy;
}
await axios.post('https://postman-echo.com/post', data,
  {headers: {'content-type': 'application/x-www-form-urlencoded'}}
);
```