# 用户体系配置指南（Firebase）

你的独立站已经内置完整用户体系。**不配置也能用**——此时是「本地演示模式」，账号数据存在访客自己浏览器里，换设备会丢失。配置 Firebase 后变为「云端真实账户」，支持跨设备、邮箱密码登录、订单和地址云端同步。

## 两种模式对比

| 能力 | 本地演示模式（默认，零配置） | Firebase 云端模式（推荐正式用） |
|------|------------------------|----------------------------|
| 注册 / 登录 / 登出 | ✅ | ✅ |
| 地址簿增删改、设默认 | ✅（仅本浏览器） | ✅（云端同步） |
| 订单历史 | ✅（仅本浏览器） | ✅（云端同步） |
| 修改姓名 / 密码 | ✅（仅本浏览器） | ✅ |
| 结账自动填充地址 | ✅ | ✅ |
| 换设备 / 换浏览器保留 | ❌ | ✅ |
| 费用 | 免费 | 免费（Spark 免费额度足够起步） |

## 一、创建 Firebase 项目（约 5 分钟）

1. 打开 https://console.firebase.google.com ，用 Google 账号登录
2. 点击 **Add project（添加项目）**，名称填 `haven-furniture`，一路 Continue，关闭 Google Analytics（不需要），创建项目
3. 项目创建好后，点击左侧齿轮 **Project settings（项目设置）**

## 二、注册 Web 应用，拿到配置（约 2 分钟）

1. 在 Project settings 页面，滚动到 **Your apps（您的应用）**，点击 `</>` 图标（Web）
2. App nickname 填 `Haven Store`，**不要**勾选 Firebase Hosting，点击 **Register app**
3. 页面会显示一段 `firebaseConfig`，长这样：

```js
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "haven-furniture.firebaseapp.com",
  projectId: "haven-furniture",
  storageBucket: "haven-furniture.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef123456"
};
```

4. 复制这 6 个值

## 三、开启邮箱登录（约 1 分钟）

1. 左侧菜单 **Build → Authentication**，点击 **Get started**
2. 在 **Sign-in method** 标签页，点击 **Email/Password**
3. 打开第一个开关 **Email/Password**（Enable），点击 Save
   - 第二个「Email link（无密码登录）」不需要开

## 四、创建数据库（约 2 分钟）

1. 左侧菜单 **Build → Firestore Database**，点击 **Create database**
2. 选择 **Start in production mode（生产模式）**，点 Next
3. Cloud Firestore location 选 **nam5 (United States)**（美国客户最快），点 Enable
4. 等待数据库创建完成后，切到 **Rules（规则）** 标签页，把规则替换为下面内容（用户只能读写自己的数据），然后点 **Publish**：

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{uid}/{subcollection}/{docId} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
  }
}
```

## 五、把配置填进网站（约 2 分钟）

打开 `index.html`，找到顶部的 `CONFIG.firebase`（约第 22 行），把第二步复制的 6 个值填进去：

```js
firebase: {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "haven-furniture.firebaseapp.com",
  projectId: "haven-furniture",
  storageBucket: "haven-furniture.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef123456"
}
```

保存后重新部署（推到 GitHub 即自动更新）。打开网站注册一个账号，账户中心底部的提示会从「Demo mode」变成「Secured by Firebase」，即配置成功。

## 数据结构（了解即可）

Firestore 中每个用户的数据结构：

```
users/{用户uid}/
├── addresses/{地址id}   → 收货地址簿
└── orders/{订单id}      → 订单历史
```

安全规则保证用户只能访问自己的数据，互相隔离。

## 免费额度（Spark 计划）

- Authentication：邮箱密码登录免费额度充足（数万次/天）
- Firestore：每日 5 万次读 + 2 万次写，起步阶段完全用不完
- 超出后才需要升级 Blaze 付费计划，前期不用开

## 常见问题

**Q：apiKey 暴露在前端代码里安全吗？**
A：Firebase 的 apiKey 只是项目标识符，不是密码。真正的安全靠第四步的 Firestore Rules 和 Auth 保证——没有登录的人读不到任何用户数据。这是 Firebase 官方推荐的标准用法。

**Q：以后想加 Google 一键登录 / 手机验证码登录？**
A：在 Authentication 的 Sign-in method 里开启对应提供商即可，前端我可以再帮你加按钮。

**Q：本地演示模式注册的账号，配置 Firebase 后还在吗？**
A：不在。两套数据相互独立，正式上线前用演示模式测试即可，上线后用户走 Firebase 云端账号。
