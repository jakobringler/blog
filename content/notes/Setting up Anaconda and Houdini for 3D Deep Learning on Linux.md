---
title: "Setting up Anaconda and Houdini for 3D Deep Learning on Linux"
tags:
- linux
- houdini
- anaconda
- machinelearning
- pytorch
- pytorch3d
enableToc: false
---

# Setting up Anaconda & Houdini for 3D Deep Learning on Linux

To get started install [Anaconda](https://anaconda.org/) and run the following commands in your terminal:

( **hou** is the name of the new enviroment and python **3.7.4** is required because this is the version used in Houdini )

```bash
conda create -n hou python=3.7.4
```

```bash
conda activate hou
```

Then install libraries like for example [PyTorch 3D](https://github.com/facebookresearch/pytorch3d). ( For simplicities sake we will go forward with vanilla [PyTorch](https://pytorch.org/). )

```bash
conda install pytorch torchvision torchaudio cudatoolkit=11.3 -c pytorch
```

As [jpparkeramnh](https://www.sidefx.com/profile/jpparkeramnh/) pointed out in [this](https://www.sidefx.com/forum/topic/58397/) sideFX forum post you have to export the **LD_PRELOAD** variable.

```bash
export LD_PRELOAD=$CONDA_PREFIX/lib/libpython3.7m.so
```

I also recommend sourcing Houdini in the terminal.

To do so first open the .bashrc in the terminal

```bash
nano .bashrc
```

and add the following lines to your .bashrc

```bash
cd /opt/hfs19.0/
source ./houdini_setup
cd ~
conda activate hou
export LD_PRELOAD=$CONDA_PREFIX/lib/libpython3.7m.so
alias expenv='LD_PRELOAD=$CONDA_PREFIX/lib/libpython3.7m.so'
```

-   The first 3 lines run the houdini setup bash script
-   Line 4 activates a default environment ( for me “hou” )
-   Line 5 exports the LD_PRELOAD varible of the currently activated environment ( hou )
-   Line 6 creates an alias to export the currently activated environment from the terminal

Now you should be able to just activate your respecitve conda environment, export the correct LD_PRELOAD variable by typing “**expenv**” and then run houdini from there by typing “**houdini**“.

To check if everything is running as expected open the Houdini Python shell and type:

```python
import torch 
```

If it doesn’t give you any errors you should be good to go.



Related: [[Linux]] [[notes/Houdini |Houdini]] [[PyTorch]] [[notes/Deep Learning |Machine Learning]] [[Anaconda]] [[PyTorch3D]]