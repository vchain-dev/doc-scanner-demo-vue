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
      "AYliMQtGKFSpLSlq8B+0zSgdWADhIhZfLkwjjaJyIM/KHX4FfQsGzXB7tFO1d6QcszmoucQUJzwiavrOhGgUiTVo3PJwVMfI/i53uFZb/OIVO6VbTT7d/ysaOw8UMfd7Xnu1XGcpuiAaaSJgRm+1lY5sfujxUMlDpAc07s1l2la0G4EkTU9jMfQQ+EyLSLXMyHK3Y4BsWfBOenCLI1l7Z2lX620EXKNqj3Ey2VFzweY0fQzqInjY4PZecEwFQQOxQSqJt3VKqimkcIzaYUx1FZ5bitgcWvHemUr1yu9errMyM/ZrUHuy75tIYBKoagHyd29NEZp8UKjGQ1wwTjPRYEhx3H7GeWOARGh+3c51skUWF2iblm9v9ZximexNTkr9mExRaNNzBADTUR5sqHy9OiMwhmFFdKA7EmldC/xtmAbycNWVhD9wFhoZMUQtcq7nCx+mUsl5Vm1uWpGHKmWBB/jWdqfkpxxWiAe/yOPhFjhT3voxcbN3IZx3RRF9OPY1x6bbLrBxOpfVUl/ekUNHs1IgUv4lAL/CiVY0UvOJjbBzLQWMkZF1QWYYQ2IZo+IcYiIt42FmQx4Gy3zJqSpDEU0j+MSKsBlT+ctqLxBo2d0YiBPcSFzJwBjdjdRL3xAxNz+/JQ8fnM12zZ1Gq4kkdjRJ9U+S74qYAmfLWSiXJO3scYzMsKEiKD8pFxNzrFkEP4BbexBRVEarNXdEhNSw3amTY58yM/LVmQMrRxJD7jgZKFUIMET3kGazyCRuQfO2WHbiMyUZeZIw88P4kKrIu2mawtLhWqp5D4Xn2ik7XIf6myRP4ngCi5hgtOREi72eKKv8oPA5F7EGUxBA0HHtNBRMjC7kL2a4ulCiP09jmHDdQp6dGV04PPCftRyYYrH7poQ46MJd9TsX7IuKS+4CDim4QJ1MqYxSItqFIlnGiDSZRfz8PGBftKxVc0kMxvsBUwkKoTyv/NTK66k5BaE4xsbGqSSSBqiMbQIuK3AI0P9vHSYB5vjpmAqkH5XOJNCjbpMh8icgyI9+tagmqc8HU6lZS332bUlEWQ5Ie6Gw1l/FNNqLTJZiOTh+in/1UPKOllZLz71DzYzFV5cSp0H/2gO+Z1EW4sjnPVYsPg0vd7ZMqh5F9RLcUAPNQowssp3x",
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

function clearResult() {
  scanResult.value.value = ""
}

</script>

<template>
  <div class="doc-scanner-container" ref="scannerContainer"></div>
  <div class="controls">
    <button @click="docScanner.pause">pause</button>
    <button @click="docScanner.resume">resume</button>
    <button @click="docScanner.destroy">destroy</button>
    <button @click="clearResult">clear</button>
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
