---
sidebar_position: 1
---
# Introduction to Collect SDK (3-5 mins)
Welcome to the comprehensive guide on working with the Collect SDK. This section provides an overview of initializing the Collect SDK, a crucial first step for integrating Collect into your applications. Understanding the initialization process is key to effectively manage and interact with your data through Collect.

## Initializing Collect SDK

The Collect SDK is designed to be straightforward and easy to set up. You start by importing the SDK and then creating an instance with your API token and optional configuration settings. Here's the basic way to initialize the SDK:

```javascript
import CollectSDK from '@collect.so/javascript-sdk';

const Collect = new CollectSDK('your_api_token_here', {
  url: 'https://api.yourbackend.com'
});
```

## Constructor Parameters
The CollectSDK constructor accepts two parameters:

- `token` (`string`): This is the API token issued from the Collect Dashboard. It is essential for authenticating your application's requests to the Collect API.
- `config` (`UserProvidedConfig`): This parameter allows for advanced configuration of the SDK to specify how it connects to the backend.

### Understanding `UserProvidedConfig`
The UserProvidedConfig provides detailed control over how the SDK interacts with your backend. It consists of the following structure:

```typescript

type ApiConnectionConfig = {
  host: string;
  port: number;
  protocol: string;
  } | {
  url: string;
};

type CommonUserProvidedConfig = {
  httpClient?: HttpClientInterface;
  timeout?: number;
  validator?: Validator;
} & ApiConnectionConfig;

export type UserProvidedConfig = CommonUserProvidedConfig;
```

### Config Options

- **url** (`string`): The simplest form of configuration, just providing the URL where the Collect backend is hosted.

- **host, port, protocol**: An alternative to `url`, providing a more granular way to define the connection parameters:
    - `host`: The domain name or IP address of the server.
    - `port`: The port number on which the server is listening.
    - `protocol`: The protocol used for the connection (e.g., `http`, `https`).

- **httpClient** (`HttpClientInterface`): Optional. Specifies a custom HTTP client interface for making network requests. Useful for environments where default HTTP clients do not meet specific needs.

- **timeout** (`number`): Optional. Defines a timeout period for each request in milliseconds.

- **validator** (`Validator`): Optional. Allows you to specify custom validation logic for outgoing data.

### When to Use Advanced Configuration

While the simple URL configuration suffices for most applications, advanced configuration options come into play in specific scenarios:

- **Custom environments**: If your application operates within a specific network environment or requires particular security protocols not covered by the default HTTP client.

- **Complex infrastructure setups**: When your Collect backend is distributed across different domains or ports, especially in microservices architectures.

- **Special performance requirements**: If your application demands fine-tuned network behavior, such as custom timeout settings or specialized error handling.

By configuring these parameters, you can tailor the Collect SDK to fit the precise needs of your application's infrastructure and operational requirements.

In the next sections, we'll delve into defining data models and performing CRUD operations using the Collect SDK, laying the groundwork for building robust data-driven applications.
