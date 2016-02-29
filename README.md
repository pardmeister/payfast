# Laravel5 Payfast

A dead simple Laravel 5.2 payment processing class for payments through payfast.co.za. This package only supports ITN transactions. Laravel5 Payfast is strictly use at own risk.

## Installation

Add Laravel5 Payfast to your composer.json


    composer require billowapp/payfast


Add the PayfastServiceProvider to your providers array in config/app.php

```php
    'providers' => [
        //
        
        'Billow\PayfastServiceProvider'
    ];
```    
### Config
publish default configuration file.

    php artisan vendor:publish
    
IMPORTANT: You will need to edit App\Http\Middleware\VerifyCsrfToken by adding the route, which handles the ITN response to the $excepted array. Validation is done via the ITN response.


    
```php

    /*
    |--------------------------------------------------------------------------
    | Merchant Settings
    |--------------------------------------------------------------------------
    | All Merchant settings below are for example purposes only. for more info
    | see www.payfast.co.za. The Merchant ID and Merchant Key can be obtained 
    | from your payfast.co.za account.
    |
    */
    
    [

    'testing' => true, // Set to false when in production.

    'currency' => 'ZAR', // ZAR is the only supported currency at this point.

    'merchant' => [
        'merchant_id' => '10000100', // TEST Credentials. Replace with your merchant ID from Payfast.
        'merchant_key' => '46f0cd694581a', // TEST Credentials. Replace with your merchant key from Payfast.
        'return_url' => 'http://your-domain.co.za/success', // The URL the customer should be redirected to after a successful payment.
        'cancel_url' => 'http://your-domain.co.za/cancelled', // The URL the customer should be redirected to after a payment is cancelled.
        'notify_url' => 'http://your-domain.co.za/itn', // The URL to which Payfast will post return variables.
    ]
    
];

```
### Usage

```php
    <?php
    
    use Billow\Contracts\PaymentProcessor;
    
    Class PaymentController extends Controller
    {
        protected $payfast;
        
        public function __construct(PaymentProcessor $payfast)
        {
            $this->payfast = $payfast;
        }
        
        public function confirmPayment()
        {
            // At this point the order should be created and most likely set to a status of pending.    
    
            $amount = 9999; // Cart Total - Example Data.
            $merchant_reference = 001 // Order Reference - Example Data.
    
            $this->payfast->setBuyer('first name', 'last name', 'email');
            $this->payfast->setAmount($amount);
            $this->payfast->setItem('item-title', 'item-description');
            $this->payfast->setMerchantReference($merchant_reference);
    
            return $this->payfast->paymentForm();
        }
            
    }
    
```    
