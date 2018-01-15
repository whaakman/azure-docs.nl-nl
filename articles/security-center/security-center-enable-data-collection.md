---
title: Verzamelen van gegevens in Azure Security Center | Microsoft Docs
description: " Informatie over het inschakelen van verzamelen van gegevens in Azure Security Center. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/12/2018
ms.author: terrylan
ms.openlocfilehash: d5f2c9960b720fc44f37956f9150e89d6425d154
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/13/2018
---
# <a name="data-collection-in-azure-security-center"></a>Verzamelen van gegevens in Azure Security Center
Security Center verzamelt gegevens van uw virtuele Azure-machines (VM's) en niet-Azure-computers om te controleren op beveiligingsproblemen en bedreigingen. Gegevens worden verzameld met behulp van Microsoft Monitoring Agent, die verschillende configuraties betrekking hebben op beveiliging en gebeurtenislogboeken van de machine leest en kopieert de gegevens naar de werkruimte voor analyse. Voorbeelden van dergelijke gegevens zijn: besturingssysteemtype en -versie, besturingssysteemlogboeken (Windows-gebeurtenislogboeken), actieve processen, computernaam, IP-adressen, aangemelde gebruiker en tenant-ID. Crashdumpbestanden Microsoft Monitoring Agent ook gekopieerd naar de werkruimte.

## <a name="enable-automatic-provisioning-of-microsoft-monitoring-agent"></a>Automatische inrichting van Microsoft Monitoring Agent inschakelen     
Wanneer automatische inrichting is ingeschakeld, wordt Security Center voorziet in de Microsoft Monitoring Agent op alle ondersteunde virtuele Azure-machines en nieuwe bestanden die zijn gemaakt. Automatische inrichting is het raadzaam en is vereist voor abonnementen op de prijscategorie Standard van Security Center.

> [!NOTE]
> Uitschakelen van automatische inrichting limieten beveiligingsbewaking voor uw resources. Zie voor meer informatie, [automatische inrichting](security-center-enable-data-collection.md#disable-automatic-provisioning) in dit artikel. VM schijf momentopnamen en artefact verzameling zijn ingeschakeld, zelfs als automatische inrichting is uitgeschakeld.
>
>

Inschakelen van automatische inrichting van de Microsoft Monitoring Agent:
1. Selecteer onder het hoofdmenu Security Center **beveiligingsbeleid**.
2. Selecteer het abonnement.
3. Onder **beveiligingsbeleid**, selecteer **gegevensverzameling**.
4. Onder **Onboarding**, selecteer **op** automatische inrichting inschakelen.
5. Selecteer **Opslaan**.

![Automatische inrichting inschakelen][1]

## <a name="default-workspace-configuration"></a>Configuratie van de standaardwerkruimte
Gegevens die worden verzameld door Security Center wordt opgeslagen in workspace(s) logboekanalyse.  U kunt kiezen om gegevens verzameld van de Azure VM's die zijn opgeslagen in de werkruimten die zijn gemaakt door Security Center of in een bestaande werkruimte die u hebt gemaakt.

Uw bestaande werkruimte voor logboekanalyse gebruiken:
- De werkruimte moet worden gekoppeld aan uw geselecteerde Azure-abonnement.
- Ten minste moet u leesmachtigingen hebben voor toegang tot de werkruimte.

Selecteer een bestaande werkruimte voor logboekanalyse:

1. Onder **beveiligingsbeleid – gegevensverzameling**, selecteer **gebruik een andere werkruimte**.

   ![Selecteer een bestaande werkruimte][2]

2. Selecteer een werkruimte voor het opslaan van verzamelde gegevens in de vervolgkeuzelijst.

> [!NOTE]
> In de vervolgkeuzelijst menu, worden alleen de werkruimten die u hebt toegang tot en in uw Azure-abonnement weergegeven.
>
>

3. Selecteer **Opslaan**.
4. Na het selecteren van **opslaan**, wordt u gevraagd als u reconfigure bewaakt virtuele machines wilt.

   - Selecteer **Nee** als u wilt dat de nieuwe werkruimte-instellingen toepassen op de nieuwe virtuele machines. De nieuwe werkruimte-instellingen zijn alleen van toepassing op nieuwe agentinstallaties; nieuwe gedetecteerde virtuele machines die u geen Microsoft Monitoring Agent geïnstalleerd hebt.
   - Selecteer **Ja** als u wilt dat de nieuwe werkruimte-instellingen wilt toepassen op alle virtuele machines. Bovendien wordt elke VM die is verbonden met een werkruimte gemaakt Beveiligingscentrum verbonden aan de nieuwe doelwerkruimte.

   > [!NOTE]
   > Als u Ja selecteert, moet u de workspace(s) gemaakt door Security Center totdat alle virtuele machines hebt opnieuw is verbonden met de nieuwe doelwerkruimte niet verwijderen. Deze bewerking mislukt als een werkruimte te vroeg is verwijderd.
   >
   >

   - Selecteer **annuleren** om de bewerking te annuleren.

   ![Selecteer een bestaande werkruimte][3]

## <a name="data-collection-tier"></a>Verzameling gegevenslaag
Security Center kunt u het aantal gebeurtenissen verminderen behoud voldoende gebeurtenissen voor onderzoek, controle en detectie van dreigingen. U kunt het recht voor het filteren van beleid voor uw abonnementen en werkruimten uit vier sets met gebeurtenissen moeten worden verzameld door de agent.

- **Alle gebeurtenissen** – voor klanten die u wilt ervoor zorgen dat alle gebeurtenissen verzameld worden. Dit is de standaardinstelling.
- **Algemene** – dit is een reeks gebeurtenissen die voldoet aan de meeste klanten en kan ze een volledige audittrail.
- **Minimale** – een kleiner aantal gebeurtenissen voor klanten die willen het volume van de gebeurtenis te minimaliseren.
- **Geen** – beveiligingsgebeurtenissen te verzamelen van het AppLocker-logboeken en beveiliging uitschakelen. Voor klanten die deze optie kiest, hebben hun dashboards beveiliging alleen de logboeken van Windows Firewall en proactieve beoordelingen zoals anti-malware, basislijn en de update.

> [!NOTE]
> Deze sets zijn ontworpen voor het oplossen van typische scenario's. Zorg ervoor dat bepalen welke past bij uw behoeften voordat u deze implementeert.
>
>

Om te bepalen van de gebeurtenissen die tot behoren de **algemene** en **minimale** gebeurtenis sets, hebben we in samenwerking met klanten en industrienormen voor meer informatie over de ongefilterde frequentie van elke gebeurtenis en hun gebruik. We de volgende richtlijnen in dit proces gebruikt:

- **Minimale** -Zorg ervoor dat deze set behandelt alleen gebeurtenissen die kunnen wijzen op een geslaagde inbreuk en belangrijke gebeurtenissen die een zeer lage volume hebt. Bijvoorbeeld: deze set bevat geslaagde en mislukte gebruikersaanmelding (gebeurtenis-id's 4624, 4625), maar het afmelden is belangrijk voor controle, maar niet zinvol is voor de detectie en relatief hoog volume heeft geen bevat. De meeste van het gegevensvolume van deze set is de aanmeldgebeurtenissen en gebeurtenis (gebeurtenis-ID 4688) van een proces gemaakt.
- **Algemene** -bieden een volledige gebruiker audittrail in deze set. Deze verzameling bevat bijvoorbeeld gebruikersaanmeldingen en afmelding (gebeurtenis-ID 4634). We opnemen controle van acties zoals wijzigingen in de groep beveiliging, key domain controller Kerberos bewerkingen en andere gebeurtenissen die zijn aanbevolen door industriële organisaties.

Gebeurtenissen die zeer lage volume hebt zijn opgenomen in de gemeenschappelijke set als de belangrijkste doel te kiezen dat via alle gebeurtenissen is op het volume verkleinen en niet op specifieke gebeurtenissen filteren.

Dit is een volledig overzicht van de beveiligings- en App Referentiekluis gebeurtenis-id's voor elke set:

| Gegevenslaag | Verzamelde gebeurtenis indicatoren |
| --- | --- |
| minimale | 1102,4624,4625,4657,4663,4688,4700,4702,4719,4720,4722,4723,4724,4727,4728,4732,4735,4737,4739,4740,4754,4755, |
| | 4756,4767,4799,4825,4946,4948,4956,5024,5033,8001,8002,8003,8004,8005,8006,8007,8222 |
| Algemene (standaard) | 1,299,300,324,340,403,404,410,411,412,413,431,500,501,1100,1102,1107,1108,4608,4610,4611,4614,461,4622, |
| |  4624,4625,4634,4647,4648,4649,4657,4661,4662,4663,4665,4666,4667,4688,4670,4672,4673,4674,4675,4689,4697, |
| | 4700,4702,4704,4705,4716,4717,4718,4719,4720,4722,4723,4724,4725,4726,4727,4728,4729,4733,4732,4735,4737, |
| | 4738,4739,4740,4742,4744,4745,4746,4750,4751,4752,4754,4755,4756,4757,4760,4761,4762,4764,4767,4768,4771, |
| | 4774,4778,4779,4781,4793,4797,4798,4799,4800,4801,4802,4803,4825,4826,4870,4886,4887,4888,4893,4898,4902, |
| | 4904,4905,4907,4931,4932,4933,4946,4948,4956,4985,5024,5033,5059,5136,5137,5140,5145,5632,6144,6145,6272, |
| | 6273,6278,6416,6423,6424,8001,8002,8003,8004,8005,8006,8007,8222,26401,30004 |

> [!NOTE]
> Als u een groepsbeleidsobject (GPO) gebruikt, is het aanbevolen controlebeleid proces maken gebeurtenis 4688 in te schakelen en de *CommandLine* veld binnen 4688-gebeurtenis. Zie voor meer informatie over het proces maken gebeurtenis 4688 van Security Center [Veelgestelde vragen over](security-center-faq.md#what-happens-when-data-collection-is-enabled). Voor meer informatie over deze controlebeleid, Zie [Audit beleid aanbevelingen](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations).
>
>

Uw beleid voor filteren volgt kiezen:
1. Op de **beveiligingsbeleid & instellingen voor** blade, selecteer uw filteren beleid onder **beveiligingsgebeurtenissen**.
2. Selecteer **Opslaan**.

   ![Kies beleid filteren][5]

## <a name="disable-automatic-provisioning"></a>Automatische inrichting
U kunt uitschakelen om automatische inrichting van bronnen op elk gewenst moment door het uitschakelen van deze instelling in het beveiligingsbeleid. Automatische inrichting wordt sterk aanbevolen om op te halen beveiligingswaarschuwingen en aanbevelingen over systeemupdates, OS beveiligingsproblemen en endpoint protection.

> [!NOTE]
> Uitschakelen van automatische inrichting verwijdert geen Microsoft Monitoring Agent van de Azure VM's waarop de agent is ingericht.
>
>

1. Terug naar het hoofdmenu Security Center en selecteer het beveiligingsbeleid.

   ![Automatische inrichting][6]

2. Selecteer het abonnement dat u wilt automatisch inrichten uitschakelen.
3. Op de **beveiligingsbeleid – gegevensverzameling** blade onder **Onboarding** Selecteer **uit** automatisch inrichten uitschakelen.
4. Selecteer **Opslaan**.  

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe gegevens verzamelen en automatische inrichting in Security Center werkt. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md): leer hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
- [Beveiliging van gegevens van Azure Security Center](security-center-data-security.md) -informatie over hoe gegevens worden beheerd en beveiligd in Security Center.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Azure-beveiligingsblog): hier vindt u het laatste nieuws over Azure-beveiliging en andere informatie.

<!--Image references-->
[1]: ./media/security-center-enable-data-collection/enable-automatic-provisioning.png
[2]: ./media/security-center-enable-data-collection/use-another-workspace.png
[3]: ./media/security-center-enable-data-collection/reconfigure-monitored-vm.png
[5]: ./media/security-center-enable-data-collection/data-collection-tiers.png
[6]: ./media/security-center-enable-data-collection/disable-automatic-provisioning.png
