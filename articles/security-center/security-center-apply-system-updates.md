---
title: Toepassing systeemupdates in Azure Security Center | Microsoft Docs
description: Dit document ziet u hoe de Azure Security Center aanbevelingen implementeren ** toepassen system updates ** en ** na system updates ** opnieuw opgestart.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: e5bd7f55-38fd-4ebb-84ab-32bd60e9fa7a
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/11/2017
ms.author: terrylan
ms.openlocfilehash: 9f7924f3f0975dc32fdf5b8e1b89a1fb8e9b7d57
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="apply-system-updates-in-azure-security-center"></a>Systeemupdates in Azure Security Center toepassen
Azure Security Center bewaakt dagelijkse Windows en Linux virtuele machines (VM's) en computers voor besturingssysteemupdates ontbreken. Security Center haalt een lijst met beschikbare beveiligingsupdates en essentiële updates via Windows Update of Windows Server Update Services (WSUS), afhankelijk van welke service is geconfigureerd op een Windows-computer. Security Center controleert ook of de meest recente updates in de Linux-systemen. Als uw VM of de computer een systeemupdate ontbreken, raden Security Center aan systeemupdates toe te passen.

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
Systeem toepassen updates wordt weergegeven als een aanbeveling in Security Center. Als uw VM of de computer een systeemupdate ontbreken, deze aanbeveling wordt weergegeven onder **aanbevelingen** en klikt u onder **Compute**.  Hiermee opent u de aanbeveling selecteren de **toepassen systeemupdates** dashboard.

In dit voorbeeld gebruiken we **Compute**.

1. Selecteer **Compute** onder het hoofdmenu Security Center.

   ![Compute selecteren][1]

2. Onder **Compute**, selecteer **ontbrekende systeemupdates**. De **toepassen systeemupdates** dashboard wordt geopend.

   ![Dashboard voor systeem-updates toepassen][2]

   De bovenkant van het dashboard biedt:

    - Het totale aantal Windows- en Linux-machines en computers met ontbrekende systeemupdates.
    - Het totale aantal essentiële updates ontbreken in uw virtuele machines en computers.
    - Het totale aantal beveiligingsupdates ontbreken in uw virtuele machines en computers.

  De onderkant van het dashboard geeft een lijst van alle ontbrekende updates voor uw virtuele machines en computers en de ernst van de ontbrekende update.  De lijst bevat:

    - NAAM: Naam van de ontbrekende update.
    - NEE. VAN virtuele machines en COMPUTERS: totaal aantal virtuele machines en computers die deze update ontbreekt.
    - STATUS: De huidige status van de aanbeveling:

      - Open: De aanbeveling is nog niet opgelost.
      - In voortgang: De aanbeveling wordt momenteel toegepast op deze resources en door u is geen actie vereist.
      - Opgelost: De aanbeveling is al voltooid. (Als het probleem is opgelost, wordt de vermelding grijs).

    - ERNST: Wordt de ernst van deze bepaalde aanbeveling beschreven:

      - Hoog: Een beveiligingsprobleem bestaat voor een belangrijke resource (toepassing, virtuele machine of netwerkbeveiligingsgroep) en aandacht vereist.
      - Gemiddeld: Niet-kritieke of extra stappen zijn vereist om een proces te voltooien of een beveiligingsprobleem elimineren.
      - Laag: Een beveiligingsprobleem moet worden opgelost, maar er is geen onmiddellijke aandacht vereist. (Aanbevelingen met de ernstaanduiding Laag worden niet standaard weergegeven, maar u kunt hierop filteren als u deze aanbevelingen wilt bekijken.)

3. Selecteer een ontbrekende update in de lijst om details te bekijken.

   ![Ontbrekende beveiligingsupdate][3]

4. Selecteer de **Search** pictogram in het bovenste lint.  Een Log Analytics-zoekquery wordt geopend gefilterd voor de computers die de update ontbreekt.

   ![Log Analytics zoeken][4]

5. Selecteer een computer uit de lijst voor meer informatie. Een andere zoekresultaat geopend met informatie die zijn gefilterd alleen voor die computer.

    ![Log Analytics zoeken][5]

## <a name="reboot-after-system-updates"></a>Opnieuw opstarten na systeemupdates
1. Ga terug naar de **aanbevelingen** blade. Een nieuwe vermelding is gegenereerd nadat u systeemupdates, aangeroepen toegepast **opnieuw opgestart na systeemupdates**. Deze vermelding laat u weten dat u de virtuele machine moet voor het voltooien van het proces van het toepassen van systeemupdates opnieuw opstarten.

   ![Opnieuw opstarten na systeemupdates][6]
2. Selecteer **opnieuw opgestart na systeemupdates**. Hiermee opent u **opnieuw worden opgestart om te voltooien systeemupdates** blade met een lijst van virtuele machines die u nodig hebt om opnieuw te starten voor het voltooien van het systeem toepassen updates proces.

   ![Opnieuw opstarten in behandeling][7]

Start opnieuw op de virtuele machine van Azure om het proces te voltooien.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) --Lees blogberichten over Azure-beveiliging en naleving.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/missing-system-updates.png
[2]:./media/security-center-apply-system-updates/apply-system-updates.png
[3]: ./media/security-center-apply-system-updates/detail-on-missing-update.png
[4]: ./media/security-center-apply-system-updates/log-search.png
[5]: ./media/security-center-apply-system-updates/search-details.png
[6]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[7]: ./media/security-center-apply-system-updates/restart-pending.png
