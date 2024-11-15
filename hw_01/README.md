Домашнее задание №1


1. Узнать IP-адрес интерфейса, подключенного к сети Интернет
```sh
[root@slonit ~]# ip address | grep inet | grep -v 127.0.0.1
    inet 93.183.73.57/24 brd 93.183.73.255 scope global noprefixroute enp0s5
```

2. Создать именованный пайп ( named pipe ). Вывести в файл через созданный pipe вывод команды ss -plnt
```sh
[root@slonit ~]# mkfifo /tmp/test_pipe
[root@slonit ~]# ss -plnt > /tmp/test_pipe &
[1] 1577
[root@slonit ~]# cat /tmp/test_pipe > output.txt
[1]+  Done                    ss -plnt > /tmp/test_pipe
[root@slonit ~]# cat output.txt
State  Recv-Q Send-Q Local Address:Port Peer Address:PortProcess
LISTEN 0      128          0.0.0.0:22        0.0.0.0:*    users:(("sshd",pid=1039,fd=5))
[root@slonit ~]# rm /tmp/test_pipe
rm: remove fifo '/tmp/test_pipe'? yes
```

3. При помощи именованного пайпа заархивировать всё, что в него отправляем.
Например, содержимое файла /var/log/messages
На выходе получить архив tar или любой другой
```sh
[root@slonit ~]# mkfifo log
[root@slonit ~]# tar czf log.tar.gz -T log &
[6] 4053
[root@slonit ~]# echo "/var/log/messages" > log
[root@slonit ~]# tar: Removing leading `/' from member names

[6]   Done                    tar czf log.tar.gz -T log
[root@slonit ~]# ls
log  log.tar.gz  output.txt
[root@slonit ~]# tar xzf log.tar.gz
[root@slonit ~]# ls
log  log.tar.gz  output.txt  var
[root@slonit ~]# cd var/log
[root@slonit log]# ls
messages
[root@slonit log]# cat messages
Nov 10 21:44:01 slonit rsyslogd[1036]: [origin software="rsyslogd" swVersion="8.2102.0-7.el8_6.1" x-pid="1036" x-info="https://www.rsyslog.com"] rsyslogd was HUPed
Nov 10 22:17:05 slonit kernel: clocksource: timekeeping watchdog on CPU0: hpet wd-wd read-back delay of 107526ns
Nov 10 22:17:05 slonit kernel: clocksource: wd-tsc-wd read-back delay of 575184ns, clock-skew test skipped!
Nov 10 23:11:24 slonit systemd[1]: Starting dnf makecache...
Nov 10 23:11:24 slonit dnf[4034]: Metadata cache refreshed recently.
Nov 10 23:11:24 slonit systemd[1]: dnf-makecache.service: Succeeded.
Nov 10 23:11:24 slonit systemd[1]: Started dnf makecache.
```

4. Вывести дату в unixtime
На вход команды date через пайп подать свой формат выводимой даты
```sh
[root@slonit ~]# echo "2007-07-07 07:07:07" | date -d "$(cat)" +%s
1183792027
```

5. При помощи HEREDOC записать в файл многострочное сообщение
```sh
[root@slonit ~]# cat <<EOF > test.txt
> hello darknes my old friend
> a ishouo botwmgfksdfg
> pinpong
> EOF
[root@slonit ~]# cat test.txt
hello darknes my old friend
a ishouo botwmgfksdfg
pinpong
```
Задание повышенного уровня сложности (его выполнять не обязательно, но если сделаете, будет вашим преимуществом)
Требуется переместить ядро или переименовать его. Перезагрузить систему.
Восстановить систему двумя способами.