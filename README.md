# Алгоритм

Выбран естественный ключ идемпотентности - ид заказа, который генерируется на стороне клиента

![image](https://github.com/YuliyaNemchaninova/Order-2/assets/47818848/305828f8-abfd-4c9b-84c7-bd4ec8c8152b)


# **Установка**

# Часть 0 Запуск minikube

minikube start

minikube addons enable ingress

minikube ip

Прописать <ip> arch.homework в системном файле etc/hosts

# Часть 1 Запуск **PosgtreSQL**

cd <путь в папку>\minikube deployment\postgresql

helm repo update

kubectl apply -f local-pv.yaml -f pv-claim.yaml

helm install postgresql-dev -f values.yaml bitnami/postgresql

# Часть 2 У**становить сервисы**

**inventory-сервис**

cd <путь в папку>\minikube deployment\inventory-deployment

kubectl apply -f inventory-config-map.yaml -f inventory-secret.yaml -f dp.yaml -f service.yaml

**billing-сервис**

cd <путь в папку>\minikube deployment\billing-deployment

kubectl apply -f billing-config-map.yaml -f billing-secret.yaml -f dp.yaml -f service.yaml

**order-сервис**

cd <путь в папку>\minikube deployment\order-deployment

kubectl apply -f order-config-map.yaml -f order-secret.yaml -f dp.yaml -f service.yaml

kubectl apply -f ingress.yaml

# Часть 3 Тестирование

В другом окне Командной строки

minikube tunnel

Выполнить тесты
newman run Order-2.postman_collection.json --env-var "url=http://arch.homework” --env-var "inventoryUrl=http://arch.homework” --env-var "billingUrl=http://arch.homework”

Тест 1. 
Шаги выполнения: 
1. Создать счет
2. Создать товар
3. Создать заказ с id N
4. Проверить баланс счета и количество доступного товара
Ожидаемый результат:
1. Заказ создан
2. Баланс клиента уменьшился
3. Количество доступного товара уменьшилось

Тест 2. 
Шаги выполнения: 
1. Повторно попробовать создать заказ с id N
4. Проверить баланс счета и количество доступного товара
Ожидаемый результат:
1. Заказ не создан повторно. В ответе вернулись параметры первого заказа
2. Баланс клиента не изменился
3. Количество доступного товара не изменилось


Результат тестирования

![image](https://github.com/YuliyaNemchaninova/Order-2/assets/47818848/eae727fd-c70e-40f2-9a29-be99b8084767)
