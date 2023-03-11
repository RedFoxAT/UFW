## Если Ваш сервер (нода) в мир смотрит без включенного фаервола то это лишь вопрос времени когда его хакнут!<br>
> If your server (noda) looks into the world without a firewall turned on, it is only a matter of time before it is broken!

Одной из первых мер по снижению рисков безопасности является грамотная настройка правил межсетевого экрана. Здесь даны основные команды и правила работы с утилитой UFW.
> One of the first measures to mitigate security risks is to properly configure firewall rules. Here are the basic commands and rules for working with the UFW utility.

## Требования / Requirements:

1. Возможность исполнения команд под sudo (Суперпользователь) 
 > Ability to execute commands under sudo (Superuser).  
2. Утилита  UFW предустановлена в системе. Если по какой-то причине она была отсутствует, вы можете установить ее  с помощью команды:
 > The UFW utility is preinstalled on the system. If for some reason it was missing, you can install it using the command:
```
sudo apt-get install ufw
```
## Проверка правил и текущего состояния UFW / Checking Rules and Current Status
```
sudo ufw status verbose
```
По умолчанию UFW отключен, так что вы должны увидеть что-то вроде этого:
> By default, UFW is disabled, so you should see something like this:
```
Status: inactive
```
***Внимание!*** Проведите начальную настройку перед включением UFW. В частности, должен быть доступен SSH(22 порт). В ином случае вы рискуете потерять доступ к серверу.
> ***Caution!*** Make the initial setting before turning on UFW. In particular, SSH (port 22) should be available. Otherwise, you risk losing access to the server.


## Начальная настройка

По умолчанию UFW настройки запрещают все входящие соединения и разрешают все исходящие. Это значит, что если кто-то попытается  подключиться к вашему серверу, он не сможет этого сделать, в то время как любое приложение на сервере имеет доступ к внешним соединениям.

## Cоответствующие правила фаервола прописываются так:
```
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

## Добавление правила для SSH-соединений
 Чтобы разрешить входящие SSH-соединения, выполните команду:
```
sudo ufw allow ssh
```
SSH (22 порт) UFW знает наименования распространенных служб (ssh, sftp, http, https), поэтому вы можете использовать их вместо номера порта.

Теперь, когда ваш межсетевой экран настроен, можете включать его.

## Запуск UFW

Чтобы включить UFW, используйте следующую команду:
```
sudo ufw enable
```
Вы получите похожее  предупреждение:
```
Command may disrupt existing ssh connections. Proceed with operation (y|n)? 
```
Это означает, что запуск этого сервиса может разорвать текущее ssh соединение.
Но, так как мы его уже добавили ssh в правила, этого не произойдет. Поэтому просто нажмите (y).

## Добавляем необходимые порты командой:
```
sudo ufw allow <port>
 # <port> меняем на нужный номер порта (22) или диапазон портов ( 3000:4000 ). Так же можно использовать IP
```
## Ограничение подключений
```
sudo ufw deny <port>
 # <port> меняем на нужный номер порта (22) или диапазон портов ( 3000:4000 )& Так же можно использовать IP
```
##Отключение UFW

Отключить UFW можно при помощи команды:

```
sudo ufw disable
```
В результате ее выполнения все созданные ранее правила утратят силу.

## Сброс правил
Если вам требуется сбросить текущие настройки, воспользуйтесь командой:
```
sudo ufw reset
```
В результате ее выполнения все правила будут отключены и удалены.
