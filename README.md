# iOS-Stereoscopic-ARKit-Template
iOS Augmented Reality App template, modified to support a basic stereoscopic (two side-by-side camera) view. A few lines of code (and storyboard tweaks) to make your ARKit experiment compatible with cheap mobile VR headsets* .

Language: Swift
Content Technology: SceneKit.

Written in: Xcode 9 beta 2 (9M137d)
Tested on: iPhone 7+ running iOS 11 beta 2 (15A5404i)

## Footnotes

* Mobile Headset needs to have an opening for the iPhone's camera (i.e. headset supports Mixed Reality or Augmented Reality). You can use a Google Cardboard (or similar) with a hole cut out.

Note: The background camera feed will appear as a flat background.

Note: This is experimental code running on beta software that is likely to change. Apple may also provide a function for this in the future (e.g. via SCNHeadMountedDisplayRenderingTechnique, MDLStereoscopicCamera) hinted at by Steve T-S' finding https://twitter.com/stroughtonsmith/status/776740802869460992 .
