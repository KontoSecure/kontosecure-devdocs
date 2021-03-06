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
  -d "shippingFee=4.99" \
  -d "successUrl=<url>" \
  -d "failedUrl=<url>" \
  -d "canceledUrl=<url>" \
  -d "webhookUrl=<url>" \
  -d "validUntil=<yyyy-mm-dd hh:ii:ss>" \
  -d "customFields[my_field_name_1]=my_field_value_1" \
  -d "customFields[my_field_name_2]=my_field_value_2"
```

```php
$client = new Client('ihrapikey');

$order = new Order();
$order->setAmount(10.00);
$order->setClientEmail('client@example.com');
$order->setDescription('Bestellung Nummer 55837462');

// Optional parameters
$order->setShippingFee(4.99);
$order->setSuccessUrl('https://www.example.com/success');
$order->setFailedUrl('https://www.example.com/failed');
$order->setCanceledUrl('https://www.example.com/canceled');
$order->setWebhookUrl('https://www.example.com/webhook');
$order->setValidUntil($dateTimeObject);
$order->addCustomField('my_field_name_1', 'my_field_value_1');
$order->addCustomField('my_field_name_2', 'my_field_value_2');

$response = $client->createOrder($order);
```

> Antwort in JSON-Format:

```json
{
  "order_id": "84I00-40K7E-65E6S-DXJ75",
  "security_token": "e4R75Wv8",
  "checkout_url": "https://www.konto-secure.de/checkout/84I00-40K7E-65E6S-DXJ75/13fd3d881bbbd55c5cab9ebeea0fdf7b91e9bb9c7d7af8563cd95d51fb060a19",
  "valid_until": "2017-05-25 12:44:17"
}
```

Dieser Endpunkt erstellt eine neue Order.
Wenn Sie die optionalen Parameter für die Redirect- bzw. die Webhook-URL definieren,
so überschreiben diese Werte die URLs, die Sie im Merchant Backoffice unter "Integration"
definiert haben.

Erstellte Orders sind grundsätzlich nur 10 Minuten gültig. Sollten Sie einen kürzeren oder längeren Validity-Zeitraum benötigen, so können Sie das mit dem Parameter "validUntil" steuern.

### HTTP Request

`POST https://api.konto-secure.de/orders`

### Parameter

Parameter | Beispiel | Beschreibung | Pflichtfeld
--------- | -------- | ------------ | ------------
amount | 10.00 | Der Betrag den Sie erhalten möchten. | Ja
clientEmail | shopper@example.com | Die Email Adresse des Käufers | Ja
description | Bestellung Nr. 927462 | Der Verwendungszweck auf der Überweisung | Ja
shippingFee | 4.95 | Die Versandkosten. | Nein
successUrl | https://www.example.com/success | Redirect nach erfolgreicher Transaktion | Nein
failedUrl | https://www.example.com/failed | Redirect nach fehlgeschlagener Transaktion | Nein
canceledUrl | https://www.example.com/canceled | Redirect nach vom User abgebrochener Transaktion | Nein
webhookUrl | https://www.example.com/kontosecure/webhook | Endpunkt empfängt Transaktionsdetails via POST Request | Nein
validUntil | 2020-05-01 12:00:00 | Die erstellte Order ist bis zu diesem Zeitpunkt gültig | Nein
customFields[] | [my_internal_reference] = intref_123 | Eigene Felder | Nein 

<aside class="success">
Der Gesamtbetrag der Überweisung ergibt sich aus amount + shippingFee
</aside>


