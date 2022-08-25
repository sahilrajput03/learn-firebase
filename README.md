# Readme

TODO: Official Unit Testing Guide | Firestore Tools: [Click here](https://firebase.google.com/docs/rules/unit-tests)

Please refer `README.cli` for referencing project setup details. Firebase seems awesome though.

# decoding firestore database rules

- You can make use this rule to define array's inside value driven authorization: yikes!! **Gonna use this to implement authorization in the chat app in totelApp by having array field `relatedTo` in `messages` collection and with each message I will store the user's uid and the target's uid in that array as values, thus:**
	- I will define rule for each user's access to chat messages only if they have their respective uid's in the `relatedTo`'s array values. Thus providing security to chat messages.
	- I would be able to fetch only the required messages by the user (Prevent receiving unrelated messages to the user i.e, in messages where user is neither sender nor receiver. Thus achieving performance.

![image](https://user-images.githubusercontent.com/31458531/186590903-d1b2e9ad-983f-4a98-9d39-7603e04c3a20.png)

- For making chat application: [great stackoverflow answer](https://stackoverflow.com/a/17984468/10012446) [his gist@withRecentUpdates](https://gist.github.com/katowulf/4741111)
- Use [diff checker to diff above answer vs. his gist](https://www.diffchecker.com/diff)
- Search for firebase database rules tutorial/crash course: [Click here](https://www.youtube.com/results?search_query=firebase+database+rules)
- [source of below screenshot](https://www.youtube.com/watch?v=qLrDWBKTUZo)


![image](https://user-images.githubusercontent.com/31458531/186439774-15ed3aa5-6b19-44a0-b40d-77f224e73d92.png)


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
- Artistic Lady: [Original url](https://icons.iconarchive.com/icons/raindropmemory/in-spirited-we-love/128/Photo-icon.png)

![image](https://user-images.githubusercontent.com/31458531/186396210-d2896738-b4f7-4f61-bda4-a1d56924a460.png)

- Exported from figma maually:

![image 2](https://user-images.githubusercontent.com/31458531/186395515-ca518a39-c12e-48b3-9ce6-d3529a577038.png)

- My own:

![image](https://user-images.githubusercontent.com/31458531/186396018-4ac4a80f-aa85-4179-8a28-1a9b3707bf19.png)


## This is how we live update of the data from firestore

I.e., using `onSnapshot` method

![image](https://user-images.githubusercontent.com/31458531/186425414-87237421-0f91-4335-9350-b56d5e425e69.png)

but instead if we wanted to read once we could have done:

![image](https://user-images.githubusercontent.com/31458531/186425617-8c42ea7b-2d76-4182-aba7-67ef40db81f9.png)

On user logout we can simply call the unsubscribe(see above image to know what is unsubscript) like this:

![image](https://user-images.githubusercontent.com/31458531/186426203-b241acc5-a88c-4834-9b0f-983bea668f8c.png)

Compount queryies:

![image](https://user-images.githubusercontent.com/31458531/186426440-d6a227b4-d663-4861-90f0-15e1c0a00f96.png)

and for above query to work make sure you make the composite index (this is helpful in scaling quickly):

![image](https://user-images.githubusercontent.com/31458531/186426686-ca30a571-5cc5-443c-a6ad-e72d26f08318.png)

![image](https://user-images.githubusercontent.com/31458531/186426779-fe1568fd-ef46-4861-a914-452b914954ed.png)

## Firebase has ton of builin services to support with machine learning activities

![image](https://user-images.githubusercontent.com/31458531/186433459-2da4a4fd-c062-4ae1-8541-a21027c17fde.png)

![image](https://user-images.githubusercontent.com/31458531/186433624-9f1fd975-122a-4d5e-aad7-c7c24ed1a946.png)

![image](https://user-images.githubusercontent.com/31458531/186433712-f2587f5b-a278-441c-b940-d6f5999fd84a.png)

## todo: checkout firebase extensions

![image](https://user-images.githubusercontent.com/31458531/186433843-55073e11-127a-4b22-a791-44eb5ea19b68.png)
