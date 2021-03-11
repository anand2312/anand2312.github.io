# Guild-specific Prefixes - discord.py

## Prerequisites
This guide assumes that you have basic knowledge of discord.py. 
You will also be needing some kind of persistent storage (preferably a database) to store the prefixes corresponding to each guild. This guide covers the discord.py aspect only, and will not be covering the database interactions. Hence the code examples will be more like pseudocode.

## Retrieving the prefix
Here's the gist of the idea - you make a function which will take two arguments - `bot, message`, and have it return the prefix corresponding to that guild. If there was no prefix saved for that guild, return a default prefix.
The while you make your instance of `commands.Bot`, you pass this _function_ as the `command_prefix` kwarg.

Pseudocode(ish):
```py
# main.py, where you define your instance of commands.Bot
import discord
from discord.ext import commands

def get_prefix(bot: commands.Bot, message: discord.Message) -> str:
    # fetch prefix from your database
    # hint: the guild ID here will be `message.guild.id`
    if prefix exists in db:    # DO THE DB INTERACTIONS YOURSELF. COPYING THIS WILL GIVE YOU ERRORS.
        return prefix
    else:
        return default
        
bot = commands.Bot(command_prefix=get_prefix)
```
_Note that you should be doing any clean up yourself, like deleting the data of guilds that your bot is no longer in, too._

You could now implement a command, that will let users edit/set the prefix for their guild (just INSERT/UPDATE data in the database).

You could also implement a cache for the prefixes, so that the bot doesn't query the database each time.
The cache could be maintained as a _bot variable_.

```py
from collections import defaultdict

import discord
from discord.ext import commands

def get_prefix(bot: commands.Bot, message: discord.Message) -> str:
    in_cache = bot.prefix_cache.get(message.guild.id, None)   # we will create the cache variable later down
    if in_cache:
        return in_cache
    else:
        # didn't exist in cache
        # so fetch from db, and then add to cache
        bot.prefix_cache[message.guild.id] = from_db   # THIS VARIABLE DOESN'T EXIST. YOU HAVE TO IMPLEMENT THE DB INTERACTIONS YOURSELF.
        return from_db

bot = commands.Bot(command_prefix=get_prefix)
bot.prefix_cache = dict()
```
