[npm]: https://img.shields.io/npm/v/@oliverphaser/nativescript-printer.svg?color=949393
[apple]: https://img.shields.io/badge/apple-%E2%9C%93-949393.svg?logo=apple&logoColor=white
[android]: https://img.shields.io/badge/android-%E2%9C%93-949393.svg?logo=android&logoColor=white
[support]: https://img.shields.io/static/v1.svg?logo=paypal&label=Support&message=oliverphaser&style=for-the-badge&color=0c67b5&labelColor=afb0b9

[![npm]](https://www.npmjs.com/package/@oliverphaser/nativescript-printer)
![apple]
![android]
<br/>
[![support]](https://paypal.me/oliverphaser)

> Think about the environment before printing!

# NativeScript Printer plugin

A [NativeScript](https://nativescript.org/) module for sending an image, PDF or the screen contents to a physical printer.

## NativeScript 8

This will work only on NativeScript 8.

## Installation
From the command prompt go to your app's root folder and execute:

```
ns plugin add @oliverphaser/nativescript-printer
```

## Demo app
Want to dive in quickly? Check out [the demo](https://github.com/oliverphaser/nativescript-printer/tree/master/demo)! Otherwise, continue reading.

Run the demo app from the root of the project: `npm run demo.ios` or `npm run demo.android`.

### Android screenshots
<img src="https://raw.githubusercontent.com/EddyVerbruggen/nativescript-printer/master/screenshots/android/android-select-printer.png" width="375px"/>&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://raw.githubusercontent.com/EddyVerbruggen/nativescript-printer/master/screenshots/android/android-printer-options.png" width="375px"/>

### iOS screenshots
<img src="https://raw.githubusercontent.com/EddyVerbruggen/nativescript-printer/master/screenshots/ios/ios-select-printer.png" width="375px"/>&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://raw.githubusercontent.com/EddyVerbruggen/nativescript-printer/master/screenshots/ios/ios-printing-in-progress.png" width="375px"/>

## API

### `isSupported`
Not all devices support printing, so it makes sense to check the device capabilties beforehand.

##### TypeScript
```typescript
// require the plugin
import { Printer } from "@oliverphaser/nativescript-printer";

// instantiate the plugin
let printer = new Printer();

printer.isSupported().then((supported) => {
  alert(supported ? "Yep!" : "Nope :'(");
}, (error) => {
  alert("Error: " + error);
});
```

### `printImage`

##### TypeScript
```typescript
// let's load an image that we can print. In this case from a local folder.
let fs = require("file-system");
let appPath = fs.knownFolders.currentApp().path;
let imgPath = appPath + "/res/printer.png";
let imgSrc = new ImageSource();
imgSrc.loadFromFile(imgPath);

printer.printImage({
  imageSrc: imgSrc
}).then((success) => {
  alert(success ? "Printed!" : "Not printed");
}, (error) => {
  alert("Error: " + error);
});
```

### `printPDF`

##### TypeScript
```typescript
import { knownFolders } from "@nativescript/core";

printer.printPDF({
  pdfPath: knownFolders.currentApp().path + "/pdf-test.pdf"
}).then((success) => {
  alert(success ? "Printed!" : "Not printed");
}, (error) => {
  alert("Error: " + error);
});
```

### `printScreen`
Prints the current screen contents. Anything off screen will not be printed.

##### TypeScript
```typescript
printer.printScreen().then((success) => {
  alert(success ? "Printed!" : "Not printed");
}, (error) => {
  alert("Error: " + error);
});
```

You can also print a specific portion of the screen, which also enables you to print
views that are larger than the viewport. This is an example of a non-Angular NativeScript app:

**Note**
If the view is either of the following depending on the size of it's contents it would break into multiple pages.

`Label | TextView | HtmlView | WebView`

```xml
  <StackLayout id="printMe">
    <Label text="Print me :)" />
  </StackLayout>

  <Button text="Print" tap="{{ print }}" />
```

```js
  public print(args) {
    printer.printScreen({
      view: args.object.page.getViewById("printMe")
    });
  }
```
