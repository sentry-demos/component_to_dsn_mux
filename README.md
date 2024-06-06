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


## Configuration

Create a configuration file in JSON format to map components to their respective DSNs. For example, `config.json`:

```json
{
    "mapping": {
        "A": "https://examplePublicKey1@o0.ingest.sentry.io/1",
        "B": "https://examplePublicKey2@o0.ingest.sentry.io/2"
    }
}
```

## Run
The main function should be provided with 3 arguments:
1) defaultDSN - The DSN of your main/default project where the events are going to be sent to in case that component name is not specified or has no mapping in the config file.
2) ConfigFilePath - A file that contains a JSON object that contains mapping from component name to DSN
3) numberOfGoWorkers - This argument is set in order to support concurency, each go worker will handle X goroutines, each goroutine will process one event.

Run the proxy with the following command:
```
go run main <defaultDSN> <configFilePath> <numberOfGoWorkers>
```
Example:
```
go run main http://efe273e1f9aae6f6f0bc4fb089fab1d7@o0.ingest.sentry/4507268303880192 config.json 20
```
This starts the server on port 8080. The server listens for incoming requests and forwards them based on the component tags defined in the configuration file.

In your app, initialize Sentry with poiting the DSN to the proxy server.
Example:
```
Sentry.init({
  dsn: 'https://b299354f889d530fcc62f4c464b44a35@www.yourserveraddress.com:8080/4507374994653185'
});
```

## Diagram
![Screenshot 2024-06-05 at 9 32 47 PM](https://github.com/sentry-demos/component_to_dsn_mux/assets/89414234/24b734ca-b77c-4f8f-a7cb-f534eccf0c9e)




