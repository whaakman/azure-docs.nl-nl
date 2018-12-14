---
title: Beheren van endpoint protection-problemen met Azure Security Center | Microsoft Docs
description: Informatie over het beheren van endpoint protection-problemen in Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 1599ad5f-d810-421d-aafc-892e831b403f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 1e1401d7f7f383f60b37a3af3a18569bd5a2520a
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53336915"
---
# <a name="manage-endpoint-protection-issues-with-azure-security-center"></a>Problemen met endpoint protection met Azure Security Center beheren
Azure Security Center bewaakt de status van bescherming tegen malware en rapporteert deze op de blade problemen van Endpoint protection. Security Center worden de kwesties, zoals de gedetecteerde bedreigingen en onvoldoende beveiliging, waardoor uw virtuele machines (VM's) en de computers kwetsbaar voor antimalwarebedreigingen gemarkeerd. Met behulp van de gegevens in onder **problemen met Endpoint protection**, kunt u een plan om alle vastgestelde problemen identificeren.

Security Center de volgende problemen met endpoint protection-rapporten:

- Endpoint protection niet geïnstalleerd op Azure VM's – een ondersteunde antimalwareoplossing is niet geïnstalleerd op deze Azure-VM's.
- Endpoint protection niet geïnstalleerd op computers met niet-Azure – een ondersteunde antimalware is niet geïnstalleerd op deze computers niet-Azure.
- Endpoint protection-status:

   - De handtekening is verouderd: een antimalwareoplossing op deze virtuele machines en computers is geïnstalleerd, maar de oplossing beschikt niet over de nieuwste definitiebestanden voor antimalware.
   - Er is geen realtime-beveiliging: een antimalwareoplossing op deze virtuele machines en computers is geïnstalleerd, maar deze is niet geconfigureerd voor realtime-beveiliging.   De service kan worden uitgeschakeld of het is mogelijk dat Security Center kan niet de status ophalen omdat de oplossing wordt niet ondersteund. Zie [partnerintegratie](security-center-partner-integration.md) voor een lijst van ondersteunde oplossingen.
   - Niet rapporteren: een malwareoplossing voor anti-is geïnstalleerd maar niet de rapportagegegevens.
   - Onbekend: een malwareoplossing voor anti-is geïnstalleerd, maar de status is onbekend of het melden van een onbekende fout.

   > [!NOTE]
   > Zie [beveiligingsoplossingen integreren](security-center-partner-integration.md#integrated-azure-security-solutions) voor een lijst met endpoint protection-beveiligingsoplossingen is geïntegreerd met Security Center.
   >
   >

## <a name="implement-the-recommendation"></a>De aanbeveling voor het implementeren
Problemen met endpoint protection wordt weergegeven als een aanbeveling in Security Center.  Als uw omgeving kwetsbaar voor anti-malware-aanvallen is, deze aanbeveling wordt weergegeven onder **aanbevelingen** en klikt u onder **Compute**. Om te zien de **dashboard problemen met Endpoint protection**, u moet volgen van de Compute-werkstroom.

In dit voorbeeld gebruiken we **Compute**.  Er wordt beschreven hoe u anti-malware op Azure Virtual machines en op niet-Azure-computers installeren.

## <a name="install-antimalware-on-azure-vms"></a>Installeer anti-malware op Azure Virtual machines

1. Selecteer **Compute** onder het hoofdmenu van Security Center of **overzicht**.

   ![Selecteer berekenen][1]

2. Onder **Compute**, selecteer **problemen met Endpoint protection**. De **problemen met Endpoint protection** dashboard wordt geopend.

   ![Problemen met Endpoint protection selecteren][2]

   De bovenkant van het dashboard biedt:

   - Endpoint protection providers - een lijst met de andere providers geïdentificeerd door Security Center wordt geïnstalleerd.
   - Status van de geïnstalleerde endpoint protection - toont de status van virtuele machines en computers waarvoor een oplossing voor eindpuntbeveiliging is geïnstalleerd. De grafiek toont het aantal virtuele machines en computers die in orde is en het aantal met onvoldoende beveiliging.
   - Malware gedetecteerd: geeft het aantal virtuele machines en computers waar het melden van Security Center malware gedetecteerd.
   - Aangevallen computers – toont het aantal virtuele machines en computers waar Security Center aanvallen door malware rapporteert.

   Aan de onderkant van het dashboard is er een lijst van het eindpunt van beveiligingsproblemen, waaronder de volgende informatie:  

   - **Totaal aantal** -het aantal virtuele machines en computers worden beïnvloed door het probleem.
   - Een balk het samenvoegen van het aantal virtuele machines en computers die worden beïnvloed door het probleem. De kleuren in de balk identificeren prioriteit:

      - Rood - hoge prioriteit en moet onmiddellijk worden opgelost
      - Oranje: gemiddelde prioriteit en moet zo snel mogelijk worden opgelost

3. Selecteer **Endpoint protection niet geïnstalleerd op virtuele Azure-machines**.

   ![Endpoint protection niet geïnstalleerd op virtuele Azure-machines selecteren][3]

4. Onder **Endpoint protection niet geïnstalleerd op virtuele Azure-machines** is een lijst met Azure-VM's waarvoor geen antimalware geïnstalleerd.  U kunt antimalware installeren op alle virtuele machines in de lijst of Selecteer afzonderlijke virtuele machines voor het installeren van anti-malware op door te klikken op de specifieke virtuele machine.
5. Onder **Selecteer Endpoint protection**, selecteert u de endpoint protection-oplossing die u wilt gebruiken. In dit voorbeeld selecteren **Microsoft Antimalware**.
6. Meer informatie over de oplossing voor eindpuntbeveiliging wordt weergegeven. Selecteer **Maken**.

## <a name="install-antimalware-on-non-azure-computers"></a>Anti-malware op niet-Azure-computers installeren

1. Ga terug naar **problemen met Endpoint protection** en selecteer **Endpoint protection niet geïnstalleerd op computers met niet-Azure**.

   ![Endpoint protection niet geïnstalleerd op niet-Azure-computers selecteren][4]

2. Onder **Endpoint protection niet geïnstalleerd op computers met niet-Azure**, selecteer een werkruimte. Een Log Analytics-zoekquery gefilterd op de werkruimte wordt geopend en geeft een lijst van computers met ontbrekende anti-malware. Selecteer een computer in de lijst voor meer informatie.

   ![Analytics voor zoeken in Logboeken][5]

Een andere zoekresultaat wordt geopend met informatie die alleen voor die computer gefilterd.

  ![Analytics voor zoeken in Logboeken][6]

> [!NOTE]
> Het is raadzaam dat endpoint protection worden ingericht voor alle virtuele machines en computers op te sporen en virussen, spyware en andere schadelijke software verwijderen.
>
>

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u voor het implementeren van de aanbeveling van Security Center 'Installatie van Endpoint Protection'. Zie voor meer informatie over het inschakelen van Microsoft Antimalware in Azure, het volgende document:

* [Microsoft Antimalware voor Cloudservices en virtuele Machines](../security/azure-security-antimalware.md) --meer informatie over het implementeren van Microsoft Antimalware.

Zie de volgende documenten voor meer informatie over Security Center:

* [Beveiligingsbeleid instellen in Azure Security Center](tutorial-security-policy.md) --informatie over het configureren van beveiligingsbeleid.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen helpen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/) : Lees blogberichten over de Azure-beveiliging en naleving.

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/compute.png
[2]:./media/security-center-install-endpoint-protection/endpoint-protection-issues.png
[3]:./media/security-center-install-endpoint-protection/install-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/endpoint-protection-issues-computers.png
[5]:./media/security-center-install-endpoint-protection/log-search.png
[6]:./media/security-center-install-endpoint-protection/info-filtered-to-computer.png
