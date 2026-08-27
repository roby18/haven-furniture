# Haven 家具独立站 — 上线部署指南

## 你现在拿到的是什么

一个功能完整的家具电商独立站（单文件 HTML），包含：
- 首页、产品列表、产品详情、购物车、4步结账、博客、心愿单、登录弹窗
- PayPal 真实支付按钮（填入 Client ID 后即可收款）
- 信用卡支付表单（接 PayPal 通道，支持 Visa/Mastercard/Amex）
- 订单自动发送到你的邮箱（通过 FormSubmit.co，免费）
- 完整法律页面（隐私政策、服务条款、退换货、配送、保修、FAQ、联系我们）
- 优惠券功能（WELCOME10）
- 响应式设计，手机/平板/电脑都适配

**你不需要买服务器，不需要懂编程。** 按下面 5 步操作即可上线。

---

## 第一步：注册 PayPal Business 账号（免费，约 30 分钟）

1. 访问 https://www.paypal.com/business
2. 用你的邮箱注册 Business 账号（需要个体工商户或公司营业执照）
3. 完成身份验证和银行账户绑定
4. 注册成功后，进入 PayPal Developer Dashboard：https://developer.paypal.com/dashboard/
5. 点击 **Apps & Credentials** → 选择 **Live** 模式 → **Create App**
6. App Name 填 "Haven Store"，创建后你会看到一个 **Client ID**（类似 `AeA1QIZXiflr1_-rU0jX...` 的字符串）
7. 复制这个 Client ID，后面要用

> 注意：先在 Sandbox 模式测试，确认没问题再切 Live 模式。

---

## 第二步：配置网站（约 5 分钟）

用记事本或任何文本编辑器打开 `index.html`，找到顶部的配置区域（大约第 20 行）：

```javascript
const CONFIG = {
  storeEmail: 'your-email@gmail.com',  // ← 改成你接收订单的邮箱
  paypalClientId: 'YOUR_CLIENT_ID'     // ← 改成第一步拿到的 PayPal Client ID
};
```

同时，找到 `<head>` 里的 PayPal SDK 引用（大约第 10 行）：

```html
<script src="https://www.paypal.com/sdk/js?client-id=YOUR_CLIENT_ID&currency=USD" ...></script>
```

把这里的 `YOUR_CLIENT_ID` 也替换成你的真实 Client ID（两处都要改）。

---

## 第三步：部署到免费托管（约 10 分钟）

### 推荐方案：Cloudflare Pages（免费、速度快、自带 CDN）

1. 访问 https://pages.cloudflare.com/ 注册账号（免费）
2. 点击 **Create a project** → **Upload assets**
3. 把 `index.html` 文件直接拖进去上传
4. Project name 填 `haven-furniture`（会成为你的临时域名）
5. 点击 **Deploy site**
6. 部署完成后你会得到一个免费域名，类似 `haven-furniture.pages.dev`

### 备选方案：Netlify（同样免费）

1. 访问 https://www.netlify.com/ 注册
2. 直接把 `index.html` 拖到 Netlify 的部署区域
3. 立刻获得一个免费链接

### 备选方案：Vercel

1. 访问 https://vercel.com/ 注册
2. New Project → Upload `index.html`
3. 部署完成

---

## 第四步：绑定你自己的域名（约 30 分钟）

### 购买域名
- 在 Namecheap（https://www.namecheap.com）或 Cloudflare Registrar 购买一个 `.com` 域名
- 费用约 $10-12/年
- 建议域名和品牌名一致，如 `havenfurniture.com`、`shophaven.com` 等

### 绑定域名到 Cloudflare Pages
1. 在 Cloudflare Pages 项目页面 → **Custom domains** → **Set up a custom domain**
2. 输入你购买的域名
3. 按提示在域名注册商处添加 DNS 记录（Cloudflare 会给你具体指引）
4. 等待 DNS 生效（几分钟到几小时）
5. 自动获得免费 SSL 证书（https）

---

## 第五步：激活订单邮件通知（约 2 分钟）

网站使用 FormSubmit.co 免费发送订单邮件，不需要注册：

1. 网站部署上线后，自己下一个测试订单
2. 你填的邮箱（`storeEmail`）会收到一封 FormSubmit 的激活邮件
3. 点击邮件中的激活链接即可
4. 之后每笔订单都会自动发送到这个邮箱，包含：
   - 订单号
   - 商品明细和数量
   - 金额（小计/税/折扣/总计）
   - 支付方式

---

## 上线后检查清单

- [ ] PayPal Client ID 已替换（两处）
- [ ] 邮箱已替换为真实邮箱
- [ ] PayPal 账号已完成验证，可以收款
- [ ] 网站已部署，能通过 https 访问
- [ ] 自己下了一个 Sandbox 测试订单，流程正常
- [ ] FormSubmit 邮件已激活
- [ ] 手机上打开网站检查显示正常
- [ ] 所有链接和按钮都能点击
- [ ] 法律页面内容已根据你的实际情况调整（特别是退货地址、联系方式）

---

## 日常运营

### 收到订单后
1. 邮箱收到订单通知
2. 登录大健云仓后台，按客户地址下单采购
3. 大健云仓直接发货给客户
4. 把物流单号发给客户（手动发邮件，或后续接入自动通知）

### 产品更新
- 编辑 `index.html` 中的 `products` 数组
- 每个产品包含：id、name、cat、price、was、rating、reviews、img、badge、desc
- 图片可以用大健云仓提供的产品图 URL，也可以自己上传到图床
- 修改后重新部署到 Cloudflare Pages（拖拽上传即可）

### 关键数据位置（在 index.html 中搜索）
| 内容 | 搜索关键词 |
|------|-----------|
| 产品数据 | `const products=` |
| 网站配置 | `const CONFIG=` |
| 优惠券代码 | `WELCOME10` |
| 公告栏文字 | `Free shipping over $99` |
| 品牌名 | `Haven`（全局替换） |
| 联系邮箱 | `hello@haven.com` |
| 客服电话 | `1-800-555-1234` |

---

## 费用总结

| 项目 | 费用 | 必要性 |
|------|------|--------|
| 域名 | $10-12/年 | 必须 |
| Cloudflare Pages 托管 | $0 | 必须 |
| PayPal Business | $0（手续费 2.99%+$0.49/笔） | 必须 |
| FormSubmit 邮件 | $0 | 必须 |
| SSL 证书 | $0（Cloudflare 自动提供） | 必须 |
| **合计启动成本** | **约 $10-12** | |

后续可选投入：
- Google/Facebook 广告费（按你的预算）
- 付费邮箱（Google Workspace $6/月，用 Gmail 更专业）
- 付费主题/插件（等有单量了再考虑）

---

## 下一步：从测试到出单

1. **第1周**：完成上述部署，用 PayPal Sandbox 测试 3-5 次下单流程
2. **第2周**：从大健云仓选 8-10 个产品，替换网站上的示例产品（名称、价格、图片）
3. **第3周**：开始引流——每天发 3-5 条 Pinterest，Google Ads 日预算 $10-20 测试
4. **第4周**：根据数据优化——有转化的品加预算，没转化的品换掉
5. **第2个月起**：稳定出单后，考虑迁移到 Shopify 或 WooCommerce，获得更完整的后台管理

---

有问题随时问。
