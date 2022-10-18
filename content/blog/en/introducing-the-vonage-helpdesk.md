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
The biggest shift that we are seeing in web applications is the requirement for multi-channel communications. It isn't enough anymore for your e-commerce site to have _just_ a 'Contact Us' page, where you can email while also having a popup semi-instant messager. Now, you can choose to be ahead of the curve by changing the medium by which you communicate with customers; an example being that an email conversation can be switched to a live WhatsApp chat, or automated voice call queues that connect with real (human) agents.

It's time we demonstrated how these channels work, so without further ado (for my PHP developers), I would like to introduce *The Vonage Helpdesk*. In this article I'll show you how to fire it up locally and then dig into how the SMS aspect of the app works. As a sneak preview: keep an eye out for more articles in the series, including Voice capabilities, Laravel Livewire and Deepgram integrations. Anyway, enough with the teasers, let's get started.

### What is the Vonage Helpdesk?

Some time ago, we had a concept web application called Deskmo. This is a new reinterpretation of that: a web application that will slowly be built over time to include best practises for working with Vonage APIs and demonstrations of modern PHP code.

### What is it written in?

Vonage Helpdesk is a PHP web application built in Laravel 9. It uses Laravel's Sail for portability, so you have a fully Dockerized app (goodbye, system-level dependencies!) that uses MySQL as the database.

### Installing

### The Ticket System

### Part 1: SMS (how does it do that?)