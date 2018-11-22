# Testing Your App

`ClCamera` is now ready for use. Update your `App.js` file to render the component, as follows:

```text
    // src/App.js

    // other imports
    [...]
    import ClCamera from "./components/ClCamera";

    class App extends Component {

      // other component methods
      [...]
      render() {
        return (
          <div className="App">
            <Notifier offline={this.state.offline} />
            <header className="App-header">
              <img src={logo} className="App-logo" alt="Cloudinary Logo" />
              <h1 className="App-title">CloudyCam</h1>
            </header>
            <ClCamera offline={this.state.offline} />
          </div>
        );
      }
    }

    export default App;
```

Next, ensure that your development server is running on `http://localhost:3000`. Navigate to that URL on your browser and verify that the various versions of your app are displayed:

![App Demo \(Online Mode\) \(1\)](https://github.com/cloudinary-developers/HackAPIs-HackathonGuide/tree/bbc5dae70724b2eeae77f8a25c06a051e5248f73/thought-starter-cloudycam/.gitbook/assets/cl-7.png)

![App Demo \(Online Mode\) \(2\)](https://github.com/cloudinary-developers/HackAPIs-HackathonGuide/tree/bbc5dae70724b2eeae77f8a25c06a051e5248f73/thought-starter-cloudycam/.gitbook/assets/cl-8.png)

![App Demo \(Offline Mode\) \(1\)](https://github.com/cloudinary-developers/HackAPIs-HackathonGuide/tree/bbc5dae70724b2eeae77f8a25c06a051e5248f73/thought-starter-cloudycam/.gitbook/assets/cl-9.png)

![Application Demo \(Offline Mode\) \(2\)](https://github.com/cloudinary-developers/HackAPIs-HackathonGuide/tree/bbc5dae70724b2eeae77f8a25c06a051e5248f73/thought-starter-cloudycam/.gitbook/assets/cl-10.png)

![App Demo \(Offline Mode\) \(3\)](https://github.com/cloudinary-developers/HackAPIs-HackathonGuide/tree/bbc5dae70724b2eeae77f8a25c06a051e5248f73/thought-starter-cloudycam/.gitbook/assets/cl-11.png)

