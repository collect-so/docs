---
sidebar_position: 1
---
# Installation (2 mins)
Getting started with Collect SDK is straightforward. This section will guide you through installing the SDK and setting up your first SDK instance.

## Step 1: Install the Package

To begin, you need to add the Collect SDK to your project. You can do this using either npm or yarn:

Using npm:

```bash
npm install @collect.so/javascript-sdk
```

Using yarn: 
```bash
yarn add @collect.so/javascript-sdk
```

### Note on SDK Size
The Collect SDK is lightweight, coming in at just 5.1kB gzipped. Learn more about the package size [here](https://pkg-size.dev/@collect.so%2Fjavascript-sdk).

## Step 2: Initialize the SDK
Once the package is installed, you can create an instance of the Collect SDK in your project. You have a couple of options depending on whether you need to specify a custom URL:

### Option 1: Basic Initialization
```typescript
import Collect from '@collect.so/javascript-sdk';

const Collect = new Collect(COLLECT_SDK_TOKEN);
```
### Option 2: Initialization with Custom URL
```typescript
import Collect from '@collect.so/javascript-sdk';

const Collect = new Collect(
    COLLECT_SDK_TOKEN,
    {
        url: 'http://your-api-url'
    }
);
```

Replace `COLLECT_SDK_TOKEN` with your actual API token, which you can obtain from the Collect Dashboard.

## Next steps
To make full use of the SDK, you'll need a valid API token. In the [next section](/quick-start/configuring-dashboard), Configuring Collect Dashboard, we'll guide you through the process of registering on the dashboard, creating a project, and generating your API token.