1. Make sure you retrieved the GCP SDK while cloning the repo (git clone --recurse-submodules <link of the repo>), and make sure you have CodeLLDB extension installed on your vscode setup (otherwise, modify launch.json)
2. Make the GCP SDK for first time (cd iot-device-sdk-embedded-c , make , Yes to retrieve mbedtls and autobuild) (This is necessary for the Cmake config!)
3. Cmake build on VSCode
