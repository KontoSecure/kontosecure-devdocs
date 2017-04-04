---
title: API Referenz

language_tabs:
  - shell
  - php

toc_footers:
  - <a href='https://www.konto-secure.de/register'>Hier als Händler registrieren</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Einleitung

KontoSecure API-Referenz
We have language bindings in Shell, Ruby, and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This example API documentation page was created with [Slate](https://github.com/tripit/slate). Feel free to edit it and use it as a base for your own API's documentation.

# Authentifizierung

Die Authentifizierung bei konto-secure.de erfolgt mit dem header key Api-Key.
Ihren API-Key können sie im Merchant Backend unter dem Menüpunkt "API-Keys" entnehmen.

```shell
curl "https://api.konto-secure.de"
  -H "Api-Key: ihrapikey"
```

```php
$client = new Client('ihrapikey');
```

> Bitte ersetzen Sie `ihrapikey` mit Ihrem echten API key.

Um einen API-Key zu erhalten, registrieren Sie sich bitte als Händler unter [https://www.konto-secure.de/register](https://www.konto-secure.de/register).
KontoSecure erwartet bei jedem Request and die API, dass Ihr Api-Key mitgesendet wird.

`Api-Key: ihrapikey`

<aside class="notice">
Sie müssen <code>ihrapikey</code> mit Ihrem API-Key ersetzen.
</aside>

# Orders

## Eine neue Order erstellen

```shell
curl -XPOST "https:/api.konto-secure.de/orders" \
  -H "Api-Key: ihrapikey" \
  -d "amount=5.99" \
  -d "clientEmail=client@example.com" \
  -d "description=Bestellung Nummer 55837462" \
  -d "shippingFee=4.99"
```

```php
$client = new Client('ihrapikey');

$order = new Order();
$order->setAmount(10.00);
$order->setClientEmail('client@example.com');
$order->setDescription('Bestellung Nummer 55837462');

$response = $client->createOrder($order);
```

> Antwort in JSON-Format:

```json
{
  "order_id": "84I00-40K7E-65E6S-DXJ75",
  "security_token": "e4R75Wv8",
  "checkout_url": "//www.konto-secure.de/checkout/84I00-40K7E-65E6S-DXJ75/13fd3d881bbbd55c5cab9ebeea0fdf7b91e9bb9c7d7af8563cd95d51fb060a19"
}
```

Dieser Endpunkt erstellt eine neue Order.

### HTTP Request

`POST https://api.konto-secure.de/orders`

### Parameter

Parameter | Beispiel | Beschreibung | Pflichtfeld
--------- | -------- | ------------ | ------------
amount | 10.00 | Der Betrag den Sie erhalten möchten. | Ja
shippingFee | 4.95 | Die Versandkosten. | Nein
clientEmail | shopper@example.com | Die Email Adresse des Käufers | Ja
description | Bestellung Nr. 927462 | Der Verwendungszweck auf der Überweisung | Ja

<aside class="success">
Der Gesamtbetrag der Überweisung ergibt sich aus amount + shippingFee
</aside>

