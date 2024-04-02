# pokedex_2
pokedex
import os
import requests
import json

def obtener_datos_pokemon(nombre_pokemon):
    url = f"https://pokeapi.co/api/v2/pokemon/{nombre_pokemon.lower()}"
    respuesta = requests.get(url)
    if respuesta.status_code == 200:
        datos_pokemon = respuesta.json()
        imagen = datos_pokemon['sprites']['front_default']
        peso = datos_pokemon['weight']
        tamaño = datos_pokemon['height']
        movimientos = [movimiento['move']['name'] for movimiento in datos_pokemon['moves']]
        habilidades = [habilidad['ability']['name'] for habilidad in datos_pokemon['abilities']]
        tipos = [tipo['type']['name'] for tipo in datos_pokemon['types']]
        return imagen, peso, tamaño, movimientos, habilidades, tipos
    else:
        return None

def guardar_en_json(datos_pokemon, nombre_pokemon):
    carpeta_pokedex = 'pokedex'
    if not os.path.exists(carpeta_pokedex):
        os.makedirs(carpeta_pokedex)
    
    ruta_archivo = os.path.join(carpeta_pokedex, f"{nombre_pokemon.lower()}.json")
    with open(ruta_archivo, 'w') as archivo:
        json.dump(datos_pokemon, archivo, indent=4)

def mostrar_datos_pokemon(nombre_pokemon):
    datos_pokemon = obtener_datos_pokemon(nombre_pokemon)
    if datos_pokemon:
        imagen, peso, tamaño, movimientos, habilidades, tipos = datos_pokemon
        print(f"Imagen: {imagen}")
        print(f"Peso: {peso}")
        print(f"Tamaño: {tamaño}")
        print(f"Tipos: {', '.join(tipos)}")
        print(f"Habilidades: {', '.join(habilidades)}")
        print(f"Movimientos: {', '.join(movimientos)}")
        
        # Guardar en JSON
        datos_pokemon_dict = {
            "imagen": imagen,
            "peso": peso,
            "tamaño": tamaño,
            "tipos": tipos,
            "habilidades": habilidades,
            "movimientos": movimientos
        }
        guardar_en_json(datos_pokemon_dict, nombre_pokemon)
    else:
        print(f"No se encontró el Pokémon {nombre_pokemon}")

if __name__ == "__main__":
    nombre_pokemon = input("Introduce el nombre del Pokémon: ")
    mostrar_datos_pokemon(nombre_pokemon)
