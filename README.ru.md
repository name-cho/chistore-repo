# chistore-repo

Репозиторий пакетов для [Chistore-go](https://github.com/name-cho/Chistore-go).
Chistore-go 2.2 и новее подключаются к нему по умолчанию.

- GitGud: <https://gitgud.io/name-cho/chistore-repo>
- GitHub: <https://github.com/name-cho/chistore-repo>

[English](README.md)

## Пакеты

| Пакет | Описание |
| --- | --- |
| `deskgen` | создание и редактирование `.desktop`-файлов |
| `envsnap` | снимки состояния системы и окружения, сравнение |
| `netdiag` | проверка IP, ping, DNS, портов и скорости |
| `pystat` | монитор системы с TUI |

## Зеркало на GitHub

Конфиг по умолчанию указывает на GitGud. Чтобы добавить зеркало:

```bash
chistore repo add github https://raw.githubusercontent.com/name-cho/chistore-repo/main --priority 20
chistore repo refresh
```

Приоритет 20 значит, что при одинаковых пакетах по-прежнему выигрывает копия с
GitGud.

## Raw-ссылки

Базовый raw-адрес это не файл, поэтому браузер получает 404:

```text
https://gitgud.io/name-cho/chistore-repo/-/raw/main             -> 404
https://gitgud.io/name-cho/chistore-repo/-/raw/main/index.json -> 200
```

Chistore сначала запрашивает `index.json`, потом качает пакеты вида
`<base>/envsnap/envsnap`. Ошибка 404 на базовом адресе ему не мешает.

Посмотреть репозиторий можно на страницах выше.

## Структура

```text
chistore-repo/
├── index.json
├── <пакет>/
│   ├── info.md
│   └── <файл>
```

`index.json` создаёт `chistore pkg index`. `info.md` нужен как исходник для
каталога пакета, при установке клиент его не скачивает.

## Как добавить пакет

```bash
chistore pkg init hello
# заполнить hello/info.md, заменить hello/hello
chistore pkg validate
chistore pkg index
```

Потом запушить в GitGud и в GitHub.

Держите `.gitattributes` с `* text=auto eol=lf`. `pkg index` хеширует байты,
которые отдаёт Git, и CRLF в рабочей копии на Windows дал бы хеши, которые не
сходятся при установке.

## Notes

`Notes` необязательны. Одна строка `## Notes:` на одну заметку в `info.md`:

```markdown
## Notes: Запустите hello --init один раз
## Notes: Конфиг лежит в ~/.config/hello
```

`pkg index` переносит их в `index.json`. Chistore 2.4 и новее показывает их в
`chistore info` и после успешной установки или обновления. Менеджер только
печатает текст и ничего не выполняет.
