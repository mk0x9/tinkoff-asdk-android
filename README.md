# Tinkoff Acquiring SDK for Android

[![Maven Central](https://img.shields.io/maven-central/v/ru.tinkoff.acquiring/ui.svg?maxAge=2592000)][search.maven]

Acquiring SDK позволяет интегрировать [Интернет-Эквайринг Tinkoff][acquiring] в мобильные приложения для платформы Android.

Возможности SDK:
- Прием платежей (в том числе рекуррентных)
- Сохранение банковских карт клиента
- Сканирование и распознавание карт с помощью камеры или NFC
- Получение информации о клиенте и сохраненных картах
- Управление сохраненными картами

![PayFormActivity][img-pay]

### Требования
Для работы Tinkoff Acquiring SDK необходим Android версии 4.0 и выше (API level 14)

### Подключение
Для подключения SDK добавьте в [_build.gradle_][build-config] вашего проекта следующую зависимость:
```groovy
compile 'ru.tinkoff.acquiring:ui:1.0.1'
```

### Подготовка к работе
Для начала работы с SDK вам понадобятся:
* Terminal key
* Пароль
* Public key
которые выдаются после подключения к [Интернет-Эквайрингу][acquiring]

### Пример работы
Для проведения оплаты необходимо запустить _**PayFormActivity**_. Активити должна быть настроена на обработку конкретного платежа, поэтому для получения интента для ее запуска необходимо вызвать цепочку из методов **PayFormActivity**#_init_, **PayFormActivity**#_prepare_ и **PayFormActivity**#_setCustomerKey_

```java
PayFormActivity
        .init("TERMINAL_KEY", "PASSWORD", "PUBLIC_KEY") // данные продавца
        .prepare(
               "ORDER-ID",                  // ID заказа в вашей системе
                1000,                       // сумма для оплаты
                "НАЗВАНИЕ ПЛАТЕЖА",         // название платежа, видимое пользователю
                "ОПИСАНИЕ ПЛАТЕЖА",         // описание платежа, видимое пользователю
                "CARD-ID",                  // ID картчки
                "batman@gotham.co",         // E-mail клиента для отправки уведомления об оплате
                false,                      // флаг определяющий является ли платеж рекуррентным [1]
                true                        // флаг использования безопасной клавиатуры [2]
        )
        .setCustomerKey("CUSTOMER_KEY")     // уникальный ID пользователя для сохранения данных его карты
        .startActivityForResult(this, REQUEST_CODE_PAYMENT);

```

[1] _Рекуррентный платеж_ может производиться для дальнейшего списания средств с сохраненной карты, без ввода ее реквизитов. Эта возможность, например, может использоваться для осуществления платежей по подписке

[2] _Безопасная клавиатура_ используется вместо системной и обеспечивает дополнительную безопасность ввода, т.к. сторонние клавиатуры на устройстве клиента могут перехватывать данные и отправлять их злоумышленнику

![Безопасная клавиатура][img-keyboard]

### Структура
SDK состоит из следующих модулей

#### Core
Является базовым модулем для работы с Tinkoff Acquiring API. Модуль реализует протокол взаимодействия с сервером и позволяет не осуществлять прямых обращений в API. Не зависит от Android SDK и может использоваться в standalone Java приложениях

Основной класс модуля [AcquirinkSdk][acquiring-sdk-javadoc], который предоставляет фасад для взаимодействия с Tinkoff Acquiring API. Для работы необходимы ключи и пароль продавца (см. **Подготовка к работе**)

#### UI
Содержит интерфейс, необходимый для приема платежей через мобильное приложение.

Основной класс [PayFormActivity][payform-javadoc] - экран с формой оплаты, который позволяет
* просматривать информацию о платеже
* вводить или сканировать реквизиты карты для оплаты
* проходить 3DS подтверждение
* управлять списком ранее сохраненных карт

#### Sample
Содержит пример интеграции Tinkoff Acquiring SDK в мобильное приложение по продаже книг

#### Поддержка
- Просьба по возникающим вопросам обращаться на [card_acquiring@tinkoff.ru][support-email].
- Баги и feature-реквесты можно направлять в раздел [issues][issues].

[search.maven]: http://search.maven.org/#search|ga|1|ru.tinkoff.acquiring.ui
[build-config]: https://developer.android.com/studio/build/index.html
[support-email]: mailto:card_acquiring@tinkoff.ru
[issues]: https://github.com/TinkoffCreditSystems/tinkoff-asdk-android/issues
[acquiring]: https://t.tinkoff.ru/
[acquiring-sdk-javadoc]: http://tinkoffcreditsystems.github.io/tinkoff-asdk-android/javadoc/
[payform-javadoc]: http://tinkoffcreditsystems.github.io/tinkoff-asdk-android/javadoc/

[img-pay]: http://tinkoffcreditsystems.github.io/tinkoff-asdk-android/res/pay1.png
[img-keyboard]: http://tinkoffcreditsystems.github.io/tinkoff-asdk-android/res/keyboard1.png