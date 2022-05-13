# @zamna/doc-scanner

## Running example application
### Install dependencies
```
npm install
```

### Run dev server
```
npm run serve
```

## Working with `@zamna/doc-scanner`
### Install
```
npm i @zamna/doc-scanner
```
### Create scanner
Depending on your task at hand you can create a scanner that scans QR-codes or MRZ data of the documents
```javascript
import { DocScannerQR, DocScannerMRZ } from "@zamna/doc-scanner";

// First we create a config object containing license key
const scannerConfig = {
    licenseKey: "<LICENSE_KEY_PROVIDED_BY_ZAMNA>"
};

// This creates an instance of QR codes scanner
const qrScanner = new DocScannerQR(scannerConfig);

// This creates an instance of MRZ scanner
const mrzScanner = new DocScannerMRZ(scannerConfig);
```

### Initialize and mount scanner
After we created and instance of a scanner, we need to initialize the scanner and mount it to some DOM object.

*NOTE: both methods `init` and `mount` are asynchronous.*

```javascript
const someDOMElement = document.getElementById("some-dom-element-id")

try {
    await scanner.init();

    /*
    When we call `mount` we provide a reference to a DOM  element and a callback function that will be called when the scanner gets either successful result of a scan or fails in the process.
    */
    await scanner.mount(someDOMElement, (result, error) => {});
  } catch (err) {
    // handling errors
}
```

**MRZ scanner result**



### Pause scanning
When the result of scanning is received, you may want to suspend scanner and stop its access to the camera. You can do it by calling `pause` method of the scanner.
```javascript
scanner.pause()
```

### Resume scanning
After pausing you can resume scanning with `resume` method.

*NOTE: `resume` method is asynchronous*
```javascript
await scanner.resume()
```

### Destroy
If you don't need a scanner instance any more, you may want to destroy it.

```javascript
scanner.destroy()
```
