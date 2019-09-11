# BDC_Mycli

This module is used as a Mycli for all BDCrops Magento 2 extensions.


## Goal:

- 1.1 Create Simple Cli Command
- 1.2 Create Advanced Command

## <a name="top"> Magento 2 CLI Module Step By Step (BDCrops) </a>

##  [Part A : News Module for Basic & Simple Cli](#PartA)
- [Step 2A.1: Create a directory for the module like above format](#Step2A1)
- [Step 2A.2: Declare module by using configuration file module.xml](#Step2A2)
- [Step 2A.3: Register module by registration.php](#Step2A3)
- [Step 2A.4: DI module by di.xml](#Step2A4)
- [Step 2A.5: Register module by registration.php](#Step2A5)

##  [Part B : News Module for Advanced Command](#PartB)

- [Step 2B.1: Adding a new command Dependency Injection](#Step2B1)
- [Step 2B.2: Adding a new command class](#Step2B2)
- [Step 2B.3: Adding a new command Helper class ](#Step2B3)




### <a name="Step2A1">Step 2A.1: Create a directory for the module like above format</a>

In this module, we will use `BDC` for Vendor name and `Mycli` for ModuleName. So we need to make this folder: `app/code/BDC/Mycli`



### <a name="Step2A2">Step 2A.2: Declare module by using configuration file module.xml</a>

Magento 2 looks for configuration information for each module in that module’s etc directory. We need to create folder etc and add module.xml:
 Create app/code/BDC/Mycli/etc/module.xml And the content for this file:

~~~ xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="BDC_Mycli" setup_version="1.0.0" />
</config>
~~~
In this file, we register a module with name `BDC_Mycli` and the version is `1.0.0`.



### <a name="Step2A3"> Step 2A.3: Register module by registration.php</a>

All Magento 2 module must be registered in the Magento system through the magento ComponentRegistrar class. This file will be placed in module root directory.
In this step, we need to create this file:
Create  app/code/BDC/Mycli/registration.php and insert this following code into it:

~~~
\Magento\Framework\Component\ComponentRegistrar::register(
    \Magento\Framework\Component\ComponentRegistrar::MODULE,
    'BDC_Mycli',
    __DIR__
);
~~~

Modules in vendor folder would update using composer And all the modules in app/code would not be updated through composer That's why when you need to override any module you add it in app/code

Create  app/code/BDC/Mycli/composer.json  and insert this following code into it:

```
{
    "name": "bdc/module-Mycli",
    "description": "BDCrops Mycli module for Magento 2 extensions.",
    "type": "magento2-module",
    "version": "1.0.0",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
	"authors": [{
            "name": "Abdul Matin",
            "email": "matinict@gmail.com",
			      "company": "BDCrops Inc"
        }
    ],
	"homepage": "https://www.bdcrops.com",
    "autoload": {
        "files": [
            "registration.php"
        ],
        "psr-4": {
            "BDC\\Mycli\\": ""
        }
    }
}

```

### <a name="Step2A4"> Step 2A.4: DI module by di.xml</a>


Create  app/code/BDC/Mycli/etc/di.xml and insert this following code into it:

```
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
   <type name="Magento\Framework\Console\CommandList">
       <arguments>
           <argument name="commands" xsi:type="array">
               <item name="bdcropsSayHello" xsi:type="object">BDC\Mycli\Console\Sayhello</item>

           </argument>
       </arguments>
   </type>
</config>

```

### <a name="Step2A5"> Step 2A.5: Sayhello module by Sayhello.php</a>


Create  app/code/BDC/Mycli/Console/Sayhello.php and insert this following code into it:

```
<?php
namespace BDC\Mycli\Console;

use Symfony\Component\Console\Command\Command;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;

class Sayhello extends Command
{
   protected function configure()
   {
       $this->setName('bdcrops:sayhello');
       $this->setDescription('Demo command line');

       parent::configure();
   }
   protected function execute(InputInterface $input, OutputInterface $output)
   {
       $output->writeln("Hello Matin!! Welcome to BDCrops CLI..");
   }
}

```
Run Command:
```
php bin/magento setup:upgrade
php bin/magento cache:clean
php bin/magento list
```
![Mycli](https://github.com/bdcrops/BDC_Mycli/blob/master/doc/helloCli.png)

```
php bin/magento bdcrops:sayhello

```
![sayHello](https://github.com/bdcrops/BDC_Mycli/blob/master/doc/sayHelloCli.png)


### <a name="Step2B1"> Step 2B.1: Adding a new command  Dependency Injection</a>

Adding a new command to CLI is based on passing on the argument from the XML level to the class Magento\Framework\Console\CommandList. Dependency Injection comes in handy here. Let’s  

Edit/Create  app/code/BDC/Mycli/etc/di.xml and insert this following code into it:


```
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
   <type name="Magento\Framework\Console\CommandList">
       <arguments>
           <argument name="commands" xsi:type="array">
               <item name="bdcropsSayHello" xsi:type="object">BDC\Mycli\Console\Sayhello</item>
               <item name="customer_user_create" xsi:type="object">BDC\Mycli\Console\Command\CustomerUserCreateCommand</item>
           </argument>
       </arguments>
   </type>
</config>

```
### <a name="Step2B2"> Step 2B.2: Adding a new command  class</a>
We add the object responsible for executing the script to the class Magento\Framework\Console\CommandList. The constructor of this class is simply an array where class objects are passed on in a similar manner as in the above example.

Let’s proceed to the next step – creating a class for our new command and a helper responsible for adding a new user:

Create  app/code/BDC/Mycli/Console/Command/CustomerUserCreateCommand.php and insert this following code into it:

```
<?php
namespace BDC\Mycli\Console\Command;

use Symfony\Component\Console\Command\Command;
use Symfony\Component\Console\Input\InputOption;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;
use BDC\Mycli\Helper\Customer;

class CustomerUserCreateCommand extends Command
{
    protected $customerHelper;

    public function __construct(Customer $customerHelper)
    {
        $this->customerHelper = $customerHelper;
        parent::__construct();
    }

    protected function configure()
    {
        $this->setName('bdcrops:user:create')
            ->setDescription('Create new customer')
            ->setDefinition($this->getOptionsList());
    }

    protected function execute(InputInterface $input, OutputInterface $output)
    {
        $output->writeln('<info>Creating new user...</info>');
        $this->customerHelper->setData($input);
        $this->customerHelper->execute();

        $output->writeln('');
        $output->writeln('<info>User created with the following data:</info>');
        $output->writeln('<comment>Customer ID: ' . $this->customerHelper->getCustomerId());
        $output->writeln('<comment>Customer Website ID ' . $input->getOption(Customer::KEY_WEBSITE));
        $output->writeln('<comment>Customer First Name: ' . $input->getOption(Customer::KEY_FIRSTNAME));
        $output->writeln('<comment>Customer Last Name: ' . $input->getOption(Customer::KEY_LASTNAME));
        $output->writeln('');
        $output->writeln('<comment>Customer Email: ' . $input->getOption(Customer::KEY_EMAIL));
        $output->writeln('<comment>Customer Password: ' . $input->getOption(Customer::KEY_PASSWORD));
    }

    protected function getOptionsList(){
        return [
            new InputOption(Customer::KEY_FIRSTNAME, null, InputOption::VALUE_REQUIRED, '(Required) Customer first name'),
            new InputOption(Customer::KEY_LASTNAME, null, InputOption::VALUE_REQUIRED, '(Required) Customer last name'),
            new InputOption(Customer::KEY_EMAIL, null, InputOption::VALUE_REQUIRED, '(Required) Customer email'),
            new InputOption(Customer::KEY_PASSWORD, null, InputOption::VALUE_REQUIRED, '(Required) Customer password'),
            new InputOption(Customer::KEY_WEBSITE, null, InputOption::VALUE_REQUIRED, '(Required) Website ID'),
            new InputOption(Customer::KEY_SENDEMAIL, 0, InputOption::VALUE_OPTIONAL, '(1/0) Send email? (default 0)')
        ];
    }
}

```

### <a name="Step2B3"> Step 2B.3: Helper  </a>


Create  app/code/BDC/Mycli/Helper/Customer.php and insert this following code into it:

```
<?php
namespace BDC\Mycli\Helper;

use \Magento\Framework\App\Helper\Context;
use \Magento\Store\Model\StoreManagerInterface;
use \Magento\Framework\App\State;
use \Magento\Customer\Model\CustomerFactory;
use \Symfony\Component\Console\Input\Input;

class Customer extends \Magento\Framework\App\Helper\AbstractHelper
{
    const KEY_EMAIL = 'customer-email';
    const KEY_FIRSTNAME = 'customer-firstname';
    const KEY_LASTNAME = 'customer-lastname';
    const KEY_PASSWORD = 'customer-password';
    const KEY_WEBSITE = 'website';
    const KEY_SENDEMAIL = 'send-email';

    protected $storeManager;
    protected $state;
    protected $customerFactory;
    protected $data;
    protected $customerId;

    public function __construct(
        Context $context,
        StoreManagerInterface $storeManager,
        State $state,
        CustomerFactory $customerFactory
    ) {
        $this->storeManager = $storeManager;
        $this->state = $state;
        $this->customerFactory = $customerFactory;

        parent::__construct($context);
    }

    public function setData(Input $input)
    {
        $this->data = $input;
        return $this;
    }

    public function execute()
    {
        $this->state->setAreaCode('frontend');

        $customer = $this->customerFactory->create();
        $customer
            ->setWebsiteId($this->data->getOption(self::KEY_WEBSITE))
            ->setEmail($this->data->getOption(self::KEY_EMAIL))
            ->setFirstname($this->data->getOption(self::KEY_FIRSTNAME))
            ->setLastname($this->data->getOption(self::KEY_LASTNAME))
            ->setPassword($this->data->getOption(self::KEY_PASSWORD));
        $customer->save();

        $this->customerId = $customer->getId();

        if($this->data->getOption(self::KEY_SENDEMAIL)) {
            $customer->sendNewAccountEmail();
        }
    }

    public function getCustomerId()
    {
        return (int)$this->customerId;
    }
}

```

The execute() method adds a new user. If any data is incorrect at this stage (i.e. too short password), the script will stop and the console will show an Exception.

### <a name="Step2B4"> Step 2B.4: Results  </a>


Let’s check if our command is on the list:
```
$ php bin/magento list
```
![cli2](https://github.com/bdcrops/BDC_Mycli/blob/master/doc/MyCli2.png)

As we can see, our command is available. Even though we didn’t add anything to Help, we can still see how our command can be run.

```
$ php bin/magento bdcrops:user:create --help
```


Next, let’s add a new user according to the usage pattern.


```
  php bin/magento bdcrops:user:create --customer-firstname="Matin" --customer-lastname="Rahman" --customer-email="matin@bdcrops.com" --customer-password="matin@123" --website="1"
  ```

![](https://github.com/bdcrops/BDC_Mycli/blob/master/doc/addDataCli.png)

Database table will insert data as below:
![](https://github.com/bdcrops/BDC_Mycli/blob/master/doc/MyCliDBData.png)

If we run the command again, we should get the following exception:

![](https://github.com/bdcrops/BDC_Mycli/blob/master/doc/MycliDublicate.png)
