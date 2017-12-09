---
title: Problemen met cloud service-implementaties oplossen | Microsoft Docs
description: Er zijn enkele veelvoorkomende problemen die u uitvoeren kunt in een cloudservice implementeren in Azure. Dit artikel bevat oplossingen voor enkele ervan.
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: a18ae415-0d1c-4bc4-ab6c-c1ddea02c870
ms.service: cloud-services
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 11/03/2017
ms.author: v-six
ms.openlocfilehash: 3c56a5750c9f8a6c59ea07c01c101f358331174b
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="troubleshoot-cloud-service-deployment-problems"></a>Problemen met cloud service-implementatie oplossen
Wanneer u een cloud service application-pakket in Azure implementeert, kunt u informatie over de implementatie van de **eigenschappen** deelvenster in de Azure-portal. Kunt u de details in dit deelvenster om te helpen bij het oplossen van problemen met de cloudservice en geef deze informatie aan het Azure-ondersteuning bij het openen van een nieuwe ondersteuningsaanvraag.

U vindt de **eigenschappen** deelvenster als volgt:

* Klik op de implementatie van uw cloudservice in de Azure portal, **alle instellingen**, en klik vervolgens op **eigenschappen**.

> [!NOTE]
> Kunt u de inhoud van de **eigenschappen** deelvenster naar het Klembord door te klikken op het pictogram in de rechterbovenhoek van het deelvenster.
>
>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="problem-i-cannot-access-my-website-but-my-deployment-is-started-and-all-role-instances-are-ready"></a>Probleem: ik geen toegang tot mijn website, maar mijn implementatie is gestart en alle rolinstanties zijn gereed
De website-URL-koppeling wordt weergegeven in de portal is de poort niet opgenomen. De standaardpoort voor websites is 80. Als uw toepassing is geconfigureerd om te worden uitgevoerd in een andere poort, moet u het juiste poortnummer toevoegen aan de URL bij het openen van de website.

1. Klik op de implementatie van uw cloudservice in de Azure portal.
2. In de **eigenschappen** deelvenster van de Azure-portal, Controleer de poorten voor de rolexemplaren (onder **invoer eindpunten**).
3. Als de poort niet 80, moet u de waarde van de juiste poort toevoegen aan de URL wanneer u toegang de toepassing tot. Als u een niet-standaardpoort, typ de URL, gevolgd door een dubbele punt (:), gevolgd door het poortnummer, zonder spaties.

## <a name="problem-my-role-instances-recycled-without-me-doing-anything"></a>Probleem: Mijn rolinstanties gerecycled zonder mij doen
Service herstel gebeurt automatisch wanneer Azure probleem knooppunten detecteert en daarom rolinstanties naar nieuwe knooppunten verplaatst. Wanneer dit gebeurt, ziet u mogelijk uw rolinstanties automatisch worden gerecycled. Nagaan of herstel van de service is opgetreden:

1. Klik op de implementatie van uw cloudservice in de Azure portal.
2. In de **eigenschappen** deelvenster van de Azure-portal, Raadpleeg de informatie en bepalen of herstel van de service opgetreden tijdens de periode die u de rollen recyclen waargenomen.

Functies wordt ook ongeveer recycle één keer per maand tijdens host-OS en guest-OS-updates.  
Zie voor meer informatie het blogbericht [rol exemplaar opnieuw wordt opgestart vanwege Upgrades voor het besturingssysteem](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)

## <a name="problem-i-cannot-do-a-vip-swap-and-receive-an-error"></a>Probleem: die ik kan geen VIP's wisselen doen en een foutbericht ontvangt
Geen VIP's wisselen is niet toegestaan als een implementatie-update uitgevoerd wordt. Implementatie-updates automatisch kunnen worden uitgevoerd wanneer:

* Een nieuwe gastbesturingssysteem beschikbaar is en u zijn geconfigureerd voor automatische updates.
* Service herstel optreedt.

Als u wilt nagaan of een automatische verhindert update dat u geen VIP's wisselen doen:

1. Klik op de implementatie van uw cloudservice in de Azure portal.
2. In de **eigenschappen** deelvenster van de Azure portal, bekijkt u de waarde van **Status**. Als het **gereed**, controleert u **laatste bewerking** om te zien als een onlangs heeft plaatsgevonden waardoor mogelijk te voorkomen dat de VIP's wisselen.
3. Herhaal stap 1 en 2 voor de productie-implementatie.
4. Als een automatische update uitgevoerd wordt, wacht tot deze is voltooid voordat u probeert te doen de VIP's wisselen.

## <a name="problem-a-role-instance-is-looping-between-started-initializing-busy-and-stopped"></a>Probleem: Er is een rolinstantie lussen tussen gestart, initialiseert, bezet en gestopt
Dit probleem kan duiden op een probleem met uw programmacode, pakket of configuratiebestand. In dat geval moet u kunnen de status wijzigen om de paar minuten en de Azure-portal staat mogelijk ongeveer **Recycling**, **bezet**, of **Bezig met initialiseren**. Dit geeft aan dat er iets mis met de toepassing die is het houden van de rolinstantie wordt uitgevoerd.

Zie voor meer informatie over het oplossen van dit probleem, het blogbericht [Azure PaaS Compute Diagnostics-gegevens](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx) en [veelvoorkomende problemen die recycling van rollen veroorzaken](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

## <a name="problem-my-application-stopped-working"></a>Probleem: Mijn toepassing werkt niet
1. Klik op de rolinstantie in de Azure portal.
2. In de **eigenschappen** deelvenster van de Azure-portal, houd rekening met de volgende voorwaarden uw probleem op te lossen:
   * Als de instantie is onlangs gestopt (u kunt de waarde van controleren **afbreken aantal**), de implementatie kan worden bijgewerkt. Wacht om te zien als de rolinstantie werkt op een eigen hervat.
   * Als het rolexemplaar **bezet**, Controleer de toepassingscode van uw om te controleren of de [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck) gebeurtenis wordt verwerkt. Mogelijk moet u toevoegen of code die verantwoordelijk is voor deze gebeurtenis te corrigeren.
   * Ga via de diagnostische gegevens en probleemoplossende scenario's in het blogbericht [Azure PaaS Compute Diagnostics-gegevens](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

> [!WARNING]
> Als u uw cloudservice recyclen, u opnieuw instellen de eigenschappen voor de implementatie van de gegevens voor het oorspronkelijke probleem effectief te wissen.
>
>

## <a name="next-steps"></a>Volgende stappen
Meer [probleemoplossing artikelen](https://docs.microsoft.com/azure/cloud-services/cloud-services-allocation-failures) voor cloudservices.

Zie voor meer informatie over het oplossen van problemen met de rol van de cloud service met behulp van Azure PaaS computer diagnostics-gegevens, [blogreeks van Kevin Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
