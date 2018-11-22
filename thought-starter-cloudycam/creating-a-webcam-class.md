# Creating a Webcam class

To grant the app access to your camera, build a `Webcam` class to host the camera’s main capabilities by creating a `webcam.js` file in the `src`directory:

```text
    // src/webcam.js
    export class Webcam {
      constructor(webcamElement, canvasElement) {
        this.webcamElement = webcamElement;
        this.canvasElement = canvasElement;
      }

      adjustVideoSize(width, height) {
        const aspectRatio = width / height;
        if (width >= height) {
            this.webcamElement.width = aspectRatio * this.webcamElement.height;
        } else  {
            this.webcamElement.height = this.webcamElement.width / aspectRatio;
        }
      }
    [...]
```

The `Webcam` constructor accepts two elements: `WebcamElement`\(`videoElement`\) and `CanvasElement`. The `adjustVideoSize()` method adjusts the video element to be proportionate to the size you specified when creating `videoElement` .

Now add the other methods to the `Webcam` class, as follows:

```text
    // src/webcam.js
    [...]
      async setup() {
        return new Promise((resolve, reject) => {
          if (navigator.mediaDevices.getUserMedia !== undefined) {
            navigator.mediaDevices.getUserMedia({
                audio: false, video: { facingMode: 'user' }
                })
                .then((mediaStream) => {
                    if ("srcObject" in this.webcamElement) {
                        this.webcamElement.srcObject = mediaStream;
                    } else {
                        // For older browsers without the srcObject.
                        this.webcamElement.src = window.URL.createObjectURL(mediaStream);
                    }
                    this.webcamElement.addEventListener(
                        'loadeddata',
                        async () => {
                            this.adjustVideoSize(
                                this.webcamElement.videoWidth,
                                this.webcamElement.videoHeight
                            );
                            resolve();
                        },
                        false
                    );
                });
          } else {
              reject();
          }
      });
      }

    [...]
```

The `setup()` function initializes the camera from the browser and assigns the video stream to your `VideoElement` in the component. That means granting access to the camera and returning the `videoStream` function to you.

Here are the methods for capturing images:

```text
    // src/webcam.js
    [...]
      _drawImage() {
        const imageWidth = this.webcamElement.videoWidth;
        const imageHeight = this.webcamElement.videoHeight;

        const context = this.canvasElement.getContext('2d');
        this.canvasElement.width = imageWidth;
        this.canvasElement.height = imageHeight;

        context.drawImage(this.webcamElement, 0, 0, imageWidth, imageHeight);
        return { imageHeight, imageWidth };
      }

      takeBlobPhoto() {
        const { imageWidth, imageHeight } = this._drawImage();
        return new Promise((resolve, reject) => {
            this.canvasElement.toBlob((blob) => {
                resolve({ blob, imageHeight, imageWidth });
            });
        });
      }

      takeBase64Photo({ type, quality } = { type: 'png', quality: 1 }) {
        const { imageHeight, imageWidth } = this._drawImage();
        const base64 = this.canvasElement.toDataURL('image/' + type, quality);
        return { base64, imageHeight, imageWidth };
      }
    }
```

The `_drawImage()` method takes the existing frame in `videoElement` when that function is called and displays the image on `canvasElement`. The `_drawImage()` method is then called in the `takeBlobPhoto()` and `takeBase64Photo()` methods to handle binary large object \(blob\) images or Base64 images, respectively.

