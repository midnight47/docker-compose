в docker-compose правим VIRTUAL_HOST ставим там наш домен

подымаем jira
```
docker-compose up -d
```


подробная информация тут:
```
https://sidmid.ru/%d0%bf%d0%be%d0%b4%d0%bd%d0%b8%d0%bc%d0%b0%d0%b5%d0%bc-%d1%81%d0%b5%d1%80%d0%b2%d0%b5%d1%80%d0%bd%d1%83%d1%8e-jira/
```

после установки, на моменте ввода ключа заходим в наш контейнер

```
docker exec -it jira bash
```

и выполняем команду:
```
java -jar /crack/atlassian-agent.jar -p jira -m admin@test.com -n MD -o http://test.com/ -s BLJN-UAPY-0SJL-STX6
```

получаем ключ и продолжаем установку


