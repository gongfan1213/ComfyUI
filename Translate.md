# COMP7065 Innovative Laboratory：Fine-tuning Stable Diffusion with LoRA
完成本实验后，学生将能够：
1. 应用现有的低秩自适应（LoRA）来辅助自己的人工智能艺术设计。
2. 描述LoRA微调自定义节点的工作原理。
3. 在ComfyUI中对稳定扩散模型进行LoRA微调。
4. 构建自己的数据集用于LoRA微调。

## 目录
1. 应用现有LoRA：“Load LoRA”节点 - 2
2. 创建自己的LoRA：“SimpleLoRA”节点 - 5
3. 练习 - 15

## 1. 应用现有LoRA：“Load LoRA”节点
LoRA代表低秩适配器，是一种针对神经网络模型的参数高效微调方法。简单来说，LoRA是模型权重的小型“附加组件”，能让你高效定制基础稳定扩散模型。例如，你可以用LoRA改变图像风格、指定喜欢的角色等。在这部分，你将学习如何将现有LoRA应用到稳定扩散模型中，并直观了解其效果。本展示中，我们将使用“80s Movie Style LoRA”。请从CivitAI的发布网站下载该LoRA，并将其权重文件“80s_offset.safetensors”放入“ComfyUI/models/loras”文件夹。

在ComfyUI中，“Load LoRA”节点为我们提供了使用现有LoRA的便捷方式。以下展示了加载“80s Movie Style LoRA”并将其插入精简版Dreamshaper 8模型的工作流程。如输出图像中的男子看起来就像老电影中的角色。

现在来研究“Load LoRA”节点的设置。“lora_name”指定要加载的LoRA；“strength_model”和“strength_clip”分别调整应用于UNet和CLIP的LoRA权重，这两个值的范围通常在0到1之间。不过在工作流程中，也可以将这两个强度值设为负值，以消除LoRA的效果。

补充说明：要确保LoRA与基础模型匹配，例如，适用于稳定扩散1.5模型的LoRA不能应用于稳定扩散XL模型。此外，LoRA通常会用一些触发词进行训练。为使LoRA生效，可能需要在正向文本提示中输入这些触发词。一般来说，可以从LoRA的发布网站找到触发词信息和适用的基础模型。如下所示，“80s Movie Style LoRA”适用于“stable diffusion 1.5”（SD1.5）模型，触发词是“80s”。有时也能找到LoRA作者推荐的“strength_model”和“strength_clip”值，但建议自行尝试，找到最合适的设置。

值得注意的是，展示中的基础模型不必是原始的稳定扩散1.5，只要与稳定扩散1.5保持相同网络架构，像Dreamshaper或DarkSushi等其他基础模型也适用。以下是DarkSushi 2.5D与“80’s Movie Style LoRA”一起生成的示例。

为确定LoRA确实对输出有影响，可以移除“Load LoRA”节点，在保持其他设置不变的情况下再次运行生成过程。以下是未使用LoRA生成的示例，可见没有LoRA时，图像风格与展示中的有很大差异，由此可得出“80’s Movie Style LoRA”效果显著的结论。

## 2. 创建自己的LoRA：“SimpleLoRA”节点
尽管人工智能艺术社区（如CivitAI）中有数千个LoRA，但有时仍可能找不到适合自己艺术风格的。这种情况下，可能需要自行对基础模型进行LoRA微调。我们将探索如何在ComfyUI中完全无需编程进行LoRA微调，这可通过“SimpleLoRA”节点实现。
### 准备工作
本展示中，你将学习如何使用Huggingface数据集对精简版Dreamshaper 8模型进行微调。我们将使用“pokemon-blip-captions-en-zh”数据集作为训练集，并通过“SimpleLoRA”节点进行LoRA微调。

数据集：可通过此链接在HuggingFace上预览“pokemon-blip-captions-en-zh”数据集：https://huggingface.co/datasets/svjack/pokemon-blip-captions-en-zh 。在展示中，Pokemon数据集将自动下载，无需手动从HuggingFace下载。
### SimpleLoRA
为在ComfyUI中启用LoRA微调，需使用自定义节点“SimpleLoRA”。可通过Github上的此链接访问该节点：https://github.com/sifengshang/ComfyUI_SimpleLoRA 。在主网站上，已说明如何将该节点安装到ComfyUI中。首先要从Github网站下载SimpleLoRA的源代码，并将其放入自定义节点文件夹“ComfyUI\custom_nodes”。

接下来，请关闭ComfyUI窗口和终端。进入“ComfyUI_SimpleLoRA”文件夹，双击批处理文件“install_requirements.bat”，它将自动下载并配置该节点的Python依赖项。

要检查安装是否成功，可以重启ComfyUI，右键选择“Add Node”。如果能看到“SimpleLoRA”选项，则说明自定义节点已成功安装。
### 使用SimpleLoRA微调Dreamshaper 8
要使用该节点进行LoRA微调，应先在ComfyUI中加载一个完全空白的工作流程，然后找到“SimpleLoRA”节点并添加到工作流程中。可按“Add Node”->“SimpleLoRA”->“LoRATrainer”的顺序找到该节点。也就是说，使用SimpleLoRA进行LoRA微调的工作流程应只包含一个（LoRATrainer）节点，且无需连接，其设置如下：
```
#10 SimpleLoRA
LoRATrainer
ckpt_name DreamShaper_8_pruned.safetensors
use_custom_dataset false
dataset_path
lora_checkpointfolder loratest
lora_name my_lora
caption_column text
learning_rate 0.00010
batch_size 1
training_steps 500
checkpointing_steps 100
rank 4
random seed 0
```
该节点让你无需编码即可对基础稳定扩散模型进行LoRA微调！你只需正确配置节点设置，然后点击“Queue”按钮运行工作流程。

以下展示如何通过SimpleLoRA节点在“pokemon-blip-captions-en-zh”数据集上对精简版Dreamshaper 8进行LoRA微调。应用训练好的LoRA后，基础模型生成的宝可梦图像将更具卡通风格。通过此展示，你可以了解SimpleLoRA节点中每个设置的含义，以及如何根据需求合理配置它们。
#### “ckpt_name”
此设置表示要微调的基础模型的“检查点名称”，可让你从“ComfyUI/models/checkpoints”文件夹中选择模型。本展示中，基础模型是精简版Dreamshaper 8，所以将“ckpt_name”设为“Dreamshaper_8_pruned.safetensors”。
#### “use_custom_dataset”和“dataset_path”
这两个设置共同指定用于LoRA微调的数据集。具体来说，当“use_custom_dataset”设为True时，节点将加载你自己构建的自定义数据集，数据集的文件夹路径由“dataset_path”设置指定，你将在练习中了解更多构建自定义数据集的细节。另一方面，当“use_custom_dataset”设为False时，节点将自动下载HuggingFace数据集并用于微调，数据集名称由“dataset_path”指定。

本展示中，我们将使用HuggingFace数据集“pokemon-blip-captions-en-zh”作为训练集，所以应将“use_custom_dataset”设为False。对于“dataset_path”，可参考“pokemon-blip-captions-en-zh”数据集在HuggingFace上的发布网站：https://huggingface.co/datasets/svjack/pokemon-blip-captions-en-zh 。在该网站上可看到数据集名称是“svjack/pokemon-blip-captions-en-zh”，这就是我们应填入“dataset_path”的值，也可以点击复制图标（蓝色圆圈圈出部分）将数据集名称直接复制到剪贴板，然后粘贴到“dataset_path”。

补充说明：一般来说，HuggingFace上的任何文本到图像数据集都受支持。
#### “lora_checkpoint_folder”和“lora_name”
在LoRA微调过程中，节点会定期将训练好的LoRA权重保存到“ComfyUI/models/lora_logs/[lora_checkpoint_folder]”文件夹。例如，如果将“checkpoint_dirname”设为“lora_test”，那么训练好的LoRA权重将保存到“ComfyUI/models/lora_logs/lora_test”文件夹。注意，该文件夹会在训练过程中由节点自动创建，无需手动创建。LoRA权重的文件名格式为“[lora_name].safetensors”。例如，当“lora_name”设为“my_lora”时，文件名就是“my_lora.safetensors”。

本展示中，我们将“lora_checkpoint_folder”设为“lora_showcase”，“lora_name”设为“pokemon_lora”。结果是，将在“ComfyUI/models/lora_logs/lora_showcase”中创建一个文件夹，训练好的lora权重将命名为“pokemon_lora.safetensors”。
#### “caption_column”
在解释此设置的含义之前，我们应了解一些数据集的基础知识。从课程中我们了解到，要训练神经网络，必须准备许多作为数据集组织的训练数据。在本展示中，可以将整个数据集想象成一个大Excel表格，如下所示。在这个表格中，每行项目由一张图像和至少一个标题组成，标题就是正向文本提示，基础模型经过训练，在给出标题时能生成与图像内容完全一致的内容。有时训练数据可能有多个标题列，如下所示，每张图像都有英文标题“en_text”和中文标题“cn_text”。在这种情况下，我们需要通过在节点中指定“caption_column”来确定用于训练的标题列。

需要注意的是，设置“caption_column”有不同方法。使用自定义数据集时（即“custom_dataset”设为True时），“caption_column”的键始终是“text”。对于现有的HuggingFace数据集，可能需要进入其发布网站，查看“Dataset Viewer”中的列。在本展示中，“pokemon-blip-captions-en-zh”在发布网站上的“Dataset Viewer”显示有两个标题列，一个是“en_text”，另一个是“cn_text”。由于我们使用英文标题进行微调，所以将“caption_column”设为“en_text”。
#### “learning_rate”
此参数控制训练过程中的学习率。较大的学习率会加快训练速度，但可能导致损失下降不稳定，甚至发散。另一方面，如果学习率太小，即使经过很多训练步骤也可能导致欠拟合。在本展示中，建议将学习率设为默认值“0.0001”。
#### “batch_size”
当“batch_size”设为1时，每个训练步骤仅向模型输入一对（图像，标题）。这可能会减慢训练过程，但会显著节省GPU内存。如果你有足够的GPU资源，可以将“batch_size”设为大于1的值，只要不触发“Cuda: out of memory”错误即可。在本实验中，我们使用只有8GB GPU内存的单个NVIDIA RTX 3050Ti进行微调，所以建议将“batch_size”设为1。
#### “training_steps”和“checkpionting_steps”
“training_steps”表示训练过程的总迭代次数，LoRA权重将每隔“checkpionting_steps”保存一次。例如，如果将“training_steps”设为2000，“checkpionting_steps”设为500，这意味着整个训练步骤为2000次，LoRA权重每隔500步保存一次。

需要注意的是，训练“training_steps”更多的LoRA可能更有效，但训练时间也会成比例增加。如果训练步骤太少，LoRA权重可能欠拟合，导致质量不佳。然而，也不能将训练步骤设得太大，因为LoRA权重可能过拟合，仍然会导致输出效果不好。在本展示中，为了快速获得训练好的LoRA，我们将“training_steps”设为1000，“checkpoint_steps”设为200。但也鼓励你自行设置，看看LoRA的质量会受到怎样的影响。
#### “rank”
这是LoRA微调方法特有的超参数，表示每层中适配器的秩数。一般来说，较高的秩可能会提高LoRA的质量，但会消耗更多的GPU内存。在本展示中，建议将“rank”设为4。
#### “random_seed”
与Ksampler中的随机种子类似，它是一个整数，用于控制训练过程中的随机性。我们可以始终将其设为默认值0。

以下展示了在宝可梦展示案例中LoRATrainer节点的正确设置，可作为快速参考：
```
#10 SimpleLoRA
LoRATrainer
ckpt name DreamShaper_8_pruned.safetensors
use custom dataset false
dataset_path svjack/pokemon-blip-captions-e
lora checkpoint folder lora showcase
lora name pokemon lora
caption_column en text
learning_rate 0.00010
batch_size
training_steps 1000
checkpointing_steps 200
rank 4
random_ seed 0
```
正确设置LoRATrainer节点后，可按下“Queue”按钮启动训练工作流程。训练过程可能需要15分钟左右，你可以通过ComfyUI的终端窗口监控训练进度。

如果训练成功，你可能会在终端窗口中看到提示“Training is finished”。

为确保LoRA权重确实已成功训练，可以检查“ComfyUI/models/lora_logs/lora_showcase”文件夹。如下所示，该文件夹确实存在。从生成的文件可以推断，LoRA权重每隔200步保存一次，总共训练了1000步，与我们的设置一致。

然后我们还可以查看“ComfyUI/models/loras”文件夹，在其中可以看到训练好的权重“pokemon_lora.safetensors”，这进一步证明了LoRA训练是成功的。

为直接展示训练好的LoRA的效果，我们可以构建一个工作流程，并将训练好的LoRA权重应用到DreamShaper 8.0模型。该工作流程与第2部分的展示非常相似，但主要区别在于这次我们应使用“LoraLoadrModelOnly”节点替换“Load LoRA”节点，因为在SimpleLoRA中，LoRA适配器仅应用于UNet模型的变压器层。可参考以下图示。当正向文本提示为“a pokemon that looks like a mouse”时，应用了LoRA的模型输出的图像真的像一只老鼠形状的宝可梦。

当我们移除“LoraLoaderModelOnly”节点时，输出图像看起来像一只真正的老鼠，而不是宝可梦。这表明LoRA有效地将输出图像的风格从写实转变为类似宝可梦的风格。

## 3. 练习
在第2部分中，我们学习了如何使用从HuggingFace下载的文本到图像数据集对稳定扩散模型进行LoRA微调。在本练习中，让我们尝试更高级的内容：使用自定义数据集对稳定扩散模型进行LoRA微调。

这与第3部分的操作类似，但这次你必须自己构建数据集。构建自定义数据集主要有两个步骤：首先，需要收集想要的图像；然后，需要手动或通过图像字幕模型为每张图像添加注释。在本练习中，我们将展示如何构建一个单线艺术图像的小数据集并手动注释。
### 步骤1：为自定义数据集准备一个根文件夹，并创建“image”和“text”文件夹
首先创建数据集的根文件夹，根文件夹可以在实验室机器的任何位置。在本练习中，我们在“D:\one-line-art”创建根文件夹。然后在数据集根文件夹中创建另外两个新文件夹，分别命名为“image”和“text”。
### 步骤2：将喜欢的图像收集到“image”文件夹中
将图像下载到在步骤1中创建的“image”文件夹中。注意，将图像重命名为从0开始的数字（索引）非常关键，例如“0.png”、“1.png”等等。

在本练习中，我们可以从HuggingFace上的“sd-concepts-library/one-linedrawing”下载图像作为示例，重命名后放入“image”文件夹。最终“image”文件夹应如下所示：“0.jpeg” “1.jpeg” “2.jpeg” “3.jpeg”
### 步骤3：注释图像并将文本文件保存到“text”文件夹中
打开在步骤1中创建的“text”文件夹，创建空文本文件，并同样重命名为从0开始的数字，如“0.txt”、“1.txt”等等。“[x].txt”中的文本就是与图像“[x].image”对应的注释（文本提示），其中[x]是你重命名的数字/索引。然后可以在文本文件中注释在步骤2中下载的图像。

在我们的示例中，由于训练图像都是“单线艺术”绘图，所以可以在所有文本文件中都写入“one-line-art”，还可以添加每张图像的一些特征描述，如为“1.jpeg”添加“side view”，为“3.jpeg”添加“closed eyes”。文本文件及其内容如下所示：
| 名称 | 日期修改 | 类型 | 大小 |
| --- | --- | --- | --- |
| 0.txt | 16/2/2025 11:28 pm | 文本文档 | 1 KB |
| 1.txt | 16/2/2025 11:29 pm | 文本文档 | 1 KB |
| 2.txt | 16/2/2025 11:29 pm | 文本文档 | 1 KB |
| 3.txt | 16/2/2025 11:29 pm | 文本文档 | 1 KB |

“0.txt”：one-line-art, faces,side view
“1.txt”：one-line-art,a face,
