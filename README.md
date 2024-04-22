# parsing_photo with Python
# Парсинг фото с их скачиванием Python
### Простой пример парсера на языке Python для скачивания фото с сайта. Итоговый код находится внизу статьи.
### Для создания парсера понадобится библиотека [requests](https://pypi.org/project/requests/) и [bs4](https://pypi.org/project/beautifulsoup4/)

Необходимые зависимости:
```
pip install requests
pip install lxml
pip install bs4
```

Переходим к написани парсера и импортируем необходимые библиотеки:
```
from bs4 import BeautifulSoup
import requests
```

Для примера возьмем сайт Reddit, а точнее его раздел с мемами:
```
url = 'https://www.reddit.com/r/memes/'
block_div_class = 'block relative cursor-pointer group bg-neutral-background focus-within:bg-neutral-background-hover hover:bg-neutral-background-hover xs:rounded-[16px] px-md py-2xs my-2xs nd:visible'
```
Переменная ```url``` хранит в себе адрес сайта, куда мы будем обращаться, а в переменной ```block_div_class``` записано значение класса, в теге ```div```, которого на сайте расположен последний вышедший пост.

Обращаемся к сайт с помощью метода ```get```:
```
response = requests.get(url)
```

C помощью библиотеки [bs4](https://pypi.org/project/beautifulsoup4/) находим изображение, которогое находится в теге ```img``` и забираем значение поля ```src```, находящееся в данном теге:
```
soup = BeautifulSoup(response.text, "lxml")
photo_url = soup.find(class_=block_div_class).find('img')['src']
```
Записываем значение ```src``` в переменную ```photo_url```.

С помощью контекстного менеджера ```with``` скачиваем фотографию по url полученному из значения ```src```:
```
photo = requests.get(photo_url)

with open('photo.png', 'wb') as file:
    file.write(photo.content)
```

Итоговый код:
```
from bs4 import BeautifulSoup
import requests

url = 'https://www.reddit.com/r/memes/'

block_div_class = 'block relative cursor-pointer group bg-neutral-background focus-within:bg-neutral-background-hover hover:bg-neutral-background-hover xs:rounded-[16px] px-md py-2xs my-2xs nd:visible'

response = requests.get(url)
soup = BeautifulSoup(response.text, "lxml")

photo_url = soup.find(class_=block_div_class).find('img')['src']

photo = requests.get(photo_url)

with open('photo.png', 'wb') as file:
    file.write(photo.content)
```




