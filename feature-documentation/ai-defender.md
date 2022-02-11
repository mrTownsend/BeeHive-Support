---
description: >-
  AutoMod Features that are powered by AI or various other forms of Machine
  Learning
cover: ../.gitbook/assets/ai__customer_service_2000x500.jpg
coverY: 0
---

# 🛡 AI Defender

Unlike other bots that say they do something and then either don't do it or do it poorly, what you're about to read here is exactly what BeeHive is capable of. After configuration thru the commands below, BeeHive is capable of detecting almost any attack and attempting to mitigate it. Truly, a server guardian that gets smarter with every day it's in your server. BeeHive's AI AutoMod offers the following features. \


* [Invite Detection](ai-defender.md#setup-invite-filter)
* [Raid Detection](ai-defender.md#setup-raider-detection)
* [Join Monitoring](ai-defender.md#undefined)
* [Emergency Mode](ai-defender.md#setup-emergency-mode)
* [Chat and Message Analysis](ai-defender.md#setup-chat-analysis)
* [Staff Alerting On-Demand ](ai-defender.md#setup-alert)



> If you're looking for non-setup and more user-facing commands regarding the Defender, [take a look starting here](ai-defender.md#defender-an-overview) for commands and more information.&#x20;

## Before We Start

> Before you're able to setup `AI AutoMod`, you need to make sure that you have set your `Administrator` and `Moderator` roles, which we detail how to do right here: [#initial-setup](../#initial-setup "mention")

> As long as you have those two things done, then we're ready to start setting up.&#x20;

## Setup - General

> First, we need to choose a channel to send the AutoMod alerts to. This is a very important channel - you'll want to send all of BeeHive's automatic logs to it so it's a one-stop place for you to check up on what the bot's been doing.&#x20;

Command: `[p]dset general notifychannel #tagurchannelhere`

> Then, select a role you'd like to get notified if the bot needs to notify someone.&#x20;

Command: `[p]dset general notifyrole @RoleMention`&#x20;

> **Fantastic!!** As long as you were given a **Green Tick** to both of those commands, you've got the core setup.&#x20;

> Let's go ahead and enable the framework with the following

Command: `[p]dset general enable on`

> As long as BeeHive responds with `Defender system activated`, you're ready to proceed. If it does not respond with that, you've already made a mistake. Go back and start from Step 1 to ensure a successful AutoMod setup.&#x20;

{% hint style="info" %}
**SERVERS INTERESTED IN MUTE INTEGRATION**:

If you already have a pre-configured `Muted` role on your server, you can take advantage of it and use that as a tool of punishment for users who violate the AutoMod!

Command: `[p]dset general punishrole @RoleMention`&#x20;
{% endhint %}

> There are additional commands not listed here for the sake of time, but if you're looking for more advanced configuration options you can view all of the general commands with `[p]dset general`

## Setup - Alert

> Run the following command to enable the "Alert" system, a way for users to alert staff that something's going on.&#x20;

Command: `[p]dset alert enable true`

> That's it! Your users can now trigger an alert using `[p]alert`

## Setup - Chat Analysis

{% hint style="danger" %}
**APPROVAL REQUIRED**\
If you'd like to have your server protected comprehensively by Chat Analysis, you'll need to reach out to us for a token. Please either join our support Discord, or request a key from our `Keymaster` by emailing them at `security@beehive.systems`&#x20;
{% endhint %}

Chat Analysis is an **EXTREMELY** powerful moderation tool to make your life as easy as possible.&#x20;

Chat Analysis can be configured to detect any of the following things, at any sensitivity percentage overall:

<details>

<summary>TOXICITY</summary>

`A rude, disrespectful, or unreasonable comment that is likely to make people leave a discussion`

``

Languages Supported: `Arabic, Chinese, Czech, Dutch, English, French, German, Hindi, Hinglish, Indonesian, Italian, Japanese, Korean, Polish, Portuguese, Russian, Spanish`

``

</details>

<details>

<summary>SEVERE_TOXICITY</summary>

`A very hateful, aggressive, disrespectful comment or otherwise very likely to make a user leave a discussion or give up on sharing their perspective. This attribute is much less sensitive to more mild forms of toxicity, such as comments that include positive uses of curse words`

``

Languages Supported: `German, English, Spanish, French, Italian, Portuguese, Russian`

</details>

<details>

<summary>IDENTITY_ATTACK</summary>

`Negative or hateful comments targeting someone because of their identity`

``

Supported Languages: `German, English, Spanish, French, Italian, Portuguese, Russian`

</details>

<details>

<summary>INSULT</summary>

`Insulting, inflammatory, or negative comments towards a person or group of people`

``

Supported Languages: `German, English, Italian, Portuguese, Russian`

</details>

<details>

<summary>PROFANITY</summary>

`Swear words, curse words, or other obscene and/or profane language`

``

Supported Languages: `German, English, Italian, Portuguese, Russian`

</details>

<details>

<summary>THREAT</summary>

`Describes an intention to inflict pain, injury, or violence against an individual or group`

``

Supported Languages: `German, English, Italian, Portuguese, Russian`

</details>

<details>

<summary>SEXUALLY_EXPLICIT</summary>

`Contains references to sexual acts, body parts in a sexual manner, or other lewd content`

``

Supported Languages: `English`

</details>

<details>

<summary>FLIRTATION</summary>

`Pickup lines, complimenting appearences, subtle sexual innuendos, and more fall under this category`

``

Supported Languages: `English`

</details>

> First, let's make sure we're only filtering the categories that we want filtered in our server.&#x20;

Command: `[p]dset commentanalysis attributes TOXICITY SEVERE_TOXICITY IDENTITY_ATTACK INSULT PROFANITY THREAT SEXUALLY_EXPLICIT FLIRATION`

> Remove categories from that command that you do not want to be filtered before running it. By default, running that command will enable detection for every single category it offers.&#x20;

> Next, let's assume we want to only censor regular users and non-staff with the next command:

Command: `[p]dset commentanalysis rank 2`

> If you want ALL users to have their messages censored regardless of role or ranking, run this command instead

Command: `[p]dset commentanalysis rank 1`

> Next, let's configure the action we want Chat Analysis to take. Personally, I recognize that people say things they shouldn't sometimes and that censorship is always very obvious when it's done by a Discord bot, so I recommend just setting Chat Analysis to delete the message.

Command: `[p]dset commentanalysis action none`

Command: `[p]dset commentanalysis deletemessage true`

{% hint style="info" %}
**MUTE ROLE SUPPORT**

If your server utilizes a mute role for automatic punishments and you'd like to enable the usage of it with this module, run`[p]dset commentanalysis action punish` to allow it to punish with that role.&#x20;
{% endhint %}

{% hint style="danger" %}
**TOKEN REQUIRED NOW**

You are not able to complete these last steps until the `Keymaster` has approved your server and assigned you a key. Once you have been assigned a key, proceed from this point forward.&#x20;
{% endhint %}

> Let's go ahead and set your authentication token that you were given by the `Keymaster`.&#x20;

Command: `[p]token yourtokengoeshere`

> You should see a check mark react to your message. If you do not, then the token was not set properly. Try again. If you see a check mark, then you're completely configured for all normal settings. Lastly, let's go ahead and let chat analysis start scanning messages.

Command: `[p]dset commentanalysis enable true`

Done! Chat Analysis is now on the prowl and hunting down things you didn't want in chat. You won't ever have to touch it again unless you prefer to adjust settings in the future. It'll remain fully autonomous.&#x20;



## Setup - Raider Detection

> Raider Detection is the perfect way to handle accounts who join and start sending malicious messages or mass messages in malicious patterns. Let's get this setup real quick. This is the setup that I recommend for the bot, but you're welcome to customize it.&#x20;
>
> Firstly, let's set the action to ban.

Command: `[p]dset raiderdetection action ban`

{% hint style="info" %}
**MUTE ROLE SUPPORT**

Does your server utilize a mute role? You can configure Raider Detection to mute the user by assigning the role instead of an autoban! Run the following command.&#x20;

`[p]dset raiderdetection action punish`
{% endhint %}

> Then let's go ahead and make sure staff are exempt.&#x20;

Command: `[p]dset raiderdetection rank 2`

> Now let's set it up to wipe messages if it autobans a raider.&#x20;

Command: `[p]dset raiderdetection wipe 7`

> Now let's enable - default settings work best with this module outside of what I've guided you thru here.&#x20;

Command: `[p]dset raiderdetection enable true`

``

## Setup - Invite Filter

Invite Filtering is a basic need in any organized Discord server, and BeeHive makes it easy to crack down on them. This module's very basic to setup, so let's get to a default or typical setup.&#x20;

> First, let's go ahead and set the action to delete it.&#x20;

Command: `[p]dset invitefilter action none`

{% hint style="info" %}
**MUTE ROLE SUPPORT**

Does your server prefer to punish using a mute for this? \
`[p]dset invitefilter action punish`
{% endhint %}

``

> Next, let's allow people to share the server's own invite inside the server just to be safe.&#x20;

Command: `[p]dset invitefilter excludeowninvites true`

> Next, let's make sure that staff can post invites and whatnot but regular users cannot.&#x20;

Command: `[p]dset invitefilter rank 2`

> Fantastic. One more thing, let's enable it.&#x20;

Command: `[p]dset invitefilter enable true`

``

``

## Setup - Emergency Mode

Emergency mode is a very unique module because in the event of a server emergency where staff members are not around to intervene, BeeHive can temporarily weaponize your trusted users and allow them to deal with incoming threats until your staff return, at which point they'd be automatically demoted and everything returned to normal. It's pretty smart, if I do say so myself. Let's get it set up real quick. These are the config options I recommend for safest operation considering this exposes some permissions to members who typically wouldn't have them.&#x20;

> First, let's set the inactivity limit to 5 minutes. If a staff member can't respond and help take care of an issue within 5 minutes then let's empower them to do it themselves.&#x20;

Command: `[p]dset emergency minutes 5`

> Then, let's allow trusted users to use voteout and silence as non-destructive protective tools.&#x20;

Command: `[p]dset emergency modules voteout silence`

Done! That's all there is to Emergency Mode.&#x20;



## Setup - Join Monitoring

Join Monitoring watches for suspicious or extreme join patterns, and can automatically raise the server security level accordingly to help fend off possible attackers. Let's get it setup with a good setup. Obviously you are welcome to customize it in the future as you get more comfortable with the bot. Below are assorted commands to run depending on the size of your server, to better get a general idea of how Join Monitoring will help you.&#x20;



<details>

<summary><code>For Servers Between 1 and 250 Members</code></summary>

The following settings are appropriate for your size server on average. Please run the following commands one-after-another to complete Join Monitoring setup.&#x20;



`[p]dset joinmonitor verificationlevel 4`

`[p]dset joinmonitor minutes 1`

`[p]dset joinmonitor users 8`

`[p]dset joinmonitor notifynew 72`

`[p]dset joinmonitor enable true`

</details>

<details>

<summary><code>For Servers Between 250 and 1</code>,<code>000 Members</code></summary>

The following settings are appropriate for your size server on average. However, depending on your statistics as your server has started to grow you may need to adjust some of these to prevent false positives. It's not as if people are getting autobanned from the wrong values, but it may frustrate you to frequently see the server on red alert because of join spikes.&#x20;



`[p]dset joinmonitor verificationlevel 4`

`[p]dset joinmonitor minutes 5`

`[p]dset joinmonitor users 73`

`[p]dset joinmonitor enable true`

</details>

<details>

<summary><code>For Servers Over 1,000 Members</code></summary>

At this point, over 1,000 member's we're going to assume the ten and ten rule. 10 people joining a minute for ten minutes is a larger-scale raid, we find that borders right on the edge between a vote wave and accuracy. Let's go thru the commands.&#x20;



`[p]dset joinmonitor verificationlevel 4`

`[p]dset joinmonitor minutes 10`

`[p]dset joinmonitor users 100`

`[p]dset joinmonitor enable true`

``

</details>

That's all for join monitoring setup!

## Defender - An Overview

Defender is an extremely powerful moderation tool that lets you utilize our "iwrnt" system to digitally sign and receive requests for user messages in a way that is both safe, and end-to-end encrypted. Here are the main user-facing commands of Defender.&#x20;

<details>

<summary><code>[p]defender emergency</code></summary>

Requires: `on` or `off`

Upon activation, staff will be pinged and any module that is set to be active in emergency mode will be made available to your helper roles.&#x20;

</details>

<details>

<summary><code>[p]defender freshmeat</code></summary>

Shows you a list of the most recent server joins, for inspection or other reference.&#x20;

</details>

<details>

<summary><code>[p]defender identify</code></summary>

Requires: `mention` or `ID`

Provide information about what rank Defender categorizes a user as well as see how many messages Defender has monitored from them total.&#x20;

</details>

<details>

<summary><code>[p]defender memberranks</code></summary>

Shows how many users are categorized in BeeHive's internal ranks.&#x20;

</details>

<details>

<summary><code>[p]defender messages</code></summary>

Utilize the `iwrnt` system to request a user's message history from our database.&#x20;

SubCommands:

`channel` - Shows recent messages of a channel

&#x20;`exportchannel` - Exports recent messages of a channel to a file&#x20;

`exportuser` - Exports recent messages of a user to a file&#x20;

`user` - Shows recent messages of a user

</details>

<details>

<summary><code>[p]defender monitor</code></summary>

Shows the administrative monitor. This lets admins review when other staff members are submitting digital warrants for user messages.&#x20;

</details>

<details>

<summary><code>[p]defender status</code></summary>

Shows the current status of Defender, including settings and configuration information.&#x20;

</details>
