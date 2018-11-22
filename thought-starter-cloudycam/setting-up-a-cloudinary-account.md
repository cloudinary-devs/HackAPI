# Setting Up a Cloudinary Account

![The Cloudinary Homepage](https://github.com/cloudinary-developers/HackAPIs-HackathonGuide/tree/bbc5dae70724b2eeae77f8a25c06a051e5248f73/thought-starter-cloudycam/.gitbook/assets/cl-1.png)

To handle image uploads in this app, leverage [Cloudinary](https://cloudinary.com/). First, [create an account](https://cloudinary.com/signup) there.

![The Cloudinary Signup Page](https://res.cloudinary.com/practicaldev/image/fetch/s--osEl7sF3--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://d2mxuefqeaa7sj.cloudfront.net/s_CB1529C18383F5548EDF13217A823CEEA09EC3BDFA57AD577E213213DCE5B3F4_1536350242110_Screen%2BShot%2B2018-09-07%2Bat%2B7.56.54%2BPM.png)

## **Find Out Your Cloud Name**

Cloudinary then takes you to your **Dashboard** \(media console\), in which your cloud name is displayed under **Account Details** \(see the screenshot below\). Replace the `CLOUDINARY_CLOUD_NAME` variable in the `ClCamera` component in the previous code segments with that name.

![Finding Your Cloudinary Cloud Name](https://github.com/cloudinary-developers/HackAPIs-HackathonGuide/tree/bbc5dae70724b2eeae77f8a25c06a051e5248f73/thought-starter-cloudycam/.gitbook/assets/cl-3.png)

## **Create a Cloudinary Upload Preset**

Cloudinary Upload Presets enable you to set up the default behavior of your image uploads. That means that, instead of having to add parameters to apply to your images every time you upload one, you can define tags, transformations, and other analysis presets from your Cloudinary console. Simply specify the the preset name in your code and you’re good to go!

To create a preset, go to the [Upload Settings](https://cloudinary.com/console/settings/upload) screen and click the **Add upload preset** link:

![Adding an Upload Preset](https://github.com/cloudinary-developers/HackAPIs-HackathonGuide/tree/bbc5dae70724b2eeae77f8a25c06a051e5248f73/thought-starter-cloudycam/.gitbook/assets/cl-4.png)

The **Add upload preset** screen is then displayed:

![Adding an Upload Preset \(1\)](https://github.com/cloudinary-developers/HackAPIs-HackathonGuide/tree/bbc5dae70724b2eeae77f8a25c06a051e5248f73/thought-starter-cloudycam/.gitbook/assets/cl-5.png)

Type a name of your choice under **Preset name**, set **Mode** to **Unsigned**, and then specify the other details, as appropriate.

![Adding an Upload Preset \(2\)](https://github.com/cloudinary-developers/HackAPIs-HackathonGuide/tree/bbc5dae70724b2eeae77f8a25c06a051e5248f73/thought-starter-cloudycam/.gitbook/assets/cl-6.png)

When the `ClCamera` component uploads an image from your app, Cloudinary returns a data element that contains the information relevant to the image. That way, if you set up an Upload Preset to perform such tasks as face detection, image-color analysis, and object detection, Cloudinary returns the results to you for use as you deem appropriate. By default, Cloudinary returns the URL of your uploaded image.

