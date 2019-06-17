---
title: Azure beleidsdefinities in Azure Security Center bewaakt | Microsoft Docs
description: Azure beleidsdefinities in Azure Security Center bewaakt.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: c89cb1aa-74e8-4ed1-980a-02a7a25c1a2f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/19/2019
ms.author: monhaber
ms.openlocfilehash: 25ed9cb624474d5da56d385f4e9c155918ec8eab
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66428327"
---
# <a name="azure-security-policies-monitored-by-security-center"></a>Beleid voor Azure-beveiliging bewaakt door Security Center
Dit artikel bevat een lijst met Azure Policy-definities die u in Azure Security Center kunt bewaken. Zie voor meer informatie over beveiligingsbeleid, [werken met beleidsregels voor veiligheid](tutorial-security-policy.md).

## <a name="available-security-policies"></a>Beschikbaar beleid

Zie voor meer informatie over de ingebouwde beleidsregels die worden bewaakt door Security Center, de volgende tabel:

| Beleid | Betekenis van het beleid |
| --- | --- |
|Diagnostische logboeken in Virtual Machine Scale Sets moeten worden ingeschakeld|U wordt aangeraden dat u Logboeken inschakelen zodat een audittrail activiteit is beschikbaar voor onderzoek na een incident of inbreuk op.|
|Alle autorisatieregels behalve RootManageSharedAccessKey moeten worden verwijderd uit de Event Hub-naamruimte|Azure Event Hubs-clients mag niet een toegangsbeleid op naamruimteniveau die toegang tot alle wachtrijen en onderwerpen in een naamruimte biedt gebruiken. Zodat deze overeenkomt met het beveiligingsmodel van minimale bevoegdheden, moet u toegangsbeleid maken op het entiteitsniveau van de voor wachtrijen en onderwerpen voor toegang tot alleen de specifieke entiteit.|
|Autorisatieregels voor de Event Hub-entiteit moeten worden gedefinieerd|Controle uitvoeren op de aanwezigheid van autorisatieregels voor Event Hubs-entiteiten om minimale bevoegdheden toegang te verlenen.|
|Toegang tot opslagaccounts met firewall en configuraties van het virtuele netwerk moet worden beperkt|Audit onbeperkte toegang tot het netwerk in de firewall-instellingen van uw storage-account. Netwerkregels zodanig configureren dat alleen de toepassingen van toegestane netwerken toegang het opslagaccount tot hebben. Als u wilt toestaan verbindingen met specifieke internet- of on-premises clients, toegang verlenen op verkeer van specifieke Azure-netwerken of naar het openbare internet-IP-adres bereiken.|
|Gebruik van aangepaste RBAC regels controleren|Controle van ingebouwde rollen, zoals "eigenaar, Inzender, lezer' in plaats van aangepaste role-based access control (RBAC) rollen, die foutgevoelig zijn. Gebruik van aangepaste rollen wordt beschouwd als een uitzondering en strenge controleren met bijbehorend risicomodel vereist.|
|Diagnostische logboeken in Azure Stream Analytics moeten worden ingeschakeld|Audit logs in te schakelen en houd ze voor maximaal een jaar. Hiermee maakt u activiteit sporen voor onderzoek als een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Veilige overdracht naar storage-accounts moet worden ingeschakeld|Controle van vereisten van veilige overdracht in uw opslagaccount. Veilige overdracht is een optie die ervoor zorgt dat uw storage-account om te accepteren van aanvragen van beveiligde verbindingen (HTTPS). Gebruik van HTTPS zorgt ervoor dat de verificatie tussen de server en de service. Het ook beveiligt gegevens die onderweg zijn van netwerklaagaanvallen, zoals man-in-the-middle, niet kan worden afgeluisterd en sessie-hijacking.|
|Azure AD-beheerder voor SQL server moet worden ingericht|Controle van de beheerder van een Azure Active Directory (Azure AD) voor SQL Server voor het inschakelen van Azure AD-verificatie wordt ingericht. Azure AD-verificatie biedt ondersteuning voor beheer van machtigingen vereenvoudigde en gecentraliseerde identiteitsbeheer van databasegebruikers en andere Microsoft-services.|
|Alle autorisatieregels behalve RootManageSharedAccessKey moeten worden verwijderd van Service Bus-naamruimte|Azure Service Bus-clients mag niet een toegangsbeleid op naamruimteniveau die toegang tot alle wachtrijen en onderwerpen in een naamruimte biedt gebruiken. Zodat deze overeenkomt met het beveiligingsmodel van minimale bevoegdheden, toegangsbeleid te maken op het entiteitsniveau van de voor wachtrijen en onderwerpen voor toegang tot alleen de specifieke entiteit.|
|Diagnostische logboeken in Service Bus moeten worden ingeschakeld|Audit logs in te schakelen en houd ze voor een jaar. Hiermee maakt u activiteit sporen voor onderzoek als een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|De eigenschap ClusterProtectionLevel EncryptAndSign in Service Fabric moet worden ingesteld|Service Fabric biedt drie niveaus van beveiliging voor communicatie met knooppunt-naar-knooppunt die gebruikmaakt van een primaire clustercertificaat: None, aanmelding en EncryptAndSign. Stel het beveiligingsniveau om ervoor te zorgen dat alle berichten van knooppunt-naar-knooppunt zijn versleuteld en digitaal zijn ondertekend.|
|Clientverificatie moet Azure Active Directory gebruiken|Het gebruik van clientverificatie alleen via Azure AD in Service Fabric controleren.|
|Diagnostische logboeken in Search-services moeten worden ingeschakeld|Audit logs in te schakelen en houd ze voor maximaal een jaar. Hiermee maakt u activiteit sporen voor onderzoek als een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Alleen beveiligde verbindingen met uw Redis-Cache moeten worden ingeschakeld|Controle inschakelen van verbindingen met SSL-beveiliging met Azure-Cache voor Redis. Gebruik van beveiligde verbindingen zorgt ervoor dat de verificatie tussen de server en de service. Het ook beveiligt gegevens die onderweg zijn van netwerklaagaanvallen, zoals man-in-the-middle, niet kan worden afgeluisterd en sessie-hijacking.|
|Diagnostische logboeken in Logic Apps moeten worden ingeschakeld|Audit logs in te schakelen en houd ze voor maximaal een jaar. Hiermee maakt u activiteit sporen voor onderzoek als een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Diagnostische logboeken in Key Vault moeten worden ingeschakeld|Audit logs in te schakelen en houd ze voor maximaal een jaar. Hiermee maakt u activiteit sporen voor onderzoek als een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Diagnostische logboeken in Event Hub moeten worden ingeschakeld|Audit logs in te schakelen en houd ze voor maximaal een jaar. Hiermee maakt u activiteit sporen voor onderzoek als een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Diagnostische logboeken in Azure Data Lake Store moeten worden ingeschakeld|Audit logs in te schakelen en houd ze maximaal een jaar. Hiermee maakt u activiteit sporen voor onderzoek als een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Diagnostische logboeken in Data Lake Analytics moeten worden ingeschakeld|Audit logs in te schakelen en houd ze voor maximaal een jaar. Hiermee maakt u activiteit sporen voor onderzoek als een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Storage-accounts moeten worden gemigreerd naar nieuwe AzureRM-resources|Gebruik Azure Resource Manager voor uw storage-accounts voor verbeterde beveiliging. Deze omvatten: <br>-Sterkere toegangsbeheer (RBAC)<br>-Beter controleren<br>-Azure-Resource Manager gebaseerde implementatie en beheer<br>-Toegang tot beheerde identiteiten<br>-Toegang tot Azure Key Vault voor geheimen<br>-Azure AD-verificatie<br>-Ondersteuning voor labels en resourcegroepen voor eenvoudiger beveiligingsbeheer|
|Virtuele machines moeten worden gemigreerd naar nieuwe AzureRM-resources|Gebruik Azure Resource Manager voor uw virtuele machines voor verbeterde beveiliging.  Deze omvatten: <br>-Sterkere toegangsbeheer (RBAC)<br>-Beter controleren<br>-Azure-Resource Manager gebaseerde implementatie en beheer<br>-Toegang tot beheerde identiteiten<br>-Toegang tot Azure Key Vault voor geheimen<br>-Azure AD-verificatie<br>-Ondersteuning voor labels en resourcegroepen voor eenvoudiger beveiligingsbeheer|
|Waarschuwingsregels voor metrische gegevens moeten worden geconfigureerd op Batch-accounts|Configuratie van de waarschuwingsregels voor metrische gegevens gebruik op Azure Batch-accounts om in te schakelen van de vereiste metrische gegevens controleren.|
|Logboeken met diagnostische gegevens in de Batch-accounts moeten worden ingeschakeld|Audit logs in te schakelen en houd ze voor maximaal een jaar. Hiermee maakt u activiteit sporen voor onderzoek als een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Versleuteling moet zijn ingeschakeld op de Automation-account-variabelen|Het is belangrijk om in te schakelen van versleuteling van variabele assets van Azure Automation-account wanneer u gevoelige gegevens opslaat.|
|Diagnostische logboeken in App Services moeten worden ingeschakeld|Controle inschakelen van diagnostische logboeken op de app. Hiermee maakt u activiteit sporen voor onderzoek als een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|De Transparent Data Encryption voor SQL-databases moet zijn ingeschakeld|Controlestatus van transparante gegevensversleuteling voor SQL-databases.|
|Controleren voor SQL server moet worden ingeschakeld|Controle uitvoeren op de aanwezigheid van de SQL-controle op serverniveau.|
|\[Preview-versie]: Niet-versleutelde SQL-database in Azure Security Center bewaken|Niet-versleutelde SQL-servers of databases zoals aanbevolen worden bewaakt door Azure Security Center.|
|\[Preview-versie]: Niet-gecontroleerde SQL-database in Azure Security Center bewaken|SQL-servers en databases waarvoor geen SQL-controles zijn ingeschakeld op zoals aanbevolen worden bewaakt door Azure Security Center.|
|\[Preview-versie]: Systeemupdates moeten worden geïnstalleerd op uw virtuele machines|Azure Security Center bewaakt ontbrekende beveiligingssysteemupdates op uw servers, zoals aanbevolen.|
|\[Preview-versie]: Controle uitvoeren op ontbrekende blobversleuteling voor opslagaccounts|Controle uitvoeren op opslagaccounts die niet gebruikmaken van blob-versleuteling. Dit is alleen van toepassing op Microsoft storage resourcetypen, niet de opslag van andere providers. Azure Security Center bewaakt mogelijke just-in-time-netwerktoegang, zoals aanbevolen.|
|\[Preview-versie]: Just-In-Time-netwerktoegangsbeheer moet worden toegepast op virtuele machines|Azure Security Center bewaakt mogelijke just-in-time-netwerktoegang, zoals aanbevolen.|
|\[Preview-versie]: Besturingselementen voor adaptieve toepassingen moeten zijn ingeschakeld op virtuele machines|Whitelistconfiguratie voor mogelijke toepassing wordt bewaakt door Azure Security Center.|
|\[Preview-versie]: Ontbrekende Netwerkbeveiligingsgroepen voor virtuele machines moet worden geconfigureerd|Azure Security Center bewaakt netwerkbeveiligingsgroepen met te ruime regels, zoals aanbevolen.|
|\[Preview-versie]: Door beveiligingslekken in de beveiligingsconfiguratie van de op uw computers moeten worden hersteld.|Servers die niet voldoen aan de basislijn die is geconfigureerd, zoals aanbevolen wordt bewaakt door Azure Security Center.| 
|\[Preview-versie]: Endpoint protection moet worden geïnstalleerd op virtuele machines|Servers die een geïnstalleerde Microsoft System Center Endpoint Protection-agent niet hebt, zoals aanbevolen wordt bewaakt door Azure Security Center.|
|\[Preview-versie]: Schijfversleuteling moet worden toegepast op virtuele machines|Virtuele machines waarvoor geen schijfversleuteling is ingeschakeld, zoals aanbevolen wordt bewaakt door Azure Security Center.|
|\[Preview-versie]: Beveiligingsproblemen moeten worden hersteld door een oplossing voor evaluatie van beveiligingsproblemen|Zwakke plekken die zijn gedetecteerd door de oplossing voor evaluatie van beveiligingsproblemen en virtuele machines die geen een oplossing voor evaluatie van beveiligingsproblemen hebben zoals aanbevolen in Azure Security Center bewaken.|
|\[Preview-versie]: Niet-beveiligde web-App in Azure Security Center bewaken|Azure Security Center bewaakt webtoepassingen zonder web application firewall-beveiliging, zoals aanbevolen.|
|\[Preview-versie]: Oplossing voor eindpuntbeveiliging moet worden geïnstalleerd op virtuele machines|Azure Security Center bewaakt netwerk eindpunten waarvoor geen volgende generatie firewall-beveiliging zoals aanbevolen.|
|\[Preview-versie]: Door beveiligingslekken in uw SQL-databases moeten worden hersteld.|Evaluatie van beveiligingsproblemen monitor scanresultaten en het herstellen van databaseproblemen raadzaam.|
|\[Preview-versie]: Maximaal 3 eigenaars moet worden aangeduid voor uw abonnement|Het is raadzaam dat u maximaal drie abonnementseigenaren te verminderen van de mogelijkheden voor inbreuk op een gecomprimeerde eigenaar toewijzen.|
|\[Preview-versie]: Er moet meer dan één eigenaar toegewezen aan uw abonnement|Het wordt aangeraden dat u meer dan één abonnementseigenaar om ervoor te zorgen toegang als beheerder aanwijzen redundantie.|
|\[Preview-versie]: MFA moet worden ingeschakeld voor accounts met eigenaarsmachtigingen voor uw abonnement |Multi-factor authentication (MFA) moet worden ingeschakeld voor alle abonnementsaccounts met eigenaarsmachtigingen om te voorkomen dat er inbreuk wordt gepleegd accounts of resources.|
|\[Preview-versie]: MFA moet worden ingeschakeld voor uw abonnementsaccounts met schrijfmachtigingen|Multi-factor authentication moet worden ingeschakeld voor alle abonnementsaccounts met schrijfmachtigingen om te voorkomen dat inbreuk wordt gepleegd op accounts of resources.|
|\[Preview-versie]: MFA moet worden ingeschakeld voor uw abonnementsaccounts met leesmachtigingen|Multi-factor authentication moet worden ingeschakeld voor alle abonnementsaccounts die leesmachtigingen hebben om te voorkomen dat inbreuk wordt gepleegd op accounts of resources.|
|\[Preview-versie]: Afgeschafte accounts met eigenaarsmachtigingen moeten worden verwijderd uit uw abonnement|Afgeschafte accounts met eigenaarsmachtigingen moeten worden verwijderd van uw abonnement. Afgeschafte accounts zijn zich geblokkeerd.|
|\[Preview-versie]: Afgeschafte accounts moeten worden verwijderd uit uw abonnement|Afgeschafte accounts moeten worden verwijderd uit uw abonnementen. Afgeschafte accounts zijn zich geblokkeerd.|
|\[Preview-versie]: Externe accounts met eigenaarsmachtigingen moeten worden verwijderd van uw abonnement|Externe accounts met eigenaarsmachtigingen moeten worden verwijderd uit uw abonnement om te voorkomen dat machtigingen voor toegang.|
|\[Preview-versie]: Externe accounts met schrijfmachtigingen moeten worden verwijderd uit uw abonnement|Externe accounts die zijn geschreven machtigingen moeten worden verwijderd uit uw abonnement om onbewaakte toegang te voorkomen.|
|\[Preview-versie]: Externe accounts met leesmachtigingen moeten worden verwijderd van uw abonnement|Externe accounts die u hebt leesmachtigingen moeten worden verwijderd uit uw abonnement om onbewaakte toegang te voorkomen.|




## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u kunnen lezen hoe u het beveiligingsbeleid in Security Center configureert. Zie de volgende artikelen voor meer informatie over Security Center.

* [Azure Security Center planning- en bedieningsgids voor](security-center-planning-and-operations-guide.md): Informatie over het plannen en ontwerpoverwegingen in Azure Security Center kennen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): Meer informatie over het controleren van de status van uw Azure-resources.
* [Beheren en erop reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md): Informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): Meer informatie over het bewaken van de status van uw partneroplossingen.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md): Krijg antwoorden op veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/): Raadpleeg de blogberichten over beveiliging en naleving in Azure.

Zie voor meer informatie over Azure Policy, [wat is Azure Policy?](../governance/policy/overview.md).
