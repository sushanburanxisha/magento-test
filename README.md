# Fake Magento Test Pages - SSRF Canonical Leak Research

## 用途
这些页面用于测试 magereport.com 的 SSRF 漏洞是否会回显 `<link rel="canonical">` 标签的内容。
每个页面伪装成 Magento 商店(含 Magento 指纹字符串),但 canonical 标签指向不同的内部/元数据地址。

## 上传方法
1. 在 GitHub 创建一个 **public 仓库**(或用现有的 Pages 仓库)
2. 把这些 HTML 文件上传到仓库根目录(或 docs/ 目录)
3. 开启 GitHub Pages: Settings → Pages → Source 选 main 分支
4. 等待 1-2 分钟,页面会在 `https://<username>.github.io/<repo>/` 上线

## 页面清单
- `index.html` - 基线:canonical = 明显标记(验证回显机制)
- `aws-metadata.html` - canonical = AWS 元数据 instance-id
- `aws-iam.html` - canonical = AWS IAM 凭据路径
- `loopback.html` - canonical = scanner 自身 loopback admin
- `internal-auth.html` - canonical = Hypernode 内部 auth 服务器
- `internal-api.html` - canonical = Hypernode 内部 api 服务器
- `file-passwd.html` - canonical = file:///etc/passwd
- `gcp-metadata.html` - canonical = GCP 元数据

## 测试
上传后告诉我你的 Pages URL(如 https://yourname.github.io/test-repo/),
我会用 magereport SSRF 扫描每个页面,验证 canonical_url 字段是否回显了内部地址。

## 安全说明
这些页面是只读的 HTML,不含恶意代码。它们只是 canonical 标签指向内部地址
(浏览器不会自动访问 canonical,它只是 SEO 元数据)。
