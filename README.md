# Readme

TODO: Official Unit Testing Guide | Firestore Tools: [Click here](https://firebase.google.com/docs/rules/unit-tests)

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

## Realtime Db vs. Firestore db

![image](https://user-images.githubusercontent.com/31458531/186386563-b7f671f5-58ee-42f3-bc88-517ee1c3b15a.png)

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


## Learn Firestore

We can query the data like that:

![image](https://user-images.githubusercontent.com/31458531/186390632-dc6d70ea-8c3e-43e4-a04b-79042efd4b42.png)

![image](https://user-images.githubusercontent.com/31458531/186390870-a0b11463-51c4-4d90-892e-81547605bc84.png)

- Are you viewing all documents from the collection ?

![image](https://user-images.githubusercontent.com/31458531/186394128-81153182-6c46-481d-96d9-3df5b8a47cbb.png)

- Use that button to clear the search query:

![image](https://user-images.githubusercontent.com/31458531/186391121-c572b2d3-791a-4bf8-94da-879e7012747d.png)

## sample photoURL

imageURL 96x96 :
- Artistic Lady:

![image](https://user-images.githubusercontent.com/31458531/186396210-d2896738-b4f7-4f61-bda4-a1d56924a460.png)

- Exported from figma maually:

![image 2](https://user-images.githubusercontent.com/31458531/186395515-ca518a39-c12e-48b3-9ce6-d3529a577038.png)

- My own:

![image](https://user-images.githubusercontent.com/31458531/186396018-4ac4a80f-aa85-4179-8a28-1a9b3707bf19.png)
