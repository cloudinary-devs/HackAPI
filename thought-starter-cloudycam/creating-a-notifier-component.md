# Creating a Notifier Component

To enable offline use and access, you need a `Notifier` component that identifies the mode that is interacting with the app.

First, create a `components` folder in the `src` directory to hold the app's components:

```text
    mkdir components
```

Create a `Notifier` folder in your `src/components` directory:

```text
    mkdir Notifier
    cd Notifier
    touch index.js Notifier.css # on Windows, run the following instead
    # copy NUL index.js
    # copy NUL Notifier.css
```

Next, install a package called `classnames` for displaying the colors for the various modes, that is, dynamically rendering the classes:

```text
    yarn add classnames # or npm install classnames
```

Afterwards, edit your `Notifier/index.js` file to read like this:

```text
    // src/components/Notifier/index.js
    import React, { Component } from "react";
    import "./Notifier.css";
    import classnames from 'classnames';

    class Notifier extends Component {
      render() {
        const notifyclass = classnames('notify', {
          danger: this.props.offline
        });
        const message = this.props.offline ?
      `CloudyCam is offline! Your images will be saved now and then uploaded to your Cloudinary Media Library once your Internet connection is back up.`
      :
      `Take a picture and it will be uploaded to your Cloudinary Media Library.`;
        return (
            <div className={notifyclass}>
                <p>
                    <em>{message}</em>
                </p>
            </div>
        );
      }
    }

    export default Notifier;
```

Here, check the value of the `offline` property that is passed when `Notifier` is called. If `offline` is `true`, the app is in offline mode and the class and message are displayed accordingly.

Edit your `Notifier/Notifier.css` file to read like this:

```text
    /* src/components/Notifier/Notifier.css */

    .notify{
        background-color: #0066B2;
        padding: 20px;
        text-align: center;
        color: white;
        margin-bottom: 20px;
    }

    .danger{
        background-color: #D77623;
    }
```

