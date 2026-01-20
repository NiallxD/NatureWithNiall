---
{"dg-publish":true,"dg-permalink":"custom-siril-scripts","permalink":"/custom-siril-scripts/","title":"Custom Siril Scripts for Astrophotography","hide":true,"tags":["Tech","astrophotography","Siril","imageprocessing"],"noteIcon":null,"created":"2024-11-20T11:39:34.000-08:00","updated":"2024-12-11T07:45:33.000-08:00"}
---

#tech #astrophotography #siril #imageprocessing
# Custom Siril Scripts for Astrophotography

>[!NOTE] Disclaimer
>I'm definitely not an expert when it comes to astrophotography. What I am is a proponent of sharing learnings, and sometimes when you're just starting out it's nice to hear from others at your level. So if you're like me and just getting stuck into Siril for astrophotography, this might be useful.

## What to Expect

This post will run through some of the custom scripts that I have created to speed up my processing workflow, take away some of the labour involved, and allow for consistent processing. I'll share a bit about why I use the scripts I do, and why I've made them do what they do. I'll try add something about how to configure them if you choose to use them.

I wont be going into depth about how to add custom scripts to Siril, nor will I go too deep into exactly how custom scripting works. Siril provides good documentation [here](https://siril.readthedocs.io/en/latest/Scripts.html). There is also loads on YouTube about this. 

## My Workflow

Firstly, I wanted to share my workflow. This is imporant as it is key to why I used the scripts I do.

1. Shoot the sky.
2. Copy the raw files (this includes the calibration files) to folders on my hard drive.
3. Load the photos into CaptureOne to cull the really dreadfully bad ones.
4. Load up Siril and use my Preprocessing Script (Preprocessing v1.1).
5. Then I manually process the image using:
	1. Photometric Colour Calibration.
	2. Background Extraction.
	3. Green Noise Removal.
	4. GraXpert for noise removal (Siril v1.4 will provide a GUI for this like StarNet!).
6. Next I remove the Stars with [StarNet](https://madscientistguy.com/astrophotography/how-to-add-starnet-star-removal-tool-to-siril/) (I followed this guide to add to Siril).
7. Stretch in Siril and export as Tiff.
8. Then I edit the starless version in Affinity Photo for colour and exposure.
9. Bring back into Siril to recombine with the star map.
10. Final tweaks in Affinity.

The scripting is simply to get a nice stack to work with.

## Preprocessing v1.1 Script

This is my custom Preprocessing Script. You can copy and past this into a text file, save with the extension .ssf, and then load into Siril as per the guide I linked above.

**Note:** I tend to capture over 75 frames per stack so these scripts are optimised for that. If you capture and stack less, you might be better off using Winsorized Sigma Clipping. To do so, replace the "Stack calibrated lights to final_stacked.fit rejecting those with a star roundness greater than 0.65" with the lines below:

```
# Stack calibrated lights to final_stacked.fit rejecting those with a star roundness greater than 0.65
stack r_pp_light rej 3 3 -norm=addscale -filter-round=0.65 -weight_from_noise -output_norm -rgb_equal -out=../export/final_stacked
```

### Main Preprocessing script with calibration:

```
############################################
#                                          #
# Script for Siril 1.2.0 (1.2.4)           #
# Updated: November 2024                   #
# Author: Niall Bell (niallbell.com)       #
# Script Name: Preprocessing v1.1          #
#                                          #
########### PREPROCESSING SCRIPT ###########
#                                          #
# Script for colour camera preprocessing   #
#                                          #
# Requires 4 sets of RAW images in the     #
# working directory, within 4 directories: #
# - biases/                                #
# - flats/                                 #
# - darks/                                 #
# - lights/                                #
#                                          #
#  Process files are stored on the SSD in  #
#       the documents folder those         #
#  speed up processing. Delete after use.  #
#                                          #
############ PREPROCESSING STEPS ###########
#                                          #
# 1. Convert the Bias, Flat, Dark, and     #
#    Light frames to .fits                 #
# 2. Stack the Bias frames.                #
# 3. Calibrate the Flat frames with those  #
#    stacked Bias.                         #
# 4. Stack the Flat frames.                #
# 5. Stack the Dark frames.                #
# 6. Calibrate the Light frames with the   #
#    stacked Darks and Flats.              #
# 7. Align (register) the Light frames.    #
# 8. Stack the registered Light frames     #
#    rejecting poor quality images.        #
#                                          #
############################################

# Requires at least Siril 1.2.0
requires 1.2.0

# Convert Bias Frames to .fit files
cd biases
convert bias -out=/Users/niallbell/siril/Processing/biases_fits
cd ..

# Convert Flat Frames to .fit files
cd flats
convert flat -out=/Users/niallbell/siril/Processing/flats_fits
cd ..

# Convert Dark Frames to .fit files
cd darks
convert dark -out=/Users/niallbell/siril/Processing/darks_fits
cd ..

# Convert Light Frames to .fit files
cd lights
convert light -out=/Users/niallbell/siril/Processing/lights_fits
cd ..

# Stack Bias Frames to bias_stacked.fit
cd /Users/niallbell/siril/Processing/biases_fits
stack bias rej 3 3 -nonorm -out=../export/masters/bias_stacked
cd ..

# Calibrate Flat Frames
cd /Users/niallbell/siril/Processing/flats_fits
calibrate flat -bias=../export/masters/bias_stacked

# Stack Flat Frames to pp_flat_stacked.fit
cd /Users/niallbell/siril/Processing/flats_fits
stack pp_flat rej 3 3 -norm=mul -out=../export/masters/pp_flat_stacked
cd ..

# Stack Dark Frames to dark_stacked.fit
cd /Users/niallbell/siril/Processing/darks_fits
stack dark rej 3 3 -nonorm -out=../export/masters/dark_stacked
cd ..

# Calibrate Light Frames
cd /Users/niallbell/siril/Processing/lights_fits
calibrate light -dark=../export/masters/dark_stacked -flat=../export/masters/pp_flat_stacked -cc=dark -cfa -equalize_cfa -debayer

# Align lights
register pp_light

# Stack calibrated lights to final_stacked.fit rejecting those with a star roundness greater than 0.65
stack r_pp_light rej g 0.3 0.05 -norm=addscale -filter-round=0.65 -weight_from_noise -output_norm -rgb_equal -out=../export/final_stacked

# Load the final stacked file into Siril.
cd ../export/
load final_stacked.fit

############ PROCESSING COMPLETE ###########
#                                          #
# DO NOT FORGET TO COPY THE FINAL STACKED  #
#      IMAGE INTO A SAFE LOCATION!!!       #
#                                          #
#  ============== WARNING: ==============  #
#                                          #
#      RUNNING THIS SCRIPT AGAIN WILL      #
#    OVERWRITE ANY EXISTING FILES SAVED    #
#         IN THE PROCESS FOLDER!!!         #
#                                          #
############################################
```

## Script Explained

I've chopped out the header and footer and I'll explain each section.

```
# Requires at least Siril 1.2.0
requires 1.2.0
```

All this bit does is tells Siril which version of Siril is needed for the script to run. So as long as you have version 1.2.0 or higher you're golden.

```
# Convert Bias Frames to .fit files
cd biases
convert bias -out=/Users/niallbell/siril/Processing/biases_fits
cd ..

# Convert Flat Frames to .fit files
cd flats
convert flat -out=/Users/niallbell/siril/Processing/flats_fits
cd ..

# Convert Dark Frames to .fit files
cd darks
convert dark -out=/Users/niallbell/siril/Processing/darks_fits
cd ..

# Convert Light Frames to .fit files
cd lights
convert light -out=/Users/niallbell/siril/Processing/lights_fits
cd ..
```

This section converts the Lights, Darks, Biases, and Flats into .fits files. It also creates the necessary sequence files for the next steps. I have configured this to store the files in my files stored on my SSD inside my MacBook. I found this gives me a boost in performance, and it keeps all of the process files (the intermediate .fits files that Siril creates) in one place, so that I can easily delete them without worry of deleting something important!

There is a note at the bottom of the script reminding me to copy the final result along with master calibration frames, onto my main hard drive. 

```
# Stack Bias Frames to bias_stacked.fit
cd /Users/niallbell/siril/Processing/biases_fits
stack bias rej 3 3 -nonorm -out=../export/masters/bias_stacked
cd ..

# Calibrate Flat Frames
cd /Users/niallbell/siril/Processing/flats_fits
calibrate flat -bias=../export/masters/bias_stacked
```

Here we stack the Bias Frames and use those to calibrate the Flat Frames.

*I'm in the process of testing wether to use a more advanced stacking process here such as the one used when stacking the lights. I'll update if I find it works well.*

```
# Stack Flat Frames to pp_flat_stacked.fit
cd /Users/niallbell/siril/Processing/flats_fits
stack pp_flat rej 3 3 -norm=mul -out=../export/masters/pp_flat_stacked
cd ..

# Stack Dark Frames to dark_stacked.fit
cd /Users/niallbell/siril/Processing/darks_fits
stack dark rej 3 3 -nonorm -out=../export/masters/dark_stacked
cd ..
```

Next we stack the calibrated Flat and Dark Frames to created Master Flat and Master Dark Frames which will be used to calibrate the Light Frames.

*I'm in the process of testing wether to use a more advanced stacking process here such as the one used when stacking the lights. I'll update if I find it works well.*

```
# Calibrate Light Frames
cd /Users/niallbell/siril/Processing/lights_fits
calibrate light -dark=../export/masters/dark_stacked -flat=../export/masters/pp_flat_stacked -cc=dark -cfa -equalize_cfa -debayer
```

Here we calibrate the Light Frames using the calibration frames stacked in the previous step.

```
# Align lights
register pp_light

# Stack calibrated lights to final_stacked.fit rejecting those with a star roundness greater than 0.65
stack r_pp_light rej g 0.3 0.05 -norm=addscale -filter-round=0.65 -weight_from_noise -output_norm -rgb_equal -out=../export/final_stacked
```

We're on the home stretch now, here we align the calibrated light frames, technically called Registration and Alignment, then stack those calibrated/registered Light Frames.

Here I have oped to use stacking with rejection, using the Generalised Extreme Studentised Deviate Test... because I've read it works well. The 0.3 and 0.05 are the lower and upper sigma values used for the GESDT. I also use the -filter-round=0.65 argument to reject images where the average star roundness value is lower that 65% (100% would be perfect). Depending on how sharp you can get the stars, you may wish to adjust this value (higher = more forgiving). Finally, the weight_from_nouse is used to apply a higher stack weighting to images with lower noise. I found this makes a cleaner final result.

For smaller datasets us the Winsorized Sigma Clipping with high and low values of 3 respectively.

```
# Load the final stacked file into Siril.
cd ../export/
load final_stacked.fit
```

This step loads the final stacked file into Siril.

## Summary

That's basically it, the script organises the files in a way that works for me, such as allowing the easy deletion of process files, and uses some slight more advanced stacking algorithms and parameters. It's nothing special or fancy, but I said I'd share so here it is.

## Stacking Without Calibration

I have another script which I used to stack images without doing any calibration first. I use this when I'm in the middle of a session and don't need to worry about the noise or artefacts in the final result and just want to get a view of the stacked data. This shares elements of my main script but instead only takes in Light Frames.

### Preprocessing script without calibiration

```
############################################
#                                          #
# Script for Siril 1.2.0 (1.2.4)           #
# Updated: November 2024                   #
# Author: Niall Bell (niallbell.com)       #
# Script Name: Preprocessing No Cal v1.0   #
#                                          #
########### PREPROCESSING SCRIPT ###########
#                                          #
# Script for colour camera preprocessing   #
#                                          #
# Requires just Lights and does not do     #
# any image calibration.                   #
#                                          #
# - lights/                                #
#                                          #
#  Process files are stored on the SSD in  #
#       the documents folder those         #
#  speed up processing. Delete after use.  #
#                                          #
############ PREPROCESSING STEPS ###########
#                                          #
# 1. Convert the Light frames to .fits     #
# 2. Stack the registered Light frames     #
#    rejecting poor quality images.        #
#                                          #
############################################

# Requires at least Siril 1.2.0
requires 1.2.0

# Convert Light Frames to .fit files
cd lights
convert light_seq -debayer -fitseq -out=/Users/niallbell/siril/Processing/lights_fits
cd ..

# Align lights
cd /Users/niallbell/siril/Processing/lights_fits
register light_seq

# Stack calibrated lights to final_stacked.fit rejecting those with a star roundness greater than 0.65
stack r_light_seq rej g 0.3 0.05 -norm=addscale -filter-round=0.65 -weight_from_noise -output_norm -rgb_equal -out=../export/final_stacked

# Load the final stacked file into Siril.
cd ../export/
load final_stacked.fit

############ PROCESSING COMPLETE ###########
#                                          #
# DO NOT FORGET TO COPY THE FINAL STACKED  #
#      IMAGE INTO A SAFE LOCATION!!!       #
#                                          #
#  ============== WARNING: ==============  #
#                                          #
#      RUNNING THIS SCRIPT AGAIN WILL      #
#    OVERWRITE ANY EXISTING FILES SAVED    #
#         IN THE PROCESS FOLDER!!!         #
#                                          #
############################################
```

I hope that this was useful to you, if it was and you have any questions, let me know. Otherwise, Clear Skies!

Niall

#### Header Image

The header image for this post was created using these scripts. It should the Horsehead and Flame Nebula in the Orion constellation.

![](https://i.imgur.com/Izkx8aB.jpeg)


---
Created by Niall Bell (niall@niallbell.com)