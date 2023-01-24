# Inkrypt Videos Android SDK Integration

## Adding dependency
### Add maven repository
In project settings.gradle:
```gradle
repositories {
    maven { url 'https://github.com/inkryptvideos/android-maven-repo/raw/main/repo' }
        }
```

In app module build.gradle:

```gradle
dependencies {
    implementation 'com.inkryptvideos.android:inkryptvideos-android:2.8.1'
}
```

## AndroidManifest.xml
Make sure you add this line in your activity tag in the:
```xml
<activity
    android:configChanges="orientation|screenSize|layoutDirection"
    ...
```

## Usage
### Initialising the player
#### Add player fragment to XML Layout
```xml
<fragment
    android:name="com.inkryptvideos.android.InkPlayerFragment"
    android:id="@+id/ink_player_fragment"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:keepScreenOn="true"
    android:tag="PlayerFragment"/>
```

#### Binding the player view
```java
InkPlayerFragment inkPlayerFragment = (InkPlayerFragment) getSupportFragmentManager().findFragmentById(R.id.ink_player_fragment);
```
#### Initialising player
Please make sure to get these credentials using your own Backend APIs. Follow the integration guide in the dashboard in order to get the OTP with the right configurations.
```java
// preparing player parameters
InkInitParams params = new InkInitParams.Builder()
        .setVideoId("a2b05c42c08047b8952aa857770ba819")
        .setOtp("2601de3780264eefb6b017f2d13441b172cea8c4242f4e5883107fd391c3444e")
        .setAutoplay(false) // set autoplay to off
        .setInitialAspectRatio(1.78f) // this is optional use it to set initial aspect ratio before any video is loaded
        //If you load another video the player will keep the same aspect ratio you don't need to call it twice
        .build();

//Pass the created parameters to the bound player fragment
        inkPlayerFragment.init(params);
```
### Callbacks
#### Error callback
Use the InkPlayerErrorCallback to set an error listener:
```java
inkPlayerFragment.setErrorCallback(new InkPlayerErrorCallback() {
@Override
public void onError(@NonNull InkPlayerError error) {
        switch (error.getCode()){
        case InkPlayerError.ERROR_AUDIO_MANAGER: {
        //todo: do something with this error
        }
        // other InkPlayerError.* errors:
        case InkPlayerError.ERROR_IO: { /* TODO: */ }
        case InkPlayerError.ERROR_CONTENT_PARSING: { /* TODO: */ }
        case InkPlayerError.ERROR_DECODING: { /* TODO: */ }
        case InkPlayerError.ERROR_DRM: { /* TODO: */ } // you can test this with the corrupt video
        case InkPlayerError.ERROR_MISSING_OTP: { /* TODO: */ }
        case InkPlayerError.ERROR_MISSING_VIDEO_ID: { /* TODO: */ }
        case InkPlayerError.ERROR_INVALID_OTP: { /* TODO: */ }
        case InkPlayerError.ERROR_NETWORK: { /* TODO: */ }
        case InkPlayerError.ERROR_RESOURCE_NOT_FOUND: { /* TODO: */ }
        }
        }
        });
```


#### Fullscreen callback
Use the InkPlayerFullScreenCallback to get notified when the player enters or exits fullscreen mode

```java
inkPlayerFragment.setFullscreenCallback(new InkPlayerFullScreenCallback() {
@Override
public void onEnterFullscreen() {
        // Player entered fullscreen
        }
@Override
public void onExitFullscreen() {
        // Player existed fullscreen
        }
        });
```
**Note:** The fragment needs to expand into its parent activity when it's in fullscreen mode. You should use this callback to hide/unhide other views from the host Activity.

### Player controls
```java
// Set autoplay
inkPlayerFragment.setAutoplay(true);
// enter fullscreen
        inkPlayerFragment.goFullScreen();
// exit fullscreen
        inkPlayerFragment.exitFullScreen();
// Play
        inkPlayerFragment.play();
// Pause
        inkPlayerFragment.pause();
// Returns true if player is playing a video
        inkPlayerFragment.isPlaying();
// Returns true if player is loading a video
        inkPlayerFragment.isLoading();
```