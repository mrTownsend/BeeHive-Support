# 🔐 WARDEN

{% hint style="danger" %}
**WARNING - DESTRUCTIVE ACTIONS POSSIBLE**\
WARDEN is an AutoMod module and is capable of executing destructive actions! Be very wary with the rules that you write, and ensure you consider whether or not they could backfire or be abused. WARDEN can take drastic, irreversible actions should there be a mistake and they make be unrecoverable. **PROCEED WITH EXTREME CAUTION AND CONSULT THE DOCUMENTATION HEAVILY WHILE WORKING WITH WARDEN**
{% endhint %}

## Warden

* [An introduction to Warden](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#an-introduction-to-warden)
* [Advanced features](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#advanced-features)
* [Events](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#events)
* [Conditions](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#conditions)
* [Actions](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#actions)
* [Context variables](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#context-variables)
* [Examples](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#examples)

### An introduction to Warden

Warden is the most versatile module that you can find in Defender.\
It allows you to define custom rules by combining a rich set of events, conditions and actions.\
Need to filter messages only in certain channels? Or maybe rename people meeting very specific requirements? Warden got you covered!

#### Rule format: a basic example

Rules can be written in the YAML language and must be structured in a very specific way to be deemed a valid rule.\
Let's start with a basic scenario: you have an innate fear of spiders and you wish to delete any message that mentions them.\
So, without further ado, you add this rule through `[p]defender warden add`

```
name: spiders-are-spooky
rank: 1
event: on-message
if:
  - message-matches-any: ["*spider*"]
do:
  - delete-user-message:
```

And now let's break this down.

* `name` is the name of this rule. Every rule that you add will have an unique name to tell them apart. If you try to add a rule with a name that is already registered a confirmation will be asked before proceeding with overwrite.
* `rank` is the highest rank that this rule will target. As explained in `[p]defender status`, Rank 1 is the highest rank there is. This means that every message that is sent will be targeted, even staff's.
* `event` is _when_ this rule will enter into effect. There are currently 5 [events](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#events) available.\
  A rule can also be defined with multiple events by using a list, example: `[on-message, on-message-edit]`
* `if` is the section where the [conditions](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#conditions) are defined. It supports both basic condition and special condition blocks (we'll get to that later). Every condition (and condition block) must resolve to `true` for the rule's action to be executed.
* `do` is the section where the [actions](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#actions) are defined. Actions are ordered and will be executed in the order you have defined them. If one of them errors out for whatever reason, the action's execution will stop. You can monitor errors in `[p]defender monitor`.

The condition `message-matches-any` takes the message in the _context_ and analyzes its content. Here we have put the word spider surrounded by the wildcard `*`: this means that the word spider simply being present in a message is enough for this condition to pass.

As you may have noticed the action `delete-user-message` doesn't need any parameter: it simply deletes the message in the context.

#### Context

Events have a context. `on-message` for example, will have `message` and `user` (the message author's) available, so any condition and action that is bound to analyze or take action on a message will be allowed to be used in that rule.\
A rule with the condition `message-matches-any` combined with the event `on-user-join` will be rejected: Warden doesn't have a message to analyze in this event, therefore the rule will be simply deemed invalid.\
Depending on the context you will have certain _context variables_ available: there are some special actions that accept these variables in their parameters.\
Example:

```
send-mod-log: "No particular reason: I just really dislike $user."
```

If the user in context is called HairySpider#9999 Warden will post a mod-log entry as

```
No particular reason: I just really dislike HairySpider#9999.
```

#### Defining a more complex rule

This first rule was very basic. What if, instead, we wanted to outright **ban** any user that mentions spiders _OR_ has the word spider in their username/nickname, like our friend HairySpider#9999?\
But, since we are also hindered by a conscience, we want to give new users time to read the #rules channel, so we'll make sure to only punish infidels who are knowingly breaking the rules.\
Finally, the ruling class, staff members, must be free to discuss the enemy of the state without ever getting banned themselves.\
Wow, this got complicated fast huh? Luckily this is where _condition blocks_ come to our aid.

At the time of writing there are three condition blocks:

* `if-any`
* `if-all`
* `if-not`

When a rule is processed, conditions blocks are individually resolved to **true** or **false**.\
For `if-all` to pass, every condition that it contains must be **true**.\
Every condition contained in `if-not` must resolve to **false**.\
And as you can probably guess by now, a single **true** condition inside `if-any` is enough for it to pass.

```
name: spiders-are-spooky
rank: 1
event: on-message
if:
  - if-any:
     - username-matches-any: ["*spider*"]
     - message-matches-any: ["*spider*"]
     - nickname-matches-any: ["*spider*"]
  - if-not:
     - user-joined-less-than: 2 hours
     - is-staff: true
do:
  - ban-user-and-delete: 1
  - send-mod-log: "Usage of the S word is not welcome in this community. Begone, $user."
```

This is how a condition block is structured: it contains basic conditions. To prevent too much complexity nested condition blocks are not allowed.\
In our case here _if_ the author's of a message has the word spider in their username/nickname _OR_ their message contains the word spider _AND_ at the same time hasn't joined less than **2** hours ago and is also not a staff member _then_ they will be banned and all the messages sent by them in the last day will be deleted.\
Additionally, a mod-log entry for this last ban will be posted, condemning the author of this unspeakable crime for everyone to see.\
Our Orwellian quest of never ever allowing the S word to be used in our community again is now completed.

As you can see condition blocks are a nice to have when you need a slightly more complex logic but remember that they're not mandatory, you can choose to use them only when you truly need to.

#### A quick recap

Rules are triggered when the specified event takes place.\
The user's rank (if applicable, not every event has a user context) will be matched against the one defined in the rule.\
After that, the conditions will be evaluated and finally, if the conditions have passed, Warden will start processing the actions one by one.\
If any of the actions fail execution will stop and the error will be reported in `[p]defender monitor`.\
Rules can have an order of execution. See [advanced features](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#advanced-features) for more informations.

#### Final notes

Rules give you complete freedom by design.\
In that last example even _trusted roles_ and _helper roles_ aren't protected: if they pass the conditions they will be targeted by the actions.\
Be careful when you design the logic of a rule with drastic actions such as bans. The parameter **rank** is there to help you protect categories of users: trusted users, helper users and staff will always be immune if you set the rank to at least level 2.

### Advanced features

#### Execution order

The rules that you add do not have any guaranteed order of execution. You can change this, however, for example to make sure that a particular rule is always executed first (or after all your other ordered rules): this is done with the optional `priority` parameter.\
Priority can be a number between 1 and 999. When something happens, Defender gathers all the rules that you have added for that particular event and reorders them based on the priority parameter. Rules that lack a priority parameter will always be executed last, in an unordered manner.\
Defender will not stop you from defining multiple rules with the same priority: this is fine for rules that are for different events as they won't "conflict" with each other, however be mindful of that when you set up prioritized rules for a single event.\
This is how you define a rule with priority:

```
name: always-first
rank: 1
priority: 1
event: on-message
if:
  - message-matches-any: ["*"]
do:
  - send-to-monitor: "I'm 1st!"
```

#### Heat level

Conditions give you a way to have your rules only execute under very specific conditions. However, what if, for example, we want our rule to execute after an user triggered some other rule a defined amount of times? That's where the heat level system comes in handy.\
Imagine that each user has an invisible bar, just like an health bar in videogames, where their heat level is being tracked:

`[____________________]`

This bar can contain a maximum of 100 heat points. Normally it is empty. Through rule actions you can assign points to this bar. Each point has a definite lifetime and you decide how much that is:

`add-user-heatpoint: 10s` (also accepts minutes and hours. Up to 24 hours)\
`[X___________________]`

After 10 seconds have elapsed, the bar will go back to being empty. You can also assign multiple heatpoints at once with the same lifetime:

`add-user-heatpoints: [5, 10m]`\
`[XXXXX_______________]`

And in case you want to empty the bar:\
`empty-user-heat:`

But how do you check for heat points in rules? With conditions:\
`user-heat-is: 5` or\
`user-heat-more-than: 2`

You may want to first assign heatpoints and _then_, in a separate rule, check the heat level. Since rules are normally executed in an unordered manner, remember to use the [priority parameter](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#execution-order) to make sure that your heat-assigning rules are executed first.\
Channels also have heat levels and they work the same way as users'. You can find all the related conditions and actions in the sections below plus some handy [examples](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#examples) at the bottom.

#### Custom heat level

Assuming you've read the previous section before this (please do so!), you may be wondering if you're limited to only two heat levels, per-channel and per-user. What if you want separate heat levels for each different rule, for example?\
This is where _custom_ heat levels can help. They work exactly like channel and user heat levels, except for the fact that you can assign to them the name you want. A few [context variables](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#context-variables) (specifically the IDs and rule name) are also available for extra flexibility.\
Custom heat levels are extremely versatile. To demonstrate the variety of use cases they can used for we'll build a rule specific cooldown using them.

```
rank: 2
name: trigger-with-cooldown
event: on-message
if:
  - message-matches-any: ["hello"]
  - custom-heat-is: ["$rule_name", 0]
do:
  - add-custom-heatpoint: ["$rule_name", 5 minutes]
  - send-message: [$channel_id, "hello $user_mention"]
```

This rule can only be triggered every 5 minutes: as you can see in the actions a single custom heatpoint gets added, making the custom heat level "one" for five minutes. This will make the condition `custom-heat-is` "zero" false, not allowing the rule to be run until the heatpoint expires.

#### Periodic rules

Periodic rules are rules that can be set to run automatically every once in a while. Just like manual rules, they run against the entire server userbase: every user that satisfies its conditions will be affected by it. They can be particularly useful for certain needs, such as enforcing name conventions.\
A simple example:

```
rank: 2
name: dehoist-task
event: [periodic]
run-every: 15 minutes
if:
  - username-matches-any: ["!*"]
  - if-not:
      - nickname-matches-any: ["*"]
do:
  - set-user-nickname: "no hoisting"
```

This rule will run every 15 minutes (as defined by the mandatory `run-every` parameter) and will rename every user that has a name starting with an exclamation mark.\
Keep in mind that a bot restart will reset the timer: the rule above, for example, will only run 15 minutes after the bot first starts.

#### Conditional actions

Sometimes you may want to carry on different actions according to certain conditions: this is doable by using conditions (or condition blocks) inside the `do` block. Each condition, or condition block, will equal to `true` or `false` and you can control the flow of the rule with `if-true` and `if-false`.

```
# Replies with pong when the user says ping, and viceversa
rank: 1
name: ping-pong
event: [on-message]
if:
  - message-matches-any: ["ping", "pong"]
do:
  - compare: ["$message", "contains-pattern", "ping"]
  - if-true:
      - send-message: [$channel_id, "pong"]
  - if-false:
     - send-message: [$channel_id, "ping"]
```

```
rank: 3
name: filter
event: on-message
if:
  - message-matches-any: ["*bad*", "*words*"]
do:
  - add-custom-heatpoint: ["filter-$user_id", 5 minutes]
  - custom-heat-more-than: ["filter-$user_id", 4] # check if the user triggered this rule 5 or more times
  - if-true: # if they did, ban them
      - ban-user-and-delete: 0
  - delete-user-message: # and proceed to delete the message in any case, ban or not
```

### Events

* `on-message`\
  **Triggered when:** a new message is sent\
  **Available context:** message, user
* `on-message-edit`\
  **Triggered when:** a message is edited\
  **Available context:** message, user
* `on-message-delete`\
  **Triggered when:** a message is deleted\
  **Available context:** message, user
* `on-user-join`\
  **Triggered when:** a new user joins the server\
  **Available context:** user
* `on-user-leave`\
  **Triggered when:** a user leaves the server\
  **Available context:** user
* `on-emergency`\
  **Triggered when:** the server enters a state of emergency, either automatic or manual\
  **Available context:** none
* `manual`\
  **Triggered when:** the rule is manually run by an admin with `[p]defender warden run`\
  **Available context:** user
* `periodic`\
  **Triggered when:** the rule runs periodically, as defined by its own `run-every` parameter. The interval can be set between 5 minutes and 24 hours. See more info about [periodic rules](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#periodic-rules).\
  **Available context:** user

### Conditions

* `message-matches-any`\
  Is `true` if any of the patterns that you list are found in the message's content\
  **Accepts:** A list of patterns. Patterns can make use of wildcards such as `*` and `?`\
  **Context:** `message`
* `message-matches-regex`\
  Is `true` if the regex returns a match against the message's content\
  **Accepts:** A string representing a regular expression. Remember that `\` must be escaped: turn all `\` characters to `\\` when entering the rule.\
  **Context:** `message`
* `message-has-attachment`\
  Will check if the message contains an attachment\
  **Accepts:** A bool (true or false)\
  **Context:** `message`
* `message-contains-url`\
  Will check if the message contains a clickable URL.\
  Note: URLs lacking a protocol (http/https) will not be detected.\
  **Accepts:** A bool (true or false)\
  **Context:** `message`
* `message-contains-invite`\
  Will check if the message contains a standard Discord invite. Invites that belong to the server are ignored.\
  **Accepts:** A bool (true or false)\
  **Context:** `message`
* `message-contains-media`\
  Will check if the message contains any media link (picture or video)\
  **Accepts:** A bool (true or false)\
  **Context:** `message`
* `message-contains-more-than-mentions`\
  Will check if the message contains more than X user mentions.\
  **Accepts:** A number representing the number of mentions\
  **Context:** `message`
* `message-contains-more-than-unique-mentions`\
  Will check if the message contains more than X unique user mentions. As opposed to the non-unique variant the mentioned users are being counted, _not_ just the mentions themselves.\
  **Accepts:** A number representing the number of unique mentions.\
  **Context:** `message`
* `message-contains-more-than-role-pings`\
  Will check if the message contains more than X unique role mentions. Only mentions that result in a ping will be counted.\
  **Accepts:** A number representing the number of role mentions\
  **Context:** `message`
* `message-contains-more-than-emojis`\
  Will check if the message contains more than X emojis. **Important:** emojis with [modifiers](https://en.wikipedia.org/wiki/Emoticons\_\(Unicode\_block\)#Emoji\_modifiers), such as a thumbs up emoji with custom skin color, will be considered 2 separate emojis.\
  **Accepts:** A number representing the number of emojis.\
  **Context:** `message`
* `message-has-more-than-characters`\
  Will check if the message contains more than X characters. Emojis and custom emojis will be considered a single character. Mentions, both users and channels, are counted too.\
  **Accepts:** A number representing the number of characters.\
  **Context:** `message`
* `user-id-matches-any`\
  Is `true` if any of the IDs that you list match the user's ID\
  **Accepts:** A list of IDs (numbers)\
  **Context:** `user`\
  **Example:** `user-id-matches-any: [123456789, 2626262626]`
* `username-matches-any`\
  Is `true` if any of the patterns that you list are found in the user's username\
  **Accepts:** A list of patterns. Patterns can make use of wildcards such as `*` and `?`\
  **Context:** `user`
* `username-matches-regex`\
  Is `true` if the regex returns a match against the user's username\
  **Accepts:** A string representing a regular expression. Remember that `\` must be escaped: turn all `\` characters to `\\` when entering the rule.\
  **Context:** `user`
* `nickname-matches-any`\
  Is `true` if any of the patterns that you list are found in the user's nickname\
  **Accepts:** A list of patterns. Patterns can make use of wildcards such as `*` and `?`\
  **Context:** `user`
* `nickname-matches-regex`\
  Is `true` if the regex returns a match against the user's nickname\
  **Accepts:** A string representing a regular expression. Remember that `\` must be escaped: turn all `\` characters to `\\` when entering the rule.\
  **Context:** `user`
* `user-activity-matches-any`\
  Is `true` if any of the patterns that you list are found in any of the user's activities\
  **Accepts:** A list of patterns. Patterns can make use of wildcards such as `*` and `?`\
  **Context:** `user`
* `user-status-matches-any`\
  Is `true` if any of the statuses that you list are the user's current status\
  **Accepts:** A list of statuses. Can be `online`, `idle`, `dnd` or `offline`\
  **Context:** `user`
* `user-created-less-than`\
  Is `true` if the user's account has been created less than ago.\
  **Accepts:** A timedelta. Or a number, representing the amount of hours to be checked\
  **Context:** `user`\
  **Example:** `user-created-less-than: 2 hours`
* `user-joined-less-than`\
  Is `true` if the user has joined the server less than ago.\
  **Accepts:** A timedelta. Or a number, representing the amount of hours to be checked\
  **Context:** `user`\
  **Example:** `user-joined-less-than: 2 hours`
* `user-has-default-avatar`\
  Will check if the user has a default Discord avatar\
  **Accepts:** A bool (true or false)\
  **Context:** `user`\
  **Example:** `user-has-default-avatar: true` will be `true` if the profile picture is a default one
* `user-has-sent-less-than-messages`\
  Will check if the user has sent less than X messages in the server. **Important:** Defender, by default, counts how many messages a user sends in a server. This does _not_ check the real total number of messages a user has sent in a server since its creation.\
  **Accepts:** A number representing the number of messages\
  **Context:** `user`
* `user-is-rank`\
  Will check if the user is the specified rank. This condition allows for _rank-specific_ Warden rules and grants more granular control compared to the standard `rank` parameter, which merely indicates the maximum rank a rule will have effect on.\
  **Accepts:** A number representing the rank the user must belong to\
  **Context:** `user`
* `channel-matches-any`\
  Will check if the message was sent in any of the specified channels.\
  **Accepts:** A list of channel names or IDs\
  **Context:** `message`\
  **Example:** `channel-matches-any: [general, testing, 483223782]` will be `true` if the message was sent in any of these channels
* `category-matches-any`\
  Will check if the channel in which the message was sent belongs to any of the specified categories.\
  **Accepts:** A list of category names or IDs\
  **Context:** `message`
* `channel-is-public`\
  Will check if the message was sent in a publicly viewable channel.\
  **Accepts:** A bool (true or false)\
  **Context:** `message`
* `in-emergency-mode`\
  Will check if the server is in emergency mode, either manual or automatic\
  **Accepts:** A bool (true or false)\
  **Context:** Any
* `user-has-any-role-in`\
  Is `true` if the user belongs to any of these roles in the list\
  **Accepts:** A list of role names or IDs\
  **Context:** `user`\
  **Example:** `user-has-any-role-in: [12345678, spider-fighter]` will be `true` if the user belongs to any of these roles
* `is-staff`\
  Will check if the user is a staff member\
  **Accepts:** A bool (true or false)\
  **Context:** `user`
* `is-helper`\
  Will check if the user is a Defender helper\
  **Accepts:** A bool (true or false)\
  **Context:** `user`
* `user-heat-is`\
  Is `true` if the user has the specified [heat level](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#heat-level)\
  **Accepts:** A number between 0 and 100\
  **Context:** `user`
* `user-heat-more-than`\
  Is `true` if the user exceeds the specified [heat level](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#heat-level)\
  **Accepts:** A number between 0 and 100\
  **Context:** `user`
* `channel-heat-is`\
  Is `true` if the channel has the specified [heat level](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#heat-level)\
  **Accepts:** A number between 0 and 100\
  **Context:** `message`
* `channel-heat-more-than`\
  Is `true` if the channel exceeds the specified [heat level](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#heat-level)\
  **Accepts:** A number between 0 and 100\
  **Context:** `message`
* `custom-heat-is`\
  Is `true` if the [custom heat](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#custom-heat-level) has the specified level\
  **Accepts:** A list with two elements: the custom heat level name and a number between 0 and 100. Rule name and IDs [context variables](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#context-variables) are available for the name.\
  **Context:** `Any`
* `custom-heat-more-than`\
  Is `true` if the [custom heat](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#custom-heat-level) exceeds the specified level\
  **Accepts:** A list with two elements: the custom heat level name and a number between 0 and 100. Rule name and IDs [context variables](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#context-variables) are available for the name.\
  **Context:** `Any`
* `compare`\
  Compares two values. Supports a variety of operators, textual and numeric. The comparison is case sensitive. This is most useful when used in conjunction with context variables. Using numeric operators with non-numeric values will raise an error.\
  _Supported operators:_ `==`, `!=`, `contains`, `contains-pattern`, `>=`, `<=`, `<`, `>`\
  `contains-pattern` supports a pattern, just like the condition `message-matches-any` and it's case insensitive.\
  **Example:**

```
- compare: [abc, "==", abc] # True
- compare: [2, "<", 1] # False

- var-assign: [value1, "I like bots"]
- var-assign: [value2, "bots"]
- compare: [$value2, "contains", $value1] # True. Notice the $ symbol which denotes the use of context variables.
```

**Context:** `Any`

### Actions

* `send-message`\
  Will send a message to the defined destination. Message editing is also supported. The message may include an embed. Please refer to the [examples](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#sending-a-message) to understand its use.\
  **Accepts:** A map with the destination, the message's content and the embed. Or a list with 2 elements, destination and message content\
  **Context:** `Any`
* `set-user-nickname`\
  Will change the nickname of a user\
  **Accepts:** A string representing the new nickname to set. Supports context variables.\
  **Context:** `user`
* `delete-user-message`\
  Will delete the user's message\
  **Accepts:** Nothing\
  **Context:** `message`
* `add-roles-to-user`\
  Will assign the listed roles to the user\
  **Accepts:** A list of roles (names / IDs)\
  **Context:** `user`
* `remove-roles-from-user`\
  Will remove the listed roles from the user\
  **Accepts:** A list of roles (names / IDs)\
  **Context:** `user`
* `ban-user-and-delete`\
  Will ban the user from the server and delete X days worth of messages\
  **Accepts:** A number representing the days worth of messages to delete\
  **Context:** `user`
* `kick-user`\
  Will kick the user from the server\
  **Accepts:** Nothing\
  **Context:** `user`
* `softban-user`\
  Will kick the user from the server and delete 1 days worth of messages\
  **Accepts:** Nothing\
  **Context:** `user`
* `punish-user`\
  Will assign to the user the "punish role" that has been set in Defender\
  **Accepts:** Nothing\
  **Context:** `user`
* `punish-user-with-message`\
  Will assign to the user the "punish role" and deliver the "punish message" that have been set in Defender (see `[p]dset general`)\
  **Accepts:** Nothing\
  **Context:** `message`
* `notify-staff`\
  Will send a message to staff's notification channel. The message may include an embed. Please refer to the [examples](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#notifying-the-staff) to understand its use.\
  **Accepts:** A string representing the message to send. Or a map that describes the embed and all the behaviors of the notification.\
  **Context:** Any
* `send-mod-log`\
  Will create a new mod-log case of the last expel action issued by the rule\
  **Accepts:** A string representing the reason of the expel action. Supports context variables.\
  **Context:** Any
* `set-channel-slowmode`\
  Will set (or deactivate) slowmode of the channel in the context's message\
  **Accepts:** A string representing the slowmode time. Some examples: '5 seconds', '2 minutes', '4 hours'. '0 seconds' will deactivate slowmode.\
  **Context:** `message`
* `enable-emergency-mode`\
  Will toggle emergency mode\
  **Accepts:** A bool (true or false), representing whether emergency mode should be enabled or disabled\
  **Context:** Any
* `send-to-monitor`\
  Will send a message to Defender's monitor, `[p]defender monitor`\
  **Accepts:** A string representing the message to send. Supports context variables.\
  **Context:** Any
* `get-user-info`\
  Gets an attribute from an arbitrary user and assigns it to a variable. Many of the [standard discord.py Member attributes](https://discordpy.readthedocs.io/en/latest/api.html#discord.Member) are supported.\
  Additionally, the attributes\
  `is_staff` - `is_helper` with return value `true` or `false`\
  `rank` which returns the rank's number\
  `message_count` which returns the number of messages recorded by Defender\
  are available.\
  If the user cannot be found an error will be raised.\
  **Accepts:** The ID of a user (context variables are supported) and a map with the attribute to fetch as the value and the target variable as the key.\
  **Context:** Any\
  **Example:**

```
- get-user-info: [$user_id, {joined: joined_at, created: created_at}]
- send-message: [$channel_id, "Your join date is $joined. Your account creation date is $created."]

# Long form
- get-user-info:
    id: 123456789
    mapping:
       joined: joined_at
       created: created_at
       staff: is_staff
       helper: is_helper
```

* `no-op`\
  Does nothing. Useful only for testing a rule's conditions with `[p]defender warden run`\
  **Accepts:** Nothing\
  **Context:** Any
* `exit`\
  Interrupts the rule's processing, not carrying on any further action. This is useful when used in conjunction with conditional actions.\
  **Accepts:** Nothing\
  **Context:** Any
* `add-user-heatpoint`\
  Adds a single heat point with the specified lifetime to the user's [heat level](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#heat-level)\
  **Accepts:** A string representing the heat point's lifetime. Some examples: '5 seconds', '2 minutes', '4 hours'\
  **Context:** `user`\
  **Example:** `add-user-heatpoint: 5 seconds` `add-user-heatpoint: 1m`
* `add-user-heatpoints`\
  Adds multiple heat points with the specified lifetime to the user's [heat level](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#heat-level)\
  **Accepts:** A list containing two elements: the amount of heatpoints to assign (1-100) and a string representing the heat points' lifetime. Some examples: '5 seconds', '2 minutes', '4 hours'\
  **Context:** `user`\
  **Example:** `add-user-heatpoints: [10, 5 seconds]` `add-user-heatpoints: [2, 1m]`
* `add-channel-heatpoint`\
  Adds a single heat point with the specified lifetime to the channel's [heat level](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#heat-level)\
  **Accepts:** A string representing the heat point's lifetime. Some examples: '5 seconds', '2 minutes', '4 hours'\
  **Context:** `message`\
  **Example:** `add-channel-heatpoint: 5 seconds` `add-channel-heatpoint: 1m`
* `add-channel-heatpoints`\
  Adds multiple heat points with the specified lifetime to the channel's [heat level](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#heat-level)\
  **Accepts:** A list containing two elements: the amount of heatpoints to assign (1-100) and a string representing the heat points' lifetime. Some examples: '5 seconds', '2 minutes', '4 hours'\
  **Context:** `message`\
  **Example:** `add-channel-heatpoints: [10, 5 seconds]` `add-channel-heatpoints: [2, 1m]`
* `add-custom-heatpoint`\
  Adds a single heat point with the specified lifetime to the [custom heat level](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#custom-heat-level)\
  **Accepts:** A list containing two elements: a string representing the custom heat level name and a string representing the heat point's lifetime. Some examples: '5 seconds', '2 minutes', '4 hours'. Rule name and IDs [context variables](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#context-variables) are available for the name.\
  **Context:** `Any`\
  **Example:** `add-custom-heatpoint: ["filter-$user_id", "5 seconds"]`
* `add-custom-heatpoints`\
  Adds multiple heat points with the specified lifetime to the [custom heat level](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#custom-heat-level)\
  **Accepts:** A list containing three elements: a string representing the custom heat level name, a number representing the amount of heat points to add and a string representing the heat points lifetime. Some examples: '5 seconds', '2 minutes', '4 hours'. Rule name and IDs [context variables](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#context-variables) are available for the name.\
  **Context:** `Any`\
  **Example:** `add-user-heatpoints: ["$rule_name", 10, 5 seconds]`
* `empty-user-heat`\
  Sets the user's [heat level](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#heat-level) to 0\
  **Accepts:** Nothing\
  **Context:** `user`
* `empty-channel-heat`\
  Sets the channel's [heat level](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#heat-level) to 0\
  **Accepts:** Nothing\
  **Context:** `message`
* `empty-custom-heat`\
  Sets the specified [custom heat level](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#custom-heat-level) to 0\
  **Accepts:** A string representing the [custom heat level](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#custom-heat-level) name. Rule name and IDs [context variables](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#context-variables) are available for the name.\
  **Context:** `Any`
* `issue-command`\
  Will issue a bot command as if it was issued by the specified user. The specified user must be the rule's author. If no destination is specified issuing a command in a `message` context will make it issue in the same channel as the message's; in any other case the command will be issued in Defender's notification channel.\
  **Accepts:** A list with two elements: a number representing the ID of the rule's author and a string representing the command to issue. The command must not contain the prefix. [Context variables](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#context-variables) are supported. The long form of this action also supports a destination.\
  **Context:** `Any`\
  **Example:**

```
- issue-command: [2626262626, "ban $user_id User has been banned by Warden's rule $rule_name"]
- issue-command:
   issue_as: 2626262626 # User ID
   command: ping # Command
   destination: 123456 # Channel ID
```

* `delete-last-message-sent-after`\
  Will delete the last message sent through a Warden action after the specified time. The time must be between 1 and 15 minutes.\
  **Accepts:** A string representing the time to wait before deleting the message\
  **Context:** `Any`\
  **Example:** `send-message: [$channel_id, I'm going to disappear in 5 seconds!]`\
  `delete-last-message-sent-after: 5 seconds`

#### Variables related actions

* `var-assign`\
  Assigns a value to a variable.

```
- var-assign: [my_var, 123]
- var-assign:
    var_name: my_var
    value: 123
    evaluate: false # (optional, if true any context variable contained in value will be evaluated)
# The variable my_var will be equal to 123
```

* `var-assign-random`\
  Assigns a random value to a variable, picking one from a list of choices. Supports weighted choices

```
# A simple, non-weighted choice
- var-assign-random: [my_var, [apple, banana, pear]]
- var-assign-random:
    var_name: my_var
    choices: [apple, banana, pear]
    evaluate: false # (optional, if true any context variable contained in value will be evaluated)
# A weighted choice
- var-assign-random:
    var_name: my_var
    choices:
       apple: 10
       banana: 1
       pear: 2
    evaluate: false # (optional, if true any context variable contained in value will be evaluated)
```

* `var-split`\
  Splits a variable into parts, using separator as the delimiter, and assigns the parts to different variables. If there are more parts than provided variables, the remaining parts will be discarded. If there are fewer parts than provided variables, an empty string will be assigned to the remaining variables. Maximum splits can be specified, by default there is no limit.

```
- var-assign: [fruit, "apple pear banana tomato"]

# fruit1 will contain "apple", fruit2 "pear", etc. Splits are done left to right.
- var-split: [fruit, " ", [fruit1, fruit2, fruit3, fruit4]]

# As before fruit1 will contain "apple", fruit2 "pear" but the remaining parts will be lost
- var-split: [fruit, " ", [fruit1, fruit2]]

# A max_split of 1 is specified, fruit1 will still be "apple", but fruit2 will be "pear banana tomato"
- var-split: [fruit, " ", [fruit1, fruit2], 1]
- compare: ["$fruit2", "==", "pear banana tomato"] # True!

# Same as before, but fruit3 will be an empty string: only one split has been done, and there's nothing left to assign to fruit3.
- var-split: [fruit, " ", [fruit1, fruit2, fruit3], 1]
- compare: ["$fruit1", "==", "apple"] # True!
- compare: ["$fruit3", "==", ""] # True!

# Long form:
- var-split:
    var_name: fruit
    separator: " "
    split_into: [fruit1, fruit2, fruit3]
    max_split: 1 # (optional)
```

* `var-slice`\
  Slices a variable according to the indices (the position of the characters) that are being passed. If a variable name is passed with slice\_into the original variable will be left untouched. The optional parameter step, found in Python's string slicing, can also be passed. Remember than indices start from 0, not 1.

```
- var-assign: [my_var, "abcdefgh"]
- var-slice: [my_var, 0, 2, sliced]
- compare: ["$sliced", "==", "ab"] # True!

- var-slice: [my_var, 0, 4]
- compare: ["$my_var", "==", "abcd"] # True!

- var-assign: [my_var, "abcdefgh"]
# Long form
- var-slice:
    var_name: my_var
    index: 0
    end_index: 99
    slice_into: sliced
    step: 2
- compare: ["$sliced", "==", "aceg"] # True!
```

* `var-replace`\
  Modifies a variable with all the occurrences of strings replaced by substring. Strings can be a single value or a list of values.

```
- var-assign: [my_var, "I like apples a lot"]
- var-replace: [my_var, a, 4]
- compare: ["$sliced", "==", "I like 4pples 4 lot"] # True!

- var-assign: [my_var, "I like apples a lot"]
- var-replace: [my_var, [a, p], x]
- compare: ["$sliced", "==", "I like xxxles x lot"] # True!

# Long form
- var-replace:
    var_name: my_var
    strings: a # Can also be a list, as shown above
    substring: b
```

* `var-transform`\
  Modifies a variable depending on the "operation" that it's being requested. Supported operations are capitalize, lowercase, reverse, uppercase, title.

```
- var-assign: [my_var, "I like apples A LOT"]
- var-transform: [my_var, lowercase]
- compare: ["$my_var", "==", "i like apples a lot"] # True!

- var-transform: [my_var, uppercase]
- compare: ["$my_var", "==", "I LIKE APPLES A LOT"] # True!

- var-assign: [my_var, "I like apples a lot"]
- var-transform: [my_var, title] # Capitalizes the first letter of each word
- compare: ["$my_var", "==", "I Like Apples A Lot"] # True!

- var-assign: [my_var, "two words"]
- var-transform: [my_var, capitalize] # Only capitalizes the first letter
- compare: ["$my_var", "==", "Two words"] # True! 

# Long form
- var-transform:
    var_name: my_var
    operation: uppercase
```

### Deprecated syntax

The following syntax is deprecated: while there are currently no plans to remove old Warden syntax it is recommended that you switch to the recommended alternatives as soon as possible. It is not possible to create new rules with deprecated syntax.

`send-dm`, `dm-user`, `send-to-channel`, `send-in-channel` replaced by `send-message`\
`notify-staff-and-ping`, `notify-staff-with-embed` replaced by `notify-staff`

### Context variables

For more informations see [context](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#context)

* `$rule_name`\
  The rule's name
* `$notification_channel_id`\
  The notification channel ID for the server
* `$guild`\
  The guild's name
* `$guild_id`\
  The guild's ID
* `$guild_icon_url`\
  The guild's icon url
* `$guild_banner_url`\
  The guild's banner url
* `$user`\
  The user's name + the discriminator
* `$user_name`\
  The user's name
* `$user_id`\
  The user's ID
* `$user_mention`\
  The user's mention
* `$user_avatar_url`\
  The user avatar's url
* `$user_nickname`\
  The user's nickname. "None" if not set.
* `$user_created_at`\
  The date and time in which the user created the account.
* `$user_joined_at`\
  The date and time in which the user joined the server.
* `$user_heat`\
  The user's [heat level](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#heat-level)
* `$message`\
  The raw message content. Mentions are automatically escaped.
* `$message_clean`\
  The message content. Mentions are transformed into the way the client shows it.
* `$message_id`\
  The message's ID.
* `$message_created_at`\
  The date and time in which the message was created.
* `$message_link`\
  The URL pointing to the message
* `$attachment_filename`\
  The message attachment's filename, if any.
* `$attachment_url`\
  The message attachment's url, if any.
* `$channel`\
  The channel's name in the form of #channel\_name
* `$channel_name`\
  The channel's name
* `$channel_id`\
  The channel's ID
* `$channel_mention`\
  The channel's mention
* `$channel_category`\
  The channel category's name, if any
* `$channel_category_id`\
  The channel category's id, if any
* `$channel_heat`\
  The channel's [heat level](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#heat-level)

### Examples

#### Repository

If you're looking for ready examples or useful templates to start working from check out [the dedicated repository](https://github.com/Twentysix26/warden-rules). Contributions are welcome!

#### Sending a message

The following rule is about `send-message`. This action can be used to send messages, which may include an embed, to a variety of destinations. It supports context variables for the destination (edit\_message\_id too) and the message content / embed.\
In this example you will notice that this action supports two formats: simple (a YAML list `[ ]`) and more complex (a YAML map `{ }`).\
Note that when using the more complex format whitespace can be finicky: it is recommended that you use a [YAML validator](https://codebeautify.org/yaml-validator) to preserve your sanity.

```
rank: 1
name: message-test
event: on-message
if:
  - message-matches-any: [test send]
do:
  - send-message: [$channel_id, hello] # Sends a message to the channel in the context
  - send-message: [$user_id, hello] # Sends a message to the user who triggered the rule
  - send-message: [133917673317401111, hello] # Sends a message to the specified id (channel or user)
  - send-message: [general, hello] # Sends a message to the specified channel (channel name)
  - send-message: {id: $channel_id, title: "My embed", description: "A simple embed"} # Sends a simple embed in the context's channel
  # The following one sends a (very complex) embed and it's done just to show the syntax
  # Note that NOT all fields are mandatory: just use the fields you need
  - send-message:
      id: $channel_id
      content: message content # content of the message
      author_name: $user
      author_url: $user_avatar_url
      author_icon_url: $user_avatar_url
      title: Have you ever wondered what happens if you use all the embed options?
      description: "Has science gone too far?" # description of the embed
      footer_text: "Made with Warden"
      footer_icon_url: $user_avatar_url
      image: $user_avatar_url
      add_timestamp: true # Add the current date / time to the embed
      thumbnail: $user_avatar_url
      url: $user_avatar_url
      color: 0x59FF00 # Use a HTML color picker for this, with due adjustments: #59FF00 -> 0x59FF00
      fields: [{name: Some, value: might}, {name: think, value: so, inline: false}] # These are the embed fields, you can add more than 2... Or fewer.
      # edit_message_id: 123456 # You can also edit a message by passing a valid message ID here. Empty values will be ignored.
      # reply_message_id: 123456 # Reply to a message (ID). Must be in the same channel the message is being sent in
      # ping_on_reply: true # Whether to ping on reply or not
```

#### Notifying the staff

This rule shows how to use `notify-staff` and its many parameters

```
rank: 1
name: notify-test
event: on-message
if:
  - message-matches-any: [test notify]
do:
  - notify-staff: "hello" # A basic, text-only, message in the staff channel

# An elaborate staff notification. It will send an embed containing the title + description, 
# a handy link to instantly jump to the message, support for quick reactions
# and additional fields with information about the user / the channel 
  - notify-staff:
      title: "Warning!"
      content: "A user is spamming"
      jump_to_ctx_message: true
      qa_target: $user_id # Quick action target
      qa_reason: "Spamming messages" # Quick action reason, optional
      no_repeat_for: 30 seconds # Ensures that this notif. won't be sent again in the next 30 secs
      no_repeat_key: $rule_name-1 # An unique key that identifies this notif., make sure to set this too

# A notification that uses all the parameters for showing purposes. Only use the parameters you need
  - notify-staff:
      content: "A user is spamming"
      title: "Warning!"
      fields:
        - {name: "Field1", value: "Foo", inline: true}
        - {name: "Field2", value: "Bar", inline: true}
      add_ctx_fields: true
      thumbnail: $user_avatar_url
      footer_text: "my footer text"
      ping: true # Pings the staff role
      jump_to:
        channel_id: "$channel_id"
        message_id: "$message_id"
      # jump_to_ctx_message: true # This cannot be done if jump_to is used
      qa_target: $user_id
      qa_reason: Spamming
      no_repeat_for: 10 seconds
      no_repeat_key: $rule_name-2
      allow_everyone_ping: false
```

#### A simple dehoister

This rule renames users who are attempting to hoist. Hoisting means prepending an exclamation mark to the username with the purpose of showing up at the top of the user list. This rule can also be manually run against every server member (`[p]def warden run`). Trusted users, staff and users who already have a nickname are ignored.

```
rank: 2
name: dehoist
event: [on-user-join, manual]
if:
  - username-matches-any: ["!*"]
  - if-not:
      - nickname-matches-any: ["*"]
do:
  - set-user-nickname: "no hoisting"
```

#### Preventing attachments from new users

This rule prevents new users (rank 4) from sending attachments. Staff is also notified about the deletion and is being given precise context about where that happened.

```
rank: 4
name: no-attachments-rank4
event: [on-message, on-message-edit]
if:
  - message-has-attachment: true
do:
  - delete-user-message:
  - send-message: [$channel_id, "$user_mention Sorry, you are not allowed to send attachments."]
  - notify-staff:
      title: "Attachment removed"
      content: "A new user attempted to send an attachment"
      jump_to_ctx_message: true
      add_ctx_fields: true
      no_repeat_for: 30 seconds
      no_repeat_key: $rule_name-1
```

#### Countering mass mentions

This rule bans any user (except trusted users and staff) who mentions at least 15 different people.

```
rank: 2
name: mention-ban-rank2
event: [on-message]
if:
  - message-contains-more-than-unique-mentions: 14
do:
  - ban-user-and-delete: 1
  - send-mod-log: "User banned for mentioning too many people."
```

#### Taking action after multiple infractions

The following two rules take advantage of the [heat level](https://github.com/Twentysix26/x26-Cogs/wiki/Warden#heat-level) system to kick a user after 3 infractions. Notice the `priority` parameter in the first rule.

```
rank: 2
name: bad-word
priority: 1
event: on-message
if:
  - message-matches-any: ["*b?d w?rd*"]
do:
  - delete-user-message:
  - send-message: [$channel_id, "No bad word here!"]
  - add-user-heatpoint: 1h
```

```
rank: 2
name: check-heat
event: on-message
if:
  - user-heat-is: 3
do:
  - kick-user:
```

* © 2022 GitHub, Inc.
* [Terms](https://docs.github.com/en/github/site-policy/github-terms-of-service)
* [Privacy](https://docs.github.com/en/github/site-policy/github-privacy-statement)
* [Security](https://github.com/security)
* [S](https://www.githubstatus.com)
