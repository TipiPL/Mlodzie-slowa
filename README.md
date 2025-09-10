meme_dict = {
    "CRINGE": "Coś wyjątkowo dziwnego lub zawstydzającego",
    "LOL": "Częsta reakcja na coś zabawnego",
    "SIGMA": "Lubiana osoba",
    "ROFL": "odpowiedź na żart",
    "SHEESH":"lekka dezaprobata",
    "CREPPY": "straszny, złowieszczy",
    "AGGRO": "stać się agresywnym/zły",
    "SLAY": "uznanie i podziw",
    "CZEMÓ": "nieformalne słowo",
    "BAMBIK": "osoba początkująca",
    
    
}

word = input("Wpisz słowo, którego nie rozumiesz (używaj wielkich liter!): ")

if word in meme_dict.keys():
    print("Znaczenie:", meme_dict[word])
else:
    print("Nie znam tego słowa. Spróbuj inne!")
