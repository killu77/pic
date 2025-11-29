---
title: Yanlx
emoji: 🚀
colorFrom: blue
colorTo: purple
sdk: docker
pinned: false
app_port: 7860
---

# Vertex AI 免费代理 (小白保姆级教程)

这是一个能让你**免费**使用 Google 最强 AI 模型（Gemini 1.5 Pro, Gemini 2.0 Flash 等）的工具。

**核心原理**：
它就像一个“传话筒”。你在聊天软件里发消息，它通过你的浏览器（利用你已经登录的 Google 账号）把消息转发给 Google，然后再把回复传回来。

**优点**：
*   ✅ **完全免费**：直接薅 Google 的羊毛。
*   ✅ **模型强大**：支持最新的 Gemini 2.0 思考模型。
*   ✅ **安全**：代码开源，凭证只在你自己的浏览器和部署的服务之间传输。

---

## 🛠️ 部署教程 (二选一)

我们提供两种方式，**强烈推荐方式一（云端部署）**，因为这样你可以在手机、公司电脑等任何地方使用，而不用一直开着家里的电脑。

### 方式一：云端部署 (推荐 🌟)
*将服务部署在 Hugging Face 上，配合本地浏览器使用。*

#### 第一步：准备 GitHub 仓库
1.  登录 [GitHub](https://github.com) (如果没有账号请注册)。
2.  创建一个新的仓库 (**New Repository**)。
    *   **Repository name**: 随便填，例如 `vertex-proxy`。
    *   **Privacy**: 建议选 **Private** (私有)，保护你的隐私。
    *   点击 **Create repository**。
3.  将本项目的所有文件上传到这个新仓库中。
    *   (小白方法: 在仓库页面点击 "Upload files"，把下载的代码文件全拖进去，点击 "Commit changes")。

#### 第二步：创建 Hugging Face Space
1.  登录 [Hugging Face](https://huggingface.co) (如果没有账号请注册)。
2.  点击右上角头像 -> **New Space**。
3.  **Space Name**: 起个名字，例如 `my-vertex-ai`。
4.  **License**: 选 `MIT`。
5.  **SDK**: 必须选 **Docker**。
6.  点击 **Create Space**。

#### 第三步：连接 GitHub 和 Hugging Face (自动同步)
为了让代码能自动跑起来，我们需要把它们连通。

1.  **获取 Hugging Face Token**:
    *   点击 Hugging Face 头像 -> **Settings** -> **Access Tokens**。
    *   点击 **Create new token**。
    *   **Type (类型)**: 必须选 **Write** (很重要！)。
    *   **Name**: 随便填，比如 `github-sync`。
    *   点击 **Create token**，复制生成的以 `hf_` 开头的字符串。

2.  **设置 GitHub Secrets**:
    *   回到你的 GitHub 仓库页面。
    *   点击上方 **Settings** -> 左侧边栏 **Secrets and variables** -> **Actions**。
    *   点击 **New repository secret**。
    *   **Name**: 填 `HF_TOKEN`。
    *   **Secret**: 粘贴刚才复制的 Hugging Face Token。
    *   点击 **Add secret**。

3.  **修改同步地址**:
    *   在 GitHub 仓库的文件列表中，找到 `.github/workflows/sync_to_hub.yml`。
    *   点击文件名，然后点击右侧的 ✏️ (编辑图标)。
    *   找到最后一行：
        ```yaml
        run: git push --force https://kilo:$HF_TOKEN@huggingface.co/spaces/YOUR_USERNAME/YOUR_SPACE_NAME main
        ``````bash
    pip install -r requirements.txt
    ```
    *   将 `YOUR_USERNAME/YOUR_SPACE_NAME` 修改为你实际的 Hugging Face Space 地址。
        *   *例如：如果你的 HF 用户名是 `tom`, Space 名字是 `my-vertex-ai`，那就改成 `tom/my-vertex-ai`。*
    *   点击右上角 **Commit changes** 保存。

*此时，GitHub 会自动把代码推送到 Hugging Face。等待几分钟，你的 Hugging Face.  下载本项目代码到本地文件夹。
3.  在文件夹内打开终端 (CMD/PowerShell)，运行安装依赖：
     Space 页面就会显示 "Running" (绿色)。*

---

### 方式二：本地电脑运行 (极客选项 💻)
*适合懂一点 Python，且只想在当前电脑上使用的用户。*

1.  确保电脑安装了 Python 3.9+。
2
4.  启动服务：
    ```bash
    python main.py
    ```
    *看到 "🚀 Headful Proxy Started" 即表示启动成功。*

---

## 🔌 浏览器脚本安装 (关键步骤！)
**无论你选了哪种部署方式，这一步都是必须的！** 它是连接 Google 的桥梁。

1.  **安装油猴插件**:
    *   使用 Chrome 或 Edge 浏览器，安装 **Tampermonkey** 扩展程序。
2.  **安装脚本**:
    *   点击浏览器右上角的油猴图标 -> **添加新脚本**。
    *   删除编辑器里原本的内容。
    *   打开本项目中的 `vertex-ai-harvester.user.js` 文件，全选复制内容，粘贴到油猴编辑器里。
    *   按 `Ctrl+S` 保存。
3.  **连接服务**:
    *   打开 [Google Vertex AI Studio](https://console.cloud.google.com/vertex-ai/studio/multimodal) 网页并登录 Google 账号。
    *   你会看到左下角出现一个悬浮窗 **"Vertex AI Harvester"**。
    *   点击悬浮窗上的 ⚙️ (设置图标)。
    *   **输入 WebSocket 地址**:
        *   **如果你是云端部署**: 填 `wss://你的Space名字.hf.space/ws` (注意是 wss，且后面有 /ws)。
        *   **如果你是本地运行**: 填 `ws://127.0.0.1:7860/ws`。
    *   点击确定。刷新网页。
    *   如果悬浮窗显示 **"✅ Connected"**，恭喜你，大功告成！🎉

---

## 📱 如何使用 (连接客户端)

现在你可以使用任何支持 OpenAI 格式的 AI 软件（如 NextChat, Chatbox, LobeChat 等）来连接了。

### 配置参数
*   **接口地址 (Base URL)**:
    *   云端: `https://你的Space名字.hf.space/v1`
    *   本地: `http://127.0.0.1:7860/v1`
*   **API Key**: 随便填 (例如 `sk-123456`)。
*   **模型名称 (Model)**:
    *   `gemini-1.5-pro`
    *   `gemini-2.0-flash-exp` (最新推荐)
    *   `gemini-2.0-flash-thinking-exp` (思考模型)

### 常见问题
*   **Q: 为什么有时候没反应？**
    *   A: 请检查你的浏览器是否还开着 Vertex AI 的网页，并且悬浮窗显示 "Connected"。如果断开了，刷新一下网页即可。
*   **Q: 提示 "Recaptcha token invalid"?**
    *   A: 别担心，系统会自动刷新网页尝试修复。你只需要保持网页打开即可。

---

## ⚠️ 免责声明
本项目仅供学习和研究使用。请遵守 Google Cloud Platform 的服务条款。不要将此工具用于非法用途。