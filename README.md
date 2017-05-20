# pycon-haar
Additional information for my poster session at PyCon 2017

## Positive Pool Expansion

As seen on mrnugget's [excellent writeup on the topic](https://github.com/mrnugget/opencv-haar-classifier-training), we can use the following script from his repo to generate additional positive images from a small sample pool of cropped positive images:

```
perl bin/createsamples.pl positives.txt negatives.txt samples 1500
"opencv_createsamples -bgcolor 0 -bgthresh 0 -maxxangle 1.1  -maxyangle 1.1 maxzangle 0.5 -maxidev 40 -w 80 -h 40"
```

There are a lot of options here that are better documented over at the repository, but the important bit is the *1500* which defines the quantity of images to generate. A good starting point is converting 40-60 positive images to 1500.

## Cascade Training

`opencv_trainscascade` comes bundled with OpenCV, and is the go-to tool for training these classifiers. Keep in mind that this can take *a very long time*. For instance, training in my example took about three days on a 2015 Macbook Pro.

```
opencv_traincascade -data classifier -vec samples.vec -bg negatives.txt -numStages 20 -minHitRate 0.999 -maxFalseAlarmRate 0.5 -numPos 1000 -numNeg 600 -w 80 -h 40 -mode ALL -precalcValBufSize 1024
-precalcIdxBufSize 1024
```

Again, we can see a lot of options here, and these are definitely worth playing around with on a per-case basis. The documentation can be found [here](http://docs.opencv.org/2.4/doc/user_guide/ug_traincascade.html).

## Real-time Detection

OpenCV actually ships with a pretty cool sample script for real-time detection. It can be found at `~/opencv-3.1.0/samples/python/facedetect.py` in your OpenCV installation. It takes a Haar classifier as a `--cascade` input, meaning it can be used with anything you've trained using this method (not just faces).

## Additional Reading

* [Train your own OpenCV Haar classifier](https://github.com/mrnugget/opencv-haar-classifier-training)
* [OpenCV Train Cascade Docs](http://docs.opencv.org/2.4/doc/user_guide/ug_traincascade.html)
* [Face Detection using Haar Cascades](http://docs.opencv.org/master/d7/d8b/tutorial_py_face_detection.html#gsc.tab=0)
* [Train Your Own OpenCV Haar Classifier](http://coding-robin.de/2013/07/22/train-your-own-opencv-haar-classifier.html)
* [Face Detection Using Haar Cascades](http://docs.opencv.org/trunk/d7/d8b/tutorial_py_face_detection.html)
* [Haar-like Features](https://en.wikipedia.org/wiki/Haar-like_features)
* [Cascading Classifiers](https://en.wikipedia.org/wiki/Cascading_classifiers)
