# shrikh-fr-dbus-service
dbus service for cash register (ShtrikhFR) 

Драйвер для Linux ККМ аппаратов серии Штрих-М, Штрих-ФР, и другие поддерживающе протокол Штрих.

Сделан в виде сервиса DBus.
Преимущество - работает в любых тулкитах биндинг на DBus (bash, javascript, c++, perl, python и.т.д), можно расшарить через сеть.

В драйвере реализован протокол Штрих 1.12 (+добавлена несколько новых команд из нового протокола 2.0.24, открыть смету и.т.д.).
Добавлены консольная утилита чтения/записи таблиц и примеры на Qt.

Установка:
- склонировать проект shtrikh-fr-dbus-service например в /opt, воспользуйтесь git clone
- установить в систему дополнительные модули для Perl - Device::SerialPort, Time::HiRes, Math::BigInt, Unix::Syslog, Net::DBus

- в файле ru.shtrih_m.fr.service исправить путь до исполняемого файла shtrih_fr.service, поправить права на запуск, также User=XXXX должен быть сервисным пользователем системы, который имеет доступ чтения и записи в порты ttyS*,ttyUSB*, для теста можете воспользоваться правами root

- файл ru.shtrih_m.fr.service скопировать в сервисную директорию, обычно это /usr/share/dbus-1/system-services
- файл ru.shtrih_m.fr.conf скопировать в директорию /etc/dbus-1/system.d
- в зависисмости от версии DBus он либо сам перечитает все свои конфиги либо надо будет перезапустить службу DBus

- запустить команду на получение статуса ККМ examples/get_status.sh, сервис стартанет автоматически и выдаст в консоль результат

Правило udev для usb подключения на фиксированный порт:
KERNEL=="ttyACM*", ACTION=="add", ENV{ID_BUS}=="usb", ENV{ID_VENDOR_ID}=="1fc9", ENV{ID_MODEL_ID}=="0083", SYMLINK+="ttyShtrikh"
