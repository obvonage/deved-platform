---
title: Announcing the Vonage Java SDK v7.0.0
description: Version 7.0.0 of the Vonage Java SDK is now available! This post
  describes the changes and what to expect in the near future.
author: sina-madani
published: true
published_at: 2022-08-15T10:23:51.517Z
updated_at: 2022-08-15T10:23:51.532Z
category: release
tags:
  - java
  - ""
comments: true
spotlight: false
redirect: ""
canonical: ""
outdated: false
replacement_url: ""
---
## Introduction
Since joining Vonage, I have been focusing on improving the [core Java SDK](https://github.com/Vonage/vonage-java-sdk) by paying down some technical debt and ensuring it is up to date with the latest [API specifications](https://developer.vonage.com/api). In this post, I'll explain some of the prominent changes to the SDK and what's on the horizon. Hopefully I can also convince you to upgrade to the latest version 🙂. You can also check out the [release notes for each version on GitHub](https://github.com/Vonage/vonage-java-sdk/releases).

## Security
If I could only give you one reason to upgrade, it would be security. Versions [prior to 6.4.2 are susceptible to over 50 CVE security vulnerabilities](https://mvnrepository.com/artifact/com.vonage/client) due to outdated dependencies (mainly Jackson, which is what we use for JSON serialisation).

Prior to v7.0.0, the SDK depended on a small in-house library for working with JWTs. This library [hadn't been updated for a few years and was still under the Nexmo brand](https://mvnrepository.com/artifact/com.nexmo/jwt). It has now moved to the Vonage org on GitHub(https://github.com/Vonage/vonage-jwt-jdk) and [Maven Central](https://search.maven.org/artifact/com.vonage/jwt), receiving a rebrand for consistency along with dependency updates for security.

## Bug fixes
TODO

## Features
TODO

## Roadmap
TODO

## Signing off
That's all for now! If you do come across any issues or have suggestions for enhancements, feel free to [raise an issue on GitHub](https://github.com/Vonage/vonage-java-sdk/issues), reach out to us on [Twitter](https://twitter.com/VonageDev) or drop by our [Community Slack](https://developer.vonage.com/community/slack). I hope that you have a great experience of using the Vonage APIs with the latest version of the Java SDK!
