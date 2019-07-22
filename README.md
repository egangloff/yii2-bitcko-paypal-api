Yii2 Bitcko PayPal Api Extension
=========================================
Yii2 Bitcko  PayPal Api extension  use to integrate simple PayPal payment in your website.

Installation
------------

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

Either run

```
php composer.phar require bitcko/yii2-bitcko-paypal-api:dev-master

```

or add

```
"bitcko/bitcko/yii2-bitcko-paypal-api": "dev-master"
```

to the require section of your `composer.json` file.


Usage
-----

Once the extension is installed, simply use it in your code by  :

1. Create developer account in PayPal and then create an app.  [PayPal Developer Dashboard](https://developer.paypal.com/).
2. Copy and paste the client Id and client secret in params.php file that exists in app config directory:
```php
<?php

return [
    'adminEmail' => 'admin@example.com',
    'payPalClientId'=>'app client id here',
    'payPalClientSecret'=>'app client secret here',
    'paypalMode'=>'sandbox' //sandbox or live
];


```
3. Configure the extension  in components section in web.php file exists in app config directory: 

```php
<?php
'components'=> [
    ...
 'PayPalRestApi'=>[
            'class'=>'bitcko\paypalrestapi\PayPalRestApi',
            'redirectUrl'=>'/site/make-payment', // Redirect Url after payment
            ]
            ...
        ]

```
4. Controller example:
       call first the checkout action that will redirect you to the redirectUrl you mentioned in the previous step,
       in this example ("/site/make-payment")

```php
<?php

namespace app\controllers;

use Yii;

use yii\web\Controller;

class SiteController extends Controller
{
   
    public function actionCheckout(){
        // Setup order information array with all items
        $params = [
            'method'=>'paypal',
            'intent'=>'sale',
            'order'=>[
                'description'=>'Payment description',
                'subtotal'=>44,
                'shippingCost'=>0,
                'total'=>44,
                'currency'=>'USD',
                'items'=>[
                    [
                        'name'=>'Item one',
                        'price'=>10,
                        'quantity'=>1,
                        'currency'=>'USD'
                    ],
                    [
                        'name'=>'Item two',
                        'price'=>12,
                        'quantity'=>2,
                        'currency'=>'USD'
                    ],
                    [
                        'name'=>'Item three',
                        'price'=>1,
                        'quantity'=>10,
                        'currency'=>'USD'
                    ],

                ]

            ]
        ];
        
        // In this action you will redirect to the PayPpal website to login with you buyer account and complete the payment
        Yii::$app->PayPalRestApi->checkOut($params);
    }

    public function actionMakePayment(){
         // Setup order information array 
        $params = [
            'order'=>[
                'description'=>'Payment description',
                'subtotal'=>44,
                'shippingCost'=>0,
                'total'=>44,
                'currency'=>'USD',
            ]
        ];
      // In case of payment success this will return the payment object that contains all information about the order
      // In case of failure it will return Null
      return  Yii::$app->PayPalRestApi->processPayment($params);

    }
}


```



