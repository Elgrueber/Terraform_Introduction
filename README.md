# Домашнее задание 1: «Введение в Terraform»

## Задание 1.1 — Установка Terraform

* Установлена версия Terraform:

terraform -version
# Terraform v1.8.4 on windows_amd64

## Задание 1.2 — Анализ `.gitignore`

Файл `.gitignore` исключает:

* `.terraform/` — служебные директории Terraform;
* `*.tfstate`, `*.tfstate.*` — state-файлы, содержат чувствительные данные (пароли, ключи, токены);
* `personal.auto.tfvars` — может хранить приватные переменные (ключи, пароли).

**Вывод:** секретные файлы не должны попадать в репозиторий.

## Задание 1.3 — Выполнение кода и поиск секрета

Код выполнен, ресурс `random_password.random_string` создан.

**Фрагмент `terraform.tfstate`:**
<img width="173" height="19" alt="image" src="https://github.com/user-attachments/assets/ca146689-4360-4dbe-b0a9-253635c6a382" />

**Ответ:**

* Ресурс: `random_password.random_string`
* Значение: `ohAZ95bml0YhtO`

## Задание 1.4 — Исправление ошибок

После раскомментирования блоков Docker при валидации появилась ошибка.

**Исправление:**

random_password.random_string.result


**Результат:**

<img width="342" height="30" alt="image" src="https://github.com/user-attachments/assets/9104cc50-0bcf-4064-ace9-ca1b76457380" />


## Задание 1.5 — Применение кода и проверка контейнера

terraform apply -auto-approve
docker ps

Контейнер создан, порты проброшены: `0.0.0.0:9090->80/tcp`.

**Скриншоты:**

<img width="766" height="104" alt="image" src="https://github.com/user-attachments/assets/1002409a-1d75-4cfe-9e23-46898ab12abc" />


## Задание 1.6 — Переименование на hello\_world

В коде изменены ресурсы:

<img width="572" height="260" alt="image" src="https://github.com/user-attachments/assets/61eb86bd-1e23-4f54-9e74-89c46822fc8c" />


**Результат:** контейнер называется `hello_world`.

**Почему нельзя latest:**

* `latest` меняется без предупреждения;
* сломает воспроизводимость;
* фиксированная версия (например `nginx:1.25.2`) гарантирует стабильность.

**Скриншоты:**

<img width="1502" height="121" alt="image" src="https://github.com/user-attachments/assets/45d99f21-44ac-4825-bb91-baecf8301031" />


## Задание 1.7 — Уничтожение ресурсов

terraform destroy -auto-approve
docker ps

**Результат:** все ресурсы удалены, контейнеров нет.

**Скриншоты:**

<img width="618" height="46" alt="image" src="https://github.com/user-attachments/assets/7d897e2a-2a7e-42a6-8081-2dc8ab30b3bb" />


## Задание 1.8 — Docker-образы

Terraform удалил контейнер, но не удалил образ, потому что в коде установлен параметр keep_locally = true.

Это сделано для оптимизации: чтобы при повторных запусках не тратить время и трафик на загрузку образа из Docker Hub.

Если бы нужно было принудительно удалить образ при destroy, следовало бы использовать параметр force_remove = true.
Подтверждение из документации Terraform (провайдер docker, ресурс docker_image):
keep_locally (Boolean) If true, then the Docker image won't be deleted on destroy operation.

**Если нужно удалить:**

resource "docker_image" "hello_world" {
  name         = "nginx:1.25.2"
  keep_locally = true
  force_remove = true
}





Хочешь, я ещё сделаю финальный **main.tf** (с random\_password + hello\_world) в отдельный блок, чтобы его тоже можно было приложить в репо?
# Terraform_Introduction
