要下载 `comfyui_controlnet_aux` 插件，可以通过以下几种方式：

### 1. **通过 ComfyUI Manager 安装（推荐）**
- 在 ComfyUI 界面中，打开 **Manager** 插件（需提前安装 ComfyUI-Manager）。
- 点击 **"Install Custom Nodes"**，搜索 `comfyui_controlnet_aux` 并安装。
- 安装完成后，重启 ComfyUI 并刷新浏览器页面。

### 2. **手动 Git 克隆**
- 在 ComfyUI 的 `custom_nodes` 目录下运行以下命令：
  ```bash
  git clone https://github.com/Fannovel16/comfyui_controlnet_aux.git
  ```
- 进入插件目录并安装依赖：
  ```bash
  cd comfyui_controlnet_aux
  pip install -r requirements.txt
  ```
- 重启 ComfyUI 生效。

### 3. **国内镜像安装（适用于网络受限情况）**
- 使用 Gitee 镜像（国内加速）：
  ```bash
  git clone https://gitee.com/ComfyUI_1/comfyui_controlnet_aux.git
  ```
- 同样需要进入目录安装依赖。

### 4. **下载预处理器模型（可选）**
该插件依赖部分预处理器模型（如 HED、OpenPose 等），可从 Hugging Face 下载并放置到：
```
ComfyUI/custom_nodes/comfyui_controlnet_aux/ckpts/lllyasviel/Annotators
```
部分模型下载地址：
- [Hugging Face - lllyasviel/Annotators](https://huggingface.co/lllyasviel/Annotators/tree/main) 。

### 5. **使用整合包（适合新手）**
如果不想手动安装，可以下载已集成该插件的 ComfyUI 整合包（如“疯狂AI一键安装包”），解压后直接使用。

安装完成后，在 ComfyUI 中右键菜单即可找到 `ControlNet Aux` 相关的预处理节点（如 Canny、HED、OpenPose 等）。
