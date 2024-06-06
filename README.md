# Component to DSN Multiplexer
### IMPORTANT NOTE: This is a code sample, NOT a supported product, use at your own risk.

## Description
A self hosted proxy server written in Golang that serves as a middleware between your app and Sentry.
This server allows you to route error events and minidumps from one codebase to multiple Sentry projects using a custom tag name called `sentry_relay_component`

## Supported Datamodels
- Errors
- Minidumps

## Tested SDKs
 - Python
 - Javascript
 - Electron

## Configuration

Inside your app, use Sentry.setTag('sentry_relay_component', {component_name}) in places/scenarios where you would like the error event to be routed to a specifc project.

Create a configuration file in JSON format to map components to their respective project DSNs. For example, `config.json`:

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
1) defaultDSN - The DSN of your main/default project where the events will be sent to in cases where `sentry_relay_component` is not specified or has no mapping in the config file.
2) ConfigFilePath - A path to a file that contains a JSON object that contains mapping from component name to DSN
3) numberOfGoWorkers - This argument is set in order to support concurency, each go worker will handle X goroutines, each goroutine will process one event.

Run the proxy with the following command:
```
go run main <defaultDSN> <configFilePath> <numberOfGoWorkers>
```
Example:
```
go run main http://b299354f889d530fcc62f4c464b44a35@o0.ingest.sentry/4507374994653185 config.json 20
```
This starts the server on port 8080. The server listens for incoming requests and forwards them based on the component tags defined in the configuration file.

In your app, initialize Sentry with pointing the DSN to the proxy server.

You will need to substitute your org ingest url with the address of the proxy server.

### Example:
Let's say you are runnning this proxy server on: 
```
www.yourserveraddress.com:8080
```
and the DSN of your main project is: 
```
https://b299354f889d530fcc62f4c464b44a35@o0.ingest.sentry.io/4507374994653185
```
The DSN that you will be pointing to is: 
```
https://b299354f889d530fcc62f4c464b44a35@www.yourserveraddress.com:8080/4507374994653185
```

Initialize Sentry in your app as follows:
```
Sentry.init({
  dsn: 'https://b299354f889d530fcc62f4c464b44a35@www.yourserveraddress.com:8080/4507374994653185'
});
```

## Diagram
![Screenshot 2024-06-05 at 9 32 47 PM](https://github.com/sentry-demos/component_to_dsn_mux/assets/89414234/24b734ca-b77c-4f8f-a7cb-f534eccf0c9e)




