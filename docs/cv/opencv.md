### OpenCV functions:

Unless otherwise stated, you can pass the same Mat as an `input` parameter and
an `output` parameter, to overwrite the original Mat.

#### void Imgproc.cvtColor(Mat input, Mat output, int code);

Convert the color representation of `input`, and save it in `output`.

`code` is a constant representing what conversion to make, like
`Imgproc.COLOR_BGR2HSV` which says to convert from BGR to HSV. You can also
convert to and much more, by using different constants. The conversion
constants are enumerated in the [Imgproc
JavaDocs](http://docs.opencv.org/java/3.1.0/index.html?org/opencv/imgproc/Imgproc.html).

#### void Core.split(Mat frame, ArrayList<Mat> channels);
```java
ArrayList<Mat> channels = new ArrayList<Mat>();
Core.split(myFrame, channels);
// The first channel is channels.get(0), the second is channels.get(1), etc.
```

#### void Core.merge(ArrayList<Mat> channels, Mat frame);

Merge individual channels into a single multi-channel frame.

```java
ArrayList<Mat> channels = new ArrayList<Mat>();
// ... do stuff with channels...
Mat merged = new Mat();
Core.merge(channels, merged);
```

#### void Core.inRange(Mat input, Scalar lowerBound, Scalar upperBound, Mat output);

Filters the `input` into `output`. A given pixel in `output` is white
(numerically, 255) if the corresponding input pixel is between `lowerBound` and
`upperBound`; it is black otherwise (numerically, 0).

```java
Core.inRange(hueChannel, new Scalar(80), new Scalar(100), filteredHueChannel);
```

#### bitwise operations

There are several bitwise-operation methods. These do logical operations (like
AND, OR, NOT) on the pixel values of two Mats. A white pixel is all 1-bits,
a black pixel is all 0-bits. So, ANDing a white and a black pixel, for example,
results in a black pixel.

- `void Core.bitwise_and(Mat input1, Mat input2, Mat output);`
- `void Core.bitwise_or(Mat input1, Mat input2, Mat output);`
- `void Core.bitwise_not(Mat input, Mat output);`
- `void Core.bitwise_xor(Mat input1, Mat input2, Mat output);`

  XOR is "exclusive or". This means OR, but not AND.

For example, you can filter the hue channel and the value channel, and then
`bitwise_and` them together with Core.bitwise_and to get a Mat filtered by both
hue and value.

**These only work if each input Mat has the same number of channels**

#### erode and dilate

```java
Mat Imgproc.getStructuringElement(int shape, Size kernelSize);
void Imgproc.erode(Mat input, Mat output, Mat kernel);
void Imgproc.dilate(Mate input, Mat output, Mat kernel);
```

There is more explanation of erode and dilate below (the
"December 15 Lesson" section).

#### void Imgproc.findContours(Mat image, ArrayList<MatOfPoint> contours, Mat hierarchy, int mode, int method);

Finds contours in `image` using the method specified by `method`, and
adds them to the list `contours`. Each contour is a `MatOfPoint`, which
is a matrix of points.

If you don't want to use the hierarchy, you can pass a `new Mat()`.

E.g., find contours with rectilinear boundaries:

```java
ArrayList<MatOfPoint> contours = new ArrayList<MatOfPoint>();
Imgproc.findContours(filteredImage, contours, new Mat(), Imgproc.RETR_EXTERNAL, Imgproc.CHAIN_APPROX_SIMPLE);
```

Now each member of `contours` is a contour, represented as a `MatOfPoint`.

#### MatOfPoint, MatOfPoint2f

A `MatOfPoint` is a matrix of `Point`s. A `Point` has integer coordinates.

A `MatOfPoint2f` is a matrix of `Point2f`s. A `Point2f` has floating point coordinates.

In certain cases these are basically used as lists of points.


Methods:
- `myMatOfPoints.convertTo(otherMat, conversion)` -- convert the `MatOfPoint` to another type.
  In particular, you'll probably use the following conversion to convert a `MatOfPoint`
  to a `MatOfPoint2f`:

  `myMatOfPoints.convertTo(myMatOfPoint2f, CvType.CV_32FC1);`

#### void Imgproc.minEnclosingCircle(MatOfPoint2f points, Point center, float[] radius);

This determines the center and radius of the smallest circle that encloses all
of the points in `points`.

The only *input parameter* is `points`.

The method tells you the centerpoint by writing to `center` (an *output
parameter*).

The method tells you the radius by assigning to the first element in the array
`radius` (an output parameter).

### java.util.ArrayList:

An `ArrayList` stores a list of things, and has some methods for gettings the
things and appending things.

The type of an ArrayList is *parameterized* by the type of the elements in the list. For example:

```java
// an ArrayList of Strings:
ArrayList<String> namesOfPeople = new ArrayList<String>();
// an ArrayList of Mats:
ArrayList<Mat> channels = new ArrayList<Mat>();
```

Methods:
- `get`: `myArrayList.get(i)` returns element `i` of `myArrayList`.
- `add`: `myArrayList.add(thing)` adds `thing` to the end of `myArrayList`
- many more, all described in the JavaDocs
