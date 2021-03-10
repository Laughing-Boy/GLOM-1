# GLOM
An attempted implementation of Geoffrey Hinton's paper "How to represent part-whole hierarchies in a neural network" for MNIST Dataset.

## Running
Open in jupyter notebook to run
Program expects an Nvidia graphics card for gpu speedup.

## Implementation details
Three Types of networks per number of vectors

1) Top-Down Network
2) Bottom-up Network
3) Attention on the same layer Network

### Intro to State
There is an initial state that all three types of network outputs get added to after every time step. 
The bottom layer of the state is the input vector where the MNIST pixel data is kept and doesn't get anything added to it to retain the MNIST pixel data.
The top layer of the state is the output layer where the loss function is applied to the networks. 

### Explanation of compute_all function

Each network will see a 3x3 grid of vectors surrounding the current network input vector at the current layer. 
This is done to allow information to travel faster laterally across vectors. 
The easy way to do this is to shift every vector along the x and y axis and then concatenate the vectors ontop of eachother so that every place a vector used to be in the state now contains every vector and its neighboring vectors in the same layer. 

![](https://github.com/RedRyan111/GLOM/blob/main/Imgs/compute_all_function_concatenation.png)

Then, these vectors are fed to each type of model. The models will get an input of all neighboring state vectors for a certain layer for each pixel that is given. Each model will then output a single list of vectors. After each model type has given an output, the three lists of vectors are added together. 

![](https://github.com/RedRyan111/GLOM/blob/main/Imgs/compute_all_function_concatenate_to_delta.png)

This will give a single list of vectors that will be added to the corresponding list of vectors at the specific x,y coordinate from the original state.

![](https://github.com/RedRyan111/GLOM/blob/main/Imgs/compute_all_function_add_delta_to_state.png)

Repeating this step for every list of vectors per x,y coordinate in the original state will yield the full new State value. 

Since each network only sees a 3x3 grid and not larger image patches, this technique can be used for any size images and is easily parrallelizable. 

## Issues
There is a current issue that the networks will try and make all the output vectors the same. 
The source of the problem was thought to be caused by one of the reasons below, but no fixes have been found so far:

* test if putting MNIST data into first vector of the state is done correctly
* test if layers are being shifted down the correct dimensions
* test if concatening the shifted states are done correctly
* test if network outputs are being added correctly
* test if MNIST target data is being shaped correclty for loss function
* test if MNIST target data is compared to the last vector of the state correctly

# If you find any issues, please feel free to contact me