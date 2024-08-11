# Как настроить vpn на android TV
Тут будет написано о том, как используя Outline vpn можно настроить впн на android tv. Пример описан именно для голой оболочки android tv, работаспособность на кастомных оболочка по типу Tizen(Samsung), WebOS(LG), VIDAA(hisense) и др.
## Установка Outline 
1. Для начала нужно купить выделенный сервер и поставить на нем Outline. Сделать это можно на разных площадках, посмотреть лучшиие тарифы можно [здесь](https://hosters.ru/europe-vps.html). Подойдет даже самая минимальная конфигурация (`*мой сервер имеет конфиг 1 CPU, 2 RAM, 12 SSD, 500 Mbit/s*`, главное чтобы был ipV4, сервера с только ipV6 для установки Otline не подходят. Также стоит обратать внимание на ограничение по трафику и скорость интернета, советую искать хостинги с безлимитным трафиком и скоростью от 500Mbit/s.  
3. После оплаты откроется первичная настройка сервера. Там можно будет выбрать OS и вариант входа на сервер, через root(password) или ssh. Для упрощения задачи выбираем Ubuntu в качестве OS и вход через root, пароль желательно сгенерировать, например в authenticator или KeePass, но можно и просто придумать свой. Далее заполняем остальные необходимые графы(`*у всех хостингов по-расзному, например можно выбрать страну,тариф и имя сервера`), запускаем сервер и запоминаем его ip(`формата 192.111.111*`). Перед нами откроется панель управления сервером. 
4. Теперь необходимо подключиться к серверу, для этог можно использовать [Putty](https://www.google.com/url?sa=t&source=web&rct=j&opi=89978449&url=https://www.putty.org/&ved=2ahUKEwj5h5O6zO2HAxUdxAIHHWzqISAQFnoECAgQAQ&usg=AOvVaw0iOGrunharr0YuZtN9wsn1), [Termius](https://www.google.com/url?sa=t&source=web&rct=j&opi=89978449&url=https://termius.com/&ved=2ahUKEwiOhpzBzO2HAxWx-gIHHTR3G0UQFnoECAkQAQ&usg=AOvVaw0GQItTs65kIr1PbJt-j5bc) и другие программы, позволяющие выполнить подключение по ip. Я буду описывать на примере Termius тк он более понятен для рядового пользователя. Но если у вас есть опыт с програмой Pytty - лучше использовать ее. Скачиваем Termius по [ссылке](https://www.google.com/url?sa=t&source=web&rct=j&opi=89978449&url=https://termius.com/&ved=2ahUKEwiOhpzBzO2HAxWx-gIHHTR3G0UQFnoECAkQAQ&usg=AOvVaw0GQItTs65kIr1PbJt-j5bc) и проходим регистрацию. После чего в программе необходимо будет задать локальный пароль и выбрать сервер для подключения, нажав `**New Host**`. В настройках подключения вписать ip сервера и пользователя(`*в нашем случае root*`), после чего нажать кнопку `**Connect**`. Сначала программа спросит локальный пароль, после чего подключится к серверу. В открывшейся консоли необходимо ввести пароль для root.
5. Ставим docker на серве командой ```bash sudo apt-get install docker.io ```. После чего, вероятнеее всего потребуется перезагрузить сервер, сделать это можно в панеле управления на хостинге.
7. Далее (скачиваем)[https://getoutline.org/get-started/] и устанавливаем Outline Manager на свой пк, выбираем `**Настройте Outline где угодно**` Там копируем ``` bash sudo bash -c "$(wget -qO- https://raw.githubusercontent.com/Jigsaw-Code/outline-server/master/src/server_manager/install_scripts/install_server.sh)" ``` и вставляем ее в терминал сервера. После исполнения команды копируем строку с {"apiUrl"} и вставляем ее в Otline Nanager.
8. С настройкой Otline все,теперь необходимо лишь создать новый ключ и вставить его в клиент [Otline](https://s3.amazonaws.com/outline-releases/client/windows/stable/Outline-Client.exe).
## Настройка vpn на Android TV
Важно понимать одну тонкость - просто взять и поставить клиент Outline на телевизор не получится, это нужно просто принять. Но сам по себе Outline это open-sourse продукт, который обладает достаточной гибкостью для подлкючения. Правда чтобы обеспечить это подключение необходимо будет немного повозиться.
1. Первое, что необходимо сделать - установить программу ShadowSocks for Android TV на телевизор. Ее вероятнее всего не будет Play Store (`~~хотя на некоторых моделях может и быть~~`), поэтому [скачиваем](https://apkpure.net/shadowsocks-for-android-tv/com.github.shadowsocks.tv/download) ее на телефон в качествее apk-файла. После чего скачиваем на телефон и телевизор программу [Send Files To TV](https://play.google.com/store/apps/details?id=com.yablio.sendfilestotv&hl=en&pli=1), найти ее можно в стандартном магазине приложений. Телевизор и телефон подключаем к одной wifi сети, выбираем на устройствах recive и send соответственно. Находим наш apk файл в загрузках на телефоне и отправляем его на телевизор. После передачи файла можно переместить его в более удачную папку, такую как Documents.
2. Теперь необходимо установить какой-нибудь файловый менеджер на телевизор, например AnExplorer TV File Manager, он есть в стандарьном магазине приложений. Открываем скаченное, даем все необходимые согласия и находим apk файл в заранее выбранной папке. Устонавливаем ShadowSocks. 
3. К сожалению, ShadowSocks не умеет работать с ключами Otline в их базовом виде, поэтому необходимо перевести их в формат json файла. Для начала нужно получить пароль из нашего ключа, все что после ``` ss://``` и до ```@``` вставляем в [base64 decoder](https://base64.guru/converter/decode) - это и есть password. А все что полсе ```@``` и до ```:``` ip сервера, оставшиеся 4 цифры - порт . После  этого создаем файл vpn.json со следуюшим содержанием
   ``` json {
    "server":"192.111.111", //ip вашего сервера
    "server_port":7645, //порт сервера
    "local_port":1080,
    "password":"2VU1rVlAg6vPFHkVMdW80O", //пароль
    "method":"chacha20-ietf-poly1305", //метод
    "remarks": "Outline Server"
} ```
Сохраняеем файл и через телефон перекидываем его на телевизор используя `**Send Files To TV**`.
4. Последним шагом будет необходимо открыть `**ShadowSocks**`, выбрать пункт `*Replase from file*` и подкинуть приложению наш vpn.json. После чего появится vpn сервер к которому можно подключиться, весь трафик с вашего телевизора будет идти через него.
