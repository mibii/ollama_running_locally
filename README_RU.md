# Ollama Setup & Usage Guide

## 1. Открой официальный сайт Ollama

Перейди на страницу загрузки Ollama (https://ollama.com/download/windows) и выбери свою ОС. Ollama официально
поддерживает macOS, Windows и Linux.

## 2. Скачай установщик

Windows: скачай .exe-установщик.\
macOS: скачай .dmg.\
Linux: обычно ставят через терминал скриптом установки.

Для Linux типовая команда выглядит так:

``` bash
curl -fsSL https://ollama.com/install.sh | sh
```

## 3. Запусти установку

Windows: просто открой скачанный .exe и пройди мастер установки.\
macOS: открой .dmg и перетащи Ollama в Applications.\
Linux: дождись завершения скрипта в терминале.

## 4. Дай Ollama завершить первичную настройку

После установки дождись, пока приложение/служба полностью поднимется. На
Windows и macOS это может занять немного времени при первом запуске.

## 5. Проверь, что Ollama доступна в терминале

``` bash
ollama --version
```

Если команда сработала, установка успешна. Если нет --- значит, нужно
проверить PATH или перезапустить терминал.

## 6. Проверь, что сервис отвечает

``` bash
ollama list
```

Если команда отвечает, значит CLI и локальный сервис работают нормально.

## 7. Если команды не видны

На Windows закрой и заново открой терминал.\
На macOS иногда помогает повторный запуск приложения.\
На Linux проверь, что установка завершилась без ошибок и что бинарь есть
в PATH.

------------------------------------------------------------------------

Ollama дает локальный CLI-чат и HTTP-сервер на localhost:11434, а для
интеграций у него есть OpenAI-compatible endpoint /v1/....

------------------------------------------------------------------------

## 1) Установи Ollama

Сначала установи Ollama для своей ОС с официального сайта/документации.
После установки проверь, что команда доступна в терминале.

## 2) Выбери модель

Для старта бери небольшую и быструю модель, чтобы сразу проверить весь
пайплайн. Практически удобно начать с llama3.2, gemma3 или mistral, а
потом уже перейти на более тяжелую модель под твое железо.

## 3) Скачай модель

``` bash
ollama pull llama3.2
```

## 4) Запусти локальный чат

``` bash
ollama run llama3.2
```

После этого откроется интерактивный чат в терминале, и Ollama при
необходимости сам поднимет локальный inference-сервер.

## 5) Проверь список моделей

``` bash
ollama list
ollama show llama3.2
ollama ps
```

list показывает скачанные модели, show --- детали модели, ps --- что
сейчас загружено в память.

## 6) Проверь HTTP API

``` bash
curl http://localhost:11434/api/chat -d '{
  "model": "llama3.2",
  "messages": [
    { "role": "user", "content": "Привет! Скажи кратко, что ты умеешь." }
  ],
  "stream": false
}'
```

Ollama также поддерживает /api/generate, /api/tags, /api/show и
embeddings endpoint.

## 7) Используй OpenAI-compatible API

``` bash
http://localhost:11434/v1
```

Пример:

``` bash
curl http://localhost:11434/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "llama3.2",
    "messages": [
      { "role": "system", "content": "You are a helpful assistant." },
      { "role": "user", "content": "Give me 3 ideas for a local AI app." }
    ],
    "temperature": 0.7
  }'
```

## 8) Подключи из Python

``` python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:11434/v1",
    api_key="ollama"
)

resp = client.chat.completions.create(
    model="llama3.2",
    messages=[{"role": "user", "content": "Привет, коротко объясни Ollama"}]
)
print(resp.choices[0].message.content)
```

## 9) Подними свой локальный сервис

Если хочешь не только чат, но и свой endpoint, заверни вызов Ollama в
простой backend:

-   FastAPI / Flask / Express\
-   Добавь хранение истории диалога\
-   При необходимости сделай streaming ответа\
-   Потом уже подключай RAG, если нужен поиск по своим документам

## 10) Что я бы сделал на практике

Для быстрого старта я бы выбрал такой стек:

-   Ollama\
-   llama3.2 или gemma3\
-   терминальный чат для теста\
-   OpenAI-compatible API для интеграций\
-   дальше FastAPI wrapper, если нужен свой backend

------------------------------------------------------------------------

## Самый короткий путь

``` bash
ollama pull llama3.2
ollama run llama3.2
curl http://localhost:11434/api/chat -d '{"model":"llama3.2","messages":[{"role":"user","content":"Привет"}],"stream":false}'
```
