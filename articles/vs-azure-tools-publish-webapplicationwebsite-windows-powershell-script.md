---
title: Publiceren WebApplicationWebSite (Windows PowerShell-script) | Microsoft Docs
description: Informatie over het publiceren van een webproject naar een Azure-website. Dit script maakt de vereiste resources in uw Azure-abonnement als deze nog niet bestaan.
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 63cfaa2d-f04d-40dc-8677-345385c278d5
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 07d21b7ce6cd8aee1cff704d316e7a2ca8c00437
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Publiceren WebApplicationWebSite (Windows PowerShell-script)
## <a name="syntax"></a>Syntaxis
Een webproject publiceert naar een Azure-website. Het script maakt de vereiste resources in uw Azure-abonnement als deze nog niet bestaan.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Configuratie
Het pad naar het JSON-configuratiebestand dat u de details van de implementatie worden beschreven.

| Parameter | Standaardwaarde |
| --- | --- |
| Aliassen |Geen |
| Vereist? |De waarde True |
| Positie |Met de naam |
| Standaardwaarde |Geen |
| Pijpleidinginvoer accepteren? |ONWAAR |
| Jokertekens accepteren? |ONWAAR |

## <a name="subscriptionname"></a>SubscriptionName
De naam van de Azure-abonnement dat u wilt maken van de website in.

| Parameter | Standaardwaarde |
| --- | --- |
| Aliassen |Geen |
| Vereist? |ONWAAR |
| Positie |Met de naam |
| Standaardwaarde |Geen |
| Pijpleidinginvoer accepteren? |ONWAAR |
| Jokertekens accepteren? |ONWAAR |

## <a name="webdeploypackage"></a>WebDeployPackage
Het pad naar het implementatiepakket web publiceren naar de website. U kunt dit pakket kunt maken met behulp van de wizard webpublicatie in Visual Studio. Zie voor meer informatie [aan de slag met Azure Cloud Services en ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).

| Parameter | Standaardwaarde |
| --- | --- |
| Aliassen |Geen |
| Vereist? |ONWAAR |
| Positie |Met de naam |
| Standaardwaarde |Geen |
| Pijpleidinginvoer accepteren? |ONWAAR |
| Jokertekens accepteren? |ONWAAR |

## <a name="databaseserverpassword"></a>DatabaseServerPassword
De gebruikersnaam en het wachtwoord voor de SQL-database in Azure.

| Parameter | Standaardwaarde |
| --- | --- |
| Aliassen |Geen |
| Vereist? |ONWAAR |
| Positie |Met de naam |
| Standaardwaarde |Geen |
| Pijpleidinginvoer accepteren? |ONWAAR |
| Jokertekens accepteren? |ONWAAR |

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
Indien waar, wordt de status van afdrukken berichten van het script naar de uitvoerstroom.

| Parameter | Standaardwaarde |
| --- | --- |
| Aliassen |Geen |
| Vereist? |ONWAAR |
| Positie |Met de naam |
| Standaardwaarde |ONWAAR |
| Pijpleidinginvoer accepteren? |ONWAAR |
| Jokertekens accepteren? |ONWAAR |

## <a name="remarks"></a>Opmerkingen
Zie voor een volledige uitleg over het gebruik van het script maken Dev- en testomgevingen, [Windows PowerShell-Scripts gebruiken om te publiceren op de ontwikkeling en testomgevingen](vs-azure-tools-publishing-using-powershell-scripts.md).

Het JSON-configuratiebestand geeft de details van wat is geïmplementeerd. Dit omvat de informatie die u hebt opgegeven toen u het project, zoals de naam en de gebruikersnaam voor de website hebt gemaakt. Dit omvat ook de database wilt inrichten, indien van toepassing. De volgende code toont een voorbeeld van de JSON-configuratiebestand:

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

Kunt u het JSON-configuratiebestand om te wijzigen wat wordt geïmplementeerd. Een gedeelte van de webSite is vereist, maar de database-sectie is optioneel.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie [publiceren-WebApplicationVM (Windows PowerShell-script)](vs-azure-tools-publish-webapplicationvm.md)

