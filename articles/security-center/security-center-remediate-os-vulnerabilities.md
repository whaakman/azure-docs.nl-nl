---
title: Herstellen van beveiligingsconfiguraties in Azure Security Center | Microsoft Docs
description: Dit document ziet u hoe de aanbeveling van Azure Security Center, 'Herstellen beveiligingsconfiguraties.' implementeren
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
ms.openlocfilehash: 477973298d8cc9d99da78e36274933e0bb737c4f
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="remediate-security-configurations-in-azure-security-center"></a>Herstellen van beveiligingsconfiguraties in Azure Security Center
Azure Security Center analyseert dagelijks het besturingssysteem (OS) van uw virtuele machines (VM's) en de computers voor een configuratie waardoor de virtuele machines en computers kwetsbaar voor aanvallen. Security Center raadt aan dat u beveiligingsproblemen oplossen wanneer de configuratie van uw besturingssysteem komt niet overeen met de aanbevolen configuratie van de beveiligingsregels en configuratiewijzigingen voor deze beveiligingsproblemen te verhelpen.

Zie voor meer informatie over de specifieke configuraties die worden bewaakt, de [lijst met regels van de aanbevolen configuratie](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Zie voor meer informatie over het aanpassen van beoordelingen van configuratie van de veiligheid, [beveiligingsconfiguraties OS aanpassen in Azure Security Center (Preview)](security-center-customize-os-security-config.md).

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
'Herstellen beveiligingsconfiguraties' wordt weergegeven als een aanbeveling in Security Center. De aanbeveling wordt weergegeven onder **aanbevelingen** > **Compute**.

In dit voorbeeld bevat informatie over de aanbeveling 'Beveiligingsconfiguraties herstellen' onder **Compute**.
1. In Security Center in het linkerdeelvenster Selecteer **Compute**.  
  De **Compute** venster wordt geopend.

   ![Beveiligingsconfiguraties herstellen][1]

2. Selecteer **herstellen beveiligingsconfiguraties**.  
  De **beveiligingsconfiguraties** venster wordt geopend.

   ![Het venster 'Beveiligingsconfiguraties'][2]

  Het bovenste gedeelte van het dashboard wordt weergegeven:

  - **Kan geen regels door ernst**: het totale aantal regels dat de configuratie van het besturingssysteem is mislukt voor de virtuele machines als de computers, onderverdeeld op ernst.
  - **Kan geen regels per type**: het totale aantal regels dat de configuratie van het besturingssysteem is mislukt voor de virtuele machines als de computers, onderverdeeld op type.
  - **Kan geen regels voor Windows**: het totale aantal regels is mislukt door de configuraties van uw Windows-besturingssysteem.
  - **Kan de Linux-regels niet**: het totale aantal regels is mislukt door de configuraties voor de Linux-besturingssysteem.

  Het onderste gedeelte van het dashboard geeft een lijst van alle mislukte regels voor uw virtuele machines en computers en de ernst van de ontbrekende update. De lijst bevat de volgende elementen:

  - **CCEID**: de CCE unieke id voor de regel. Security Center gebruikt Common Configuration Enumeration (CCE) om de unieke id's toewijzen aan configuratieregels.
  - **Naam**: de naam van de regel is mislukt.
  - **Regeltype**: de *registersleutel*, *beveiligingsbeleid*, of *controlebeleid* regeltype.
  - **Nee. van virtuele machines & computers**: het totale aantal virtuele machines en computers die de mislukte regel van toepassing op.
  - **Ernst regel**: waarde van de CCE *Kritiek*, *belangrijk*, of *waarschuwing*.
  - **Status**: de huidige status van de aanbeveling:

    - **Open**: de aanbeveling is nog niet opgelost.
    - **Bezig**: de aanbeveling wordt momenteel toegepast op de bronnen en door u is geen actie vereist.
    - **Opgelost**: de aanbeveling is toegepast. Als het probleem is opgelost, wordt de vermelding lichter gekleurd weergegeven.

3. Om de details van een mislukte regel weergeven, selecteert u deze in de lijst.

   ![Gedetailleerde weergave van een mislukte configuratieregel][3]

   De gedetailleerde weergave bevat de volgende informatie:

   - **Naam**: de naam van de regel.
   - **CCIED**: de CCE unieke id voor de regel.
   - **De versie van besturingssysteem**: de OS-versie van de virtuele machine of de computer.
   - **Ernst regel**: waarde van de CCE *Kritiek*, *belangrijk*, of *waarschuwing*.
   - **Volledige beschrijving**: de beschrijving van de regel.
   - **Beveiligingslek**: de uitleg van beveiligingslek of het risico als de regel wordt niet toegepast.
   - **Potentiële impact**: de zakelijke impact wanneer de regel wordt toegepast.
   - **Kunt dit tegengaan**: de herstelstappen uit.
   - **Verwachte waarde**: de waarde die wordt verwacht wanneer Security Center uw configuratie van het besturingssysteem van de virtuele machine op basis van de regel analyseert.
   - **Werkelijke waarde**: de waarde die wordt geretourneerd na een analyse van uw configuratie van het besturingssysteem van de virtuele machine op basis van de regel.
   - **Regel voor bewerking**: de regel-bewerking die door Security Center wordt gebruikt tijdens de analyse van uw configuratie van het besturingssysteem van de virtuele machine op basis van de regel.

4. Selecteer boven aan het venster gedetailleerde weergave **Search**.  
  Zoeken wordt een lijst met werkruimten die virtuele machines en computers met de geselecteerde beveiligingsgroep configuraties mismatch hebben geopend. De selectie van de werkruimte wordt alleen weergegeven als de geselecteerde regel van toepassing op meerdere virtuele machines die zijn verbonden met verschillende werkruimten.

   ![Vermelde werkruimten][4]

5. Selecteer een werkruimte.  
  Een Log Analytics-zoekquery wordt geopend gefilterde naar de werkruimte met het volgende niet overeenkomt configuraties van beveiliging.

   ![Werkruimte met OS-beveiligingsproblemen][5]

6. Selecteer een computer in de lijst.  
  Een nieuwe zoekresultaat geopend met informatie die zijn gefilterd alleen voor die computer.

   ![Gedetailleerde informatie over de geselecteerde computer][6]

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u implementeert de aanbeveling Security Center 'Herstellen beveiligingsconfiguraties'. Zie voor meer informatie over het aanpassen van beoordelingen van configuratie van de veiligheid, [beveiligingsconfiguraties OS aanpassen in Azure Security Center (Preview)](security-center-customize-os-security-config.md).

De specifieke configuraties die worden bewaakt, Zie [lijst met regels van de aanbevolen configuratie](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Security Center gebruikt Common Configuration Enumeration (CCE) om de unieke id's toewijzen aan configuratieregels. Ga voor meer informatie naar de [CCE](https://nvd.nist.gov/cce/index.cfm) site.

Zie de volgende bronnen voor meer informatie over Security Center:

* Zie voor een lijst met ondersteunde Windows- en Linux-machines, [ondersteunde platforms in Azure Security Center](security-center-os-coverage.md). 
* Zie voor meer informatie over het configureren van beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen, [beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md). 
* Zie voor meer informatie over hoe aanbevelingen u helpen bij het beveiligen van uw Azure-resources, [aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md). 
* Zie voor meer informatie over de status van uw Azure-resources, [beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md). 
* Zie voor informatie over het beheren van en reageren op beveiligingswaarschuwingen, [beheren en erop reageren beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md).
* Zie voor meer informatie over het bewaken van de status van uw partneroplossingen, [partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md).
* Zie voor antwoorden op veelgestelde vragen over het gebruik van de service [Veelgestelde vragen over Azure Security Center](security-center-faq.md).
* Zie voor over de Azure-beveiliging en naleving blogberichten, [Azure Security-blog](http://blogs.msdn.com/b/azuresecurity/).

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/compute-blade.png
[2]:./media/security-center-remediate-os-vulnerabilities/os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
[4]: ./media/security-center-remediate-os-vulnerabilities/search.png
[5]: ./media/security-center-remediate-os-vulnerabilities/log-search.png
[6]: ./media/security-center-remediate-os-vulnerabilities/search-results.png
