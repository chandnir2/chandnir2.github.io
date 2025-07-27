---
title: GrocerEye - A YOLO Model for Grocery Object Detection
excerpt: >-
    This project is an investigation into real time object detection for food sorting technologies to assist food banks during the Covid-19 pandemic. I trained a YOLOv3 model, pretrained on ImageNet, on the Frieburg grocery dataset that was annotated with object detection labels. By training for 6000 iterations and 13 hours on a Google Colab GPU, the model was able to achieve 84.59% mAP and 70.10% IOU on the test set. I demonstrate the effectiveness of the model on images from the test set and inference from a natural video. Though the model performs well on the test set, it does not seem to be effective enough to deploy for real time object detection at this time. I discuss the challenges and possible extensions of this work.
date: '2020-12-18'
thumb_img_path: images/GrocerEye/grocereye_thumbnail.JPG
thumb_img_alt: GrocerEye Predictions
external_url: 'https://github.com/bhimar/GrocerEye'
layout: post
---