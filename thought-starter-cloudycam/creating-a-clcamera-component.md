# Creating and Styling a ClCamera Component

To put `Webcam` to use, create a Cloudinary Camera component called `ClCamera`. First, create a new `ClCamera` folder in your `src/components`folder:

```text
    mkdir ClCamera
    cd ClCamera
    touch index.js ClCamera.css # on Windows, run the command
    # copy NUL index.js
    # copy NUL ClCamera.css
```

Install `axios`, which enables you to make HTTP requests in the app:

```text
    yarn add axios # or npm install axios
```

Afterwards, edit the `ClCamera/index.js` file to read like this:

```text
    // src/components/ClCamera.js

    import React, { Component } from 'react';
    import { Webcam } from '../../webcam';
    import './ClCamera.css';
    import axios from 'axios';

    class ClCamera extends Component {
      constructor() {
        super();
        this.webcam = null;
        this.state = {
          capturedImage: null,
          captured: false,
          uploading: false
        }
      }

      componentDidMount() {
        // initialize the camera
        this.canvasElement = document.createElement('canvas');
        this.webcam = new Webcam(
            document.getElementById('webcam'),
            this.canvasElement
        );
        this.webcam.setup().catch(() => {
            alert('Error getting access to your camera');
        });
      }

      componentDidUpdate(prevProps) {
        if (!this.props.offline && (prevProps.offline === true)) {
          // if its online
          this.batchUploads();
        }
      }

      render() {
            const imageDisplay = this.state.capturedImage ?
                <img src={this.state.capturedImage} alt="captured" width="350" />
                :
                <span />;

            const buttons = this.state.captured ?
                <div>
                    <button className="deleteButton" onClick={this.discardImage} > Delete Photo </button>
                    <button className="captureButton" onClick={this.uploadImage} > Upload Photo </button>
                </div> :
                <button className="captureButton" onClick={this.captureImage} > Take Picture </button>

            const uploading = this.state.uploading ?
                <div><p> Uploading Image, please wait ... </p></div>
                :
                <span />

            return (
                <div>
                    {uploading}
                    <video autoPlay playsInline muted id="webcam" width="100%" height="200" />
                    <br />
                    <div className="imageCanvas">
                        {imageDisplay}
                    </div>
                    {buttons}
                </div>
            )
        }

    [...]
```

The `ClCamera` component contains three states:

* The `capturedImage` state, which holds a Base64 version of an image.
* A boolean `captured` state, which specifies whether an image has been captured.
* An `uploading` state, which specifies if an image is being uploaded to Cloudinary.

When the `ClCamera` component is mounted, the `componentDidMount()`function creates a `canvas` element and a `Webcam` object, passing the `videoElement` and `canvasElement` elements as parameters. Afterwards, you initialize the camera feed.

When the app goes from offline to online mode, the `componentDidUpdate`method calls the `batchUpload()` method for uploading the images that were saved in the browser’s cache while the app was offline.

Here are the other methods that perform tasks in your app:

* When the `captureImage()` function is clicked, the `takeBase64Photo()` method is called to capture the image.
* The Base64 image is stored in the `capturedImage` state of `ClCamera` with the `captured` state of the component set to `true`.
* Two buttons are displayed, which trigger the `discardImage` method and the `uploadImage` method, prompting you to either discard or upload the image, respectively. The `discardImage()` method discards the image from the state of `ClCamera` and then sets the `captured` state to `false`.

```text
    // src/components/ClCamera/index.js
    [...]
        captureImage = async () => {
            const capturedData = this.webcam.takeBase64Photo({ type: 'jpeg', quality: 0.8 });
            this.setState({
                captured: true,
                capturedImage: capturedData.base64
            });
        }

        discardImage = () => {
            this.setState({
                captured: false,
                capturedImage: null
            })
        }

    [...]
```

The `uploadImage` function checks your connection status and then does the following:

* If the connection is offline, `uploadImage` creates a new unique string with the prefix `cloudy_pwa_` and then stores your Base64 image in the component’s `this.state.capturedImage` state in the browser’s `localStorage`. Finally, `uploadImage` calls the `discardImage()`method.
* If the connection is online, `uploadImage` makes a `POST` request to upload your Base64 image along with a Cloudinary Preset as a parameter.

> **Note:** Cloudinary Upload Presets are described in depth later in this tutorial.

```text
    // src/components/ClCamera/index.js
    [...]

        uploadImage = () => {
            if (this.props.offline) {
                console.log("you're using in offline mode sha");
                // create a random string with a prefix
                const prefix = 'cloudy_pwa_';
                // create random string
                const rs = Math.random().toString(36).substr(2, 5);
                localStorage.setItem(`${prefix}${rs}`, this.state.capturedImage);
                alert('Image saved locally, it will be uploaded to your Cloudinary media library once internet connection is detected');
                this.discardImage();
                // save image to local storage
            } else {
                this.setState({ 'uploading': true });
                axios.post(
                    `https://api.cloudinary.com/v1_1/CLOUDINARY_CLOUD_NAME/image/upload`,
                    {
                        file: this.state.capturedImage,
                        upload_preset: 'CLOUDINARY_CLOUD_PRESET'
                    }
                ).then((data) => this.checkUploadStatus(data)).catch((error) => {
                    alert('Sorry, we encountered an error uploading your image');
                    this.setState({ 'uploading': false });
                });
            }
        }

    [...]
```

> **Note:** Be sure to replace `CLOUDINARY_CLOUD_NAME` and `CLOUDINARY_UPLOAD_PRESET` with the actual values in your setup. See the next section for how to obtain those values.

When `ClCamera` detects that your Internet connection has been restored, the `batchUploads` method is called, which searches `localStorage` for any previously stored images with the `findLocalItems` method. If no images are found, the function exits. Otherwise, the images are uploaded to the Cloudinary media library through a `POST` request to the upload endpoint with the image and preset as parameters. The `checkUploadStatus` method accepts the data response from Cloudinary’s API and then checks if the upload succeeded. In case of an error, `checkUploadStatus` displays a message to the effect that the image remains in `localStorage` for the next batch upload.

```text
        findLocalItems = (query) => {
            let i;
            let results = [];
            for (i in localStorage) {
                if (localStorage.hasOwnProperty(i)) {
                    if (i.match(query) || (!query && typeof i === 'string')) {
                        const value = localStorage.getItem(i);
                        results.push({ key: i, val: value });
                    }
                }
            }
            return results;
        }

        checkUploadStatus = (data) => {
            this.setState({ 'uploading': false });
            if (data.status === 200) {
                alert('Image Uploaded to Cloudinary Media Library');
                this.discardImage();
            } else {
                alert('Sorry, we encountered an error uploading your image');
            }
        }

        batchUploads = () => {
            // this is where all the images saved can be uploaded as batch uploads
            const images = this.findLocalItems(/^cloudy_pwa_/);
            let error = false;
            if (images.length > 0) {
                this.setState({ 'uploading': true });
                for (let i = 0; i < images.length; i++) {
                    // upload
                    axios.post(
                        `https://api.cloudinary.com/v1_1/CLOUDINARY_CLOUD_NAME/image/upload`,
                        {
                            file: images[i].val,
                            upload_preset: 'CLOUDINARY_CLOUD_PRESET'
                        }

                    ).then(
                      (data) => this.checkUploadStatus(data)
                    ).catch((error) => {
                        error = true;
                    })
                }
                this.setState({ 'uploading': false });
                if (!error) {
                    alert("All saved images have been uploaded to your Cloudinary Media Library");
                }
            }
        }
    }

    export default ClCamera;
```

> **Note:** Again, replace `CLOUDINARY_CLOUD_NAME` and `CLOUDINARY_UPLOAD_PRESET` with the actual values in your setup. See the next section for how to obtain those values.

The `ClCamera` component contains these style properties:

```text
    /* src/components/ClCamera/ClCamera.css */

    .captureButton{
      margin-top: 20px;
      padding: 10px;
      padding-left: 20px;
      padding-right: 20px;
      background-color: #0066B2;
      color: white;
      border-radius: 5px;
    }

    .deleteButton{
      margin-top: 20px;
      padding: 10px;
      padding-left: 20px;
      padding-right: 20px;
      background-color: #D77623;
      color: white;
      border-radius: 5px;
    }

    .imageCanvas{
      margin-top: 20px;
      width: 100%;
      height: 200px;
      display: flex;
      justify-content: center;
    }
```

Feel free to edit the properties as you see fit.

