---
title: Dynamics 365 Solution Preparation
description: Framework voor het verpakken, installeren en verwijderen van onderdelen
services: Azure, Marketplace, Cloud Partner Portal,
author: pbutlerm
manager: Ricarod.Villalobos
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 919feb39c9cd84f8da0ef89827ad3e83e0eb9bc5
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64935258"
---
# <a name="dynamics-365-solution-preparation"></a>Dynamics 365 Solution Preparation

Het Dynamics 365 solutioning-systeem is een structuur voor het verpakken, installeren en verwijderen van onderdelen die specifieke bedrijfsfunctionaliteit leveren. Oplossingen worden gebruikt door ISV's en andere partners Microsoft Dynamics 365 voor het distribueren van extensies die ze maken.

Als u een bestaande Dynamics 365 (xRM) ISV bent, hebt u waarschijnlijk al hebt gemaakt een beheerde oplossing en een solution.zip-bestand. Zorg ervoor dat de 'weergavenaam' en 'Description' velden weer wat u wilt de klanten om te zien in uw oplossing. Deze worden weergegeven in het beheercentrum voor CRM Online.

![CRMScreenShot1](media/CRMScreenShot1.png)

_**Opmerking:** In het volgende voorbeeld pakket wordt ervan uitgegaan dat naam van de oplossing is "SampleSolution.zip"_

Als u een nieuwe ISV bent, krijgt u meer informatie over het maken van een oplossing hier: [https://msdn.microsoft.com/library/gg334530.aspx](https://msdn.microsoft.com/library/gg334530.aspx)

Als uw oplossing ondersteuning van gegevens vereist:

* De voorbeeldgegevens in uw testomgeving maken
* Gebruik het hulpprogramma voor migratie van configuratie om een Schema met vergelijking van de regels voor uw gegevens.
* Uw configuratieschema met de projectbestanden niet opslaan. U nodig deze later als u de configuratiegegevens van uw bijwerken.
* Uw configuratiegegevens exporteren. Zorg ervoor dat het exportbestand Geef een naam die zinvol is voor het exporteren.
