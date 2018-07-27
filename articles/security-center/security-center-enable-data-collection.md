---
title: Verzamelen van gegevens in Azure Security Center | Microsoft Docs
description: " Informatie over het verzamelen van gegevens in Azure Security Center inschakelen. "
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2018
ms.author: rkarlin
ms.openlocfilehash: a5151d1f9498b29c79638445a58a8337abff8961
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281919"
---
# <a name="data-collection-in-azure-security-center"></a>Verzamelen van gegevens in Azure Security Center
Security Center verzamelt gegevens van uw virtuele Azure-machines (VM's) en niet-Azure-computers om te controleren op beveiligingsproblemen en bedreigingen. De gegevens worden verzameld met behulp van de MMA, die verschillende configuraties en gebeurtenislogboeken met betrekking tot beveiliging van de machine leest en de gegevens kopieert naar uw werkruimte voor analyse. Voorbeelden van dergelijke gegevens zijn: besturingssysteemtype en -versie, besturingssysteemlogboeken (Windows-gebeurtenislogboeken), actieve processen, computernaam, IP-adressen, aangemelde gebruiker, AppLocker-gebeurtenissen en tenant-ID. De Microsoft Monitoring Agent kopieert ook crashdumpbestanden naar uw werkruimte.

> [!NOTE]
> Om gegevens te verzamelen voor [besturingselementen voor adaptieve toepassingen](security-center-adaptive-application.md), Security Center configureert u een lokale AppLocker-beleid in de controlemodus om toe te staan alle toepassingen. Dit zorgt ervoor dat AppLocker voor het genereren van gebeurtenissen die worden verzameld en gebruikt door Security Center. Het is belangrijk te weten dat dit beleid niet worden geconfigureerd op alle computers waarop er al een geconfigureerde AppLocker-beleid is. 
>

## <a name="enable-automatic-provisioning-of-microsoft-monitoring-agent"></a>Automatische inrichting van Microsoft Monitoring Agent inschakelen     
Automatische inrichting is standaard uitgeschakeld. Als automatisch inrichten is ingeschakeld, ondersteund levert Security Center de Microsoft Monitoring Agent op alle Azure-VM's en een nieuwe VM's die worden gemaakt. Automatische inrichting wordt sterk aanbevolen, maar handmatige agentinstallatie is ook beschikbaar. [Informatie over het installeren van de Microsoft Monitoring Agent-extensie](../log-analytics/log-analytics-quick-collect-azurevm.md#enable-the-log-analytics-vm-extension).

> [!NOTE]
> - Door automatische inrichting uit te schakelen, wordt de beveiligingsbewaking voor uw resources beperkt. Zie voor meer informatie, [automatische inrichting](security-center-enable-data-collection.md#disable-automatic-provisioning) in dit artikel. Momentopnamen van de VM-schijf en artefacten verzameling zijn ingeschakeld, zelfs als automatisch inrichten is uitgeschakeld.
>

Automatische inrichting van de MMA inschakelen:
1. Selecteer in het hoofdmenu van Security Center de optie **beveiligingsbeleid**.
2. Selecteer het abonnement.

  ![Abonnement selecteren][7]

3. Onder **Beveiligingsbeleid** selecteert u **Gegevensverzameling**.
4. Onder **automatische inrichting**, selecteer **op** voor automatische inrichting inschakelen.
5. Selecteer **Opslaan**.

  ![Automatische inrichting inschakelen][1]

## <a name="default-workspace-configuration"></a>Configuratie van de standaardwerkruimte
Gegevens die zijn verzameld door Security Center wordt opgeslagen in Log Analytics-werkruimte.  U kunt ervoor kiezen de gegevens die worden verzameld van Azure VM's die zijn opgeslagen in werkruimten die zijn gemaakt door Security Center of in een bestaande werkruimte die u hebt gemaakt.

Gebruik uw bestaande Log Analytics-werkruimte:
- De werkruimte moet worden gekoppeld aan uw geselecteerde Azure-abonnement.
- Ten minste, moet u machtigingen voor toegang tot de werkruimte hebt gelezen.

Selecteer een bestaande Log Analytics-werkruimte:

1. Onder **configuratie van de standaardwerkruimte**, selecteer **gebruik een andere werkruimte**.

   ![Selecteer een bestaande werkruimte][2]

2. Selecteer een werkruimte voor het opslaan van verzamelde gegevens in de vervolgkeuzelijst.

  > [!NOTE]
  > In de vervolgkeuzelijst menu zijn alle werkruimten in al uw abonnementen beschikbaar. Zie [cross-werkruimte kunt selecteren. abonnement](security-center-enable-data-collection.md#cross-subscription-workspace-selection) voor meer informatie.
  >
  >

3. Selecteer **Opslaan**.
4. Na het selecteren van **opslaan**, wordt u gevraagd als u reconfigure bewaakte VM's wilt.

   - Selecteer **Nee** als u wilt dat de nieuwe werkruimte-instellingen wilt toepassen op nieuwe virtuele machines. De nieuwe werkruimte-instellingen zijn alleen van toepassing op nieuwe agentinstallaties; nieuwe gedetecteerde virtuele machines die geen van de Microsoft Monitoring Agent is geïnstalleerd.
   - Selecteer **Ja** als u wilt dat de nieuwe werkruimte-instellingen wilt toepassen op alle virtuele machines. Bovendien wordt elke virtuele machine verbonden met een Security Center-werkruimte gemaakt verbonden aan de nieuwe doelwerkruimte.

   > [!NOTE]
   > Als u Ja selecteert, moet u de werkruimten die zijn gemaakt door Security Center totdat alle virtuele machines hebt opnieuw is verbonden met de nieuwe doelwerkruimte niet verwijderen. Met deze bewerking mislukt als een werkruimte te vroeg is verwijderd.
   >
   >

   - Selecteer **annuleren** om de bewerking te annuleren.

     ![Selecteer een bestaande werkruimte][3]

## <a name="cross-subscription-workspace-selection"></a>Cross-abonnement werkruimte kunt selecteren.
Wanneer u een werkruimte voor het opslaan van uw gegevens selecteert, worden alle werkruimten in al uw abonnementen beschikbaar. Cross-abonnement kunt werkruimteselectie u gegevens van virtuele machines die worden uitgevoerd in verschillende abonnementen verzamelen en op te slaan in de werkruimte van uw keuze. Deze functie werkt voor zowel virtuele machines die worden uitgevoerd op Linux- en Windows.

> [!NOTE]
> Cross-abonnement is werkruimteselectie onderdeel van de gratis laag van Azure Security Center. Bekijk de pagina [Prijzen](security-center-pricing.md) voor meer informatie over de tariefopties van Security Center.
>
>

## <a name="data-collection-tier"></a>Verzameling gegevenslaag
Security Center kan het aantal gebeurtenissen verminderen terwijl voldoende gebeurtenissen voor onderzoek, controle en detectie van bedreigingen. U kunt het recht filterbeleid voor uw abonnementen en werkruimten op vier sets gebeurtenissen moeten worden verzameld door de agent.

- **Alle gebeurtenissen** – voor klanten die willen Zorg ervoor dat alle gebeurtenissen verzameld worden. Dit is de standaardinstelling.
- **Algemene** : dit is een set van gebeurtenissen die voldoet aan de meeste klanten en kan ze een volledige audittrail.
- **Minimale** – een kleiner aantal gebeurtenissen voor klanten die willen minimaliseren van het volume van de gebeurtenis.
- **Geen** – beveiligingsgebeurtenissen te verzamelen van het AppLocker-logboeken en beveiliging uitschakelen. Voor klanten die deze optie kiest, hebben hun dashboards beveiliging alleen Windows Firewall-logboeken en proactieve evaluaties zoals antimalware, basislijn en update.

> [!NOTE]
> Deze gebeurtenissen security sets zijn alleen beschikbaar op Standard-laag van Security Center bevinden. Bekijk de pagina [Prijzen](security-center-pricing.md) voor meer informatie over de tariefopties van Security Center.
Deze sets zijn ontworpen voor typische scenario's. Zorg ervoor dat u evalueren welke aan uw behoeften voldoet voordat u deze implementeert.
>
>

Om te bepalen van de gebeurtenissen die tot behoren de **algemene** en **minimale** evenementen, wij werken samen met klanten en industrienormen voor meer informatie over de ongefilterde frequentie van elke gebeurtenis en hun gebruik. We de volgende richtlijnen in dit proces gebruikt:

- **Minimale** -Zorg ervoor dat deze set omvat alleen de gebeurtenissen die kunnen wijzen op een geslaagde inbreuk en belangrijke gebeurtenissen die een zeer lage volume hebt. Bijvoorbeeld, deze verzameling bevat geslaagde en mislukte gebruikersaanmelding (gebeurtenis-id's 4624, 4625), maar het afmelden is belangrijk voor het controleren van, maar niet zinvol is voor de detectie en relatief hoog volume heeft geen bevat. De meeste van het gegevensvolume van deze set is de aanmeldgebeurtenissen en het maken van procesgebeurtenis (gebeurtenis-ID 4688).
- **Algemene** -bieden een volledige gebruikersaudittrail in deze set. Deze verzameling bevat bijvoorbeeld zowel gebruikersaanmelding en -afmelding (gebeurtenis-ID 4634). We nemen controle van acties zoals wijzigingen, belangrijke domain controller Kerberos operations en andere gebeurtenissen die door de bedrijfstak van organisaties worden aanbevolen.

Gebeurtenissen die zeer laag volume hebben zijn opgenomen in de gemeenschappelijke set als de belangrijkste reden om te kiezen dat het over alle gebeurtenissen is dat er minder i en niet voor het filteren van specifieke gebeurtenissen.

Hier volgt een compleet overzicht van de beveiliging en AppLocker gebeurtenis-id's voor elke set:

| Gegevenslaag | Verzamelde gebeurtenis indicatoren |
| --- | --- |
| Minimaal | 1102,4624,4625,4657,4663,4688,4700,4702,4719,4720,4722,4723,4724,4727,4728,4732,4735,4737,4739,4740,4754,4755, |
| | 4756,4767,4799,4825,4946,4948,4956,5024,5033,8001,8002,8003,8004,8005,8006,8007,8222 |
| Algemeen | 1,299,300,324,340,403,404,410,411,412,413,431,500,501,1100,1102,1107,1108,4608,4610,4611,4614,461,4622, |
| |  4624,4625,4634,4647,4648,4649,4657,4661,4662,4663,4665,4666,4667,4688,4670,4672,4673,4674,4675,4689,4697, |
| | 4700,4702,4704,4705,4716,4717,4718,4719,4720,4722,4723,4724,4725,4726,4727,4728,4729,4733,4732,4735,4737, |
| | 4738,4739,4740,4742,4744,4745,4746,4750,4751,4752,4754,4755,4756,4757,4760,4761,4762,4764,4767,4768,4771, |
| | 4774,4778,4779,4781,4793,4797,4798,4799,4800,4801,4802,4803,4825,4826,4870,4886,4887,4888,4893,4898,4902, |
| | 4904,4905,4907,4931,4932,4933,4946,4948,4956,4985,5024,5033,5059,5136,5137,5140,5145,5632,6144,6145,6272, |
| | 6273,6278,6416,6423,6424,8001,8002,8003,8004,8005,8006,8007,8222,26401,30004 |

> [!NOTE]
> - Als u gebruikmaakt van groepsbeleidsobject (GPO), is het aanbevolen dat u controlebeleid proces maken gebeurtenis 4688 inschakelt en de *CommandLine* veld binnen gebeurtenis 4688. Zie voor meer informatie over het proces maken gebeurtenis 4688 van Security Center [Veelgestelde vragen over](security-center-faq.md#what-happens-when-data-collection-is-enabled). Voor meer informatie over deze controlebeleid, Zie [Audit beleid aanbevelingen](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations).
> -  Om gegevens te verzamelen voor [besturingselementen voor adaptieve toepassingen](security-center-adaptive-application.md), Security Center configureert u een lokale AppLocker-beleid in de controlemodus om toe te staan alle toepassingen. Dit zorgt ervoor dat AppLocker voor het genereren van gebeurtenissen die worden verzameld en gebruikt door Security Center. Het is belangrijk te weten dat dit beleid niet worden geconfigureerd op alle computers waarop er al een geconfigureerde AppLocker-beleid is. 
>

Uw filterbeleid kiezen:
1. Op de **beveiligingsbeleid voor het verzamelen van gegevens** blade, selecteert u de filters gebruiken om beleid onder **beveiligingsgebeurtenissen**.
2. Selecteer **Opslaan**.

   ![Kies beleid filteren][5]

## <a name="disable-automatic-provisioning"></a>Automatische inrichting uitschakelen
Automatische inrichting van resources op elk gewenst moment door het uitschakelen van deze instelling in het beveiligingsbeleid, kunt u uitschakelen. Automatische inrichting wordt sterk aanbevolen om op te halen beveiligingswaarschuwingen en aanbevelingen over systeemupdates, beveiligingsproblemen en endpoint protection.

> [!NOTE]
> Wanneer u automatische inrichting uitschakelt, wordt MMA niet verwijderd van Azure-VM's waarop de agent is ingericht.
>
>

1. Ga terug naar het hoofdmenu van Security Center en selecteert u het beveiligingsbeleid.
2. Selecteer het abonnement waarvoor u automatisch inrichten wilt uitschakelen.
3. Op de **beveiligingsbeleid – gegevensverzameling** blade onder **automatische inrichting** Selecteer **uit**.
4. Selecteer **Opslaan**.

  ![Automatische inrichting uitschakelen][6]

Wanneer automatische inrichting is uitgeschakeld (uitgeschakeld), wordt de configuratiesectie van de standaard-werkruimte niet weergegeven.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe gegevens verzamelen en automatische inrichting in Security Center werkt. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen helpen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md): leer hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
- [Beveiliging van Azure Security Center gegevens](security-center-data-security.md) -informatie over hoe gegevens worden beheerd en beveiligd in Security Center.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Azure-beveiligingsblog): hier vindt u het laatste nieuws over Azure-beveiliging en andere informatie.

<!--Image references-->
[1]: ./media/security-center-enable-data-collection/enable-automatic-provisioning.png
[2]: ./media/security-center-enable-data-collection/use-another-workspace.png
[3]: ./media/security-center-enable-data-collection/reconfigure-monitored-vm.png
[5]: ./media/security-center-enable-data-collection/data-collection-tiers.png
[6]: ./media/security-center-enable-data-collection/disable-data-collection.png
[7]: ./media/security-center-enable-data-collection/select-subscription.png
