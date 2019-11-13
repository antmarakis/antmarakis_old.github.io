---
layout: post
title: Keras k-Fold Validation
category: Artificial Intelligence
---

When we need to evaluate our model, a common and easy method we can employ is k-Fold validation. In this validation method, we split our dataset into k parts (called folds), train the model on one fold and test it on the rest.

For this tutorial, we will use the [Keras](https://keras.io/) machine learning library (alongside Pandas and Numpy) to build our model. We will then evaluate the model using k-Fold validation. Note that even though we use external libraries, k-Fold validation itself doesn't require anything on its own and can therefore be used independently.

This tutorial can be used alongside [this DataCamp tutorial](https://www.datacamp.com/community/tutorials/deep-learning-python), although it is not needed.

#### Read Data

The data we will use is the [Wine Quality Dataset](https://archive.ics.uci.edu/ml/datasets/wine+quality). It is a dataset that shows the chemical compenents of wine and the wine's quality (rated by experts). There are two types of wine in the dataset, red and white.

We are assigned with the task of predicting the quality of wine (on a scale from 0-10) given the dataset.

First we need to read the data. For this, we will use the [Pandas library](https://pandas.pydata.org/). Let's import the library and read from the csv files.

<pre>
import pandas as pd

white = pd.read_csv("winequality-white.csv", sep=';')
red = pd.read_csv("winequality-red.csv", sep=';')
</pre>

Unfortunately the two wine types are given separately. We need to merge them. To do so, we will create a new column for the type of wine. We will assign the value 0 to white and 1 to red. This is how we can add the new column:

<pre>
white['type'] = 0
red['type'] = 1
</pre>

Next we will merge the two datasets, while at the same time shuffling them.

<pre>
wines = red.append(white, ignore_index=True).sample(frac=1)
</pre>

#### Processing Our Dataset

We choose to ignore the given indices to make things cleaner, even though that is usually not necessary. The shuffling is done by the Pandas function "sample", which takes as input the fraction of the dataset we want sampled (sampling means "take a fraction of items at random").

Next we want to create the dataset (X) alongside the target values (Y). Note that since we want to predict the quality of the wine, we will have to remove the corresponding column.

<pre>
Y = np.ravel(wines.quality)
X = wines.drop(['quality'], axis=1)
</pre>

The "ravel" function takes a multidimensional array and removes the dimensionality, creating a line containing all the array elements.

#### Creating Our Model

Now we are ready to create a model. From the tutorial above, our model is the following:

<pre>
model = Sequential()
model.add(Dense(64, activation='relu', input_dim=12))
model.add(Dense(1))
model.compile(optimizer='rmsprop', loss='mse', metrics=['mae'])
</pre>

#### K-Fold Validation

Finally, we will employ the k-Fold validation technique. We will split the dataset into five parts/folds and we will calculate the average Minimum Square Error (mse) and Minimum Absolute Error (mae). Remember, in k-Fold validation we train our model on one fold and test it on the rest.

<pre>
k = 5
l = int(len(X) / k)
mse_total, mae_total = 0, 0
for i in range(k):
    test_x = X[i*l:(i+1)*l]
    test_y = Y[i*l:(i+1)*l]

    train_x = np.concatenate([X[:i*l], X[(i+1)*l:]]);
    train_y = np.concatenate([Y[:i*l], Y[(i+1)*l:]]);

    model.fit(train_x, train_y, epochs=15)

    predictions = model.predict(test_x)
    mse, mae = model.evaluate(test_x, test_y)
    mse_total += mse
    mae_total += mae

mse_avg = mse_total / k
mae_avg = mae_total / k
print(mse_avg, mae_avg)
</pre>

#### Finale

And that is it! We can now evaluate our models with the k-Fold validation method. You can tailor this function yourself by calculating your model's evaluation metrics. Here I only evaluated MSE and MAE, but you can easily alter that.

You can read the code in its entirety [here](https://github.com/MrDupin/Machine-Learning/blob/master/Keras/kFold.py). Thanks for reading!
