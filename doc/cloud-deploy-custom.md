# Deploy web app (as a custom app) on IBM Cloud using Cloud Foundry

These are instructions for a quick deployment of the web app (as a custom app) on IBM Cloud.

## Prerequisites

* Ensure that the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli/index.html?locale=en-US#overview)
  tool is installed locally. Follow the instructions in the linked documentation to
  configure your environment.

## Steps
1. Copy IBM STT credentials to Train-Custom-Speech/services.json

2. Copy base model to Train-Custom-Speech/model/user.json

3. Edit the name of app in Train-Custom-Speech/client/src/App.js

4. Edit the title of app page in Train-Custom-Speech/client/public/index.html

5. Edit name and route the app in Train-Custom-Speech/manifest.yml
```
---
applications:
 - name: watson-stt-customizer
   memory: 512M
   routes:
   - route: watson-stt-customizer.mybluemix.net
```

6. Edit the API_ENDPOINT/WS_ENDPOINT
```
export default {
  API_ENDPOINT: 'https://watson-stt-customizer.mybluemix.net/api',
  WS_ENDPOINT: 'wss://watson-stt-customizer.mybluemix.net/',
  MAX_AUDIO_SIZE: 15000000,
};
```

7. Make sure build files are up to date. From the root of the project, run:

```bash
npm run build
cd client && npm run build
```

8. Deploy the app. From the root of the project run:

```bash
ibmcloud app push
```
