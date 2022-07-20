<script setup>
import { ref, onMounted } from "vue";
import { DocScannerMRZ } from "@zamna/doc-scanner";

const docScanner = ref(null);
const scannerContainer = ref(null);
const scanResult = ref(null);

onMounted(async () => {
  if (!scannerContainer.value) {
    throw new Error("We need a DOM element to mount a scanner.");
  }

  const scannerConfig = {
    licenseKey:
      // Below is a temporary Zamna dev key. When deploying the application to your subdomain – change this key to the one provided to you by Zamna
      "ATjR2jO+EutFHOV9mTZ7YpQidNFzDLM483sDl70k98s8S1Deh0WmVRBzaFy+ceFc9n7lGIR7bFvuac/9q3iJF2FXkQyUMHYKoHkvRdZ2RCpxZP+eZWDS1FtixLAlXkNoNDpU160c7V4kOjTAQG0TE3lomh32WH98k1j1pzJzHISHanij8Ec+HiVHM5xxTzigyEDYYah8eNTST6MFmh0IB9ZWYqa3cfKwPWSOj2dawhLmcZRHWg2pGCB1vQ77a4aP/2/4ZlQAneuVZZJvkk5x5BxabX0YSuoAu29W8VVRW5KUHfg1sEyaQ7ZJOQ1/d85GsHF04wdpc6Nde37ajGwwMCQ47PeqZ1lfRW902i1LODWbdq1ZnV5fI7N3ddd6btvmsn/o1bUWFRdDfv6GuRLUDHIITuhMcpfC3hsqz6xU0++EFOQISROkJ9NWKriXefHSqlm9/3RTqeFOCSI7gDIwiyt8H2BVU1oir2DIoNxElmwxN4G8P83VCmp2KiuJ6bYWrtGpjFBbpPAnhd2gJSXQgEoVOfTW/MqN4/Hj8NXfDj//SRW1Kh3Q3C9DY2Ia/alaYYmSEuvHk0G9Zip0LOnFSkmRkOzwqaWkxhSKGvWFKPdBd4tHhGP6FDDTxa7qnX+MkDNh6vOq8kcHOzz/7a0EcbaRdv/nK3TG+aGsyD/PaBoImzTjsZnkvbbCT5cp+t1M7mNQKMjAVTHPumveK1KSlvXai8z+uopcpZz2ewVGzQY1Y5xoFnYCb1st6kvACVSGrbCB+zTR9ZGnZ5ID+ys/ef0J7f6MVHpb2vuW3QaIOHsY/L0Jgyll3+Zjeka/hl5m4IvAJSBjlGc+u2thNpZprnvJxC5WTIliVxPMXxPVii1j4TsEn8YiNEMSekJW55lSQz+fpwYz+VmpgdOEkD7a+l6HDyvCil7+Wv22Xk41YYeCeqTrLgaP3Ol1+OymR15ZjbVKKFHCurdrVbEW/LLhswRMkspeTd3YQnAQWCEuMNuVjbaZIny75mih2PWEsYoxX6fFvK9+YzXJTLxO2ZUNPrApye148A85OJj34u7AoQX9x2jxSsfVZ3+s5O5cqtK2tMnSRL70yDtR80LVYCjl7SIKmDOgh691kA5UCUYcMJ2asXvS3O6z8nwWV87or4x9n9ozMmUruQ3qAd2yVw==",
  };

  // Next line creates the instance of the scanner for QR codes
  // docScanner.value = new DocScannerQR(scannerConfig);

  // Next line creates the instance of the scanner for MRZ of passport
  docScanner.value = new DocScannerMRZ(scannerConfig);

  function onScanResult(result, error) {
    if (error) {
      throw error;
    }

    scanResult.value.value = JSON.stringify(result, undefined, 2);
  }

  try {
    const scanner = docScanner.value;
    await scanner.init();

    await scanner.mount(scannerContainer.value, onScanResult);
  } catch (err) {
    switch (err.name) {
      case "NotAllowedError":
        console.error("User did not allow usage of the camera");
        break;
      default:
        console.log(err);
    }
  }
});
</script>

<template>
  <div class="doc-scanner-container" ref="scannerContainer"></div>
  <div class="controls">
    <button @click="docScanner.pause">pause</button>
    <button @click="docScanner.resume">resume</button>
    <button @click="docScanner.destroy">destroy</button>
  </div>
  <textarea class="doc-scanner-result" ref="scanResult"></textarea>
</template>

<script>
export default {
  name: "DocScanner",
};
</script>

<style scoped>
.doc-scanner-container {
  font-weight: bold;
  width: 100%;
  max-width: 600px;
  height: 400px;
  background-color: orange;
}

.doc-scanner-result {
  display: block;
  padding: 20px;
  width: 100%;
  max-width: 600px;
  height: 500px;
  box-sizing: border-box;
  white-space: pre-wrap;
}

.controls {
  width: 100%;
  max-width: 600px;
  box-sizing: border-box;
}
</style>
