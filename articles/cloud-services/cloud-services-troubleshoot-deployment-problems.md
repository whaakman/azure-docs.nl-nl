---
title: Problemen met cloud service-implementaties oplossen | Microsoft Docs
description: Er zijn enkele veelvoorkomende problemen die u ondervindt mogelijk bij het implementeren van een service in de cloud naar Azure. In dit artikel voorziet in oplossingen voor enkele ervan.
services: cloud-services
documentationcenter: ''
author: simonxjx
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: a18ae415-0d1c-4bc4-ab6c-c1ddea02c870
ms.service: cloud-services
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/15/2018
ms.author: v-six
ms.openlocfilehash: 7f0e65b1de1df48603cab29148c7f4c6fb909714
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47094994"
---
# <a name="troubleshoot-cloud-service-deployment-problems"></a>Problemen met cloud service-implementaties oplossen
Wanneer u een toepassingspakket voor cloud-service in Azure implementeert, kunt u informatie over de implementatie van de **eigenschappen** deelvenster in de Azure-portal. U kunt de details in dit deelvenster gebruiken om u te helpen bij het oplossen van problemen met de cloudservice en u kunt deze informatie opgeven voor de ondersteuning van Azure bij het openen van een nieuwe ondersteuningsaanvraag.

U vindt de **eigenschappen** deelvenster als volgt te werk:

* In de Azure-portal, klikt u op de implementatie van uw cloudservice **alle instellingen**, en klik vervolgens op **eigenschappen**.

> [!NOTE]
> U kunt de inhoud van de **eigenschappen** deelvenster naar het Klembord door te klikken op het pictogram in de rechterbovenhoek van het deelvenster.
>
>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="problem-i-cannot-access-my-website-but-my-deployment-is-started-and-all-role-instances-are-ready"></a>Probleem: ik kan geen toegang tot mijn website, maar mijn implementatie is gestart en alle rolinstanties zijn gereed
De website-URL-koppeling die wordt weergegeven in de portal bevat geen de poort. De standaardpoort voor websites is 80. Als uw toepassing is geconfigureerd om te worden uitgevoerd in een andere poort, moet u het juiste poortnummer toevoegen aan de URL bij het openen van de website.

1. Klik op de implementatie van uw cloudservice in de Azure-portal.
2. In de **eigenschappen** in de Azure portal, controleert u de poorten voor de rolinstanties (onder **invoer eindpunten**).
3. Als de poort niet 80, moet u de waarde van de juiste poort toevoegen aan de URL wanneer u toegang de toepassing tot. Als een niet-standaard poort opgeven, typt u de URL, gevolgd door een dubbele punt (:), gevolgd door het poortnummer, zonder spaties.

## <a name="problem-my-role-instances-recycled-without-me-doing-anything"></a>Probleem: Mijn rolinstanties gerecycled zonder dat ik iets doen
Service healing gebeurt automatisch wanneer u Azure knooppunten probleem wordt gedetecteerd en daarom rolinstanties verplaatst naar nieuwe knooppunten. Wanneer dit gebeurt, ziet u mogelijk uw rolinstanties automatisch herhalen. Zoek uit of serviceherstel zijn opgetreden:

1. Klik op de implementatie van uw cloudservice in de Azure-portal.
2. In de **eigenschappen** in de Azure portal, lees de informatie en bepalen of serviceherstel opgetreden tijdens de periode dat u de rollen recycling waargenomen.

Rollen wordt ook ongeveer recycle één keer per maand tijdens het host-OS- en Gast-OS-updates.  
Zie voor meer informatie het blogbericht [rol exemplaar wordt opnieuw opgestart vanwege de Upgrades voor het besturingssysteem](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)

## <a name="problem-i-cannot-do-a-vip-swap-and-receive-an-error"></a>Probleem: die ik kan geen VIP's wisselen doen en een foutbericht ontvangt bij
Geen VIP's wisselen is niet toegestaan als een implementatie-update uitgevoerd wordt. Implementatie-updates automatisch kunnen worden uitgevoerd wanneer:

* Een nieuwe gastbesturingssysteem beschikbaar is en u zijn geconfigureerd voor automatische updates.
* Serviceherstel optreedt.

Als u wilt nagaan of een automatische update voorkomt dat u geen VIP's wisselen doen:

1. Klik op de implementatie van uw cloudservice in de Azure-portal.
2. In de **eigenschappen** deelvenster van de Azure-portal, kijkt u naar de waarde van **Status**. Als het **gereed**, controleert u **laatste bewerking** om te zien als een onlangs is er gebeurd die mogelijk te voorkomen dat het wisselen van VIP.
3. Herhaal stappen 1 en 2 voor de productie-implementatie.
4. Als een automatische update uitgevoerd wordt, wacht tot deze is voltooid voordat u probeert uit te voeren van het wisselen van VIP.

## <a name="problem-a-role-instance-is-looping-between-started-initializing-busy-and-stopped"></a>Probleem: Er is een rolinstantie lus van gestart, Bezig met initialiseren, bezet en gestopt
Dit probleem kan duiden op een probleem met uw programmacode, pakket of configuratiebestand. In dat geval moet u kunnen de status wijzigen om de paar minuten kunt bekijken en de Azure-portal kan staan er ongeveer als **Bezig met recyclen**, **bezet**, of **Bezig met initialiseren**. Dit geeft aan dat er iets mis met de toepassing die is het houden van de rolinstantie die worden uitgevoerd.

Zie voor meer informatie over het oplossen van dit probleem, het blogbericht [Azure PaaS Compute Diagnostics Data](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx) en [algemene problemen waardoor rollen worden herhaald](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

## <a name="problem-my-application-stopped-working"></a>Probleem: Mijn toepassing werkt niet
1. Klik op de rolinstantie in de Azure-portal.
2. In de **eigenschappen** in de Azure portal, houd rekening met de volgende voorwaarden om uw probleem te verhelpen:
   * Als de rolinstantie is onlangs gestopt (u kunt de waarde van controleren **aantal afbrekingen**), de implementatie kan worden bijgewerkt. Wacht om te zien als de rolinstantie werkt op een eigen hervat.
   * Als de rolinstantie is **bezet**, controleert u de code van uw toepassing om te controleren of de [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck) gebeurtenis wordt verwerkt. Mogelijk moet u toevoegen of corrigeren van code die verantwoordelijk is voor deze gebeurtenis.
   * Ga via de diagnostische gegevens en probleemoplossende scenario's in het blogbericht [Azure PaaS Compute Diagnostics Data](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

> [!WARNING]
> Als u uw cloudservice recyclen, u opnieuw instellen van de eigenschappen voor de implementatie daadwerkelijk wissen van de gegevens voor het oorspronkelijke probleem.
>
>

## <a name="next-steps"></a>Volgende stappen
Meer weergeven [artikelen over probleemoplossing](https://docs.microsoft.com/azure/cloud-services/cloud-services-allocation-failures) voor cloudservices.

Zie voor informatie over het oplossen van problemen met de rol van de cloud service met behulp van Azure PaaS computer diagnostische gegevens, [van Kevin Williamson blogserie](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
