
# Multi-UAVConflictPrediction
<p align="center">
<img src="https://github-production-user-asset-6210df.s3.amazonaws.com/117992631/281018241-133bcff4-2cd8-4606-b424-5a42d834f374.gif">
</p>

UAV stands for Unmanned Aerial Vehicle. It can also be called a drone. Rather than having a human pilot on board, it’s controlled remotely by another human or it can be programmed to work on its own. They come in all different shapes and sizes too, from tiny drones that people use for hobbies and photography to larger and more complicated ones that the military uses for combat missions, surveillance, and reconnaissance.

----
**Projects**
1. Classification problem: estimate the total number conflicts between UAVs given the provided features.
2. Regression problem: predict the minimum Closest Point of Approach (CPA) [1] among all the possible pairs of UAVs
[1] CPA: An estimated point in which the distance between two objects, of which at least
one is in motion, will reach its minimum value
---
# **Solution**

------------
## **Classification task:**
### Reading Data
Reading Data
Reading the data from CSV file using pandas: To use scikit-learn library, we have to convert the Pandas data frame to a Numpy array. we consider the first 35 columns as inputs and the I num_collisions l column as output.
We can see the 5 first rows of the dataset and for UAV_5 as an example:
<p align="center">
<img width="788" alt="image" src="https://github.com/JalehFar/Multi-UAVConflictPrediction/assets/117992631/81c4ca59-d259-438a-8e5b-623ef72f2521">
</p>
### Data Analysis
Let's see how many of each class is in our data set:
<p align="center">
<img width="591" alt="image" src="https://github.com/JalehFar/Multi-UAVConflictPrediction/assets/117992631/ed458f30-8c47-40d4-aba3-a7b57c841933">
</p>
To tackle the problem of unbalanced dataset, Random over-sampling with imblearn is used. Then the dataset is normalized. Data Standardization gives the data zero mean and unit variance, it is good practice, especially for algorithms such as KNN which is based on the distance of data points. Data normalization is used to make model training less sensitive to the scale of features. This allows our model to converge to better weights and, in turn, leads to a more accurate model.

### K nearest neighbors (k-NN)(Results):

Here you can see that by increasing k the accuracy is decreased. The best accuracy (0.808) is with k=1.

<p align="center">
<img width="368" alt="image" src="https://github.com/JalehFar/Multi-UAVConflictPrediction/assets/117992631/efcfc0da-91e3-4b7b-b988-1b3f55845482">
</p>
<p align="center">
<img width="372" alt="image" src="https://github.com/JalehFar/Multi-UAVConflictPrediction/assets/117992631/a2e82667-005b-4fe9-8e05-5e7a216b4997">
</p>
<p align="center">
<img width="449" alt="image" src="https://github.com/JalehFar/Multi-UAVConflictPrediction/assets/117992631/74391b20-46ed-4b3a-be19-e8c6b189de2c">
</p>


### Support Vector Machine(SVM) (Results):

The SVM algorithm offers a choice of kernel functions for performing its processing. Basically, mapping data into a higher dimensional space is called kernelling. The mathematical function used for the transformation is known as the kernel function, and can be of different types, such as:
- 	Linear
-  	Polynomial
-  	Radial basis function (RBF)
-  	Sigmoid

Each of these functions has its characteristics, its pros and cons, and its equation, but as there l s no easy way of knowing which function performs best with any given dataset. We usually choose different functions in turn and compare the results. Let's use different kenrnels and compare the results.
However it is not a good idea to use linear kernel for this problem as our dataset has many features and they are not linearly seperable. By comparing the results, the 6-degree polynomial has the highest accuracy among all other kernels.

<p align="center">
<img width="372" alt="image" src="https://github.com/JalehFar/Multi-UAVConflictPrediction/assets/117992631/e8e7d7e4-a459-4086-8e9b-bb7409996b33">
</p>
<p align="center">
<img width="400" alt="image" src="https://github.com/JalehFar/Multi-UAVConflictPrediction/assets/117992631/17343269-32eb-4833-976a-34fcb31b021f">
</p>

### Conclusion:
Comparing to the KNN model, SVM classifies class 0 better but class 1 is less accurate. However the overal accuracy has been increased to 0.86 by the 6-degree polynomial SVM model.


------------

## **Regression task:**

### Reading and analyzing Data:
we consider the first 35 columns as inputs and the I min_CPA' column as output.

<p align="center">
<img width="250" alt="image" src="https://github.com/JalehFar/Multi-UAVConflictPrediction/assets/117992631/8a0ee7c8-5491-4562-b7a8-614fe1b6e1bd">
</p>  
Target range: [194.2, 57899.8]. So the dataset should be normalized. MinMaxScaler has been used to normalize the features and also the target.

### Modeling and Evaluation:
#### Linear Regression:
This model is so limited and it is not expected to get a good r2-score.

<p align="center">
<img width="200" alt="image" src="https://github.com/JalehFar/Multi-UAVConflictPrediction/assets/117992631/820a2ff1-14c1-4ed0-af51-a615026bcdd4">
</p>
Best possible score is 1.0 and in this case is negative (because the model is arbitrarily worse). In the general case when the true y is non-constant, a constant model that always predicts the average y disregarding the input features would get a r2-score of 0.0.
On the other hand, we can see the features are distributed somehow on a line(but with lots of outliers). So this model, with all the limitations it has, performs better than some of the other regressors.

<p align="center">
<img width="286" alt="image" src="https://github.com/JalehFar/Multi-UAVConflictPrediction/assets/117992631/d517aacf-8379-4dae-8022-c838b517b2e3">
</p>
#### Decision Tree Regression:
This model is even worse than the linear regressor as mentioned above.

<p align="center">
<img width="200" alt="image" src="https://github.com/JalehFar/Multi-UAVConflictPrediction/assets/117992631/8c0ddef6-233f-4fc7-8195-274ec645713c">
</p> 
<p align="center">
<img width="286" alt="image" src="https://github.com/JalehFar/Multi-UAVConflictPrediction/assets/117992631/107ebc42-ed7f-4db5-b77f-00488cbc6928">

#### K-Nearest Neighbors Regression:
In this case we do not have a model and we just consider the instances in the dataset. The drawback of this nonparametric model is to define a good distance fuction and I tried to do so but that was in vein and again I got negative R2-score.

<p align="center">
<img width="200" alt="image" src="https://github.com/JalehFar/Multi-UAVConflictPrediction/assets/117992631/5fdbc4b0-8935-441d-ae01-fe2516f917b0">
</p>
<p align="center">
<img width="286" alt="image" src="https://github.com/JalehFar/Multi-UAVConflictPrediction/assets/117992631/d42ed730-dd39-4fcd-9cff-bbaddace58fd">

#### Linear Support Vector Regression:
###### Regularization tuning:
If this term is high our model underfits since hyper-parameter (C) is used to penalize the overfitting problem.
- C=1:

<p align="center">
<img width="200" alt="image" src="https://github.com/JalehFar/Multi-UAVConflictPrediction/assets/117992631/a5ef97b9-9be9-4ab1-abb6-fbb9226c2dfe">
</p>
<p align="center">
<img width="286" alt="image" src="https://github.com/JalehFar/Multi-UAVConflictPrediction/assets/117992631/067cb4f4-d9ea-4d59-a4c4-5c7708acfa80">
</p>
- C=0.001:
<p align="center">
<img width="200" alt="image" src="https://github.com/JalehFar/Multi-UAVConflictPrediction/assets/117992631/400ef976-7c55-4564-be4b-2a6720bbe1cd">
</p>
<p align="center">
<img width="286" alt="image" src="https://github.com/JalehFar/Multi-UAVConflictPrediction/assets/117992631/0d0ed571-3d94-4185-ad2e-8e2e598e1aa0">
</p>
#### Polynomial Support Vector Regression:
To find the best hyper-parameters a grid search technique has been used. The best regression hyper-parameters: 'C':0.2, 'degree': 1, 'kernel':  rbf}

<p align="center">
<img width="200" alt="image" src="https://github.com/JalehFar/Multi-UAVConflictPrediction/assets/117992631/b2e83f72-5503-46f0-a5f3-2bbb79427488">
</p>
<p align="center">
<img width="286" alt="image" src="https://github.com/JalehFar/Multi-UAVConflictPrediction/assets/117992631/dd8664e0-78c2-400d-8a0a-165764d95e2a">
</p>
### Conclusion:
To sumerize, kernelized SVM with the regularization term is the best method for our problem. Since it is more robust to outliers and noises.





