# Readme

Please refer `README.cli` for referencing project setup details. Firebase seems awesome though.


# Using firebase emulator for locally running all firebase utilities

```bash
# run firebase emulator suite
fes # alias of below
firebase emulators:start

# start firestore only
fesf # alias of below
firebase emulators:start --only firestore
# FYI: This will start both firestart emulator but with only firestore running, yo!
```

# Connect your existing firebase project to use firebase emulator's firestore (or other other services)

In my totel project I am using it like:

```js
const firestore = firebase.firestore();

// Connecting app to "Cloud Firebase Emulator"
const enableEmulator_onLocalhost = 1;
const isBrowser = typeof window !== 'undefined';
if (enableEmulator_onLocalhost && isBrowser) {
	if (window.location.hostname === 'localhost') {
		firestore.useEmulator('localhost', 8080); // https://firebase.google.com/docs/emulator-suite/connect_firestore#web-version-8 // ~sahil
	}
}
```

## Cli help text

```bash
firebase emulators:start -h
# OUTPUT:
# Usage: firebase emulators:start [options]

# start the local Firebase emulators

# Options:
#   --only <emulators>          only specific emulators. This is a comma separated list of
#                               emulator names. Valid options are:
#                               ["auth","functions","firestore","database","hosting","pubsub","storage","eventarc"]
#   --inspect-functions [port]  emulate Cloud Functions in debug mode with the node inspector
#                               on the given port (9229 if not specified)
#   --import [dir]              import emulator data from a previous export (see
#                               emulators:export)
#   --export-on-exit [dir]      automatically export emulator data (emulators:export) when the
#                               emulators make a clean exit (SIGINT), when no dir is provided
#                               the location of --import [dir] is used
#   -h, --help                  output usage information
```