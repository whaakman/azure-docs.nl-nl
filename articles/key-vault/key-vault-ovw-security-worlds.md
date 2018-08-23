---
ms.assetid: ''
title: Beveiligingswerelden van Azure Key Vault | Microsoft Docs
ms.service: key-vault
author: bryanla
ms.author: bryanla
manager: mbaldwin
ms.date: 07/03/2017
ms.openlocfilehash: 7fd7357a317d5059d6169de2c1536568a254e016
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2018
ms.locfileid: "42058675"
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a>Azure Key Vault-beveiligingswerelden en geografische grenzen

Azure Key Vault is een multitenant-service en maakt gebruik van een pool van Hardware Security Modules (HSM's) in elke Azure-locatie. 

Alle HSM's van Azure-locaties in dezelfde geografische regio delen dezelfde cryptografische grens (Thales-Beveiligingswereld). Bijvoorbeeld, delen VS-Oost en VS-West de dezelfde beveiligingswereld omdat ze deel uitmaken van de Verenigde Staten geo-locatie. Alle Azure-locaties in Japan delen op dezelfde manier de dezelfde beveiligingswereld en alle Azure-locaties in Australië, India, enzovoort. 

## <a name="backup-and-restore-behavior"></a>Gedrag van de back-up en herstel

Een back-up gemaakt van een sleutel van een key vault in een Azure-locatie kan worden hersteld naar een key vault in een andere Azure-locatie, zolang beide volgende voorwaarden voldaan wordt:

- Beide van de Azure-locaties deel uitmaken van dezelfde geografische locatie
- Beide van de sleutelkluizen deel uitmaken van hetzelfde Azure-abonnement

Bijvoorbeeld, kunnen een back-up gemaakt door een bepaald abonnement van een sleutel in een key vault in West-India, alleen worden hersteld naar een andere sleutelkluis in hetzelfde abonnement en geolocatie. West-India, centraal-India of Zuid-India.

## <a name="regions-and-products"></a>Regio's en -producten

- [Azure-regio's](https://azure.microsoft.com/regions/)
- [Microsoft-producten per regio](https://azure.microsoft.com/regions/services/)

Regio's zijn toegewezen aan beveiligingswerelden, weergegeven als primaire kolomkoppen in de tabellen:

In de producten per regio artikel, bijvoorbeeld de **Americas** tabblad bevat OOSTELIJKE VS, VS-midden, VS-WEST alle toewijzing naar de regio Amerika. 

>[!NOTE]
>Een uitzondering is dat VS DOD Oost en VS DOD-CENTRAAL, hun eigen beveiligingswerelden hebben. 

Op dezelfde manier op de **Europa** tabblad beide toegewezen aan de regio Europa Noord-Europa en WEST-Europa. Hetzelfde geldt ook voor de **Azië en Stille Oceaan** tabblad.



