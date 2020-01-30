# Ansible role: helm-charts

Устанавливает helm-чарты в кластер kubernetes. Работает через helm2 и helm3.

## Требования

Специальных требований нет.

В среде исполнения должны быть установлены клиенты git, kubectl и helm.

## Параметры

[Полный список](defaults/main.yaml) параметров, используемых ролью.

Версия helm для работы выбирается при помощи переменной helm_version (default: 3).

Если работа происходит через helm3 и не указан параметр helm_tiller, то helm связывается с tiller через механизм port-forwarding. Для этого требуется, чтобы на коиенте и сервере стоял пакет socat. Если параметр helm_tiller указан, то работа с tiller происходит напрямую.

Стандартные параметры при использовании существующей конфигурации клиента kubectl:
```
k8s_static_config: true
k8s_config: "~/.kube/config" # Путь до вашей конфигурации
k8s_context: "default"       # Имя контекста
```

Стандартные параметры при отсутствии конфигурации клиента:
```
k8s_server: https://api.kubernetes.org:6443   # Адрес API-сервера
k8s_ca: |                                     # Корневой сертификат сервера
k8s_client_cert: |                            # Клиентский сертификат
k8s_client_key: |                             # Клиентский ключ
k8s_username: "kube-admin"                    # Имя пользователя
k8s_password: "kube-admin-password"           # Пароль пользователя
```
Одновременное использование k8s_client-* и k8s_username не допускается.

Параметры для установки helm-репозиториев:
```
helm_repos:
  - name: stable
    url: https://kubernetes-charts.storage.googleapis.com/
    state: present
```

Параметры для установки чартов:
```
helm_charts:
- name: node-exporter
  chart: "stable/node-exporter"
  git_repo: "git@bitbucket.org:smartube/helm-node-exporter.git"
  git_path: ""
  namespace: "prometheus"
  state: present
  params: {}
```
Одновременное использование chart и git_repo не допускается. 

Словарь params в процессе работы роли преобразуется в values helm-чарта.
