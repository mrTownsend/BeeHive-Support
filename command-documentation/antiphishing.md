---
description: Part of AutoMod, this module is enabled by default silently.
---

# 🔗 AntiPhishing

{% hint style="success" %}
**ENABLED BY DEFAULT**\
No need to worry, BeeHive's already scanning for bad links as soon as it joins your server! Configuration is optional, but still nice.
{% endhint %}

**AntiPhishing** helps combat Discord Phishing Scams by watching for known malicious links and removing them. Configurable options include where to send mod alerts when bad links get caught, and where to alert your **users** about malicious phishing attempts. Some server owners prefer to configure this to a public chat channel where the most users are typically going to be, others prefer to set it to some sort of announcement channel. Do as you please.&#x20;

## Screenshots

> Help Menu

![Help Menu](https://i.imgur.com/72XbVrj.png)

> Malicious Link Detected Message

![Detection Message](https://i.imgur.com/2zqKBza.png)

> Default Settings

![Default Settings](https://i.imgur.com/Vi56iCt.png)

## Commands

> \[p]antiphishing settings
>
> > Shows current AntiPhishing settings. AntiPhishing is always enabled by default.

> \[p]antiphishing log <#channel>
>
> > Lets you set a logging channel for phishing reports

> \[p]antiphishing publicalerts <#channel>
>
> > Lets you specify a channel so regular server members can be alerted to malicious users in the server. Helps bring awareness to users who are DMing malicious links and things of that sort. I recommend setting this to your server's "general" chat channel.
