---
title: Introducing the Laravel Vonage Helpdesk
description: "The best way of showing Vonage APIs in action is through examples:
  welcome to our PHP App!"
author: james-seconde
published: true
published_at: 2022-10-17T18:30:04.957Z
updated_at: 2022-10-17T18:30:04.990Z
category: tutorial
tags:
  - php
  - laravel
comments: true
spotlight: false
redirect: ""
canonical: ""
outdated: false
replacement_url: ""
---
The biggest shift that we are seeing in web applications is the requirement for multi-channel communications. It isn't enough anymore for your e-commerce site to have *just* a "Contact Us" page, where you can email while also having a popup semi-instant messager like Hotjar. Now, you can choose to be ahead of the curve by changing or promoting the medium by which you communicate with customers; an example being that an email conversation can be switched to a live WhatsApp chat, or automated voice call queues that connect with real (human) agents.

It's time we demonstrated how these channels work, so without further ado (for my PHP developers), I would like to introduce *The Vonage Helpdesk*. In this article I'll show you how to fire it up locally and then dig into how the SMS aspect of the app works to start us off.

### What is the Vonage Helpdesk?

Some time ago, we had a concept web application called Deskmo. This is a new reinterpretation of that: a web application that will slowly be built over time to include best practises for working with Vonage APIs and demonstrations of modern PHP code.

### What is it written in?

Vonage Helpdesk is a PHP web application built in Laravel 9. It uses Laravel's Sail for portability, so you have a fully Dockerized app (goodbye, system-level dependencies!) that uses MySQL as the database.

### Installing

#### Requirements

* A Windows, Linux, or Mac machine that can run Docker v20+ (the current version)
* PHP v8.0+
* NodeJS v17+
* npm v8.5+
* Composer v2+
* Git installed
* A Vonage account in credit

First up, we need to download the repository. Type the following into the command line:

```bash
git clone git@github.com:Vonage-Community/sample-messages_voice-php-helpdesk.git
```

Now, we should have the application in a new folder. Next up, we install the PHP dependencies. Do this by navigating into the folder (i.e. `sample-messages_voice-php-helpdesk`) and running Composer:

```bash
composer install
```

Laravel Sail should have been pulled into the `vendor` folder, so providing you have Docker installed, you can run the following command to boot up the application:

```bash
./vendor/bin/sail up
```

\### Migrations, Seeders and Vite

Next up we need to run the database migrations:

```bash
./vendor/bin/sail artisan migrate
```

Our application needs a super user to login, so we run the database seeder:

```bash
./vendor/bin/sail artisan db:seed
```

Because the application uses Laravel Breeze scaffolding for authentication out-of-the box, we'll need to run the Vite development server outside of your Docker containers to compile your JavaScript (this now comes with Laravel pre-configured). To run Vite, open a new terminal in your application folder and run the following:

```bash
npm run dev
```

OK, we should all be set. Open a browser and navigate to `localhost` and hopefully you should see the splash screen:

![Splash screen for helpdesk with Vonage logo](/content/blog/introducing-the-laravel-vonage-helpdesk/screenshot-2022-10-20-at-11.17.47.png)

### The Ticket System

So, what have we got? The Vonage Helpdesk emulates a ticketing system where customers all have an account and can create a ticket, choosing a communication medium of choice. Admins can then view the tickets, and respond to them. The application will take the users' provided phone number and use that for ticket responses from the admin on the web application side.

### How does it do that? Part 1: SMS

You can log in already now as the superuser (the seeded user is \`admin@vonage.com\` and the password is \`password\` - hey, it's a concept app so by all means change it to a not-awful password!). Now we need a new "customer" user.

On the splash screen, navigate to the top right-hand link to register. We're going to be looking at the SMS interactions, so we're going to choose 'SMS' as the communication method. Make sure you choose a working phone number.

![Filled out helpdesk sign up form](/content/blog/introducing-the-laravel-vonage-helpdesk/screenshot-2022-10-21-at-10.42.18.png)

Now log in with your new details, and you should see the dashboard:

![Helpdesk dashboard with no tickets](/content/blog/introducing-the-laravel-vonage-helpdesk/screenshot-2022-10-21-at-11.15.46.png)

OK! Time to create a new ticket. Hit 'New Ticket' and fill out the details like so:

![creating a new ticket in the dashboard](/content/blog/introducing-the-laravel-vonage-helpdesk/screenshot-2022-10-21-at-11.23.01.png)

For reference, \`In-App Messaging\` is for using the Conversations API for realtime messaging, which we're not doing in this article, so leave that unchecked. Once you create the ticket, after hitting submit you'll be taken to the new ticket:

![newly created ticket with email. channel source and message](/content/blog/introducing-the-laravel-vonage-helpdesk/screenshot-2022-10-21-at-11.23.18.png)

Success! Nothing has actually happened just yet over the communication channel, as you are the ticket creator. But, logging in as the administrator to respond to the ticket is where the magic comes in. You'll notice the ticket entry has \`web\` on it: this is where our multichannel communications come in. For this SMS chat, if the admin responds, it will be sent to the user via SMS, which they can then reply to using their phone. However, the user could **also** reply via the web application as well. So, we're already multichannel.

If we log out, and re-login as the 

### Coming Next...

Aha! We're not done yet by a country mile! Keep an eye out for more articles in the series as we add to the app, including Voice capabilities using Deepgram, realtime updates using Laravel Livewire, and building out the test suite with PEST.