---
title: Voorbeeld - ISO 27001 blauwdruk - besturingselement toewijzing
description: De toewijzing van de controle van de blauwdruk met ISO 27001-voorbeeld.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: d487ce81babc2d1d6a35e3bdb1c13e1a24f8d1ca
ms.sourcegitcommit: 4133f375862fdbdec07b70de047d70c66ac29d50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/15/2019
ms.locfileid: "58014194"
---
# <a name="control-mapping-of-the-azure-blueprints-iso-27001-blueprint-sample"></a>Toewijzing van het voorbeeld van de blauwdruk Azure blauwdrukken ISO 27001-besturingselement

Het volgende artikel wordt uitgelegd hoe het voorbeeld van de blauwdruk Azure blauwdrukken ISO 27001 ASE/SQL-Workload wordt toegewezen aan de ISO 27001-besturingselementen. Zie voor meer informatie over de besturingselementen [ISO 27001](https://www.iso.org/isoiec-27001-information-security.html).

De volgende toewijzingen worden naar de **ISO 27001: 2013** besturingselementen. Gebruik de navigatiebalk aan de rechterkant om rechtstreeks naar de toewijzing van een bepaald besturingselement te gaan. Veel van de toegewezen besturingselementen worden geïmplementeerd met een [Azure Policy](../../../policy/overview.md) initiatief. Als u wilt controleren van de volledige initiatief, open **beleid** in Azure portal en selecteer de **definities** pagina. Vervolgens, zoek en selecteer de  **[Preview] controleren ISO 27001: 2013 besturingselementen en implementeren van specifieke VM-extensies ter ondersteuning van audit vereisten** ingebouwd beleid initiatief.

## <a name="a612-segregation-of-duties"></a>A.6.1.2 scheiding van taken

Met de eigenaar van slechts één Azure-abonnement zijn niet toegestaan voor administratieve redundantie. Met Azure-abonnement zijn te veel eigenaren kan daarentegen de mogelijkheid van een inbreuk op via een account waarmee is geknoeid eigenaar verhogen. Deze blauwdruk helpt u bij het onderhouden van een geschikt aantal eigenaars van Azure-abonnement door toe te wijzen twee [Azure Policy](../../../policy/overview.md) definities die het aantal eigenaars voor Azure-abonnementen te controleren. Eigenaarsmachtigingen abonnement beheren, kunt u de juiste scheiding van functies implementeren.

- [Preview]: Audit minimum number of owners for subscription
- [Preview]: Audit maximum number of owners for a subscription

## <a name="a912-access-to-networks-and-network-services"></a>A.9.1.2 toegang tot netwerken en -services

Azure implementeert [op rollen gebaseerd toegangsbeheer](../../../../role-based-access-control/overview.md) (RBAC) voor het beheren van wie toegang heeft tot de Azure-resources. Deze blauwdruk helpt u bij het beheren van toegang tot Azure-resources door toe te wijzen zeven [Azure Policy](../../../policy/overview.md) definities. Dit beleid gebruik van de resourcetypen controleren en configuraties die mogelijk ruimer toegang tot resources.
Informatie over resources die in strijd is met deze beleidsregels kan help u corrigerende maatregelen te nemen Zorg ervoor dat toegang tot Azure-resources is beperkt tot gemachtigde gebruikers.

- [Preview]: Deploy VM extension to audit Linux VM accounts with no passwords
- [Preview]: Deploy VM extension to audit Linux VM allowing remote connections from accounts with no
  Wachtwoordens
- [Preview]: Audit Linux VM accounts with no passwords
- [Preview]: Audit Linux VM allowing remote connections from accounts with no passwords
- Het gebruik van klassieke opslagaccounts controleren
- Het gebruik van klassieke virtuele machines controleren
- Controleer virtuele machines die niet gebruikmaken van beheerde schijven

## <a name="a922-user-access-provisioning"></a>A.9.2.2 gebruikers toegang inrichten

Azure implementeert [op rollen gebaseerd toegangsbeheer](../../../../role-based-access-control/overview.md) (RBAC) voor het beheren van wie toegang heeft tot de Azure-resources. Deze blauwdruk wordt toegewezen drie [Azure Policy](../../../policy/overview.md) definities om te controleren het gebruik van [Azure Active Directory](../../../../active-directory/fundamentals/active-directory-whatis.md) -verificatie voor SQL-Servers en [Service Fabric](../../../../service-fabric/service-fabric-overview.md). Met Azure Active Directory kunt authentication machtigingsbeheer voor vereenvoudigde en gecentraliseerde identiteitsbeheer van databasegebruikers en andere Microsoft-services. Deze blauwdruk wijst ook de definitie van een Azure-beleid om te controleren van het gebruik van aangepaste RBAC-regels. Inzicht krijgen in waar aangepaste RBAC-regels implementeren zijn kan u helpen controleren nodig en de juiste implementatie, zoals aangepaste RBAC-regels foutgevoelig zijn.

- Inrichting van een Azure Active Directory-beheerder voor SQL-server controleren
- Het gebruik van Azure Active Directory voor clientverificatie in Service Fabric controleren
- Het gebruik van aangepaste RBAC-regels controleren

## <a name="a923-management-of-privileged-access-rights"></a>A.9.2.3 beheer van rechten toegangsrechten

Deze blauwdruk helpt u bij het beperken en beheren van bevoegde toegang, rechten door toe te wijzen vier [Azure Policy](../../../policy/overview.md) definities om te controleren van externe accounts met de eigenaar en/of schrijfmachtigingen en accounts met de eigenaar en/of schrijfmachtigingen die geen multi-factor authentication ingeschakeld.

- [Preview]: Audit accounts with owner permissions who are not MFA enabled on a subscription
- [Preview]: Audit accounts with write permissions who are not MFA enabled on a subscription
- [Preview]: Audit external accounts with owner permissions on a subscription
- [Preview]: Audit external accounts with write permissions on a subscription

## <a name="a924-management-of-secret-authentication-information-of-users"></a>A.9.2.4 beheer van geheime verificatiegegevens van gebruikers

Deze blauwdruk wordt toegewezen drie [Azure Policy](../../../policy/overview.md) definities om te controleren van accounts waarvoor geen multi-factor authentication ingeschakeld. Met meervoudige verificatie kunnen accounts beveiligen, zelfs als er een stukje informatie van de verificatie is geknoeid. Door de bewaking van accounts zonder multi-factor authentication is ingeschakeld, kunt u de accounts die mogelijk meer waarschijnlijk worden aangetast identificeren. Deze blauwdruk wijst twee Azure Policy-definities die Linux-VM controleren ook de bestandsmachtigingen wachtwoord om u te waarschuwen als ze niet juist zijn ingesteld. Deze instelling kunt u corrigerende maatregelen nemen om ervoor te zorgen verificators niet worden aangetast.

- [Preview]: Audit accounts with owner permissions who are not MFA enabled on a subscription
- [Preview]: Audit accounts with read permissions who are not MFA enabled on a subscription
- [Preview]: Audit accounts with write permissions who are not MFA enabled on a subscription
- [Preview]: Deploy VM extension to audit Linux VM passwd file permissions
- [Preview]: Audit Linux VM /etc/passwd file permissions are set to 0644

## <a name="a925-review-of-user-access-rights"></a>A.9.2.5 beoordeling van de toegangsrechten voor gebruikers

Azure implementeert [op rollen gebaseerd toegangsbeheer](../../../../role-based-access-control/overview.md) (RBAC) kunt u beheren wie toegang heeft tot resources in Azure. Met behulp van de Azure-portal, kunt u controleren wie toegang heeft tot de Azure-resources en de bijbehorende machtigingen. Deze blauwdruk wordt toegewezen vier [Azure Policy](../../../policy/overview.md) definities om te controleren van accounts die met prioriteit moeten worden toegepast voor evaluatie, waaronder afgeschreven accounts en externe accounts met verhoogde bevoegdheden.

- [Preview]: Audit deprecated accounts on a subscription
- [Preview]: Audit deprecated accounts with owner permissions on a subscription
- [Preview]: Audit external accounts with owner permissions on a subscription
- [Preview]: Audit external accounts with write permissions on a subscription

## <a name="a926-removal-or-adjustment-of-access-rights"></a>A.9.2.6 verwijderen of aanpassing van de toegangsrechten

Azure implementeert [op rollen gebaseerd toegangsbeheer](../../../../role-based-access-control/overview.md) (RBAC) kunt u beheren wie toegang heeft tot resources in Azure. Met behulp van [Azure Active Directory](../../../../active-directory/fundamentals/active-directory-whatis.md) en RBAC, kunt u gebruikersrollen als gevolg van organisatorische wijzigingen bijwerken. Wanneer dat nodig is, kunnen accounts worden geblokkeerd voor aanmelden (of verwijderd), waarbij rechten voor het Azure-resources onmiddellijk worden verwijderd. Deze blauwdruk wijst twee [Azure Policy](../../../policy/overview.md) definities om te controleren afgeschreven account dat moet worden overwogen voor verwijdering.

- [Preview]: Audit deprecated accounts on a subscription
- [Preview]: Audit deprecated accounts with owner permissions on a subscription

## <a name="a943-password-management-system"></a>A.9.4.3 wachtwoord management-systeem

Deze blauwdruk helpt u bij het afdwingen van sterke wachtwoorden door toe te wijzen 10 [Azure Policy](../../../policy/overview.md) definities die controle uitvoeren op Windows-VM's die niet van minimaal sterkte en andere vereisten voor wachtwoorden afdwingen. Kennis van virtuele machines in strijd is met het wachtwoordbeleid sterkte helpt u corrigerende maatregelen nemen om ervoor te zorgen wachtwoorden voor alle VM-gebruikersaccounts zijn compatibel met het beleid.

- [Preview]: Deploy VM extension to audit Windows VM enforces password complexity requirements
- [Preview]: Deploy VM extension to audit Windows VM maximum password age 70 days
- [Preview]: Deploy VM extension to audit Windows VM minimum password age 1 day
- [Preview]: Deploy VM extension to audit Windows VM passwords must be at least 14 characters
- [Preview]: Deploy VM extension to audit Windows VM should not allow previous 24 passwords
- [Preview]: Audit Windows VM enforces password complexity requirements
- [Preview]: Audit Windows VM maximum password age 70 days
- [Preview]: Audit Windows VM minimum password age 1 day
- [Preview]: Audit Windows VM passwords must be at least 14 characters
- [Preview]: Audit Windows VM should not allow previous 24 passwords

## <a name="a1011-policy-on-the-use-of-cryptographic-controls"></a>A.10.1.1 beleid op het gebruik van cryptografische besturingselementen

Deze blauwdruk helpt u bij het afdwingen van het beleid voor het gebruik van cryptograph besturingselementen door toe te wijzen 13 [Azure Policy](../../../policy/overview.md) definities die specifiek cryptograph besturingselementen en audit afdwingen van zwakke cryptografische instellingen gebruiken.
Inzicht krijgen in waar uw Azure-resources mogelijk niet-optimale cryptografische configuraties kunt u corrigerende maatregelen nemen om ervoor te zorgen bronnen worden geconfigureerd in overeenstemming met het beveiligingsbeleid van uw gegevens. Met name vereisen de beleidsregels die zijn toegewezen door deze blauwdruk versleuteling voor blob storage-accounts en data lake storage-accounts; transparante gegevensversleuteling in SQL-databases; vereisen controle uitvoeren op ontbrekende versleuteling op storage-accounts, SQL-databases, virtuele-machineschijven en variabelen van automation-account. onbeveiligde verbindingen met de storage-accounts, functie-Apps, Web-App, API-Apps en Redis Cache controleren controle van zwakke virtuele machine wachtwoordversleuteling; en niet-versleutelde Service Fabric-communicatie.

- [Preview]: Audit HTTPS only access for a Function App
- [Preview]: Audit HTTPS only access for a Web Application
- [Preview]: Audit HTTPS only access for an API App
- [Preview]: Audit missing blob encryption for storage accounts
- [Preview]: Deploy VM extension to audit Windows VM should not store passwords using reversible
  versleutelingn
- [Preview]: Audit Windows VM should not store passwords using reversible encryption
- [Preview]: Monitor unencrypted SQL database in Azure Security Center
- [Preview]: Monitor unencrypted VM Disks in Azure Security Center
- Inschakelen van versleuteling van Automation-accountvariabelen controleren
- Inschakelen van uitsluitend beveiligde verbindingen met Redis Cache controleren
- Veilige overdracht naar opslagaccounts controleren
- Controle op de instelling van de eigenschap ClusterProtectionLevel op EncryptAndSign in Service Fabric
- Controlestatus van transparante gegevensversleuteling

## <a name="a1241-event-logging"></a>A.12.4.1-logboekregistratie

Deze blauwdruk kunt u ervoor zorgen systeemgebeurtenissen worden vastgelegd door toe te wijzen zeven [Azure Policy](../../../policy/overview.md) definities die die controle-instellingen op de Azure-resources. Een toegewezen beleid controleert ook als virtuele machines worden niet logboeken te naar een opgegeven log analytics-werkruimte verzenden.

- [Preview]: Implementatie van de afhankelijkheid Agent - VM-installatiekopie (OS) niet-vermelde controleren
- [Preview]: Implementatie van de Agent in VMSS - VM-installatiekopie (OS) niet-vermelde afhankelijkheden controleren
- [Preview]: Implementatie in de Log Analytics-Agent - VM-installatiekopie (OS) niet-vermelde controleren
- [Preview]: Audit Log Analytics-Agent-implementatie in VMSS - VM-installatiekopie (OS) niet-vermelde
- [Preview]: Monitor unaudited SQL database in Azure Security Center
- Diagnostische instelling voor controleren
- Instellingen voor SQL-controle op serverniveau

## <a name="a121-management-of-technical-vulnerabilities"></a>Beheer van A.12.1 van technische problemen

Deze blauwdruk helpt bij het beheren van gegevens system beveiligingsproblemen door toe te wijzen vijf [Azure Policy](../../../policy/overview.md) definities die ontbrekende systeemupdates, beveiligingsproblemen van besturingssystemen, SQL-beveiligingsproblemen en virtuele machine controleren door beveiligingslekken. Deze inzichten bieden realtime informatie over de beveiligingsstatus van uw geïmplementeerde resources en kunnen u herstelacties prioriteren.

- [Preview]: Monitor missing Endpoint Protection in Azure Security Center
- [Preview]: Monitor missing system updates in Azure Security Center
- [Preview]: Monitor OS vulnerabilities in Azure Security Center
- [Preview]: Monitor SQL vulnerability assessment results in Azure Security Center
- [Preview]: Monitor VM Vulnerabilities in Azure Security Center

## <a name="a1262-restrictions-on-software-installation"></a>A.12.6.2 beperkingen met betrekking tot software-installatie

Adaptieve toepassingsbesturingselementen is een oplossing van Azure Security Center die helpt u bepalen welke toepassingen kunnen worden uitgevoerd op uw virtuele machines die zich in Azure. Deze blauwdruk wijst een definitie van een Azure-beleid waarmee wijzigingen in de set met toegestane toepassingen worden gecontroleerd. Beperkingen met betrekking tot software-installatie kunt u de kans van de invoering van beveiligingsonderzoekers.

- [Preview]: Monitor possible app Whitelisting in Azure Security Center

## <a name="a1311-network-controls"></a>Hiermee bepaalt u A.13.1.1 netwerk

Deze blauwdruk helpt u bij het beheren en netwerken toe te wijzen een [Azure Policy](../../../policy/overview.md) definitie waarmee netwerkbeveiligingsgroepen met ruime regels worden gecontroleerd. Regels die te ruime zijn onbedoelde netwerktoegang mogelijk en moeten worden gecontroleerd.

- [Preview]: Monitor permissive network access in Azure Security Center

## <a name="a1321-information-transfer-policies-and-procedures"></a>A.13.2.1 informatie-overdracht beleid en procedures

De blauwdruk kunt u ervoor zorgen informatie mag geen overdracht met Azure-services is beveiligd door toe te wijzen twee [Azure Policy](../../../policy/overview.md) definities onveilig verbindingen met de storage-accounts en Redis Cache controleren.

- Inschakelen van uitsluitend beveiligde verbindingen met Redis Cache controleren
- Veilige overdracht naar opslagaccounts controleren

## <a name="a1413-protecting-application-services-transactions"></a>A.14.1.3 toepassingsservices Protecting transacties

Deze blauwdruk helpt u bij het systeem gegevensassets beschermen door toe te wijzen drie [Azure Policy](../../../policy/overview.md) definities die niet-beveiligde eindpunten, toepassingen en storage-accounts bewaken. Eindpunten en toepassingen die niet zijn beveiligd door een firewall en storage-accounts met onbeperkte toegang kunnen toestaan onbedoelde toegang tot de gegevens in het systeem voor klantinformatie.

- [Preview]: Monitor unprotected network endpoints in Azure Security Center
- [Preview]: Monitor unprotected web application in Azure Security Center
- Onbeperkte netwerktoegang tot opslagaccounts controleren

## <a name="a1613-reporting-information-security-weaknesses"></a>A.16.1.3 rapportage information security zwakke punten

Deze blauwdruk helpt u bij het bewustzijn van systeem beveiligingsproblemen onderhouden door toe te wijzen vijf [Azure Policy](../../../policy/overview.md) definities die beveiligingsproblemen, patch en waarschuwingen voor schadelijke software in Azure Security Center bewaken. Azure Security Center biedt rapportagemogelijkheden waarmee u realtime inzicht in de beveiligingsstatus van geïmplementeerde Azure-resources.

- [Preview]: Monitor missing Endpoint Protection in Azure Security Center
- [Preview]: Monitor missing system updates in Azure Security Center
- [Preview]: Monitor OS vulnerabilities in Azure Security Center
- [Preview]: Monitor SQL vulnerability assessment results in Azure Security Center
- [Preview]: Monitor VM Vulnerabilities in Azure Security Center

## <a name="next-steps"></a>Volgende stappen

Nu dat u de toewijzing van het besturingselement van het voorbeeld met ISO 27001-App Service-omgeving/SQL Database-workload blauwdruk hebt doorgenomen, gaat u naar de volgende artikelen voor meer informatie over de architectuur en over het implementeren van dit voorbeeld:

> [!div class="nextstepaction"]
> [ISO 27001-App Service-omgeving/SQL Database-workload blauwdruk - overzicht](./index.md)
> [ISO 27001-App Service Environment/SQL Database-workload blauwdruk - stappen implementeren](./deploy.md)

Toevoeging artikelen over blauwdrukken en hoe ze te gebruiken:

- Meer informatie over de [blauwdruk levenscyclus](../../concepts/lifecycle.md).
- Meer informatie over het gebruik van [statische en dynamische parameters](../../concepts/parameters.md).
- Meer informatie over het aanpassen van de [blauwdruk volgorde](../../concepts/sequencing-order.md).
- Ontdek hoe u het gebruik van [blauwdruk resource vergrendelen](../../concepts/resource-locking.md).
- Meer informatie over het [bijwerken, bestaande toewijzingen](../../how-to/update-existing-assignments.md).