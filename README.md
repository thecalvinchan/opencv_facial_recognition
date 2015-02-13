# Eigenface Facial Recognition with OpenCV

## Dependencies

- OpenCV (http://opencv.org/)

To install OpenCV, I suggest following the directions [here](http://docs.opencv.org/doc/tutorials/introduction/linux_install/linux_install.html#linux-installation)

If you're on OS X, you can also use [brew](https://jjyap.wordpress.com/2014/05/24/installing-opencv-2-4-9-on-mac-osx-with-python-support/)

    brew tap homebrew/science 
    brew install opencv

Try your respective package managers if you're on some other UNIX distribution.

After you install OpenCV, it's important you put the directory to the Python
scripts into your path, otherwise you won't be able to use the Python Wrapper.
An easy way to do this is to symlink them into your existing Python path, or you
can just add the new directories to your path.

    cd /Library/Python/2.7/site-packages/
    sudo ln -s /usr/local/lib/python2.7/site-packages/cv2.so cv2.so
    sudo ln -s /usr/local/lib/python2.7/site-packages/cv.py cv.py
    // Assuming you installed OpenCV into /usr/local
    // If you used brew, it would actually be /usr/local/Cellar

## Compile and Build

    mkdir build
    g++ src/eigenface_classifier/classifier.cpp -o build/classifier -lopencv_core -lopencv_highgui -lopencv_contrib
    g++ src/eigenface_recognizer/recognizer.cpp -o build/recognizer -lopencv_core -lopencv_highgui -lopencv_contrib

## Generate an Eigenface Classifier with Sample Images

We'll use a simple csv to determine each sample image location as well as its
label (a unique identifier of the person in the image).

    /location/to/image/of/person/1_a.jpg;1
    /location/to/image/of/person/1_b.jpg;1
    /location/to/image/of/person/1_c.jpg;1
    /location/to/image/of/person/2_a.jpg;2
    /location/to/image/of/person/2_b.jpg;2
    /location/to/image/of/person/2_c.jpg;2

We can then create a classifier by running the following program

    ./classifier data.csv classifier.yml

The classifier will be stored in `classifier.yml` or whatever name you pass as
the second argument.

## Loading an Existing Classifier and Predicting a Match

    ./recognizer classifier.yml image_to_predict.jpg

It is important to note that the image you want to run the prediction on must be
the same size as the images you used to train the classifier. An easy way to do
this is to run the included Python script to crop the pictures prior to
classification and prediction.

## Useful Utilities

Detect primary face in a picture and crop image to that face

    python lib/python2.7/face_crop/face_crop.py --cascade=lib/python2.7/face_crop/haarcascade_frontalface_alt.xml `ls -d <TRAINING_IMAGE_DIR>/*`
    python lib/python2.7/face_crop/face_crop.py --cascade=lib/python2.7/face_crop/haarcascade_frontalface_alt.xml image_to_predict.jpg

