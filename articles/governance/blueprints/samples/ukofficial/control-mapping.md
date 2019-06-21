---
title: Toewijzing van de voorbeeld - UK-OFFICIAL blauwdruk - besturingselement
description: De toewijzing van de controle van het Verenigd Koninkrijk officiële blauwdruk-voorbeeld.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/13/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 2eb6e62bc891a147a89107f5e3de7c2b605bbd09
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276975"
---
# <a name="control-mapping-of-the-uk-official-blueprint-sample"></a>Toewijzing van de controle van de voorbeeld-UK-OFFICIAL blauwdruk

Het volgende artikel wordt uitgelegd hoe het Verenigd Koninkrijk officiële voorbeeld van de blauwdruk wordt toegewezen aan de officiële UK besturingselementen.
Zie voor meer informatie over de besturingselementen [UK-OFFICIAL](https://www.gov.uk/government/publications/government-security-classifications).

De volgende toewijzingen worden naar de **UK-OFFICIAL** besturingselementen. Gebruik de navigatiebalk aan de rechterkant om rechtstreeks naar de toewijzing van een bepaald besturingselement te gaan. Veel van de toegewezen besturingselementen worden geïmplementeerd met een [Azure Policy](../../../policy/overview.md) initiatief. Als u wilt controleren van de volledige initiatief, open **beleid** in Azure portal en selecteer de **definities** pagina. Klik, zoek en selecteer de  **[Preview] UK-OFFICIAL Audit bepaalt en implementeren van specifieke VM-extensies ter ondersteuning van audit vereisten** ingebouwd beleid initiatief.

## <a name="1-data-in-transit-protection"></a>1-gegevens in transit beveiliging

De blauwdruk kunt u ervoor zorgen informatie mag geen overdracht met Azure-services is beveiligd door toe te wijzen twee [Azure Policy](../../../policy/overview.md) definities onveilig verbindingen met de storage-accounts en Redis Cache controleren.

- Alleen beveiligde verbindingen met uw Redis-Cache moeten worden ingeschakeld
- Veilige overdracht naar storage-accounts moet worden ingeschakeld

## <a name="23-data-at-rest-protection"></a>2.3 data-at-rest-beveiliging

Deze blauwdruk helpt u bij het afdwingen van het beleid voor het gebruik van cryptograph besturingselementen door toe te wijzen 11 [Azure Policy](../../../policy/overview.md) definities die specifiek cryptograph besturingselementen en audit afdwingen van zwakke cryptografische instellingen gebruiken.
Inzicht krijgen in waar uw Azure-resources mogelijk niet-optimale cryptografische configuraties kunt u corrigerende maatregelen nemen om ervoor te zorgen bronnen worden geconfigureerd in overeenstemming met het beveiligingsbeleid van uw gegevens. Met name vereisen de beleidsregels die zijn toegewezen door deze blauwdruk versleuteling voor data lake storage-accounts; transparante gegevensversleuteling in SQL-databases; vereisen controle uitvoeren op ontbrekende versleuteling op storage-accounts, SQL-databases, virtuele-machineschijven en variabelen van automation-account. onbeveiligde verbindingen met de storage-accounts en Redis Cache controleren controle van zwakke virtuele machine wachtwoordversleuteling; en niet-versleutelde Service Fabric-communicatie.

- Niet-versleutelde SQL-databases in Azure Security Center bewaken
- Schijfversleuteling moet worden toegepast op virtuele machines
- Automation-account variabelen moeten worden versleuteld.
- Veilige overdracht naar storage-accounts moet worden ingeschakeld
- Veilige overdracht naar storage-accounts moet worden ingeschakeld
- Service Fabric-clusters moet de eigenschap ClusterProtectionLevel is ingesteld op EncryptAndSign
- De Transparent Data Encryption voor SQL-databases moet zijn ingeschakeld
- Transparent data encryption voor SQL-database implementeren
- Versleuteling vereisen op een Data Lake Store-accounts
- Locaties toegestaan (is hard gecodeerd "UK-Zuid" en 'UK-WEST')
- Locaties voor resourcegroepen toegestaan (is hard gecodeerd "UK-Zuid" en 'UK-WEST')

## <a name="52-vulnerability-management"></a>5.2-beveiligingsproblemen

Deze blauwdruk helpt bij het beheren van gegevens system beveiligingsproblemen door toe te wijzen vijf [Azure Policy](../../../policy/overview.md) definities die ontbrekende endpoint protection, ontbrekende systeemupdates, operationele systeem kwetsbaarheden, SQL controleren beveiligingsproblemen en beveiligingsproblemen van de virtuele machine. Deze inzichten bieden realtime informatie over de beveiligingsstatus van uw geïmplementeerde resources en kunnen u herstelacties prioriteren.

- Ontbrekende Endpoint Protection bewaken in Azure Security Center
- Systeemupdates moeten worden geïnstalleerd op uw virtuele machines
- Door beveiligingslekken in de beveiligingsconfiguratie van de op uw computers moeten worden hersteld.
- Door beveiligingslekken in uw SQL-databases moeten worden hersteld.
- Beveiligingsproblemen moeten worden hersteld door een oplossing voor evaluatie van beveiligingsproblemen

## <a name="53-protective-monitoring"></a>5.3 beschermende controleren

Deze blauwdruk helpt u bij het beveiligen van gegevensassets van het systeem door toe te wijzen een [Azure Policy](../../../policy/overview.md) definitie waarmee wordt gecontroleerd onbeperkte storage-accounts. Deze blauwdruk wijst ook de definitie van een Azure Policy waarmee goedgekeurde activiteiten worden gecontroleerd.

- Onbeperkte netwerktoegang tot de storage-accounts controleren
- Besturingselementen voor adaptieve toepassingen moeten zijn ingeschakeld op virtuele machines
- Detectie van bedreigingen op SQL-servers implementeren
- Standaard Microsoft IaaS Anti-malware-extensie voor Windows Server implementeren

## <a name="9-secure-user-management--10-identity-and-authentication"></a>9 secure Gebruikersbeheer / 10 identiteit en verificatie

Op rollen gebaseerd toegangsbeheer (RBAC) kunt die u beheren wie toegang tot resources in Azure heeft de implementatie van Azure. Met behulp van de Azure-portal, kunt u controleren wie toegang heeft tot de Azure-resources en de bijbehorende machtigingen. Deze blauwdruk helpt u bij het beperken en beheren van bevoegde toegang, rechten door toe te wijzen zes [Azure Policy](../../../policy/overview.md) definities om te controleren van externe accounts met de eigenaar en/of lezen/schrijven machtigingen en -accounts met de eigenaar, lees-en/of schrijven machtigingen die nog geen multi-factor authentication ingeschakeld.

- MFA moet worden ingeschakeld voor accounts met eigenaarsmachtigingen voor uw abonnement
- MFA moet worden ingeschakeld accounts met schrijfmachtigingen voor uw abonnement
- MFA moet worden ingeschakeld voor accounts met leesmachtigingen voor uw abonnement
- Externe accounts met eigenaarsmachtigingen voor een abonnement controleren
- Externe accounts met schrijfmachtigingen voor een abonnement controleren
- Externe accounts met leesmachtigingen voor een abonnement controleren

Deze blauwdruk wijst twee Azure Policy-definities voor het gebruik van Azure Active Directory-verificatie voor SQL-servers en Service Fabric controleren. Met Azure Active Directory kunt authentication machtigingsbeheer voor vereenvoudigde en gecentraliseerde identiteitsbeheer van databasegebruikers en andere Microsoft-services.

- Een Azure Active Directory-beheerder moet worden ingericht voor SQL-servers
- Service Fabric-clusters moeten alleen Azure Active Directory gebruiken voor clientverificatie

Deze blauwdruk wijst vijf Azure Policy-definities voor de accounts die met prioriteit moeten worden toegepast voor evaluatie, waaronder afgeschreven accounts en externe accounts controleren ook toe. Wanneer dat nodig is, kunnen accounts worden geblokkeerd voor aanmelden (of verwijderd), waarbij rechten voor het Azure-resources onmiddellijk worden verwijderd. Deze blauwdruk wijst twee Azure Policy-definities toe en audit afgeschreven-account dat moet worden overwogen voor verwijdering.

- Afgeschafte accounts moeten worden verwijderd uit uw abonnement
- Afgeschafte accounts met eigenaarsmachtigingen moeten worden verwijderd uit uw abonnement
- Externe accounts met eigenaarsmachtigingen moeten worden verwijderd van uw abonnement
- Externe accounts met schrijfmachtigingen moeten worden verwijderd uit uw abonnement

Deze blauwdruk wijst ook de definitie van een Azure Policy die voert een controle uit Linux-VM bestandsmachtigingen wachtwoord om u te waarschuwen als ze niet juist zijn ingesteld. Dit ontwerp kunt u corrigerende maatregelen nemen om ervoor te zorgen verificators niet worden aangetast.

- [Preview]: Audit Linux VM /etc/passwd file permissions are set to 0644

Deze blauwdruk helpt u bij het afdwingen van sterke wachtwoorden 10 Azure Policy-definities die controle uitvoeren op Windows-VM's die niet van minimaal sterkte en andere vereisten voor wachtwoorden afdwingen toe te wijzen. Kennis van virtuele machines in strijd is met het wachtwoordbeleid sterkte helpt u corrigerende maatregelen nemen om ervoor te zorgen wachtwoorden voor alle VM-gebruikersaccounts zijn compatibel met het beleid.

- [Preview]: Deploy requirements to audit Windows VMs that do not have the password complexity setting enabled
- [Preview]: Deploy requirements to audit Windows VMs that do not have a maximum password age of 70 days
- [Preview]: Deploy requirements to audit Windows VMs that do not have a minimum password age of 1 day
- [Preview]: Deploy requirements to audit Windows VMs that do not restrict the minimum password length to 14 characters
- [Preview]: Deploy requirements to audit Windows VMs that allow re-use of the previous 24 passwords
- [Preview]: Audit Windows VMs that do not have the password complexity setting enabled
- [Preview]: Audit Windows VMs that do not have a maximum password age of 70 days
- [Preview]: Audit Windows VMs that do not have a minimum password age of 1 day
- [Preview]: Audit Windows VMs that do not restrict the minimum password length to 14 characters
- [Preview]: Audit Windows VMs that allow re-use of the previous 24 passwords

Deze blauwdruk ook kunt u toegang tot Azure-resources beheren door toe te wijzen zeven Azure Policy-definities. Dit beleid gebruik van de resourcetypen controleren en configuraties die mogelijk ruimer toegang tot resources. Informatie over resources die in strijd is met deze beleidsregels kan help u corrigerende maatregelen te nemen Zorg ervoor dat toegang tot Azure-resources is beperkt tot gemachtigde gebruikers.

- [Preview]: Deploy requirements to audit Linux VMs that have accounts without passwords
- [Preview]: Deploy requirements to audit Linux VMs that allow remote connections from accounts without passwords
- [Preview]: Audit Linux VMs that have accounts without passwords
- [Preview]: Audit Linux VMs that allow remote connections from accounts without passwords
- Storage-accounts moeten worden gemigreerd naar nieuwe Azure Resource Manager-resources
- Virtuele machines moeten worden gemigreerd naar nieuwe Azure Resource Manager-resources
- Controleer virtuele machines die geen beheerde schijven gebruiken

## <a name="11-external-interface-protection"></a>11 externe Interface-beveiliging

Dan met behulp van meer dan 25 beleid voor beheer van de juiste beveiligde gebruikers, deze blauwdruk helpt u bij het service-interfaces beveiligen tegen onbevoegde toegang door toe te wijzen een [Azure Policy](../../../policy/overview.md) definitie die monitors onbeperkte Storage-accounts. Storage-accounts met onbeperkte toegang kunnen onbedoelde toegang tot de gegevens in het systeem voor klantinformatie toestaan. Deze blauwdruk wijst ook een beleid waarmee besturingselementen voor adaptieve toepassingen op virtuele machines.

- Onbeperkte netwerktoegang tot de storage-accounts controleren
- Besturingselementen voor adaptieve toepassingen moeten zijn ingeschakeld op virtuele machines

## <a name="12-secure-service-administration"></a>12 beveiligde servicebeheer

Op rollen gebaseerd toegangsbeheer (RBAC) kunt die u beheren wie toegang tot resources in Azure heeft de implementatie van Azure. Met behulp van de Azure-portal, kunt u controleren wie toegang heeft tot de Azure-resources en de bijbehorende machtigingen. Deze blauwdruk helpt u bij het beperken en beheren van bevoegde toegang, rechten door toe te wijzen vijf [Azure Policy](../../../policy/overview.md) definities om te controleren van externe accounts met de eigenaar en/of schrijfmachtigingen en accounts met de eigenaar en/of schrijfmachtigingen die geen multi-factor authentication ingeschakeld hebben.

Systemen die worden gebruikt voor het beheer van een service in de cloud wordt zeer uitgebreide toegang tot die service. Hun inbreuk zou aanzienlijke invloed, met inbegrip van de middelen te omzeilen beveiligingsmaatregelen en stelen of manipuleren van grote hoeveelheden gegevens hebben. De methoden die door beheerders van de serviceprovider wordt gebruikt voor het beheren van de operationele service moeten worden ontworpen om een risico van misbruik die de beveiliging van de service kan ondermijnen. Als dit principe is niet geïmplementeerd, kan een aanvaller de middelen om de beveiligingsmaatregelen overslaan en stelen of manipuleren van grote hoeveelheden gegevens hebben.

- MFA moet worden ingeschakeld voor accounts met eigenaarsmachtigingen voor uw abonnement
- MFA moet worden ingeschakeld accounts met schrijfmachtigingen voor uw abonnement
- Externe accounts met eigenaarsmachtigingen moeten worden verwijderd van uw abonnement
- Externe accounts met schrijfmachtigingen moeten worden verwijderd uit uw abonnement

Deze blauwdruk wijst twee Azure Policy-definities voor het gebruik van Azure Active Directory-verificatie voor SQL-servers en Service Fabric controleren. Met Azure Active Directory kunt authentication machtigingsbeheer voor vereenvoudigde en gecentraliseerde identiteitsbeheer van databasegebruikers en andere Microsoft-services.

- Een Azure Active Directory-beheerder moet worden ingericht voor SQL-servers
- Service Fabric-clusters moeten alleen Azure Active Directory gebruiken voor clientverificatie

Deze blauwdruk wijst ook vier Azure Policy-definities om te controleren van accounts die met prioriteit moeten worden toegepast voor evaluatie, waaronder afgeschreven accounts en externe accounts met verhoogde bevoegdheden. Wanneer dat nodig is, kunnen accounts worden geblokkeerd voor aanmelden (of verwijderd), waarbij rechten voor het Azure-resources onmiddellijk worden verwijderd. Deze blauwdruk wijst twee Azure Policy-definities toe en audit afgeschreven-account dat moet worden overwogen voor verwijdering.

- Afgeschafte accounts moeten worden verwijderd uit uw abonnement
- Afgeschafte accounts met eigenaarsmachtigingen moeten worden verwijderd uit uw abonnement
- Externe accounts met eigenaarsmachtigingen moeten worden verwijderd van uw abonnement
- Externe accounts met schrijfmachtigingen moeten worden verwijderd uit uw abonnement

Deze blauwdruk wijst ook de definitie van een Azure Policy die voert een controle uit Linux-VM bestandsmachtigingen wachtwoord om u te waarschuwen als ze niet juist zijn ingesteld. Dit ontwerp kunt u corrigerende maatregelen nemen om ervoor te zorgen verificators niet worden aangetast.

- [Preview]: Audit Linux VM /etc/passwd file permissions are set to 0644

## <a name="13-audit-information-for-users"></a>13 controle-informatie voor gebruikers

Deze blauwdruk kunt u ervoor zorgen systeemgebeurtenissen worden vastgelegd door toe te wijzen 6 [Azure Policy](../../../policy/overview.md) definities die de instellingen voor op Azure-resources controleren. Een toegewezen beleid controleert ook als virtuele machines worden niet logboeken te naar een opgegeven log analytics-werkruimte verzenden.

- Niet-gecontroleerde SQL-servers in Azure Security Center bewaken
- Diagnostische instelling voor controleren
- SQL server niveau controle-instellingen controleren
- [Preview]: Deploy Log Analytics Agent for Linux VMs
- [Preview]: Deploy Log Analytics Agent for Windows VMs
- Network watcher implementeren wanneer virtuele netwerken worden gemaakt

## <a name="next-steps"></a>Volgende stappen

Aanvullende artikelen over blauwdrukken en het gebruik hiervan:

- Meer informatie over de [levenscyclus van een blauwdruk](../../concepts/lifecycle.md).
- Meer informatie over hoe u [statische en dynamische parameters](../../concepts/parameters.md) gebruikt.
- Meer informatie over hoe u de [blauwdrukvolgorde](../../concepts/sequencing-order.md) aanpast.
- Meer informatie over hoe u gebruikmaakt van [resourcevergrendeling in blauwdrukken](../../concepts/resource-locking.md).
- Meer informatie over hoe u [bestaande toewijzingen bijwerkt](../../how-to/update-existing-assignments.md).
