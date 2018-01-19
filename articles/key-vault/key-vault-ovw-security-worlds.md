---
ms.assetid: 
title: Beveiligingswerelden voor Azure Sleutelkluis | Microsoft Docs
ms.service: key-vault
author: lleonard-msft
ms.author: alleonar
manager: mbaldwin
ms.date: 07/03/2017
ms.openlocfilehash: aeab36153d3a4e004d12206bab92cea9b30400ad
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a>Beveiligingswerelden voor Azure Sleutelkluis en de geografische grenzen

Azure Sleutelkluis is een multitenant-service en een pool van Hardware Security Modules (HSM's) in elke Azure-locatie gebruikt. 

Alle HSM's van Azure locaties in dezelfde geografische regio delen dezelfde cryptografische grens (Beveiligingswereld Thales). Bijvoorbeeld, VS-Oost en VS-West delen de dezelfde beveiligingswereld omdat ze deel uitmaken van de VS geografische locatie. Op deze manier delen alle Azure locaties in Japan de dezelfde beveiligingswereld en alle Azure locaties in Australië en India. 

## <a name="backup-and-restore-behavior"></a>Gedrag van back-up en herstel

Een back-up van een sleutel van een sleutelkluis in een Azure-locatie kan worden hersteld naar een sleutelkluis in een andere Azure-locatie, mits beide volgende voorwaarden voldaan wordt:

- Zowel de Azure locaties deel uitmaken van dezelfde geografische locatie
- Zowel de sleutelkluizen behoren tot dezelfde Azure-abonnement

Bijvoorbeeld, kan een back-up door een bepaald abonnement van een sleutel in een sleutelkluis in India West, alleen worden teruggezet naar een andere sleutelkluis in hetzelfde abonnement en dezelfde geografische locatie; India West, India centraal of Zuid, India.

## <a name="regions-and-products"></a>Regio's en -producten

- [Azure-regio's](https://azure.microsoft.com/regions/)
- [Microsoft-producten per regio](https://azure.microsoft.com/regions/services/)

Regio's zijn toegewezen aan beveiligingswerelden, weergegeven als primaire koppen in de tabellen:

In de producten per regio artikel, bijvoorbeeld de **Americas** tabblad bevat Oost US, VS-midden, VS-WEST alle toewijzing voor de regio Americas. 

>[!NOTE]
>Een uitzondering is dat ons DOD Oost en ons DOD centrale hun eigen beveiligingswerelden hebben. 

Op deze manier op de **Europa** tabblad beide toegewezen aan de regio Europa Noord-Europa en WEST-Europa. Hetzelfde geldt ook voor de **Azië** tabblad.



