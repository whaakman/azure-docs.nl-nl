---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 3a065e5cd6e951544b3147d5833b4ad300ae5e30
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50166402"
---
De [Configuration Manager-bibliotheek van Microsoft Azure voor .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) biedt een klasse voor het parseren van een verbindingsreeks uit een configuratiebestand. De klasse [CloudConfigurationManager](https://msdn.microsoft.com/library/azure/mt634650.aspx) parseert configuratie-instellingen, ongeacht of de clienttoepassing wordt uitgevoerd op een computer, op een mobiel apparaat, op een virtuele machine in Azure of in een Azure-cloudservice.

Voeg de volgende `using`-richtlijn toe om naar het CloudConfigurationManager-pakket te verwijzen:

```csharp
using Microsoft.Azure; //Namespace for CloudConfigurationManager
```

In het volgende voorbeeld ziet u hoe u een verbindingsreeks ophaalt uit een configuratiebestand:

```csharp
// Parse the connection string and return a reference to the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));
```

Het gebruik van Azure Configuration Manager is optioneel. U kunt ook een API gebruiken, zoals de [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx)-klasse van .NET Framework.

