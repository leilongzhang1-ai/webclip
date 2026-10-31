# 跨设备剪贴板

不登录、不装 App，手机和电脑之间互传文字。打开同一个网址就能用，内容通过公共 MQTT 服务器中转，不做长期存储。

## 怎么用

1. 电脑上打开：<https://leilongzhang1-ai.github.io/webclip/>
2. 页面右上角点二维码图标，用手机扫一下（或把网址发到手机）。
3. 任意一边粘贴文字 → 发送，另一边几乎立刻出现。
4. 点卡片即可复制；手机建议「添加到主屏幕」，下次一点就开。

## 说明

- 网址里的 `#r=xxxxxx` 是房间号，两台设备要一致。直接打开首页会自动生成一个并记住。
- 最后一条内容会留在服务器上（保留消息），所以对方晚点打开也能看到；「清空记录」可删掉。
- 本机历史存在浏览器的 localStorage 里，只在自己设备上。
- 不加密，房间号被猜到就能看到内容，**不要传敏感信息**。
- 依赖公共服务器 `broker.emqx.io`，它不可用时页面会显示「连接断开，正在重试」。

## 开发

```bash
node build.mjs          # 生成 dist/index.html（单文件，零外部依赖）
node test/mqtt-test.mjs # 公共 MQTT 服务器连通性测试
node test/qr-test.mjs   # 二维码生成 + 解码验证
node test/server.mjs    # 本地预览 http://127.0.0.1:8787
node test/e2e.mjs       # 真实浏览器端到端测试（需先启动 server）
```

源码在 `src/`：`mqtt-lite.js`（手写的 MQTT 3.1.1 客户端，约 300 行）、`app.js`（页面逻辑）、
`index.template.html`（结构与样式）。二维码用的 qrcode-generator（MIT）在构建时内联进单文件。
