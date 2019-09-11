# BDC_Mycli

This module is used as a Mycli for all BDCrops Magento 2 extensions.





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




```
  php bin/magento bdcrops:user:create --customer-firstname="Matin" --customer-lastname="Rahman" --customer-email="matin@bdcrops.com" --customer-password="matin@123" --website="1"
  ```
