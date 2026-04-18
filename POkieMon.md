# PokéAPI Student Reference Guide

**A beginner-friendly guide to the PokéAPI list endpoints**

> **Base URL:** `https://pokeapi.co/api/v2/`
>
> Free · No Authentication · JSON Responses · RESTful
>
> API Version 2 · Classroom Edition · April 2026

---

## Table of Contents

1. [Introduction to PokéAPI](#1-introduction-to-pokéapi)
2. [How List Endpoints Work](#2-how-list-endpoints-work)
3. [Pokémon Endpoints](#3-pokémon-endpoints)
4. [Types Endpoints](#4-types-endpoints)
5. [Moves Endpoints](#5-moves-endpoints)
6. [Items Endpoints](#6-items-endpoints)
7. [Berries Endpoints](#7-berries-endpoints)
8. [Beginner's Glossary](#8-beginners-glossary)
9. [Quick Reference Cheat Sheet](#9-quick-reference-cheat-sheet)

---

## 1. Introduction to PokéAPI

PokéAPI is a free, open-source RESTful API that serves detailed data about the Pokémon video game franchise. It covers Pokémon species, moves, abilities, types, items, berries, game versions, and much more. Because it requires no authentication and returns clean JSON, it is an ideal learning tool for students exploring how APIs work.

Every request uses the HTTP GET method. The base URL for all requests is:

```
GET  https://pokeapi.co/api/v2/{endpoint}/
```

> 💡 **Tip:** You can test any endpoint right in your browser's address bar — just paste the URL and hit Enter!

---

## 2. How List Endpoints Work

When you call an endpoint without specifying an ID or name, PokéAPI returns a **paginated list** of all available resources. By default each page contains 20 results. You can control pagination with two query parameters:

- **`limit`** — how many results per page (e.g., `?limit=5`)
- **`offset`** — how many results to skip (e.g., `?offset=20`)

### Example — Fetching the first 3 Pokémon:

```
GET  https://pokeapi.co/api/v2/pokemon?limit=3
```

**Example Response:**

```json
{
  "count": 1302,
  "next": "https://pokeapi.co/api/v2/pokemon?offset=3&limit=3",
  "previous": null,
  "results": [
    { "name": "bulbasaur", "url": "https://pokeapi.co/api/v2/pokemon/1/" },
    { "name": "ivysaur", "url": "https://pokeapi.co/api/v2/pokemon/2/" },
    { "name": "venusaur", "url": "https://pokeapi.co/api/v2/pokemon/3/" }
  ]
}
```

### List Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `count` | integer | Total number of resources available across all pages. |
| `next` | string | URL for the next page of results (`null` if last page). |
| `previous` | string | URL for the previous page (`null` if first page). |
| `results` | array | Array of objects, each with a `name` and `url`. |

> 💡 **Tip:** Use `?limit=100000` to fetch all resources in one request — handy for small datasets, but be respectful of the API's fair-use policy!

---

## 3. Pokémon Endpoints

The Pokémon group of endpoints is the heart of PokéAPI. Use these to retrieve data about individual Pokémon, their stats, abilities, sprites, and species information.

### 3.1 List All Pokémon

```
GET  https://pokeapi.co/api/v2/pokemon/
```

Returns a paginated list of all Pokémon. Each result contains the Pokémon's name and a URL pointing to its full detail record.

**Example — First 2 Pokémon:**

```json
{
  "count": 1302,
  "next": "https://pokeapi.co/api/v2/pokemon?offset=2&limit=2",
  "previous": null,
  "results": [
    { "name": "bulbasaur", "url": "https://pokeapi.co/api/v2/pokemon/1/" },
    { "name": "ivysaur", "url": "https://pokeapi.co/api/v2/pokemon/2/" }
  ]
}
```

### 3.2 Get a Single Pokémon

```
GET  https://pokeapi.co/api/v2/pokemon/{id or name}/
```

Retrieves detailed information about one Pokémon, including base stats, types, abilities, held items, moves, sprites, and game indices.

**Example — Pikachu (id 25):**

```json
{
  "id": 25,
  "name": "pikachu",
  "base_experience": 112,
  "height": 4,
  "weight": 60,
  "types": [
    { "slot": 1, "type": { "name": "electric", "url": "...type/13/" } }
  ],
  "abilities": [
    {
      "ability": { "name": "static", "url": "...ability/9/" },
      "is_hidden": false,
      "slot": 1
    },
    {
      "ability": { "name": "lightning-rod", "url": "...ability/31/" },
      "is_hidden": true,
      "slot": 3
    }
  ],
  "stats": [
    { "base_stat": 35, "stat": { "name": "hp" } },
    { "base_stat": 55, "stat": { "name": "attack" } },
    { "base_stat": 40, "stat": { "name": "defense" } },
    { "base_stat": 50, "stat": { "name": "special-attack" } },
    { "base_stat": 50, "stat": { "name": "special-defense" } },
    { "base_stat": 90, "stat": { "name": "speed" } }
  ],
  "sprites": {
    "front_default": "https://raw.githubusercontent.com/.../25.png"
  }
}
```

### Key Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `id` | integer | National Pokédex number. |
| `name` | string | Lowercase Pokémon name. |
| `base_experience` | integer | Base XP gained when defeated. |
| `height` | integer | Height in decimetres. |
| `weight` | integer | Weight in hectograms. |
| `types` | array | List of type slots (e.g., electric, water). |
| `abilities` | array | Regular + hidden abilities. |
| `stats` | array | Base stats: HP, Atk, Def, SpA, SpD, Spd. |
| `sprites` | object | URLs to official sprite images. |

> 💡 **Tip:** Height is in decimetres (÷10 for metres) and weight is in hectograms (÷10 for kg).

### 3.3 List Abilities

```
GET  https://pokeapi.co/api/v2/ability/
```

Abilities provide passive effects during battle or in the overworld. This list endpoint returns every ability in the game series.

**Example Response (limit=2):**

```json
{
  "count": 367,
  "results": [
    { "name": "stench", "url": "https://pokeapi.co/api/v2/ability/1/" },
    { "name": "drizzle", "url": "https://pokeapi.co/api/v2/ability/2/" }
  ]
}
```

---

## 4. Types Endpoints

Pokémon types (Fire, Water, Grass, etc.) are fundamental to the battle system. The types endpoints let you look up type match-ups, damage relations, and which Pokémon belong to each type.

### 4.1 List All Types

```
GET  https://pokeapi.co/api/v2/type/
```

**Example Response (limit=3):**

```json
{
  "count": 21,
  "results": [
    { "name": "normal", "url": "https://pokeapi.co/api/v2/type/1/" },
    { "name": "fighting", "url": "https://pokeapi.co/api/v2/type/2/" },
    { "name": "flying", "url": "https://pokeapi.co/api/v2/type/3/" }
  ]
}
```

### 4.2 Get a Single Type

```
GET  https://pokeapi.co/api/v2/type/{id or name}/
```

Returns full details about a type, including its **damage relations** — which types it is super effective against, resistant to, or immune to.

**Example — Fire type:**

```json
{
  "id": 10,
  "name": "fire",
  "damage_relations": {
    "double_damage_to":   [{"name": "grass"}, {"name": "ice"}, {"name": "bug"}, {"name": "steel"}],
    "half_damage_to":     [{"name": "fire"}, {"name": "water"}, {"name": "rock"}, {"name": "dragon"}],
    "no_damage_to":       [],
    "double_damage_from": [{"name": "water"}, {"name": "ground"}, {"name": "rock"}],
    "half_damage_from":   [{"name": "fire"}, {"name": "grass"}, {"name": "ice"},
                           {"name": "bug"}, {"name": "steel"}, {"name": "fairy"}],
    "no_damage_from":     []
  },
  "pokemon": [
    { "pokemon": { "name": "charmander", "url": "..." }, "slot": 1 },
    { "pokemon": { "name": "charmeleon", "url": "..." }, "slot": 1 }
  ]
}
```

### Key Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `id` | integer | Unique type identifier. |
| `name` | string | Type name (lowercase). |
| `damage_relations` | object | Six arrays describing damage multipliers. |
| `pokemon` | array | All Pokémon that have this type. |
| `moves` | array | All moves that belong to this type. |

> 💡 **Tip:** The `damage_relations` object is perfect for building a type-effectiveness chart in your app!

---

## 5. Moves Endpoints

Moves are the attacks and techniques Pokémon use in battle. Each move has a type, power, accuracy, PP (power points), damage class, and potential side effects.

### 5.1 List All Moves

```
GET  https://pokeapi.co/api/v2/move/
```

**Example Response (limit=3):**

```json
{
  "count": 937,
  "results": [
    { "name": "pound", "url": "https://pokeapi.co/api/v2/move/1/" },
    { "name": "karate-chop", "url": "https://pokeapi.co/api/v2/move/2/" },
    { "name": "double-slap", "url": "https://pokeapi.co/api/v2/move/3/" }
  ]
}
```

### 5.2 Get a Single Move

```
GET  https://pokeapi.co/api/v2/move/{id or name}/
```

Returns all details about a specific move, including power, accuracy, type, and a plain-English effect description with variable placeholders.

**Example — Thunderbolt (id 85):**

```json
{
  "id": 85,
  "name": "thunderbolt",
  "accuracy": 100,
  "power": 90,
  "pp": 15,
  "priority": 0,
  "type": { "name": "electric", "url": "...type/13/" },
  "damage_class": { "name": "special", "url": "...move-damage-class/3/" },
  "effect_entries": [
    {
      "effect": "Inflicts regular damage. Has a $effect_chance% chance of paralyzing the target.",
      "short_effect": "Has a $effect_chance% chance to paralyze.",
      "language": { "name": "en" }
    }
  ],
  "effect_chance": 10,
  "learned_by_pokemon": [
    { "name": "pikachu", "url": "...pokemon/25/" },
    { "name": "raichu", "url": "...pokemon/26/" }
  ]
}
```

### Key Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `id` | integer | Unique move identifier. |
| `name` | string | Move name (lowercase, hyphenated). |
| `power` | integer | Base damage (`null` for status moves). |
| `accuracy` | integer | Hit chance in percent (`null` = never misses). |
| `pp` | integer | Max power points (number of uses). |
| `type` | object | The elemental type of the move. |
| `damage_class` | object | physical, special, or status. |
| `effect_entries` | array | Human-readable effect description. |
| `effect_chance` | integer | Probability of the move's secondary effect. |
| `learned_by_pokemon` | array | All Pokémon that can learn this move. |

> 💡 **Tip:** Replace `$effect_chance` in effect text with the `effect_chance` field value to get the real description!

---

## 6. Items Endpoints

Items are objects players collect throughout the Pokémon games — potions, Poké Balls, held items, TMs, and more. The API provides cost, category, effect text, and sprite images.

### 6.1 List All Items

```
GET  https://pokeapi.co/api/v2/item/
```

**Example Response (limit=3):**

```json
{
  "count": 2180,
  "results": [
    { "name": "master-ball", "url": "https://pokeapi.co/api/v2/item/1/" },
    { "name": "ultra-ball", "url": "https://pokeapi.co/api/v2/item/2/" },
    { "name": "great-ball", "url": "https://pokeapi.co/api/v2/item/3/" }
  ]
}
```

### 6.2 Get a Single Item

```
GET  https://pokeapi.co/api/v2/item/{id or name}/
```

**Example — Potion (id 17):**

```json
{
  "id": 17,
  "name": "potion",
  "cost": 200,
  "fling_power": 30,
  "category": { "name": "medicine", "url": "...item-category/26/" },
  "attributes": [
    { "name": "consumable", "url": "...item-attribute/2/" },
    { "name": "usable-in-battle", "url": "...item-attribute/4/" }
  ],
  "effect_entries": [
    {
      "effect": "Used on a Pokémon. Restores 20 HP.",
      "short_effect": "Restores 20 HP.",
      "language": { "name": "en" }
    }
  ],
  "sprites": {
    "default": "https://raw.githubusercontent.com/.../potion.png"
  }
}
```

### Key Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `id` | integer | Unique item identifier. |
| `name` | string | Item name (lowercase, hyphenated). |
| `cost` | integer | Price in Poké Marts (0 = not sold). |
| `fling_power` | integer | Damage when used with the move Fling. |
| `category` | object | Category (medicine, pokeballs, etc.). |
| `attributes` | array | Properties like consumable, holdable. |
| `effect_entries` | array | Human-readable effect text. |
| `sprites` | object | URL to the item's sprite image. |

### 6.3 List Item Categories

```
GET  https://pokeapi.co/api/v2/item-category/
```

Groups items into categories like standard-balls, medicine, vitamins, held-items, and many more.

> 💡 **Tip:** Use item categories to build filters in your Pokédex app — group potions, balls, and TMs into neat tabs!

---

## 7. Berries Endpoints

Berries are special fruits in the Pokémon world. They restore HP, cure status conditions, boost stats, and even fuel the Natural Gift move. The API tracks growth time, firmness, flavors, and more.

### 7.1 List All Berries

```
GET  https://pokeapi.co/api/v2/berry/
```

**Example Response (limit=3):**

```json
{
  "count": 64,
  "results": [
    { "name": "cheri", "url": "https://pokeapi.co/api/v2/berry/1/" },
    { "name": "chesto", "url": "https://pokeapi.co/api/v2/berry/2/" },
    { "name": "pecha", "url": "https://pokeapi.co/api/v2/berry/3/" }
  ]
}
```

### 7.2 Get a Single Berry

```
GET  https://pokeapi.co/api/v2/berry/{id or name}/
```

**Example — Cheri Berry:**

```json
{
  "id": 1,
  "name": "cheri",
  "growth_time": 3,
  "max_harvest": 5,
  "natural_gift_power": 60,
  "size": 20,
  "smoothness": 25,
  "soil_dryness": 15,
  "firmness": { "name": "soft", "url": "...berry-firmness/2/" },
  "flavors": [
    { "potency": 10, "flavor": { "name": "spicy" } },
    { "potency": 0, "flavor": { "name": "dry" } },
    { "potency": 0, "flavor": { "name": "sweet" } },
    { "potency": 0, "flavor": { "name": "bitter" } },
    { "potency": 0, "flavor": { "name": "sour" } }
  ],
  "item": { "name": "cheri-berry", "url": "...item/126/" },
  "natural_gift_type": { "name": "fire", "url": "...type/10/" }
}
```

### Key Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `id` | integer | Unique berry identifier. |
| `name` | string | Berry name (lowercase). |
| `growth_time` | integer | Hours per growth stage (4 stages total). |
| `max_harvest` | integer | Max berries from one tree (Gen IV). |
| `natural_gift_power` | integer | Power of Natural Gift with this berry. |
| `size` | integer | Size in millimetres. |
| `smoothness` | integer | Smoothness for Pokéblocks / Poffins. |
| `soil_dryness` | integer | How fast the soil dries when growing. |
| `firmness` | object | Firmness level (very-soft to super-hard). |
| `flavors` | array | Five flavor potencies: spicy, dry, sweet, bitter, sour. |
| `item` | object | Reference to the corresponding item resource. |
| `natural_gift_type` | object | Type of the Natural Gift move with this berry. |

### 7.3 List Berry Flavors

```
GET  https://pokeapi.co/api/v2/berry-flavor/
```

Returns the five berry flavors: spicy, dry, sweet, bitter, sour. Each flavor maps to a contest type and affects Pokémon differently based on their nature.

> 💡 **Tip:** A Pokémon's nature determines which flavors it likes and dislikes — for example, Adamant Pokémon like Spicy but dislike Dry!

---

## 8. Beginner's Glossary

| Term | Definition |
|------|------------|
| **API** (Application Programming Interface) | A set of rules that lets one program talk to another. PokéAPI lets your code request Pokémon data from a server. |
| **REST** (Representational State Transfer) | An architectural style for APIs where each URL (endpoint) represents a resource you can retrieve. |
| **Endpoint** | A specific URL path you call to get data. Example: `/api/v2/pokemon/` is the Pokémon list endpoint. |
| **HTTP GET** | The request method used to retrieve (read) data from a server. PokéAPI only supports GET. |
| **JSON** (JavaScript Object Notation) | A lightweight data format using key-value pairs and arrays. It's human-readable and the standard format for API responses. |
| **Base URL** | The root address of the API that all endpoints are built on: `https://pokeapi.co/api/v2/` |
| **Pagination** | Splitting a large list into smaller pages. Controlled by `limit` (items per page) and `offset` (starting position). |
| **Query Parameter** | Extra info added to a URL after a `?` to filter or modify the request. Example: `?limit=5&offset=10` |
| **Resource** | A single data object in the API, like one Pokémon, one move, or one item. |
| **Response** | The data the server sends back after you make a request. In PokéAPI, this is always JSON. |
| **Status Code** | A number the server returns indicating success (200) or failure (404 = not found, 500 = server error). |
| **Sprite** | A small 2D image of a Pokémon or item, available via URL in the API response. |
| **Base Stat** | A Pokémon's innate stat value (HP, Attack, etc.) before EVs, IVs, or nature modifiers. |
| **Damage Class** | Whether a move is physical (uses Attack), special (uses Sp. Attack), or status (no direct damage). |
| **Ability** | A passive trait that gives a Pokémon a special effect in battle or in the overworld. |
| **Hidden Ability** | A rare ability some Pokémon can have, only obtainable through special methods. |

---

## 9. Quick Reference Cheat Sheet

### All Major Endpoints

| Category | Endpoint | Description |
|----------|----------|-------------|
| **Pokémon** | `/pokemon/` | List or get Pokémon details |
| | `/ability/` | List or get abilities |
| | `/pokemon-species/` | Species data (evolution, habitat) |
| **Types** | `/type/` | List types and damage relations |
| **Moves** | `/move/` | List or get move details |
| | `/move-damage-class/` | Physical, special, or status |
| **Items** | `/item/` | List or get item details |
| | `/item-category/` | Item groupings (balls, medicine) |
| | `/item-attribute/` | Item properties (consumable…) |
| **Berries** | `/berry/` | List or get berry details |
| | `/berry-flavor/` | Spicy, dry, sweet, bitter, sour |
| | `/berry-firmness/` | Firmness levels for Pokéblocks |
| **Games** | `/generation/` | Game generations (I – IX) |
| | `/pokedex/` | Regional Pokédex entries |
| | `/version/` | Game versions (Red, Blue…) |
| **Evolution** | `/evolution-chain/` | Full evolution family trees |
| **Encounters** | `/encounter-method/` | Wild encounter methods |

### Pagination Quick Reference

| Parameter | Example | Effect |
|-----------|---------|--------|
| `limit` | `?limit=5` | Return 5 results per page |
| `offset` | `?offset=20` | Skip the first 20 results |
| Both | `?limit=10&offset=30` | Results 31–40 |

---

**Happy coding, Trainers! 🎮⚡**

Full documentation: [https://pokeapi.co/docs/v2](https://pokeapi.co/docs/v2)


Everything from the original PDF is faithfully represented — all 9 sections, every endpoint with its example request/response JSON, field tables, 💡 tip callouts, the 16-term glossary, and the cheat sheet. Just save the block above as a `.md` file and it'll render cleanly in GitHub, VS Code, or any Markdown viewer. Let me know if you'd like any tweaks!