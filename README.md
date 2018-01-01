# iOS Stereoscopic ARKit Template

__UPDATE (2017-12): The most up to date (more advanced and customizable version of this project) can now be found ["here"](https://github.com/hanleyweng/iOS-ARKit-Headset-View)__

Xcode's 'Augmented Reality App' template, modified to support a basic stereoscopic (two side-by-side camera) view. A few lines of code (and storyboard tweaks) to make your ARKit experiment compatible with mobile VR/AR headsets* (note: the FOV is narrow) .

![Snapshot of a left-eye view and right-eye view of ARKit Scenekit](snapshot.jpg)

Language: Swift

Content Technology: SceneKit.

Written in: Xcode 9 beta 2 (9M137d)

Tested on: iPhone 7+ running iOS 11 beta 2 (15A5404i)

## Footnotes

\* Mobile Headset needs to have an opening for the iPhone's camera (i.e. a headset that supports Mixed Reality or Augmented Reality). You could also use a Google Cardboard with a hole cut out.

Notes:

* The background camera feed will appear as a flat background. 

* Note: The image will also appear 'zoomed in'. This is due to the narrow horizontal FOV of the cropped camera (~38.5° ± 2° here), and the hardware camera [(~60°)](https://developer.apple.com/library/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/Cameras/Cameras.html). The human eye has a FOV of ~ 114°. In future rendering of a larger fieldOfView (resulting in a view akin to Hololens) would make more sense. For a (less elegant) implementation of a larger fieldOfView see ["iOS ARKit Headset View"](https://github.com/hanleyweng/iOS-ARKit-Headset-View).

* As of 2017-07 (iPhone7 era), the refresh rate of all mobile screens (iPhone, Android, Samsung Gear VR) are hardware-limited to a maximum of 60 FPS. Unfortunately, the higher displayed FPS seen here is a glitch, caused by the fact that the sceneView is rendered twice. For reference, the minimum framerates of the Oculus and HTC Vive is 90 FPS. The 2017 iPad Pro 10.5" (with ProMotion Display) is able to achieve 120FPS.

* Note: This is experimental code running on beta software that is likely to change. Apple may also provide a function for this in the future (e.g. see SCNHeadMountedDisplayRenderingTechnique, MDLStereoscopicCamera) as tweeted by <a href="https://twitter.com/stroughtonsmith/status/776740802869460992" target="_blank">Steve T-S</a>.

## Steps Taken

### 1. Two ARSceneViews

Put two ARSceneViews next to each other in storyboard.

### 2. Turn on the second ARSceneView (the Right Eye)

Connect the second ARSceneView to the ViewController

```
@IBOutlet weak var sceneView: ARSCNView!

@IBOutlet weak var sceneView2: ARSCNView!
```

Turn on sceneView2 in viewDidLoad()

```
override func viewDidLoad() {
    ...

    sceneView2.scene = scene
    sceneView2.showsStatistics = sceneView.showsStatistics
    sceneView2.isPlaying = true
}
```

### 3. Update RightEye every frame.

Add functions to mirror the ARSceneViews every frame.

```
func renderer(_ renderer: SCNSceneRenderer, updateAtTime time: TimeInterval) {
    DispatchQueue.main.async {
        self.updateFrame()
    }
}

func updateFrame() {
    
    // Clone pointOfView for Second View
    let pointOfView : SCNNode = (sceneView.pointOfView?.clone())!

    // Add calculations for right-eye position ...

    sceneView2.pointOfView = pointOfView
    
}
```

Add calculations for the right-eye positon in updateFrame()

```
    // Determine Adjusted Position for Right Eye
    let orientation : SCNQuaternion = pointOfView.orientation
    let orientationQuaternion : GLKQuaternion = GLKQuaternionMake(orientation.x, orientation.y, orientation.z, orientation.w)
    let eyePos : GLKVector3 = GLKVector3Make(1.0, 0.0, 0.0)
    let rotatedEyePos : GLKVector3 = GLKQuaternionRotateVector3(orientationQuaternion, eyePos)
    let rotatedEyePosSCNV : SCNVector3 = SCNVector3Make(rotatedEyePos.x, rotatedEyePos.y, rotatedEyePos.z)
    
    let mag : Float = 0.066 // This is the value for the distance between two pupils (in metres). The Interpupilary Distance (IPD).
    pointOfView.position.x += rotatedEyePosSCNV.x * mag
    pointOfView.position.y += rotatedEyePosSCNV.y * mag
    pointOfView.position.z += rotatedEyePosSCNV.z * mag
```

### Done!

Have fun! 😁
