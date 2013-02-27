## Capture funds with omnipay gateways.

Omnipay lib provide [support of 25+ gateways](https://github.com/adrianmacneil/omnipay#payment-gateways). 

### Step 1. Download Omnipay Bridge.

Add the following lines in your `composer.json` file:

```json
{
    "require": {
        "payum/ominpay-bridge": "dev-master"
    }
}
```

_**Note:** You may want to adapt this line to use a specific version._

Now, run composer.phar to download the bundle:

```bash
$ php composer.phar install
```

_**Note:** You can immediately start using it. The autoloading files have been generated by composer and already included to the app autoload file._

### Step 2: Configure payum bundle

```yaml
#app/config/config.yml

payum:
    contexts:
        your_context_here:
            ominpay_payment:
                type: Stripe
                options:
                    apiKey: abc123
            null_storage: true
```

**Warning:**

> You have to changed this name `your_context_name` to something related to your domain, for example `post_a_job_with_stripe` 

### Step 3. Capture payment:

```php
<?php
//src/Acme/DemoBundle/Controller
namespace AcmeDemoBundle\Controller;

use Symfony\Component\HttpFoundation\Request;

class PaymentController extends Controller 
{
    public function captureWithStripeAction(Request $request)
    {
        $contextName = 'your_context_name';
    
        $paymentContext = $this->get('payum')->getContext($contextName);
    
        $model = array(
            'amount' => 10,
            'card' => array(
                'number' => '5555556778250000',
                'cvv' => 123,
                'expiryMonth' => 6,
                'expiryYear' => 16,
                'firstName' => 'foo',
                'lastName' => 'bar',
            )
        );
        
        return $this->forward('PayumBundle:Capture:do', array(
            'contextName' => $contextName,
            'model' => $model
        ));
    }
}
```

### Next Step

You are ready to read:

* [how to manage interactive](interactive_requests.md).
* [how to customize capture finished controller](customize_capture_finished_controller.md).
* [look at configuration reference](configuration_reference.md).