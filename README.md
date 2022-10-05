# yunusovtr_platform
yunusovtr Platform repository

## Домашнее задание к уроку №2

- Подготовил репозиторий yunusovtr-platform для работы, внёс необходимые файлы для работы автотестов
- Задание: Разберитесь почему все pod в namespace kube-system восстановились
  - поды для etcd, kube-apiserver, kube-controller-manager, kube-scheduler являются статичными и управляет ими kubelet ноды, которая в minikube имеет имя minikube. Манифесты для них находятся в обычных файлах /etc/kubernetes/manifests.
  - core-dns и kube-proxy - это Deployment и DaemonSet, манифесты которых сохраняются в базе etcd и становятся доступными после поднятия пода etcd.
- 