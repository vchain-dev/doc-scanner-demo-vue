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
---
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
---
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

When scanning of MRZ data is successfully finished, the `result` object in the callback function will have the following structure
```javascript
{
    parsedMRZ: {
        format: string, // Detected format of MRZ. For passports it is equal "TD3"
        fields: {[key:string]: string}, // Dictionary containing values of parsed fields keyed by field names
        valid: boolean, // "true" if all checksums of fields in the document are successfully verified. Otherwise "false"
        details: {...} // Object with detailed data about each field scanned and parsed
    },
    rawScannedText: string // Raw value of scanned MRZ data before parsing
}
```

<details>
<summary>Example: passport scanning result</summary>

```json
{
  "parsedMRZ": {
    "format": "TD3",
    "details": [
      {
        "label": "Document code",
        "field": "documentCode",
        "value": "P",
        "valid": true,
        "ranges": [
          {
            "line": 0,
            "start": 0,
            "end": 2,
            "raw": "P<"
          }
        ],
        "line": 0,
        "start": 0,
        "end": 1
      },
      {
        "label": "Issuing state",
        "field": "issuingState",
        "value": "KAZ",
        "valid": true,
        "ranges": [
          {
            "line": 0,
            "start": 2,
            "end": 5,
            "raw": "KAZ"
          }
        ],
        "line": 0,
        "start": 2,
        "end": 5
      },
      {
        "label": "Last name",
        "field": "lastName",
        "value": "ZENIN",
        "valid": true,
        "ranges": [
          {
            "line": 0,
            "start": 5,
            "end": 44,
            "raw": "ZENIN<<DENIS<<<<<<<<<<<<<<<<<<<<<<<<<<<"
          }
        ],
        "line": 0,
        "start": 5,
        "end": 10
      },
      {
        "label": "First name",
        "field": "firstName",
        "value": "DENIS",
        "valid": true,
        "ranges": [
          {
            "line": 0,
            "start": 5,
            "end": 44,
            "raw": "ZENIN<<DENIS<<<<<<<<<<<<<<<<<<<<<<<<<<<"
          }
        ],
        "line": 0,
        "start": 12,
        "end": 17
      },
      {
        "label": "Document number",
        "field": "documentNumber",
        "value": "999999999",
        "valid": true,
        "ranges": [
          {
            "line": 1,
            "start": 0,
            "end": 9,
            "raw": "999999999"
          }
        ],
        "line": 1,
        "start": 0,
        "end": 9
      },
      {
        "label": "Document number check digit",
        "field": "documentNumberCheckDigit",
        "value": "7",
        "valid": true,
        "ranges": [
          {
            "line": 1,
            "start": 9,
            "end": 10,
            "raw": "7"
          },
          {
            "line": 1,
            "start": 0,
            "end": 9,
            "raw": "999999999"
          }
        ],
        "line": 1,
        "start": 9,
        "end": 10
      },
      {
        "label": "Nationality",
        "field": "nationality",
        "value": "KAZ",
        "valid": true,
        "ranges": [
          {
            "line": 1,
            "start": 10,
            "end": 13,
            "raw": "KAZ"
          }
        ],
        "line": 1,
        "start": 10,
        "end": 13
      },
      {
        "label": "Birth date",
        "field": "birthDate",
        "value": "850101",
        "valid": true,
        "ranges": [
          {
            "line": 1,
            "start": 13,
            "end": 19,
            "raw": "850101"
          }
        ],
        "line": 1,
        "start": 13,
        "end": 19
      },
      {
        "label": "Birth date check digit",
        "field": "birthDateCheckDigit",
        "value": "0",
        "valid": true,
        "ranges": [
          {
            "line": 1,
            "start": 19,
            "end": 20,
            "raw": "0"
          },
          {
            "line": 1,
            "start": 13,
            "end": 19,
            "raw": "850101"
          }
        ],
        "line": 1,
        "start": 19,
        "end": 20
      },
      {
        "label": "Sex",
        "field": "sex",
        "value": "male",
        "valid": true,
        "ranges": [
          {
            "line": 1,
            "start": 20,
            "end": 21,
            "raw": "M"
          }
        ],
        "line": 1,
        "start": 20,
        "end": 21
      },
      {
        "label": "Expiration date",
        "field": "expirationDate",
        "value": "211017",
        "valid": true,
        "ranges": [
          {
            "line": 1,
            "start": 21,
            "end": 27,
            "raw": "211017"
          }
        ],
        "line": 1,
        "start": 21,
        "end": 27
      },
      {
        "label": "Expiration date check digit",
        "field": "expirationDateCheckDigit",
        "value": "8",
        "valid": true,
        "ranges": [
          {
            "line": 1,
            "start": 27,
            "end": 28,
            "raw": "8"
          },
          {
            "line": 1,
            "start": 21,
            "end": 27,
            "raw": "211017"
          }
        ],
        "line": 1,
        "start": 27,
        "end": 28
      },
      {
        "label": "Personal number",
        "field": "personalNumber",
        "value": "",
        "valid": true,
        "ranges": [
          {
            "line": 1,
            "start": 28,
            "end": 42,
            "raw": "<<<<<<<<<<<<<<"
          }
        ],
        "line": 1,
        "start": 28,
        "end": 28
      },
      {
        "label": "Personal number check digit",
        "field": "personalNumberCheckDigit",
        "value": "0",
        "valid": true,
        "ranges": [
          {
            "line": 1,
            "start": 42,
            "end": 43,
            "raw": "0"
          },
          {
            "line": 1,
            "start": 28,
            "end": 42,
            "raw": "<<<<<<<<<<<<<<"
          }
        ],
        "line": 1,
        "start": 42,
        "end": 43
      },
      {
        "label": "Composite check digit",
        "field": "compositeCheckDigit",
        "value": "4",
        "valid": true,
        "ranges": [
          {
            "line": 1,
            "start": 43,
            "end": 44,
            "raw": "4"
          },
          {
            "line": 1,
            "start": 0,
            "end": 10,
            "raw": "9999999997"
          },
          {
            "line": 1,
            "start": 13,
            "end": 20,
            "raw": "8501010"
          },
          {
            "line": 1,
            "start": 21,
            "end": 43,
            "raw": "2110178<<<<<<<<<<<<<<0"
          }
        ],
        "line": 1,
        "start": 43,
        "end": 44
      }
    ],
    "fields": {
      "documentCode": "P",
      "issuingState": "KAZ",
      "lastName": "ZENIN",
      "firstName": "DENIS",
      "documentNumber": "999999999",
      "documentNumberCheckDigit": "7",
      "nationality": "KAZ",
      "birthDate": "850101",
      "birthDateCheckDigit": "0",
      "sex": "male",
      "expirationDate": "211017",
      "expirationDateCheckDigit": "8",
      "personalNumber": "",
      "personalNumberCheckDigit": "0",
      "compositeCheckDigit": "4"
    },
    "valid": true
  },
  "rawScannedText": "P<KAZZENIN<<DENIS<<<<<<<<<<<<<<<<<<<<<<<<<<<\n9999999997KAZ8501010M2110178<<<<<<<<<<<<<<04\n"
}
```
</details>


<br/>
<br/>

**QR scanner result**

When scanning of QR code data is successfully finished, the `result` is a string with the contents of the QR code

<details>
<summary>Example: EU DCC qr code scanning result</summary>

```json
"HC1:6BFOXNUTSMAHN-HP/RG:MOQ6RFP6E8-7P3XHV5U6R5RHH//7KHROF2YKE*P5-FJLF6CB9YPD.+IKYJIGK:H3J1D1I3-*TW CXBD86U+ CU.CI8CNEDXJCCECHIDQ*C:5D%QTZA3OR3JZIM-14$4UX4795L*KDYPWGO+9AXDOHCRBR7PTMLZR3L4ZIOML65TMS96L95OD6%28%%BPHQOGOC53.DPEC5L64HX6IAS3DS2980IQODPUHLO$GAHLW 70SO:GOLIROGO3T59YLLYP-HQLTQ9R0+L69/9-3AKI63ZM-.QMU6UW6.V99Q9E$BDZIR4J3-IJ7JE7J92KBQTFTCQ0531TOYK0NN-IF9Y42J05%5X-ANKE$JDVPLZ2KD0KJWGR-M1V7LU4-YG$JPSH2W7WVIDVEWCWVE-HJ06GZP-$4-9V4$V0IU8LL-RT3$9JRIE2BV0D+GM481BAWVX7/5D*QTD3RO+VC7R*M045LA20IXG62\n"
```
</details>
<br/><br/>

---

### Pause scanning
When the result of scanning is received, you may want to suspend scanner and stop its access to the camera. You can do it by calling `pause` method of the scanner.
```javascript
scanner.pause()
```
---
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
