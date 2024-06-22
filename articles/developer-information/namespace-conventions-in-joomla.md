<!-- Filename: J4.x:Namespace_Conventions_In_Joomla / Display title: Conventions d'espace de nommage dans Joomla -->

Joomla!  4.0

### Espaces de noms de base

Les conventions d'appellation recommandées dans Joomla 4 sont les
suivantes :

### Composants

#### Composants natifs

- Joomla\Component\\*ComponentName*\\*ClientName*
  - Joomla\Component\Content\Site
  - Joomla\Component\Content\Administrator

##### Composants tiers

- *CompanyName*\Component\\*ComponentName*\\*ClientName*
  - Acme\Component\Backup\Site
  - Acme\Component\Backup\Administrator

##### Exemples d'emplacement

*Joomla\Component\Content\Site\Controller*  
components/com_content/Controller/ArticleController.php

*Joomla\Component\Content\Administrator\Controller*  
administrator/components/com_content/Controller/ArticlesController.php

### Modules

##### Modules natifs

- Joomla\Module\\*ModuleName*\\*ClientName*
  - Joomla\Module\UsersLatest\Site
  - Joomla\Module\Quickicon\Administrator

##### Modules tiers

- *CompanyName*\Module\\*ModuleName*\\*ClientName*
  - Acme\Module\SubscriptionStatus\Site
  - Acme\Module\LastBackupStatus\Administrator

##### Exemples d'emplacement

*Joomla\Module\Quickicon\Administrator\Dispatcher*  
administrator/modules/mod_quickicon/Dispatcher/Dispatcher.php

*Joomla\Module\UsersLatest\Site\Helper*  
modules/mod_users_latest/Helper/UsersLatestHelper.php

### Plugins

#### Plugins natifs

- Joomla\Plugin\\*PluginGroup*\\*PluginName*
  - Joomla\Plugin\System\Debug
  - Joomla\Plugin\Quickicon\Joomlaupdate

#### Plugins tiers

- *CompanyName*\Plugin\\*PluginGroup*\\*PluginName*\>
  - Acme\Plugin\System\BackupOnDemand

##### Exemples d'emplacement

*Joomla\Plugin\System\Debug*  
plugins/system/debug/DataFormatter.php

*Joomla\Plugin\Quickicon\Joomlaupdate\Extension*  
plugins/quickicon/joomlaupdate/Extension/Joomlaupdate.php

### Framework

#### Librairie Joomla Framework

- Joomla
  - Joomla\Session
  - Joomla\Image

#### Librairie Joomla CMS

- Joomla\CMS
  - Joomla\CMS\Mail
  - Joomla\CMS\MVC

##### Exemples d'emplacement

*Joomla\Session*  
libraries/vendor/joomla/session/src/Session.php

*Joomla\CMS\Mail*  
libraries/src/Mail/Mail.php
