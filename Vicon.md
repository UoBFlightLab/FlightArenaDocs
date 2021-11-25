# Vicon

The Vicon motion capture system is set up in the arena to enable tracking of objects throughout the
volume. 

## Background

The Vicon cameras work by flashing an set of powerful infrared LEDs. The light from these LEDs is
reflected back to the cameras by retroreflective markers, typically small spheres. Each camera
processes the image it captures and attempts to detect circles. It then sends the centroid and
radius of each detected circle over the network to the Vicon PC.

On the Vicon PC, an application called Tracker receives the centroids and radii from all the cameras
and uses them, along with knowledge of the camera positions, to calculate the 3D position of each
marker within the volume. Using knowledge of relative marker positions on a particular object,
Tracker is able to detect which object it is tracking and reconstruct the 3D pose of the object.

Reconstruction can be impaired by a number of factors:
- Occlusion: Where markers are not visible or overlap from certain orientations
- Marker size: Smaller markers are harder for Vicon to track
- Position in the volume: Certain positions are covered by fewer cameras
- Ambiguity: Where the observed pattern could come from multiple orientations

### Marker placement

While setting out the markers in an aesthetically pleasing pattern is hard to resist, it makes
the orientation of the object much more likely to be ambiguous to Tracker. Placing the markers as
randomly as possible over your object will give better results. Equally, using the biggest markers
you can get away with will improve tracking robustness.

As a caveat to that, if you are setting up a fleet of similar vehicles, it may be worth coming up
with some form of pattern to ensure that the marker distribution is distinct enough between each
vehicle. However, be *very* careful of rotational symmetry.

## Calibration

The Vicon calibration process is simple, but can be time consuming.

### Masking

The first stage of calibration is setting the camera masks. These set regions of the image which
the cameras will not search for markers. In the Flight Arena, the masks are used to prevent the 
cameras from detecting each other and to eliminate reflections off the scaffolding used to support
them.

The masks can be automatically generated, but this requires that all markers (or spurious reflective
objects) are removed from the tracking volume. Once all objects are removed, you can check to see if
any cameras are still detecting centroids using the camera view. To do this, go to the "System" tab
and select all the cameras (Use Shift+Click). Then in the top-left of the view, choose "Cameras"
from the dropdown.

In this view, the output of the cameras can be viewed in a few ways. On the top bar, three icons are
used to configure the view. The left most controls if the greyscale output of the cameras is
displayed. This is very useful when trying to remove additional reflective objects. The centroids
seen by each camera are shown as small yellow crosses when the second icon is enabled.

Ideally, you will want no grayscale points visible throughout the image. Small grey marker balls are
very hard to spot against the carpet but they are almost certainly there. Awkwardly stepping around
the room, trying to spot when the grey point disappears has been the most reliable technique. This
is greatly sped up with the help of the Vicon Control app which allows you to view the camera
outputs on a phone.

Once the volume is free from objects and random reflective things, you can go to the "Calibrate"
tab. The first panel in this tab is "Create Camera Masks". Clicking "Start" in this panel will 
automatically paint the mask over any points that are currently grey in each camera image. You can
click "Stop" pretty soon after as this process is very quick.

### Calibration

Next is calibrating the cameras positions relative to each other. Tracker does this by having you
move a known calibration object through the volume. In the Flight Arena, we use the Vicon Active
Wand (v2). The Active Wand has a set of both red and IR LEDs that are illuminated throughout the
calibration process. See the [Active Wand](#Active-Wand) section below for details on use.

In the "Calibrate Cameras" panel on the "Calibrate" tab, ensure that, under "Show Advanced" the
"Wand" is set to "Active Wand v2". Then click "Start".

The (visible) LEDs on all cameras will start flashing, showing that they are gathering calibration
frames. The display on each camera will show a circle that fills in as calibration progresses.

During calibration, you should move the wand gracefully through the tracking volume. The aim is to
ensure all cameras see the wand while it is also seen by at least one other camera in a number of
different positions. Ideally the wand should be seen across the entire frame of each camera, though
this is difficult to achieve.

### Origin

## Adding a new object

A new object is added to be tracked on the "Objects" tab.

## Output

Typically, you will want to get the output of Vicon for use in your own code. This can be done in a
number of ways. The most commonly used in the lab is Tracker's UDP output. Tracker also supports TCP
output and now has an offical Python API.

### UDP Output

Tracker can output a UDP stream that contains the position and orientation of each tracked object.
The stream is in a binary format that is documented towards the end of the Vicon Tracker User Guide.
The guide is available to download from [docs.vicon.com](https://docs.vicon.com/display/Tracker39/PDF+downloads+for+Vicon+Tracker).

The [vicon_udp](https://github.com/UoBFlightLab/vicon_udp) GitHub repository contains C++ code for
both ROS and ROS2 nodes that listen to the Vicon UDP stream. Versions are also  available for
[Rust](https://github.com/rob-clarke/vicon_rs) with a Python version pending.

#### Enabling the output

*In the Flight Arena, the UDP stream will typically be enabled by default so this should not be
needed.*

### TCP Output

## Active Wand

The Active Wand has an internal battery that needs charging prior to use.