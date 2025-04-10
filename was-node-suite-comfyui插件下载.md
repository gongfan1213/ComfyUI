要安装 **WAS Node Suite for ComfyUI**，你可以按照以下步骤进行操作：

---

### **1. 通过 ComfyUI Manager 安装（推荐）**
1. **打开 ComfyUI**，确保已安装 **ComfyUI Manager**（大多数整合包已预装）。
2. 点击 **`Manager`** 按钮，进入插件管理界面。
3. 选择 **`Install Custom Nodes`**（安装自定义节点）。
4. 在搜索框中输入 **`was-node-suite-comfyui`** 或 **`WAS`**。
5. 找到插件后，点击 **`Install`**，等待安装完成。
6. 安装完成后，**重启 ComfyUI** 使插件生效。

---

### **2. 手动 Git 克隆安装**
如果 ComfyUI Manager 不可用，可以手动安装：
1. 进入 ComfyUI 的 **`custom_nodes`** 目录：
   ```bash
   cd ComfyUI/custom_nodes
   ```
2. 运行 Git 克隆命令：
   ```bash
   git clone https://github.com/WASasquatch/was-node-suite-comfyui.git
   ```
3. 安装依赖项（如有）：
   ```bash
   cd was-node-suite-comfyui
   pip install -r requirements.txt
   ```
4. **重启 ComfyUI** 以加载插件。

---

### **3. 通过整合包安装**
如果你使用的是 **秋葉 ComfyUI 整合包** 或其他预装版：
1. 打开整合包提供的 **插件管理器**（如秋叶启动器）。
2. 在 **“版本管理”** 或 **“扩展管理”** 中搜索 **`was-node-suite-comfyui`**。
3. 点击安装并重启 ComfyUI。

---

### **4. 检查安装是否成功**
- 重启 ComfyUI 后，右键菜单应出现 **`WAS Suite`** 或 **`WAS Nodes`** 相关选项。
- 如果没有显示，检查 **`ComfyUI/custom_nodes/was-node-suite-comfyui`** 目录是否存在。

---

### **5. 常见问题**
- **缺少依赖项**：如果运行时报错（如 `cv2` 或 `numpy` 缺失），手动安装：
  ```bash
  pip install opencv-python numpy
  ```
- **节点未加载**：检查 ComfyUI 启动日志，确认插件是否被正确加载。

---

### **总结**
- **推荐方法**：使用 **ComfyUI Manager** 安装（最简单）。
- **备选方法**：手动 Git 克隆或整合包安装。
- **验证**：确保插件出现在右键菜单中。

如需更多帮助，可参考 [WAS Node Suite GitHub](https://github.com/WASasquatch/was-node-suite-comfyui) 或 ComfyUI 社区讨论。
