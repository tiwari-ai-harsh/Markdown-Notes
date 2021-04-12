# Getting Started with Tensorman

2020-05-09, Ian Hensel

# My Tensorman Workflow

This is a quick and dirty write up of my tensorman (nvida-docker) workflow. Like others, I've had to dig through various github issues and discussions to figure out which arguments I need to use.

I hope this helps simplify getting started! Happy training! 

**My system:** 2018 Oryx Pro, Pop_os! 19.10

## What do we want and why?

I am a data scientist so I want jupyter notebooks and gpu support in a container!

This is what tensorman is for! to help make nvidia-docker management easy. Sweet!

The gpu(s) accelerate compute for data sci workflow with RAPIDS ([https://rapids.ai/](https://rapids.ai/)) or general training of Deep Learning models. 

Jupyter notebooks make experimentation easy before we wrap our model code in scripts for serving.

## Scenario

You have an image classification problem with a local dataset of images and you want to build your model in jupyter notebooks and train CNNs on your local gpu.

## Let's get started

These instructions expect that you've already installed `tensorman` and it works. 

**Install Instructions:** [https://support.system76.com/articles/use-tensorman/](https://support.system76.com/articles/use-tensorman/)

## Build our Custom DL Image

```python
tensorman +1.14.0 run -p 8888:8888 --root --python3 --gpu --jupyter --name pasta-gpu  bash
```

- `1.14.0` specifies the TF version
- `-p`  sets the port
- `--root` runs as root for more control
- `--python3` we want to use python 3
- `--name` this specifies the name of the container, in this case **pasta-gpu**

**Note:** this might take a few mins to download everything 

After you run this you will get a bash prompt in your terminal, that looks something like: 

![Getting%20Started%20with%20Tensorman%207a68dd942f134d97a3f64e29a205ed34/Untitled.png](Getting%20Started%20with%20Tensorman%207a68dd942f134d97a3f64e29a205ed34/Untitled.png)

## Let's install some packages we will want

Probably want OpenCV :-)

Resource: [https://github.com/NVIDIA/nvidia-docker/issues/864](https://github.com/NVIDIA/nvidia-docker/issues/864)

```bash
# In the container shell

apt-get update
apt-get install -y libsm6 libxext6 libxrender-dev
pip install opencv-python
```

**Check what's installed:**

`pip list`

## Save Custom image

From the Tensorman page - [https://support.system76.com/articles/use-tensorman/](https://support.system76.com/articles/use-tensorman/)

Once you’ve made the changes needed, open another terminal and save it as a new image:

```bash
tensorman save CONTAINER_NAME IMAGE_NAME
```

**In this case this would look like:**

_*In a new terminal*_,

```bash
tensorman save pasta-gpu pasta-gpu
```

## IMPORTANT NOTE:

As you develop your project, it is likely you will add more libraries, you will want to keep track of these with a requirements.txt file as you go and I recommend saving out your image. 

```bash
$ pip freeze > requirements.txt
```

## Check to see available containers

```bash
tensorman list
```

![Getting%20Started%20with%20Tensorman%207a68dd942f134d97a3f64e29a205ed34/Untitled%201.png](Getting%20Started%20with%20Tensorman%207a68dd942f134d97a3f64e29a205ed34/Untitled%201.png)

## Start your Custom container

- notice the quotations around the image name (I am using zsh, if you are using bash I dont think you need these)

```bash
tensorman '=pasta-gpu' run -p 8888:8888 --gpu --root bash
```

The container will spin up and you will have the familiar bash prompt

Now, to fire up our notebooks!

```bash
jupyter notebook --allow-root --ip=0.0.0.0 --no-browser
```

You will see some output from the `NotebookApp` in your terminal, then there will be a URL like:

```bash
http://127.0.0.1:8888/?token=8bf2ba5dca7dd6dgggd9c7drandom9cc3a5cda170f
```

![Getting%20Started%20with%20Tensorman%207a68dd942f134d97a3f64e29a205ed34/Untitled%202.png](Getting%20Started%20with%20Tensorman%207a68dd942f134d97a3f64e29a205ed34/Untitled%202.png)

**open this in your browser:**

![Getting%20Started%20with%20Tensorman%207a68dd942f134d97a3f64e29a205ed34/Untitled%203.png](Getting%20Started%20with%20Tensorman%207a68dd942f134d97a3f64e29a205ed34/Untitled%203.png)

## General Docker stuff

To exit the container: `cntrl-d`

# :party-parrot: