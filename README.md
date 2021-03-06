# APIS
«APIS» — Стандарт запросов и ответов для приложений и платформ использующих RESTful API интерфейс.

## Теперь вам не нужно изобретать велосипед !
Мы предлагаем один понятный, хорошо документированный и очень удобный стандарт обмена информацией между базами данных, приложениями, платформами, интернет-магазинами, поставщиками товаров и услуг, через RESTful API интерфейс.

Вы можете писать свою API зная что другим API использующим стандарт APIS не придется тратиться на дополнительную доработку и интеграцию с вашим API. Для подключения к вашему API будет необходимо только получить данные аутентификации для доступа к учетной записи. Пример использования: [pllano-api](https://github.com/ruslan-avantis/pllano-api)

Этот стандарт используют: [«API Shop»](https://github.com/ruslan-avantis/api-shop) и база данных [«jsonDB»](https://github.com/ruslan-avantis/json-db)

Примеры демо запросов: [demo](https://github.com/ruslan-avantis/json-db/blob/master/demo.md)

## Стандарт APIS стоит на двух основных принципах: 
- Адреса ресурсов и их назначение должны быть одинаковыми
- Структура ответов и запросов должна быть одинаковая

### 1. Адреса ресурсов и их назначение должны быть одинаковыми
Например:
- ресурс `/order` обрабатывает только `POST` `GET` `PUT` запросы и работает с заказами.
- ресурс `/price` обрабатывает только `GET` запросы и выдает информацию о ценах и наличии товара.
- ресурс `/product` обрабатывает только `GET` запросы и выдает информацию о товаре.
- ресурс `/search` обрабатывает только `GET` запросы и его основная задача обслуживание поисковых запросов.

### URL API
- URL: `https://example.com/{api}/{version}/{type}/{resource}/{id}`
- или
- URL: `https://{api}.example.com/{version}/{type}/{resource}/{id}`
- `{api}` - директория API по умолчанию `api`
- `{version}` - версия API по умолчанию `v1`
- `{type}` - тип данных `json`
- `{resource}` - название ресурса к которому обращаемся. Например `price` или `order`.
- `{id}` - уникальный индефикатор записи
- `{param}` - праметры запроса
#### Типы запросов: `POST` `GET` `PUT` `PATCH` `DELETE`
- `POST /{resource}` Создание записи 
- `POST /{resource}/{id}` Ошибка
- `GET /{resource}` Список всех записей
- `GET /{resource}?{param}` Список всех записей с фильтром по параметрам
- `GET /{resource}/{id}` Данные конкретной записи
- `PUT /{resource}` Обновить данные записей
- `PUT /{resource}/{id}` Обновить данные конкретной записи
- `PATCH /{resource}` Обновить данные записей
- `PATCH /{resource}/{id}` Обновить данные конкретной записи
- `DELETE /{resource}` Удалить все записи
- `DELETE /{resource}/{id}` Удалить конкретную запись
#### Параметры GET запроса
`?offset={offset}&limit={limit}&order={order}&sort={sort}&public_key={public_key}`
- `{public_key}` - Ключ доступа к API
- `{limit}` - Записей на страницу. По умолчанию `30`
- `{offset}` - Страница. По умолчанию `0`
- `{order}` - Тип сортировки. По умолчанию `DESC`
- `{sort}` - Поле сортировки. По умолчанию `id`
- `{*}` - Пользовательский параметр
#### При использовании только `GET` запросов : Не обязательно !
Если вы хотите дать возможность обращаться к вашему API только с GET запросами, логика URL должна быть следующей:
- `GET /_post/{resource}?{param}` Создание записи 
- `GET /_post/{resource}/{id}` Ошибка
- `GET /_get/{resource}?{param}` Список всех записей с фильтром по параметрам
- `GET /_get/{resource}/{id}` Данные конкретной записи
- `GET /_put/{resource}?{param}` Обновить данные записей
- `GET /_put/{resource}/{id}?{param}` Обновить данные конкретной записи
- `GET /_patch/{resource}?{param}` Обновить данные записей
- `GET /_patch/{resource}/{id}?{param}` Обновить данные конкретной записи
- `GET /_delete/{resource}` Удалить все записи
- `GET /_delete/{resource}/{id}` Удалить конкретную запись

### Транзитная API
#### Типы запросов : `POST` `GET` `PUT` `PATCH` `DELETE`
`{service_name}` - Название сервиса который будет выполнять запрос к транзитной API

- `POST /{service_name}/{resource}` Создание записи
- `POST /{service_name}/{resource}/{id}` Ошибка
- `GET /{service_name}/{resource}` Список всех записей
- `GET /{service_name}/{resource}?{param}` Список всех записей
- `GET /{service_name}/{resource}/{id}` Данные конкретной записи
- `PUT /{service_name}/{resource}` Обновить данные записей
- `PUT /{service_name}/{resource}/{id}` Обновить данные конкретной записи
- `PATCH /{service_name}/{resource}` Обновить данные записей
- `PATCH /{service_name}/{resource}/{id}` Обновить данные конкретной записи
- `DELETE /{service_name}/{resource}` Удалить все записи
- `DELETE /{service_name}/{resource}/{id}` Удалить конкретную запись

Пример реализации: [api-shop/api](https://github.com/ruslan-avantis/api-shop/tree/master/api)
##### Пример ответа от LiqPay в виде GET запроса к ресурсу `/liqpay/pay`
`https://example.com/api/v1/json/liqpay/pay/1234?status=success`

## Ресурсы
Какие поля должны поддерживать ресурсы вы можете посмотреть в структуре базы данных [db.json](https://github.com/ruslan-avantis/db.json)
### Список всех поддерживаемых ресурсов
Использование всех ресурсов не является обязательным. Вы можете использовать только необходимые вам ресурсы.
- `/test` - `GET`, `POST`, `PUT`, `PATCH`, `DELETE` - Для тестов подключения
- [`/install`](https://github.com/ruslan-avantis/APIS/blob/master/resource/install.md) `POST`, `PUT` - Установка
- `/language` - `GET` - Локализация (без public_key)
- `/stores_list` - `GET` - Список магазинов (без public_key)
- `/templates_list` - `GET` - Список шаблонов (без public_key)
- [`/price`](https://github.com/ruslan-avantis/APIS/blob/master/resource/price.md) `GET` - Каталог товаров продавца
- [`/site`](https://github.com/ruslan-avantis/APIS/blob/master/resource/site.md) `GET` - Конфигурация сайта
- [`/cart`](https://github.com/ruslan-avantis/APIS/blob/master/resource/cart.md) `GET`, `POST`, `PUT`, `PATCH`, `DELETE` - Корзина (товары в корзине)
- [`/order`](https://github.com/ruslan-avantis/APIS/blob/master/resource/order.md) `GET`, `POST`, `PUT`, `PATCH` - Заказы
- [`/user`](https://github.com/ruslan-avantis/APIS/blob/master/resource/user.md) `GET`, `POST` - Пользователи
- [`/warehouse`](https://github.com/ruslan-avantis/APIS/blob/master/resource/warehouse.md) `GET`, `POST`, `PUT`, `PATCH` - Склады, точки отгрузки, магазины
- [`/delivery`](https://github.com/ruslan-avantis/APIS/blob/master/resource/delivery.md) `POST` - Информация о статусе груза
- [`/pay`](https://github.com/ruslan-avantis/APIS/blob/master/resource/pay.md) `GET` - Платежи
- [`/checkout`](https://github.com/ruslan-avantis/APIS/blob/master/resource/checkout.md) `GET`, `POST` - Создание платежей
- [`/payment`](https://github.com/ruslan-avantis/APIS/blob/master/resource/payment.md) `POST` - Информация о статусе платежа
- [`/price-list`](https://github.com/ruslan-avantis/APIS/blob/master/resource/price-list.md) `GET`, `POST`, `PUT`, `PATCH`, `DELETE` - Каталог товаров от поставщиков
- [`/discount`](https://github.com/ruslan-avantis/APIS/blob/master/resource/discount.md) `GET` - Акции
- [`/category`](https://github.com/ruslan-avantis/APIS/blob/master/resource/category.md) `GET` - Категории товаров
- [`/brand`](https://github.com/ruslan-avantis/APIS/blob/master/resource/brand.md) `GET` - Бренды
- [`/sitemap`](https://github.com/ruslan-avantis/APIS/blob/master/resource/sitemap.md) `GET` - Карта сайта
- [`/export`](https://github.com/ruslan-avantis/APIS/blob/master/resource/export.md) `GET` - Экспорт
- [`/review`](https://github.com/ruslan-avantis/APIS/blob/master/resource/review.md) `GET`, `POST` - Отзывы
- [`/article`](https://github.com/ruslan-avantis/APIS/blob/master/resource/article.md) `GET` - Статьи
- [`/article-category`](https://github.com/ruslan-avantis/APIS/blob/master/resource/article-category.md) `GET` - Категории статей
- [`/seller`](https://github.com/ruslan-avantis/APIS/blob/master/resource/seller.md) `GET`, `POST`, `PUT`, `PATCH` - Продавцы
- [`/feedback`](https://github.com/ruslan-avantis/APIS/blob/master/resource/feedback.md) `POST` - Сообщить об ошибке

Если вы не нашли необходимого ресурса, напишите нам и предложите структуру этого ресурса. Если ресурс ключевой мы его добавим в общий список.

### 2. Структура ответов и запросов должна быть одинаковая

Глобальная структура ответа всех ресурсов должна иметь следующую форму:
- [`headers`](https://github.com/ruslan-avantis/APIS/blob/master/structure/headers.md) - заголовки
- [`request`](https://github.com/ruslan-avantis/APIS/blob/master/structure/request.md) - запрос
- [`response`](https://github.com/ruslan-avantis/APIS/blob/master/structure/response.md) - ответ
- [`head`](https://github.com/ruslan-avantis/APIS/blob/master/structure/head.md) - голова (не обязательно)
- [`body`](https://github.com/ruslan-avantis/APIS/blob/master/structure/body.md) - тело
- [`footer`](https://github.com/ruslan-avantis/APIS/blob/master/structure/footer.md) - подвал (не обязательно)

### Структура ответа API при ошибке аутентификации
```json
{
    "headers": {
        "status": "401 Unauthorized",
        "code": 401,
        "message": "Access is denied. The authentication method does not match.",
        "message_id": "https:\/\/github.com\/pllano\/APIS\/tree\/master\/http-codes\/401.md"
    },
    "request": {
        "Параметр запроса": "Значение параметра"
    },
    "response": {
        "auth": "CryptoAuth",
        "total": 0
    },
    "body": {
        "items": []
    }
}
```
### [Коды состояния HTTP](https://github.com/ruslan-avantis/APIS/tree/master/http-codes)

Вы можете использовать документацию кодов состояния HTTP APIS, это удобно, так как вам не нужно писать и поддерживать свою документацию.

#### Ипользование кодов состояния HTTP APIS:
Достаточно подставлять код ошибки `$code` в URL
```php
$code = 200;
$message_id = "https://github.com/ruslan-avantis/APIS/tree/master/http-codes/".$code.".md";
```

### Минимальная структура ответа API
```json
{
    "headers": {
        "status": "200 OK",
        "code": 200,
        "message": "RESTfull API json DB works!",
        "message_id": "https:\/\/github.com\/pllano\/APIS\/tree\/master\/http-codes\/200.md"
    },
    "request": {
        "Параметр запроса": "Значение параметра"
    },
    "response": {
        "auth": "CryptoAuth",
        "total": 0
    },
    "body": {
        "items": []
    }
}
```

### Параметр [`relations`](https://github.com/ruslan-avantis/APIS/blob/master/structure/relations.md)
[`relations`](https://github.com/ruslan-avantis/APIS/blob/master/structure/relations.md) - Очень важный параметр запроса позволяющий получать в ответе необходимые данные из других связанных ресурсов.
 
Два формата передачи параметров `relations`
- Передача дополнительных параметров в `json` формате с последующим кодированием данных в формат MIME base64 функцией base64_encode. Строка с параметрами или если необходимо получить все поля  `"all"`
- Передача дополнительных параметров в строке с разделителями для ресурсов `,` и `:` для необходимых полей 

Например в нашем запросе к ресурсу `order` мы хотим дополнительно получить следующие данные: 
- `cart` - товары в заказе
- `user` - данные покупателя
- `address` - адрес покупателя

### `GET` - Получение данных через [`RouterDb`](https://github.com/ruslan-avantis/router-db)
```php
use RouterDb\Db;
 
// Подключаемся к api или базе данных mysql
$db = new Db("api", $config);
// Передача дополнительных параметров в строке с разделителями
$relations = "cart,user:phone:email:fname:iname,address";
 
// Массив с данными запроса
$getArr = [
    "limit" => 10,
    "offset" => 0,
    "order" => "DESC",
    "sort" => "created",
    "state" => 1
    "relations" => $relations
];
 
$response = $db->get("order", $getArr);
 
```
### `GET` запрос к `API` ресурс `order` через HTTP клиент Guzzle
Пример URL https://example.com/json-db/order?relation=address,cart,user:user_id:iname:oname:phone:email
``` php
use GuzzleHttp\Client as Guzzle;

// Взять public_key из конфигурации
$public_key = $config->get("public_key");

// Если нужно только необходимые поля
$relations = base64_encode('{
    "cart": "all",
    "user": ["phone","email","fname","iname"],
    "address": "all"
}');

// Или более простой вариант: ресурсы через запятую, а необходимые поля через двоеточие
$relations = "cart,user:phone:email:fname:iname,address";

// Наш запрос
$data = [
    "public_key" => $public_key,
    "limit" => 10,
    "offset" => 0,
    "order" => "DESC",
    "sort" => "created",
    "state" => 1
    "relations" => $relations
];

// Массив в URL-кодированную строку запроса
$data_query = http_build_query($data) . "\n";
// Формируем URL запроса
$uri = "https://example.com/api/v1/json/order?".$data_query;
// Подключаем Guzzle
$client = new Guzzle();
// Отправляем запрос
$resp = $client->request("GET", $uri);
// Получаем тело ответа
$get_body = $resp->getBody();
// json в массив
$response = json_decode($get_body, true);
 
```
### Обрабатываем ответ
``` php
if (isset($response["headers"]["code"])) {
    if ($response["headers"]["code"] == 200) {
        $count = count($response["body"]["items"]);
        if ($count >= 1) {
            foreach($response["body"]["items"] as $item)
            {
                // Если $value object переводим в array
                $item = is_array($value["item"]) ? $item["item"] : (array)$value["item"];
                // Получаем данные
                print_r($item["name"]);
            }
        }
    }
}
```
### Вывести результат
``` php
print_r($response);
```
```php
$records = array (
  'headers' => array (
    'status' => '200 OK',
    'code' => '200',
    'message' => 'OK',
    'message_id' => 'https://github.com/ruslan-avantis/APIS/tree/master/http-codes/200.md',
  ),
  'response' => array (
    'auth' => 'QueryKeyAuth',
    'total' => '10',
  ),
  'request' => array (
    'query' => 'GET',
    'resource' => 'order',
    'limit' => '10',
    'offset' => '0',
    'order' => 'DESC',
    'sort' => 'created',
    'state' => '1',
    'relations' => array (
      'product' => 'all',
      'user' => array (
        0 => 'phone',
        1 => 'email',
        2 => 'fname',
        3 => 'iname',
        4 => 'oname',
      ),
      'address' => 'all',
    ),
  ),
  'body' => array (
    'items' => array (
      0 => array (
        'item' => array (
          'id' => '1234567890',
          'created' => '2017-12-11 10:30',
          'status' => 1,
          'delivery' => 'Novaposhta',
          'delivery_code' => '1234567890121',
          'total_amount' => '10501.00',
          'currency_id' => 'UAH',
          'comment' => 'Могу принять после 17:00',
          'user' => array (
            0 => array (
              'phone' => '380670000001',
              'email' => 'user@example.com',
              'fname' => 'Иванов',
              'iname' => 'Юрий',
              'oname' => 'Петрович',
            ),
          ),
          'cart' => array (
            0 => array (
              'product_id' => '1000120',
              'name' => 'Ноутбук Asus X751NV (X751NV-TY001) Black',
              'type' => 'Ноутбук',
              'brand' => 'Asus',
              'serie' => 'X751NV',
              'articul' => '(X751NV-TY001) Black',
              'price' => '5750.50',
              'oldprice' => '5500.00',
              'num' => 1,
              'available' => 5,
              'total_price' => '5500.00',
              'currency_id' => 'UAH',
              'guarantee' => 36,
              'pay_online' => 1,
            ),
            1 => array (
              'product_id' => '1000120',
              'name' => 'Ноутбук Asus X751NV (X751NV-TY001) Black',
              'type' => 'Ноутбук',
              'brand' => 'Asus',
              'serie' => 'X751NV',
              'articul' => '(X751NV-TY001) Black',
              'price' => '5750.50',
              'oldprice' => '5500.00',
              'num' => 1,
              'available' => 5,
              'total_price' => '5500.00',
              'currency_id' => 'UAH',
              'guarantee' => 36,
              'pay_online' => 1,
            ),
          ),
          'address' => array (
            0 => array (
              'country' => 'Украина',
              'region' => 'Киевская область',
              'postal_code' => 0,
              'city' => 'Киев',
              'district' => 'Позняки',
              'street' => 'Бажана',
              'number' => '12а/17',
              'parade' => '0',
              'floor' => '0',
              'apartment' => '75',
              'additional' => 'Код на парадном 107',
            ),
          ),
        ),
      ),
    ),
  ),
);
```
## Аутентификация
Вы должны поддерживать один из нижеуказанных методов аутентификации.

### LoginPasswordAuth

[«LoginPasswordAuth»](https://github.com/ruslan-avantis/APIS/blob/master/doc/LoginPasswordAuth.md) — реализация базовой Http аутентификации. Пара логин пароль передаются в виде строки `login:password` в `Authorization` заголовке. [подробнее ...](https://github.com/ruslan-avantis/APIS/blob/master/doc/LoginPasswordAuth.md)

### HttpTokenAuth
[«HttpTokenAuth»](https://github.com/ruslan-avantis/APIS/blob/master/doc/HttpTokenAuth.md) — реализация аутентификации по токену `HTTP Bearer token`. Для авторизации с помощью токена нужно передать его в `http` заголовке.  [подробнее ...](https://github.com/ruslan-avantis/APIS/blob/master/doc/HttpTokenAuth.md)

### QueryKeyAuth
[«QueryKeyAuth»](https://github.com/ruslan-avantis/APIS/blob/master/doc/QueryKeyAuth.md) — пожалуй самый простой из имеющихся способов авторизации. Работает по тому-же принципу, что и `HttpTokenAuth`, т.е. идентифицирует пользователя по токену, только в данном случае токен передается в строке запроса, как GET параметр. По умолчанию параметр называется `public_key`. [подробнее ...](https://github.com/ruslan-avantis/APIS/blob/master/doc/QueryKeyAuth.md)

### CryptoAuth
[«CryptoAuth»](https://github.com/ruslan-avantis/APIS/blob/master/doc/CryptoAuth.md) работает по тому-же принципу, что и [`QueryKeyAuth`](https://github.com/ruslan-avantis/APIS/blob/master/doc/QueryKeyAuth.md), т.е. токен передается как `GET` или `POST` параметр, только в данном случае данные передаются в зашифрованном виде с помощью `private_key` библиотекой [defuse/php-encryption](https://github.com/defuse/php-encryption) или подобной. [подробнее ...](https://github.com/ruslan-avantis/APIS/blob/master/doc/CryptoAuth.md)

### Мы ищем единомышленников — Присоединяйтесь!

<a name="feedback"></a>
## Поддержка, обратная связь, новости

Общайтесь с нами через почту open.source@pllano.com

Если вы нашли баг в APIS загляните в
[issues](https://github.com/ruslan-avantis/APIS/issues), возможно, про него мы уже знаем и
чиним. Если нет, лучше всего сообщить о нём там. Там же вы можете оставлять свои
пожелания и предложения.

За новостями вы можете следить по
[коммитам](https://github.com/ruslan-avantis/APIS/commits/master) в этом репозитории.
[RSS](https://github.com/ruslan-avantis/APIS/commits/master.atom).

Лицензия APIS
-------

The MIT License (MIT). Please see [LICENSE](LICENSE.md) for more information.
