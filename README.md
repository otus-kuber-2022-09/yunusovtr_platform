# yunusovtr_platform
yunusovtr Platform repository

## Домашнее задание к уроку №2

- Подготовил репозиторий yunusovtr-platform для работы, внёс необходимые файлы для работы автотестов
- Задание: Разберитесь почему все pod в namespace kube-system восстановились
  - поды для etcd, kube-apiserver, kube-controller-manager, kube-scheduler являются статичными и управляет ими kubelet ноды, которая в minikube имеет имя minikube. Манифесты для них находятся в обычных файлах /etc/kubernetes/manifests.
  - core-dns и kube-proxy - это Deployment и DaemonSet, манифесты которых сохраняются в базе etcd и становятся доступными после поднятия пода etcd.
- Подготовил Dockerfile для web-сервера nginx по условиям
- Создал манифест пода web, создал основной и инит-контейнеры, проверил работоспособность пода и открытие целевой страницы
- Склонировал репозиторий https://github.com/GoogleCloudPlatform/microservices-demo
- Собрал образ hipster-frontend, отправил в docker hub
- Задание со *: добавил все переменные окружения, необходимые для успешного старта контейнера, и статус пода после старта стал Running.

## Домашнее задание к уроку №3

- Запустил кластер kind состоящий из 4 нод
- Создал и запустил ReplicaSet, добавив описание селектора и переменных окружения
- Проскалировал ReplicaSet до 3, используя команду ad-hoc: kubectl scale rs frontend --replicas=3, и используя изменения в манифесте
- Обновление тэга образа контейнера в манифесте ReplicaSet не привело после применения манифеста к обновлению образов контейнеров подов, потому что обновление шаблонов контейнеров не относится к сфере ответственности ReplicaSet.
- Удалил все поды после изменения тега в манифесте и получил обновление образа в контейнерах.
- Собрал образ yunusovtr/hipster-paymentservice с тегами v0.0.1 и v0.0.2 и успешно запустил с ними репликасет с 3 репликами
- Скопировал манифест репликасета в деплоймент, стартовал
- Поменял тег образа в деплойменте и применил манифест, проследил роллинг апдейт подов, убедился, что тэги образов изменились
- Испытал откат деплоя
- Задание со *: изменил деплой для имитации Blue-Green и Reverse стратегий деплоя
- Создал деплоймент для frontend с readinessProbe
- Задание со *: запустил DaemonSet с node-exporter
- Задание с **: добавил tolerations в DaemonSet, чтобы запускался на мастер-ноде

## Домашнее задание к уроку №4

- Добавил readinessProbe и livenessProbe к подe web из каталога kubernetes-intro
- Добавление проверок Pod. Ответ на вопрос для самопроверки 1: для начала в контейнере нет утилиты ps, во-вторых отслеживать факт запущенности основного процесса контейнера не имет смысла, т.к. контейнер без него в принципе не будет работать, т.е. при работающем контейнере не будет такого момента, чтобы указанная команда не вернула бы код 0, при отсутствии процесса не будет контейнера и команда не будет запущена. Ответ на вопрос 2: имеет смысл, если в контейнере запущено более одного процесса и отслеживать нужно неосновной процесс.
- Создал web-deploy.yaml и каталог kubernetes-networks, применил и изменил для работоспособности деплоймента
- Самостоятельную работу провёл. Для начала приходится изменять каждый раз deployment в чём-то, чтобы он применялся, иначе kubectl сообщает об отсутствии изменений в deployment и никаких новых событий в кластере не происходит. Результаты:
  - при maxUnavailable=0 и maxSurge=100 сначала создались новые поды почти одновременно и только по готовности новых подов начали удаляться старые поды по одному.
  - при maxUnavailable=0 и maxSurge=0 деплой не применяется, возникает ошибка, что так нельзя
  - при maxUnavailable=100% и maxSurge=100% или maxSurge=0% старый репликасет сразу удалил все поды и одновременно начали создаваться новые поды.
- Создал сервис типа ClusterIP, установил режим IPVS для kube-proxy
- Развернул MetalLB v.0.13.7 в кластере
- Модифицировал конфиг под новую версию metallb, т.к. задание пулов айпи адресов там перенесено в CRD. Новый конфиг назвал metallb-config-new.yaml.
- Успешно развернул сервис с типом LoadBalancer и получил для него внешний IP из заданного пула адресов
- Указал статичный маршрут и успешно открыл в браузере на хостовой машине страничку из подов web
- Задание со *: добавил LoadBalancer сервис для coredns
- Добавил nginx ingress контроллер, запустил для него LoadBalancer сервис
- Создал headless сервис, к которому прикрутил ingress и проверил его работу
- Задание со *. Установил kubernetes dashboard, прокинул его через ingress по пути /dashboard
- Задание со *. Вывел дополнительный деплоймент, заменил содержание index.html там, настроил дополнительный ингресс, чтобы он адресовал от наличия http заголовка WebCanary со значение always
