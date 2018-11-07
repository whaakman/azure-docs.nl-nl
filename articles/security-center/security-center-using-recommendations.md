---
title: Gebruik Azure Security Center aanbevelingen om beveiliging te verbeteren | Microsoft Docs
description: " Informatie over het gebruik van beveiligingsbeleid en aanbevelingen in Azure Security Center om te beperken van een aanval op de beveiliging. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2017
ms.author: terrylan
ms.openlocfilehash: 978ef8087665edb3f0bfaa0bd0a55f4d8ab9a12d
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51251500"
---
# <a name="use-azure-security-center-recommendations-to-enhance-security"></a>Gebruik Azure Security Center aanbevelingen om beveiliging te verbeteren
U kunt de kans op een aanzienlijke beveiligingsgebeurtenis beperken door een beveiligingsbeleid configureren en vervolgens implementeren van de aanbevelingen die door Azure Security Center. Dit artikel ziet u hoe u beveiligingsbeleid en aanbevelingen in Security Center gebruikt om te beperken van een aanval op de beveiliging.

> [!NOTE]
> In dit artikel is gebaseerd op de functies en concepten die in het Security Center ter [planning- en bedieningsgids voor](security-center-planning-and-operations-guide.md). Het is een goed idee te bekijken van de handleiding voor capaciteitsplanning voordat u doorgaat.
>
>

## <a name="managing-security-recommendations"></a>Aanbevelingen voor beveiliging beheren
Een beveiligingsbeleid bepaalt welke set besturingselementen die worden aanbevolen voor resources binnen het opgegeven abonnement of resourcegroep. In Security Center definieert u beleid op basis van de beveiligingsvereisten van uw bedrijf. Zie voor meer informatie, [beveiligingsbeleid instellen in Security Center](security-center-policies.md).

Beveiligingsbeleid voor resourcegroepen worden overgenomen van het abonnementsniveau.

![Overname van Groepsbeleid voor beveiliging][1]

Als u aangepaste beleidsregels in bepaalde resourcegroepen nodig hebt, kunt u overname in de resourcegroep uitschakelen. Als u wilt uitschakelen, overname ingesteld op Unique op de blade beveiligingsbeleid en de besturingselementen die Security Center aanbevelingen voor het bevat aanpassen.

Bijvoorbeeld, als u workloads waarvoor niet de SQL Database transparante gegevensversleuteling (TDE)-beleid hebt, uitschakelen van het beleid op abonnementsniveau en schakelt u die alleen in de resourcegroepen waar SQL TDE vereist is.

> [!NOTE]
> Als er een conflict tussen het beleid op abonnementsniveau en het beleid op het niveau van de resourcegroep is, heeft het beleid op het niveau van de resourcegroep voorrang.
>
>

De beveiligingsstatus van uw Azure-resources wordt gecontroleerd met Security Center. Wanneer Security Center potentiële beveiligingsproblemen worden geïdentificeerd, worden er aanbevelingen gedaan op basis van de besturingselementen die zijn ingesteld in het beveiligingsbeleid. De aanbevelingen begeleiden u bij het proces van het configureren van de vereiste beveiligingscontroles.

Huidige beleid aanbevelingen in Security Center focus op systeemupdates, configuratie van het besturingssysteem, de netwerkbeveiligingsgroepen op subnetten en virtuele machines (VM's), SQL Database Auditing, SQL Database TDE en web application firewalls. Zie voor de meest recente dekking van de aanbevelingen van Security Center, [aanbevelingen voor beveiliging in Security Center beheren](security-center-recommendations.md).

## <a name="scenario"></a>Scenario
In dit scenario laat zien hoe u Security Center gebruiken om te helpen verminderen de kans op een aanzienlijke beveiligingsincident door Security Center aanbevelingen voor bewaking en actie te ondernemen. Het scenario wordt gebruikt voor het fictieve bedrijf, Contoso en rollen die zijn gepresenteerd in het Security Center [planning- en bedieningsgids voor](security-center-planning-and-operations-guide.md#security-roles-and-access-controls). De rollen vertegenwoordigen individuen en teams die van Security Center gebruikmaken mogelijk verschillende beveiliging gerelateerde taken uit te voeren. De rollen zijn:

![Scenario-rollen][2]

Contoso heeft enkele onlangs gemigreerd van de on-premises resources naar Azure. Contoso wil implementeren en onderhouden van beveiliging die Verminder de kwetsbaarheid voor een aanval op de beveiliging van de resources in de cloud.

## <a name="recommended-solution"></a>Aanbevolen oplossing
Een oplossing is het gebruik van Security Center om te voorkomen en detecteren van beveiligingsrisico's. Contoso heeft toegang tot Security Center via hun Azure-abonnement. De [gratis laag](security-center-pricing.md) van Security Center is automatisch ingeschakeld op alle Azure-abonnementen en het verzamelen van gegevens is ingeschakeld op alle virtuele machines in het abonnement.

David, in het Contoso IT-beveiliging, configureert u een **beveiligingsbeleid** met Security Center. Security Center analyseert de beveiligingsstatus van de Contoso Azure-resources. Wanneer Security Center potentiële beveiligingsproblemen worden geïdentificeerd, maakt het **aanbevelingen** op basis van de besturingselementen die zijn ingesteld in het beveiligingsbeleid.

Jeff, een eigenaar cloud-workload is verantwoordelijk voor het implementeren en onderhouden van beveiliging volgens het beveiligingsbeleid van Contoso. Jeff kan de aanbevelingen die zijn gemaakt door Security Center om toe te passen beveiligingen bewaken. Jeff begeleiden de aanbevelingen bij het configureren van de vereiste beveiligingscontroles.

Opdat Jeff te implementeren en onderhouden van beveiliging en oplossen van beveiligingsproblemen, wat hij nodig heeft voor:

- Beveiligingsaanbevelingen voor die door Security Center bewaken
- Aanbevelingen voor beveiliging evalueren en bepalen of hij moet toepassen of verwijderen
- Aanbevelingen voor beveiliging toepassen

Laten we stappen Jeff om te zien hoe hij aanbevelingen van Security Center gebruikt om hem te begeleiden bij het configureren van besturingselementen om te voorkomen van beveiligingsproblemen.

## <a name="how-to-implement-this-solution"></a>Deze oplossing implementeren
Jeff zich aanmeldt bij [Azure-portal](https://azure.microsoft.com/features/azure-portal/) en opent u de Security Center-console. Hij controleert als onderdeel van de dagelijkse bewakingsactiviteiten, om te zien of er aanbevelingen voor beveiliging door de volgende stappen uit:

1. Jeroen selecteert de **aanbevelingen** tegel om te openen **aanbevelingen**.
   ![Selecteer de tegel aanbevelingen][3]
2. Jeroen controleert de lijst met aanbevelingen. Hij ziet dat de lijst met aanbevelingen in volgorde van prioriteit, van de hoogste prioriteit naar laagste prioriteit is opgegeven door Security Center. Hij besluit om een hoge prioriteit aanbeveling in de lijst. Hij selecteert **Endpoint Protection installeren** onder **aanbevelingen**.
3. **Endpoint Protection installeren** wordt geopend met een lijst van virtuele machines zonder-antimalware zijn ingeschakeld. Jeff beoordelingen van de lijst met virtuele machines, selecteert u alle virtuele machines en selecteert vervolgens **installeren op 3 virtuele machines**.
   ![Eindpuntbeveiliging installeren][4]
4. **Endpoint Protection selecteren** wordt geopend Jeff voorzien van twee antimalware-oplossingen. Jeroen selecteert de **Microsoft Antimalware** oplossing.
5. Meer informatie over de antimalwareoplossing wordt weergegeven. Jeff selecteert **maken**.
   ![Microsoft antimalware][5]
6. Jeff voert de vereiste configuratie-instellingen onder **installeren** en selecteert u **OK**.

[Microsoft Antimalware](../security/azure-security-antimalware.md) is nu actief op de geselecteerde virtuele machines.

Jeff blijft verplaatsen door middel van de hoge prioriteit en gemiddelde prioriteit aanbevelingen, het nemen van besluiten over de uitvoering. Jeff verwijst naar de [aanbevelingen voor beveiliging beheren](security-center-recommendations.md) artikel voor informatie over de aanbevelingen en wat elke doet als hij van toepassing is.

Jeff leert die [Microsoft Security Response Center (MSRC)](../security/azure-security-response-center.md) voert selecteert u beveiligingsbewaking van de Azure-netwerk en de infrastructuur en threat intelligence en misbruik klachten ontvangt van een derde partij. Als Jeff biedt contactgegevens voor beveiliging voor Contoso Azure-abonnement, Microsoft contactpersonen Contoso als het MSRC dat het Contoso-klantgegevens detecteert zijn geopend door een illegale of niet-geautoriseerde partij. Laten we Jeff volgt als hij is van toepassing de **contactgegevens voor beveiliging verstrekken** aanbeveling (een aanbeveling met urgentie gemiddeld in de lijst met bovenstaande aanbevelingen).

1. Jeff selecteert **contactgegevens voor beveiliging verstrekken** onder **aanbevelingen**, wordt geopend **contactgegevens voor beveiliging verstrekken**.
2. Jeff selecteert voor de contactgegevens op de Azure-abonnement. Een tweede **contactgegevens voor beveiliging verstrekken** blade wordt geopend.
   ![Contactgegevens voor beveiliging][6]
3. Onder **contactgegevens voor beveiliging verstrekken**, krijgt de Jeff:

  - de beveiliging contact op met e-mailadressen gescheiden door komma's (Er is een limiet aan het aantal e-mailadressen die hij kunt opgeven)
  - Eén beveiligingsgroep Telefoonnummer contactpersoon

4. Jeff ook Hiermee schakelt u de optie **Stuur mij e-mails over waarschuwingen** voor het ontvangen van e-mailberichten over waarschuwingen met hoge urgentie.
5. Jeff selecteert **OK** om toe te passen van de contactgegevens voor beveiliging van de Contoso-abonnement.

Tot slot Jeff bekijkt de aanbeveling met lage prioriteit **beveiligingsproblemen van besturingssystemen herstellen** en bepaalt deze aanbeveling is niet van toepassing. Hij wil de aanbeveling negeren. Jeroen selecteert de drie punten die worden weergegeven aan de rechterkant, en selecteert vervolgens **sluiten**.
   ![De aanbeveling negeren][7]

## <a name="conclusion"></a>Conclusie
Bewaking van de aanbevelingen in Security Center kunt u beveiligingsproblemen te elimineren voordat een aanval plaatsvindt. U kunt een beveiligingsincident voorkomen door te implementeren en onderhouden van beveiliging aan het beveiligingsbeleid in Security Center.

## <a name="next-steps"></a>Volgende stappen
In dit scenario laat zien hoe u beveiligingsbeleid en aanbevelingen in Security Center gebruikt om te beperken van een aanval op de beveiliging. Zie de [respons op incidenten scenario](security-center-incident-response.md) voor meer informatie over hoe u met reacties op incidenten in plaats van plan bent voordat er een aanval plaatsvindt.

Zie voor meer informatie over Security Center:

* [Beveiligingsstatus bewaken](security-center-monitoring.md) : informatie over het bewaken van de status van uw Azure-resources.
* [Beheren en erop reageren op beveiligingswaarschuwingen](security-center-managing-and-responding-alerts.md) : informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Controle en het verwerken van beveiligingsgebeurtenissen](security-center-events-dashboard.md) : informatie over het bewaken en proces beveiligingsgebeurtenissen verzameld na verloop van tijd.
* [Partneroplossingen bewaken](security-center-partner-solutions.md) : informatie over het bewaken van de status van uw partneroplossingen.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/) : ophalen van de meest recente Azure-beveiliging-nieuws en informatie.

<!--Image references-->
[1]: ./media/security-center-using-recommendations/security-center-policy-inheritance.png
[2]: ./media/security-center-using-recommendations/scenario-roles.png
[3]: ./media/security-center-using-recommendations/select-recommendations-tile.png
[4]: ./media/security-center-using-recommendations/install-endpoint-protection.png
[5]:./media/security-center-using-recommendations/microsoft-antimalware.png
[6]: ./media/security-center-using-recommendations/provide-security-contact-details.png
[7]: ./media/security-center-using-recommendations/dismiss-recommendation.png
