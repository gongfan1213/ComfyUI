We’ve learnt in Part 2 how to LoRA fine-tune a stable diffusion model with a text-to-image dataset downloaded from HuggingFace. In this exercise, let’s try something more advanced: LoRA fine-tune a stable diffusion model with a custom dataset.

This is similar to what you’ve done in Part 3, but this time you have to construct the dataset by yourself. There are two major steps to construct a custom dataset. First of all, you need to collect some images that you want, and then you need to annotate each of them with a caption, either manually or by an image caption model. In this exercise we show you construct a small dataset of one-line-art images and annotate them manually.

### Step 1: Prepare a root folder for your custom dataset and create ‘image’ and ‘text’ folders
Let’s start by create a root folder of the dataset. The root folder can be everywhere inside your lab machine. In this exercise, we create the root folder at : ‘D:\one-line-art’. Then we create another two new folders in the dataset root folder, and name them as ‘image’ and ‘text’ respectively.

![image](https://github.com/user-attachments/assets/473a153d-38b7-4567-92ec-d13b8bee37b9)


### Step 2: Collect images you like into the ‘image’ folder
Download the images inside the ‘image’ folder you’ve created in step 1. Note that it is very critical that you rename the images as a number(index) starting from 0, for example, ‘0.png’, ‘1.png’, so on and so forth.

In this exercise, we can download the images from ‘sd-concepts-library/one-linedrawing’ on HuggingFace as examples, rename them and place them inside the ‘image’ folder. And finally the image folder should look like this:
0.jpeg 1.jpeg 2.jpeg 3.jpeg

![image](https://github.com/user-attachments/assets/a0502a3f-30f4-4a6f-9684-2f062f288e50)


### Step 3: Annotate the images and save the text files in ‘text’ folder
Open the ‘text’ folder you’ve created in step 1. Create empty text files and also rename them as a number starting from 0, such as ‘0.txt’, ‘1.txt’, so on and so forth. The texts inside ‘[x].txt’ is exactly the annotation (textual prompts) corresponding with the image ‘[x].image’, where [x] is the number/index you renamed. Then you can annotate the images you’ve downloaded in Step 2 within the text files.

In our examples, since the training images are all ‘one-line-art’ drawings, so we can put the word ‘one-line-art’ in all text files. And we also add some descriptions of the features in each image, such as ‘side view’ for ‘1.jpeg’ and ‘closed eyes’ for ‘3.jpeg’. The text files and their contents will look like this:

![image](https://github.com/user-attachments/assets/d71a9788-c9fe-4c8e-b2f7-81c0fa13b1a2)


![image](https://github.com/user-attachments/assets/133a0898-0051-4cce-a728-32c81cd0d346)



| Name | Date modified | Type | Size |
| --- | --- | --- | --- |
| 0.txt | 16/2/2025 11:28 pm | Text Document | 1 KB |
| 1.txt | 16/2/2025 11:29 pm | Text Document | 1 KB |
| 2.txt | 16/2/2025 11:29 pm | Text Document | 1 KB |
| 3.txt | 16/2/2025 11:29 pm | Text Document | 1 KB |

0.txt
1.txt
2.txt
3.txt
File
Edit
View
one-line-art, faces,side view
0.txt
1.txt
2.txt
3.txt
File
Edit
View
one-line-art,a face,side view
0.txt
1.txt
2.txt
3.txt
File
Edit
View
one-line-art,head,back
0.txt
1.txt
2.txt
3.txt
File
Edit
View
one-line-art, face,front view,closed eyes

Sometimes when there are too many training images, you may also wish to annotate the images automatically by an image caption network. There are various ways to do so. For example, you can either use the custom node ‘Image_Captioning_in_ComfyUI’ to utilize the WD Tagger network, or try to run the ComfyUI MoonDream Caption workflow and use the visual language model MoonDream for captioning.

### Step 4: Run the SimpleLoRA node and LoRA fine-tune the base model with custom dataset
Build the one-node-only workflow for LoRA training with the ‘LoRATrainer’ node. Set ‘use_custom_dataset’ to ‘true’, and put the dataset root folder as the ‘dataset_path’. In our examples, the ‘dataset_path’ should be ‘D:\one-line-art’. Since there are only 4 images in a dataset, to avoid overfitting we don’t need to train the LoRA so many steps. For example, we can set the ‘training_steps’ and ‘checkpointing_steps’ to 64 and 16 respectively. The LoRATrainer with proper settings for our example can look like this:
```
#13 SimpleLoRA
LoRATrainer
ckpt_name DreamShaper_8_pruned.safetensors
use_custom_dataset true
dataset_path D:/one-line-art
lora_checkpointfolder one-line-art
lora_name one-line-art_lora
caption_column text
learning_rate 0.00010
batch_size 1
training_steps 64
checkpointing_steps 16
rank 4
random_seed 0
```

![image](https://github.com/user-attachments/assets/28f36313-67a1-43d3-a63f-3bf17837def5)

![image](https://github.com/user-attachments/assets/5f3a12db-bb9f-4fd9-9be5-886bda86428f)


After the training, we can apply the LoRA to the base network, and put the prompts ‘one-line-art, face, side view’ into the positive textual prompt to see the result. The image on the left is generated with LoRA, while the one on the right not. By comparison, we can conclude the trained LoRA indeed works.

Final Submission: Please submit the ComfyUI workflow, fine-tuned LoRA weights, and 10 output images generated by the fine-tuned model, as a single zip file. Please do remember to include a prompt text file that contains the 10 prompts used to generate the 10 submitted images. 
