# Emotion-Recognition
AI in Emotion Recognition. CUDA GPU. 2019.

## About
This project uses the JAFFE Database (DOI: 10.5281) to train a computer vision model to detect human facial expressions.

## The Database
The database contains 213 images of 7 facial expressions (6 basic facial expressions + 1 neutral) posed by 10 Japanese female models. Each image has been rated on 6 emotion adjectives by 60 Japanese subjects. The database was planned and assembled by Michael Lyons, Miyuki Kamachi, and Jiro Gyoba. The photos were taken at the Psychology Department in Kyushu University.

Sample image of "Happiness" from the dataset:

![TM HA2 181](https://user-images.githubusercontent.com/44657125/142353552-8557ce46-833c-4e11-b3f8-e3a0853b0f59.jpg)

## Methodology
### Why Only Six?
One of the challenges in detecting emotion is that the expressions an individual makes in response to a stimulus is unique to their facial features and character. For the purposes of constructing my Neural Network (NN) to indetify emotion the entire range of human experiences needed to quantised. My work as well as that of the JAFFE Database works off of Paul Eckamn's identification of happiness, sadness, disgust, fear, surprise, and anger as the dominant categroies emotions fall into in psycology. However, it is impossible to acertain a 'pure' emotion, as the emotions one feels lie on a spectrum across all possible states. 

Professor Michael Lyons writes:

"Included ... are files containing semantic rating data from
psychological experiments using the images. I send it because I
believe it is important to realize that expression are never pure
expressions of one emotion, but always admixtures of different
emotions. The expression labels on the images just represent the
predominant expression in that image - the expression that the subject
was asked to pose.

Best regards,

Michael J. Lyons, Ph.D.

Professor of Image Arts and Sciences

Ritsumeikan University

Kyoto, Japan

michael.lyons@gmail.com"

  
### Our Approach

By taking the averaged semantic ratings we can see a distribution across the six main categories of emtions. If maximum value will be marked as the correct answer when asked 'What emotion is this?' However, by acknolwedging that an expression can be calssified into multiple categories, we can normalize the outputs and interpreet them as the correct output vector. This will allow for loss propogation methods such as Mean Sqaured Error to be more effectively deployed since there is more than one output channel to be considered, the output vector now has a size from zero to six non-negligible outputs affecting the loss function.

## The Model
The models for this task ranged custom convultion NN's to more complex pre-made NN's such as AlexNet. I chose several candidate NN architectures from those that performed best ImageNet and could be easily loaded by PyTorch to be customized. I decided to customize the ResNet152 NN for my task and it reached an out-of-sample accuracy of 98.4% when tasked with identifying the dominant emotion. The "Adam" optimizer, a batch size of 16 over the course of 30 epochs was used to achieve this result. A snapshot of the weights of the model were saved in after each epoch so regardless if the model became overfit, the state of best performance could be recovered. Instead it became the goal to train until an overfit occured as this would signal that all the necessary epochs and more have already passed and we can be confident that the saved state is the best for this NN.

### Training
The JAFFED Database is relatively small which becomes an even greater challenge when the dataset needed to be partitoioned into seperate traigning and evaluation subsets. Furthermore, because there are only Japenese female models for the images, the accuracy of the model on faces with non-common features in Japan is questionable.

I tackled the first issue of a small dataset by creating duplicates that are different in some way, such as rotation, lighting, or color.
Here is an example of such an image altered for trainign purposes, classified under "Sad."

![image](https://user-images.githubusercontent.com/44657125/142353602-8b88f419-f185-4b52-b114-f6e04e5c1ffa.png)

Training a NN with 152 layers such as the ResNet152 architecture was intensive for my desktop, so this project was my first use of CUDA GPU computing optimizations.
Thanks to this optimization the model converged on an acceptable out-of-sample and in-sample loss in a handful of epochs.

![image](https://user-images.githubusercontent.com/44657125/142354066-304e2c9a-daa2-4829-937a-aa22123dcd2c.png)

![image](https://user-images.githubusercontent.com/44657125/142354074-5a5872be-f0eb-439c-ad74-716904390fe4.png)

Accuracy initially plummeted but then rose to an appreciable amount in a dozen epochs, finally reaching a maximum of 98.4% accuracy out-of-sample.

![image](https://user-images.githubusercontent.com/44657125/142354262-2f01898b-1951-44fd-ae73-783f8af9be9a.png)

## Future
I have already attempted to export this model through PyTorche's trace methods into 'traced script module' of ResNet18 to export this project to mobile.
As my first mobile app project, I am currently working with making the Android's camera output bitmap agreeable with the data fed into the model to turn this projetc into an app in the future.

I also hope to augment this with the FACES dataset and over registration free databases.

## Acknolwdgements
Huge thanks to Michael J. Lyons, Shigeru Akamatsu, Miyuki Kamachi, and Jiro Gyoba for making this database open source! 20 years later, without this database this project would not have been possible. This was my first AI project which I started in my Junior year of high school, I [scoured databses](https://en.wikipedia.org/wiki/List_of_facial_expression_databases). 

However after filling out forms for these universities and instutions I never recieved any response.

### Citations
Michael J. Lyons, Shigeru Akamatsu, Miyuki Kamachi, Jiro Gyoba.
Coding Facial Expressions with Gabor Wavelets, 3rd IEEE International Conference on Automatic Face and Gesture Recognition, pp. 200-205 (1998).
http://doi.org/10.1109/AFGR.1998.670949
Open access content available at: https://zenodo.org/record/3430156
