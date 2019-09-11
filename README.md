# BDC_Mycli

This module is used as a Mycli for all BDCrops Magento 2 extensions.


## Goal:

- 1.1 Create Simple Cli Command
- 1.2 Create Advanced Command
- 1.3 FAQ of Utilize the CLI

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



## [FAQ of Utilize the CLI](#faqcli)



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


##  <a name="faqcli"> FAQ of Utilize the CLI</a>


### 1.1 Utilize the CLI ?
Command Line Inteface (CLI) in Magento 2. As you know, from Magento 2, they add many commands in bin/magento. This may difficult to get approach this , but let me explain more detail in this tutorial.

When you run command in terminal:

php bin/magento  or

bin/magento
You will get the list of Magento 2 command line available, this list includes custom command line

```
Usage:                                                                                                                      
 command [options] [arguments]                                                                                              

Options:                                                                                                                    
 --help (-h)           Display this help message                                                                            
 --quiet (-q)          Do not output any message                                                                            
 --verbose (-v|vv|vvv) Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug   
 --version (-V)        Display this application version                                                                     
 --ansi                Force ANSI output                                                                                    
 --no-ansi             Disable ANSI output                                                                                  
 --no-interaction (-n) Do not ask any interactive question                                                                  

Available commands:                                                                                                         
 help                                      Displays help for a command                                                      
 list                                      Lists commands                                                                   
admin                                                                                                                       
 admin:user:create                         Creates an administrator                                                         
 admin:user:unlock                         Unlock Admin Account                                                             
cache                                                                                                                       
 cache:clean                               Cleans cache type(s)                                                             
 cache:disable                             Disables cache type(s)                                                           
 cache:enable                              Enables cache type(s)                                                            
 cache:flush                               Flushes cache storage used by cache type(s)                                      
 cache:status                              Checks cache status                                                              
catalog                                                                                                                     
 catalog:images:resize                     Creates resized product images                                                   
 catalog:product:attributes:cleanup        Removes unused product attributes.                                               
cron                                                                                                                        
 cron:run                                  Runs jobs by schedule                                                            
customer                                                                                                                    
 customer:hash:upgrade                     Upgrade customer's hash according to the latest algorithm                        
deploy                                                                                                                      
 deploy:mode:set                           Set application mode.                                                            
 deploy:mode:show                          Displays current application mode.                                               
dev                                                                                                                         
 dev:source-theme:deploy                   Collects and publishes source files for theme.                                   
 dev:tests:run                             Runs tests                                                                       
 dev:urn-catalog:generate                  Generates the catalog of URNs to *.xsd mappings for the IDE to highlight xml.    
 dev:xml:convert                           Converts XML file using XSL style sheets                                         
i18n                                                                                                                        
 i18n:collect-phrases                      Discovers phrases in the codebase                                                
 i18n:pack                                 Saves language package                                                           
 i18n:uninstall                            Uninstalls language packages                                                     
indexer                                                                                                                     
 indexer:info                              Shows allowed Indexers                                                           
 indexer:reindex                           Reindexes Data                                                                   
 indexer:reset                             Resets indexer status to invalid                                                 
 indexer:set-mode                          Sets index mode type                                                             
 indexer:show-mode                         Shows Index Mode                                                                 
 indexer:status                            Shows status of Indexer                                                          
info                                                                                                                        
 info:adminuri                             Displays the Magento Admin URI                                                   
 info:backups:list                         Prints list of available backup files                                            
 info:currency:list                        Displays the list of available currencies                                        
 info:dependencies:show-framework          Shows number of dependencies on Magento framework                                
 info:dependencies:show-modules            Shows number of dependencies between modules                                     
 info:dependencies:show-modules-circular   Shows number of circular dependencies between modules                            
 info:language:list                        Displays the list of available language locales                                  
 info:timezone:list                        Displays the list of available timezones                                         
maintenance                                                                                                                 
 maintenance:allow-ips                     Sets maintenance mode exempt IPs                                                 
 maintenance:disable                       Disables maintenance mode                                                        
 maintenance:enable                        Enables maintenance mode                                                         
 maintenance:status                        Displays maintenance mode status                                                 
module                                                                                                                      
 module:disable                            Disables specified modules                                                       
 module:enable                             Enables specified modules                                                        
 module:status                             Displays status of modules                                                       
 module:uninstall                          Uninstalls modules installed by composer                                         
sampledata                                                                                                                  
 sampledata:deploy                         Deploy sample data modules                                                       
 sampledata:remove                         Remove all sample data packages from composer.json                               
 sampledata:reset                          Reset all sample data modules for re-installation                                
setup                                                                                                                       
 setup:backup                              Takes backup of Magento Application code base, media and database                
 setup:config:set                          Creates or modifies the deployment configuration                                 
 setup:cron:run                            Runs cron job scheduled for setup application                                    
 setup:db-data:upgrade                     Installs and upgrades data in the DB                                             
 setup:db-schema:upgrade                   Installs and upgrades the DB schema                                              
 setup:db:status                           Checks if DB schema or data requires upgrade                                     
 setup:di:compile                          Generates DI configuration and all missing classes that can be auto-generated    
 setup:install                             Installs the Magento application                                                 
 setup:performance:generate-fixtures       Generates fixtures                                                               
 setup:rollback                            Rolls back Magento Application codebase, media and database                      
 setup:static-content:deploy               Deploys static view files                                                        
 setup:store-config:set                    Installs the store configuration                                                 
 setup:uninstall                           Uninstalls the Magento application                                               
 setup:upgrade                             Upgrades the Magento application, DB data, and schema                            
theme                                                                                                                       
 theme:uninstall                           Uninstalls theme
```


### 1.2 Describe the usage of bin/magento commands in the development cycle.

- Setup upgrade
php bin/magento setup:upgrade
- In case you want to keep pub/static files while installing/updating database, apply following command:

php bin/magento setup:upgrade --keep-generated
- Clean Cache
php bin/magento cache:clean
- Flush Cache
php bin/magento cache:flush
- View cache status
php bin/magento cache:status
- Enable Cache
php bin/magento cache:enable [cache_type]
- Disable Cache
php bin/magento cache:disable [cache_type]
- Deploy static content
php bin/magento setup:static-content:deploy
- Deploy static content for particular language
php bin/magento setup:static-content:deploy en_US
- Deploy static content for Magento backend theme
php bin/magento setup:static-content:deploy --theme="Magento/backend"
- Deploy static content for specific themes
php bin/magento setup:static-content:deploy --theme Magento/luma --theme Magento/second_theme
- Deploy exclude themes on static content while does not minify html files
php bin/magento setup:static-content:deploy en_US --exclude-theme Magento/luma --no-html-minify
- Reindex
php bin/magento indexer:reindex
- View indexer list
php bin/magento indexer:info
- View indexer status
php bin/magento indexer:status
- Show all indexers mode
php bin/magento indexer:show-mode
- See all modules status
php bin/magento module:status
- Enable module
php bin/magento module:enable Namespace_Module
- Disable module
php bin/magento module:disable Namespace_Module
- Uninstall module
php bin/magento module:uninstall Namespace_Module
- Check current mode
php bin/magento deploy:mode:show
- Change to developer mode
php bin/magento deploy:mode:set developer
- Change to production mode
php bin/magento deploy:mode:set production
- Run the single-tenant Compiler
php bin/magento setup:di:compile
- Unlock Admin user
php bin/magento admin:user:unlock adminusername
- Enable maintenance mode
php bin/magento maintenance:enable
- Enable maintenance mode for all clients except 192.0.0.1 and 192.0.0.1:
php bin/magento maintenance:enable --ip=192.0.0.1 --ip=192.0.0.2
- Clear the list of IPs.
php bin/magento maintenance:enable --ip=none
- Disable maintenance mode
php bin/magento maintenance:disable
- Check maintenance mode status
php bin/magento maintenance:status
- Allow IP on maintenance mode
php bin/magento maintenance:allow-ips --ip=192.0.0.1 --ip=192.0.0.2

That’s all SSH/CLI commands for Magento 2 that I can cover for you. Hope that they will help you.

### 1.3 Which commands are available?
see 1.1

### 1.4 How are commands used in the development cycle?

see 1.2
### 1.5 Demonstrate an ability to create a deployment process?

### 1.6 How does the application behave in different deployment modes?

### 1.7 how do these behaviors impact the deployment approach for PHP code, frontend assets, etc.?

### How to deploy static asset for different file types?
Static files are the files that can be cached on the site, which increases page loading speed. These include CSS, fonts, images, and JavaScript used by the theme. Such files are located in the / pub / static and / var / view_preprocessed / pub / static folders, which get there after deployment. From this article you will learn how to deploy static files.

### What commands must be executed to deploy static file types?
To launch the deployment process manually you need to use the following command in the command line to make sure that your user has sufficient rights and you are in the site root:
```
php bin/magento setup:static-content:deploy
```
This was the example of a standard command without additional parameters. As a result, all static files of all themes, areas, and so on will be redeployed.

However, there are many parameters that adjust the deployment process of this command. With parameters added, the line will look like this:
```
php bin/magento setup:static-content:deploy [<languages>] [--theme[="<theme>"]]
[--exclude-theme[="<theme>"]][--language[="<language>"]]
[--exclude-language[="<language>"]] [--area[="<area>"]] [--exclude-area[="<area>"]]
[--jobs[="<number>"]] [--no-javascript] [--no-css] [--no-less] [--no-images] [--no-fonts]
[--no-html] [--no-misc] [--no-html-minify] [--dry-run] [--force] [--verbose]
```
where

- <languages> – list of languages (language defined by code), which is separated with spaces. Static content is deployed only for specified languages; by default for en_US only (if the parameter is not specified). For example, en_US fr_FR.
- –theme [= “<theme>”] – list of topics which static content is deployed to (if the parameter is not set, then static content is deployed to all topics). For example, –theme Magento / blank –theme Magento / luma.
- –exclude-theme [= “<theme>”] – list of topics for which static content is NOT deployed. For example, –exclude-theme Magento / blank –exclude-theme Magento / luma.
- –language [= “<language>”] –  list of languages to which static content is deployed (if the parameter is not set, then static content is applied for all languages). For example, –language en_US –language es_ES. If you specify this parameter and the <languages> parameter (described above), then the <languages> parameter will have priority. For shorthand, use -l instead of –language (example: -l es_ES).
–exclude-language [= “<language>”] – a list of languages for which static content is NOT deployed. For example, –exclude-language en_US –exclude-language es_ES.
- –area [= “<area>”] – list of areas to which static content is deployed (if the parameter is not set, then static content is deployed in all areas). For example, –area adminhtml. For shorthand use -a (example: -a adminhtml) instead of –area.
- –exclude-area [= “<area>”] – list of areas for which static content is NOT deployed. For example, –exclude-area adminhtml.
- –jobs [= “<number>”] – this option allows you to use a certain number of tasks in parallel processing (4 by default). For example, –jobs 1. For shorthand, use -j (example: -j 1) instead of –jobs .
- –no-javascript – with this parameter JavaScript files will not be deployed.
- –no-css – with this parameter CSS files will not be deployed.
- –no-less – with this parameter LESS files will not be deployed.
- –no-images – with this parameter images will not be deployed.
- –no-fonts – with this parameter font files will not be deployed.
- –no-html – with this parameter HTML files will not be deployed.
- –no-misc – with this parameter no other file types will deploy (for example, .md, .jbf, .csv, .json, .txt, .htc, or .swf).
- –no-html-minify – with this parameter HTML files will not be minified.
- –dry-run is a parameter that allows you to view output files without any changes.
- –force – deploy files in any mode. For shorthand use -f instead of –force .
- –verbose – change the amount of information in the deploy output. As a rule, the abbreviated form of the parameter is used, where -v is for minimum information on deployment (default value), -vv is for more detailed information, -vvv is used for debu

### What are common mistakes during the process?
During file deletion you may make the following mistakes, so please avoid them:

Forgetting to make a deploy for some languages. With the standard deployment command, the system will only deploy the files of the current language version (by default, en_US) and if we have several language versions on our site, then they will not be redeployed. To deploy content from other language versions, use the <language> or –language [= “<language>”] parameter I mentioned above.
deployment without sufficient rights for files record. Therefore, before performing deployment, I recommend running the following commands:
cd <your Magento install dir>
```
find . -type f -exec chmod 644 {} \;   
find . -type d -exec chmod 755 {} \;  
find ./var -type d -exec chmod 777 {} \;   
find ./pub/media -type d -exec chmod 777 {} \;
find ./pub/static -type d -exec chmod 777 {} \;
chmod 777 ./app/etc
chmod 644 ./app/etc/*.xml
chown -R :<web server group> .
chmod u+x bin/magento
```
Forgeting to clean up static cache files, which may cause old static files to distort the correct display of the site.
Deployment without being in the root of the site.
Common careless mistakes in command syntax.

### What the differences between development and production mode in regard to frontend development?
There are 3 website operating modes: Default, Developer and Production.

Default mode – this mode is set to Magento initially when no other mode is selected. This mode allows to deploy Magento on one server without changing settings. At the same time, this mode does not provide such performance optimization as production mode. Therefore, as a rule, the default mode is later changed to a developer or production mode, depending on the work specifics.

Default mode features:

for each requested static view file link is generated in the pub / static directory;
static files are generated dynamically, causing the website to load much longer than in production mode;
errors are recorded in the report files on the server and not shown to the user (on the frontend side).
In order to deploy Magento on several servers, as well as to make site debugging more convenient, turn on developer mode. To maximize performance optimization, turn on production mode. Further, I will describe how to change development modes.

Developer mode should be used when you develop new functionality or customize the current one because it allows you to get feedback from the system much faster than in other modes.

Developer mode features are:

static view files are not cached; they are recorded to the pub / static folder every time they are called;
static files are generated dynamically, making your site load much longer than in production mode;
non-displayable errors are displayed in the browser;
exceptions and errors are recorded to var / log and / var / reports _ ;
exceptions are generated in the error handler, and are not recorded in the log;
exceptions are generated when the event caller cannot be called.
Production mode is better used at the finished site, which will be available to all users. This mode allows for site maximum performance and download speed.

You should turn it on only after all development works on your website have been completed. Before enabling this mode, you must configure all the necessary settings on the server (optimize the server environment) which will host the website. After transferring your site to the server, you need to run the static view files deployment tool in order to generate static files that will be written to the pub / static folder. This action improves performance by ensuring that all the necessary static files are immediately available instead of being generated on request, as in other modes.

Production mode features are:

static presentation files are loaded only from the cache; they have no generated links in the pub / static directory, and links are compiled in real time. A new or updated file is not written into the file system;
errors are not displayed to the user (on the frontend side) but are recorded in the report files on the server.
From the description of the modes above it becomes clear that the main difference between the development and production modes is that the first is used at the development and debugging of the site, and the second – after the development is completed and you need maximum performance of the site.

There is also Maintenance mode, which is used to temporarily close the site for visitors. When a user goes to the site, he is redirected to “Service Temporarily Unavailable” page. This mode is useful when you perform configurations that may affect website functionality or appearance. You can’t but agree that it’s better to close the website from visitor’s view completely then let them see and underconstructed website.

In order to detect which mode is currently used, apply the following command: bin / magento deploy: mode: show.

To enable production mode use the following command: bin / magento deploy: mode: set production.

To enable developer mode, use the following command: bin / magento deploy: mode: set developer.

To enable maintenance mode, use the following command: bin / magento maintenance: enable.

To disable maintenance mode, use the following command: bin / magento maintenance: disable.
