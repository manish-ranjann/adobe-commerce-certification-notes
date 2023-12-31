# Configuration importers

###### A configuration importer provides consistent configurations across multiple systems (such as development, staging, and production). This is useful for deployment strategies such as pipeline deployment.

###### Adobe Commerce and Magento Open Source use configuration importers to import configuration data from the shared configuration file, config.php, to the appropriate storage, such as a database.

######  Use the magento app:config:import command to import the configuration from the command line.

######  Currently, the application has the following importers:

* Magento\Config\Model\Config\Importer
* Magento\Store\Model\Config\Importer
* Magento\Theme\Model\Config\Importer

#### ImporterInterface
###### All importers implement the interface Magento\Framework\App\DeploymentConfig\ImporterInterface and define the following methods:

###### import(array $data) - The argument $data is the configuration array from config.php.

###### This method should return the array of messages generated during the import process.

###### getWarningMessages(array $data) - Generates and returns the array of warning messages that contain information about what will be changed in the system.

###### The $data argument is the same as for the method import.

###### If this method returns an empty array, the import proceeds without interaction.

###### You can also provide a message such as Do you want to continue [yes/no]?

###### If the user enters no, import is canceled; otherwise, if the user enters yes, the import is processed.

#### Implement your own importer
###### Create an Importer class that implements Magento\Framework\App\DeploymentConfig\ImporterInterface.

##### Register your importer in your module's di.xml:

```
<type name="Magento\Deploy\Model\DeploymentConfig\ImporterPool">
    <arguments>
        <argument name="importers" xsi:type="array">
            <item name="i18n" xsi:type="array">
                <item name="class" xsi:type="string">Vendor\Module\Model\Config\Importer</item>
                <item name="sortOrder" xsi:type="number">110</item>
            </item>
        </argument>
    </arguments>
</type>
```

###### The sample code in the preceding example registers the importer Vendor\Module\Model\Config\Importer for the i18n array in config.php.

###### The i18n array has a queue order of 110, which means this importer runs after importers that have value of sort order less than 110 has and if values in the section i18n were changed.

###### An array cannot be imported by more than one importer.

