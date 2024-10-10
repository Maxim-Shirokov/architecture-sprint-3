# Задание 1.
## Функциональность монолита
1. Управление отоплением:
Пользователи могут удалённо включать/выключать отопление в своих домах.
Пользователи могут устанавливать желаемую температуру.
Система автоматически поддерживает заданную температуру, регулируя подачу тепла.
1. Мониторинг температуры:
Система получает данные о температуре с датчиков, установленных в домах.
Пользователи могут просматривать текущую температуру в своих домах через веб-интерфейс.
## Архитектура монолита
1. Стек: Java, PostgreSQL, 
1. Монолитная архитектура
1. Взаимодействие: синхронное (rest api)
1. Масштабируемость: ограниченная - тяжело/практически невозможно масштабировать по частям
1. Развертывание - требует редеплоя всего приложения


## Домены и границы контекстов
1. **Домен**: Управление отоплением
    1. поддомен включение/выключение отопления
    1. поддомен регулирования температуры
1. Мониторинг температуры
    1. поддомен получения температуры с датчиков
    1. поддомен просмотра температуры с веб интерфейса
## c4 Диаграмма
Находятся в папке /diagrams
## Документация
Находится в папке /docs

# Базовая настройка

## Запуск minikube

[Инструкция по установке](https://minikube.sigs.k8s.io/docs/start/)

```bash
minikube start
```


## Добавление токена авторизации GitHub

[Получение токена](https://github.com/settings/tokens/new)

```bash
kubectl create secret docker-registry ghcr --docker-server=https://ghcr.io --docker-username=<github_username> --docker-password=<github_token> -n default
```


## Установка API GW kusk

[Install Kusk CLI](https://docs.kusk.io/getting-started/install-kusk-cli)

```bash
kusk cluster install
```


## Настройка terraform

[Установите Terraform](https://yandex.cloud/ru/docs/tutorials/infrastructure-management/terraform-quickstart#install-terraform)


Создайте файл ~/.terraformrc

```hcl
provider_installation {
  network_mirror {
    url = "https://terraform-mirror.yandexcloud.net/"
    include = ["registry.terraform.io/*/*"]
  }
  direct {
    exclude = ["registry.terraform.io/*/*"]
  }
}
```

## Применяем terraform конфигурацию 

```bash
cd terraform
terraform apply
```

## Настройка API GW

```bash
kusk deploy -i api.yaml
```

## Проверяем работоспособность

```bash
kubectl port-forward svc/kusk-gateway-envoy-fleet -n kusk-system 8080:80
curl localhost:8080/hello
```


## Delete minikube

```bash
minikube delete
```
