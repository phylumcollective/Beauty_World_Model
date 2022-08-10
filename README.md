## Beauty_World_Model


# **VAE Model**

## Data Prepper and VAE Trainer Read Me

### **How to Use The “Data Prepper and VAE Trainer” Jupyter Notebook**

## **Packages:**
If you can run the first code block with no problems, this section can be ignored.
First and foremost, OpenCV (and potentially other packages depending on your Anaconda version) may not be in the Jupyter Notebook environment by default. The easiest way I’ve found to handle this is to use the code I’ve commented out in the “imports” block (the first one).  The commented out code does the following:
1)	Imports sys, a package that I believe all recent Python versions have that allows for virtual environment manipulation
2)	Uses sys to install opencv-python, which is OpenCV’s package for python

The line immediately following the commented out section imports cv2, which is what we need from opencv-python.

The code only needs to be run once and future running of this commented out code will cause an error along the lines of “you already have this package.” I know of no further issues this will cause, but I wouldn’t test it as environments can be tricky (also it makes doing the imports take substantially longer).

## **Data Processing:**

The data processor starts by changing directory to a folder called “images.” It is expected that the images folder has the following properties:

1)	It is in the same directory as the notebook

2)	Other directories in images (there can be other stuff in images too, and in fact we will save our processed data in this directory) are collections of .NET images (if you have saved the images in another file type, change the .NET expectation in the second code block. I recommend CTRL-F’ing to find it). Each .NET image will be a training example.
    
    a)	Since we are prepping the data for the VAE, there are no further stipulations needed on how the images are organized in each directory. They could all be in one big directory for all this processor cares.

The processor then checks if **“train_data.z”** already exists. If it does, it skips all cells relating to data processing, because it already has the processed data and the notebook assumes you just want to train the VAE. In this case, it loads the already saved train data. If you would like to make a new preprocessed version of the data, you could:

1)	Delete the old preprocessed data

2)	Rename the old preprocessed data

3)	Move the old preprocessed data to another directory

Otherwise, the script will download all the images and reshape them into (128, 128, 1) np arrays. To ensure the preprocessing went as planned, look at the “Info on Picture Data” block and see if the results match the commented out section. This test assumes that the first image in the first directory (alphabetically) is the same as what I tested with, which is the following image:

<img width="468" alt="image" src="https://user-images.githubusercontent.com/49796738/183588271-3b543b8f-d110-41b4-847e-a275362c7323.png">

 
(This is image 2021-02-27-1350-01-24-22)

(Honestly though, if it goes wrong the long printout will almost certainly be nonsense that is easily detectable by human eyes, such as all 0’s or all the same value).

Once the data is done being processed and you’ve confirmed it went correctly, some more reshaping needs to happen so the VAE can train on it. 

## **Making the VAE:**

This part of the code determines the architecture of the VAE. The variable “latent_dim” controls the size of the latent vector produced by the encoder. I used 2048 exclusively, but feel free to test with larger numbers. Some architecture changes may be needed to accommodate larger dimensions, but I recommend check just changing this variable first.

## **Training the VAE:**
This part trains and saves the vae. Training will take time depending on the number of samples, size of the latent dimension, batch size, and epochs. Verbose is on so keeping track of how long training will take should be clear after the first epoch. The model is saved in a folder called “models,” which needs to be made beforehand (I think) under the name “vae.” TensorFlow has a weird way of saving weights, so do not touch the odd files made when saving. Note: If the model is already saved, this save will overwrite the old one, so move the old model if you want to keep it.



# **RNN Model**

## RNN Data Prepper Read Me

### **How to Use The “RNN Data Prepper” Jupyter Notebook**

## **Packages:**

If you can run the first code block with no problems, this section can be ignored.
First and foremost, OpenCV (and potentially other packages depending on your Anaconda version) may not be in the Jupyter Notebook environment by default. The easiest way I’ve found to handle this is to use the code I’ve commented out in the “imports” block (the first one).  The commented out code does the following:

1)	Imports sys, a package that I believe all recent Python versions have that allows for virtual environment manipulation

2)	Uses sys to install opencv-python, which is OpenCV’s package for python

The line immediately following the commented out section imports cv2, which is what we need from opencv-python.

The code only needs to be run once and future running of this commented out code will cause an error along the lines of “you already have this package.” I know of no further issues this will cause, but I wouldn’t test it as environments can be tricky (also it makes doing the imports take substantially longer).

## **Data Processing:**

The code begins by accessing the data within the images folder. This directory is assumed to be collections of folders with the following assumptions:

1)	Each directory within images represents one culture growing over time. The first image is the beginning of the culture, and each following image is the culture after a certain amount of time. The “certain amount of time” should be the same for every sample, but I doubt it will be too much of an issue if it isn’t.

2)	The images are saved as NEFs, but this is relatively easy to change in the code (simply CTRL-F “NEF”)

The data begins as a list of images from each directory, with a “|” as a delimiter between folders. Next, any sequential images are paired and these 2-tuples are saved in a list. We then use the encoder to convert each of the image tuples to latent vector tuples using the encoder, and save the training data as “train_data_rnn.z” in the images folder.

This whole process shouldn’t take too long, it took less than 10 minutes on my machine

## **Future Work:**

Carlos has spoken about the RNN taking into consideration the action taken when predicting future growth, and while I can see this being an effective strategy for different domains, the extreme reduction in dimensions from 128x128 images (16384 dimensions) to 2048 dimensions (1/8th reduction exactly) coupled with the relatively short time between photos (30 minutes if I am not mistaken) indicate to me that this would complicate the problem for the RNN for minimal benefit. However, if the latent vector dimensions were increased to say 4096 (1/4th reduction) or 8192 (1/2 reduction), I could see this potentially being a worthwhile endeavor. Whether actions are considered or not, increasing the dimensions of the latent vector would allow for the RNN to make more accurate and nuanced predictions.

## RNN Trainer Read Me

### **How to Use The “RNN Trainer” Jupyter Notebook**

## **Packages:**

Presumably cv2 has been downloaded for your Jupyter environment by now, because you’ve run one of the other data preprocessing scripts. If it hasn’t, this script will fail anyway, as it searches for `train_data_rnn.z` in the images directory, which you won’t have

## **RNN Building and Training:**

The RNN is quite small, as it inputs and outputs 2048-dimension vectors. If the dimensionality of the vectors is expanded, it may be worth considering making the RNN have an extra dropout layer, but I would be surprised if this had any appreciable yields (on the flip side, it will almost certainly contribute nearly nothing to training time as it is already a sub-10 minute script on my machine). 
Sorry for the short doc, but the notebook has about as many imports as lines of other code.


# **Controller**

## Controller Data Prepper Read Me

### **How to Use The “Controller Data Prepper” Jupyter Notebook**

## **Packages:**
Potential issues with OpenCV have been discussed in Readme’s for files that need to be ran before this one. Check those docs for information.

## **Data Processing:**
The code begins by making a list of all images within directories of the “images” directory. See the assumptions made for the VAE trainer. It then puts all these images through the trained encoder to get their latent vectors (z) and saves them. It then puts all these latent vectors through the RNN to get their predicted growth over time vector (z’) and saves these.

## **Action Processing and Future Work:**

Next, the script assigns an action to all the z-z’ pairs. Since the data we have is difficult to process algorithmically, right now the script simply assigns 22 to all the actions. The actions can be made with higher dimensions easily, as we’d just need to make a dataset that has all the dimensions as information and then alter the current action assignment section of code to format this information into the way we want (dimensionally speaking) into a list that can be appended to the “actions” list. This is what is currently done with the one dimension being action taken, making it a list of lists.

The easiest, but potentially less robust way, of matching images to actions is to just create one large single-column excel sheet/csv of actions in the chronological order they were taken. This method requires no delimiters between sessions or anything like that, as the data of z and z’ vectors are already sorted chronologically by the script (due to the images being sorted chronologically in the directories by alphanumeric ordering, the naming convention used for the images is perfect for that). This would mean you could load the action dataset in using pandas and simply go line by line of the dataset and assign that action information row to each z-z’ pair (requiring only one code block modification and a pretty minor one at that). However, using this method would make it hard for the RNN to consider actions one day, but I’ve discussed in the “RNN Data Prepper Readme” why I think that might not be worthwhile and thus not an important consideration anyway.

You could also make an excel sheet for each dataset with each image and make the initial image loading also load in the actions taken. This would require more refactoring of the code written than the previous method, as both the action pairing cell and the image importer cell would need to be changed. However, this method would allow the RNN to consider actions more easily, but that code would need to be refactored to consider the actions to make this worthwhile as well.


## **Controller Training Read Me**

### How to Use The “Controller Trainer” Jupyter Notebook

## **Packages:**
See other readme’s about cv2

## **Controller Training:**
The training data is expected to be in the images folder, which is where the data processor for this NN puts it.

I made the architecture of the controller a neural net of simple dense and dropout layers based around the length of z. The architecture was made to be robust against changes in the length of z and z’, so changes should not be needed even if editing this parameter. However, once actual training data (actions) is given, it may be worth adjusting the architecture to improve loss outcomes.

Otherwise, this script is the same as the other trainers. Compiles with MSE loss and saves the weights in the models folder.

# **Output**

## **Output Script Read Me**

### How to Use The “Output Script” Jupyter Notebook

## **Packages:**
See other Readme’s about cv2

## **Variables for Modification:**
Make sure each variable matches how each neural net was trained, otherwise there will be an error in the code.

## **Input/Output:**
This section of code begins with a line that can be edited to put an image path. This is the image to be processed and put into the neural nets. The code alters the picture so the encoder can encode it. This gives us our z. Then, z is reshaped so the rnn can accept it. z is then passed through the rnn and gives us z’. z and z’ are combined into one long vector and given to the controller. This gives us our action to do. The three outputs are saved in variables for later use and printed by the last block.




