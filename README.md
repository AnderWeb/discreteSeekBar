#DiscreteSeekBar

![screenshot](https://lh6.googleusercontent.com/-JjvxVMCm1ug/VHUPWVBfpbI/AAAAAAAAHtQ/TPtoOjHI5MA/w639-h356/seekbar2.gif)

![screenshot](https://lh3.googleusercontent.com/-7nbVPXxUhYk/VG-rO64pMWI/AAAAAAAAHsM/aMRglt2Vzrk/w639-h480/animation.gif)

DiscreteSeekbar is my poor attempt to develop an android implementation of the [Discrete Slider] component from the Google Material Design Guidelines.

##Prologe
I really hope Google provides developers with a better (and official) implementation ;)

##Warning
After a bunch of hours trying to replicate the exact feel of the Material's Discrete Seekbar, with a beautiful stuff-that-morphs-into-other-stuff animation I was convinced about releasing the current code.

```java
android.util.Log.wtf("WARNING!! HACKERY-DRAGONS!!");
```
I've done a few bit of hacky cede and a bunch of things I'm not completely proud of, so use under your sole responsibility (or help me improve it via pull-requests!)

##Implementation details
This thing runs on minSDK=7 (well, technically could run 4 but can't test since AVDs for api 4 are deprecated and just don't boot).
Obviously some of the subtle animations (navigating with the Keyboard, the Ripple effect, text fade ins/fade outs, etc) are not going to work on APIS lower than 11, but the bubble thing does. And I haven't found a way of improving this with 11-21 APIs, so...

The base SeekBar is pretty simple. Just 3 drawables for the track, progress and thumb. Some touch event logic to drag, some key event logic to move, and that's all.

It supports custom ranges (custom min/max), even for negative values.

The bubble thing **DOESN'T USE** [VectorDrawableMagic] . I was not really needed for such a simple morph. It uses instead an [Animatable Drawable] for the animation with a lot of hackery for callbacks, drawing and a bunch of old simple math.

>For this to work (and sync with events, etc) I've written a fair amount of shit questionable code...

The material-floating-thing is composed into the WindowManager (like the typical overflow menus) to be able to show it over other Views without needing to set the SeekBar big enough to account for the (variable) size of he floating thing.

>For this I'm not sure about the amounts of things I've copied from [PopupWindow] and the possible issues.

##Dependencies
It uses **com.android.support:support-v4** as the only dependency.

##Usage
This is published in jCenter so you need to use the appropiate repo:

```groovy
repositories {
    jcenter()
}

dependencies {
    compile 'org.adw.library:discrete-seekbar:1.0.1'
}
```

Once imported into your project, you just need to put them into your layous like:

```xml
<org.adw.library.widgets.discreteseekbar.DiscreteSeekBar
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:dsb_min="2"
        app:dsb_max="15"
/>
```

####Parameters
You can tweak a few things of the DiscreteSeekbar:

* **dsb_min**: minimum value
* **dsb_max**: maximum value
* **dsb_value**: current value
* **dsb_mirrorForRtl**: reverse the DiscreteSeekBar for RTL locales
* **dsb_allowTrackClickToDrag**: allows clicking outside the thumb circle to initiate drag. Default TRUE
* **dsb_indicatorFormatter**: a string [Format] to apply to the value inside the bubble indicator.
* **dsb_indicatorPopupEnabled**: choose if the bubble indicator will be shown. Default TRUE 

####Design
 
* **dsb_progressColor**: color/colorStateList for the progress bar and thumb drawable
* **dsb_trackColor**: color/colorStateList for the track drawable
* **dsb_indicatorTextAppearance**: TextAppearance for the bubble indicator
* **dsb_indicatorColor**: color/colorStateList for the bubble shaped drawable
* **dsb_indicatorElevation**: related to android:elevation. Will only be used on API level 21+
* **dsb_rippleColor**: color/colorStateList for the ripple drawable seen when pressing the thumb. (Yes, it does a kind of "ripple" on API levels lower than 21 and a real RippleDrawable for 21+.
* **dsb_trackHeight**: dimension for the height of the track drawable.
* **dsb_scrubberHeight**: dimension for the height of the scrubber (selected area) drawable.
* **dsb_thumbSize**: dimension for the size of the thumb drawable.
* **dsb_indicatorSeparation**: dimension for the vertical distance from the thumb to the indicator. 

You can also use the attribute **discreteSeekBarStyle** on your themes with a custom Style to be applied to all the DiscreteSeekBars on your app/activity/fragment/whatever.

##License
```
Copyright 2014 Gustavo Claramunt (Ander Webbs)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

[Discrete Slider]:http://www.google.com/design/spec/components/sliders.html#sliders-discrete-slider
[VectorDrawableMagic]:https://developer.android.com/reference/android/graphics/drawable/AnimatedVectorDrawable.html
[Animatable Drawable]:https://developer.android.com/reference/android/graphics/drawable/Animatable.html
[PopupWindow]:https://developer.android.com/reference/android/widget/PopupWindow.html
[Format]:https://developer.android.com/reference/java/util/Formatter.html

