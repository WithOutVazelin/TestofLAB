import requests
from pathlib import Path

TOKEN = "y0__xDv7anKAxiAhD4gpv3lvRZ65N72FHD6sHbKj8xhjTZpy7VgZw"
FILE = Path("file.txt")


def upload():
    if not FILE.exists():
        print(f"Файл {FILE} не найден")
        return

    r = requests.get( 
       "https://cloud-api.yandex.net/v1/disk/resources/upload", 
       headers={"Authorization": f"OAuth {TOKEN}"}, 
       params={"path": f"app:/{FILE.name}", "overwrite": "true"} 
   ) 

   if r.status_code != 200: 
       print(f"Ошибка: {r.json()}") 
       return 

   with open(FILE, "rb") as f: 
       r2 = requests.put(r.json()["href"], data=f) 

   print(f"Успешно" if r2.status_code in (200, 201) else f"Ошибка: {r2.text}")


if __name__ == "__main__": 
   upload()