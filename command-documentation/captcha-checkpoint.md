# 🆔 CAPTCHA / Checkpoint

{% hint style="warning" %}
**ADVANCED SECURITY MODULE**\
This module contains advanced options for deploying an anti-bot checkpoint in your server. This is very much a hands-on setup process, and you'll need to double check your work to ensure your server's safety.&#x20;
{% endhint %}



![Example CAPTCHA](https://i.imgur.com/2PsED4y.png)

CAPTCHA is a fantastic tool to protect your server that lets you challenge new users to a visual CAPTCHA code. Solving this code in practice proves they are human, while failing it or being halted by it and kicked indicates they may be a bot. This also lets you create a safe, quarantined space in your server for in the event a bot raid was to occur, all your channels can be protected in advance and the spam bots would end up trapped in your CAPTCHA channel.

## Screenshots + Demo

First off, we believe that if you deploy the CAPTCHA it is important to give newly joining users' information about what they're actually looking at. In our case, we recommend placing an embed in your challenge channel that reads something similar to this one: ![BeeHive CAPTCHA Information](https://i.imgur.com/G5DzHfz.png)

Next, when users join your server after CAPTCHA has been enabled, they will be mentioned and greeted with a challenge that looks like this: ![BeeHive CAPTCHA Challenge](https://i.imgur.com/Zpnv8lk.png)

It is from here they are able to reply in the channel and solve the CAPTCHA, or request a new CAPTCHA image if they cannot understand the one, they were already provided with.

## Command List:

Here are all the commands included in this module:

> `[p]setcaptcha`
>
> > Configure CAPTCHA in your server.

> `[p]setcaptcha allowedretries <number>`
>
> > Set the number of retries or failures allowed before getting kicked.

> `[p]setcaptcha autorole`
>
> > Set the roles to give when passing the captcha.

> `[p]setcaptcha autorole add [roles...]`
>
> > Add a role to give.

> `[p]setcaptcha autorole list`
>
> > List all roles that will be given.

> `[p]setcaptcha autorole remove [roles...]`
>
> > Remove a role to give.

> `[p]setcaptcha channel <text_channel>`
>
> > Set the channel where the user will be challenged.

> `[p]setcaptcha enable <true_or_false>`
>
> > Enable or disable CAPTCHA security.

> `[p]setcaptcha logschannel <text_channel>`
>
> > Set a channel where logs are sent for user failure, success, etc.

> `[p]setcaptcha temprole <temporary_role_or_'none'>`
>
> > Give a temporary role when initilalizing the captcha challenge.

> `[p]setcaptcha timeout <time_in_minutes>`
>
> > Set the timeout before the bot kick the user if the user doesn't answer.

> `[p]setcaptcha type <type_of_captcha>`
>
> > Change the type of CAPTCHA challenge.
