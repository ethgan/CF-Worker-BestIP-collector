# Cloudflare 优选IP 收集器
由于GitHub版的被官方以滥用资源为理由封禁了项目，特推出基于Cloudflare worker版的优选IP，更快，更高效，更直观！抛弃github Action~

<p align="center">
  <a href="https://www.youtube.com/watch?v=onrDa-iNJeY&pp=0gcJCRUKAYcqIYz" target="_blank">
    <img src="https://img.icons8.com/color/48/000000/youtube-play.png" alt="YouTube" width="40" height="40"/>
  </a>
  &nbsp;&nbsp;
  <a href="https://github.com/ethgan/CF-Worker-BestIP-collector" target="_blank">
    <img src="https://img.icons8.com/ios-glyphs/48/000000/github.png" alt="GitHub" width="40" height="40"/>
  </a>
  &nbsp;&nbsp;
  <a href="https://t.me/yt_hytj" target="_blank">
    <img src="https://img.icons8.com/color/48/000000/telegram-app--v1.png" alt="Telegram" width="40" height="40"/>
  </a>
</p>

一个基于 Cloudflare Workers 的优选 CF IP 地址收集与测速工具，自动从多个公开来源收集 Cloudflare IP 地址，并提供可视化界面和测速功能。

## 🌟 功能特点

- **自动收集**：定时从多个公开来源自动收集 Cloudflare IP 地址
- **智能测速**：内置一键测速功能，支持批量测试 IP 延迟
- **多种格式**：支持 TXT 格式下载和原始数据获取
- **ITDog 集成**：支持导出 IP 列表到 ITDog 进行批量 TCPing 测试
- **现代化界面**：简洁美观的 Web 界面，支持响应式设计
- **实时排序**：测速完成后自动按延迟排序，快速找到最优 IP

V2.5版本，添加管理员登录参数，需要到CF worker环境变量里添加 ADMIN_PASSWORD，网页增加Token管理，登陆后可用
<img width="1339" height="575" alt="图片" src="https://github.com/user-attachments/assets/9edcd160-85ca-4d85-9344-6d3699161300" />
<img width="1598" height="517" alt="图片" src="https://github.com/user-attachments/assets/e32a4353-6954-40c7-b0d2-01860eace439" />
<img width="1370" height="674" alt="图片" src="https://github.com/user-attachments/assets/4a727f2d-8eb6-4edb-b9a8-d5fe68fbb2b1" />


## 🚀 快速开始

### 前置要求

- Cloudflare 账户
- Workers 权限
- KV 命名空间（用于存储 IP 数据）

### 部署步骤

1. **克隆项目**
   ```bash
   git clone https://github.com/yumingyuan2/CF-Worker-BestIP-collector.git
   cd CF-Worker-BestIP-collector
   ```

2. **创建 KV 命名空间**
   - 在 Cloudflare Dashboard 中进入 Workers & Pages
   - 创建新的 KV 命名空间，名称建议为 `IP_STORAGE`
   - 记录下命名空间的 ID

3. **配置 Wrangler**
   - 复制 `wrangler.toml.example` 为 `wrangler.toml`
   - 更新 `wrangler.toml` 中的 KV 命名空间 ID：

   ```toml
   [[kv_namespaces]]
   binding = "IP_STORAGE"
   id = "your_kv_namespace_id_here"
   ```

4. **部署到 Cloudflare**
   ```bash
   npm install
   npx wrangler deploy
   ```

5. **配置定时任务**
   - 在 Cloudflare Dashboard 中为 Worker 添加定时触发器
   - 建议设置为每 12 小时运行一次
   - 修改定时更新操作如下
   - 登录Cloudflare Dashboard，进入Workers & Pages。
   - 选择您部署的Worker。
   - 点击“设置”选项卡，然后选择“触发器”。
   - 在“Cron 触发器”部分，点击“添加 Cron 触发器”。（推荐这种 简单）
    <img width="1920" height="913" alt="图片" src="https://github.com/user-attachments/assets/032434ca-8586-44ae-bc97-999a13d50f8f" />

  
   - （另一种方式）输入Cron表达式：0 */12 * * * （每12小时执行一次）
   - 选择时区（例如：UTC）。
   - 点击“保存”
   - Cron 表达式
   - 推荐设置：0 */12 * * * （每 12 小时执行一次）
   - 其他常用选项：
   - 0 * * * * - 每小时执行一次
   - 0 0 * * * - 每天午夜执行
   - 0 */6 * * * - 每 6 小时执行一次

## 📖 使用方法

### Web 界面

访问部署后的 Worker 地址即可使用完整功能：

- **查看 IP 列表**：浏览所有收集到的 Cloudflare IP 地址
- **一键测速**：批量测试所有 IP 的延迟，自动排序
- **导出数据**：下载 TXT 格式的 IP 列表
- **ITDog 集成**：复制 IP 列表到 ITDog 进行更详细的测试

### API 接口

- `GET /` - 主页面
- `GET /ips` 或 `GET /ip.txt` - 获取纯文本 IP 列表
- `GET /raw` - 获取原始 JSON 数据
- `POST /update` - 手动触发 IP 更新
- `GET /speedtest?ip=<ip>` - 测试指定 IP 的速度
- `GET /itdog-data` - 获取 ITDog 格式数据

## ⚙️ 配置说明

### 数据来源

项目从多个公开的 Cloudflare IP 数据源自动收集，包括：

- ip.164746.xyz
- ip.haogege.xyz
- stock.hostmonit.com/CloudFlareYes
- api.uouin.com/cloudflare.html
- addressesapi.090227.xyz
- www.wetest.vip

### 环境变量

无需额外环境变量，所有配置通过代码管理。

## 🛠️ 开发

### 本地开发

```bash
# 安装依赖
npm install

# 启动本地开发服务器
npx wrangler dev

# 部署到生产环境
npx wrangler deploy
```

### 项目结构

```
├── cfip.js              # 主 Worker 代码
├── wrangler.toml        # Wrangler 配置
├── package.json         # 项目依赖
└── README.md           # 项目说明
```

## 📊 技术栈

- **运行时**：Cloudflare Workers
- **存储**：Cloudflare KV
- **前端**：原生 HTML/CSS/JavaScript
- **部署**：Wrangler

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

1. Fork 本项目
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 创建 Pull Request

## 📄 开源协议

本项目基于 MIT 协议开源，详见 [LICENSE](LICENSE) 文件。

## ⚠️ 免责声明

本项目仅用于学习和研究目的，请勿用于商业用途或违反相关服务条款。使用者需自行承担相关风险。


如果这个项目对你有帮助，请给个 ⭐️ 支持一下！
## Star History


