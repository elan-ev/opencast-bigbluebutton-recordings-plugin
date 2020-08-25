# opencast-bigbluebutton-recordings-plugin

This plugin serves BigBlueButton frontends recordings from an Opencast instead of the actual
BigBlueButton. It does this by redirecting calls from the BBB-API `/bigbluebutton/api/getRecordings`
to the Opencast API. This allows to create recordings BBB recordings to be hosted in Opencast
and have them still show up in BBB frontends, without requiring any changes to the said
frontends. 

To successfully retrieve a recording from Opencast, the recordings UUID needs to match the
meetingID specified in the query.

For more information on how to get your BBB recordings to Opencast, visit: 
https://github.com/elan-ev/opencast-bigbluebutton-integration/tree/master/post-publish

## Build

To install the plugin, clone the repository and run the commands below. 
You will need [Golang](https://golang.org/) to build successfully.
After building successfully, copy the GO binarys to the BBB server.

```
go mod download
go build
```

Note: When building on debian or derivatives, it is recommended to install Golang from the 
official archive at https://golang.org/doc/install,
as current packages are liable to be outdated.

## Configuration

Configuration of the plugin is done via the configuration file `config.yml`, which should be self-explanatory.
The file is read from the directory the service is started from.

Furthermore, the proxy server needs to configured so that it redirects API calls from the plugin.
Add the statement below to `/etc/nginx/sites-available/bigbluebutton`.

```
/etc/nginx/sites-available/bigbluebutton
location /api/getRecordings {
	proxy pass,
	proxy redirect
}
```

## Run

```
./opencast-bigbluebutton-recordings-plugin
```

## Example query

```
https://my-bigbluebutton.de/bigbluebutton/api/getRecordings?meetingID=9b35688d-9dba-4074-adb6-0f6aab1d4805 
```
