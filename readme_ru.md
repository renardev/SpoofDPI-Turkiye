**⭐Pull Request-ы или любые формы вклада будут признательны⭐**

# SpoofDPI

Можете прочитать на других языках: [🇬🇧English](https://github.com/xvzc/SpoofDPI), [🇰🇷한국어](https://github.com/xvzc/SpoofDPI/blob/main/readme_ko.md), [🇨🇳简体中文](https://github.com/xvzc/SpoofDPI/blob/main/readme_zh-cn.md), [🇷🇺Русский](https://github.com/xvzc/SpoofDPI/blob/main/readme_ru.md)

Простое и быстрое ПО, созданное для обхода **Deep Packet Inspection**  
  
![image](https://user-images.githubusercontent.com/45588457/148035986-8b0076cc-fefb-48a1-9939-a8d9ab1d6322.png)

# Установка
## Бинарник
SpoofDPI будет установлен в директорию `~/.spoof-dpi/bin`.  
Чтобы запустить SpoofDPI в любой директории, добавьте строку ниже в `~/.bashrc || ~/.zshrc || ...`
```
export PATH=$PATH:~/.spoof-dpi/bin
```
---
```bash
# OSX
curl -fsSL https://raw.githubusercontent.com/xvzc/SpoofDPI/main/install.sh | bash -s darwin-amd64

# linux-amd64
curl -fsSL https://raw.githubusercontent.com/xvzc/SpoofDPI/main/install.sh | bash -s linux-amd64

# linux-arm
curl -fsSL https://raw.githubusercontent.com/xvzc/SpoofDPI/main/install.sh | bash -s linux-arm

# linux-arm64
curl -fsSL https://raw.githubusercontent.com/xvzc/SpoofDPI/main/install.sh | bash -s linux-arm64

# linux-mips
curl -fsSL https://raw.githubusercontent.com/xvzc/SpoofDPI/main/install.sh | bash -s linux-mips

# linux-mipsle
curl -fsSL https://raw.githubusercontent.com/xvzc/SpoofDPI/main/install.sh | bash -s linux-mipsle
```

## Go
Вы также можете установить SpoofDPI с помощью **go install**  
`$ go install github.com/xvzc/SpoofDPI/cmd/spoof-dpi`  
  > Не забудьте, что $GOPATH должен быть установлен в Вашем $PATH

## Git
Вы также можете собрать SpoofDPI

`$ git clone https://github.com/xvzc/SpoofDPI.git`  
`$ cd SpoofDPI`  
`$ go build ./cmd/...`  

# Использование
```
Использование: spoof-dpi [опции...]
  -addr string
        адрес (по умолчанию "127.0.0.1")
  -debug
        включает режим отладки
  -dns-addr string
        адрес DNS (по умолчанию "8.8.8.8")
  -dns-port int
        порт DNS сервера (по умолчанию 53)
  -enable-doh
        включает 'dns over https'
  -no-banner
        выключает баннер
  -pattern string
        обходить DPI только на пакетах с указанным regex
  -port int
        порт (по умолчанию 8080)
  -timeout int
        таймаут в миллисекундах (по умолчанию 2000)
  -url value
        обходить DPI только на указанном URL, опцию можно указывать несколько раз
  -v    выводит версию spoof-dpi. может содержать другую полезную информацию
  -window-size int
        размер чанка в байтах для фрагментации client hello,
        если не работает по умолчанию, можно попробовать значения меньше;
        при 0 используется старый (до v0.10.0) метод разделения client hello:
        фрагментация для первого дата пакета и остальных (по умолчанию 50)

```
> Если Вы используете любые VPN-расширения по типу Hotspot Shield в браузере  
  Chrome, зайдите в Настройки > Расширения и отключите их.

### OSX
Пропишите `$ spoof-dpi` и прокси автоматически установится

### Linux
Пропишите `$ spoof-dpi` и откройте Chrome с параметром прокси
`google-chrome --proxy-server="http://127.0.0.1:8080"`

# Как это работает
### HTTP
Поскольку большинство веб-сайтов в мире теперь поддерживают HTTPS, SpoofDPI не обходит Deep Packet Inspection для HTTP запросов, однако он по-прежнему обеспечивает прокси-соединение для всех HTTP запросов.

### HTTPS
Хотя TLS 1.3 шифрует каждый процесс рукопожатия, имена доменов по-прежнему отображаются в виде открытого текста в пакете Client Hello. Другими словами, когда кто-то другой смотрит на пакет, он может легко догадаться, куда направляется пакет. Домен может предоставлять значительную информацию во время обработки DPI, и мы можем видеть, что соединение блокируется сразу после отправки пакета Client Hello.
Я попробовал несколько способов обойти это, и обнаружил, что, похоже, проверяется только первый фрагмент, когда мы отправляем пакет Client Hello, разделенный на фрагменты. Чтобы обойти DPI, SpoofDPI отправляет на сервер первый 1 байт запроса, а затем отправляет все остальное.
 > SpoofDPI не расшифровывает Ваши HTTPS запросы, так что нам не нужны SSL сертификаты.

# Вдохновение
[Green Tunnel](https://github.com/SadeghHayeri/GreenTunnel) от @SadeghHayeri  
[GoodbyeDPI](https://github.com/ValdikSS/GoodbyeDPI) от @ValdikSS