---
title: Azure beleidsdefinities in Azure Security Center bewaakt | Microsoft Docs
description: Azure beleidsdefinities in Azure Security Center bewaakt.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: c89cb1aa-74e8-4ed1-980a-02a7a25c1a2f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/15/2019
ms.author: rkarlin
ms.openlocfilehash: 9d9369afd36f64c27cd2222cab0de5912aa913de
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57877493"
---
# <a name="azure-security-policies-monitored-by-security-center"></a>Beleid voor Azure-beveiliging bewaakt door Security Center
Dit artikel bevat een lijst met Azure Policy-definities die u in Azure Security Center kunt bewaken. Zie voor meer informatie over beveiligingsbeleid, [werken met beleidsregels voor veiligheid](tutorial-security-policy.md).

## <a name="available-security-policies"></a>Beschikbaar beleid

Zie voor meer informatie over de ingebouwde beleidsregels die worden bewaakt door Security Center, de volgende tabel:

| Beleid | Betekenis van het beleid |
| --- | --- |
|Controle van diagnostische gegevens inschakelen in Azure Service Fabric en virtual machine scale sets-Logboeken|U wordt aangeraden dat u Logboeken inschakelen zodat een audittrail activiteit is beschikbaar voor onderzoek na een incident of inbreuk op.|
|Autorisatieregels voor Event Hubs-naamruimten controleren|Azure Event Hubs-clients mag niet een toegangsbeleid op naamruimteniveau die toegang tot alle wachtrijen en onderwerpen in een naamruimte biedt gebruiken. Zodat deze overeenkomt met het beveiligingsmodel van minimale bevoegdheden, moet u toegangsbeleid maken op het entiteitsniveau van de voor wachtrijen en onderwerpen voor toegang tot alleen de specifieke entiteit.|
|Controle van de aanwezigheid van autorisatieregels voor Event Hubs-entiteiten|Controle uitvoeren op de aanwezigheid van autorisatieregels voor Event Hubs-entiteiten om minimale bevoegdheden toegang te verlenen.|
|Onbeperkte netwerktoegang tot opslagaccounts controleren|Audit onbeperkte toegang tot het netwerk in de firewall-instellingen van uw storage-account. Netwerkregels zodanig configureren dat alleen de toepassingen van toegestane netwerken toegang het opslagaccount tot hebben. Als u wilt toestaan verbindingen met specifieke internet- of on-premises clients, toegang verlenen op verkeer van specifieke Azure-netwerken of naar het openbare internet-IP-adres bereiken.|
|Het gebruik van aangepaste RBAC-regels controleren|Controle van ingebouwde rollen, zoals "eigenaar, Inzender, lezer' in plaats van aangepaste role-based access control (RBAC) rollen, die foutgevoelig zijn. Gebruik van aangepaste rollen wordt beschouwd als een uitzondering en strenge controleren met bijbehorend risicomodel vereist.|
|Inschakelen van diagnostische logboeken in Azure Stream Analytics controleren|Audit logs in te schakelen en houd ze voor maximaal een jaar. Hiermee maakt u activiteit sporen voor onderzoek als een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Veilige overdracht naar opslagaccounts controleren|Controle van vereisten van veilige overdracht in uw opslagaccount. Veilige overdracht is een optie die ervoor zorgt dat uw storage-account om te accepteren van aanvragen van beveiligde verbindingen (HTTPS). Gebruik van HTTPS zorgt ervoor dat de verificatie tussen de server en de service. Het ook beveiligt gegevens die onderweg zijn van netwerklaagaanvallen, zoals man-in-the-middle, niet kan worden afgeluisterd en sessie-hijacking.|
|Het inrichten van een Azure Active Directory-beheerder voor SQL Server controleren|Controle van de beheerder van een Azure Active Directory (Azure AD) voor SQL Server voor het inschakelen van Azure AD-verificatie wordt ingericht. Azure AD-verificatie biedt ondersteuning voor beheer van machtigingen vereenvoudigde en gecentraliseerde identiteitsbeheer van databasegebruikers en andere Microsoft-services.|
|Autorisatieregels voor Service Bus-naamruimten controleren|Azure Service Bus-clients mag niet een toegangsbeleid op naamruimteniveau die toegang tot alle wachtrijen en onderwerpen in een naamruimte biedt gebruiken. Zodat deze overeenkomt met het beveiligingsmodel van minimale bevoegdheden, toegangsbeleid te maken op het entiteitsniveau van de voor wachtrijen en onderwerpen voor toegang tot alleen de specifieke entiteit.|
|Inschakelen van diagnostische logboeken in Service Bus controleren|Audit logs in te schakelen en houd ze voor een jaar. Hiermee maakt u activiteit sporen voor onderzoek als een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Controle op de instelling van de eigenschap ClusterProtectionLevel op EncryptAndSign in Service Fabric|Service Fabric biedt drie niveaus van beveiliging voor communicatie met knooppunt-naar-knooppunt die gebruikmaakt van een primaire clustercertificaat: None, aanmelding en EncryptAndSign. Stel het beveiligingsniveau om ervoor te zorgen dat alle berichten van knooppunt-naar-knooppunt zijn versleuteld en digitaal zijn ondertekend.|
|Het gebruik van Azure Active Directory voor clientverificatie in Service Fabric controleren|Het gebruik van clientverificatie alleen via Azure AD in Service Fabric controleren.|
|Inschakelen van diagnostische logboeken voor Search-service controleren|Audit logs in te schakelen en houd ze voor maximaal een jaar. Hiermee maakt u activiteit sporen voor onderzoek als een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Audit alleen beveiligde verbindingen met Azure-Cache voor Redis in te schakelen|Controle inschakelen van verbindingen met SSL-beveiliging met Azure-Cache voor Redis. Gebruik van beveiligde verbindingen zorgt ervoor dat de verificatie tussen de server en de service. Het ook beveiligt gegevens die onderweg zijn van netwerklaagaanvallen, zoals man-in-the-middle, niet kan worden afgeluisterd en sessie-hijacking.|
|Inschakelen van diagnostische logboeken in Logic Apps controleren|Audit logs in te schakelen en houd ze voor maximaal een jaar. Hiermee maakt u activiteit sporen voor onderzoek als een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Inschakelen van diagnostische logboeken in Key Vault controleren|Audit logs in te schakelen en houd ze voor maximaal een jaar. Hiermee maakt u activiteit sporen voor onderzoek als een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Controle inschakelen van diagnostische logboeken in Event Hubs|Audit logs in te schakelen en houd ze voor maximaal een jaar. Hiermee maakt u activiteit sporen voor onderzoek als een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Inschakelen van diagnostische logboeken in Azure Data Lake Store controleren|Audit logs in te schakelen en houd ze maximaal een jaar. Hiermee maakt u activiteit sporen voor onderzoek als een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Inschakelen van diagnostische logboeken in Data Lake Analytics controleren|Audit logs in te schakelen en houd ze voor maximaal een jaar. Hiermee maakt u activiteit sporen voor onderzoek als een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Het gebruik van klassieke opslagaccounts controleren|Gebruik Azure Resource Manager voor uw storage-accounts voor verbeterde beveiliging. Deze omvatten: <br>-Sterkere toegangsbeheer (RBAC)<br>-Beter controleren<br>-Azure-Resource Manager gebaseerde implementatie en beheer<br>-Toegang tot beheerde identiteiten<br>-Toegang tot Azure Key Vault voor geheimen<br>-Azure AD-verificatie<br>-Ondersteuning voor labels en resourcegroepen voor eenvoudiger beveiligingsbeheer|
|Het gebruik van klassieke virtuele machines controleren|Gebruik Azure Resource Manager voor uw virtuele machines voor verbeterde beveiliging.  Deze omvatten: <br>-Sterkere toegangsbeheer (RBAC)<br>-Beter controleren<br>-Azure-Resource Manager gebaseerde implementatie en beheer<br>-Toegang tot beheerde identiteiten<br>-Toegang tot Azure Key Vault voor geheimen<br>-Azure AD-verificatie<br>-Ondersteuning voor labels en resourcegroepen voor eenvoudiger beveiligingsbeheer|
|Configuratie van metrische waarschuwingsregels in Batch-accounts controleren|Configuratie van de waarschuwingsregels voor metrische gegevens gebruik op Azure Batch-accounts om in te schakelen van de vereiste metrische gegevens controleren.|
|Inschakelen van diagnostische logboeken in Batch-accounts controleren|Audit logs in te schakelen en houd ze voor maximaal een jaar. Hiermee maakt u activiteit sporen voor onderzoek als een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Controle inschakelen van versleuteling van Automation-account-variabelen|Het is belangrijk om in te schakelen van versleuteling van variabele assets van Azure Automation-account wanneer u gevoelige gegevens opslaat.|
|Controle inschakelen van diagnostische logboeken in App Service|Controle inschakelen van diagnostische logboeken op de app. Hiermee maakt u activiteit sporen voor onderzoek als een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Controlestatus van transparante gegevensversleuteling|Controlestatus van transparante gegevensversleuteling voor SQL-databases.|
|SQL Server-niveau van controle-instellingen controleren|Controle uitvoeren op de aanwezigheid van de SQL-controle op serverniveau.|
|\[Preview-versie]: Niet-versleutelde SQL-database in Azure Security Center bewaken|Niet-versleutelde SQL-servers of databases zoals aanbevolen worden bewaakt door Azure Security Center.|
|\[Preview-versie]: Niet-gecontroleerde SQL-database in Azure Security Center bewaken|SQL-servers en databases waarvoor geen SQL-controles zijn ingeschakeld op zoals aanbevolen worden bewaakt door Azure Security Center.|
|\[Preview-versie]: Ontbrekende systeemupdates in Azure Security Center bewaken|Azure Security Center bewaakt ontbrekende beveiligingssysteemupdates op uw servers, zoals aanbevolen.|
|\[Preview-versie]: Controle uitvoeren op ontbrekende blobversleuteling voor opslagaccounts|Controle uitvoeren op opslagaccounts die niet gebruikmaken van blob-versleuteling. Dit is alleen van toepassing op Microsoft storage resourcetypen, niet de opslag van andere providers. Azure Security Center bewaakt mogelijke just-in-time-netwerktoegang, zoals aanbevolen.|
|\[Preview-versie]: Mogelijke just-in-time-netwerktoegang in Azure Security Center bewaken|Azure Security Center bewaakt mogelijke just-in-time-netwerktoegang, zoals aanbevolen.|
|\[Preview-versie]: Mogelijke opname Apps op Whitelist in Azure Security Center bewaken|Whitelistconfiguratie voor mogelijke toepassing wordt bewaakt door Azure Security Center.|
|\[Preview-versie]: Ruime netwerktoegang in Azure Security Center bewaken|Azure Security Center bewaakt netwerkbeveiligingsgroepen met te ruime regels, zoals aanbevolen.|
|\[Preview-versie]: Beveiligingsproblemen van besturingssystemen bewaken in Azure Security Center|Servers die niet voldoen aan de basislijn die is geconfigureerd, zoals aanbevolen wordt bewaakt door Azure Security Center.| 
|\[Preview-versie]: Ontbrekende Endpoint Protection bewaken in Azure Security Center|Servers die een geïnstalleerde Microsoft System Center Endpoint Protection-agent niet hebt, zoals aanbevolen wordt bewaakt door Azure Security Center.|
|\[Preview-versie]: Niet-versleutelde VM-schijven in Azure Security Center bewaken|Virtuele machines waarvoor geen schijfversleuteling is ingeschakeld, zoals aanbevolen wordt bewaakt door Azure Security Center.|
|\[Preview-versie]: Beveiligingsproblemen van virtuele machines in Azure Security Center bewaken|Zwakke plekken die zijn gedetecteerd door de oplossing voor evaluatie van beveiligingsproblemen en virtuele machines die geen een oplossing voor evaluatie van beveiligingsproblemen hebben zoals aanbevolen in Azure Security Center bewaken.|
|\[Preview-versie]: Niet-beveiligde web-App in Azure Security Center bewaken|Azure Security Center bewaakt webtoepassingen zonder web application firewall-beveiliging, zoals aanbevolen.|
|\[Preview-versie]: Netwerkeindpunten in Azure Security Center bewaken|Azure Security Center bewaakt netwerk eindpunten waarvoor geen volgende generatie firewall-beveiliging zoals aanbevolen.|
|\[Preview-versie]: Evaluatie van beveiligingsproblemen SQL monitor resulteert in Azure Security Center|Evaluatie van beveiligingsproblemen monitor scanresultaten en het herstellen van databaseproblemen raadzaam.|
|\[Preview-versie]: Maximum aantal eigenaars voor een abonnement controleren|Het is raadzaam dat u maximaal drie abonnementseigenaren te verminderen van de mogelijkheden voor inbreuk op een gecomprimeerde eigenaar toewijzen.|
|\[Preview-versie]: Minimum aantal eigenaren voor het abonnement controleren|Het wordt aangeraden dat u meer dan één abonnementseigenaar om ervoor te zorgen toegang als beheerder aanwijzen redundantie.|
|\[Preview-versie]: Accounts met eigenaarsmachtigingen die geen MFA is ingeschakeld op een abonnement controleren|Multi-factor authentication (MFA) moet worden ingeschakeld voor alle abonnementsaccounts met eigenaarsmachtigingen om te voorkomen dat er inbreuk wordt gepleegd accounts of resources.|
|\[Preview-versie]: Accounts met schrijfmachtigingen die geen MFA is ingeschakeld op een abonnement controleren|Multi-factor authentication moet worden ingeschakeld voor alle abonnementsaccounts met schrijfmachtigingen om te voorkomen dat inbreuk wordt gepleegd op accounts of resources.|
|\[Preview-versie]: Accounts met leesmachtigingen die geen MFA is ingeschakeld op een abonnement controleren|Multi-factor authentication moet worden ingeschakeld voor alle abonnementsaccounts die leesmachtigingen hebben om te voorkomen dat inbreuk wordt gepleegd op accounts of resources.|
|\[Preview-versie]: Afgeschafte accounts met eigenaarsmachtigingen voor een abonnement controleren|Afgeschafte accounts met eigenaarsmachtigingen moeten worden verwijderd van uw abonnement. Afgeschafte accounts zijn zich geblokkeerd.|
|\[Preview-versie]: Afgeschafte accounts op een abonnement controleren|Afgeschafte accounts moeten worden verwijderd van uw abonnementen. Afgeschafte accounts zijn zich geblokkeerd.|
|\[Preview-versie]: Externe accounts met eigenaarsmachtigingen voor een abonnement controleren|Externe accounts met eigenaarsmachtigingen moeten worden verwijderd uit uw abonnement om te voorkomen dat machtigingen voor toegang.|
|\[Preview-versie]: Controle van externe accounts met schrijfmachtigingen heeft voor een abonnement|Externe accounts die zijn geschreven machtigingen moeten worden verwijderd uit uw abonnement om onbewaakte toegang te voorkomen.|
|\[Preview-versie]: Externe accounts met leesmachtigingen voor een abonnement controleren|Externe accounts die u hebt leesmachtigingen moeten worden verwijderd uit uw abonnement om onbewaakte toegang te voorkomen.|




## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u kunnen lezen hoe u het beveiligingsbeleid in Security Center configureert. Zie de volgende artikelen voor meer informatie over Security Center.

* [Azure Security Center planning- en bedieningsgids voor](security-center-planning-and-operations-guide.md): Informatie over het plannen en ontwerpoverwegingen in Azure Security Center kennen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): Meer informatie over het controleren van de status van uw Azure-resources.
* [Beheren en erop reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md): Informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): Meer informatie over het bewaken van de status van uw partneroplossingen.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md): Krijg antwoorden op veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/): Raadpleeg de blogberichten over beveiliging en naleving in Azure.

Zie voor meer informatie over Azure Policy, [wat is Azure Policy?](../governance/policy/overview.md).
