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
      // Below is Zamna dev key. When deploying the application to your subdomain – change this key to the one provided to you by Zamna
       "AZ8BOw+iLjqmMAbzbwY90r9Cv7cjK9PQA3ATUnQSvgbJf7UeL3F+6pJGaor+dBLWM3IK2gdgnk8DZo1QV0dvCGhybEcCPPA8qmnFnvd7hTuILdYQemcdTLxB8+MaZKy8ZCP0asczB1hgHJCJLRhAB7JjrX1Dc8yxg2H4BS5+LI7Lez8JU4gJJrbx/oJ82tKYdaFG2wMJ7y1Ti+amLQa/tH3jQnFJgaItUZkVHqGlc7gHfzLTPIjPwUxOMkqYbLecR02s/xQ/m/WKMSmiebwyvam3LOmXUw78KMqA/CBC8ETy87BSNXpSxGIPCimaHUMjrLzLwQufadpgpb7WUYG/QJCK5TCBcEE2fnyaC6XTydPj0IhtkOZtfgl734j1qygnmUvrdT5RuDcW2asorLOgCG0nxhmRg3VYghjhqvyScmWeJCqUcQU3TEgLGNlnRwF0+Qg5Oxr36PDSN0Y5zbaIbfcscjXGcZRkS5dnonZBzRuWtENloBZwH6N8kAVGkIK0TDtpA9AsFjnmddwLkYk+zVc8zETUznH8Z0kF8AdQZvp2HZZKIVqyZbqZfB/g0gbohBMo3xkPsvyRsthZchs7jFweW/a+FN45VxUNt3SIjd9vnwKf92/9iPLW4HBd3OajGMDueou8m43TaF2b1p7yH5SFsZTO3Rj+BOkwColCCmGnbO0CC8i0niRhqUX0EOo1UzdlGdT5DbgzDL3p1v48ALbQuCrTt3t3fFYeRwxaV8TKpp68wZLvaNYU+h2vLe4B1gPnlD31Z1rKOZA7gUzmotdlUm3hWef6sHIS5n6v2Czx/RXwYfwIC3laTqAkpoVTE/BTL9gAKrz51fIW"
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
