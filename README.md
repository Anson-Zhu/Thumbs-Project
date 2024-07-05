# Gym Equipment Classification

 This project is aimed to help beginners to fitness who want to join a gym and start working out. By using ImageNet and a retrained version of resnet-18, this project classifies several common pieces of gym equipment (treadmill, bench press, dumbells, row machine, aerobic steppers, and elliptical) commonly found in most commercial gyms. This way, anyone with a New Years resolution or simply a keen will to get in shape can do so without as much fear or aprehension as now they have a way of identifying foreign equipment to them, allowing them to then to search online how to use that equipment now that they have its name.

![Learning Right and Wrong](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTP48_Pa0TpCLrywae00zr7EROeEhLgVvg67A&s)

ImageNet

ImageNet, and ResNet-18 in this case take large datasets of pictures and are trained to recognize those pictures and classify them based on the categories they were trained on. In this case, I retrained the ResNet-18 model on images of gym equipment, meaning over hundreds of images and several trials it was trained to learn how to differentiate a bench press, dumbell, treadmill, row machine, aerobic stepper, or elliptical. With the trained model, by inputting a relatively isolated picture (one piece of equipment, so as to not clutter the picture) into the retrained ResNet-18 model, the AI can then output the image back to the user with the classification and the confidence (ex- 56.45% Confidence Bench Press).

## Running this project
**Downloading My Model (Quicker)
1.Log into your nano on VS Code.
2.  Make sure you are in the root directory and download this repository with this command in the VS Code terminal: git clone https://github.com/Anson-Zhu/Thumbs-Project.git
3. Rename the downloaded directory to "my-recognition"
4. Also make sure you have jetson-inference. If you don't, follow download steps here: https://student.idtech.com/courses/331/pages/build-the-project-from-source?module_item_id=26813
5. Skip down to the video tutorial or written instructions for how to use my gym equipment classification model!

**Manually training your own gym classification model
1. Log into your nano on VS Code.
2. If you haven't already, go through the steps to download jetson-inference (https://student.idtech.com/courses/331/pages/build-the-project-from-source?module_item_id=26813)
3. Now, download the following file off of Kaggle and into your computer directly: https://www.kaggle.com/datasets/dutt2302/gym-equipment
4. Then, drag and drop the file in its entirety from your downloads into jetson-inference/python/training/classification/data  **delete the multi machine file because of inconsistency in the pictures. Also, it is wise to manually go into the bench press and row machine files and manually remove clipart or other misrepresentative pictures as they may throw off results when training.
5. Rename the file we just transfered as "gym_data".
6. Navigate into gym_data and click "new folder" and add a "train", "val", and "test" folder
7. Then, move the folders of all the images (named "dumb_bell", "bench_press", etc) into the train folder. Then, navigate back to gym_data and then into the "val" folder. In this folder, create new folders to match what is inside the "train" folder (so "dumb_bell", "bench_press", etc). Then take a couple images from each folder in "train" and move them to their corresponding folder in "val". This should only be a few images per file, the vast majority remain in "train".
8. Navigate back to gym_data, and press "new file". Name this file "labels.txt", and then navigate into it. Within "labels.txt", write one line for each folder within "train", so line 1 would be "aerobic_steppers", line 2 would be "dumb_bells", etc.
9. Now, navigate up back to jetson-inference/python/training/classification, and then cick on "models". In models, click on "New Folder" and name it "gym".
10. Now, we train! We are working in the terminal now, so within the terminal type: cd jetson-inference
11. Now that we are inside jetson-inference, run the following command: echo 1 | sudo tee /proc/sys/vm/overcommit_memory (this will overcommit memory, as during training you will likely run out otherwise)
12. Now, we're going to run the docker. Input into the terminal: ./docker/run.sh
13. Make sure your text is white now (indicating you are inside the docker), and input: cd python/training/classification
14. Now, the training command: python3 train.py --model-dir=models/gym data/gym_data --epochs=15 (you can set epochs to whatever you like, but I consider 10 a minimum)
15. After training has finished, it's time to export the model: python3 onnx_export.py --model-dir=models/gym
16. Now, look in jetson-inference/python/training/classification/models/gym and there should now be something named resnet18.onnx (this is your trained model!)
17. Exit the docker using Ctl + D
18. If you don't have one already, click on the nvidia file at the top and create a new folder called "my-recognition".
19. Drag and drop the "gym" folder with the model inside (resnet18.onnx) and the gym_data folder with all the training, val, and test images. Now, you are ready to use the model with the video tutorial. Download some images off the internet (of one of the pieces of gym equipment the model was trained on), drag and drop them into my-recognition/gym_data/test and follow along.

[View a video explanation here](https://drive.google.com/file/d/15fmB0E7TFRuXqO7OAEsNwKkkUqXVvwSr/view?usp=drivesdk)
**Written instructions
1. In the terminal, go up a directory into nvidia: cd ..
2. Navigate into my-recognition: cd my-recognition
3. Verify that the resnet18.onnx model is inside "gym": ls gym
4. Next we're going to set the NET and DATASET variables:
      1. NET=gym
      2. DATASET=gym_data
5. Now, we're going to actually run an image through our retrained model: imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/image1.jpg test1.jpg
6. Done! Now for any image you can copy the command above and just change "image1.jpg" to whatever the name of the image you want to classify is (inside my-recognition/gym_data/test), and change "test1.jpg" to whatever name you want the classification output to be named as!
