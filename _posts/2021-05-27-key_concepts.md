# ML Concepts
* Independent variables - variables on which the predictions are calculated
* Predictions - results of the model
* Label - The data that we're trying to predict, such as "dog" or "cat"
* Architecture - The _template_ of the model that we're trying to fit; the actual mathematical function that we're passing the input data and parameters to
* Weights - parameters of the model
* Model - The combination of the architecture with a particular set of parameters
* Fit - Update the parameters of the model such that the predictions of the model using the input data match the target labels
* Train - A synonym for _fit_
* Fine-tune - Update a pretrained model for a different task
* Epoch - One complete pass through the input data
* Loss - A measure of how good the model is, chosen to drive training via SGD
  > Remember that while `loss` reminds one of a metric, it is slightly different. `loss` is a measure of performance that the training system can use to update weights accordingly. In our case `loss` is something that `SGD` can conveniently use. `metric` is designed for easy human consumption, and might not be suitable for weight updation. There could be a few rare situations where `loss` is a good metric, apparently.
* Target - dependent variable that is supposed to be predicted, but used for training through training set
* SGD (Stochastic Gradient Descent) - a mechanism to update the weight assignments
* Training accuracy - accuracy seen on the train data set (in cases of classification)
* Validation accuracy - accuracy seen on the validation data set, using model trained on training data set (also in case of classification)
* Overfitting - when a model is trained a little too much on the data, and so isn't generalizable enough on new and unseen data. When validation accuracy starts getting worse, we would know that the model has been overfit. There are many ways to reduce overfitting
* Underfitting - when a model hasn't been trained enough yet, and so can be trained further. Model is underfit as long as we see both the training and validation accuracies continue to go down
* Metric - a function that predicts the quality of model performance using validation set. In case of classification, `error_rate` is a common metric used in classification tasks. Another common metric is `accuracy` (which is just `1 - error_rate`)
* Transfer Learning - using a pretrained model for a task different to what it was originally trained for
* Epoch - the number of times the model should look at each unit of data, usually dependent on the training time available
* Fine-tuning: A transfer learning technique where the parameters of a pretrained model are updated by training for additional epochs using a different task to that used for pretraining
* Pretrained model - A model that has already been trained, generally using a large dataset, and will be fine-tuned
* CNN - Convolutional neural network; a type of neural network that works particularly well for computer vision tasks

