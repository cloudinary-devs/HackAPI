# Using the Notifier Component

To use the `Notifier` component, edit the `src/App.js` file to read like this:

```text
    // src/App.js

    import React, { Component } from 'react';
    import logo from './logo.png';
    import './App.css';
    import Notifier from './components/Notifier';

    class App extends Component {
      constructor() {
        super();
        this.state = {
          offline: false
        }
      }

      componentDidMount() {
        window.addEventListener('online', () => {
          this.setState({ offline: false });
        });

        window.addEventListener('offline', () => {
          this.setState({ offline: true });
        });
      }

      componentDidUpdate() {
        let offlineStatus = !navigator.onLine;
        if (this.state.offline !== offlineStatus) {
          this.setState({ offline: offlineStatus });
        }
      }

      render() {
        return (
          <div className="App">
            <Notifier offline={this.state.offline} />
            <header className="App-header">
              <img src={logo} className="App-logo" alt="Cloudinary Logo" />
              <h1 className="App-title">CloudyCam</h1>
            </header>
          </div>
        );
      }
    }

    export default App;
```

The `App.js` component has one state, `offline`, which specifies whether the app is in offline mode. By default, the state is `false`. When `App.js`is mounted, the `componentDidMount` function, which is executed when the app is loaded, listens for the online/offline event and updates the `App.js`state accordingly.

The `render` function defines the layout of the app and the `Notifier`component, passing the offline state as a property to `Notifier` for display.

Fetch the Cloudinary logo from the [Cloudinary site](https://res.cloudinary.com/clo) and save it in your `src` directory as `logo.png`.

Now you might wonder how all that is displayed in the app. In the `src/index.js` file, the `App` component is rendered on a `<div>` tag with the ID `root`, as follows:

```text
    // src/index.js
    import React from 'react';
    import ReactDOM from 'react-dom';
    import './index.css';
    import App from './App';
    import registerServiceWorker from './registerServiceWorker';

    ReactDOM.render(<App />, document.getElementById('root'));
    registerServiceWorker();
```

To view your app, first run this command on your development server:

```text
    yarn start
```

Afterwards, go to `http://localhost:3000` on your browser to display the app. Toggle your Internet connection and you will see one of the two versions on display, depending on whether you are online or offline. See below.

![Notifier component when the app is online](https://github.com/cloudinary-developers/HackAPIs-HackathonGuide/tree/bbc5dae70724b2eeae77f8a25c06a051e5248f73/thought-starter-cloudycam/.gitbook/assets/online-cloudyshot.png)

![Notifier component when the app is offline](https://github.com/cloudinary-developers/HackAPIs-HackathonGuide/tree/bbc5dae70724b2eeae77f8a25c06a051e5248f73/thought-starter-cloudycam/.gitbook/assets/offline.png)

