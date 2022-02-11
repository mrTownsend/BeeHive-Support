# Email verification

{% hint style="warning" %}
**STORES USER EMAIL ADDRESS**\
This module store's the user's email.&#x20;
{% endhint %}

**Email Verification** is an alternate form of CAPTCHA that requires a little bit more advanced setup to function properly. However, it is a nearly un-bypassable human verification method considering it's adaptation rate.

# Screenshots and Demo

User-Facing Verify Command:

![Verify Command](https://i.imgur.com/e8JBbsx.png)

# Commands

**Configuratiion Commands**

>[p]verifyset domain
>>Restrict verification to a specific email domain.

>[p]verifyset email
>>Set the email for verification.

>[p]verifyset instructions
>>Instructions for verification setup.

>[p]verifyset logchannel
>>Set the channel for verification logs

>[p]verifyset role
>>Set the role for verification

**User-Facing Commands**

>[p]verify
>>Command for users to submit their email/code for verification.
