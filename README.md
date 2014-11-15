go-natty [![Travis CI Status](https://travis-ci.org/getlantern/go-natty.svg?branch=master)](https://travis-ci.org/getlantern/go-natty)&nbsp;[![Coverage Status](https://coveralls.io/repos/getlantern/go-natty/badge.png)](https://coveralls.io/r/getlantern/go-natty)&nbsp;[![GoDoc](https://godoc.org/github.com/getlantern/go-natty?status.png)](http://godoc.org/github.com/getlantern/go-natty)
==========
go-natty provides a Go wrapper around the
[natty](https://github.com/getlantern/natty) NAT-traversal utility.

To install:

`go get github.com/getlantern/go-natty`

For docs:

[`godoc github.com/getlantern/go-natty/natty`](https://godoc.org/github.com/getlantern/go-natty/natty)

## Embedding Natty

To build the go files that embed the natty executables for different platforms,
just place the binaries into the right subfolder of `binaries` and then run
`embednatty.bash`. This script takes care of code signing the Windows and OS X
executables.

Note - you will need to set the environment variables BNS_CERT and BNS_CERT_PASS
to point to [bns-cert.p12](https://github.com/getlantern/too-many-secrets/blob/master/bns_cert.p12)
and its [password](https://github.com/getlantern/too-many-secrets/blob/master/build-installers/env-vars.txt#L3)
so that the script can sign the Windows executable.

## Demo

There's a [demo application](https://github.com/getlantern/go-natty/tree/master/demo) available. Right now it only
works on OS X. Binaries are available
[here](https://github.com/getlantern/go-natty/releases/download/demo-0.0.1/natty-demo-osx).

The client and server signal with each other using
[waddell](getlantern/waddell) and the client sends UDP packets to the server
once NAT-traversal is complete. The client finds the server on waddell using
its waddell id.

### Example Demo Session

#### Server

```bash
Macintosh% ./natty-demo-osx -mode server                                                            
2014/09/16 18:41:36 Starting server, waddell id is "e6679a41-0003-4f9b-8ae4-671a8a196d13"
2014/09/16 18:41:49 Got five tuple: &{udp 192.168.1.160:55285 192.168.1.160:60530}
2014/09/16 18:41:49 Listening for UDP packets at: 192.168.1.160:55285
2014/09/16 18:41:49 Got UDP message from 192.168.1.160:60530: 'Hello from 192.168.1.160:60530'
2014/09/16 18:41:50 Got UDP message from 192.168.1.160:60530: 'Hello from 192.168.1.160:60530'
2014/09/16 18:41:51 Got UDP message from 192.168.1.160:60530: 'Hello from 192.168.1.160:60530'
2014/09/16 18:41:52 Got UDP message from 192.168.1.160:60530: 'Hello from 192.168.1.160:60530'
```

Note - you have to specify the waddell id emitted by the server when running the
client.

#### Client

```bash
Macintosh% ./natty-demo-osx -mode client -server "e6679a41-0003-4f9b-8ae4-671a8a196d13"
2014/09/16 18:41:46 Starting client, connecting to server e6679a41-0003-4f9b-8ae4-671a8a196d13 ...
2014/09/16 18:41:48 Got five tuple: &{udp 192.168.1.160:60530 192.168.1.160:55285}
2014/09/16 18:41:49 Sending UDP message: Hello from 192.168.1.160:60530
2014/09/16 18:41:50 Sending UDP message: Hello from 192.168.1.160:60530
2014/09/16 18:41:51 Sending UDP message: Hello from 192.168.1.160:60530
2014/09/16 18:41:52 Sending UDP message: Hello from 192.168.1.160:60530
```

Acknowledgements:

go-natty is just a wrapper around [natty](https://github.com/getlantern/natty),
which is itself just a wrapper around the
[WebRTC Native Code Package](http://www.webrtc.org/webrtc-native-code-package).
