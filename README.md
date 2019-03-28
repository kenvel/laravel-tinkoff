# Simple Tinkoff bank acquiring library.
Простая библиотека для приема платежей через интернет для Тинькофф банк.

### Возможности

 * Генерация URL для оплаты товаров
 * Подттверждение платежа
 * Просмотр статуса платжа
 * Отмена платежа

### Установка

С помощью [Composer](https://getcomposer.org/):

```php
composer require kenvel/laravel-tinkoff
```

Подключение в контроллере:

```php
use Kenvel\Tinkoff;
```

## Примеры использования
### 1. Инициализация

```php
$api_url    = 'https://securepay.tinkoff.ru/v2/';
$terminal   = '152619634343';
$secret_key = 'terminal_secret_password';

$tinkoff = new Tinkoff($api_url, $terminal, $secret_key);
```

### 2. Получить URL для оплаты
```php
//Подготовка массива с данными об оплате
$payment[
    'OrderId'       => '123456',        //Ваш идентификатор платежа
    'Amount'        => '100',           //сумма всего платежа в рублях
    'Language'      => 'ru',            //язык - используется для локализации страницы оплаты
    'Description'   => 'Some buying',   //описание платежа
    'Email'         => 'user@email.com',//email покупателя
    'Phone'         => '89099998877',   //телефон покупателя
    'Name'          => 'Customer name', //Имя покупателя
    'Taxation'      => 'usn_income'     //Налогооблажение
];

//подготовка массива с покупками
$items[] = [
    'Name'  => 'Название товара',
    'Price' => '100',    //цена товара в рублях
    'NDS'   => 'vat20',  //НДС
];

//Получение url для оплаты
$paymentURL = $tinkoff->paymentURL($payment, $items);

//Контроль ошибок
if(!$paymentURL){
  echo($tinkoff->error);
} else {
  $payment_id = $tinkoff->payment_id;
  header('Location: ' . $paymentURL);
}
```

### 3. Получить статус платежа
```php
//$payment_id Идентификатор платежа банка (полученый в пункте "2 Получить URL для оплаты")

$status = $tinkoff->getState($payment_id)

//Контроль ошибок
if(!$status){
  echo($tinkoff->error);
} else {
  echo($status);
}
```

### 4. Отмена платежа
```php
$status = $tinkoff->cencelPayment($payment_id)

//Контроль ошибок
if(!$status){
  echo($tinkoff->error);
} else {
  echo($status);
}
```

### 5. Подтверждение платежа
```php
$status = $tinkoff->confirmPayment($payment_id)

//Контроль ошибок
if(!$status){
  echo($tinkoff->error);
} else {
  echo($status);
}
```

---

[![Donate button](https://www.paypal.com/en_US/i/btn/btn_donate_SM.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=FGCHZNKKVG622&source=url)

*Если вы нашли этот проект полезным, пожалуйста сделайте небольшой донат - это поможет мне улучшить код*
