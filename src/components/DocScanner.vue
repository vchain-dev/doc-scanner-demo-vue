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
       "AXSRTwO+Iof8PZzEuCyKCQA2MPRnE2eCZQE1Wf4IFGsrefdFmWkomytu05qLJCtmjyGeP7Uy3ZuzdnBxVHhuDugq4a33MhVxhVg+/hlmuG6PCICWDXsnVdEwyFgxUcv4gn1UZtEq4V8mHHaYVRs1A+ApHdqbRpQvL33wmMF3+pJy2nVJH6nUmY5teCbUK4bQnJQF1YTZ+vzOTR7+6eWAPu8grOefTJmKfyqO8jJDT0snkpqM8U5UH1lmQfg721RyZOlAOEKR0l0Gm7uK+NNdjtxlZX1XSTmYcRR89W1gBaa/pY0sW9r37Lpreza1Ej9p0nmoP1R3cnDCZH7Lxj/nRXexQtajaeST4xOfQYypBQxRQ8P7SKNZo7mpVvuRyxTc85TA8ik/ltUE0KLt3XWmPLbIgokaRPCQbyo9Ez6PWGzvrj5JjQ3gGSuzjAHuSKTR5dQ1mG/Fo0QP8cjI5yI2MUz35zlWUscEBJsnbwEcBRynn+mZ2LbBkfzFwIZU1P52RM0pYidWXEyXqeO4WqHIfjrNJnmnl/c+JJnjGkit1HodJEgRPpNtC6zta4c+3YFNzkSIKZwINQ1u/sFagcMpI5iZuRyR6MTSNPJm2ox/Z7PO4XGLa0ZVedLgUKCaPtZH2RCIipIlmbj4d6sJ5cLBjx2VpcI7dQ3AB3CVOq8ir4yjdS7PlVz3egRxmLF+VfU4GYkCIRLR0v2dy3yCVM6ev+qOED8AM4PykIkUheknl0HqQUYDlYLCm8HmpWYgjeJuiCL+Y5IkWzYZsUCF4+EgP8D9HrzWhltHMlnQOmnwHUO3dW0eTXO7xuypd0BdbVd1lJxkZomk94R54DyVE1OOGQ=="
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
    const scanner = docScanner.value
    await scanner.init();
    await scanner.mount(scannerContainer.value, onScanResult);
  } catch (err) {
    console.log(err);
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
