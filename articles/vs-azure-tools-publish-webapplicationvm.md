---
title: Publiceren WebApplicationVM | Microsoft Docs
description: Informatie over het implementeren van een webtoepassing met een virtuele machine. Dit script maakt de vereiste resources in uw Azure-abonnement als deze nog niet bestaan.
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: de4cec95-f73f-44d9-babd-9f47f2633cdb
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 2738fc1dff50a177a227ae2c7719bd9a192d82ad
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="publish-webapplicationvm-windows-powershell-script"></a>Publiceren WebApplicationVM (Windows PowerShell-script)
Een webtoepassing met een virtuele machine implementeert. Het script maakt de vereiste resources in uw Azure-abonnement als deze nog niet bestaan.

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
Het pad naar het JSON-configuratiebestand dat u de details van de implementatie worden beschreven.

| Aliassen | Geen |
| --- | --- |
| Vereist? |De waarde True |
| Positie |Met de naam |
| Standaardwaarde |Geen |
| Pijpleidinginvoer accepteren? |ONWAAR |
| Jokertekens accepteren? |ONWAAR |

### <a name="subscriptionname"></a>SubscriptionName
De naam van het Azure-abonnement waarin u wilt maken van de virtuele machine.

| Aliassen | Geen |
| --- | --- |
| Vereist? |ONWAAR |
| Positie |Met de naam |
| Standaardwaarde |Maakt gebruik van het eerste abonnement in het abonnementsbestand |
| Pijpleidinginvoer accepteren? |ONWAAR |
| Jokertekens accepteren? |ONWAAR |

### <a name="webdeploypackage"></a>WebDeployPackage
Het pad naar het implementatiepakket web publiceren naar de virtuele machine. U kunt dit pakket kunt maken met behulp van de wizard webpublicatie in Visual Studio. Zie [procedure: een Web-implementatiepakket maken in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).

| Aliassen | Geen |
| --- | --- |
| Vereist? |ONWAAR |
| Positie |Met de naam |
| Standaardwaarde |Geen |
| Pijpleidinginvoer accepteren? |ONWAAR |
| Jokertekens accepteren? |ONWAAR |

### <a name="allowuntrusted"></a>AllowUntrusted
Indien waar, wordt het gebruik van certificaten die niet zijn ondertekend door een vertrouwde basiscertificeringsinstantie toestaan.

| Aliassen | Geen |
| --- | --- |
| Vereist? |ONWAAR |
| Positie |Met de naam |
| Standaardwaarde |ONWAAR |
| Pijpleidinginvoer accepteren? |ONWAAR |
| Jokertekens accepteren? |ONWAAR |

### <a name="vmpassword"></a>VMPassword
De referenties voor de account van de virtuele machine. Voorbeeld: - VMPassword @{naam = 'admin'; Wachtwoord = 'password'}

| Aliassen | Geen |
| --- | --- |
| Vereist? |ONWAAR |
| Positie |Met de naam |
| Standaardwaarde |Geen |
| Pijpleidinginvoer accepteren? |ONWAAR |
| Jokertekens accepteren? |ONWAAR |

### <a name="databaseserverpassword"></a>DatabaseServerPassword
De referenties voor de SQL-database in Azure. Voorbeeld: - DatabaseServerPassword @{naam = 'admin'; Wachtwoord = 'password'}

| Aliassen | Geen |
| --- | --- |
| Vereist? |ONWAAR |
| Positie |Met de naam |
| Standaardwaarde |Geen |
| Pijpleidinginvoer accepteren? |ONWAAR |
| Jokertekens accepteren? |ONWAAR |

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
Indien waar, wordt de status van afdrukken berichten van het script naar de uitvoerstroom.

| Aliassen | Geen |
| --- | --- |
| Vereist? |ONWAAR |
| Positie |Met de naam |
| Standaardwaarde |ONWAAR |
| Pijpleidinginvoer accepteren? |ONWAAR |
| Jokertekens accepteren? |ONWAAR |

## <a name="remarks"></a>Opmerkingen
Zie voor een volledige uitleg over het gebruik van het script maken Dev- en testomgevingen, [Windows PowerShell-Scripts gebruiken om te publiceren op de ontwikkeling en testomgevingen](vs-azure-tools-publishing-using-powershell-scripts.md).

Het JSON-configuratiebestand geeft de details van wat is geïmplementeerd. Deze bevat de informatie die u hebt opgegeven toen u het project, zoals de naam, de affiniteitsgroep, de VHD-installatiekopie en de grootte van de virtuele machine hebt gemaakt. Ook bevat de eindpunten van de virtuele machine en de databases naar inricht, indien van toepassing en implementatieparameters van web. De volgende code toont een voorbeeld van de JSON-configuratiebestand:

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

U kunt het JSON-configuratiebestand om te wijzigen wat is ingericht bewerken. Een virtuele machine en een cloudservice zijn vereist, maar de database-sectie is optioneel.

