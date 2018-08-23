---
title: Publiceren-WebApplicationWebSite (Windows PowerShell-script) | Microsoft Docs
description: Leer hoe u een webproject kunt publiceren naar een Azure-website. Dit script maakt u de vereiste resources in uw Azure-abonnement als deze nog niet bestaan.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 63cfaa2d-f04d-40dc-8677-345385c278d5
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/11/2016
ms.author: ghogen
ms.openlocfilehash: ea8e36aabb75839a9c301f45a82241e3a859d42a
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42054518"
---
# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Publiceren-WebApplicationWebSite (Windows PowerShell-script)
## <a name="syntax"></a>Syntaxis
Een webproject publiceert naar een Azure-website. Het script maakt u de vereiste resources in uw Azure-abonnement als deze nog niet bestaan.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Configuratie
Het pad naar het JSON-configuratiebestand dat de details van de implementatie worden beschreven.

| Parameter | Standaardwaarde |
| --- | --- |
| Aliassen |geen |
| Vereist? |true |
| Positie |met de naam |
| Standaardwaarde |geen |
| Pijpleidinginvoer accepteren? |false |
| Jokertekens accepteren? |false |

## <a name="subscriptionname"></a>Abonnementsnaam
De naam van de Azure-abonnement dat u wilt maken van de website in.

| Parameter | Standaardwaarde |
| --- | --- |
| Aliassen |geen |
| Vereist? |false |
| Positie |met de naam |
| Standaardwaarde |geen |
| Pijpleidinginvoer accepteren? |false |
| Jokertekens accepteren? |false |

## <a name="webdeploypackage"></a>WebDeployPackage
Het pad naar de webimplementatiepakket om naar de website te publiceren. U kunt dit pakket maken met behulp van de wizard Web publiceren in Visual Studio. Zie voor meer informatie, [aan de slag met Azure Cloud Services en ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).

| Parameter | Standaardwaarde |
| --- | --- |
| Aliassen |geen |
| Vereist? |false |
| Positie |met de naam |
| Standaardwaarde |geen |
| Pijpleidinginvoer accepteren? |false |
| Jokertekens accepteren? |false |

## <a name="databaseserverpassword"></a>DatabaseServerPassword
De gebruikersnaam en wachtwoord voor de SQL-database in Azure.

| Parameter | Standaardwaarde |
| --- | --- |
| Aliassen |geen |
| Vereist? |false |
| Positie |met de naam |
| Standaardwaarde |geen |
| Pijpleidinginvoer accepteren? |false |
| Jokertekens accepteren? |false |

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
Indien waar, wordt de status van afdrukken berichten van het script naar de uitvoerstroom.

| Parameter | Standaardwaarde |
| --- | --- |
| Aliassen |geen |
| Vereist? |false |
| Positie |met de naam |
| Standaardwaarde |false |
| Pijpleidinginvoer accepteren? |false |
| Jokertekens accepteren? |false |

## <a name="remarks"></a>Opmerkingen
Zie voor een volledige uitleg van hoe u het script te maken met ontwikkel- en testomgevingen, [Windows PowerShell-Scripts gebruiken om te publiceren op de ontwikkel- en testomgevingen](vs-azure-tools-publishing-using-powershell-scripts.md).

De JSON-configuratiebestand Hiermee geeft u de details van wat is om te worden geïmplementeerd. Deze bevat de informatie die u hebt opgegeven tijdens het maken van het project, zoals de naam en de gebruikersnaam voor de website. Dit omvat ook de database wilt inrichten, indien van toepassing. De volgende code toont een voorbeeld van JSON-configuratiebestand:

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

U kunt de JSON-configuratiebestand om te wijzigen wat wordt geïmplementeerd. Een webSite-sectie is vereist, maar de database-sectie is optioneel.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie, [publiceren-WebApplicationVM (Windows PowerShell-script)](vs-azure-tools-publish-webapplicationvm.md)

