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
