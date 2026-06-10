# Домашнее задание занятию `«Запуск приложений в K8S»` - `Чернышов Андрей`
 
## Задание 1. Создать Deployment и обеспечить доступ к репликам приложения из другого Pod.  

**Файл:** [`01-deployment.yaml`](./01-deployment.yaml)

Решенная проблема: Конфликт портов (оба контейнера пытались использовать 80). Исправлено через переменную окружения HTTP_PORT.  

![1](https://github.com/ANDREYTOLOGY/k8s-hw/blob/main/img/k8s2-1.png)  

На скриншоте показан процесс создания `Deployment app-multicontainer` и его масштабирование до 2 реплик.  
Видно успешное применение манифеста, запуск пода в статусе `2/2 Running`, а после команды `kubectl scale` — появление двух работающих подов.  

**Файл:** [`02-service.yaml`](./02-service.yaml)

![1](https://github.com/ANDREYTOLOGY/k8s-hw/blob/main/img/k8s2-2.png)  

`Service` успешно создан с типом `ClusterIP`, получил внутренний IP и два порта (80 и 8080), а команда `kubectl get endpoints` подтверждает, что сервис видит IP-адреса подов.  

![1](https://github.com/ANDREYTOLOGY/k8s-hw/blob/main/img/k8s2-3.png)  

Команда `curl -I http://app-service:80` вернула ответ от `nginx (HTTP 200 OK)`, а `curl http://app-service:8080` — ответ от `multitool`, подтверждая корректную работу Service и доступ к обоим контейнерам.  

## Задание 2. Создать Deployment и обеспечить старт основного контейнера при выполнении условий

**Файл** [`04-deployment-init.yaml`](/04-deployment-init.yaml)  

![3](https://github.com/ANDREYTOLOGY/k8s-hw/blob/main/img/k8s-5.png)      
 

На скриншоте продемонстрирована работа `Init Container`: после пересоздания `Deployment Pod` находится в состоянии `0/1 Init:0/1`, а после применения `Service app-service` успешно переходит в `1/1 Running`.
