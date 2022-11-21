---
title: I Promise version 3 of the Node SDK is worth it
description: v3.0. of the NodeJS SDK, brings new improvements and long "await"ed features
author: chuck-reeves
published: false
published_at: 2022-11-21T15:40:57.452Z
updated_at: 2022-11-21T15:40:57.464Z
category: announcement
tags:
  - node
comments: true
spotlight: false
redirect: ""
canonical: ""
outdated: false
replacement_url: ""
---
That's right, [v3](https://github.com/Vonage/vonage-node-sdk/releases/tag/%40vonage%2Fserver-sdk%403.0.0) of the node SDK is now fully promised. A long-asked-for feature has been released, along with breaking out the SDK into smaller components, which will help the team release new features more quickly. 

## Breaking Changes

### Promises

All function callbacks have been removed. In the future, you will need to use `async/await` or `.then/.catch`. This should make your code cleaner by avoiding callback hell. For example:

```js
const Vonage = require('@vonage/server-sdk')

const vonage = new Vonage({
	apiKey: VONAGE_API_KEY,
	apiSecret: VONAGE_API_SECRET,
})

const from = VONAGE_BRAND_NAME;
const to = TO_NUMBER;
const text = 'A text message sent using the Vonage SMS API'

vonage.message.sendSms(from, to, text, (err, responseData) => {
	if (err) {
		console.log(err);
		return;
	}

	if(responseData.messages[0]['status'] === "0") {
		console.log("Message sent successfully.");
		return;
	} 
	
	console.log(`Message failed with error: ${responseData.messages[0]['error-text']}`);
})
```

It can now be written as:

```js
const { Vonage } = require('@vonage/server-sdk')

const vonage = new Vonage({
	apiKey: VONAGE_API_KEY,
	apiSecret: VONAGE_API_SECRET,
})

const from = VONAGE_BRAND_NAME;
const to = TO_NUMBER;
const text = 'A text message sent using the Vonage SMS API';

const sendSMS = async () {
	try {
		const response = await vonage.sms.send({to, from, text});
		console.log(response);
	} catch (err) {
		console.log('There was an error sending the messages.'); 
		console.error(err);
	}
};
```

### Parameter updates

Functions have also been updated to use parameter objects over using function arguments instead. This will help keep function with many optional parameters more manageable. We have published a migration document in each package that will describe in more detail which functions have been updated.  

## Smaller packages and Typescript

Version 3 uses Typescript to help with code completion in your preferred IDE. But an even bigger improvement is the use of monorepos. Now you can use specific package(s) in your application rather than including the entire suite of products. We made this change strategically to allow us to release beta features and fixes without holding up the main development branch. One example of this is the new video API (which in the past was part of the [OpenTok SDK](https://github.com/opentok/opentok-node)):

```js
import { Auth } from '@vonage/auth';
import { Video } from '@vonage/video';

const credentials = new Auth({
	apiKey: VONAGE_API_KEY,
	apiSecret: VONAGE_API_SECRET,
});

const video = new Video(credentials);

const getVideoSession = async () => {
	try {
		const session = await video.createSession({ mediaMode: 'routed' });
		console.log('Session created', session);
		return session;
	} catch (error) {
		console.log('Failed to create session');
		console.error(error);
		throw error;
	}
}
```

Now you can use a beta SDK package and keep the stable packages. 

## Support

Following [our support](https://github.com/Nexmo/server-sdk-specification/blob/main/SPECIFICATION.md#sdk-support) document, we will continue to support version 2 for another six months bringing us to around May of 2023. We will not be adding new features, just fixes.

For more detailed information, please visit the [SDK documentation](https://developer.vonage.com/documentation), look at the [code snippets](https://github.com/Vonage/vonage-node-code-snippets) or reach out to us on [slack](https://developer.vonage.com/community/slack).