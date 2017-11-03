### <a name="install-via-composer"></a>Installeren via de Composer
1. Maak een bestand met de naam **composer.json** in de hoofdmap van uw project en voeg de volgende code toe:
   
    ```json
    {
      "require": {
        "microsoft/azure-storage": "*"
      }
    }
    ```
2. Download  **[composer.phar] [ composer-phar]**  in de hoofdmap van uw project.
3. Open een opdrachtprompt en voer de volgende opdracht in de hoofdmap van uw project
   
    ```
    php composer.phar install
    ```

U kunt ook gaat u naar de [Azure Storage-clientbibliotheek met PHP] [ php-sdk-github] op GitHub voor het klonen van de broncode.

[php-sdk-github]: https://github.com/Azure/azure-storage-php
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[download-SDK-PHP]: ../articles/php-download-sdk.md
[composer-phar]: http://getcomposer.org/composer.phar
