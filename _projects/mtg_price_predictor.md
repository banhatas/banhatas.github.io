---
layout: page
title: MTG Price Predictor
description: Using PyTorch models to predict prices of Magic the Gathering cards
img: assets/img/neural_net.jpg
importance: 1
category: work
related_publications: 
---

<h3>Introduction</h3>
Magic: the Gathering (MTG) is one of the most popular trading card games worldwide. One subculture that has developed from this is that of treating the game as a form of investment. Many individuals and businesses make profits from buying cards for cheap and selling them. On top of this, Wizards of the Coast (the company behind MTG) releases new sets of cards a few times per year. Wizards of the Coast show off cards weeks before they release, and many people are interested in what the eventual price of the card will be. Therefore, in this project I aim to create a tool that will predict the stable price of a MTG card based solely on the image.

<h3>Data</h3>
Due to the massive popularity and community support behind MTG, it was not difficult to create a dataset of card images and their associated prices. I used the <a href='https://github.com/EskoSalaka/mtgtools'>mtgtools python module</a> to download both card image data and its associated price. Because cards can have different prices associated with the card’s art, for each unique card name I used the most recent art/price pair for the dataset. In the end I was able to aggregate over 2000 cards to train the model on. Unfortunately due to size limitations, only 1000 were able to be trained on at one time without further work to make it more efficient.

<h3>Modeling and Training</h3>
For this project I developed and trained 3 regression models, named Aleph, Beit, and Gimel. I chose to develop regression models as I am attempting to predict card prices which is a continuous variable.


<h5>Aleph</h5>
The first model is a simple linear model. I flattened each image into a 1-dimensional array and trained on this data. It has 5 linear layers with a ReLU between each one. Below is the graph for the train and validation loss.

<div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/aleph.png" title="aleph model" class="img-fluid rounded z-depth-1" %}
</div>
<div class="caption">
    Figure 1. Train and Validation loss for the Aleph model
</div>

<h5>Beit</h5>
The second model is a convolutional neural network (CNN). Using a CNN, I was able to input the original image (3-dimensions: width, height, and RGB channels). On each card there are certain features that can be indicative of how much a card could cost. These include, rarity, set, release year, etc. In terms of size, these features are typically very small relative to the rest of the card. My hope was that the model would pick up on these features to help inform the model, but looking at the feature map below it does not seem like it was able to pick up on these small features.

<div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/beit.png" title="example image" class="img-fluid rounded z-depth-1" %}
</div>
<div class="caption">
    Figure 2. Train and Validation loss for the Beit model.
</div>

<div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/feature_map.png" title="beit feature maps" class="img-fluid rounded z-depth-1" %}
</div>
<div class="caption">
    Figure 3. Feature map of first convolutional layer for the beit model.
</div>

<h5>Gimel</h5>
After creating and developing both of these models, I decided to find a preconstructed model online and adapt it to this dataset. For this I found <a href='https://github.com/hugohadfield/pytorch_image_regession'>Hugo Hadfield’s image regression model</a> on GitHub. This model is a more complicated but generalized CNN. Below is the graph for train and validation loss.

<div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/gimel.png" title="example image" class="img-fluid rounded z-depth-1" %}
</div>
<div class="caption">
    Figure 4. Train and Validation loss for the Gimel model.
</div>

For each model I used a training loop that trained on 75% of the data and tested on the other 25%. Throughout the 750 epochs (1 card per epoch), I did validation testing on the entire test set about 100 times. Using MSE as the objective and the Adam optimizer, I ran the training loop for each model and plotted the training loss and validation loss per vs the epochs.

<h3>Results</h3>
Looking at the MSE of each model, we see that the Gimel model had the lowest validation loss. As expected, the linear model had the highest validation loss of all of the models. With image data, it seems consistent that convolutional networks work better, especially when working with RGB data. Looking at all the figures below, we see that all models tended to overfit, though some were more drastic than others.

In practice, there is still much work to be done, but it seems that the Gimel model performed better than just predicting the mean price.

<h3>Resources</h3>
You can find the Github repository for this project <a href='https://github.com/banhatas/MTG_Price_Predictor'>here</a>.
