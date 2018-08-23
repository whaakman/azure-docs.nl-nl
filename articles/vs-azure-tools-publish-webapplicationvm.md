---
title: Publiceren WebApplicationVM | Microsoft Docs
description: Informatie over het implementeren van een webtoepassing met een virtuele machine. Dit script maakt u de vereiste resources in uw Azure-abonnement als deze nog niet bestaan.
services: visual-studio-online
author: ghogen
manager: douge
assetId: de4cec95-f73f-44d9-babd-9f47f2633cdb
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/11/2016
ms.author: ghogen
ms.openlocfilehash: c2dc6057eeb4eba1306309785e13192674bc43c6
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42060787"
---
# <a name="publish-webapplicationvm-windows-powershell-script"></a>Publiceren-WebApplicationVM (Windows PowerShell-script)
Een webtoepassing met een virtuele machine implementeert. Het script maakt u de vereiste resources in uw Azure-abonnement als deze nog niet bestaan.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a>Configuratie
Het pad naar het JSON-configuratiebestand dat de details van de implementatie worden beschreven.

| Aliassen | geen |
| --- | --- |
| Vereist? |true |
| Positie |met de naam |
| Standaardwaarde |geen |
| Pijpleidinginvoer accepteren? |false |
| Jokertekens accepteren? |false |

### <a name="subscriptionname"></a>Abonnementsnaam
De naam van het Azure-abonnement waarin u wilt maken van de virtuele machine.

| Aliassen | geen |
| --- | --- |
| Vereist? |false |
| Positie |met de naam |
| Standaardwaarde |Maakt gebruik van het eerste abonnement in het abonnementsbestand |
| Pijpleidinginvoer accepteren? |false |
| Jokertekens accepteren? |false |

### <a name="webdeploypackage"></a>WebDeployPackage
Het pad naar de webimplementatiepakket om te publiceren naar de virtuele machine. U kunt dit pakket maken met behulp van de wizard Web publiceren in Visual Studio. Zie [hoe: een Web-implementatiepakket maken in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).

| Aliassen | geen |
| --- | --- |
| Vereist? |false |
| Positie |met de naam |
| Standaardwaarde |geen |
| Pijpleidinginvoer accepteren? |false |
| Jokertekens accepteren? |false |

### <a name="allowuntrusted"></a>AllowUntrusted
Indien waar, kunt u het gebruik van certificaten die niet zijn ondertekend door een vertrouwde basis-CA.

| Aliassen | geen |
| --- | --- |
| Vereist? |false |
| Positie |met de naam |
| Standaardwaarde |false |
| Pijpleidinginvoer accepteren? |false |
| Jokertekens accepteren? |false |

### <a name="vmpassword"></a>VMPassword
De referenties voor de virtuele machine-account. Voorbeeld: - VMPassword @{naam = 'admin'; Wachtwoord = "password"}

| Aliassen | geen |
| --- | --- |
| Vereist? |false |
| Positie |met de naam |
| Standaardwaarde |geen |
| Pijpleidinginvoer accepteren? |false |
| Jokertekens accepteren? |false |

### <a name="databaseserverpassword"></a>DatabaseServerPassword
De referenties voor de SQL-database in Azure. Voorbeeld: - DatabaseServerPassword @{naam = 'admin'; Wachtwoord = "password"}

| Aliassen | geen |
| --- | --- |
| Vereist? |false |
| Positie |met de naam |
| Standaardwaarde |geen |
| Pijpleidinginvoer accepteren? |false |
| Jokertekens accepteren? |false |

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
Indien waar, wordt de status van afdrukken berichten van het script naar de uitvoerstroom.

| Aliassen | geen |
| --- | --- |
| Vereist? |false |
| Positie |met de naam |
| Standaardwaarde |false |
| Pijpleidinginvoer accepteren? |false |
| Jokertekens accepteren? |false |

## <a name="remarks"></a>Opmerkingen
Zie voor een volledige uitleg van hoe u het script te maken met ontwikkel- en testomgevingen, [Windows PowerShell-Scripts gebruiken om te publiceren op de ontwikkel- en testomgevingen](vs-azure-tools-publishing-using-powershell-scripts.md).

De JSON-configuratiebestand Hiermee geeft u de details van wat is om te worden geïmplementeerd. Deze bevat de informatie die u hebt opgegeven tijdens het maken van het project, zoals de naam, de affiniteitsgroep, het VHD-installatiekopie en de grootte van de virtuele machine. Ook bevat de eindpunten van de virtuele machine, de databases om in te richten, indien van toepassing en implementatieparameters web. De volgende code toont een voorbeeld van JSON-configuratiebestand:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

U kunt de JSON-configuratiebestand om te wijzigen wat is ingericht bewerken. Een virtuele machine en een service in de cloud zijn vereist, maar de database-sectie is optioneel.

