### Castle

### Vegetation Prediction
Vegetation prediction is part of a demo I created for my bachelor's thesis to demonstrate practical applications of #MachineLearning using #PyTorch in @sidefx #Houdini. In this example a pix2pix-like ML model is used to translate terrain data into vegetation scattering maps: 🧵

video demo vegetation

This concept is heavily inspired by the erosion prediction use case that @sidefx and @anastasiaopara already demonstrated. @chriskopic also provided some inspiration with a ML focused tutorial in @entagma 's PDG 101. To get started and see what it can do I also implemented it.

video demo erosion

I used PDG in both cases to generate the training data. pix2pix can be trained with image pairs like the one below. The RGB channels were used to encode different kinds of information:
Left: R (Height), G (Sun Direction), B (Water)
Right: R(Trees), G (Grassy Stuff), B (Bushes)

test image

To generate the training data you already need to have a working algorithm. In the erosion example the erosion node provides the necessary logic to generate eroded terrain based on an input. For the vegetation I built an algorithm using the heightfield tools and some custom VEX.

At that moment the following question arises: "why do you need a trained ML model, if you already have the algorithm that gets it right 100% of the time?" While this is true, a ML model can improve cooking performance from multiple seconds or even minutes to mere milliseconds.

At the moment a big part of the task is wrangling and preparing / reading back the data and making sure everything has the right shape and properties to be used in a ML model. 

#PyTorch is really well documented and not too complicated considering the advanced math that's happening under the hood. I recommend to start by building simple models from scratch to learn about the concepts before diving into pretrained models and more advanced architectures.

I wrote in more detail about it on here: fxnotes.xyz/notes/

### Tower from Sketch

"Tower Sketcher" is another demo I created for my bachelor's thesis to demonstrate practical applications of #MachineLearning using #PyTorch in @sidefx #Houdini. In this example a convolutional and a feed forward neural net predict HDA parameter values from a user drawing: 🧵

The underlying idea is to train a ML model to learn the relationship between the user sketch and the resulting parameter values. Once it has seen enough examples it should be able to make sense of the drawings and output values for towers it has never seen before.

IMAGE

To generate a big dataset I created an HDA that can output a 3D Model as well as the corresponding 2D Silhouette. PDG then generates many samples consisting of an image and the parameter values, which were used to generate that image. 

dataset example

At the moment a big part of the task is wrangling and preparing / reading back the data and making sure everything has the right shape and properties to be used in a ML model. 

#PyTorch is really well documented and not too complicated considering the advanced math that's happening under the hood. I recommend to start by building simple models from scratch to learn about the concepts before diving into pretrained models and more advanced architectures.

I wrote in more detail about it on here: fxnotes.xyz/notes/