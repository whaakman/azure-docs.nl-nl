---
title: Voorbeeld - SP NIST 800-53 R4 blauwdruk - besturingselement toewijzing
description: Toewijzing van de Serviceprovider NIST 800-53 R4 blauwdruk voorbeeld Azure-beleid en RBAC beheren.
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/24/2019
ms.topic: sample
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: b887c4e6812d201dc83465a578f71e1742e8e9cf
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342038"
---
# <a name="control-mapping-of-the-nist-sp-800-53-r4-blueprint-sample"></a>Toewijzing van de Serviceprovider NIST 800-53 R4 blauwdruk voorbeeld beheren

Het volgende artikel wordt uitgelegd hoe het voorbeeld van Azure blauwdrukken NIST SP 800-53 R4 blauwdruk wordt toegewezen aan de SP NIST 800-53 R4 besturingselementen. Zie voor meer informatie over de besturingselementen [SP NIST 800-53](https://nvd.nist.gov/800-53).

De volgende toewijzingen worden naar de **SP NIST 800-53 (Rev. 4)** besturingselementen. Gebruik de navigatiebalk aan de rechterkant om rechtstreeks naar de toewijzing van een bepaald besturingselement te gaan. Veel van de toegewezen besturingselementen worden geïmplementeerd met een [Azure Policy](../../../policy/overview.md) initiatief. Als u wilt controleren van de volledige initiatief, open **beleid** in Azure portal en selecteer de **definities** pagina. Klik, zoek en selecteer de  **[Preview]: SP NIST 800-53 R4 besturingselementen controleren en implementeren van specifieke VM-extensies ter ondersteuning van audit vereisten** ingebouwd beleid initiatief.

## <a name="ac-2-account-management"></a>AC-2-accountbeheer

Deze blauwdruk helpt u accounts die niet aan de vereisten van uw organisatie-account voldoen mogelijk bekijken. Deze blauwdruk wordt toegewezen vijf Azure Policy-definities die controle uitvoeren op externe accounts met machtigingen voor lezen, schrijven en eigenaar voor een abonnement en accounts afgeschaft. Aan de hand van de accounts gecontroleerd door deze beleidsregels, kunt u Neem passende maatregelen om ervoor te zorgen management accountvereisten wordt voldaan.

- [Preview]: Audit deprecated accounts on a subscription
- [Preview]: Audit deprecated accounts with owner permissions on a subscription
- [Preview]: Audit external accounts with owner permissions on a subscription
- [Preview]: Audit external accounts with read permissions on a subscription
- [Preview]: Audit external accounts with write permissions on a subscription

## <a name="ac-2-7-account-management--role-based-schemes"></a>Accountbeheer AC-2 (7) | Schema's op basis van rollen

Azure implementeert [op rollen gebaseerd toegangsbeheer](../../../../role-based-access-control/overview.md) (RBAC) kunt u beheren wie toegang heeft tot resources in Azure. Met behulp van de Azure-portal, kunt u controleren wie toegang heeft tot de Azure-resources en de bijbehorende machtigingen. Deze blauwdruk wijst twee [Azure Policy](../../../policy/overview.md) definities om te controleren gebruiken van Azure Active Directory-verificatie voor SQL-Servers en Service Fabric. Met Azure Active Directory kunt authentication machtigingsbeheer voor vereenvoudigde en gecentraliseerde identiteitsbeheer van databasegebruikers en andere Microsoft-services.

- Het inrichten van een Azure Active Directory-beheerder voor SQL server controleren
- Gebruik van Azure Active Directory voor clientverificatie in Service Fabric controleren

## <a name="ac-2-12-account-management--account-monitoring--atypical-usage"></a>Accountbeheer AC-2 (12) | Controle / ongewone gebruik-account

Just-in-time (JIT) virtuele machine toegang vergrendelingen binnenkomend verkeer naar virtuele machines van Azure, blootstelling aan aanvallen te verminderen terwijl eenvoudige toegang tot het verbinding maken met virtuele machines wanneer dat nodig is. Alle JIT-verzoeken voor toegang tot virtuele machines worden vastgelegd in het activiteitenlogboek zodat u kunt om te controleren op ongewone gebruik. Deze blauwdruk wijst een [Azure Policy](../../../policy/overview.md) definitie die helpt u bij het controleren van virtuele machines die just-in-time-toegang kan ondersteunen, maar nog niet is geconfigureerd.

- [Preview]: Mogelijk alleen In tijd (JIT)-toegang in Azure Security Center bewaken

## <a name="ac-4-information-flow-enforcement"></a>AC-4 informatie stroom afdwingen

Delen (CORS) om App Services-resources te worden aangevraagd vanuit een extern domein cross-origin-resources. Microsoft raadt aan dat u alleen vereiste domeinen om te communiceren met uw API, de functie en de web-apps toestaan. Deze blauwdruk wijst een [Azure Policy](../../../policy/overview.md) definitie om u te helpen bij het controleren van CORS toegangsbeperkingen van resources in Azure Security Center.
Informatie over implementaties van CORS kunt u verifiëren dat informatie stroom besturingselementen zijn geïmplementeerd.

- [Preview]: Audit CORS resource access restrictions for a Web Application

## <a name="ac-5-separation-of-duties"></a>AC-5-scheiding van functies

Met de eigenaar van slechts één Azure-abonnement zijn niet toegestaan voor administratieve redundantie. Met Azure-abonnement zijn te veel eigenaren kan daarentegen de mogelijkheid van een inbreuk op via een account waarmee is geknoeid eigenaar verhogen. Deze blauwdruk helpt u bij het onderhouden van een geschikt aantal eigenaars van Azure-abonnement door toe te wijzen twee [Azure Policy](../../../policy/overview.md) definities die het aantal eigenaars voor Azure-abonnementen te controleren. Deze Blauwdruk toegewezen vier Azure beleidsdefinities die u helpen bij het lidmaatschap van de groep Administrators op Windows-machines beheren. Beheer van de eigenaar van het abonnement en beheerdersmachtigingen voor virtuele machine, kunt u de juiste scheiding van functies implementeren.

- [Preview]: Audit maximum number of owners for a subscription
- [Preview]: Audit minimum number of owners for subscription
- Controleren of de groep Administrators in Windows virtuele machines niet van toepassing op de opgegeven leden
- Controleren of de groep Administrators in Windows-VM's de opgegeven leden bevat
- VM-extensie om te controleren dat de groep Administrators in Windows virtuele machines niet van toepassing op de opgegeven leden implementeren
- VM-extensie om te controleren dat de groep Administrators in Windows-VM's de opgegeven leden bevat implementeren

## <a name="ac-6-7-least-privilege--review-of-user-privileges"></a>De minimale bevoegdheden AC-6 (7) | Overzicht van de gebruiker heeft bevoegdheden

Azure implementeert [op rollen gebaseerd toegangsbeheer](../../../../role-based-access-control/overview.md) (RBAC) kunt u beheren wie toegang heeft tot resources in Azure. Met behulp van de Azure-portal, kunt u controleren wie toegang heeft tot de Azure-resources en de bijbehorende machtigingen. Deze blauwdruk wordt toegewezen zes [Azure Policy](../../../policy/overview.md) definities om te controleren van accounts die met prioriteit moeten worden toegepast voor controle. Deze indicatoren account bekijken, kunt u er zeker van minimale bevoegdheden besturingselementen worden geïmplementeerd.

- [Preview]: Audit maximum number of owners for a subscription
- [Preview]: Audit minimum number of owners for subscription
- Controleren of de groep Administrators in Windows virtuele machines niet van toepassing op de opgegeven leden
- Controleren of de groep Administrators in Windows-VM's de opgegeven leden bevat
- VM-extensie om te controleren dat de groep Administrators in Windows virtuele machines niet van toepassing op de opgegeven leden implementeren
- VM-extensie om te controleren dat de groep Administrators in Windows-VM's de opgegeven leden bevat implementeren

## <a name="ac-17-1-remote-access--automated-monitoring--control"></a>Externe toegang van AC-17 (1) | Geautomatiseerde bewaking / beheren

Deze blauwdruk helpt u bewaking en beheer RAS door toe te wijzen drie [Azure Policy](../../../policy/overview.md) definities voor de monitors die externe foutopsporing voor Azure App Service-toepassing is uitgeschakeld en twee beleidsdefinities die Linux controleren virtuele machines waarmee externe verbindingen van accounts zonder wachtwoorden. Deze blauwdruk wijst ook de definitie van een Azure-beleid waarmee u onbeperkte toegang tot de storage-accounts bewaken. Deze indicatoren kunt u ervoor zorgen externe toegangsmethoden voldoen aan uw beveiligingsbeleid.

- [Preview]: Audit remote debugging state for a Function App
- [Preview]: Audit remote debugging state for a Web Application
- [Preview]: Audit remote debugging state for an API App
- [Preview]: Audit that Linux VMs do not allow remote connections from accounts without passwords
- [Preview]: Deploy VM extension to audit that Linux VMs do not allow remote connections from accounts without passwords
- Onbeperkte netwerktoegang tot de storage-accounts controleren

## <a name="au-3-2-content-of-audit-records--centralized-management-of-planned-audit-record-content"></a>Inhoud van controlerecords AU-3 (2) | Gecentraliseerd beheer van geplande controle Record inhoud

Logboekgegevens die zijn verzameld door Azure Monitor is opgeslagen in een Log Analytics-werkruimte maken gecentraliseerde configuratie en beheer. Deze blauwdruk kunt u ervoor zorgen gebeurtenissen worden vastgelegd door toe te wijzen zeven [Azure Policy](../../../policy/overview.md) definities die controleren en afdwingen van de implementatie van de Log Analytics-agent op Azure virtual machines.

- [Preview]: Implementatie in de Log Analytics-Agent - VM-installatiekopie (OS) niet-vermelde controleren
- [Preview]: Audit Log Analytics-Agent-implementatie in VMSS - VM-installatiekopie (OS) niet-vermelde
- [Preview]: Audit Log Analytics Workspace for VM - Report Mismatch
- [Preview-versie]: Deploy Log Analytics Agent for Linux VM Scale Sets (VMSS)
- [Preview]: Deploy Log Analytics Agent for Linux VMs
- [Preview-versie]: Deploy Log Analytics Agent for Windows VM Scale Sets (VMSS)
- [Preview]: Deploy Log Analytics Agent for Windows VMs

## <a name="au-5-response-to-audit-processing-failures"></a>AU-5-antwoord om te controleren met verwerkingsfouten

Deze blauwdruk wordt toegewezen vijf [Azure Policy](../../../policy/overview.md) definities die controle en logboekregistratie-configuraties van gebeurtenis bewaken. Bewaking van deze configuraties kunt bieden een indicator van een systeem Controlefout of onjuiste configuratie en helpen u corrigerende maatregelen nemen.

- [Preview]: Monitor unaudited SQL servers in Azure Security Center
- Diagnostische instelling voor controleren
- Audit SQL beheerde exemplaren zonder de beveiliging van geavanceerde gegevens
- SQL server niveau controle-instellingen controleren
- SQL-servers zonder de beveiliging van geavanceerde gegevens controleren

## <a name="au-6-4-audit-review-analysis-and-reporting--central-review-and-analysis"></a>AU-6 (4) Audit controle, analyse en rapportage | Centrale controle en analyse

Logboekgegevens die zijn verzameld door Azure Monitor is opgeslagen in een Log Analytics-werkruimte inschakelen gecentraliseerde rapportage en analyse. Deze blauwdruk kunt u ervoor zorgen gebeurtenissen worden vastgelegd door toe te wijzen zeven [Azure Policy](../../../policy/overview.md) definities die controleren en afdwingen van de implementatie van de Log Analytics-agent op Azure virtual machines.

- [Preview]: Implementatie in de Log Analytics-Agent - VM-installatiekopie (OS) niet-vermelde controleren
- [Preview]: Audit Log Analytics-Agent-implementatie in VMSS - VM-installatiekopie (OS) niet-vermelde
- [Preview]: Audit Log Analytics Workspace for VM - Report Mismatch
- [Preview-versie]: Deploy Log Analytics Agent for Linux VM Scale Sets (VMSS)
- [Preview]: Deploy Log Analytics Agent for Linux VMs
- [Preview-versie]: Deploy Log Analytics Agent for Windows VM Scale Sets (VMSS)
- [Preview]: Deploy Log Analytics Agent for Windows VMs

## <a name="au-12-audit-generation"></a>AU-12-Audit genereren

Deze blauwdruk kunt u ervoor zorgen systeemgebeurtenissen worden vastgelegd door toe te wijzen 15 [Azure Policy](../../../policy/overview.md) definities die de instellingen voor op Azure-resources controleren. Deze beleidsdefinities controleren en afdwingen van de implementatie van de Log Analytics-agent op Azure virtual machines en configuratie van controle-instellingen voor andere typen Azure-resources. Deze beleidsdefinities controleren ook de configuratie van diagnostische logboeken naar het bieden van inzicht in bewerkingen die worden uitgevoerd binnen de Azure-resources. Bovendien, controle en beveiliging van geavanceerde gegevens op SQL-servers zijn geconfigureerd.

- [Preview]: Implementatie in de Log Analytics-Agent - VM-installatiekopie (OS) niet-vermelde controleren
- [Preview]: Audit Log Analytics-Agent-implementatie in VMSS - VM-installatiekopie (OS) niet-vermelde
- [Preview]: Audit Log Analytics Workspace for VM - Report Mismatch
- [Preview-versie]: Deploy Log Analytics Agent for Linux VM Scale Sets (VMSS)
- [Preview]: Deploy Log Analytics Agent for Linux VMs
- [Preview-versie]: Deploy Log Analytics Agent for Windows VM Scale Sets (VMSS)
- [Preview]: Deploy Log Analytics Agent for Windows VMs
- [Preview]: Monitor unaudited SQL servers in Azure Security Center
- Diagnostische instellingen voor Netwerkbeveiligingsgroepen toepassen
- Diagnostische instelling voor controleren
- Audit SQL beheerde exemplaren zonder de beveiliging van geavanceerde gegevens
- SQL server niveau controle-instellingen controleren
- SQL-servers zonder de beveiliging van geavanceerde gegevens controleren
- Geavanceerde beveiliging van gegevens op SQL-servers implementeren
- Controle op SQL-servers implementeren

## <a name="cm-7-2-least-functionality--prevent-program-execution"></a>Minimale functionaliteit CM-7 (2) | Voorkomen dat de programma-uitvoering

Adaptieve toepassingsbesturingselementen in Azure Security Center is een oplossing voor het in de whitelist van intelligente, geautomatiseerde end-to-end-toepassingen die kan blokkeren of te voorkomen dat specifieke software die wordt uitgevoerd op uw virtuele machines. Toepassingsbeheer kunt uitvoeren in een afdwingingsmodus die voorkomt niet-goedgekeurde toepassingen die worden uitgevoerd dat. Deze blauwdruk wijst een Azure Policy-definitie die helpt u bij het controleren van virtuele machines waarbij een toegestane toepassingen wordt aanbevolen, maar nog niet is geconfigureerd.

- [Preview]: Monitor possible app whitelisting in Azure Security Center

## <a name="cm-7-5-least-functionality--authorized-software--whitelisting"></a>Minimale CM-7 (5)-functionaliteit | Software geautoriseerd / in een whitelist opnemen

Adaptieve toepassingsbesturingselementen in Azure Security Center is een oplossing voor het in de whitelist van intelligente, geautomatiseerde end-to-end-toepassingen die kan blokkeren of te voorkomen dat specifieke software die wordt uitgevoerd op uw virtuele machines. Toepassingsbeheer helpt u bij het maken van de goedgekeurde toepassing lijsten voor uw virtuele machines. Deze blauwdruk wijst een [Azure Policy](../../../policy/overview.md) definitie die helpt u bij het controleren van virtuele machines waarbij een toegestane toepassingen wordt aanbevolen, maar nog niet is geconfigureerd.

- [Preview]: Monitor possible app whitelisting in Azure Security Center

## <a name="cm-11-user-installed-software"></a>CM-11 gebruikers geïnstalleerde Software

Adaptieve toepassingsbesturingselementen in Azure Security Center is een oplossing voor het in de whitelist van intelligente, geautomatiseerde end-to-end-toepassingen die kan blokkeren of te voorkomen dat specifieke software die wordt uitgevoerd op uw virtuele machines. Toepassingsbeheer kunt u afdwingen en naleving met beleid voor softwarebeperking controleren. Deze blauwdruk wijst een [Azure Policy](../../../policy/overview.md) definitie die helpt u bij het controleren van virtuele machines waarbij een toegestane toepassingen wordt aanbevolen, maar nog niet is geconfigureerd.

- [Preview]: Monitor possible app whitelisting in Azure Security Center

## <a name="cp-7-alternate-processing-site"></a>Alternatieve verwerking CP-7-Site

Azure Site Recovery repliceert workloads die op virtuele machines worden uitgevoerd vanaf een primaire locatie naar een secundaire locatie. Als er uitval op de primaire site optreedt, is de werkbelasting failover van de secundaire locatie. Deze blauwdruk wijst een [Azure Policy](../../../policy/overview.md) definitie die virtuele machines zonder herstel na noodgevallen geconfigureerd controles zijn uitgevoerd. Bewaking van deze indicator kunt u ervoor zorgen noodzakelijke besturingselementen voor onvoorziene gebeurtenissen is voldaan.

- Virtuele machines zonder herstel na noodgevallen geconfigureerd controleren

## <a name="ia-2-1-identification-and-authentication-organizational-users--network-access-to-privileged-accounts"></a>IA-2 (1)-identificatie en verificatie (organisatie-gebruikers) | Netwerktoegang tot beschermde Accounts

Deze blauwdruk helpt u bij het beperken en beheren van bevoegde toegang door toe te wijzen twee [Azure Policy](../../../policy/overview.md) definities voor accounts controleren met de eigenaar en/of schrijfmachtigingen waarvoor geen multi-factor authentication ingeschakeld. Met meervoudige verificatie kunnen accounts beveiligen, zelfs als er een stukje informatie van de verificatie is geknoeid. Door de bewaking van accounts zonder multi-factor authentication is ingeschakeld, kunt u de accounts die mogelijk meer waarschijnlijk worden aangetast identificeren.

- [Preview]: Audit accounts with owner permissions who are not MFA enabled on a subscription
- [Preview]: Audit accounts with write permissions who are not MFA enabled on a subscription

## <a name="ia-2-2-identification-and-authentication-organizational-users--network-access-to-non-privileged-accounts"></a>IA-2 (2)-identificatie en verificatie (organisatie-gebruikers) | Netwerktoegang tot niet-gemachtigde Accounts

Deze blauwdruk helpt u bij het beperken en toegang beheren door toe te wijzen een [Azure Policy](../../../policy/overview.md) definitie om te controleren van accounts met leesmachtigingen waarvoor geen multi-factor authentication ingeschakeld. Met meervoudige verificatie kunnen accounts beveiligen, zelfs als er een stukje informatie van de verificatie is geknoeid. Door de bewaking van accounts zonder multi-factor authentication is ingeschakeld, kunt u de accounts die mogelijk meer waarschijnlijk worden aangetast identificeren.

- [Preview]: Audit accounts with read permissions who are not MFA enabled on a subscription

## <a name="ia-5-authenticator-management"></a>IA-5-verificator Management

Deze blauwdruk wordt toegewezen vijf [Azure Policy](../../../policy/overview.md) definities die controle uitvoeren op virtuele Linux-machines die externe verbindingen van accounts zonder wachtwoorden toestaan en/of onjuiste machtigingen zijn ingesteld voor de passwd-bestand. Deze blauwdruk wijst een beleidsdefinitie die voert een controle uit de conjugatie van het type wachtwoord versleuteling voor Windows virtual machines ook toe. Bewaking van deze indicatoren zorgt die u ervoor zorgen dat systeem verificators aan de identificatie en verificatie beleid van uw organisatie voldoen.

- [Preview]: Audit that Linux VMs do not have accounts without passwords
- [Preview]: Deploy VM extension to audit that Linux VMs do not have accounts without passwords
- [Preview]: Audit that Linux VMs have the passwd file permissions set to 0644
- [Preview]: Audit that Windows VMs store passwords using reversible encryption
- [Preview]: Deploy VM extension to audit that Linux VMs have the passwd file permissions set to 0644

## <a name="ia-5-1-authenticator-management--password-based-authentication"></a>Beheer van de Authenticator IA-5 (1) | Verificatie op basis van wachtwoord

Deze blauwdruk helpt u bij het afdwingen van sterke wachtwoorden door toe te wijzen 12 [Azure Policy](../../../policy/overview.md) definities die Windows virtuele machines die niet van minimaal sterkte en andere vereisten voor wachtwoorden afdwingen controleren. Bewust te maken van virtuele machines in een overschrijding van het wachtwoordbeleid sterkte helpt u corrigerende maatregelen nemen om ervoor te zorgen wachtwoorden voor gebruikersaccounts voor alle virtuele machines te voldoen aan het wachtwoordbeleid van uw organisatie.

- [Preview]: Audit that Windows VMs cannot re-use the previous 24 passwords
- [Preview]: Audit that Windows VMs have a maximum password age of 70 days
- [Preview]: Audit that Windows VMs have a minimum password age of 1 day
- [Preview]: Audit that Windows VMs have the password complexity setting enabled
- [Preview]: Audit that Windows VMs restrict the minimum password length to 14 characters
- [Preview]: Audit that Windows VMs store passwords using reversible encryption
- [Preview]: Deploy VM extension to audit that Windows VMs cannot re-use the previous 24 passwords
- [Preview]: Deploy VM extension to audit that Windows VMs have a maximum password age of 70 days
- [Preview]: Deploy VM extension to audit that Windows VMs have a minimum password age of 1 day
- [Preview]: Deploy VM extension to audit that Windows VMs have the password complexity setting enabled
- [Preview]: Deploy VM extension to audit that Windows VMs restrict the minimum password length to 14 characters
- [Preview]: Deploy VM extension to audit that Windows VMs store passwords using reversible encryption

## <a name="ra-5-vulnerability-scanning"></a>Scannen van RA-5-beveiligingsproblemen

Deze blauwdruk helpt bij het beheren van gegevens system beveiligingsproblemen door toe te wijzen vier [Azure Policy](../../../policy/overview.md) definities die beveiligingsproblemen van besturingssystemen, SQL-beveiligingsproblemen en beveiligingsproblemen van de virtuele machine in Azure controleren Security Center. Azure Security Center biedt rapportagemogelijkheden waarmee u realtime inzicht in de beveiligingsstatus van geïmplementeerde Azure-resources. Deze blauwdruk wijst ook drie beleidsdefinities die controleren en afdwingen van geavanceerde gegevensbeveiliging voor SQL-servers. Beveiliging van geavanceerde gegevens opgenomen evaluatie van beveiligingsproblemen en geavanceerde threat protection mogelijkheden om te begrijpen van zwakke plekken in uw geïmplementeerde resources.

- Audit SQL beheerde exemplaren zonder de beveiliging van geavanceerde gegevens
- SQL-servers zonder de beveiliging van geavanceerde gegevens controleren
- Geavanceerde beveiliging van gegevens op SQL-servers implementeren
- [Preview]: Audit OS vulnerabilities on your virtual machine scale sets in Azure Security Center
- [Preview]: Monitor OS vulnerabilities in Azure Security Center
- [Preview]: Monitor SQL vulnerability assessment results in Azure Security Center
- [Preview]: Monitor VM Vulnerabilities in Azure Security Center

## <a name="sc-5-denial-of-service-protection"></a>SC-5 Denial of Service-beveiliging

Azure gedistribueerde denial-of standard-laag van service (DDoS) biedt extra functies en mogelijkheden voor risicobeperking ten opzichte van de basis-servicelaag. Deze extra functies omvatten integratie van Azure Monitor en de mogelijkheid om te na netwerkaanval rapporten te bekijken. Deze blauwdruk wijst een [Azure Policy](../../../policy/overview.md) definitie die controleert of de DDoS standard-laag is ingeschakeld. Informatie over de mogelijkheid verschil tussen de Servicelagen, kunt u de beste oplossing voor adres denial of service-beveiliging voor uw Azure-omgeving selecteren.

- [Preview]: Audit standard tier of DDoS protection is enabled for a virtual network

## <a name="sc-7-boundary-protection"></a>SC-7-grens beveiliging

Deze blauwdruk helpt u bij het beheren en besturen van de systeemgrens door toe te wijzen een [Azure Policy](../../../policy/overview.md) definitie waarmee netwerkbeveiligingsgroepen met ruime regels worden gecontroleerd. Regels die te ruime zijn onbedoelde netwerktoegang mogelijk en moeten worden gecontroleerd. Deze blauwdruk wijst ook de beleidsdefinitie van een voor network security group hardening aanbevelingen in Azure Security Center bewaakt. Azure Security Center analyseert verkeerspatronen van virtuele machines-internetservices en netwerkbeveiliging biedt aanbevelingen voor regel te verminderen het risico van aanvallen.
Deze Blauwdruk toegewezen bovendien ook drie beleidsdefinities die niet-beveiligde eindpunten, toepassingen en storage-accounts bewaken. Eindpunten en toepassingen die niet zijn beveiligd door een firewall en storage-accounts met onbeperkte toegang kunnen toestaan onbedoelde toegang tot de gegevens in het systeem voor klantinformatie.

- [Preview]: Monitor Internet-facing virtual machines for Network Security Group traffic hardening recommendations
- [Preview]: Monitor permissive network access in Azure Security Center
- [Preview]: Monitor unprotected network endpoints in Azure Security Center
- [Preview]: Monitor unprotected web application in Azure Security Center
- Onbeperkte netwerktoegang tot de storage-accounts controleren

## <a name="sc-7-3-boundary-protection--access-points"></a>SC-7 (3) grens Protection | Toegangspunten

Just-in-time (JIT) virtuele machine toegang vergrendelingen binnenkomend verkeer naar virtuele machines van Azure, blootstelling aan aanvallen te verminderen terwijl eenvoudige toegang tot het verbinding maken met virtuele machines wanneer dat nodig is. Toegang tot JIT-virtuele machine kunt u het aantal externe verbindingen met uw resources in Azure. Deze blauwdruk wijst een [Azure Policy](../../../policy/overview.md) definitie die helpt u bij het controleren van virtuele machines die just-in-time-toegang kan ondersteunen, maar nog niet is geconfigureerd.

- [Preview]: Mogelijk alleen In tijd (JIT)-toegang in Azure Security Center bewaken

## <a name="sc-7-4-boundary-protection--external-telecommunications-services"></a>SC-7 (4) grens Protection | Externe telecommunicatie

Just-in-time (JIT) virtuele machine toegang vergrendelingen binnenkomend verkeer naar virtuele machines van Azure, blootstelling aan aanvallen te verminderen terwijl eenvoudige toegang tot het verbinding maken met virtuele machines wanneer dat nodig is. Toegang tot JIT-virtuele machine kunt u uitzonderingen voor uw stroom verkeer toepassen door te vereenvoudigen, de toegang tot aanvraag- en goedkeuringsprocessen processen beheren. Deze blauwdruk wijst een [Azure Policy](../../../policy/overview.md) definitie die helpt u bij het controleren van virtuele machines die just-in-time-toegang kan ondersteunen, maar nog niet is geconfigureerd.

- [Preview]: Mogelijk alleen In tijd (JIT)-toegang in Azure Security Center bewaken

## <a name="sc-28-1-protection-of-information-at-rest--cryptographic-protection"></a>SC-28 (1) bescherming van gegevens in rust | Cryptografische beveiliging

Deze blauwdruk helpt u bij het afdwingen van het beleid voor het gebruik van besturingselementen voor het beveiligen van gegevens in rust door toe te wijzen 9 cryptograph [Azure Policy](../../../policy/overview.md) definities die specifiek cryptograph besturingselementen en audit afdwingen van zwakke cryptografische gebruiken Instellingen. Inzicht krijgen in waar uw Azure-resources mogelijk niet-optimale cryptografische configuraties kunt u corrigerende maatregelen nemen om ervoor te zorgen bronnen worden geconfigureerd in overeenstemming met het beveiligingsbeleid van uw gegevens. Met name vereisen de beleidsdefinities die is toegewezen door deze blauwdruk versleuteling voor data lake storage-accounts; transparante gegevensversleuteling in SQL-databases; vereisen en ontbrekende versleuteling voor SQL-databases, virtuele-machineschijven en automation-account-variabelen.

- [Preview]: Monitor unencrypted SQL databases in Azure Security Center
- [Preview]: Monitor unencrypted VM Disks in Azure Security Center
- Veilige overdracht naar storage-accounts controleren
- Audit SQL beheerde exemplaren zonder de beveiliging van geavanceerde gegevens
- SQL-servers zonder de beveiliging van geavanceerde gegevens controleren
- Controlestatus van transparante gegevensversleuteling
- Geavanceerde beveiliging van gegevens op SQL-servers implementeren
- Transparent data encryption voor SQL-database implementeren
- Versleuteling voor Data Lake Store-accounts wordt afgedwongen

## <a name="si-2-flaw-remediation"></a>Herstel van SI-2-fout

Deze blauwdruk helpt bij het beheren van gegevens system fouten door toe te wijzen zes [Azure Policy](../../../policy/overview.md) definities die ontbrekende systeemupdates, beveiligingsproblemen van besturingssystemen, SQL-beveiligingsproblemen en virtuele machine controleren door beveiligingslekken in Azure Security Center. Azure Security Center biedt rapportagemogelijkheden waarmee u realtime inzicht in de beveiligingsstatus van geïmplementeerde Azure-resources. Deze blauwdruk wijst een beleidsdefinitie die ervoor zorgt dat de automatische upgrade van het besturingssysteem voor virtuele-machineschaalsets ook toe.

- [Preview]: Audit any missing system updates on virtual machine scale sets in Azure Security Center
- [Preview]: Audit OS vulnerabilities on your virtual machine scale sets in Azure Security Center
- [Preview]: Monitor missing system updates in Azure Security Center
- [Preview]: Monitor OS vulnerabilities in Azure Security Center
- [Preview]: Monitor SQL vulnerability assessment results in Azure Security Center
- [Preview]: Monitor VM Vulnerabilities in Azure Security Center
- Afdwingen met controles van de toepassingsstatus in VMSS Automatische upgrade van besturingssysteem

## <a name="si-3-malicious-code-protection"></a>Schadelijke Code SI-3-beveiliging

Deze blauwdruk helpt u bij het beheren van endpoint protection, met inbegrip van schadelijke code bescherming door toe te wijzen drie [Azure Policy](../../../policy/overview.md) definities die voor ontbrekende endpoint protection op virtuele machines in Azure Security Center bewaken en afdwingen van de Microsoft antimalware-oplossing op Windows-machines.

- [Preview]: Audit the endpoint protection solution on virtual machine scale sets in Azure Security Center
- [Preview]: Monitor missing Endpoint Protection in Azure Security Center
- Standaard Microsoft IaaSAntimalware-uitbreiding voor Windows Server implementeren

## <a name="si-3-1-malicious-code-protection--central-management"></a>Bescherming van de schadelijke Code SI-3 (1) | Centraal beheer

Deze blauwdruk helpt u bij het beheren van endpoint protection, met inbegrip van schadelijke code bescherming door toe te wijzen twee [Azure Policy](../../../policy/overview.md) definities die voor ontbrekende endpoint protection op virtuele machines in Azure Security Center bewaken. Azure Security Center biedt gecentraliseerd beheer en rapportage mogelijkheden waarmee u realtime inzicht in de beveiligingsstatus van geïmplementeerde Azure-resources.

- [Preview]: Audit the endpoint protection solution on virtual machine scale sets in Azure Security Center
- [Preview]: Monitor missing Endpoint Protection in Azure Security Center

## <a name="si-4-information-system-monitoring"></a>Systeem voor klantinformatie SI-4 controleren

Deze blauwdruk helpt u bij het bewaken van uw systeem met controle en het afdwingen van logboekregistratie en beveiliging van gegevens tussen Azure-resources. Met name de beleidsregels die zijn toegewezen, controleren en afdwingen van implementatie van de Log Analytics-agent en instellingen voor verbeterde beveiliging voor SQL-databases, opslagaccounts en netwerkbronnen. Aan de hand van deze mogelijkheden kunnen u detecteren van afwijkend gedrag en indicatoren van aanvallen zodat u de passende maatregelen kunt nemen.

- [Preview]: Implementatie in de Log Analytics-Agent - VM-installatiekopie (OS) niet-vermelde controleren
- [Preview]: Audit Log Analytics-Agent-implementatie in VMSS - VM-installatiekopie (OS) niet-vermelde
- [Preview]: Audit Log Analytics Workspace for VM - Report Mismatch
- [Preview-versie]: Deploy Log Analytics Agent for Linux VM Scale Sets (VMSS)
- [Preview]: Deploy Log Analytics Agent for Linux VMs
- [Preview-versie]: Deploy Log Analytics Agent for Windows VM Scale Sets (VMSS)
- [Preview]: Deploy Log Analytics Agent for Windows VMs
- Audit SQL beheerde exemplaren zonder de beveiliging van geavanceerde gegevens
- SQL-servers zonder de beveiliging van geavanceerde gegevens controleren
- Geavanceerde beveiliging van gegevens op SQL-servers implementeren
- Advanced Threat Protection op Storage-Accounts te implementeren
- Controle op SQL-servers implementeren
- Network watcher implementeren wanneer virtuele netwerken worden gemaakt
- Detectie van bedreigingen op SQL-servers implementeren

## <a name="si-4-18-information-system-monitoring--analyze-traffic--covert-exfiltration"></a>Systeem voor klantinformatie SI-4 (18) controleren | Analyseer verkeer / Steelser Exfiltratie

Advanced Threat Protection voor Azure Storage detecteert ongebruikelijke en potentieel schadelijke pogingen om toegang tot of misbruik te maken van storage-accounts. Beveiliging waarschuwingen bevatten afwijkende toegangspatronen, afwijkende extraheert/uploaden en verdachte storage-activiteit. Deze indicatoren kunt u detecteren steelser exfiltration van gegevens.

- Advanced Threat Protection op Storage-Accounts te implementeren

## <a name="next-steps"></a>Volgende stappen

Aanvullende artikelen over blauwdrukken en het gebruik hiervan:

- Meer informatie over de [levenscyclus van een blauwdruk](../../concepts/lifecycle.md).
- Meer informatie over hoe u [statische en dynamische parameters](../../concepts/parameters.md) gebruikt.
- Meer informatie over hoe u de [blauwdrukvolgorde](../../concepts/sequencing-order.md) aanpast.
- Meer informatie over hoe u gebruikmaakt van [resourcevergrendeling in blauwdrukken](../../concepts/resource-locking.md).
- Meer informatie over hoe u [bestaande toewijzingen bijwerkt](../../how-to/update-existing-assignments.md).