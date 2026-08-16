# Online_CF_opencode

edgetunnel 订阅转换自定义配置仓库。

所有 `ACL4SSR` 预设规则均注入了 **opencode.ai 官方入口规则**，用于解决 opencode API 走代理时 TLS 握手不稳定 / 502 的问题。

## 做了什么

在每个 `.ini` 配置中注入了：

```ini
;====== 新增：opencode 走官方入口（最高优先级，必须放最前）======
ruleset=🤖 opencode官方,[]DOMAIN-SUFFIX,opencode.ai
ruleset=🤖 opencode官方,[]DOMAIN-SUFFIX,opencode.dev
```

并新增策略组（默认选中官方入口 ZeroTrust，可在客户端随时切换 WireGuard / MASQUE / 手动切换 / 节点选择）：

```ini
custom_proxy_group=🤖 opencode官方`select`[]官方入口 | ZeroTrust`[]官方入口 | WireGuard`[]官方入口 | MASQUE`[]☑️ 手动切换`[]🚀 节点选择`.*
```

原版预设规则内容未做任何改动。

## 文件清单

URL 规律：`https://raw.githubusercontent.com/18773616071/Online_CF_opencode/main/<文件名>`

### ACL4SSR 规则（17 个）

| 预设 | 文件名 |
|---|---|
| 默认版 分组比较全 ACL4SSR_Online | acl4ssr_ACL4SSR_Online.ini |
| 更多去广告 ACL4SSR_Online_AdblockPlus | acl4ssr_ACL4SSR_Online_AdblockPlus.ini |
| 多国分组 ACL4SSR_Online_MultiCountry | acl4ssr_ACL4SSR_Online_MultiCountry.ini |
| 无自动测速 ACL4SSR_Online_NoAuto | acl4ssr_ACL4SSR_Online_NoAuto.ini |
| 无广告拦截规则 ACL4SSR_Online_NoReject | acl4ssr_ACL4SSR_Online_NoReject.ini |
| 精简版 ACL4SSR_Online_Mini | acl4ssr_ACL4SSR_Online_Mini.ini |
| 精简版 更多去广告 ACL4SSR_Online_Mini_AdblockPlus | acl4ssr_ACL4SSR_Online_Mini_AdblockPlus.ini |
| 精简版 不带自动测速 ACL4SSR_Online_Mini_NoAuto | acl4ssr_ACL4SSR_Online_Mini_NoAuto.ini |
| 精简版 带故障转移 ACL4SSR_Online_Mini_Fallback | acl4ssr_ACL4SSR_Online_Mini_Fallback.ini |
| 精简版 自动测速、故障转移、负载均衡 ACL4SSR_Online_Mini_MultiMode | acl4ssr_ACL4SSR_Online_Mini_MultiMode.ini |
| 精简版 带港美日国家 ACL4SSR_Online_Mini_MultiCountry | acl4ssr_ACL4SSR_Online_Mini_MultiCountry.ini |
| 全分组 重度用户使用 ACL4SSR_Online_Full | acl4ssr_ACL4SSR_Online_Full.ini |
| 全分组 多模式 重度用户使用 ACL4SSR_Online_Full_MultiMode | acl4ssr_ACL4SSR_Online_Full_MultiMode.ini |
| 全分组 无自动测速 重度用户使用 ACL4SSR_Online_Full_NoAuto | acl4ssr_ACL4SSR_Online_Full_NoAuto.ini |
| 全分组 重度用户使用 更多去广告 ACL4SSR_Online_Full_AdblockPlus | acl4ssr_ACL4SSR_Online_Full_AdblockPlus.ini |
| 全分组 重度用户使用 奈飞全量 ACL4SSR_Online_Full_Netflix | acl4ssr_ACL4SSR_Online_Full_Netflix.ini |
| 全分组 重度用户使用 谷歌细分 ACL4SSR_Online_Full_Google | acl4ssr_ACL4SSR_Online_Full_Google.ini |

### CM 自用规则（9 个）

| 预设 | 文件名 |
|---|---|
| 默认版 识别港美地区 CM_Online | cm_ACL4SSR_Online.ini |
| 精简版 自动测速、故障转移、负载均衡 识别CloudFlareCDN CM_Online_Mini_MultiMode_CF | cm_ACL4SSR_Online_Mini_MultiMode_CF.ini |
| 精简版 不带自动测速(有效减少-1情况) 识别CloudFlareCDN CM_Online_Mini_NoAuto_CF | cm_ACL4SSR_Online_Mini_NoAuto_CF.ini |
| 识别港美地区 负载均衡 CM_Online_MultiCountry | cm_ACL4SSR_Online_MultiCountry.ini |
| 识别港美地区、CloudFlareCDN 负载均衡 CF节点专用 CM_Online_MultiCountry_CF | cm_ACL4SSR_Online_MultiCountry_CF.ini |
| 识别多地区分组 CM_Online_Full | cm_ACL4SSR_Online_Full.ini |
| 识别多地区、CloudFlareCDN 分组 CF节点专用 CM_Online_Full_CF | cm_ACL4SSR_Online_Full_CF.ini |
| 识别多地区 负载均衡 CM_Online_Full_MultiMode | cm_ACL4SSR_Online_Full_MultiMode.ini |
| 识别多地区、CloudFlareCDN 负载均衡 CF节点专用 CM_Online_Full_MultiMode_CF | cm_ACL4SSR_Online_Full_MultiMode_CF.ini |

## 使用方法

1. 登录 edgetunnel 管理后台（`https://<你的域名>/admin`）
2. 进入「🔄 订阅转换配置」
3. 「订阅转换配置文件」下拉框选择 **自定义**
4. 在 URL 输入框填入对应预设的 raw 地址，例如当前默认使用的 CM_Online：

   ```
   https://raw.githubusercontent.com/18773616071/Online_CF_opencode/main/cm_ACL4SSR_Online.ini
   ```

5. 保存后重新拉取订阅（`/sub?target=clash&token=...`），确认生成的配置中包含 `opencode.ai` 规则和 `🤖 opencode官方` 策略组

## 注意事项

- ⚠️ 切换订阅转换预设会覆盖 SUBCONFIG 配置，opencode 自定义规则会丢失；需要换预设时，重新选择「自定义」并填入对应文件的 URL
- 原版配置来源：
  - [ACL4SSR/ACL4SSR](https://github.com/ACL4SSR/ACL4SSR)
  - [cmliu/ACL4SSR](https://github.com/cmliu/ACL4SSR)
  - 预设列表定义见 [cmliu/cmliu 的 SUBCONFIG.json](https://raw.githubusercontent.com/cmliu/cmliu/main/SUBCONFIG.json)
