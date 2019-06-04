---
type: slides
---

# Training a model with resampling

Julia Silge
Data Scientist at Stack Overflow

Notes: You just built and then evaluated models that were trained one time on the whole training set at once. Data scientists have come up with a slew of approaches to build models that perform better than this approach and a lot of important ones fall under the category of resampling.

---

# Bootstrap resampling

Sample with replacement from the original dataset

Notes: The first resampling approach we're going to try in this course is called the bootstrap. Bootstrap resampling means drawing with replacement from our original dataset and then fitting on that dataset. Let's think about cars. Let's say our training dataset has 900 cars in it. 

---


![Alt text](http://s3.amazonaws.com/assets.datacamp.com/production/course_6013/datasets/bootstrap.png)

Notes: To make a bootstrap sample, we draw with replacement 900 times from that training data to get the same size sample that we started with. Since we're drawing with replacement, we will probably draw some cars more than once. We then fit our model on that new set of 900 cars that contains some duplicates. Then we do that again.

---

![Alt text](http://s3.amazonaws.com/assets.datacamp.com/production/course_6013/datasets/bootstrap2.png)


Notes: We draw 900 times from the training dataset with replacement and fit a model. We repeat that some number of times, look at all the models we fit on the bootstrap samples, combine them, and then take an average of some kind.

This approach does take longer, obviously, than training the data one time. In your exercise, you will have a subset of the complete dataset to try this out with.

---

# Bootstrap resampling with caret

```r
> cars_rf_bt <- train(log(MPG) ~ ., method = "rf", 
+                     data = training, 
+                     trControl = trainControl(method = "boot"))
```

Notes: I am very happy to be able to tell you that training a model with bootstrap resampling is pretty darn easy in caret. All you have to do is specify method = "boot" in trainControl(), like you see here. The default behavior is to do 25 bootstrap resamplings, but you can change this if you want to.

---

# Comparing predicted to real values

```out
  `log(MPG)` `Linear regression` `Random forest`
        <dbl>               <dbl>           <dbl>
 1       2.89                2.79            2.83
 2       2.89                3.00            2.89
 3       3.26                3.22            3.26
 4       3.14                3.09            3.10
 5       3.26                3.22            3.26
 6       2.89                3.11            2.98
 7       2.48                2.59            2.51
 8       2.71                2.81            2.82
 9       3.37                3.29            3.27
10       2.83                2.90            2.90
```

Notes: We are going to evaluate our models again using metrics() from the yardstick package and I want to emphasize what it is that we are comparing when we do that. Each car has a real fuel efficiency as reported by the Department of Energy and then we have built models that predict fuel efficiency for each car. When we evaluate a model, we are calculating how far apart each predicted value is from each real value.

---

# Visualizing model predictions

![Alt text](http://s3.amazonaws.com/assets.datacamp.com/production/course_6013/datasets/cars_metrics.png)

Notes: In this lesson, you also are going to visualize these differences, like you see here. The x-axis has the actual fuel efficiency and the y-axis has the predicted fuel efficiency for each kind of model. The difference between linear regression and random forest isn't huge here, but in this case, we can see visually that the random forest model is performing better.

---

# Let's practice!

Notes: OK, now it's your turn. Let's see if you can fit these kinds of models with bootstrap resampling and find which model performs better.










