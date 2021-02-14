# Waiting for User Inputs [discord.py](https://discordpy.readthedocs.io)
## Prerequite Knowledge
This article assumes you have a basic understanding of Python (including Object Oriented Programming i.e Classes), and that you know the basics of `discord.py` - like its basic events and models. This also assumes that you're using the `commands` extension, although `wait_for` works the same for `discord.Client` too.
******

While you are programming your Discord Bot, a common issue that you're going to run into quickly is getting user inputs from Discord users. Now obviously something like the `input` function will not work here, so what do you do?\
Enter,
## [`commands.Bot.wait_for`](https://discordpy.readthedocs.io/en/stable/ext/commands/api.html#discord.ext.commands.Bot.wait_for)
Let's see what the documentation says about `wait_for`s:
> ### `wait_for(event, *, check=None, timeout=None)`
> This function is a coroutine.
>
> Waits for a WebSocket event to be dispatched.

Let's break this down:
`bot.wait_for` _waits for a WebSocket event to be dispatched_ - this could be any of the events you've already been using, like `on_message`, `on_reaction_add` etc.
The `check` is a function that is used to narrow results down. It has to return True, for some specific condition.
The usage could best be understood from an example: 

### Example 1: Waiting for textual user input
In this example, we're waiting for the person who invoked the command to send a `yes/no` reply.
```py
# Step 1: Defining the check function
# Check functions take the same parameter as the event you're waiting for.
# For example, if you were waiting for the `on_message` event, your check function would be taking a 
# single parameter of `message`.
# If you were waiting for the `on_reaction_add` event, your check function would be taking two parameters,
# `reaction, user`, and so on.
# In this check function, we'll have to check that:
#  - The message is sent in the same channel as the channel where the command was called 
#  - The message is sent by the same person as the person who called the command
#  - The message content is either 'yes' or 'no'.
def check(message: discord.Message) -> bool:
    return message.channel == ctx.channel and message.author == ctx.author and message.content.lower() in ["yes", "no"]

# Step 2: wait_for
# The first argument to wait_for is the name of the event we're waiting for, without the `on` part.
# For example: 
#   on_message -> message
#   on_reaction_add -> reaction_add
# The wait_for will return the arguments that passed the check function. You can pass in an optional timeout keyword-argument also. 
# When this time is exceeded, `asyncio.TimeoutError` is raised.
reply_message = await bot.wait_for("message", check=check)
await ctx.send(f"You said: {reply_message.content}")
```
Working:
![Execution of above code](/images/wait_for_example.png)

[Back to Homepage](https://anand2312.github.io)

