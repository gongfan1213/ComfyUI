# ComfyUI

https://civitai.com/models/26873/80s-movie-style-lora?modelVersionId=32164

https://civitai.com/models/48671/dark-sushi-25d-25d

https://huggingface.co/datasets/svjack/pokemon-blip-captions-en-zh



该文档聚焦于COMP7065创新实验室中使用LoRA对Stable Diffusion进行微调的实践教学，主要内容涵盖理论知识、操作方法与实践练习三大板块，旨在提升学生在AI艺术设计中运用LoRA技术的能力。
1. **LoRA基础与应用现有LoRA**：LoRA即低秩适配器，是一种高效微调神经网络模型的方法，能对基础稳定扩散模型进行定制。以 “80s Movie Style LoRA” 为例，学生需从CivitAI下载其权重文件并放入ComfyUI指定文件夹。在ComfyUI中，借助 “Load LoRA” 节点，通过设置 “lora_name”“strength_model”“strength_clip” 等参数应用LoRA。同时要注意LoRA与基础模型的匹配，以及输入相应触发词。通过对比应用前后图像，学生可直观感受LoRA对图像风格的影响。
2. **创建自定义LoRA**
    - **准备工作**：以微调pruned Dreamshaper 8模型为例，使用Huggingface上的 “pokemon - blip - captions - en - zh” 数据集，该数据集会自动下载。
    - **安装SimpleLoRA节点**：从Github下载SimpleLoRA源代码至ComfyUI的自定义节点文件夹，运行 “install_requirements.bat” 配置依赖。重启ComfyUI后，若在 “Add Node” 中能找到 “SimpleLoRA”，则安装成功。
    - **设置参数进行微调**：在ComfyUI中构建仅含 “LoRATrainer” 节点的工作流程，设置 “ckpt_name” 指定基础模型；根据数据集来源设置 “use_custom_dataset” 和 “dataset_path”；“lora_checkpoint_folder” 和 “lora_name” 确定LoRA权重保存路径和文件名；依据数据集的标注情况设置 “caption_column”；合理设置 “learning_rate”“batch_size”“training_steps”“checkpointing_steps”“rank”“random_seed” 等训练参数，点击 “Queue” 按钮开始训练。训练成功后，在相应文件夹可查看保存的LoRA权重，通过构建新工作流程，使用 “LoraLoadrModelOnly” 节点应用训练好的LoRA，对比应用前后图像效果，验证LoRA的有效性。
3. **练习与提交**：学生尝试用自定义数据集对稳定扩散模型进行LoRA微调。先准备根文件夹，并在其中创建 “image” 和 “text” 文件夹；从指定位置下载图像到 “image” 文件夹并按规则重命名；在 “text” 文件夹创建同名文本文件进行手动注释，也可选择自动注释；运行 “LoRATrainer” 节点，设置 “use_custom_dataset” 为 “true”，指定数据集路径，调整合适的训练参数进行训练；训练完成后应用LoRA，对比生成图像判断其效果。最后，学生需将ComfyUI工作流程、微调后的LoRA权重、10张输出图像及对应的提示词文本文件打包提交。 
