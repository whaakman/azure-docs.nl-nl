---
title: Systeemupdates in Azure Security Center toepassen | Microsoft Docs
description: Dit document ziet u hoe u de aanbevelingen voor Azure Security Center implementeren **systeemupdates toepassen** en **opnieuw opstarten na systeemupdates**.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: e5bd7f55-38fd-4ebb-84ab-32bd60e9fa7a
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: ebd9939128d1f2b870541e82710792d13b69728e
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58098998"
---
# <a name="apply-system-updates-in-azure-security-center"></a>Systeemupdates in Azure Security Center toepassen
Azure Security Center bewaakt dagelijkse Windows en Linux-machines (VM's) en computers voor het besturingssysteemupdates ontbreken. Security Center wordt een lijst met beschikbare beveiligingsupdates en essentiële updates opgehaald van Windows Update of Windows Server Update Services (WSUS), afhankelijk van welke service is geconfigureerd op een Windows-computer. Security Center wordt ook gecontroleerd voor de meest recente updates in Linux-systemen. Als uw virtuele machine of computer het bijwerken van een systeem ontbreekt, wordt Security Center aangeraden dat u systeemupdates toepassen.

## <a name="implement-the-recommendation"></a>De aanbeveling voor het implementeren
Toepassing system updates wordt weergegeven als een aanbeveling in Security Center. Als uw virtuele machine of computer het bijwerken van een systeem ontbreekt, deze aanbeveling wordt weergegeven onder **aanbevelingen** en klikt u onder **Compute**.  Hiermee opent u de aanbeveling selecteren de **systeemupdates toepassen** dashboard.

In dit voorbeeld gebruiken we **Compute**.

1. Selecteer **Compute** onder het hoofdmenu van Security Center.

   ![Selecteer berekenen][1]

2. Onder **Compute**, selecteer **ontbrekende systeemupdates**. De **systeemupdates toepassen** dashboard wordt geopend.

   ![Dashboard voor systeem-updates toepassen][2]

   De bovenkant van het dashboard biedt:

    - Het totale aantal Windows en Linux-VM's en computers met ontbrekende systeemupdates.
    - Het totale aantal essentiële updates ontbreken in uw virtuele machines en computers.
    - Het totale aantal beveiligingsupdates dat ontbreekt in uw virtuele machines en computers.

   Onder aan het dashboard geeft een lijst van alle ontbrekende updates voor uw virtuele machines, computers en de ernst van de ontbrekende update.  De lijst bevat:

    - NAAM: De naam van de ontbrekende update.
    - NEE. VM's en COMPUTERS: Totale aantal virtuele machines en computers die niet over deze update beschikken.
    - STATUS: De huidige status van de aanbeveling:

      - Open: De aanbeveling is nog niet opgelost.
      - Wordt uitgevoerd: De aanbeveling wordt momenteel toegepast op deze resources, en er is geen actie te ondernemen.
      - Opgelost: De aanbeveling is al voltooid. (Als het probleem is opgelost, wordt de vermelding grijs).

    - ERNST: Hierin wordt de ernst van deze bepaalde aanbeveling beschreven:

      - Hoog: Een beveiligingslek in de bestaat een belangrijke resource (toepassing, virtuele machine of netwerkbeveiligingsgroep) en aandacht vereist.
      - Normaal: Niet-kritieke of extra stappen zijn vereist om een proces te voltooien of een beveiligingsprobleem.
      - Laag: Een beveiligingsprobleem moet worden opgelost, maar geen onmiddellijke aandacht vereist. (Aanbevelingen met de ernstaanduiding Laag worden niet standaard weergegeven, maar u kunt hierop filteren als u deze aanbevelingen wilt bekijken.)

3. Selecteer een ontbrekende update in de lijst om details weer te geven.

   ![Ontbrekende beveiligingsupdate][3]

4. Selecteer de **zoeken** pictogram in het bovenste lint.  Er wordt een zoekquery voor logboeken van Azure Monitor geopend gefilterd op de computers met ontbrekende van de update.

   ![Azure Monitor-logboeken zoeken][4]

5. Selecteer een computer in de lijst voor meer informatie. Een andere zoekresultaat wordt geopend met informatie die alleen voor die computer gefilterd.

    ![Azure Monitor-logboeken zoeken][5]

## <a name="reboot-after-system-updates"></a>Opnieuw opstarten na systeemupdates
1. Ga terug naar de **aanbevelingen** blade. Een nieuwe vermelding is gegenereerd nadat u systeemupdates, met de naam toegepast **opnieuw opstarten na systeemupdates**. Deze vermelding laat u weten dat u de virtuele machine moet voor het voltooien van het proces van het toepassen van systeemupdates van het opnieuw opstarten.

   ![Opnieuw opstarten na systeemupdates][6]
2. Selecteer **opnieuw opstarten na systeemupdates**. Hiermee opent u **opnieuw opstarten is in behandeling voor het voltooien van systeemupdates** blade weergegeven met een lijst van virtuele machines die u nodig hebt om opnieuw te starten voor het voltooien van het systeem toepassen proces werkt.

   ![Opnieuw opstarten is in behandeling][7]

Start opnieuw op de virtuele machine van Azure om het proces te voltooien.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](tutorial-security-policy.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen helpen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/) : Lees blogberichten over de Azure-beveiliging en naleving.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/missing-system-updates.png
[2]:./media/security-center-apply-system-updates/apply-system-updates.png
[3]: ./media/security-center-apply-system-updates/detail-on-missing-update.png
[4]: ./media/security-center-apply-system-updates/log-search.png
[5]: ./media/security-center-apply-system-updates/search-details.png
[6]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[7]: ./media/security-center-apply-system-updates/restart-pending.png
