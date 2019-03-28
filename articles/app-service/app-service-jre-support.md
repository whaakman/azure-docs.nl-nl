---
title: Azure App Service voor Windows Java Runtime-ondersteuning
description: Dit onderwerp vindt u de Java-Runtime-instructie van ondersteuning voor Azure App Service op Windows.
author: bmitchell287
ms.author: brendm; joshuapa
ms.date: 3/22/2019
ms.topic: article
ms.service: app-service
manager: dougE
ms.openlocfilehash: ab0e9b9f272108745b629d2cb284f6c5ab52c76f
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58522954"
---
# <a name="java-runtime-statement-of-support-for-app-service-on-windows"></a>Java runtime-instructie van de ondersteuning voor App Service onder Windows

## <a name="jdk-versions-and-maintenance"></a>JDK versies en onderhoud

Azure ondersteunde Java Development Kit (JDK) is [Zulu](https://www.azul.com/downloads/azure-only/zulu/) geleverd via [Azul Systems](https://www.azul.com/).

Belangrijke versieupdates aangeboden via de nieuwe runtime-opties in Azure App Service voor Windows. Klanten hebben bijgewerkt naar deze nieuwere versies van Java door het configureren van de App Service-implementatie en zijn verantwoordelijk voor het testen en ervoor te zorgen dat de grote update aan hun eisen voldoen.

Ondersteunde JDK worden automatisch gevuld op basis van een kwartaal in januari, April, juli en oktober van elk jaar.

## <a name="security-updates"></a>Beveiligingsupdates

Patches en oplossingen voor bekende beveiligingsproblemen wordt uitgebracht zodra deze beschikbaar van Azul Systems. Een beveiligingslek in de 'belangrijke' is gedefinieerd door een basis score van 9.0 of hoger op de [NIST algemene beveiligingsproblemen Scoring-systeem, versie 2](https://nvd.nist.gov/cvss.cfm).

## <a name="deprecation-and-retirement"></a>Afschaffing en buiten gebruik stellen

Als een ondersteunde Java-runtime wordt beëindigd, Azure-ontwikkelaars met behulp van de betrokken runtime krijgt een kennisgeving over afschaffing ten minste zes maanden voordat de runtime is buiten gebruik gesteld.

## <a name="local-development"></a>Lokale ontwikkeling

Ontwikkelaars kunnen de productie-editie van Azul Zulu Enterprise JDK voor lokale ontwikkeling van downloaden [site voor het downloaden van Azul](https://www.azul.com/downloads/azure-only/zulu/).

## <a name="development-support"></a>Ontwikkelingsondersteuning voor

Ondersteuning voor het product voor de [Azure wordt ondersteund Azul Zulu JDK](https://www.azul.com/downloads/azure-only/zulu/) is beschikbaar via Microsoft bij het ontwikkelen voor Azure of [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) met een [gekwalificeerde Azure-ondersteuningsplan](https://azure.microsoft.com/support/plans/).

## <a name="runtime-support"></a>Runtime-ondersteuning

Ontwikkelaars kunnen [opent u een probleem](/azure/azure-supportability/how-to-create-azure-support-request) met de JDK Azul Zulu via ondersteuning voor Azure als ze beschikken over een [gekwalificeerde ondersteuningsplan](https://azure.microsoft.com/support/plans/).

## <a name="next-steps"></a>Volgende stappen

Dit onderwerp vindt u de Java-Runtime-instructie van ondersteuning voor Azure App Service op Windows.
- Voor meer informatie over het hosten van webtoepassingen met Azure App Service gaat [App Service-overzicht](overview.md).
- Zie voor meer informatie over Java op Azure-ontwikkeling [Azure voor Java-ontwikkelaarscentrum](https://docs.microsoft.com/java/azure/?view=azure-java-stable).
