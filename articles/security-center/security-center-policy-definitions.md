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
ms.openlocfilehash: 895afb7af8ed7100af237f88e2953f4dc0991acb
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57195248"
---
# <a name="azure-security-policies-monitored-by-azure-security-center"></a>Beleid voor Azure-beveiliging bewaakt door Azure Security Center
In dit artikel biedt een overzicht van Azure Policy-definities die kan worden bewaakt in Security Center. Zie voor meer informatie over beveiligingsbeleid, [werken met beleidsregels voor veiligheid](tutorial-security-policy.md).

## <a name="available-security-policies"></a>Beschikbaar beleid

Zie voor meer informatie over de ingebouwde beleidsregels die worden bewaakt door Security Center, de volgende tabel:

| Beleid | Betekenis van het beleid |
| --- | --- |
|Audit inschakelen van diagnostische logboeken in Service Fabric en schaalsets van virtuele machines|Het verdient aanbeveling om Logboeken in te schakelen zodat het activiteitenspoor kan worden gemaakt als er onderzoek is vereist na een incident of inbreuk.|
|Autorisatieregels voor Event Hub-naamruimten controleren|Event Hub-clients moeten een beleid voor het toegangsniveau van naamruimte die toegang tot alle wachtrijen en onderwerpen in een naamruimte biedt niet gebruiken. Om uit te lijnen met minimale bevoegdheden beveiligingsmodel, moet u toegangsbeleid maken op het entiteitsniveau van de voor wachtrijen en onderwerpen voor toegang tot alleen de specifieke entiteit.|
|Aanwezigheid van autorisatieregels in Event Hub-entiteiten controleren|Controle sprake is van autorisatieregels voor Event Hub-entiteiten minste bevoegdheden toegang te verlenen.|
|Onbeperkte netwerktoegang tot opslagaccounts controleren|Audit onbeperkte toegang tot het netwerk in de firewall-instellingen van uw storage-account. In plaats daarvan network-regels configureren, zodat alleen toepassingen van toegestane netwerken toegang het opslagaccount tot hebben. Als u wilt toestaan verbindingen van specifieke Internet of on-premises clients, kan toegang worden verleend aan verkeer van specifieke Azure-netwerken of aan het openbare Internet-IP-adresbereiken.|
|Het gebruik van aangepaste RBAC-regels controleren|Controle van ingebouwde rollen, zoals eigenaar, Inzender, lezer in plaats van een aangepaste RBAC-rollen, die foutgevoelig zijn. Met behulp van aangepaste rollen wordt beschouwd als een uitzondering en vereist een strenge controleren en een risicomodel.|
|Inschakelen van diagnostische logboeken in Azure Stream Analytics controleren|Audit logs in te schakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Veilige overdracht naar opslagaccounts controleren|De vereiste controle van Secure transfer in uw opslagaccount. Veilige overdracht is een optie die ervoor zorgt dat uw storage-account om te accepteren van aanvragen van beveiligde verbindingen (HTTPS). Gebruik van HTTPS-verificatie tussen de server en de service garandeert en gegevens die onderweg zijn beschermd tegen aanvallen op toepassingslagen, zoals man-in-the-middle, niet kan worden afgeluisterd en sessiehijacking netwerk.|
|Inrichting van een Azure Active Directory-beheerder voor SQL-server controleren|Controleren met het inrichten van een Azure Active Directory-beheerder voor uw SQL-server voor Azure AD-verificatie. Azure AD-verificatie kunt u beheer van machtigingen vereenvoudigde en gecentraliseerde identiteitsbeheer van databasegebruikers en andere Microsoft-services.|
|Autorisatieregels voor Service Bus-naamruimten controleren|Service Bus-clients moeten een beleid voor het toegangsniveau van naamruimte die toegang tot alle wachtrijen en onderwerpen in een naamruimte biedt niet gebruiken. Om uit te lijnen met minimale bevoegdheden beveiligingsmodel, moet u toegangsbeleid maken op het entiteitsniveau van de voor wachtrijen en onderwerpen voor toegang tot alleen de specifieke entiteit.|
|Inschakelen van diagnostische logboeken in Service Bus controleren|Audit logs in te schakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Controle op de instelling van de eigenschap ClusterProtectionLevel op EncryptAndSign in Service Fabric|Service Fabric biedt drie niveaus van beveiliging (geen, aanmelding en EncryptAndSign) voor communicatie van knooppunt-naar-knooppunt met behulp van een primaire clustercertificaat. Stel het beveiligingsniveau om ervoor te zorgen dat alle berichten van knooppunt-naar-knooppunt zijn versleuteld en digitaal zijn ondertekend.| 
|Het gebruik van Azure Active Directory voor clientverificatie in Service Fabric controleren|Exclusief gebruik van clientverificatie via Azure Active Directory in Service Fabric controleren| 
|Inschakelen van diagnostische logboeken voor Search-service controleren|Audit logs in te schakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.| 
|Audit alleen beveiligde verbindingen naar uw Azure-Cache voor Redis in te schakelen|De controlemodus alleen verbindingen met SSL-beveiliging met Azure-Cache voor Redis in te schakelen. Gebruik van beveiligde verbindingen zorgt ervoor dat de verificatie tussen de server en de service en gegevens die onderweg zijn beschermd tegen aanvallen op toepassingslagen, zoals man-in-the-middle, niet kan worden afgeluisterd en sessiehijacking netwerk| 
|Inschakelen van diagnostische logboeken in Logic Apps controleren|Audit logs in te schakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.| 
|Inschakelen van diagnostische logboeken in Key Vault controleren|Audit logs in te schakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.|
|Inschakelen van diagnostische logboeken in Event Hub controleren|Audit logs in te schakelen en ze maximaal een jaar bewaren. Hierdoor kunnen sporen onderzoek activiteit opnieuw kunt maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.| 
|Inschakelen van diagnostische logboeken in Azure Data Lake Store controleren|Audit logs in te schakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.| 
|Inschakelen van diagnostische logboeken in Data Lake Analytics controleren|Audit logs in te schakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.| 
|Het gebruik van klassieke opslagaccounts controleren|Azure Resource Manager voor uw storage-accounts gebruiken voor verbeterde beveiliging, zoals: sterkere toegangsbeheer (RBAC), betere controle, op basis van Azure Resource Manager-implementatie en beheeracties, toegang tot beheerde identiteiten, toegang tot key vault voor geheimen , Azure AD gebaseerde verificatie en ondersteuning voor labels en resourcegroepen voor eenvoudiger beveiligingsbeheer.| 
|Het gebruik van klassieke virtuele machines controleren|Gebruik Azure Resource Manager voor uw virtuele machines voor verbeterde beveiliging, zoals: sterkere toegangsbeheer (RBAC), betere controle, op basis van Azure Resource Manager-implementatie en beheeracties, toegang tot beheerde identiteiten, toegang tot key vault voor geheimen , Azure AD gebaseerde verificatie en ondersteuning voor labels en resourcegroepen voor eenvoudiger beveiligingsbeheer.| 
|Configuratie van metrische waarschuwingsregels in Batch-accounts controleren|Configuratie van regels voor metrische waarschuwingen op Batch-account om in te schakelen van de vereiste metrische gegevens controleren.| 
|Configuratie van metrische waarschuwingsregels in Batch-accounts controleren|Configuratie van regels voor metrische waarschuwingen op Batch-account om in te schakelen van de vereiste metrische gegevens controleren.| 
|Inschakelen van diagnostische logboeken in Batch-accounts controleren|Audit logs in te schakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.| 
|Inschakelen van versleuteling van Automation-accountvariabelen controleren|Het is belangrijk om de versleuteling van variabele assets in een Automation-account in te schakelen bij het opslaan van gevoelige gegevens.| 
|Inschakelen van diagnostische logboeken in App Services controleren|Controle inschakelen van diagnostische logboeken op de app. Hiermee kunt u activiteit sporen onderzoek opnieuw maken als een beveiligingsincident optreedt of als uw netwerk is gecompromitteerd.| 
|Controlestatus van transparante gegevensversleuteling|Controlestatus van transparante gegevensversleuteling voor SQL-databases.| 
|Instellingen voor SQL-controle op serverniveau|Controleert het bestaan van de SQL-controle op serverniveau.| 
|\[Preview-versie]: Niet-versleutelde SQL-database in Azure Security Center bewaken|Niet-versleutelde SQL-servers of databases wordt bewaakt door Azure Security Center als aanbevelingen.| 
|\[Preview-versie]: Niet-gecontroleerde SQL-database in Azure Security Center bewaken|SQL-servers en databases waarvoor geen SQL-controle ingeschakeld zal worden bewaakt door Azure Security Center als aanbevelingen.| 
|\[Preview-versie]: Ontbrekende systeemupdates in Azure Security Center bewaken|Ontbrekende beveiligingssysteemupdates op uw servers zullen worden bewaakt door Azure Security Center als aanbevelingen.| 
|\[Preview-versie]: Controle uitvoeren op ontbrekende blobversleuteling voor opslagaccounts|Dit beleid controleert opslagaccounts zonder blobversleuteling. Dit is alleen van toepassing op Microsoft.Storage-resourcetypen, niet op andere opslagproviders. Mogelijk netwerk Just In Time-toegang wordt worden als aanbevelingen bewaakt door Azure Security Center.| 
|\[Preview-versie]: Mogelijk alleen In tijd (JIT)-toegang in Azure Security Center bewaken|Mogelijke netwerktoegang alleen In tijd (JIT) worden door Azure Security Center als aanbevelingen bewaakt.| 
|\[Preview-versie]: Mogelijke opname Apps op Whitelist in Azure Security Center bewaken|Mogelijke whitelistconfiguratie voor toepassingen wordt bewaakt door Azure Security Center.| 
|\[Preview-versie]: Ruime netwerktoegang in Azure Security Center bewaken|Netwerkbeveiligingsgroepen met te ruime regels worden door Azure Security Center als aanbevelingen bewaakt.| 
|\[Preview-versie]: Beveiligingsproblemen van besturingssystemen bewaken in Azure Security Center|Servers die niet voldoen aan de ingestelde basislijn worden bewaakt door Azure Security Center als aanbevelingen.| 
|\[Preview-versie]: Ontbrekende Endpoint Protection bewaken in Azure Security Center|Servers zonder een geïnstalleerde Endpoint Protection-agent worden bewaakt door Azure Security Center als aanbevelingen.| 
|\[Preview-versie]: Niet-versleutelde VM-schijven in Azure Security Center bewaken|Virtuele machines zonder ingeschakelde schijfversleuteling worden door Azure Security Center als aanbevelingen bewaakt.| 
|\[Preview-versie]: Beveiligingsproblemen van virtuele machines bewaken in Azure Security Center|Hiermee worden beveiligingsproblemen die door de oplossing voor evaluatie van beveiligingsproblemen zijn gedetecteerd en virtuele machines zonder oplossing voor evaluatie van beveiligingsproblemen bewaakt als aanbevelingen in Azure Security Center.| 
|\[Preview-versie]: Niet-beveiligde web-App in Azure Security Center bewaken|Webtoepassingen zonder Web Application Firewall bescherming wordt worden als aanbevelingen bewaakt door Azure Security Center.| 
|\[Preview-versie]: Netwerkeindpunten in Azure Security Center bewaken|Netwerkeindpunten zonder een Next Generation Firewall beveiliging wordt worden als aanbevelingen bewaakt door Azure Security Center.| 
|\[Preview-versie]: Evaluatie van beveiligingsproblemen SQL monitor resulteert in Azure Security Center|Bewaken van de scanresultaten en aanbevelingen van de beveiligingsevaluatie over het herstellen van beveiligingsproblemen in een database.| 
|\[Preview-versie]: Maximum aantal eigenaars voor een abonnement controleren|Het wordt aanbevolen maximaal drie abonnementseigenaren toe te wijzen om het risico dat een gecomprimeerde eigenaar inbreuk kan plegen te beperken.| 
|\[Preview-versie]: Minimum aantal eigenaren voor het abonnement controleren|We raden u aan meer dan één abonnementseigenaar toe te wijzen voor toegangsredundantie voor beheerders.| 
|\[Preview-versie]: Accounts met eigenaarsmachtigingen die geen MFA is ingeschakeld op een abonnement controleren|Schakel meervoudige verificatie (MFA) in voor alle abonnementsaccounts met eigenaarsmachtigingen om te voorkomen dat er inbreuk wordt gepleegd op accounts of resources.| 
|\[Preview-versie]: Accounts met schrijfmachtigingen die geen MFA is ingeschakeld op een abonnement controleren|Schakel meervoudige verificatie (MFA) in voor alle abonnementsaccounts met schrijfmachtigingen om te voorkomen dat er inbreuk wordt gepleegd op accounts of resources.| 
|\[Preview-versie]: Accounts met leesmachtigingen die geen MFA is ingeschakeld op een abonnement controleren|Schakel meervoudige verificatie (MFA) in voor alle abonnementsaccounts met leesmachtigingen om te voorkomen dat er inbreuk wordt gepleegd op accounts of resources.| 
|\[Preview-versie]: Afgeschafte accounts met eigenaarsmachtigingen voor een abonnement controleren|Afgeschafte accounts met eigenaarsmachtigingen moeten worden verwijderd uit uw abonnement. Afgeschafte accounts zijn accounts die zich zijn geblokkeerd.| 
|\[Preview-versie]: Afgeschafte accounts op een abonnement controleren|Afgeschafte accounts moeten worden verwijderd van uw abonnementen. Afgeschafte accounts zijn accounts die zich zijn geblokkeerd.| 
|\[Preview-versie]: Externe accounts met eigenaarsmachtigingen voor een abonnement controleren|Externe accounts met eigenaarsmachtigingen moeten worden verwijderd uit uw abonnement om onbewaakte toegang te voorkomen.| 
|\[Preview-versie]: Controle van externe accounts met schrijfmachtigingen heeft voor een abonnement|Externe accounts met schrijfmachtigingen moeten worden verwijderd van uw abonnement om onbewaakte toegang te voorkomen.| 
|\[Preview-versie]: Externe accounts met leesmachtigingen voor een abonnement controleren|Externe accounts met leesmachtigingen moeten worden verwijderd van uw abonnement om onbewaakte toegang te voorkomen.| 




## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u kunnen lezen hoe u het beveiligingsbeleid in Security Center configureert. Zie de volgende artikelen voor meer informatie over Security Center:

* [Azure Security Center planning- en bedieningsgids voor](security-center-planning-and-operations-guide.md): Leer hoe u Azure Security Center de ontwerpoverwegingen kennen en plan.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): Meer informatie over het controleren van de status van uw Azure-resources.
* [Beheren en erop reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md): Informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): Meer informatie over het bewaken van de status van uw partneroplossingen.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md): Krijg antwoorden op veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): Raadpleeg de blogberichten over beveiliging en naleving in Azure.

Zie [Wat is Azure Policy?](../governance/policy/overview.md) voor meer informatie over Azure Policy.
