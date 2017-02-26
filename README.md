# floydhub.fast.ai
## Introduction
This is an examle of how to setup instance on floydhub to run lesson 1 (currently with the sample dataset) of the awesome deep learning course available at http://course.fast.ai/. 

## Set up floydhub account and working directory
First, you'll need a floydhub account and have the floyd CLI installed. Follow their online instruction at https://www.floydhub.com/welcome.

Next, you'll want to create a working directory in your local computer. All files in this directory will be uploaded to the floydhub cloud instance when you do <code> floyd run </code> (explained in detail later). 

<pre><code>
mkdir ~/Projects/
cd ~/Projects/
</code></pre>

## Set up necessary files for lesson one

If you would like to skip all the details and just to get things going, you may choose to clone all files in this repo.

<pre><code>
git clone https://github.com/YuelongGuo/floydhub.fast.ai.git
cd floydhub.fast.ai/
</code></pre>

Now, to fill in with a bit more details:

#### scripts

The fast.ai github repository (https://github.com/fastai/courses/tree/master/deeplearning1/nbs) includes files for all the phase I lessons. For the purpose of this example, and to keep it simple, I copied only the bare minimul necessary files to the working directory. This includes:
* lesson1.ipynb
* utils.py
* vgg16.py
* vgg16bn.py
* some data to analyze

#### data

I only copied the dogscats sample dataset (dogscats/sample if you unzip the dogscats.zip at http://www.platform.ai/files/dogscats.zip). However, this is considered very bad practice, because I am creating a copy of the data every time I start a new cloud instance. Floydhub has some instruction on how to manage data seperately, which I tried but haven't had it all figured out. I'll keep trying because for the full dataset this is necessary.

#### dependencies

The scripts written in lesson 1 runs with python 2 + Keras + Theano, so you need to specify <code> --env theano:py2 </code> when you start the cloud instance (more details below). However, Floydnet theano:py2 environment is still missing one package - bcolz. For this, you need to add it to file "floyd_requirements.txt". This is the floydnet default for installing dependencies.

<pre><code>
echo "bcolz" > floyd_requirements.txt
</code></pre>

## Start a floydnet instance

We will use Floyd CLI to initiate a cloud instance. Initiation:

<pre><code>
floyd init your_favorate_task_name_e.g._neural_networks
</code></pre>

To start a GPU instance with Jupyter notebook that is compatable to lesson 1 scripts, you need to specify the following parameters:

<pre><code>
floyd run \
  --mode jupyter \
  --env theano:py2 \
  --gpu
</code></pre>

Wait for a few minutes, then you should get a website address in your console for the running Jupyter notebook.

#### One more thing before running lesson 1 code

We are almost there, but there is one more thing you need to configure - the keras.json file

Start a console session in jupyter notebook, then 

<pre><code>
mkdir ~/.keras
echo '{
    "image_dim_ordering": "th",
    "epsilon": 1e-07,
    "floatx": "float32",
    "backend": "theano"
}' > ~/.keras/keras.json
</code></pre>

Please note the home directory on Floydhub instance is actually /root/ directory.

Now lesson 1 should run with the sample dataset.

## Run for the full dogs and cats dataset

For the full dataset, it is recommended that you build a data object in the floydnet. Refer to http://docs.floydhub.com/commands/data/
Then, specify the data id when you start a cloud instance:

<pre><code>
floyd run \
  --mode jupyter \
  --data data_ID_e.g._jq4ZXUCSVer4t65rWeyieG
  --env theano:py2 \
  --gpu
</code></pre>

Then the data should be available in /input directory.

After I try this out, I'll come back and share my experience. 
