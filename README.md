# Readme

TODO: IN depth for testing: https://firebase.google.com/docs/rules/unit-tests

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

- Connect your app to the Cloud Firestore Emulator: [Click here - Official Docs](https://firebase.google.com/docs/emulator-suite/connect_firestore#web-version-8)

In my totel project I am connecting to emulator's firestore and auth like that:

	```js
	const auth = firebase.auth();
	const firestore = firebase.firestore();

	// ENABLE DISABLE FOR USING EMULATOR ON LOCALHOST
	const enableEmulator_onLocalhost = 1;
	// Connecting app to "CLOUD FIREBASE EMULATOR"
	const isBrowser = typeof window !== 'undefined';
	if (enableEmulator_onLocalhost && isBrowser) {
		if (window.location.hostname === 'localhost') {
			firestore.useEmulator('localhost', 8080); // https://firebase.google.com/docs/emulator-suite/connect_firestore#web-version-8 // ~sahil
			auth.useEmulator('http://localhost:9099');
		}
	}
	```

- Connect your app to the Authentication Emulator: [Click here - Official Docs](https://firebase.google.com/docs/emulator-suite/connect_auth)

## Amazing usage agnostics of using firebase auth via **firebase emulator**

![image](https://user-images.githubusercontent.com/31458531/186378967-cc6bc9ec-ba70-4391-a505-9a26cfab69b0.png)

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
