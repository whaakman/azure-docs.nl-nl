---
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 11/25/2018
ms.author: spelluru
ms.openlocfilehash: b6e0e57881154f5885e9f518363eda3c5b1169a0
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52330902"
---
### <a name="install-via-composer"></a>Via Composer installeren
1. Maak een bestand met de naam **composer.json** in de hoofdmap van uw project en voeg de volgende code toe:
   
    ```json
    {
      "require": {
        "microsoft/azure-storage": "*"
      }
    }
    ```
2. Download **[composer.phar] [ composer-phar]** in de hoofdmap van uw project.
3. Open een opdrachtprompt en voer de volgende opdracht uit in de hoofdmap van uw project
   
    ```
    php composer.phar install
    ```

U kunt ook naar de [Azure Storage-clientbibliotheek met PHP] [ php-sdk-github] op GitHub te klonen van de broncode.

[php-sdk-github]: https://github.com/Azure/azure-storage-php
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[download-SDK-PHP]: ../articles/php-download-sdk.md
[composer-phar]: http://getcomposer.org/composer.phar
