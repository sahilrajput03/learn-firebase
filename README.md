# Readme

TODO: Official Unit Testing Guide | Firestore Tools: [Click here](https://firebase.google.com/docs/rules/unit-tests)

Please refer `README.cli` for referencing project setup details. Firebase seems awesome though.

## Creating firebase service account to have an admin sdk (useful to handle admin operations on server)

   Source: [Click here](https://firebase.google.com/docs/admin/setup)

1. Go to [Service Accounts](https://console.firebase.google.com/project/for-next-auth-example-project/settings/serviceaccounts/adminsdk) in firebase and follow the image -

	![image](https://user-images.githubusercontent.com/31458531/187941130-88343e28-d6a0-4a04-b8bf-68b7f66d0ed9.png)

2. Using firebase service account on server for admnin access, below is an example of using `firebase-admin` to get emails list so we can send a simple `/api/user-exists` requests to check if a certain user exists in the current firestore db or not. Awesome isn't it?

	```js
	const firebase = require('firebase-admin')
	// private key
	const serviceAccount = require('services/auth/firebase/for-next-auth-example-project-firebase-adminsdk-y74xa-8d5ab6b743.json')

	let firebaseApp

	if (!firebase.apps.length) {
		firebaseApp = firebase.initializeApp({
			credential: firebase.credential.cert(serviceAccount),
			databaseURL: 'https://for-next-auth-example-project-default-rtdb.firebaseio.com',
		})
	} else {
		// idk if this returs firebaseApp or not,, ~Sahil
		firebaseApp = firebase.app(); // if already initialized, use that one
	}

	// Only admin can retrieve all users BTW #1/2 https://stackoverflow.com/questions/46939765/retrieving-a-list-of-users-who-have-registered-using-firebase-auth, #2/2: https://stackoverflow.com/questions/56380107/typeerror-admin-listusers-is-not-a-function
	async function getAllUsers() {
		const allUsers: any = []

		const listAllUsers = async (nextPageToken?: string) => {
			const res: any = await (firebase.auth() as any).listUsers(1000, nextPageToken)
			allUsers.push(...res.users)
			if (res.pageToken) {
				await listAllUsers(res.pageToken)
			}
		}

		await listAllUsers()

		// console.log('allUsers?', allUsers)
		return allUsers
	}

	export default async function allUserList(req, res) {
		const users = await getAllUsers()
		const emails = users.map(u => u.email)
		// console.log({emails});
		const userAlreadyExists = emails.includes(req.body.email)
		// console.log({userAlreadyExists});
		res.status(200).json(userAlreadyExists)
	}
	```

# Firestore Fetching Data Docs

- [Perform simple and compound queries in Cloud Firestore](https://firebase.google.com/docs/firestore/query-data/queries#array_membership)
- add more urls here..

# Update user details of a firebase user
Source: [Stackoverflow answer](https://stackoverflow.com/questions/39607023/in-firebase-how-do-you-update-the-displayname-field-of-a-user-in-auth)

**Amazing Methods in Firebase Docs**: [Firebase Reference Docs](https://firebase.google.com/docs/reference/js/v8/firebase.User)

*Learn: Using method [updateProfile](https://firebase.google.com/docs/reference/js/v8/firebase.User#updateprofile) you can change the `displaName` of the user probably.*

# decoding firestore database rules

- You can make use this rule to define array's inside value driven authorization: yikes!! **Gonna use this to implement authorization in the chat app in totelApp by having array field `relatedTo` in `messages` collection and with each message I will store the user's uid and the target's uid in that array as values, thus:**
	- I will define rule for each user's access to chat messages only if they have their respective uid's in the `relatedTo`'s array values. Thus providing security to chat messages.
	- I would be able to fetch only the required messages by the user (Prevent receiving unrelated messages to the user i.e, in messages where user is neither sender nor receiver. Thus achieving performance.
		- Source (below SS): [Firebase Database Security Rules API](https://firebase.google.com/docs/reference/security/database)
		- Also: [Get started with Cloud Firestore Security Rules](https://firebase.google.com/docs/firestore/security/get-started)

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

## Code cheatsheet

- To add extra information to a user in firebase: https://stackoverflow.com/questions/39076988/add-extra-user-information-with-firebase
- `.add()` is equivalent to `.doc().set()` [source - comment on this answer](https://stackoverflow.com/a/48544954), which directs to [docs page here](https://firebase.google.com/docs/firestore/manage-data/add-data).
- To create or overwrite a single document, use the set() method, src: https://firebase.google.com/docs/firestore/manage-data/add-data
```js
import firebase from 'firebase/compat/app';
const firebaseApp = firebase.initializeApp(firebaseConfig);
const firestore = firebase.firestore();
// collections
export const usersCollection = firestore.collection('users');
export const messagesCollection = firestore.collection('messages');

// adding document
await usersCollection.add({
	email,
	uid,
	photoURL,
});

// adding document with custom id as "LA" , src: https://stackoverflow.com/questions/48541270/how-to-add-document-with-custom-id-to-firestore
usersCollection.doc("LA").set({
    name: "Los Angeles",
    state: "CA",
    country: "USA"
})

// querying documents
const snapshot: any = await usersCollection.where('email', '==', details.user.email).get();
if (snapshot.docs.length > 0) {
	const users = snapshot.docs.map((t: any) => t.data())
}
```


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
