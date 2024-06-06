# Component to DSN Multiplexer
### IMPORTANT NOTE: This is a code sample, NOT a supported product, use at your own risk.

## Description
A self hosted proxy server written in Golang that serves as a middleware between your app and Sentry.
This server allows you to route events from one codebase to multiple Sentry projects using a custom tag name called `sentry_relay_component`

## Supported Datamodels
- Errors
- Minidumps

## Tested SDKs
 - Python
 - Javascript
 - Electron

## Usage

The Sentry Relay Proxy is designed to forward incoming HTTP requests to different Sentry projects based on a configuration file. This enables dynamic routing and load balancing of Sentry events.


## Configuration

Create a configuration file in JSON format to map components to their respective DSNs. For example, `config.json`:

```json
{
    "mapping": {
        "componentA": "https://examplePublicKey@o0.ingest.sentry.io/0",
        "componentB": "https://examplePublicKey@o0.ingest.sentry.io/1"
    }
}
```

## Usage
Run the proxy with the following command:
./sentry-relay-proxy <defaultDSN> <configFilePath> <numberOfGoWorkers>

Example
./sentry-relay-proxy https://defaultPublicKey@o0.ingest.sentry.io/0 ./config.json 15

This starts the server on port 8080. The server listens for incoming requests and forwards them based on the component tags defined in the configuration file.
