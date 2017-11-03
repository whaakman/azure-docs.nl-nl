---
title: Gebruik Azure Security Center aanbevelingen om beveiliging te verbeteren | Microsoft Docs
description: " Informatie over het gebruiken van beveiligingsbeleid en aanbevelingen in Azure Security Center om te beperken van een aanval op de beveiliging. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2017
ms.author: terrylan
ms.openlocfilehash: 0616f5e501324bfd821c1455ce234602f1fcf1bd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-security-center-recommendations-to-enhance-security"></a>Gebruik Azure Security Center aanbevelingen om beveiliging te verbeteren
U kunt de kans op een beveiligingsgebeurtenis aanzienlijke verminderen door een beveiligingsbeleid configureren en vervolgens implementeren van de aanbevelingen die door Azure Security Center. Dit artikel laat zien hoe u beveiligingsbeleid en aanbevelingen in Security Center om te beperken van een aanval op de beveiliging.

> [!NOTE]
> In dit artikel is gebaseerd op de rollen en -concepten in de Security Center [plannen en de operations guide](security-center-planning-and-operations-guide.md). Er is een goed idee om te controleren van de planning guide voordat u doorgaat.
>
>

## <a name="managing-security-recommendations"></a>Aanbevelingen voor beveiliging beheren
Een beveiligingsbeleid bepaalt welke set besturingselementen die worden aanbevolen voor resources binnen het opgegeven abonnement of resourcegroep. In Security Center definieert u beleid volgens de vereisten van de beveiliging van uw bedrijf. Zie voor meer informatie, [instellen van beveiligingsbeleid in Security Center](security-center-policies.md).

Beveiligingsbeleid voor resourcegroepen worden overgenomen van het abonnementsniveau.

![Beveiliging overname][1]

Als u aangepaste beleidsregels in bepaalde resourcegroepen nodig hebt, kunt u overname in de resourcegroep uitschakelen. Als u wilt uitschakelen, overname ingesteld op unieke op de blade beveiligingsbeleid en de besturingselementen die Security Center bevat aanbevelingen voor aanpassen.

Als u werkbelastingen waarvoor het beleid voor SQL Database Transparent Data Encryption (TDE) geen hebt, bijvoorbeeld het beleid op abonnementsniveau uitschakelen en alleen in de resourcegroepen waar SQL TDE vereist is het inschakelen.

> [!NOTE]
> Als er een conflict tussen het beleid op abonnementsniveau en het beleid op het niveau van de resourcegroep is, heeft het beleid op het niveau van de resourcegroep voorrang.
>
>

De beveiligingsstatus van uw Azure-resources wordt gecontroleerd met Security Center. Wanneer het Beveiligingscentrum identificeert mogelijke beveiligingsproblemen, maakt deze aanbevelingen op basis van de besturingselementen die zijn ingesteld in het beveiligingsbeleid. De aanbevelingen leiden u door het configuratieproces van de benodigde beveiligingsmechanismen.

Huidige beleid aanbevelingen in Security Center focus op het, systeemupdates, configuratie van het besturingssysteem, netwerk-beveiligingsgroepen op subnetten en virtuele machines (VM's), SQL Database Auditing, SQL Database TDE en web application firewalls. Zie voor de meest recente dekking met Security Center aanbevelingen [aanbevelingen voor beveiliging in Security Center beheren](security-center-recommendations.md).

## <a name="scenario"></a>Scenario
Dit scenario wordt beschreven hoe u met Security Center te verminderen de kans op een aanzienlijke beveiligingsincident voordoet door Security Center aanbevelingen bewaken en actie ondernemen. Het scenario wordt het fictieve bedrijf, Contoso en rollen die zijn gepresenteerd in het Beveiligingscentrum [plannen en de operations guide](security-center-planning-and-operations-guide.md#security-roles-and-access-controls). De rollen vertegenwoordigen individuen en teams die van Security Center gebruikmaken mogelijk verschillende taken die betrekking hebben op beveiliging uit te voeren. De rollen zijn:

![Scenario-functies][2]

Contoso onlangs gemigreerd enkele van hun lokale bronnen in Azure. Contoso wil implementeren en onderhouden van beveiliging die Verminder de kwetsbaarheid voor een aanval op de beveiliging van hun bronnen in de cloud.

## <a name="recommended-solution"></a>Aanbevolen oplossing
Er is een oplossing Security Center gebruiken om te voorkomen en beveiligingsproblemen detecteren. Contoso heeft toegang tot Security Center via het Azure-abonnement. De [gratis laag](security-center-pricing.md) van Security Center wordt automatisch ingeschakeld op alle Azure-abonnementen en verzamelen van gegevens is ingeschakeld op alle VM's in het abonnement.

David in de Contoso IT-beveiliging, configureert u een **beveiligingsbeleid** met Security Center. Security Center analyseert de beveiligingsstatus van de Contoso Azure-resources. Wanneer het Beveiligingscentrum identificeert mogelijke beveiligingsproblemen, maakt deze **aanbevelingen** op basis van de besturingselementen die zijn ingesteld in het beveiligingsbeleid.

Jeff, de eigenaar van een cloud-werkbelasting is verantwoordelijk voor het implementeren en onderhouden van bescherming in overeenstemming met het beveiligingsbeleid van Contoso. Jeff kunt bewaken de aanbevelingen die is gemaakt door Security Center beveiliging toepassen. De aanbevelingen leiden Jeff door het configuratieproces van de benodigde beveiligingsmechanismen.

In de volgorde voor Jeff implementeren en onderhouden van beveiliging en beveiligingsproblemen elimineren, moet hij:

- Beveiligingsaanbevelingen voor de monitor geleverd door Security Center
- Aanbevelingen voor beveiliging evalueren en te bepalen of hij moet toepassen of verwijderen
- Aanbevelingen voor beveiliging toepassen

Laten we stappen van Jeff om te zien hoe gebruikt hij Security Center aanbevelingen om hem te helpen bij het configuratieproces van besturingselementen om te voorkomen beveiligingsproblemen.

## <a name="how-to-implement-this-solution"></a>Deze oplossing implementeren
Jeff zich aanmeldt bij [Azure-portal](https://azure.microsoft.com/features/azure-portal/) en opent u de console Security Center. Als onderdeel van de dagelijkse bewakingsactiviteiten controleert hij of er aanbevelingen voor beveiliging zijn door de volgende stappen uit te voeren:

1. Jeroen selecteert de **aanbevelingen** tegel openen **aanbevelingen**.
   ![Selecteer de tegel aanbevelingen][3]
2. Jeroen controleert de lijst met aanbevelingen. Hij ziet dat de lijst met aanbevelingen in volgorde van prioriteit, van de hoogste prioriteit naar de laagste prioriteit is opgegeven door Security Center. Hij besluit om een aanbeveling hoge prioriteit in de lijst. Hij selecteert **Endpoint Protection installeren** onder **aanbevelingen**.
3. **Endpoint Protection installeren** wordt geopend met een lijst van virtuele machines zonder antimalware is ingeschakeld. Jeff controleert de lijst met virtuele machines, selecteert u alle virtuele machines en vervolgens selecteert **installeren op 3 VM's**.
   ![Eindpuntbeveiliging installeren][4]
4. **Selecteer de Endpoint Protection** wordt geopend Jeff twee antimalware-oplossingen bieden. Jeroen selecteert de **Microsoft Antimalware** oplossing.
5. Meer informatie over de antimalwareoplossing wordt weergegeven. Jeroen selecteert **maken**.
   ![Microsoft antimalware][5]
6. Ties voert de vereiste configuratie-instellingen onder **installeren** en selecteert u **OK**.

[Microsoft Antimalware](../security/azure-security-antimalware.md) is nu actief zijn op de geselecteerde virtuele machines.

Jeff blijft doorlopen van de hoge prioriteit en de gemiddelde prioriteit aanbevelingen, met het nemen van besluiten over de uitvoering. Jeff verwijst naar de [aanbevelingen voor beveiliging beheren](security-center-recommendations.md) artikel leert de aanbevelingen en elk biedt als hij van toepassing is.

Jeff leert die [Microsoft Security Response Center (MSRC)](../security/azure-security-response-center.md) voert selecteert u beveiligingsbewaking van de Azure-netwerk en de infrastructuur en threat intelligence en misbruik klachten ontvangt van een derde partij. Als Jeff biedt informatie over contact op met de beveiliging voor Contoso Azure-abonnement, contactpersonen van Microsoft Contoso als het MSRC klantgegevens die Contoso detecteert is geopend door een onrechtmatig of niet-geautoriseerde partij. Laten we volgen Jeff hij de van toepassing is de **contact op met informatie over de beveiliging bieden** aanbeveling (een aanbeveling met de ernst van de media die in de lijst met de bovenstaande aanbevelingen).

1. Jeroen selecteert **contact op met informatie over de beveiliging bieden** onder **aanbevelingen**, wordt geopend **contact op met informatie over de beveiliging bieden**.
2. Jeroen selecteert contact op met informatie te verstrekken over het Azure-abonnement. Een tweede **contact op met informatie over de beveiliging bieden** blade wordt geopend.
   ![Neem contact op met informatie over de beveiliging][6]
3. Onder **contact op met informatie over de beveiliging bieden**, krijgt de Jeff:

  - de beveiliging contact op met e-mailadressen gescheiden door komma's (Er is niet een limiet aan het aantal e-mailadressen die hij kunt invoeren)
  - een beveiliging Neem contact op met telefoonnummer

4. Jeff ook Hiermee schakelt u de optie **Stuur mij e-mails over waarschuwingen** voor het ontvangen van e-mailberichten over waarschuwingen met hoog dreigingsniveau.
5. Jeroen selecteert **OK** de contactgegevens van de beveiliging toepassen op Contoso abonnement.

Tot slot Jeff bekijkt de aanbeveling met lage prioriteit **herstellen OS beveiligingslekken** en bepaalt deze aanbeveling is niet van toepassing. Hij wil de aanbeveling negeren. Jeroen selecteert de drie punten die worden weergegeven aan de rechterkant, en selecteert vervolgens **sluiten**.
   ![Aanbeveling negeren][7]

## <a name="conclusion"></a>Conclusie
Bewaking van de aanbevelingen in Security Center kunt u beveiligingsproblemen elimineren voordat een aanval plaatsvindt. Door het implementeren en onderhouden van beveiligingen aan het beveiligingsbeleid in Security Center kunt u voorkomen dat een beveiligingsincident voordoet.

## <a name="next-steps"></a>Volgende stappen
Dit scenario hebt u geleerd u het gebruik van beveiligingsbeleid en aanbevelingen in Security Center om te beperken van een aanval op de beveiliging. Zie de [respons op incidenten scenario](security-center-incident-response.md) voor meer informatie over hoe u een respons op incidenten in plaats van plan bent voordat een aanval plaatsvindt.

Zie voor meer informatie over Security Center:

* [Beveiligingsstatus bewaken](security-center-monitoring.md) : informatie over het bewaken van de status van uw Azure-resources.
* [Beheren en erop reageren op beveiligingswaarschuwingen](security-center-managing-and-responding-alerts.md) : informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Bewaking en het verwerken van beveiligingsgebeurtenissen](security-center-events-dashboard.md) - informatie over het bewaken en er proces beveiligingsgebeurtenissen verzameld gedurende een bepaalde periode.
* [Partneroplossingen bewaken](security-center-partner-solutions.md) : informatie over het bewaken van de status van uw partneroplossingen.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) — Lees het laatste nieuws van de Azure-beveiliging en de informatie.

<!--Image references-->
[1]: ./media/security-center-using-recommendations/security-center-policy-inheritance.png
[2]: ./media/security-center-using-recommendations/scenario-roles.png
[3]: ./media/security-center-using-recommendations/select-recommendations-tile.png
[4]: ./media/security-center-using-recommendations/install-endpoint-protection.png
[5]:./media/security-center-using-recommendations/microsoft-antimalware.png
[6]: ./media/security-center-using-recommendations/provide-security-contact-details.png
[7]: ./media/security-center-using-recommendations/dismiss-recommendation.png
