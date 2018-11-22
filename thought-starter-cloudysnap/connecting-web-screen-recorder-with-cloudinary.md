# Connecting With Cloudinary

## Understand the Connection Process

Note this code snippet in the `index.html` file:

```javascript
[...]        
    let opts = { mimeType: 'video/webm; codecs=vp9' };
    let rec = new MediaRecorder(videoElement.srcObject, opts);
    let blobs = [];

    rec.ondataavailable = (e) => (e.data && e.data.size > 0) ? blobs.push(e.data) : null;
    rec.onstop = () => {
        //  get the image blob
        let finalBlob = new Blob(blobs, { type: 'video/mp4' });
        // create form data for submission         
        let formData = new FormData();
        formData.append('upload_preset', 'CLOUDINARY_UPLOAD_PRESET');
        formData.append('api_key', "CLOUDINARY_API_KEY");
        formData.append('file', finalBlob);
        var xhr = new XMLHttpRequest();
        xhr.open("POST", 'https://api.cloudinary.com/v1_1/CLOUDINARY_CLOUD_NAME/auto/upload');
        xhr.onreadystatechange = function () {
            if (this.readyState == XMLHttpRequest.DONE && this.status == 200) {
                console.log(this.status);
                alert("Video uploaded to your cloudinary media library");
            }
        }
        xhr.send(formData);
    }
    rec.start(100);
    setTimeout(() => rec.stop(), 2000)
[...]
```

Lines `2-26` implement the core upload capability by first creating a media recording with `videoElement.srcObject`. Below that are the definitions of the properties for the `rec` variable, which instructs the recorder on how to act in various situations.

The `rec.onstop` property is of particular interest. When a recording is complete, the recorder obtains the recorded data and sends them as a single blob to Cloudinary with Cloudinary's Upload Presets.

![Cloudinary Home Page](../.gitbook/assets/screenshot-2018-11-15-at-5.22.03-am.png)

To further handle the videos you have uploaded, leverage [Cloudinary](https://cloudinary.com/). First, [create an account](https://cloudinary.com/signup) there.

![The Cloudinary Signup Page](../.gitbook/assets/screenshot-2018-11-15-at-5.22.28-am.png)

## **Find Out Your Cloud Name**  <a id="find-out-your-cloud-name"></a>

Cloudinary then takes you to your **Dashboard** \(media console\), in which your cloud name is displayed under **Account Details** \(see the screenshot below\). Replace the `CLOUDINARY_CLOUD_NAME` and `CLOUDINARY_API_KEY` variable in the `index.html` file in the previous code segments with that name.

![Finding Your Cloudinary Cloud Name and API Key](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LQxqjHiNgHCfPtd6mkQ%2F-LRAT_zQ5FlwNEB6AZHt%2F-LRAVWKufBuF9k3qEShm%2Fcl-3.png?alt=media&token=3437d2b1-c229-431b-9477-cea0854d1c01)

## **Create a Cloudinary Upload Preset**  <a id="create-a-cloudinary-upload-preset"></a>

Cloudinary Upload Presets enable you to set up the default behavior of your image uploads. That means that, instead of having to add parameters to apply to your images every time you upload one, you can define tags, transformations, and other analysis presets from your Cloudinary console. Simply specify the preset name in your code and you’re good to go!

To create a preset, go to the [Upload Settings](https://cloudinary.com/console/settings/upload) screen and click the **Add upload preset** link:

![Adding an Upload Preset \(1\)](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LQxqjHiNgHCfPtd6mkQ%2F-LRAT_zQ5FlwNEB6AZHt%2F-LRAVivHbtupVFyl_l9-%2Fcl-4.png?alt=media&token=78f120d5-f819-4c39-b3a5-ec392755744e)

The **Add upload preset** screen is then displayed:

![Adding an Upload Preset \(2\)](../.gitbook/assets/screenshot-2018-11-15-at-5.27.17-am.png)

Type a name of your choice under **Preset name**, set **Mode** to **Unsigned**, and then specify the other details, as appropriate.

Now go back to the `index.html` file and replace `CLOUDINARY_UPLOAD_PRESET` with the name of your preset.

\*\*\*\*

## Test Uploads to Cloudinary

Now that you've added all your Cloudinary details to the `index.html` file, go to your Chrome browser and run your demo app. Afterwards, you'll see this display:

![Screen Recorder Connected to Cloudinary for Uploads](../.gitbook/assets/screenshot-2018-11-15-at-5.37.21-am.png)

