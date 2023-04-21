установка и запуск
https://www.dmosk.ru/miniinstruktions.php?mini=rsyslog


apt-get install rsyslog
systemctl enable rsyslog
systemctl start rsyslog


ufw allow 514/tcp
ufw allow 514/udp
ufw reload


vi /etc/rsyslog.conf

добавить в файл
```
$ModLoad imudp
$UDPServerRun 514

$ModLoad imtcp
$InputTCPServerRun 514
```
можно оставить только один протокол

После добавляем в конфигурационный файл строки:
```
$template RemoteLogs,"/var/log/rsyslog/%HOSTNAME%/%PROGRAMNAME%.log"
*.* ?RemoteLogs
& ~
```



мы создаем шаблон с названием RemoteLogs, который принимает логи всех категорий,
оги, полученный по данному шаблону будут сохраняться в каталоге по маске
/var/log/rsyslog/<имя компьютера, откуда пришел лог>/<приложение, чей лог пришел>.log;
конструкция & ~ 
говорит о том, что после получения лога, необходимо остановить дальнейшую
его обработку.

systemctl restart rsyslog

после этого можно отправлять файлы

/var/log/rsyslog/