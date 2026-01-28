# aaaaaaaaaa
aaaa
import discord
from discord.ext import commands
import asyncio

# Configurações de permissões (Intents)
intents = discord.Intents.default()
intents.message_content = True

bot = commands.Bot(command_prefix="!", intents=intents)

@bot.event
async def on_ready():
    print(f'Bot logado como {bot.user.name}')

@bot.command()
async def repetir(ctx, quantidade: int, *, mensagem: str):
    """Envia uma mensagem várias vezes com um intervalo de segurança."""
    
    # Limite máximo para evitar ban imediato
    if quantidade > 50:
        await ctx.send("Ops! O limite máximo para evitar ban é 50 mensagens.")
        return

    await ctx.send(f"Iniciando envio de {quantidade} mensagens...")

    for i in range(quantidade):
        try:
            await ctx.send(mensagem)
            # Espera 1.5 segundos entre as mensagens para não levar Rate Limit
            await asyncio.sleep(1.5) 
        except discord.errors.Forbidden:
            print("Erro: O bot não tem permissão para enviar mensagens aqui.")
            break
        except Exception as e:
            print(f"Ocorreu um erro: {e}")
            break

    await ctx.send("Concluído!")

# Substitua 'SEU_TOKEN_AQUI' pelo token que você pegou no Discord Developer Portal
bot.run('SEU_TOKEN_AQUI')
