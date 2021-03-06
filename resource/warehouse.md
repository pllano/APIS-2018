# Точки отгрузки `warehouse`
## Склады, точки отгрузки, магазины
Пример запроса продавца и ответа торговой площадки
### Торговая площадка должна потдерживать запросы: `POST` `GET` `PUT`
- `POST /warehouse` Создание склада (точки отгрузки)
- `POST /warehouse/{id}` Ошибка
- `GET /warehouse` Список всех складов (Точек отгрузки)
- `GET /warehouse?{param}` Список всех складов с фильтром по параметрам
- `GET /warehouse/{id}` Данные конкретного склада по `id`
- `PUT /warehouse` Обновить данные складов
- `PUT /warehouse/{id}` Обновить данные конкретного склада по `id`

### Пример `GET` запроса с HTTP клиентом Guzzle

В параметре `relations` - мы указываем какие связанные данные получить,в нашем случае: `product` `user` `address`

``` php
use GuzzleHttp\Client as Guzzle;

// Взять public_key из конфигурации
$public_key = $config->get('public_key');

// Наш запрос
$data = [
    'public_key' => $public_key,
    'limit' => 10,
    'offset' => 0,
    'order' => "DESC",
    'sort' => "created",
    'state' => 1,
    'date_from' => "2017-12-07",
    'date_to' => "2017-12-14",
    'relations' => "product,user,address"
];

// Массив в URL-кодированную строку запроса
$data_query = http_build_query($data) . "\n";

// Формируем URL запроса
$uri = 'https://example.com/api/v1/json/marketplace-warehouse?'.$data_query;

// Подключаем Guzzle
$client = new Guzzle();

// Отправляем запрос
$response = $client->request('GET', $uri);

// Получаем тело ответа
$output = $response->getBody();

// json в массив
$records = json_decode($output, true);

// Работаем с массивом
if (isset($records['headers']['code'])) {
if ($records['headers']['code'] == '200') {
	$count = count($records['body']['items']);
	if ($count >= 1) {
		foreach($records['body']['items'] as $item)
		{
			print_r($item['item']);
		}
	}
}
}

// Вывести на экран json
print_r($records);
```
## Структура ответа торговой площадки на наш `GET` запрос в json формате
```json
{
  "header": {
    "status": "200 OK",
    "code": "200",
    "message": "OK",
    "message_id": "https:\/\/github.com\/pllano\/api-json-db\/blob\/master\/doc\/http-codes\/200.md"
  },
  "response": {
    "auth": "QueryKeyAuth",
    "total": "10"
  },
  "request": {
    "query": "GET",
    "resource": "supplier-order",
    "limit": "10",
    "offset": "0",
    "order": "DESC",
    "sort": "created",
    "state": "1",
    "date_from": "2017-12-07",
    "date_to": "2017-12-14",
    "relations": "product, user, address"
  },
  "body": {
    "items": [{
      "item": {
        "products": [
           {
             "product": {
               "product_id": "2001500",
               "status": 1,
               "name": "Type BrendName SerieName Articul",
               "price": "2500.50",
               "oldprice": "2500.50",
               "num": 2,
               "available": 20,
               "total_price": "5001.00",
               "currency_id": "UAH",
               "guarantee": 24,
               "pay_online": 1
             }
           }, {
             "product": {
               "product_id": "1000120",
               "status": 1,
               "name": "Ноутбук Asus X751NV (X751NV-TY001) Black",
               "type": "Ноутбук",
               "brand": "Asus",
               "type": "X751NV",
               "articul": "(X751NV-TY001) Black",
               "price": "5750.50",
               "oldprice": "5500.00",
               "num": 1,
               "available": 5,
               "total_price": "5500.00",
               "currency_id": "UAH",
               "guarantee": 36,
               "pay_online": 1
             }
           }
        ]
      }
    }],
        "id": "1234567890",
        "created": "2017-12-11 10:30",
        "status": 1,
        "delivery": "Novaposhta",
        "delivery_code": "1234567890121",
        "total_amount": "10501.00",
        "currency_id": "UAH",
        "comment": "Могу принять после 17:00",
        "user": {
          "fname": "Иванов",
          "iname": "Юрий",
          "oname": "Петрович",
          "phone": "380670000001",
          "email": "user@example.com"
        },
        "address": {
          "country": "Украина",
          "region": "Киевская область",
          "postal_code": 0,
          "city": "Киев",
          "district": "Позняки",
	  "street": "Бажана",
	  "number": "12а/17",
	  "parade": "0",
          "floor": "0",
	  "apartment": "75",
	  "additional": "Код на парадном 107"
        }
  }
}
```
