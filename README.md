import discord
from discord.ext import commands
from datetime import date
import random

intents = discord.Intents.default()
bot = commands.Bot(command_prefix="!", intents=intents)

@bot.event
async def on_ready():
    print(f" Bot {bot.user} jest online!")


@bot.command(name="segregacja", help="Dowiedz się, do którego pojemnika wrzucić dany odpad ")
async def segregacja(ctx, *, odpad: str = None):
    if not odpad:
        await ctx.send("♻️ Użycie: `!segregacja <nazwa odpadu>`\nPrzykład: `!segregacja szklana butelka`")
        return

    odpad = odpad.lower()

    pojemniki = {
        "papier": ["gazeta", "karton", "zeszyt", "tektura", "papier", "ulotka"],
        "plastik": ["butelka", "folia", "reklamówka", "nakrętka", "puszka", "metal", "opakowanie"],
        "szkło": ["butelka szklana", "słoik", "szkło"],
        "bio": ["resztki jedzenia", "skórka", "owoce", "warzywa", "fus", "kwiaty", "liście", "trawa"],
        "zmieszane": ["ceramika", "brudny papier", "pielucha", "popiół", "resztki mięsa", "guma"],
        "niebezpieczne": ["bateria", "baterie", "akumulator", "żarówka", "farba", "lakier", "chemikalia", "elektrośmieć", "elektrośmieci"]
    }

    kolory = {
        "papier": " **niebieski** (papier)",
        "plastik": " **żółty** (plastik i metale)",
        "szkło": " **zielony** (szkło)",
        "bio": " **brązowy** (bioodpady)",
        "zmieszane": " **czarny** (odpady zmieszane)",
        "niebezpieczne": " **specjalny punkt zbiórki odpadów niebezpiecznych (np. baterie, elektrośmieci)**"
    }

    znaleziono = False
    for pojemnik, slowa in pojemniki.items():
        if any(slowo in odpad for slowo in slowa):
            await ctx.send(f"✅ **{odpad.capitalize()}** wrzucamy do pojemnika {kolory[pojemnik]}.")
            znaleziono = True
            break

    if not znaleziono:
        await ctx.send(" Nie jestem pewien, do którego pojemnika to należy. "
                       "Spróbuj wpisać bardziej szczegółowo, np. `!segregacja butelka plastikowa`.")


@bot.command(name="ogranicz", help="Poznaj sposoby na ograniczenie ilości odpadów ")
async def ogranicz(ctx):
    sposoby = [
        " Używaj toreb wielorazowych zamiast jednorazówek.",
        " Pij wodę z kranu zamiast kupować butelkowaną.",
        " Kupuj produkty bez zbędnych opakowań.",
        " Segreguj odpady i oddawaj surowce do recyklingu.",
        " Oddawaj baterie i sprzęt elektroniczny do punktów zbiórki.",
        " Wybieraj produkty w szkle lub kartonie zamiast plastiku.",
        " Korzystaj z bidonu i kubka wielorazowego.",
        " Naprawiaj ubrania zamiast wyrzucać.",
        " Planuj zakupy spożywcze, aby nie marnować jedzenia.",
        " Wykorzystuj ponownie pudełka i słoiki."
    ]
    await ctx.send(f" Sposób na ograniczenie odpadów:\n{random.choice(sposoby)}")


@bot.command(name="kalendarz", help="Sprawdź ekologiczne święta i wydarzenia ")
async def kalendarz(ctx):
    eko_dni = {
        (2, 2): " Światowy Dzień Mokradeł",
        (3, 21): " Międzynarodowy Dzień Lasów",
        (3, 22): " Światowy Dzień Wody",
        (4, 22): " Dzień Ziemi",
        (5, 22): " Międzynarodowy Dzień Różnorodności Biologicznej",
        (6, 5): " Światowy Dzień Ochrony Środowiska",
        (9, 16): " Europejski Tydzień Zrównoważonego Transportu",
        (9, 22): " Dzień bez Samochodu",
        (10, 15): " Światowy Dzień Mycia Rąk",
        (11, 19): " Światowy Dzień Toalety",
        (12, 5): " Międzynarodowy Dzień Gleby"
    }

    dzis = date.today()
    klucz_dzis = (dzis.month, dzis.day)

    if klucz_dzis in eko_dni:
        await ctx.send(f" Dziś jest **{eko_dni[klucz_dzis]}!** ")
    else:
        przyszle = sorted(
            [(m, d, nazwa) for (m, d), nazwa in eko_dni.items() if (m, d) >= (dzis.month, dzis.day)],
            key=lambda x: (x[0], x[1])
        )
        if przyszle:
            m, d, nazwa = przyszle[0]
            await ctx.send(f" Dziś brak eko-święta, ale następne to **{nazwa}** ({d}.{m}). ")
        else:
            m, d, nazwa = list(eko_dni.items())[0][0][0], list(eko_dni.items())[0][0][1], list(eko_dni.items())[0][1]
            await ctx.send(f" W tym roku już nie ma więcej eko-świąt. Najbliższe to **{nazwa}** ({d}.{m}). ")


@bot.command(name="info", help="Informacje o EcoBocie ")
async def info(ctx):
    await ctx.send(
        "**EcoBot** pomaga Ci żyć ekologicznie \n\n"
        "Dostępne komendy:\n"
        " `!segregacja <odpad>` – sprawdź, gdzie wyrzucić odpad\n"
        " `!ogranicz` – sposób na ograniczenie ilości śmieci\n"
        " `!kalendarz` – sprawdź ekologiczne święta\n"
        " `!info` – pokazuje listę komend"
    )








































bot.run("MTQyODA1NTg0NTM1ODUzODg5NQ.Ga-YiR.oUlHYgMqfQxbnPTtP9qW4lPA12VhGZkjGjJMYI")
