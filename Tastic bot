import discord
from discord.ext import commands
import random

intents = discord.Intents.default()
intents.message_content = True
intents.members = True

bot = commands.Bot(command_prefix="!", intents=intents)

# Casual replies the bot can say when tagged
casual_replies = [
    "Hey there! How’s it going?",
    "I’m here if you need me!",
    "Tastic day, isn’t it?",
    "What’s up?",
    "Tell me more!",
    "I’m just a bot, but I’m listening.",
]

@bot.event
async def on_ready():
    print(f"Logged in as {bot.user}")

@bot.event
async def on_message(message):
    # Ignore messages from itself
    if message.author == bot.user:
        return
    
    # Check if bot is tagged in the message
    if bot.user.mentioned_in(message):
        # Remove the mention from content so commands work normally
        ctx = await bot.get_context(message)
        
        # If message is a command, process it
        if ctx.valid:
            await bot.process_commands(message)
            return
        
        # Otherwise, reply casually 50% of the time when tagged but no command
        if random.random() < 0.5:
            await message.channel.send(random.choice(casual_replies))

@bot.command()
async def tastic(ctx, member: discord.Member = None):
    target = member or ctx.author
    
    if "tastic" in (target.nick or target.name).lower():
        await ctx.send(f"{target.display_name} already has 'tastic' in their nickname.")
        return
    
    new_nick = (target.nick or target.name) + "tastic"
    try:
        await target.edit(nick=new_nick)
        await ctx.send(f"Added 'tastic' to {target.display_name}'s nickname!")
    except discord.Forbidden:
        await ctx.send("I don't have permission to change that nickname.")
    except Exception as e:
        await ctx.send(f"Error: {e}")

@bot.command()
async def untastic(ctx, member: discord.Member = None):
    target = member or ctx.author
    
    current_nick = target.nick or target.name
    if "tastic" not in current_nick.lower():
        await ctx.send(f"{target.display_name} doesn't have 'tastic' in their nickname.")
        return
    
    new_nick = current_nick.lower().replace("tastic", "")
    try:
        await target.edit(nick=new_nick)
        await ctx.send(f"Removed 'tastic' from {target.display_name}'s nickname!")
    except discord.Forbidden:
        await ctx.send("I don't have permission to change that nickname.")
    except Exception as e:
        await ctx.send(f"Error: {e}")

@bot.command()
async def curseword(ctx):
    insults = [
        "You absolute buffoon with the IQ of a damp sponge!",
        "You're a walking disaster of epic proportions.",
        "Your brain's so tiny, even a goldfish would school you.",
        "You’re like a cloud. When you disappear, it’s a beautiful day.",
        "If stupidity was a crime, you’d be serving a life sentence.",
        "You have something on your chin… no, the third one down.",
        # Add as many as you want...
    ]
    await ctx.send(random.choice(insults))

@bot.command()
async def roastme(ctx):
    roasts = [
        "You're so lame, your reflection ducks when it sees you.",
        "You have the charm of a damp rag and the personality of a brick wall.",
        "Your existence is a glitch in the matrix.",
        "If you were any slower, you'd be moving backwards.",
        "You bring everyone so much joy... when you leave the room.",
        "You have the charisma of a wet sock.",
        # Add more with curse words if you want
    ]
    await ctx.send(random.choice(roasts))

# Replace with your real token here
import os

bot.run(os.getenv("DISCORD_TOKEN"))
