# Deploy web app (as a custom app) on IBM Cloud using Cloud Foundry

These are instructions for a quick deployment of the web app (as a custom app) on IBM Cloud.

## Prerequisites

* Ensure that the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli/index.html?locale=en-US#overview)
  tool is installed locally. Follow the instructions in the linked documentation to
  configure your environment.

## Steps
1. Copy IBM STT credentials to Train-Custom-Speech/services.json
```
{
  "services": {
    "code-pattern-custom-language-model": [
      {
        "credentials": {
          "apikey": "<your api key>",
          "url": "<your api url>"
        },
        "label": "speech_to_text",
        "name": "code-pattern-custom-language-model"
      }
    ]
  }
}
```

2. Copy base model to Train-Custom-Speech/model/user.json
```
{
  "user1": {
    "password": "user1",
    "langModel": "custom-model-1",
    "acousticModel": "acoustic-model-1",
    "baseModel": "en-US_NarrowbandModel"
  },
  "user2": {
    "password": "user2",
    "langModel": "custom-model-2",
    "acousticModel": "acoustic-model-2",
    "baseModel": "en-US_NarrowbandModel"
  }
}
```

3. Edit the name of app in Train-Custom-Speech/client/src/App.js
```
<Navbar.Header>
    <Navbar.Brand>
        <Link to="/">Watson STT Customizer</Link>
    </Navbar.Brand>
    <Navbar.Toggle />
</Navbar.Header>
```

4. Edit the title of app page in Train-Custom-Speech/client/public/index.html
```
<head>
    ...
    <title>Watson Speech to Text Customizer</title>
</head>
```

5. Edit name and route the app in Train-Custom-Speech/manifest.yml
```
---
applications:
 - name: stt-customizer-bigclient-campaign-xx
   memory: 512M
   routes:
   - route: stt-customizer-bigclient-campaign-xx.mybluemix.net
```

6. Edit the Train-Custom-Speech/client/src/config.js
```
export default {
  API_ENDPOINT: 'https://stt-customizer-bigclient-campaign-xx.mybluemix.net/api',
  WS_ENDPOINT: 'wss://stt-customizer-bigclient-campaign-xx.mybluemix.net/',
  MAX_AUDIO_SIZE: 15000000,
};
```

7. Make sure build files are up to date. From the root of the project, run:

```bash
npm install
npm run build
cd client && npm run build
cd ..
```

8. Deploy the app. From the root of the project run:

```bash
ibmcloud login
ibmcloud target --cf
ibmcloud app push
```