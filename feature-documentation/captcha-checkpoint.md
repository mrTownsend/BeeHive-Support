---
description: >-
  A way to secure your server against user-bots and hacked accounts running
  en-masse scripts. Bot's can't post bullshit in your chat if they can't ever
  see the chat to begin with...
cover: ../.gitbook/assets/infrastructure-transition-containers-blog-banner.png
coverY: -86.84824902723736
---

# 🆔 CAPTCHA

{% hint style="warning" %}
**ADVANCED SECURITY MODULE**\
This module contains advanced options for deploying an anti-bot checkpoint in your server. This is very much a hands-on setup process, and you'll need to double check your work to ensure your server's safety.
{% endhint %}

![Example CAPTCHA](https://i.imgur.com/2PsED4y.png)

CAPTCHA is a fantastic tool to protect your server that lets you challenge new users to a visual CAPTCHA code. Solving this code in practice proves they are human, while failing it or being halted by it and kicked indicates they may be a bot. This also lets you create a safe, quarantined space in your server for in the event a bot raid was to occur, all your channels can be protected in advance and the spam bots would end up trapped in your CAPTCHA channel.

## Screenshots + Demo

First off, we believe that if you deploy the CAPTCHA it is important to give newly joining users' information about what they're actually looking at. In our case, we recommend placing an embed in your challenge channel that reads something similar to this one: ![BeeHive CAPTCHA Information](https://i.imgur.com/G5DzHfz.png)

Next, when users join your server after CAPTCHA has been enabled, they will be mentioned and greeted with a challenge that looks like this:&#x20;

![BeeHive CAPTCHA Challenge](https://i.imgur.com/Zpnv8lk.png)

It is from here they are able to reply in the channel and solve the CAPTCHA, or request a new CAPTCHA image if they cannot understand the one, they were already provided with.

## Command List:

Here are all the commands included in this module:

<details>

<summary>[p]setcaptcha</summary>

Main CAPTCHA configuration menu

</details>

<details>

<summary>[p]setcaptcha allowedretries</summary>

Requires: `a Number`

Set the number of retries or failures allowed before the user is automatically kicked from the server.&#x20;

</details>

<details>

<summary>[p]setcaptcha autorole</summary>

Set the roles to give when passing the CAPTCHA. Run this command to see autorole role management options.&#x20;

</details>

<details>

<summary>[p]setcaptcha channel</summary>

Requires: `a channel mention`

Set the channel where the user will be presented with the captcha challenge when they join the server. You typically want to make this a channel that only new joins can see, but you also want it set up so that new joins can't see any of your other channels until they solve the CAPTCHA.&#x20;

</details>

<details>

<summary>[p]setcaptcha enable</summary>

Requires: `True` or `False`

Enable or disable CAPTCHA security. Note that sometimes this can be done strategically to reduce bot impact.&#x20;

</details>

<details>

<summary>[p]setcaptcha logschannel</summary>

Requires: `a channel mention`

Set a channel where logs are sent for user failures, successes, refreshes, kicks, etc. You want other moderators to be able to see this channel so they can help users who are struggling with the CAPTCHA, unless you operate like an absolute savage.&#x20;

</details>

<details>

<summary>[p]setcaptcha timeout</summary>

Requires: `time in minutes`

Set the timeout before BeeHive will automatically kick a user if they haven't tried to answer the CAPTCHA. This is an important setting as this helps prevent stale bots from getting stuck in the checkpoint channel.&#x20;

</details>

<details>

<summary>[p]setcaptcha type</summary>

Requires: `an option`

Three different security modes. Three different difficulties.&#x20;

For **Basic, Text-Only Captchas**, say `text`

For **Gen 1 Image Captchas**, say `wheezy`

For **Gen 2 Image Captchas** say `image` (these are the most advanced CAPTCHA's we offer)

</details>
