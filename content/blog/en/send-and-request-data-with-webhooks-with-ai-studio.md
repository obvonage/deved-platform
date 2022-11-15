---
title: Send and Request Data with Webhooks with AI Studio
description: Learn how to Send and Request Data with Webhooks with AI Studio
author: michael-crump
published: true
published_at: 2022-11-07T03:19:11.707Z
updated_at: 2022-11-07T03:19:11.883Z
category: tutorial
tags:
  - rest
  - aistudio
  - ""
comments: true
spotlight: false
redirect: ""
canonical: ""
outdated: false
replacement_url: ""
---
## Introduction

[Vonage AI Studio](https://www.vonage.com/communications-apis/ai-studio/?icmp=l3nav%7Cl3nav_gototheaistudiooverviewpage_novalue) is a Low-Code / No-Code conversational AI platform that helps businesses handle complex customer interactions through voice and text. It starts with a friendly user interface where you can drag and drop modules to build a conversational Virtual Agent that your customers can use. One feature often overlooked is the ability to get data from a third-party source such as a Webhook. For a refresher, a Webhook is triggered by some event, such as pushing code to a repository. When that event occurs, the source site makes an HTTP request to the URL configured for the webhook and returns data typically formatted in JSON. In this blog post, we'll retrieve data from a Webhook in AI Studio, and once you get the hang of it, you can replace our Webhook with your own!

## What are we going to build?

We will build a Virtual Agent for a fictitious bank called the **Bank of the People**. In this scenario, a customer could call a phone number, and the system would verify their identity by validating a pin number associated with their account. Once the system validates that the PIN they entered is correct, they will have access to their account. They could then ask questions such as "What is their account balance" and get a verbal reply from the Virtual Agent. To make things easier to test, we will use the built-in AI Studio test tool and have the Virtual Agent dial our personal phone number. Let's begin.

## Setting up the Webhook

To begin, we will need a place to store our customer data to access it from AI Studio. A great place to create a fake online REST server is  <https://my-json-server.typicode.com/>. You only need to create a JSON file on GitHub and point to that repo using the following syntax: my-json-server.typicode.com/`user/repo/`. Let's walk through this step-by-step:

1. Create a repository on GitHub and make a note of the following: `(<your-username>/<your-repo>)`
2. Create a db.json file. You can use [mine](https://github.com/mbcrump/ai-studio-api/blob/main/db.json) as an example. 
3. Now go to the following URL: `https://my-json-server.typicode.com/<your-username>/<your-repo>` to access your server. 

If you'd prefer to use mine, then you can. For example, we can access the `customer's` table with the following URL: `https://my-json-server.typicode.com/mbcrump/ai-studio-api/customers`. Below is a snippet of what data is returned:

```json
[
  ...
  {
    "id": 4,
    "email": "michael.crump@vonage.com",
    "phone": "14259999999",
    "name": "Michael Crump",
    "cc_balance": "76.31",
    "pin": "1234"
  }
]
```

We'll be using this endpoint to retrieve data for the application, such as **Phone** number, **Name**, **Balance**, and **PIN**. 

In this scenario, once the customer dials the Bank's phone number, we'll use the phone number to look up the correct value for the PIN via this Webhook. We'll use a query to get specific details from the `customer's` table, such as `https://my-json-server.typicode.com/mbcrump/ai-studio-api/customers?phone=14259999999`, where we pass in the phone number as a parameter.

## Getting Started

Now that we have a Webhook created, we will access it by building an application in [AI Studio](https://www.vonage.com/communications-apis/ai-studio/?icmp=l3nav%7Cl3nav_gototheaistudiooverviewpage_novalue) . Log in and press the button to **Create an Agent**. You will see an option asking what type of Agent you would like to create.

![The Agent Creation Screen](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/agent-creation.png "agent-creation.png")

To begin, select the **Telephony** agent and press **Next** to create an agent that will interact over the telephone.

We will need to fill in some details here:

* Region: Where will your Agent be typically used - The USA or in Europe
* Agent Name: Give your Agent a unique name that is meaningful to you. In our case, we will use BankOfThePeople.
* API Key: This shows the API Key associated with your account.
  Language: Select the language of your Agent.
* Language: Select the language of your Agent.
* Voices: Select any voice that you like.
* Time Zone: Choose the time zone where your Agent will operate.

After completing the form, mine looks like this:

![The Agent Creation Form Completed](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/agent-creation-completed.png "agent-creation-completed.png")

Next, there is the option to choose a template. Since we want to learn how to do this from scratch, we'll select **Start from Scratch** and press **Next**.

![The Agent Start From Scratch Template](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/agent-start-from-scratch.png "agent-start-from-scratch.png")

Finally, we need to select an event that will trigger our agent. In this case, we'll choose **Inbound call** as a customer would dial our number to get information on their account.

Next, you will see the main user interface of AI Studio! There is a node in the center of your screen called **START** with a checkmark in **Record Call**, as shown below.

![The Start Screen of AI Studio](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/ai-studio-startscreen.png "ai-studio-startscreen.png")

## Building the App

The conversational flow starts here and has to be connected to other nodes for the Virtual Agent to know what to do next.
We will begin by dragging and dropping a **Speak** Conversation Node onto the canvas and entering a Welcome Message such as "Welcome to Bank of the People!". Once complete, press Enter, and remember to press **Save & Exit**.

![The Send Message Node](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/send-message-1.png "send-message-1.png")

Next, we need to connect the **Speak 1** Node to the **START** node. 

![Connecting the Speak 1 Node with the Start Node](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/connect-nodes.png "connect-nodes.png")

Press the **Tester** button at the top right of the screen, select **Inbound call** as the event, then **Start phone call** and enter your phone number, then your agent will call your phone and repeat the Welcome Message! How cool is that? 

![Testing our Webhook call](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/ai-studio-tester.png "ai-studio-tester.png")

## Adding the Webhook

Scroll down from the **Nodes** menu until you get to **Integrations**, and select **Webhook** and drag and drop that onto your canvas. 

![Integrations Dialog](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/integrations.png "integrations.png")

Once you open it, you will need to provide your endpoint URL. Since we will query based on our phone number, we'll use the following hardcoded URL (which points to a specific phone number): `https://my-json-server.typicode.com/mbcrump/ai-studio-api/customers?phone=14259999999`. Once you've added it, then press **Test request** as shown below. 

![Testing our Webhook call](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/test-webhook-1.png "test-webhook-1.png")

If you see a status of 200, that means the request returned successfully (You can see in the **Response** the information we got back based on this user's phone number (which was hardcoded).

While this data was hardcoded in the query URL, how does AI Studio pass this information based on the number that is called in? This is where we'll use one of the predefined system parameters called **CALLER_PHONE_NUMBER,** as shown below.

![Testing our Webhook call](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/system_parameters.png "system_parameters.png")

You can trigger this dialog by pressing the `$` and selecting the system parameter you wish to use. Now our URL is `https://my-json-server.typicode.com/mbcrump/ai-studio-api/customers?phone=$CALLER_PHONE_NUMBER`.

Once complete, remember to press **Save & Exit**. 

![Testing our Webhook call](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/webhook-with-phone-number.png "webhook-with-phone-number.png")

If we try to test the WebHook now, you'll get a 200 status code, but there won't be any data inside the response.

![No Data Returned](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/no-data-returned.png "no-data-returned.png")

We can fix this by clicking the **Settings** icon and specifying a **CALLER_PHONE_NUMBER,** as shown below. 

![Specifying a Phone Number](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/setting-system-parameters.png "setting-system-parameters.png")

Now, if you press **Resend request**, you will get a proper response, as shown below.

![Resent Webhook Request](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/successful-response.png "successful-response.png")

## Setting up Parameters to Do Something With the Data Returned

Now that we can call a WebHook from AI Studio and return data, we will use **Parameters** to extract and utilize specific information from the user’s input. For example, To collect a user's information (i.e., PIN or Account Balance), you need to define a parameter that will store the information so that it can be accessed later. 

To **Create** a parameter, in the left navigation, click on **Properties** then **Parameters**. 

![Parameters Selection](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/parameters-selection.png "parameters-selection.png")

Add a parameter by clicking on the first row of the **Custom Parameters** table.

We need to add four parameters here to store the PIN that belongs to the customer, the PIN that the customer typed in, the customer's name, and the account balance in case they successfully entered a pin number.

| Name         | Entity Type  |
| ------------ | ------------ |
| pin_number   | @ sys.number |
| input_number | @ sys.number |
| name         | @ sys.any    |
| balance      | @ sys.number |

Your screen should look like the following once complete: 

![Custom Parameters](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/parameters.png "parameters.png")

Press **Close** once all the information has been entered.  

Click on **Webhook 1** again and look under **Response Mapping**. Here we'll enter the first **Object path** as `[0][name]` and give it the **parameter** we created called **name**. 

![Response Mapping](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/response-mapping.png "response-mapping.png")

We'll collect the **balance** and **PIN** while we are here. 

After you add them, your screen should look like the following: 

![Response Mapping](./Images/response-mapping-1.png "response-mapping-1.png")

Once complete, do not forget to press **Save & Exit**. 

We will use a **Collect Input** node to ask the user for their PIN. Go ahead and drag and drop the node from the **Conversation** menu. 

![Collect Input Node](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/collect-input.png "collect-input.png")

For the **Parameter**, use the **input_number** that we defined earlier. For **Prompts**, enter "Enter your four-digit pin number to access your account." or something similar, as shown below. 

![Collect Input Node](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/parameter-input-number.png "parameter-input-number.png")

Now we will connect the **Webhook 1** to the **Collect Input 1** node as shown below. 

![Connect Webhook Collect Input](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/connect-webhook-collect-input.png "connect-webhook-collect-input.png")

Drag and drop a **Condition 1** node from the **Conversation** menu. Click on "Default" inside the note and press **Create Condition**. 

As shown below, we will create a condition that checks if the **Input Number** they entered matches the **PIN** from our Webhook.

![Condition Requirements](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/condition-req.png "condition-req.png")

Once complete, do not forget to press **Save & Exit**.

Drag and drop two more **Speak** Conversation Nodes onto the canvas. For the **Speak 2** node, enter the message, `Verification Successful Welcome $name. Your balance is $balance.` Where `$name` is the parameter that we defined earlier.

![Speak 2 Node](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/speak-2-node.png "speak-2-node.png")

For the **Speak 3** node, enter the message, "I'm sorry that was not correct." where `$name` is the parameter that we defined earlier. Once complete, press Enter and do not forget to press **Save & Exit**. 

Finally, add an **End call** node onto the canvas. 

Now you need to connect the **Condition 1** (New Condition) to **Speak 2** and **Condition 1** (Default) to **Speak 3**. Then connect **Speak 2** and **Speak 3** to the **End Call 1** node, as shown below. 

![Final Result](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/final-result.png "final-result.png")

You'd want to add an entry to the `db.json` file we created earlier with your phone number if you're going to try it on a physical device. 

If you'd prefer to use my sample phone number, you can test the Virtual Agent with the following steps: 

Press the **Tester** button at the top right of the screen, then select **Inbound call** as the event, then **Start chat**. You'll need to set the initial parameters as we did earlier for the **CALLER_PHONE_NUMBER** and set it to `14259999999`. 

![Set Inital Parameters](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/set-initial-parameters.png "set-initial-parameters.png")

Now, if you re-run the tester tool, you can supply a four-digit number (such as 1234), and the Virtual Agent will return your **Name**, and **Account Balance** found from the `db.json` file as shown below.

![Tester Chat Completed](/content/blog/send-and-request-data-with-webhooks-with-ai-studio/tester-chat-completed.png "tester-chat-completed.png")

Awesome! We've successfully implemented a Webhook and created logic with what the data returned. 

## Wrap-up

Now it is your turn. There are several ways that we could extend the application, such as:

* The logic for an instance when no PIN is found
* Webhook Authentication for security purposes such as authentication tokens, etc.
* Maximum number of retries in case of a failed attempt

What are you waiting for? Head over to [AI Studio](https://www.vonage.com/communications-apis/ai-studio/?icmp=l3nav%7Cl3nav_gototheaistudiooverviewpage_novalue)  to begin your journey. 

If you have questions or feedback, join us on the [Vonage Developer Slack](https://developer.vonage.com/community/slack) or send me a Tweet on [Twitter](https://twitter.com/mbcrump), and I will get back to you. Thanks again for reading, and I will catch you on the next one!