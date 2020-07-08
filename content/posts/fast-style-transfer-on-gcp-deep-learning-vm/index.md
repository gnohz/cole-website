---
title: "Fast Style Transfer on GCP Deep Learning Vm"
date: 2020-07-03T12:57:19-04:00
draft: true
---

# Prereqs

Read the README https://github.com/hwalsuklee/tensorflow-fast-style-transfer

# Setup


## Start a tensorflow vm from google cloud.

First create a GCP Project.

Then navigate to [Google Cloud Deep Learning VM](https://console.cloud.google.com/marketplace/details/click-to-deploy-images/deeplearning?_ga=2.183710050.555794926.1594171077-1241815506.1593038576)

Create a vm named "style-trained-vm". I'm using zone "us-west1-b" and GPU "NVIDIA Tesla K80".

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
# Download the vgg19 file with curl
curl -O -L http://www.vlfeat.org/matconvnet/models/imagenet-vgg-verydeep-19.mat

# Download the MSCOCO database with curl
curl -O -L http://msvocds.blob.core.windows.net/coco2014/train2014.zip

# Download one of the pretrained tensorflow style models. 
# First download megatools.
sudo apt install megatools
# Use megatools to download the file
# This command is interactive and will 
# prompt you to select which files to download. 
# Make sure to select three files corresponding to a single style. 
# These three styles will work together as a single trained model. 
# For example I selected `2 3 4`.
megatools dl "https://mega.nz/folder/VEAm1CDD#ILTR1TA5zFJ_Cp9I5DRofg" --choose-files
{{</ highlight >}}

# Run Style Transfer

The following command will run the style transfer algorithm.

{{< highlight bash >}}
# Command outline
python run_test.py --content <content file> --style_model <style-model file> --output <output file> 

# Example command
python tensorflow-fast-style-transfer/run_test.py --content tensorflow-fast-style-transfer/content/chicago.jpg --style_model la_muse.ckpt --output first.jpg
{{</ highlight >}}

Pat yourself on the back; You just performed your first style transfer, and it was fast!

# Train

Now we can use the following command to start training a model with one of the given style images from the model

{{< highlight bash >}}
# Command outline
python run_train.py --style <style file> --output <output directory> --trainDB <trainDB directory> --vgg_model <model directory>

# Example command
python tensorflow-fast-style-transfer/run_train.py --style tensorflow-fast-style-transfer/style/la_muse.jpg --output models --trainDB train2014 --vgg_model vgg19 --test tensorflow-fast-style-transfer/content/chicago.jpg --batch_size 8
{{</ highlight >}}

In order to train your own styles you need to copy them over to the instance. I like putting all the styles I am interested in into one directory and then copying it over using scp.

`gcloud compute scp --project artifai --zone us-west1-b --recurse myStyles style-trainer-vm:~/`

In order to streamline the process for training multiple models at once, you can use the following bash script which starts a training process for each of the images in a folder.

{{< highlight bash >}}
FILES="./myStyles/*"

for file in $FILES; do

  filename=$(basename -- "$fullfile")
  extension="${filename##*.}"
  filename="${filename%.*}"
  echo "Creating model directory"
  mkdir -p model_${filename}
  echo "Starting trainer process"
  nohup python tensorflow-fast-style-transfer/run_train.py --style colorbeet.jpg --output models --trainDB train2014 --vgg_model vgg19 --test tensorflow-fast-style-transfer/content/chicago.jpg --batch_size 8 > nohup_${filename}.out &

done

# https://stackoverflow.com/questions/965053/extract-filename-and-extension-in-bash
# https://www.cyberciti.biz/faq/bash-loop-over-file/
# https://unix.stackexchange.com/questions/45913/is-there-a-way-to-redirect-nohup-output-to-a-log-file-other-than-nohup-out
{{</ highlight >}}

The `nohup` is in order to ensure that the process keeps running when you terminate the ssh session. Execute the file `./start-trainers.sh`.

# Using Your Trained Model.

Now that your model is trained, we can put it to use just like we did with the pretrained model we downloaded earlier.
