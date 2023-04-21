# firewall
## Домашнее задание по теме _Фильтрация трафика - firewalld, iptables_  
> Дано  
реализовать knocking port  
centralRouter может попасть на ssh inetrRouter через knock скрипт, пример в материалах.  
добавить inetRouter2, который виден(маршрутизируется (host-only тип сети для виртуалки)) с хоста или форвардится порт через локалхост.  
запустить nginx на centralServer.  
пробросить 80й порт на inetRouter2 8080.  
дефолт в инет оставить через inetRouter.  

### Решение ДЗ.  
1. Выполнение происходит автоматически, при помощи _Vagrant+Ansible_. После запуска всех машин заходим по _ssh_ на _inetRouter_ 
и во второй консоли на _centralRouter_. Проверяем правила _iptables_ на _inetRouter:  

![](https://github.com/Vitaliy7/firewall/blob/main/screenshots/1.png?raw=true)  

Видим, что доступ по _ssh_ на 22 порту зактрыт. Пробуем постучаться с _centralRouter_:  

_knock 192.168.150.1 2221:udp 3332:tcp 4443:udp_  

Снова проверяем правила _iptables_ на _inetRouter:  

![](https://github.com/Vitaliy7/firewall/blob/main/screenshots/2.png?raw=true)  

И видим, что появилось правило, разрешающее доступ на 22 порт. 

2. Добавить inetRouter2, который виден(маршрутизируется (host-only тип сети для виртуалки)) с хоста или форвардится порт через локалхост  
Реализовано в _ansible_  

3. Запустить nginx на centralServer  
Реализовано в _ansible_  

4. Пробросить 80й порт на inetRouter2 8080  
Реализовано в _ansible_  при помощи правил _iptables_  

5. Дефолт в инет оставить через inetRouter  
Реализовано в _ansible_, убираем маршрут по умолчанию для интерфейса eth0 (10.0.2.15 - это сеть по умолчанию для _virtualbox_) 
и прописываем шлюз по умолчанию для другого интерфейса, который смотрит на _centralRouter_.
