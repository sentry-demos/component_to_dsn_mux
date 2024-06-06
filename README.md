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

## Run
Run the proxy with the following command:
`go run main <defaultDSN> <configFilePath> <numberOfGoWorkers>`
Example:
`go run main http://efe273e1f9aae6f6f0bc4fb089fab1d7@a2f9-73-158-188-16.ngrok-free.app/4507268303880192 config.json 20`
This starts the server on port 8080. The server listens for incoming requests and forwards them based on the component tags defined in the configuration file.

## Diagram
![Screenshot 2024-06-05 at 9 32 47 PM](https://github.com/sentry-demos/component_to_dsn_mux/assets/89414234/24b734ca-b77c-4f8f-a7cb-f534eccf0c9e)




