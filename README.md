
# saaf-rn-web-server

A cross platform component for serving static assets with React Native.

## Getting started

`$ npm install saaf-rn-web-server --save`

### Installation

From react-native 0.60 autolinking will take care of the link step but don't forget to run pod install

`$ react-native link saaf-rn-web-server`

## Usage

Declare the `StaticServer` with a port or use the default `0` to pick a random available port.

```javascript
import StaticServer from 'saaf-rn-web-server';

let server = new StaticServer(8080);

// Start the server
server.start().then((url) => {
  console.log("Serving at URL", url);
});

// Stop the server
server.stop();

// Check if native server running
const isRunning = await server.isRunning()
// isRunning - true/false
```

`StaticServer` serves from the document directory (default) or takes an optional absolute path to serve from.

For instance, using [react-native-fs](https://github.com/johanneslumpe/react-native-fs) you can get the document directory and specify a directory from there.

#### Default (document directory)

```javascript
import StaticServer from 'saaf-rn-web-server';
import RNFS from 'react-native-fs';

// create a path you want to write to
let path = RNFS.DocumentDirectoryPath + '/www';

let server = new StaticServer(8080, path);
```

#### Custom folder (iOS)

##### Create the folder for static files

Create a folder in your project's top-level directory (usually next to your node_modules and index.js file), and put the files you want to access over http in there.

##### Add folder (static files) to XCode

This folder **must be added to XCode** so it gets bundled with the app.

In XCode, `Project Navigator` right click in the folder project → `Add files to "<project>"` → Select the static folder **and clic options (Uncheck copy items if needed, Create folder references)** so don't duplicate files → Clic Add.

When the app gets bundled, this folder will be next to the compiled app, so using `MainBundlePath` property from `react-native-fs` you can access to the directory.

```javascript
import StaticServer from 'saaf-rn-web-server';
import RNFS from 'react-native-fs';

// path where files will be served from (index.html here)
let path = RNFS.MainBundlePath + '/www';

let server = new StaticServer(8080, path);
```

If the server should only be accessible from within the app, set `localOnly` to `true`

```javascript
import StaticServer from 'saaf-rn-web-server';

// Just set options with defaults
let server = new StaticServer({localOnly : true });
// Or also valid are:
let server = new StaticServer(8080, {localOnly : true });
let server = new StaticServer(8080, path, {localOnly : true });

```

If the server should not pause when the app is in the background, set `keepAlive` to `true`

```javascript
let server = new StaticServer({keepAlive : true });
```

Passing `0` as the port number will cause a random port to be assigned every time the server starts.
It will reset to a new random port each time the server unpauses, so this should only be used with `keepAlive`.

```javascript
let server = new StaticServer(0, {keepAlive : true });
```

## Credits

* iOS server: [GCDWebServer](https://github.com/swisspol/GCDWebServer)
* Android server: [NanoHttpd Webserver](https://github.com/NanoHttpd/nanohttpd)

Thanks to [CorHttpd](https://github.com/floatinghotpot/cordova-httpd) and [react-native-httpserver](https://gitlab.com/base.io/react-native-httpserver#README) for the basis of this library.
