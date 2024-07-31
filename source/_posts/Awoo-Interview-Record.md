---
title: 💼面試之旅 - 測驗題目紀錄
date: 2024-07-31 16:34:32
code_block_shrink:  false
categories: [Interview]
tags: [Python, requests, API]
---

嗨嗨，本篇主要是用來記錄面試旅途中遇到的測驗題目...✍🏻
<!-- more -->

**Open API 規格**
- URL: https://pokeapi.co/docs/v2#info
    - https://pokeapi.co/api/v2/pokemon/{id or name}/
    
## 目標
請協助設計一個 Automation Script 透過 `/api/v2/pokemon` API 確認：

1. 列出 id 為 6 的寶可夢名稱（name）
2. 列出 id < 20, id > 0 的寶可夢名稱（name）以及其寶可夢的屬性（types），依照 id 由小至大排序
3. 列出 id < 100, id > 0 的寶可夢中，體重（weight） < 50 的寶可夢名稱（name）及寶可夢體重（weight），並且依照體重由大至小排序

## 思考模式：
1. 使用 requests 庫從 PokeAPI 獲取寶可夢的數據。
2. 定義函數以實現特定的功能需求。
3. 定義 main 函數以執行腳本時觸發相關功能。

### Step 1 

完成目標1：列出 id 為 6 的寶可夢名稱（name）。
```python!
import requests

BASE_URL = "https://pokeapi.co/api/v2/pokemon/"

def get_pokemon_data(pokemon_id):
    """根據 ID 獲取寶可夢詳細資料"""
    response = requests.get(f"{BASE_URL}{pokemon_id}/")
    response.raise_for_status()  
    return response.json()

def get_pokemon_name_by_id(pokemon_id):
    """列出指定 ID 的寶可夢名稱"""
    pokemon_data = get_pokemon_data(pokemon_id)
    return pokemon_data['name']

def main():
    pokemon_id = 6
    name = get_pokemon_name_by_id(pokemon_id)
    print(f"ID {pokemon_id} 的寶可夢名稱為: {name}")


if __name__ == "__main__":
    main()
```

說明：
1. `get_pokemon_data(pokemon_id)` :
- 該函數使用 `requests.get` 從 API 獲取指定 ID 的寶可夢詳細資料。
- 使用 `response.raise_for_status()` 確保請求成功，並返回 JSON 格式的數據。
2. `get_pokemon_name_by_id(pokemon_id)` :

- 該函數調用 `get_pokemon_data` 來獲取寶可夢的數據。
- 從返回的 JSON 數據中提取並返回寶可夢的名稱。
3. `main()` :

- 主函數設置寶可夢的 ID（目標條件為 6），調用 `get_pokemon_name_by_id` 獲取該寶可夢的名稱。
- 打印出該寶可夢的名稱。

### Step 2

完成目標2：列出 id < 20, id > 0 的寶可夢名稱（name）以及其寶可夢的屬性（types），依照 id 由小至大排序

```python!
def list_pokemon_names_and_types(lower_id, upper_id):
    """列出 ID 範圍內的寶可夢名稱及屬性，依照 ID 由小至大排序"""
    names_and_types = []
    for pokemon_id in range(lower_id, upper_id):
        if pokemon_id > 0:
            pokemon_data = get_pokemon_data(pokemon_id)
            name = pokemon_data['name']
            types = [t['type']['name'] for t in pokemon_data['types']]
            names_and_types.append((pokemon_id, name, types))
    return sorted(names_and_types, key=lambda x: x[0])

def main():
    names_and_types = list_pokemon_names_and_types(1, 20)
    for pokemon_id, name, types in names_and_types:
        print(f"ID: {pokemon_id} 寶可夢名稱: {name}, 屬性: {', '.join(types)}")
        
if __name__ == "__main__":
    main()
```

說明：
1. `list_pokemon_names_and_types(lower_id, upper_id)` ：
- 在指定的 ID 範圍內，獲取寶可夢的名稱及屬性。
- 將結果按 ID 由小到大排序並返回。

2. `main()` :
- 調用 `list_pokemon_names_and_types` 獲取範圍(0~20)內的寶可夢名稱及屬性，並打印每個寶可夢的詳細訊息。

### Step 3

完成目標3：列出 id < 100, id > 0 的寶可夢中，體重（weight） < 50 的寶可夢名稱（name）及寶可夢體重（weight），並且依照體重由大至小排序

```python!
def list_pokemon_names_and_weights(lower_id, upper_id, weight_limit):
    """列出 ID 範圍內體重小於指定限制的寶可夢名稱及體重，依照體重由大至小排序"""
    names_and_weights = []
    for pokemon_id in range(lower_id, upper_id):
        if pokemon_id > 0:
            pokemon_data = get_pokemon_data(pokemon_id)
            weight = pokemon_data['weight']
            if weight < weight_limit:
                name = pokemon_data['name']
                names_and_weights.append((name, weight))
    return sorted(names_and_weights, key=lambda x: x[1], reverse=True)

def main():
    names_and_weights = list_pokemon_names_and_weights(0, 100, 50)
    print("【目標三】")
    for name, weight in names_and_weights:
        print(f'{name}, 體重: {weight}')
        
if __name__ == "__main__":
    main()
```

說明：
1. `list_pokemon_names_and_weights(lower_id, upper_id, weight_limit)` ：
- 列出 ID 範圍內體重小於指定限制的寶可夢名稱及體重，依照體重由大至小排序。
- 將符合條件的寶可夢名稱和體重存入列表中並返回。

2. `main()` :
- 調用 `list_pokemon_names_and_weights` 獲取 ID 範圍(0~100)內體重小於50的寶可夢名稱及體重並打印。

