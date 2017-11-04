---
title: Beheren van endpoint protection-problemen met Azure Security Center | Microsoft Docs
description: Informatie over het beheren van endpoint protection-problemen in Azure Security Center.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 1599ad5f-d810-421d-aafc-892e831b403f
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/23/2017
ms.author: terrylan
ms.openlocfilehash: abbcb0a8e0206d78ca94520dfa81ab92506c47af
ms.sourcegitcommit: 4d90200f49cc60d63015bada2f3fc4445b34d4cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="manage-endpoint-protection-issues-with-azure-security-center"></a>Endpoint protection-problemen met Azure Security Center beheren
Azure Security Center bewaakt de status van de antimalware-bescherming en rapporteert deze onder de blade Endpoint protection-problemen. Security Center licht kwesties, zoals gedetecteerde bedreigingen en onvoldoende beveiliging, waardoor uw virtuele machines (VM's) en computers kwetsbaar voor antimalware bedreigingen. Met behulp van de gegevens in onder **Endpoint protection-problemen**, kunt u een plan voor het oplossen van alle vastgestelde problemen identificeren.

Security Center rapporten de volgende endpoint protection-problemen:

- Endpoint protection niet geïnstalleerd op Azure Virtual machines – een ondersteunde antimalwareoplossing is niet geïnstalleerd op deze Azure Virtual machines.
- Endpoint protection niet geïnstalleerd op computers met niet-Azure – een ondersteunde antimalware is niet geïnstalleerd op deze computers niet Azure.
- Endpoint protection-status:

   - Handtekening is verouderd: een anti-malwareoplossing op deze virtuele machines en computers is geïnstalleerd, maar de oplossing heeft niet de nieuwste definitiebestanden voor antimalware.
   - Geen realtime-beveiliging – een anti-malwareoplossing op deze virtuele machines en computers is geïnstalleerd, maar deze is niet geconfigureerd voor realtime-beveiliging.   De service kan worden uitgeschakeld of Security Center mogelijk niet de status ophalen omdat de oplossing wordt niet ondersteund. Zie [integratie partner](security-center-partner-integration.md) voor een lijst met ondersteunde oplossingen.
   - Niet rapporteren: een antimalwareoplossing is geïnstalleerd maar niet de rapportagegegevens.
   - Onbekend: een antimalwareoplossing is geïnstalleerd, maar de status is onbekend of het melden van een onbekende fout.

   > [!NOTE]
   > Zie [integreren beveiligingsoplossingen](security-center-partner-integration.md#integrated-azure-security-solutions) voor een lijst met endpoint protection-beveiligingsoplossingen is geïntegreerd met Security Center.
   >
   >

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
Endpoint protection-problemen wordt weergegeven als een aanbeveling in Security Center.  Als uw omgeving kwetsbaar voor antimalware bedreigingen is, deze aanbeveling wordt weergegeven onder **aanbevelingen** en klikt u onder **Compute**. Om te zien de **Endpoint protection uitgeeft dashboard**, u moet volgen de Compute-werkstroom.

In dit voorbeeld gebruiken we **Compute**.  Kijken we anti-malware op Azure VM's en op niet-Azure-computers te installeren.

## <a name="install-antimalware-on-azure-vms"></a>Installeren van anti-malware op Azure Virtual machines

1. Selecteer **Compute** onder het hoofdmenu Security Center of **overzicht**.

   ![Compute selecteren][1]

2. Onder **Compute**, selecteer **Endpoint protection-problemen**. De **Endpoint protection-problemen** dashboard wordt geopend.

   ![Selecteer de Endpoint protection-problemen][2]

   De bovenkant van het dashboard biedt:

   - Endpoint protection providers - lijsten met de verschillende providers geïdentificeerd door Security Center wordt geïnstalleerd.
   - Status van de geïnstalleerde endpoint protection - toont de status van virtuele machines en computers die een oplossing voor eindpuntbeveiliging geïnstalleerd hebben. Het diagram toont het aantal virtuele machines en computers die een status en het nummer met onvoldoende beveiliging.
   - Malware gedetecteerd: geeft het aantal virtuele machines en computers waar het melden van Security Center malware gedetecteerd.
   - Aangevallen computers – toont het aantal VM's en de computers waar Security Center aanvallen door malware wordt gerapporteerd.

   Aan de onderkant van het dashboard is er een lijst van het eindpunt beveiligingsproblemen, waaronder de volgende informatie:  

   - **Totaal aantal** -het aantal virtuele machines en computers worden beïnvloed door het probleem.
   - Een balk samenvoegen van het aantal virtuele machines en computers die worden beïnvloed door het probleem. De kleuren in de balk identificeren prioriteit:

      - Rood - hoge prioriteit en moet onmiddellijk worden opgelost
      - Oranje - gemiddelde prioriteit en moet zo snel mogelijk worden opgelost

3. Selecteer **Endpoint protection niet geïnstalleerd op Azure Virtual machines**.

   ![Endpoint protection niet geïnstalleerd op Azure Virtual machines selecteren][3]

4. Onder **Endpoint protection niet geïnstalleerd op Azure Virtual machines** is een lijst met virtuele Azure-machines waarvoor geen antimalware geïnstalleerd.  U kunt antimalware installeren op alle virtuele machines in de lijst of selecteren van afzonderlijke virtuele machines voor het installeren van anti-malware op door te klikken op de specifieke virtuele machine.
5. Onder **Selecteer Endpoint protection**, selecteert u de endpoint protection-oplossing die u wilt gebruiken. In dit voorbeeld selecteren **Microsoft Antimalware**.
6. Meer informatie over de oplossing voor eindpuntbeveiliging wordt weergegeven. Selecteer **Maken**.

## <a name="install-antimalware-on-non-azure-computers"></a>Antimalware installeren op computers met niet-Azure

1. Ga terug naar **Endpoint protection-problemen** en selecteer **Endpoint protection niet geïnstalleerd op computers met niet-Azure**.

   ![Selecteer de Endpoint protection niet geïnstalleerd op computers met niet-Azure][4]

2. Onder **Endpoint protection niet geïnstalleerd op computers met niet-Azure**, selecteer een werkruimte. Een Log Analytics zoekquery gefilterd op de werkruimte wordt geopend en een lijst met computers met ontbrekende antimalware. Selecteer een computer uit de lijst voor meer informatie.

   ![Log Analytics zoeken][5]

Een andere zoekresultaat geopend met informatie die zijn gefilterd alleen voor die computer.

  ![Log Analytics zoeken][6]

> [!NOTE]
> We raden aan dat de endpoint protection worden ingericht voor alle virtuele machines en computers om te identificeren en virussen, spyware en andere schadelijke software verwijderen.
>
>

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u implementeert de aanbeveling Security Center "Endpoint Protection installeren." Zie voor meer informatie over het inschakelen van Microsoft Antimalware in Azure, het volgende document:

* [Microsoft Antimalware voor Cloudservices en virtuele Machines](../security/azure-security-antimalware.md) --informatie over het implementeren van Microsoft Antimalware.

Zie de volgende documenten voor meer informatie over Security Center:

* [Beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md) --informatie over het configureren van beveiligingsbeleid.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) --Lees blogberichten over Azure-beveiliging en naleving.

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/compute.png
[2]:./media/security-center-install-endpoint-protection/endpoint-protection-issues.png
[3]:./media/security-center-install-endpoint-protection/install-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/endpoint-protection-issues-computers.png
[5]:./media/security-center-install-endpoint-protection/log-search.png
[6]:./media/security-center-install-endpoint-protection/info-filtered-to-computer.png
