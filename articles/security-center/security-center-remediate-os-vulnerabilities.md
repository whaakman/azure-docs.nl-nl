---
title: Herstellen van beveiligingsconfiguraties in Azure Security Center | Microsoft Docs
description: Dit document ziet u hoe u de aanbeveling Azure Security Center implementeert **herstellen beveiligingsconfiguraties**.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 991d41f5-1d17-468d-a66d-83ec1308ab79
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2018
ms.author: terrylan
ms.openlocfilehash: 412234b1486fa15cbc399bcf43be8ce90aac252a
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="remediate-security-configurations-in-azure-security-center"></a>Herstellen van beveiligingsconfiguraties in Azure Security Center
Azure Security Center analyseert dagelijks het besturingssysteem (OS) van uw virtuele machines (VM's) en de computers voor een configuratie waardoor de virtuele machines en computers kwetsbaar voor aanvallen. Security Center raadt aan dat u beveiligingsproblemen oplossen wanneer de configuratie van uw besturingssysteem komt niet overeen met de aanbevolen configuratie van de beveiligingsregels en raadt aan om de configuratiewijzigingen voor deze beveiligingsproblemen te verhelpen.

Zie voor meer informatie over de specifieke configuraties die worden bewaakt de [lijst met regels van de aanbevolen configuratie](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Zie [OS beveiligingsconfiguraties aanpassen](security-center-customize-os-security-config.md) voor informatie over het aanpassen van beoordelingen van configuratie van de veiligheid in Security Center.

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
Beveiliging configuraties verschil wordt weergegeven als een aanbeveling in Security Center oplossen. Deze aanbeveling wordt weergegeven onder **aanbevelingen** en klikt u onder **Compute**.

In dit voorbeeld kijken we de **herstellen beveiligingsconfiguraties** aanbeveling onder **Compute**.
1. Selecteer **Compute** onder het hoofdmenu Security Center.

   ![Beveiligingsconfiguraties herstellen][1]

2. Onder **Compute**, selecteer **herstellen beveiligingsconfiguraties**. **Beveiligingsconfiguraties** wordt geopend.

   ![Beveiligingsconfiguraties][2]

  De bovenkant van het dashboard biedt:

  - Het totale aantal regels op volgorde van prioriteit die de configuratie van het besturingssysteem is mislukt voor de van de virtuele machine als een van de computer.
  - Het totale aantal regels per type dat de configuratie van het besturingssysteem is mislukt voor de van de virtuele machine als een van de computer.
  - Het totale aantal regels is mislukt door de configuraties van uw Windows-besturingssysteem en de configuraties voor de Linux-besturingssysteem.

  De onderkant van het dashboard geeft een lijst van alle mislukte regels voor uw virtuele machines en computers en de ernst van de ontbrekende update. De lijst bevat:

  - **CCEID**: CCE unieke id voor de regel. Security Center gebruikt Common Configuration Enumeration (CCE) om de unieke id's voor configuratieregels toewijzen.
  - **NAAM**: naam van de regel is mislukt
  - **REGELTYPE**: registersleutel, beveiligingsbeleid of controlebeleid
  - **AANTAL VAN virtuele machines & COMPUTERS**: totale aantal virtuele machines en computers die worden uitgesloten van het mislukken van toepassing op
  - **ERNST van de regel**: CCE waarde voor ernst van kritieke, belangrijk of waarschuwingsstatus
  - **STATUS**: de huidige status van de aanbeveling:

    - **Open**: de aanbeveling is nog niet opgelost
    - **Bezig**: de aanbeveling wordt momenteel toegepast op deze resources en de door u is geen actie vereist.
    - **Opgelost**: de aanbeveling is al voltooid. (Als het probleem is opgelost, de vermelding is grijs)

3. Selecteer een mislukte regel in de lijst om details te bekijken.

   ![Van configuratieregels die zijn mislukt][3]

  De volgende informatie wordt in deze blade verstrekt:

  - --Naam van regel
  - CCIED--CCE unieke id voor de regel
  - De versie van besturingssysteem: versie van het besturingssysteem van de virtuele machine of de computer
  - ERNST van de regel--Waarde voor CCE ernst van kritieke, belangrijk of waarschuwingsstatus
  - VOLLEDIGE beschrijving--Beschrijving van regel
  - BEVEILIGINGSLEK--Uitleg van beveiligingslek of het risico als de regel wordt niet toegepast
  - POTENTIËLE IMPACT--Zakelijke impact als regel wordt toegepast
  - Kunt dit TEGENGAAN – herstelstappen
  - VERWACHTE waarde--Waarde verwacht wanneer Security Center uw configuratie van het besturingssysteem van de virtuele machine op basis van de regel analyseert
  - WERKELIJKE waarde--Waarde geretourneerd na analyse van uw besturingssysteem voor VM-configuratie op basis van de regel
  - --REGEL regel-bewerking door Security Center gebruikt tijdens de analyse van de configuratie van het besturingssysteem van virtuele machine op basis van de regel

4. Selecteer de **Search** pictogram in het bovenste lint. Zoeken wordt geopend in de lijst werkruimten die virtuele machines en computers waarop het geselecteerde beveiligingsbereik configuraties volgende niet overeenkomt. Deze werkruimte selectie blade wordt alleen weergegeven als de geselecteerde regel van toepassing op meerdere virtuele machines die zijn verbonden met verschillende werkruimten.

  ![Vermelde werkruimten][4]

5. Selecteer een werkruimte. Een Log Analytics-zoekquery wordt geopend gefilterde naar de werkruimte met het volgende niet overeenkomt configuraties van beveiliging.

  ![Werkruimte met OS-beveiligingsproblemen][5]

6. Selecteer een computer uit de lijst voor meer informatie. Een andere zoekresultaat geopend met informatie die zijn gefilterd alleen voor die computer.

  ![Gefilterd voor die computer][6]

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u implementeert de aanbeveling Security Center 'Herstellen beveiligingsconfiguraties'. Zie [OS beveiligingsconfiguraties aanpassen](security-center-customize-os-security-config.md) voor informatie over het aanpassen van beoordelingen van configuratie van de veiligheid in Security Center.

U kunt de reeks configuratieregels bekijken [hier](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Security Center gebruikt CCE (algemene configuratie opsomming) om de unieke id's voor configuratieregels toewijzen. Ga naar de [CCE](https://nvd.nist.gov/cce/index.cfm) website voor meer informatie.

Zie de volgende bronnen voor meer informatie over Security Center:

* [Ondersteunde platforms in Azure Security Center](security-center-os-coverage.md) -geeft een lijst van ondersteunde Windows- en Linux-machines.
* [Beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md) -informatie over het beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen configureren.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) -Leer hoe aanbevelingen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) -informatie over het bewaken van de status van uw Azure-resources.
* [Het beheren van en reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) -informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md) -informatie over het bewaken van de status van uw partneroplossingen.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md) -Raadpleeg Veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) -Lees blogberichten over Azure-beveiliging en naleving.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/compute-blade.png
[2]:./media/security-center-remediate-os-vulnerabilities/os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
[4]: ./media/security-center-remediate-os-vulnerabilities/search.png
[5]: ./media/security-center-remediate-os-vulnerabilities/log-search.png
[6]: ./media/security-center-remediate-os-vulnerabilities/search-results.png
