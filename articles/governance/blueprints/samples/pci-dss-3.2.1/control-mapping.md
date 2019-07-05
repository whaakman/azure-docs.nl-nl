---
title: Voorbeeld - PCI-DSS v3.2.1 blauwdruk - besturingselement toewijzing
description: De toewijzing van de controle van de Payment Card Industry Data Security Standard v3.2.1 blauwdruk voorbeeld Azure-beleid en RBAC.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/24/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 38b1cc6249da98e11167416c8e18d06de1645679
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67540950"
---
# <a name="control-mapping-of-the-pci-dss-v321-blueprint-sample"></a>Toewijzing van de controle van het voorbeeld van PCI-DSS v3.2.1 blauwdruk

Het volgende artikel wordt uitgelegd hoe het voorbeeld van Azure blauwdrukken PCI-DSS v3.2.1 blauwdruk wordt toegewezen aan de PCI-DSS v3.2.1 besturingselementen. Zie voor meer informatie over de besturingselementen [PCI-DSS v3.2.1](https://www.pcisecuritystandards.org/documents/PCI_DSS_v3-2-1.pdf).

De volgende toewijzingen worden naar de **PCI-DSS v3.2.1:2018** besturingselementen. Gebruik de navigatiebalk aan de rechterkant om rechtstreeks naar de toewijzing van een bepaald besturingselement te gaan. Veel van de toegewezen besturingselementen worden geïmplementeerd met een [Azure Policy](../../../policy/overview.md) initiatief. Als u wilt controleren van de volledige initiatief, open **beleid** in Azure portal en selecteer de **definities** pagina. Vervolgens, zoek en selecteer de  **[Preview] controleren PCI v3.2.1:2018 besturingselementen en implementeren van specifieke VM-extensies ter ondersteuning van audit vereisten** ingebouwd beleid initiatief.

## <a name="132-and-134-boundary-protection"></a>1.3.2 en 1.3.4 grens-beveiliging

Deze blauwdruk helpt u bij het beheren en netwerken toe te wijzen [Azure Policy](../../../policy/overview.md) definities waarmee netwerkbeveiligingsgroepen met ruime regels worden gecontroleerd. Regels die te ruime zijn onbedoelde netwerktoegang mogelijk en moeten worden gecontroleerd. Deze blauwdruk wijst een Azure Policy-definities die niet-beveiligde eindpunten, toepassingen en storage-accounts bewaken. Eindpunten en toepassingen die niet zijn beveiligd door een firewall en storage-accounts met onbeperkte toegang kunnen toestaan onbedoelde toegang tot de gegevens in het systeem voor klantinformatie.

- Onbeperkte netwerktoegang tot de storage-accounts controleren
- Toegang via internetgerichte eindpunt moet worden beperkt

## <a name="34a-41-41g-41h-and-653-cryptographic-protection"></a>3.4.a, 4.1, 4.1.g, 4.1.h en 6.5.3 cryptografische beveiliging

Deze blauwdruk helpt u bij het afdwingen van uw beleid met het gebruik van cryptograph besturingselementen toe te wijzen [Azure Policy](../../../policy/overview.md) definities die afdwingen van specifieke cryptograph besturingselementen en controle van zwakke cryptografische instellingen gebruiken. Inzicht krijgen in waar uw Azure-resources mogelijk niet-optimale cryptografische configuraties kunt u corrigerende maatregelen nemen om ervoor te zorgen bronnen worden geconfigureerd in overeenstemming met het beveiligingsbeleid van uw gegevens. Met name vereisen de beleidsregels die zijn toegewezen door deze blauwdruk transparante gegevensversleuteling in SQL-databases; controle uitvoeren op ontbrekende versleuteling op storage-accounts en automation-account-variabelen. Er zijn ook beleidsregels welk adres onveilig verbindingen met de storage-accounts, functie-Apps, Web-App, API-Apps en Redis Cache controleren en analyseren van niet-versleutelde Service Fabric-communicatie.

- Functie-App moet alleen toegankelijk zijn via HTTPS
- Web-App moet alleen toegankelijk zijn via HTTPS
- API-App moet alleen toegankelijk zijn via HTTPS
- Niet-versleutelde SQL-database in Azure Security Center bewaken
- Schijfversleuteling moet worden toegepast op virtuele machines
- Automation-account variabelen moeten worden versleuteld.
- Alleen beveiligde verbindingen met uw Redis-Cache moeten worden ingeschakeld
- Veilige overdracht naar storage-accounts moet worden ingeschakeld
- Service Fabric-clusters moet de eigenschap ClusterProtectionLevel is ingesteld op EncryptAndSign
- De Transparent Data Encryption voor SQL-databases moet zijn ingeschakeld
- Transparent data encryption voor SQL-database implementeren

## <a name="51-62-66-and-1121-vulnerability-scanning-and-system-updates"></a>5.1, 6.2, 6.6 en 11.2.1 scannen op beveiligingsproblemen en systeemupdates

Deze blauwdruk helpt bij het beheren van gegevens system beveiligingsproblemen door toe te wijzen [Azure Policy](../../../policy/overview.md) definities die ontbrekende systeemupdates, beveiligingsproblemen van besturingssystemen, SQL-beveiligingsproblemen en virtuele machine controleren door beveiligingslekken in Azure Security Center. Azure Security Center biedt rapportagemogelijkheden waarmee u realtime inzicht in de beveiligingsstatus van geïmplementeerde Azure-resources.

- Ontbrekende Endpoint Protection bewaken in Azure Security Center
- Standaard Microsoft IaaSAntimalware-uitbreiding voor Windows Server implementeren
- Detectie van bedreigingen op SQL-Servers implementeren
- Systeemupdates moeten worden geïnstalleerd op uw virtuele machines
- Door beveiligingslekken in de beveiligingsconfiguratie van de op uw computers moeten worden hersteld.
- Door beveiligingslekken in uw SQL-databases moeten worden hersteld.
- Beveiligingsproblemen moeten worden hersteld door een oplossing voor evaluatie van beveiligingsproblemen

## <a name="711-712-and-713-separation-of-duties"></a>7.1.1. 7.1.2 en 7.1.3 scheiding van functies

Met de eigenaar van slechts één Azure-abonnement zijn niet toegestaan voor administratieve redundantie. Met Azure-abonnement zijn te veel eigenaren kan daarentegen de mogelijkheid van een inbreuk op via een account waarmee is geknoeid eigenaar verhogen. Deze blauwdruk helpt u bij het onderhouden van een geschikt aantal eigenaars van Azure-abonnement toe te wijzen [Azure Policy](../../../policy/overview.md) definities die het aantal eigenaars voor Azure-abonnementen te controleren. Eigenaarsmachtigingen abonnement beheren, kunt u de juiste scheiding van functies implementeren.

- Er moet meer dan één eigenaar toegewezen aan uw abonnement
- Maximaal 3 eigenaars moet worden aangeduid voor uw abonnement 

## <a name="32-721-831a-and-831b-management-of-privileged-access-rights"></a>3.2, 7.2.1, 8.3.1.a en 8.3.1.b beheer van rechten voor bevoorrechte toegang

Deze blauwdruk helpt u bij het beperken en beheren van bevoegde toegang, rechten toe te wijzen [Azure Policy](../../../policy/overview.md) definities om te controleren van externe accounts met de eigenaar, schrijven en/of machtigingen en werknemersacounts worden gelezen met de eigenaar en/of schrijven machtigingen die u geen multi-factor authentication ingeschakeld hebt. De implementatie van Azure op rollen gebaseerd toegangsbeheer (RBAC) voor het beheren van wie toegang heeft tot Azure-resources. Inzicht krijgen in waar aangepaste RBAC-regels implementeren zijn kan u helpen controleren nodig en de juiste implementatie, zoals aangepaste RBAC-regels foutgevoelig zijn. Deze blauwdruk toewijzen [Azure Policy](../../../policy/overview.md) definities om te controleren van Azure Active Directory-verificatie voor SQL-Servers gebruiken. Met Azure Active Directory-verificatie vereenvoudigt het beheer van machtigingen en centraliseert het identiteitsbeheer van databasegebruikers en andere Microsoft-  
Services.
 
- Externe accounts met eigenaarsmachtigingen moeten worden verwijderd van uw abonnement
- Externe accounts met schrijfmachtigingen moeten worden verwijderd uit uw abonnement
- Externe accounts met leesmachtigingen moeten worden verwijderd van uw abonnement
- MFA moet worden ingeschakeld voor accounts met eigenaarsmachtigingen voor uw abonnement
- MFA moet worden ingeschakeld accounts met schrijfmachtigingen voor uw abonnement
- MFA moet worden ingeschakeld voor accounts met leesmachtigingen voor uw abonnement
- Een Azure Active Directory-beheerder moet worden ingericht voor SQL-servers
- Gebruik van aangepaste RBAC regels controleren

## <a name="812-and-815-least-privilege-and-review-of-user-access-rights"></a>8.1.2 en 8.1.5 minimale bevoegdheden en beoordeling van de gebruiker toegangsrechten

Op rollen gebaseerd toegangsbeheer (RBAC) kunt die u beheren wie toegang tot resources in Azure heeft de implementatie van Azure. Met behulp van de Azure-portal, kunt u controleren wie toegang heeft tot de Azure-resources en de bijbehorende machtigingen. Deze blauwdruk wordt toegewezen [Azure Policy](../../../policy/overview.md) definities om te controleren van accounts die met prioriteit moeten worden toegepast voor evaluatie, waaronder afgeschreven accounts en externe accounts met verhoogde bevoegdheden.

- Afgeschafte accounts moeten worden verwijderd uit uw abonnement
- Afgeschafte accounts met eigenaarsmachtigingen moeten worden verwijderd uit uw abonnement
- Externe accounts met eigenaarsmachtigingen moeten worden verwijderd van uw abonnement
- Externe accounts met schrijfmachtigingen moeten worden verwijderd uit uw abonnement
- Externe accounts met leesmachtigingen moeten worden verwijderd van uw abonnement

## <a name="813-removal-or-adjustment-of-access-rights"></a>8.1.3 verwijdering of aanpassing van de toegangsrechten

De implementatie van Azure op rollen gebaseerd toegangsbeheer (RBAC) kunt u beheren wie toegang heeft tot resources in Azure. U kunt gebruikersrollen om organisatorische wijzigingen weer te geven met behulp van Azure Active Directory en RBAC, bijwerken. Wanneer dat nodig is, kunnen accounts worden geblokkeerd voor aanmelden (of verwijderd), waarbij rechten voor het Azure-resources onmiddellijk worden verwijderd. Deze blauwdruk wordt toegewezen [Azure Policy](../../../policy/overview.md) definities om te controleren afgeschreven account dat moet worden overwogen voor verwijdering.

- Afgeschafte accounts moeten worden verwijderd uit uw abonnement
- Afgeschafte accounts met eigenaarsmachtigingen moeten worden verwijderd uit uw abonnement

## <a name="823ab-824ab-and-825-password-based-authentication"></a>8.2.3.a,b, 8.2.4.a,b en 8.2.5 verificatie op basis van wachtwoord

Deze blauwdruk helpt u bij het afdwingen van sterke wachtwoorden toe te wijzen [Azure Policy](../../../policy/overview.md) definities die controle uitvoeren op Windows-VM's die niet van minimaal sterkte en andere vereisten voor wachtwoorden afdwingen. Kennis van virtuele machines in strijd is met het wachtwoordbeleid sterkte helpt u corrigerende maatregelen nemen om ervoor te zorgen wachtwoorden voor alle VM-gebruikersaccounts zijn compatibel met het beleid.

- [Preview]: Audit Windows VMs that do not have a maximum password age of 70 days
- [Preview]: Deploy requirements to audit Windows VMs that do not have a maximum password age of 70 days
- [Preview]: Audit Windows VMs that do not restrict the minimum password length to 14 characters
- [Preview]: Deploy requirements to audit Windows VMs that do not restrict the minimum password length to 14 characters
- [Preview]: Audit Windows VMs that allow re-use of the previous 24 passwords
- [Preview]: Deploy requirements to audit Windows VMs that allow re-use of the previous 24 passwords

## <a name="103-and-1054-audit-generation"></a>10.3 en 10.5.4 controleren generatie

Deze blauwdruk kunt u ervoor zorgen systeemgebeurtenissen worden vastgelegd door toe te wijzen [Azure Policy](../../../policy/overview.md) definities die de instellingen voor op Azure-resources controleren.
Diagnoselogboeken bieden inzicht in bewerkingen die zijn uitgevoerd in Azure-resources. Logboeken in Azure, is afhankelijk van interne klokken aan te brengen van een record tijd gecorreleerde gebeurtenissen op resources.

- Niet-gecontroleerde SQL-servers in Azure Security Center bewaken
- Diagnostische instelling voor controleren
- SQL server niveau controle-instellingen controleren
- Controle op SQL-servers implementeren
- Storage-accounts moeten worden gemigreerd naar nieuwe Azure Resource Manager-resources
- Virtuele machines moeten worden gemigreerd naar nieuwe Azure Resource Manager-resources

## <a name="1236-and-1237-information-security"></a>12.3.6 en 12.3.7 informatiebeveiliging

Deze blauwdruk helpt u bij het beheren en regelen van uw netwerk door toe te wijzen [Azure Policy](../../../policy/overview.md) definities controleren de aanvaardbare netwerklocaties en de producten goedgekeurde bedrijf is toegestaan voor de omgeving. Deze kunnen worden aangepast door elk bedrijf via de beleidsparameters binnen elk van deze beleidsregels.

- Toegestane locaties
- Toegestane locaties voor resourcegroepen

## <a name="next-steps"></a>Volgende stappen

Nu dat u de toewijzing van het besturingselement van de PCI-DSS v3.2.1 blauwdruk hebt doorgenomen, gaat u naar de volgende artikelen voor meer informatie over het overzicht en over het implementeren van dit voorbeeld:

> [!div class="nextstepaction"]
> [PCI-DSS v3.2.1 blauwdruk - overzicht](./index.md)
> [PCI-DSS v3.2.1 blauwdruk - stappen implementeren](./deploy.md)

## <a name="addition-articles-about-blueprints-and-how-to-use-them"></a>Aanvullende artikelen over blauwdrukken en het gebruik hiervan:

- Meer informatie over de [levenscyclus van een blauwdruk](../../concepts/lifecycle.md).
- Meer informatie over hoe u [statische en dynamische parameters](../../concepts/parameters.md) gebruikt.
- Meer informatie over hoe u de [blauwdrukvolgorde](../../concepts/sequencing-order.md) aanpast.
- Meer informatie over hoe u gebruikmaakt van [resourcevergrendeling in blauwdrukken](../../concepts/resource-locking.md).
- Meer informatie over hoe u [bestaande toewijzingen bijwerkt](../../how-to/update-existing-assignments.md).
