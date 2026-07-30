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
