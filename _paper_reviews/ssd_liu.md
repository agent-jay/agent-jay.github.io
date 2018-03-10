---
title: "SSD: Single Shot MultiBox Detector"
date: 2017-11-15
authors: Wei Liu, Dragomir Anguelov, Dumitru Erhan, Christian Szegedy, Scott Reed, Cheng-Yang Fu, Alexander C. BergPaper name

---

## Summary

This paper introduces a new end-to-end deep learning architecture to perform
object detection. This isn't the first paper to do such a thing, but has higher
accuracy and speed (FPS) than previous approaches. This also removes the need
for region proposals. Borrows quite a few ideas from YOLO, Faster-RCNN and MultiBox.

Architecture:
VGG base net-> multiple conv layers to generate feature maps at diff scales-> conv. prediction layer

Pixel wise classification of object type, as well as offset of the default box
or anchor box (that is centered on the pixel). Similar to pixel wise
segmentation. The essential difference between this and sliding window approach
is that this is done with convolution, and the outputs integrate into the loss.

If you look at it as a cell-wise prediction (not exactly pixel since it's based
on feature map volume), then it's easy to understand how each conv filter
actually performs the final prediction. Each activation map(5) refers to
confidence of class in the default box (corresponding to pixel) as well as
location error or offsets(x,y,w,h)- so 5 predictions.

## Things I learned

- You can use convolutional filters for prediction. Just use multiple filters
  for multiple classes/targets. You can do both classfication and regression

## Questions

- What are default boxes?
- How are default boxes different from sliding windows? 
- How to combine predictions for *multiple objects* at different scales? Non max
suppression doesn't care about labels, just bounding box positions and scores. Is it a per category NMS?

## Answers

- The default boxes are essentially boxes(of differing aspect ratios) around
anchor points. Each anchor point corresponds to a cell in the feature map. This
operation of extracting default boxes by striding by single pixels is
exactly a convolutional operation. So it's easy & fast to implement.
- Since the default boxes are extracted using convolution (part of the
network), the correct output (ground truth) needs to be provided to the
specific set of outputs of the net (while the others outputs are told that they
are wrong. This is done in the loss function).  In a sliding window approach,
the NN is trained on the positive and negative windows. That said, it isn't
very hard to modify sliding window into this convolutional framework.



