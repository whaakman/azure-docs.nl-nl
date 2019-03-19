---
title: Beveiligingsconfiguraties in Azure Security Center herstellen | Microsoft Docs
description: Dit document wordt beschreven hoe u voor het implementeren van de aanbeveling van Azure Security Center, "Herstellen beveiligingsconfiguraties."
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 991d41f5-1d17-468d-a66d-83ec1308ab79
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: f4558c6fdb1e5e4f0ffb7a4b4fdb1ab62eb4cfa9
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58121278"
---
# <a name="remediate-security-configurations-in-azure-security-center"></a>Beveiligingsconfiguraties in Azure Security Center herstellen
Azure Security Center analyseert dagelijks het besturingssysteem (OS) van uw virtuele machines (VM's) en de computers voor een configuratie die kan de virtuele machines en computers die meer kwetsbaar voor aanvallen. Security Center raadt aan dat u beveiligingsproblemen oplossen wanneer de configuratie van het besturingssysteem komt niet overeen met de aanbevolen configuratie van beveiligingsregels en er configuratiewijzigingen aanbevolen om deze beveiligingsproblemen te verhelpen.

Zie voor meer informatie over de specifieke configuraties die worden bewaakt, de [lijst met aanbevolen configuratieregels](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Zie voor meer informatie over het aanpassen van de configuratie worden uitgevoerd, [beveiligingsconfiguraties OS aanpassen in Azure Security Center (Preview)](security-center-customize-os-security-config.md).

## <a name="implement-the-recommendation"></a>De aanbeveling voor het implementeren
'Beveiligingsconfiguraties herstellen' wordt weergegeven als een aanbeveling in Security Center. De aanbeveling wordt weergegeven onder **aanbevelingen** > **Compute en apps**.

In dit voorbeeld bevat informatie over de aanbeveling 'Beveiligingsconfiguraties herstellen' onder **Compute en apps**.
1. In Security Center in het linkerdeelvenster selecteert **Compute en apps**.  
   De **Compute en apps** venster wordt geopend.

   ![Beveiligingsconfiguraties herstellen][1]

2. Selecteer **beveiligingsconfiguraties herstellen**.  
   De **beveiligingsconfiguraties** venster wordt geopend.

   ![Het venster "Beveiligingsconfiguraties"][2]

   Het bovenste gedeelte van het dashboard wordt weergegeven:

   - **Mislukte regels op ernst**: Het totale aantal regels dat de configuratie van het besturingssysteem in uw virtuele machines en computers, onderverdeeld op basis van ernst is mislukt.
   - **Mislukte regels per type**: Het totale aantal regels dat de configuratie van het besturingssysteem in uw virtuele machines en computers, opgedeeld per type is mislukt.
   - **Mislukte regels Windows**: Het totale aantal regels is mislukt door de configuraties van uw Windows-besturingssysteem.
   - **Mislukte regels Linux**: Het totale aantal regels is mislukt door de configuraties van uw Linux-besturingssysteem.

   Het onderste gedeelte van het dashboard geeft een lijst van alle mislukte regels voor uw virtuele machines en computers en de ernst van de ontbrekende update. De lijst bevat de volgende elementen:

   - **CCEID**: De CCE unieke id voor de regel. Unieke id's toewijzen aan configuratieregels gebruikt Security Center Common Configuration Enumeration (CCE).
   - **Naam**: De naam van de regel die is mislukt.
   - **Regeltype**: De *registersleutel*, *beveiligingsbeleid*, *controlebeleid*, of *IIS* regeltype.
   - **Nee. VM's en computers**: Het totale aantal virtuele machines en computers die de mislukte regel van toepassing op.
   - **Ernst van regel**: De waarde CCE *kritieke*, *belangrijk*, of *waarschuwing*.
   - **status**: De huidige status van de aanbeveling:

     - **Open**: De aanbeveling is nog niet opgelost.
     - **Bezig**: De aanbeveling wordt momenteel toegepast op de resources en er is geen actie te ondernemen.
     - **Opgelost**: De aanbeveling is toegepast. Als het probleem is opgelost, wordt de vermelding lichter gekleurd weergegeven.

3. Als u wilt weergeven van de details van een regel die is mislukt, selecteert u deze in de lijst.

   ![Gedetailleerde weergave van een mislukte configuratieregel][3]

   De gedetailleerde weergave bevat de volgende informatie:

   - **Naam**: De naam van de regel.
   - **CCIED**: De CCE unieke id voor de regel.
   - **Versie van het besturingssysteem**: De versie van het besturingssysteem van de virtuele machine of computer.
   - **Ernst van regel**: De waarde CCE *kritieke*, *belangrijk*, of *waarschuwing*.
   - **Volledige beschrijving**: De beschrijving van de regel.
   - **Beveiligingslek met betrekking tot**: De uitleg van beveiligingsproblemen of het risico als de regel wordt niet toegepast.
   - **Mogelijke impact**: De impact op bedrijf wanneer de regel wordt toegepast.
   - **Tegenmaatregel**: De herstelstappen.
   - **Verwachte waarde**: De waarde die wordt verwacht wanneer Security Center de configuratie van het besturingssysteem van de virtuele machine op basis van de regel analyseert.
   - **Werkelijke waarde**: De waarde die wordt geretourneerd na een analyse van uw configuratie van het besturingssysteem van de virtuele machine op basis van de regel.
   - **Regel bewerking**: De regelbewerking die door Security Center wordt gebruikt tijdens de analyse van uw configuratie van het besturingssysteem van de virtuele machine op basis van de regel.

4. Selecteer aan de bovenkant van het venster van de gedetailleerde weergave, **zoeken**.  
   Zoeken wordt een lijst met werkruimten die virtuele machines en computers met de geselecteerde beveiligingsgroep configuraties mismatch hebt geopend. De werkruimteselectie wordt alleen weergegeven als de geselecteerde regel van toepassing op meerdere virtuele machines die zijn verbonden met verschillende werkruimten.

   ![Vermelde werkruimten][4]

5. Selecteer een werkruimte.  
   Een zoekquery voor logboeken van Azure Monitor wordt gefilterd op de werkruimte met de beveiliging configuraties mismatch geopend.

   ![Werkruimte met besturingssysteem-beveiligingsproblemen][5]

6. Selecteer een computer in de lijst.  
   Een nieuwe zoekresultaat wordt geopend met informatie die alleen voor die computer gefilterd.

   ![Gedetailleerde informatie over de geselecteerde computer][6]

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u voor het implementeren van de aanbeveling van Security Center "Herstellen beveiligingsconfiguraties." Zie voor meer informatie over het aanpassen van de configuratie worden uitgevoerd, [beveiligingsconfiguraties OS aanpassen in Azure Security Center (Preview)](security-center-customize-os-security-config.md).

De specifieke configuraties die worden bewaakt, Zie [lijst met aanbevolen configuratieregels](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Unieke id's toewijzen aan configuratieregels gebruikt Security Center Common Configuration Enumeration (CCE). Voor meer informatie gaat u naar de [CCE](https://nvd.nist.gov/cce/index.cfm) site.

Zie voor meer informatie over Security Center, de volgende bronnen:

* Zie voor een lijst met ondersteunde Windows en Linux-VM's, [ondersteunde platforms in Azure Security Center](security-center-os-coverage.md).
* Zie voor informatie over het configureren van beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen, [beveiligingsbeleid instellen in Azure Security Center](tutorial-security-policy.md).
* Zie voor meer informatie hoe aanbevelingen u helpen bij uw Azure-resources beveiligen, [aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md).
* Zie voor meer informatie over het bewaken van de status van uw Azure-resources, [beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md).
* Zie voor meer informatie over het beheren en reageren op beveiligingswaarschuwingen, [beheren en erop reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md).
* Zie voor meer informatie over het bewaken van de status van uw partneroplossingen, [partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md).
* Zie voor antwoorden op veelgestelde vragen over het gebruik van de service [Veelgestelde vragen over Azure Security Center](security-center-faq.md).
* Voor over de Azure-beveiliging en naleving blogberichten, Zie [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/).

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/compute-blade.png
[2]:./media/security-center-remediate-os-vulnerabilities/os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
[4]: ./media/security-center-remediate-os-vulnerabilities/search.png
[5]: ./media/security-center-remediate-os-vulnerabilities/log-search.png
[6]: ./media/security-center-remediate-os-vulnerabilities/search-results.png
