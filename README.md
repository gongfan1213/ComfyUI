# ComfyUI

https://civitai.com/models/26873/80s-movie-style-lora?modelVersionId=32164

https://civitai.com/models/48671/dark-sushi-25d-25d

https://huggingface.co/datasets/svjack/pokemon-blip-captions-en-zh



该文档聚焦于COMP7065创新实验室中使用LoRA对Stable Diffusion进行微调的实践教学，主要内容涵盖理论知识、操作方法与实践练习三大板块，旨在提升在AI艺术设计中运用LoRA技术的能力。
1. **LoRA基础与应用现有LoRA**：LoRA即低秩适配器，是一种高效微调神经网络模型的方法，能对基础稳定扩散模型进行定制。以 “80s Movie Style LoRA” 为例，学生需从CivitAI下载其权重文件并放入ComfyUI指定文件夹。在ComfyUI中，借助 “Load LoRA” 节点，通过设置 “lora_name”“strength_model”“strength_clip” 等参数应用LoRA。同时要注意LoRA与基础模型的匹配，以及输入相应触发词。通过对比应用前后图像，学生可直观感受LoRA对图像风格的影响。
2. **创建自定义LoRA**
    - **准备工作**：以微调pruned Dreamshaper 8模型为例，使用Huggingface上的 “pokemon - blip - captions - en - zh” 数据集，该数据集会自动下载。
    - **安装SimpleLoRA节点**：从Github下载SimpleLoRA源代码至ComfyUI的自定义节点文件夹，运行 “install_requirements.bat” 配置依赖。重启ComfyUI后，若在 “Add Node” 中能找到 “SimpleLoRA”，则安装成功。
    - **设置参数进行微调**：在ComfyUI中构建仅含 “LoRATrainer” 节点的工作流程，设置 “ckpt_name” 指定基础模型；根据数据集来源设置 “use_custom_dataset” 和 “dataset_path”；“lora_checkpoint_folder” 和 “lora_name” 确定LoRA权重保存路径和文件名；依据数据集的标注情况设置 “caption_column”；合理设置 “learning_rate”“batch_size”“training_steps”“checkpointing_steps”“rank”“random_seed” 等训练参数，点击 “Queue” 按钮开始训练。训练成功后，在相应文件夹可查看保存的LoRA权重，通过构建新工作流程，使用 “LoraLoadrModelOnly” 节点应用训练好的LoRA，对比应用前后图像效果，验证LoRA的有效性。
3. **练习与提交**：尝试用自定义数据集对稳定扩散模型进行LoRA微调。先准备根文件夹，并在其中创建 “image” 和 “text” 文件夹；从指定位置下载图像到 “image” 文件夹并按规则重命名；在 “text” 文件夹创建同名文本文件进行手动注释，也可选择自动注释；运行 “LoRATrainer” 节点，设置 “use_custom_dataset” 为 “true”，指定数据集路径，调整合适的训练参数进行训练；训练完成后应用LoRA，对比生成图像判断其效果。最后，学生需将ComfyUI工作流程、微调后的LoRA权重、10张输出图像及对应的提示词文本文件打包提交。 


将系统学习基于ControlNet的空间控制技术及其在AI艺术创作中的应用，提升实践动手能力和创新思维。
1. **构建工作流程**：学生能够学会使用ComfyUI搭建包含ControlNet节点的工作流程。在人体姿态控制部分，需添加负责图像预处理和配置ControlNet的两组节点；探索其他空间控制类型时，也需依据不同空间提示调整预处理节点和模型权重来构建工作流程。
2. **应用多种空间控制**：掌握多种空间控制方式。人体姿态控制中，利用OpenPose网络检测人体关键点，将生成的关键点图像作为空间提示应用于ControlNet；Canny边缘图控制里，学会使用Canny算子检测图像边缘，通过调整阈值控制边缘像素数量；M-LSD线图控制方面，了解基于深度学习的M-LSD线段检测方法，用于场景设计；HED边界图控制上，明白HED基于全卷积神经网络的边缘检测原理，其生成的“软”边缘图更具细节；涂鸦控制则只需提供涂鸦作为空间提示并调整相应模型权重。
3. **模型权重管理与节点操作**：学会管理ControlNet的模型权重，知晓从HuggingFace下载修剪版模型权重的方法及优势。熟练操作ComfyUI中的节点，如添加、配置“OpenPose Pose”“Canny Edge”“Load ControlNet Model”“Apply ControlNet”等各类节点，了解每个节点的功能和参数设置含义。
4. **整合技术创作AI艺术品**：将空间控制技术融入到AI艺术品创作中。在练习环节，学生需利用“Canvas Tab”节点绘制涂鸦并作为空间提示，结合ControlNet生成图像，从而实现将空间控制技术应用于实际创作。 
