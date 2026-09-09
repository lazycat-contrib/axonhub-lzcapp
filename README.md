# axonhub-lzcapp

AxonHub 的懒猫微服应用包。

## 自动发布

GitHub Actions 每天检查 `docker.io/looplj/axonhub`，也支持手动触发：

```bash
gh workflow run lazycat.yml --field channel=auto
```

- beta 版本使用 `1.0.0-beta1` 形式，只发布到喵喵私有商店。
- 正式版本使用 `1.0.0` 形式，同时发布到喵喵私有商店和懒猫应用商店。
- 每个版本都会生成 `community.lazycat.app.axonhub-v<version>.lpk` Release Asset。

工作流引用已有的 `LAZYCAT_TOKEN`、`APPSTORE_URL`、`APPSTORE_TOKEN`，以及可选的
`APP_ID` 和 `PRIVATE_STORE_GROUP_CODES` GitHub Actions Secrets。

## 向其他应用提供 API

本应用作为 API 服务端供其他应用调用，要求 lzcos **1.5.2+**。配置保留 `public_path: /` 和现有 HTTP upstream，并启用 `application.background_task: true`。不需要为被调用而申请 `lzcapp.user_delegate` 权限，也不使用 ctx 注入。

| 接入方式 | OpenAI 兼容 Base URL | 条件 |
|----------|----------------------|------|
| 微服应用间委托访问 | `http://app.community.lazycat.app.axonhub.lzcx/v1` | 调用方声明 `lzcapp.user_delegate` 并携带真实用户的 `X-HC-USER-TICKET` |
| 微服客户端网络访问 | `https://axonhub.<设备域名>/v1` | 调用端须能访问微服虚拟网络；设备域名以实际应用 URL 为准 |

应用 API Key 通过 `Authorization: Bearer <API Key>` 发送。API Key 在 AxonHub 中创建。多实例部署时，`.lzcx` 按真实用户票据路由到对应实例。

`public_path` 使请求不必经过平台网页登录，但不会取消网络隔离或 `.lzcx` 的委托票据要求。只支持 Base URL 和 API Key 的调用程序，不能假定它会自动取得 `.lzcx` 票据；需要在调用方接入平台委托机制或先确认上述客户端网络地址可达。不要用内部 `service.<package>.lzcapp` 地址跨应用直连。

部署后从实际调用方请求 `/v1/models`，确认返回模型列表 JSON，再按实际模型发起推理请求；关闭浏览器后重复请求，确认后台服务仍运行。遇到登录页或委托错误检查网络与平台票据，遇到应用 401 检查 API Key。

参考：[应用间访问](https://developer.lazycat.cloud/advanced-app-interconnect.html)。
