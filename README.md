
# Image to Speech: Accessibility Helper 

![picture](https://github.com/Team5CSYEFall/Project/blob/main/images/Intr.png)

Machine replication of human functions, like reading, is an ancient dream. However, over the last five decades, machine reading has grown from a dream to reality. 
Image to speech conversion is a trending aspect of computer technology. It determines an important criterion in which we interact with the system and interfaces across a variety of platforms.
It is a popular feature that lets your computer or phone read images aloud to you and is commonly used as an accessibility feature to help people who have trouble reading on-screen text, but it's also convenient for those who want to be read to.
Though the systems rely on a computerized voice speaking to you, in recent years these voices have become much more natural sounding. Many modern TTS voices are almost indistinguishable from humans, and some even incorporate natural human inflections to make them sound more lifelike. 

# Goal:
While there are many systems and applications already developed that detect objects, scenes, and faces; extract text; or systems which convert text to speech,a very few systems have been developed to extract textual information which is embedded in an image or scene and convert it to speech.
So with our project we intend to design a model/system to extract textual information from JPG, PNG files and convert it to speech.
The functionality of such a system can help to interpret information within images for people with disabilities. 

# Process Outline:

![picture](https://github.com/Team5CSYEFall/Project/blob/main/images/PD.png)

## Part 1: 
Here we provide a functionality for the user to upload images they want to read out with a webpage hosted on Amazon S3. Those images will be stored in Dynamodb for the further process.
## Part 2: 
The extraction of the text from the uploaded images will happen in this part of the pipeline. This process will take place with the help of Amazon Rekognition Image. Rekognition Image uses deep neural network models to detect and label thousands of objects and scenes in your images, and we are continually adding new labels and facial recognition features to the service. 
## Part 3:
The extracted text from the previous steps will get stored in Dynamodb to be retrieved for future use as well as will be used as an input to the Text-to-Speech conversion job.
## Part 4:
In the last part of the pipelined the stored extracted text will get converted to speech and an mp3 will be generated as an output. One can play the mp3 by using the functionality made available on the webpage.

# System Architecture: 

![picture](https://github.com/Team5CSYEFall/Project/blob/main/images/Arch.png)

# Steps to follow:
## 1. Create and host a webpage on Amazon S3
## 2. Deploying Amazon API Gateway to set off a dedicated Lambda function, “New Image Uploadt”, which is responsible for initializing the process of generating MP3 files.
## 3. Creating and deploying " New Image Upload" Lambda Function
This lambda function will be doing following tasks:
a. Inserts/stores the image into a DynamoDB table, where information about all posts is stored.
b. Run the Amazon recognition job which will extract the text from uploaded image.
c. Send notification to SNS topic to decouple the process of receiving information about new posts and starting their conversion.
## 4. Creating and deploying "Text to Audio" Lambda Function
a. this lambda function is subscribed to our SNS topic whenever a new message appears (which means that a new post should be converted into an audio file). This is the trigger.
b. it also uses Amazon Polly to convert the text into an audio file in the specified language
## 5. Creating and deploying "Get Audio" Lambda Function 
this Lambda function retrieves information about the post from the DynamoDB table.



