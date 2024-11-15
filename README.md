# Начало

Перед началой установки, нужно установить Linux Oracle на VirtualBox, для этого нужно:

Иметь образ Linux

Выделить 2+ ядер.

Выделать 4096+ МБ оперативы.


# docker, compose, grafana
Далее переходим к установке docker с использованием grafana, вводим следующий набор команд:

1. `sudo yum install wget`

• устанавливает утилиту wget на вашу систему
![image](https://github.com/user-attachments/assets/3c74cb2a-58ae-49ff-aa1b-38574cef086d)

2. `sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo`

• Скачиваем файл репозитория
![image](https://github.com/user-attachments/assets/abc50d32-8c82-48d7-9516-4f3aab713436)

3. `sudo yum install docker-ce docker-ce-cli containerd.io`

• Устанавливаем docker
![image](https://github.com/user-attachments/assets/7fcfacaa-e5db-4526-ba43-da390ce7e82a)

4. `sudo systemctl enable docker --now`

• Запускаем его и разрешаем автозапуск

5. `sudo yum install curl`

• Для этого сначала убедимся в наличие пакета curl
![image](https://github.com/user-attachments/assets/f378b717-d7dc-446a-9cbc-e5e387cdfee2)

6. `COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)`

• Объявление переменной COMVER, полученной в результате curl запроса, хранящей в себе номер последней
версии Docker Compose

7. `sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose`                        

• Теперь скачиваем скрипт docker-compose последней версии, используя объявленную ранее переменную и помещаем его в каталог /usr/bin

![image](https://github.com/user-attachments/assets/befbb351-0184-4810-ae34-93a70b5eab97)

8. `sudo chmod +x /usr/bin/docker-compose`

• Предоставление прав на выполнение файла docker-compose.

9. `docker-compose --version`

• Проверка установленной версии Docker Compose.

![image](https://github.com/user-attachments/assets/55cd797c-9c6e-46ed-b243-14ce6ed80a56)

10. Установка git

`sudo yum install git`

![image](https://github.com/user-attachments/assets/a5595a4a-f962-4750-8fb0-b0e2b6e90883)

Этот код скачивает содержимое репозитория skl256/grafana_stack_for_docker

`sudo git clone https://github.com/skl256/grafana_stack_for_docker.git`

Заходит в папку - cd

`cd grafana_stack_for_docker`

cd .. - возвращает в папку выше

![image](https://github.com/user-attachments/assets/84ca8ad7-d109-4624-9dee-1bfd5946fc57)

Cоздаем папки двумя разными способами

`sudo mkdir -p /mnt/common_volume/swarm/grafana/config`

`sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data,loki-data,promtail-data}`

Выдаем права

`sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}`

Создаем файл

`sudo touch /mnt/common_volume/grafana/grafana-config/grafana.ini`

Копирование файлов

`sudo cp config/* /mnt/common_volume/swarm/grafana/config/`

Переименовывание файла

`sudo mv grafana.yaml docker-compose.yaml`

![image](https://github.com/user-attachments/assets/2f6443f9-282f-40c0-9f86-699f6c4f4bd0)

Собрать докер (нужно запускать из папки где docker-compose.yaml)

`sudo docker compose up -d`

Опустить докер - sudo docker compose stop

![image](https://github.com/user-attachments/assets/1680e53d-45eb-4218-ac93-a430d86dd96f)
![image](https://github.com/user-attachments/assets/dfd8b24a-a43e-4789-908f-cc6b092164ab)


# Делаем чистку файлов

Команда открывает файл docker-compose.yaml в текстовом редакторе vi с правами суперпользователя

`sudo vi docker-compose.yaml`

Что-бы что-то изменить в тесковом редакторе нужно нажать insert на клавиатуре

Затем в docker-compose нужно вставить node-exporter и удалить ненужные файлы (можно вставить готовый докер)

![image](https://github.com/user-attachments/assets/1f763083-5811-41c8-b300-ee14d416867b)

выйти не сохраняясь из vim - `esc -> :q!`

выйти сохраняясь из vim - `esc -> :wq!`

Заходим в другую папку 

`cd /mnt/common_volume/swarm/grafana/config/`

Открываем файл prometheus.yaml в текстовом редакторе vi с правами суперпользователя

`sudo vi prometheus.yaml`

Далее нужно исправить targets: на exporter:9100

![image](https://github.com/user-attachments/assets/0d2f1f13-dc27-4d99-9bcf-466e2674d5a4)

# Grafana на сайте

User & Password GRAFANA: admin
Код графаны: 3000
Код прометеуса: http://prometheus:9090

переходим на сайт localhost:3000
в меню выбираем вкладку Dashboards и создаем Dashboard
ждем кнопку +Add visualization, а после "Configure a new data source"
выбираем Prometheus

Connection
http://prometheus:9090

Authentication
Basic authentication
User: admin
Password:admin

в меню выбираем вкладку Dashboards и создаем Dashboard
ждем кнопку "Import dashboard"
Find and import dashboards for common applications at grafana.com/dashboards: 1860  //ждем кнопку Load
Select Prometheus
ждем кнопку "Import"

![image](https://github.com/user-attachments/assets/0629ea9e-b996-4b2e-9b36-a24fd72e249e)

# VictoriaMetrics
Для начала изменим docker-compose.yaml

1. `cd grafana_stack_for_docker`

• команда cd grafana_stack_for_docker изменяет текущий рабочий каталог на каталог grafana_stack_for_docker.

2. `sudo vi docker-compose.yaml`

• команда sudo открывает файл docker-compose.yaml в редакторе vi с правами суперпользователя.

В самом текстовом редакторе после prometheus вставляем

Захом в connection
там где мы писали http//:prometheus:9090 пишем http:victoriametrics:9090 И заменяем имя из "Prometheus-2" в "Vika"
нажимаем на dashboards add visualition выбираем "Vika"
снизу меняем на "code"
Переходим в терминал и пишем

3. `echo -e "# TYPE OILCOINT_metric1 gauge\nOILCOINT_metric1 0" | curl --data-binary @- http://localhost:8428/api/v1/import/prometheus  `

• команда отправляет бинарные данные (например, метрики в формате Prometheus) на локальный сервер, который слушает на порту 8428.

4. `curl -G 'http://localhost:8428/api/v1/query' --data-urlencode 'query=OILCOINT_metric1'`

• команда делает запрос к API для получения данных по метрике OILCOINT_metric1

• команда выводит текст, который может быть использован для определения метрики в формате, совместимом с Prometheus

• команда выводит информацию о типе и значении этой метрики в формате, который может быть использован системой мониторинга Prometheus.

![image](https://github.com/user-attachments/assets/e14d5752-aee1-4242-980a-317554d77564)

Значение 0 меняем на любое другое

Копируем переменную OILCOINT_metric1 и вставляем в query

Нажимаем run

![image](https://github.com/user-attachments/assets/b90a007a-8dd6-4d60-8f1a-15d03f6b7773)
![image](https://github.com/user-attachments/assets/1c366cbe-d59f-4a81-83af-fd65b319cb5f)

Копируем переменную OILCOINT_metric1 и вставляем в code

![image](https://github.com/user-attachments/assets/6708a5d9-3f72-4051-94f9-1618044d4764)

