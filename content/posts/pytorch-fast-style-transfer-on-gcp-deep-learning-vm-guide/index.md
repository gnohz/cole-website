---
title: "Pytorch Fast Style Transfer on Gcp Deep Learning Vm Guide"
date: 2020-07-20T21:40:23-04:00
draft: true
---

# Prereqs

Read the README https://github.com/hwalsuklee/tensorflow-fast-style-transfer

# Setup

## Start a pytorch vm from google cloud.

First create a GCP Project.

Then navigate to [Google Cloud Deep Learning VM](https://console.cloud.google.com/marketplace/details/click-to-deploy-images/deeplearning?_ga=2.183710050.555794926.1594171077-1241815506.1593038576)

Create a vm named "pytorch-style-vm". I'm using zone "us-west1-b" and GPU "NVIDIA Tesla K80".

Change the framework to pytorch.


Select "Install NVIDIA GPU driver automatically on first startup?".

## Setup developer environment

Set up the [Cloud SDK](https://cloud.google.com/sdk/install).

Copy files like this:
`gcloud compute scp --project artifai --zone us-west1-b --recurse <local file or directory> style-trainer-vm:~/`

Connect ssh like this:
`gcloud compute ssh --project artifai --zone us-west1-b style-trainer-vm -- -L 8080:localhost:8080`

## Configure the vm

Connect via ssh, and then we can start downloading the necessary files.

{{< highlight bash >}}

# Download the MSCOCO database with curl (this will take a while)
curl -O -L http://msvocds.blob.core.windows.net/coco2014/train2014.zip
unzip train2014.zip
mkdir train2014/main
echo train2014/* | xargs mv -t train2014/main

Use my fork which has some small fixes.

# Install the requirements

python -m pip install -r requirements.txt
sudo apt update && sudo apt upgrade
sudo apt install expect
conda install -c eumetsat expect

# Download one of the pretrained styling models. 

wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1JhKKnNQfkCuIK54ApYK5QnIX4KrOWSR3' -O cuphead_10000.pth

{{</ highlight >}}

# Run Style Transfer

The following command will run the style transfer algorithm.

{{< highlight bash >}}

# Command outline
python3 test_on_image.py --image_path <path-to-image> \
                         --checkpoint_model <path-to-checkpoint> \

# Example command
python3 test_on_image.py  --image_path images/content/zurich.jpeg \
                                  --checkpoint_model cuphead_10000.pth

{{</ highlight >}}

Pat yourself on the back; You just performed your first style transfer, and it was fast!

Copy it to your computer using gcloud scp:

{{< highlight bash >}}
gcloud compute scp pytorch-1-vm:~/Fast-Neural-Style-Transfer/images/outputs/stylized-zurich.jpeg ./
{{</ highlight >}}

Here is something what your image should look like:

{{< image src="zurich.jpeg" caption="Prestylized Zurich">}}

<br><br>

{{< image src="stylized-zurich.jpeg" caption="Stylized Zurich">}}

# Train

Now we can use the following command to start training a model with one of the given style images from the model

# Command outline
{{< highlight bash >}}
python3 train.py  --dataset_path <path-to-dataset> \
                  --style_image <path-to-style-image> \
                  --epochs 1 \
                  --batch_size 4 \
                  --image_size 256
{{</ highlight >}}


# Example command

{{< highlight bash >}}
python3 train.py  --dataset_path train2014 \
                  --style_image images/styles/cuphead.jpg \
                  --epochs 1 \
                  --batch_size 8 \
                  --image_size 256
{{</ highlight >}}

In order to train your own styles you need to copy them over to the instance. I like putting all the styles I am interested in into one directory and then copying it over using scp.

`gcloud compute scp --project artifai --zone us-west1-b --recurse myStyles style-trainer-vm:~/`

In order to streamline the process for training multiple models at once, you can use the following bash script which starts a training process for each of the images in a folder, two at a time (the gpu can only handle two parallel training processes at a time).

{{< highlight bash >}}
#!/usr/bin/env bash

FILES="./popularPresetImages/*"
# try using parallel

mkdir -p trainingOutput

# trap 'kill $(jobs -p)' SIGINT SIGTERM EXIT

batch_count=0
for file in $FILES; do
        filename=$(basename -- "$file")
        extension="${filename##*.}"
        filename="${filename%.*}"
        echo "Starting Training Process for ${file}"
        echo "stdbuf -o 0 python3 train.py --dataset_path train2014 \
                         --style_image $file \
                         --epochs 1 \
                         --batch_size 8 \
                         --image_size 256 \
                         --style_size 512 \
                         --lambda_content 1e5 \
                         --lambda_style 1e10 \
                         --lr 1e-3 \
                         --checkpoint_interval 2000 \
                         --sample_interval 1000 &> trainingOutput/${filename}_output.txt &"

        stdbuf -o 0 python3 train.py --dataset_path train2014 \
                         --style_image $file \
                         --epochs 1 \
                         --batch_size 8 \
                         --image_size 256 \
                         --style_size 512 \
                         --lambda_content 1e5 \
                         --lambda_style 1e10 \
                         --lr 1e-3 \
                         --checkpoint_interval 2000 \
                         --sample_interval 1000 &> trainingOutput/${filename}_output.txt &

        # Increment batch count
        ((batch_count=batch_count+1))
        echo "Batch count: $batch_count"
        # If batch_count equals 2, wait for processes to end and then reset batch_count
        [[ batch_count -eq 2 ]] && echo "Waiting for training to complete" && wait && batch_count=0
done

{{</ highlight >}}

Execute the script `nohup bash start-train.sh`.

# Now the models are training. This will take a long time.

`nohup` is in order to ensure that you can disconnect from the vm without interrupting the training.

Started training at 12:30.

# Using Your Trained Model.

Now that your model is trained, we can put it to use just like we did with the pretrained model we downloaded earlier.
