---
title: Toewijzing van de voorbeeld - ISO 27001 gedeelde Services blauwdruk - besturingselement
description: De toewijzing van het besturingselement van het voorbeeld van de blauwdruk ISO 27001 gedeelde Services aan Azure-beleid en RBAC.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: sample
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 5c225d76e1822d42500713578a7159eed0699a66
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "68225967"
---
# <a name="control-mapping-of-the-iso-27001-shared-services-blueprint-sample"></a>Toewijzing van het besturingselement van het voorbeeld van de blauwdruk ISO 27001 gedeelde Services

Het volgende artikel wordt uitgelegd hoe de Azure blauwdrukken ISO 27001 gedeelde Services blauwdruk voorbeeld is toegewezen aan de ISO 27001-besturingselementen. Zie voor meer informatie over de besturingselementen [ISO 27001](https://www.iso.org/isoiec-27001-information-security.html).

De volgende toewijzingen worden naar de **ISO 27001: 2013** besturingselementen. Gebruik de navigatiebalk aan de rechterkant om rechtstreeks naar de toewijzing van een bepaald besturingselement te gaan. Veel van de toegewezen besturingselementen worden geïmplementeerd met een [Azure Policy](../../../policy/overview.md) initiatief. Als u wilt controleren van de volledige initiatief, open **beleid** in Azure portal en selecteer de **definities** pagina. Vervolgens, zoek en selecteer de  **\[Preview\] controleren ISO 27001: 2013 besturingselementen en implementeren van specifieke VM-extensies ter ondersteuning van audit vereisten** ingebouwd beleid initiatief.

## <a name="a612-segregation-of-duties"></a>A.6.1.2 scheiding van taken

Met de eigenaar van slechts één Azure-abonnement zijn niet toegestaan voor administratieve redundantie. Met Azure-abonnement zijn te veel eigenaren kan daarentegen de mogelijkheid van een inbreuk op via een account waarmee is geknoeid eigenaar verhogen. Deze blauwdruk helpt u bij het onderhouden van een geschikt aantal eigenaars van Azure-abonnement door toe te wijzen twee [Azure Policy](../../../policy/overview.md) definities die het aantal eigenaars voor Azure-abonnementen te controleren. Eigenaarsmachtigingen abonnement beheren, kunt u de juiste scheiding van functies implementeren.

- \[Preview-versie\]: Minimum aantal eigenaars voor een abonnement controleren
- \[Preview-versie\]: Maximum aantal eigenaars voor een abonnement controleren

## <a name="a821-classification-of-information"></a>A.8.2.1 classificatie van gegevens

Azure [evaluatie van beveiligingsproblemen SQL service](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) kunt u gevoelige gegevens die zijn opgeslagen in uw databases te detecteren en bevat aanbevelingen om die gegevens te classificeren. Deze blauwdruk wijst een [Azure Policy](../../../policy/overview.md) definitie om te controleren dat zwakke plekken die we tijdens de scan voor evaluatie van de SQL-beveiligingsproblemen worden hersteld.

- \[Preview-versie\]: Evaluatie van beveiligingsproblemen SQL monitor resulteert in Azure Security Center

## <a name="a912-access-to-networks-and-network-services"></a>A.9.1.2 toegang tot netwerken en -services

Azure implementeert [op rollen gebaseerd toegangsbeheer](../../../../role-based-access-control/overview.md) (RBAC) voor het beheren van wie toegang heeft tot de Azure-resources. Deze blauwdruk helpt u bij het beheren van toegang tot Azure-resources door toe te wijzen zeven [Azure Policy](../../../policy/overview.md) definities. Dit beleid gebruik van de resourcetypen controleren en configuraties die mogelijk ruimer toegang tot resources.
Informatie over resources die in strijd is met deze beleidsregels kan help u corrigerende maatregelen te nemen Zorg ervoor dat toegang tot Azure-resources is beperkt tot gemachtigde gebruikers.

- \[Preview-versie\]: VM-extensie om te controleren van accounts zonder wachtwoorden Linux-VM implementeren
- \[Preview-versie\]: VM-extensie om te controleren van externe verbindingen van accounts zonder wachtwoorden Linux-VM implementeren
- \[Preview-versie\]: Controle uitvoeren op Linux-VM-accounts met geen wachtwoorden
- \[Preview-versie\]: Linux-VM zodat externe verbindingen van accounts zonder wachtwoorden controleren
- Het gebruik van klassieke opslagaccounts controleren
- Het gebruik van klassieke virtuele machines controleren
- Controleer virtuele machines die geen beheerde schijven gebruiken

## <a name="a923-management-of-privileged-access-rights"></a>A.9.2.3 beheer van rechten toegangsrechten

Deze blauwdruk helpt u bij het beperken en beheren van bevoegde toegang, rechten door toe te wijzen vier [Azure Policy](../../../policy/overview.md) definities om te controleren van externe accounts met de eigenaar en/of schrijfmachtigingen en accounts met de eigenaar en/of schrijfmachtigingen die geen multi-factor authentication ingeschakeld. De implementatie van Azure op rollen gebaseerd toegangsbeheer (RBAC) voor het beheren van wie toegang heeft tot Azure-resources. Deze blauwdruk wijst ook drie Azure Policy-definities voor het gebruik van Azure Active Directory-verificatie voor SQL-Servers en Service Fabric controleren. Met Azure Active Directory kunt authentication machtigingsbeheer voor vereenvoudigde en gecentraliseerde identiteitsbeheer van databasegebruikers en andere Microsoft-services. Deze blauwdruk wijst ook de definitie van een Azure-beleid om te controleren van het gebruik van aangepaste RBAC-regels. Inzicht krijgen in waar aangepaste RBAC-regels implementeren zijn kan u helpen controleren nodig en de juiste implementatie, zoals aangepaste RBAC-regels foutgevoelig zijn.

- \[Preview-versie\]: Accounts met eigenaarsmachtigingen die geen MFA is ingeschakeld op een abonnement controleren
- \[Preview-versie\]: Accounts met schrijfmachtigingen die geen MFA is ingeschakeld op een abonnement controleren
- \[Preview-versie\]: Externe accounts met eigenaarsmachtigingen voor een abonnement controleren
- \[Preview-versie\]: Externe accounts met schrijfmachtigingen voor een abonnement controleren
- Het inrichten van een Azure Active Directory-beheerder voor SQL server controleren
- Gebruik van Azure Active Directory voor clientverificatie in Service Fabric controleren
- Gebruik van aangepaste RBAC regels controleren

## <a name="a924-management-of-secret-authentication-information-of-users"></a>A.9.2.4 beheer van geheime verificatiegegevens van gebruikers

Deze blauwdruk wordt toegewezen drie [Azure Policy](../../../policy/overview.md) definities om te controleren van accounts waarvoor geen multi-factor authentication ingeschakeld. Met meervoudige verificatie kunnen accounts beveiligen, zelfs als er een stukje informatie van de verificatie is geknoeid. Door de bewaking van accounts zonder multi-factor authentication is ingeschakeld, kunt u de accounts die mogelijk meer waarschijnlijk worden aangetast identificeren. Deze blauwdruk wijst twee Azure Policy-definities die Linux-VM controleren ook de bestandsmachtigingen wachtwoord om u te waarschuwen als ze niet juist zijn ingesteld. Deze instelling kunt u corrigerende maatregelen nemen om ervoor te zorgen verificators niet worden aangetast.

- \[Preview-versie\]: Accounts met eigenaarsmachtigingen die geen MFA is ingeschakeld op een abonnement controleren
- \[Preview-versie\]: Accounts met leesmachtigingen die geen MFA is ingeschakeld op een abonnement controleren
- \[Preview-versie\]: Accounts met schrijfmachtigingen die geen MFA is ingeschakeld op een abonnement controleren
- \[Preview-versie\]: VM-extensie om te controleren van Linux-VM passwd bestandsmachtigingen implementeren
- \[Preview-versie\]: Audit Linux-VM/etc/passwd bestandsmachtigingen zijn ingesteld op 0644

## <a name="a925-review-of-user-access-rights"></a>A.9.2.5 beoordeling van de toegangsrechten voor gebruikers

Azure implementeert [op rollen gebaseerd toegangsbeheer](../../../../role-based-access-control/overview.md) (RBAC) kunt u beheren wie toegang heeft tot resources in Azure. Met behulp van de Azure-portal, kunt u controleren wie toegang heeft tot de Azure-resources en de bijbehorende machtigingen. Deze blauwdruk wordt toegewezen vier [Azure Policy](../../../policy/overview.md) definities om te controleren van accounts die met prioriteit moeten worden toegepast voor evaluatie, waaronder afgeschreven accounts en externe accounts met verhoogde bevoegdheden.

- \[Preview-versie\]: Afgeschafte accounts op een abonnement controleren
- \[Preview-versie\]: Afgeschafte accounts met eigenaarsmachtigingen voor een abonnement controleren
- \[Preview-versie\]: Externe accounts met eigenaarsmachtigingen voor een abonnement controleren
- \[Preview-versie\]: Externe accounts met schrijfmachtigingen voor een abonnement controleren

## <a name="a926-removal-or-adjustment-of-access-rights"></a>A.9.2.6 verwijderen of aanpassing van de toegangsrechten

Azure implementeert [op rollen gebaseerd toegangsbeheer](../../../../role-based-access-control/overview.md) (RBAC) kunt u beheren wie toegang heeft tot resources in Azure. Met behulp van [Azure Active Directory](../../../../active-directory/fundamentals/active-directory-whatis.md) en RBAC, kunt u gebruikersrollen als gevolg van organisatorische wijzigingen bijwerken. Wanneer dat nodig is, kunnen accounts worden geblokkeerd voor aanmelden (of verwijderd), waarbij rechten voor het Azure-resources onmiddellijk worden verwijderd. Deze blauwdruk wijst twee [Azure Policy](../../../policy/overview.md) definities om te controleren afgeschreven account dat moet worden overwogen voor verwijdering.

- \[Preview-versie\]: Afgeschafte accounts op een abonnement controleren
- \[Preview-versie\]: Afgeschafte accounts met eigenaarsmachtigingen voor een abonnement controleren

## <a name="a942-secure-log-on-procedures"></a>Beveiligde A.9.4.2 aanmelden met procedures

Deze blauwdruk wordt toegewezen drie Azure Policy-definities om te controleren van accounts waarvoor geen multi-factor authentication ingeschakeld. Azure multi-factor Authentication biedt extra beveiliging doordat een tweede vorm van verificatie en biedt een robuuste verificatie. Door de bewaking van accounts zonder multi-factor authentication is ingeschakeld, kunt u de accounts die mogelijk meer waarschijnlijk worden aangetast identificeren.

- \[Preview-versie\]: Accounts met eigenaarsmachtigingen die geen MFA is ingeschakeld op een abonnement controleren
- \[Preview-versie\]: Accounts met leesmachtigingen die geen MFA is ingeschakeld op een abonnement controleren
- \[Preview-versie\]: Accounts met schrijfmachtigingen die geen MFA is ingeschakeld op een abonnement controleren

## <a name="a943-password-management-system"></a>A.9.4.3 wachtwoord management-systeem

Deze blauwdruk helpt u bij het afdwingen van sterke wachtwoorden door toe te wijzen 10 [Azure Policy](../../../policy/overview.md) definities die controle uitvoeren op Windows-VM's die niet van minimaal sterkte en andere vereisten voor wachtwoorden afdwingen. Kennis van virtuele machines in strijd is met het wachtwoordbeleid sterkte helpt u corrigerende maatregelen nemen om ervoor te zorgen wachtwoorden voor alle VM-gebruikersaccounts zijn compatibel met het beleid.

- \[Preview-versie\]: Implementeren van VM-extensie om te controleren Windows virtuele machine wordt afgedwongen vereisten voor wachtwoordcomplexiteit
- \[Preview-versie\]: VM-extensie om te controleren van Windows-VM maximale wachtwoordduur 70 dagen implementeren
- \[Preview-versie\]: VM-extensie om te controleren van Windows-VM minimale wachtwoordduur 1 dag implementeren
- \[Preview-versie\]: Implementeren van VM-extensie voor het controleren van wachtwoorden voor Windows-VM moet ten minste 14 tekens
- \[Preview-versie\]: Implementeren van VM-extensie om te controleren Windows virtuele machine kunt u voorkomen dat wachtwoorden 24
- \[Preview-versie\]: Controle Windows-VM wordt afgedwongen vereisten voor wachtwoordcomplexiteit
- \[Preview-versie\]: Windows VM maximale wachtwoordduur 70 dagen controleren
- \[Preview-versie\]: Windows VM minimale wachtwoordduur 1 dag controleren
- \[Preview-versie\]: Controle Windows VM-wachtwoord moet ten minste 14 tekens
- \[Preview-versie\]: Controle Windows VM kunt u voorkomen dat wachtwoorden 24

## <a name="a1011-policy-on-the-use-of-cryptographic-controls"></a>A.10.1.1 beleid op het gebruik van cryptografische besturingselementen

Deze blauwdruk helpt u bij het afdwingen van het beleid voor het gebruik van cryptograph besturingselementen door toe te wijzen 13 [Azure Policy](../../../policy/overview.md) definities die specifiek cryptograph besturingselementen en audit afdwingen van zwakke cryptografische instellingen gebruiken.
Inzicht krijgen in waar uw Azure-resources mogelijk niet-optimale cryptografische configuraties kunt u corrigerende maatregelen nemen om ervoor te zorgen bronnen worden geconfigureerd in overeenstemming met het beveiligingsbeleid van uw gegevens. Met name vereisen de beleidsregels die zijn toegewezen door deze blauwdruk versleuteling voor blob storage-accounts en data lake storage-accounts; transparante gegevensversleuteling in SQL-databases; vereisen controle uitvoeren op ontbrekende versleuteling op storage-accounts, SQL-databases, virtuele-machineschijven en variabelen van automation-account. onbeveiligde verbindingen met de storage-accounts, functie-Apps, Web-App, API-Apps en Redis Cache controleren controle van zwakke virtuele machine wachtwoordversleuteling; en niet-versleutelde Service Fabric-communicatie.

- \[Preview-versie\]: Alleen HTTPS-toegang controleren voor een functie-App
- \[Preview-versie\]: Alleen HTTPS-toegang voor een webtoepassing controleren
- \[Preview-versie\]: Alleen HTTPS-toegang controleren voor een API-App
- \[Preview-versie\]: Controle uitvoeren op ontbrekende blobversleuteling voor opslagaccounts
- \[Preview-versie\]: Implementeren van VM-extensie om te controleren Windows virtuele machine moet geen wachtwoorden met omkeerbare versleuteling opslaan
- \[Preview-versie\]: Controle Windows-VM moet wachtwoorden met omkeerbare versleuteling niet opslaan
- \[Preview-versie\]: Niet-versleutelde SQL-database in Azure Security Center bewaken
- \[Preview-versie\]: Niet-versleutelde VM-schijven in Azure Security Center bewaken
- Controle inschakelen voor versleuteling van Automation-account variabelen
- Controle van alleen beveiligde verbindingen met uw Redis-Cache inschakelen
- Veilige overdracht naar storage-accounts controleren
- De instelling van de eigenschap ClusterProtectionLevel op EncryptAndSign in Service Fabric controleren
- Controlestatus van transparante gegevensversleuteling

## <a name="a1241-event-logging"></a>A.12.4.1-logboekregistratie

Deze blauwdruk kunt u ervoor zorgen systeemgebeurtenissen worden vastgelegd door toe te wijzen zeven [Azure Policy](../../../policy/overview.md) definities die de instellingen voor op Azure-resources controleren.
Diagnoselogboeken bieden inzicht in bewerkingen die zijn uitgevoerd in Azure-resources.

- \[Preview-versie\]: Implementatie van de afhankelijkheid Agent - VM-installatiekopie (OS) niet-vermelde controleren
- \[Preview-versie\]: Implementatie van de Agent in VMSS - VM-installatiekopie (OS) niet-vermelde afhankelijkheden controleren
- \[Preview-versie\]: Implementatie in de Log Analytics-Agent - VM-installatiekopie (OS) niet-vermelde controleren
- \[Preview-versie\]: Audit Log Analytics-Agent-implementatie in VMSS - VM-installatiekopie (OS) niet-vermelde
- \[Preview-versie\]: Niet-gecontroleerde SQL-database in Azure Security Center bewaken
- Diagnostische instelling voor controleren
- SQL server niveau controle-instellingen controleren

## <a name="a1243-administrator-and-operator-logs"></a>A.12.4.3 beheerder en de operator-Logboeken

Deze blauwdruk kunt u ervoor zorgen systeemgebeurtenissen worden vastgelegd door toe te wijzen zeven Azure Policy-definities die de instellingen voor op Azure-resources controleren. Diagnoselogboeken bieden inzicht in bewerkingen die zijn uitgevoerd in Azure-resources.

- \[Preview-versie\]: Implementatie van de afhankelijkheid Agent - VM-installatiekopie (OS) niet-vermelde controleren
- \[Preview-versie\]: Implementatie van de Agent in VMSS - VM-installatiekopie (OS) niet-vermelde afhankelijkheden controleren
- \[Preview-versie\]: Implementatie in de Log Analytics-Agent - VM-installatiekopie (OS) niet-vermelde controleren
- \[Preview-versie\]: Audit Log Analytics-Agent-implementatie in VMSS - VM-installatiekopie (OS) niet-vermelde
- \[Preview-versie\]: Niet-gecontroleerde SQL-database in Azure Security Center bewaken
- Diagnostische instelling voor controleren
- SQL server niveau controle-instellingen controleren

## <a name="a1244-clock-synchronization"></a>Synchronisatie van computerklokken A.12.4.4

Deze blauwdruk kunt u ervoor zorgen systeemgebeurtenissen worden vastgelegd door toe te wijzen zeven Azure Policy-definities die de instellingen voor op Azure-resources controleren. Logboeken in Azure, is afhankelijk van interne klokken aan te brengen van een record tijd gecorreleerde gebeurtenissen op resources.

- \[Preview-versie\]: Implementatie van de afhankelijkheid Agent - VM-installatiekopie (OS) niet-vermelde controleren
- \[Preview-versie\]: Implementatie van de Agent in VMSS - VM-installatiekopie (OS) niet-vermelde afhankelijkheden controleren
- \[Preview-versie\]: Implementatie in de Log Analytics-Agent - VM-installatiekopie (OS) niet-vermelde controleren
- \[Preview-versie\]: Audit Log Analytics-Agent-implementatie in VMSS - VM-installatiekopie (OS) niet-vermelde
- \[Preview-versie\]: Niet-gecontroleerde SQL-database in Azure Security Center bewaken
- Diagnostische instelling voor controleren
- SQL server niveau controle-instellingen controleren

## <a name="a1251-installation-of-software-on-operational-systems"></a>A.12.5.1 installatie van software op operationele systemen

Adaptieve toepassingsbesturingselementen is een oplossing van Azure Security Center die helpt u bepalen welke toepassingen kunnen worden uitgevoerd op uw virtuele machines die zich in Azure. Deze blauwdruk wijst een definitie van een Azure-beleid waarmee wijzigingen in de set met toegestane toepassingen worden gecontroleerd. Deze mogelijkheid helpt u bij het bepalen van de installatie van software en toepassingen op Azure Virtual machines.

- \[Preview-versie\]: Mogelijke opname Apps op Whitelist in Azure Security Center bewaken

## <a name="a1261-management-of-technical-vulnerabilities"></a>Beheer van A.12.6.1 van technische problemen

Deze blauwdruk helpt bij het beheren van gegevens system beveiligingsproblemen door toe te wijzen vijf [Azure Policy](../../../policy/overview.md) definities die ontbrekende systeemupdates, beveiligingsproblemen van besturingssystemen, SQL-beveiligingsproblemen en virtuele machine controleren door beveiligingslekken in Azure Security Center. Azure Security Center biedt rapportagemogelijkheden waarmee u realtime inzicht in de beveiligingsstatus van geïmplementeerde Azure-resources.

- \[Preview-versie\]: Ontbrekende Endpoint Protection bewaken in Azure Security Center
- \[Preview-versie\]: Ontbrekende systeemupdates in Azure Security Center bewaken
- \[Preview-versie\]: Beveiligingsproblemen van besturingssystemen bewaken in Azure Security Center
- \[Preview-versie\]: Evaluatie van beveiligingsproblemen SQL monitor resulteert in Azure Security Center
- \[Preview-versie\]: Beveiligingsproblemen van de monitor voor virtuele machines in Azure Security Center

## <a name="a1262-restrictions-on-software-installation"></a>A.12.6.2 beperkingen met betrekking tot software-installatie

Adaptieve toepassingsbesturingselementen is een oplossing van Azure Security Center die helpt u bepalen welke toepassingen kunnen worden uitgevoerd op uw virtuele machines die zich in Azure. Deze blauwdruk wijst een definitie van een Azure-beleid waarmee wijzigingen in de set met toegestane toepassingen worden gecontroleerd. Beperkingen met betrekking tot software-installatie kunt u de kans van de invoering van beveiligingsonderzoekers.

- \[Preview-versie\]: Mogelijke opname Apps op Whitelist in Azure Security Center bewaken

## <a name="a1311-network-controls"></a>Hiermee bepaalt u A.13.1.1 netwerk

Deze blauwdruk helpt u bij het beheren en netwerken toe te wijzen een [Azure Policy](../../../policy/overview.md) definitie waarmee netwerkbeveiligingsgroepen met ruime regels worden gecontroleerd. Regels die te ruime zijn onbedoelde netwerktoegang mogelijk en moeten worden gecontroleerd. Deze blauwdruk wijst ook drie Azure Policy-definities die niet-beveiligde eindpunten, toepassingen en storage-accounts bewaken. Eindpunten en toepassingen die niet zijn beveiligd door een firewall en storage-accounts met onbeperkte toegang kunnen toestaan onbedoelde toegang tot de gegevens in het systeem voor klantinformatie.

- \[Preview-versie\]: Ruime netwerktoegang in Azure Security Center bewaken
- \[Preview-versie\]: Netwerkeindpunten in Azure Security Center bewaken
- \[Preview-versie\]: Niet-beveiligde web-App in Azure Security Center bewaken
- Onbeperkte netwerktoegang tot de storage-accounts controleren

## <a name="a1321-information-transfer-policies-and-procedures"></a>A.13.2.1 informatie-overdracht beleid en procedures

De blauwdruk kunt u ervoor zorgen informatie mag geen overdracht met Azure-services is beveiligd door toe te wijzen twee [Azure Policy](../../../policy/overview.md) definities onveilig verbindingen met de storage-accounts en Redis Cache controleren.

- Controle van alleen beveiligde verbindingen met uw Redis-Cache inschakelen
- Veilige overdracht naar storage-accounts controleren

## <a name="next-steps"></a>Volgende stappen

Nu dat u de toewijzing van het besturingselement van de blauwdruk ISO 27001 gedeelde Services hebt doorgenomen, gaat u naar de volgende artikelen voor meer informatie over de architectuur en over het implementeren van dit voorbeeld:

> [!div class="nextstepaction"]
> [ISO 27001 gedeelde Services blauwdruk - overzicht](./index.md)
> [ISO 27001 gedeelde Services blauwdruk - stappen implementeren](./deploy.md)

Aanvullende artikelen over blauwdrukken en het gebruik hiervan:

- Meer informatie over de [levenscyclus van een blauwdruk](../../concepts/lifecycle.md).
- Meer informatie over hoe u [statische en dynamische parameters](../../concepts/parameters.md) gebruikt.
- Meer informatie over hoe u de [blauwdrukvolgorde](../../concepts/sequencing-order.md) aanpast.
- Meer informatie over hoe u gebruikmaakt van [resourcevergrendeling in blauwdrukken](../../concepts/resource-locking.md).
- Meer informatie over hoe u [bestaande toewijzingen bijwerkt](../../how-to/update-existing-assignments.md).
