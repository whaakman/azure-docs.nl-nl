---
title: Azure beleidsdefinities in Azure Security Center bewaakt | Microsoft Docs
description: Azure beleidsdefinities in Azure Security Center bewaakt.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: c89cb1aa-74e8-4ed1-980a-02a7a25c1a2f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/22/2018
ms.author: rkarlin
ms.openlocfilehash: e76464e294a3378bf3a275ec4fe7ccbd87c09475
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53011627"
---
# <a name="azure-security-policies-monitored-by-azure-security-center"></a>Beleid voor Azure-beveiliging bewaakt door Azure Security Center
In dit artikel biedt een overzicht van Azure Policy-definities die kan worden bewaakt in Security Center.

## <a name="available-security-policies"></a>Beschikbaar beleid

Zie voor meer informatie over de ingebouwde beleidsregels die worden bewaakt door Security Center, de volgende tabel:

| Beleid | Betekenis van het beleid |
| --- | --- |
|Audit inschakelen van diagnostische logboeken in Service Fabric en schaalsets van virtuele machines|Het verdient aanbeveling om Logboeken in te schakelen zodat het activiteitenspoor kan worden gemaakt als er onderzoek is vereist na een incident of inbreuk.|
|Autorisatieregels voor Event Hub-naamruimten controleren|Event Hub-clients moeten een beleid voor het toegangsniveau van naamruimte die toegang tot alle wachtrijen en onderwerpen in een naamruimte biedt niet gebruiken. Om uit te lijnen met minimale bevoegdheden beveiligingsmodel, moet u toegangsbeleid maken op het entiteitsniveau van de voor wachtrijen en onderwerpen voor toegang tot alleen de specifieke entiteit.|
|Aanwezigheid van autorisatieregels in Event Hub-entiteiten controleren|Controle sprake is van autorisatieregels voor Event Hub-entiteiten minste bevoegdheden toegang te verlenen.|
|Onbeperkte netwerktoegang tot opslagaccounts controleren|Audit onbeperkte toegang tot het netwerk in de firewall-instellingen van uw storage-account. In plaats daarvan network-regels configureren, zodat alleen toepassingen van toegestane netwerken toegang het opslagaccount tot hebben. Als u wilt toestaan verbindingen van specifieke Internet of on-premises clients, kan toegang worden verleend aan verkeer van specifieke Azure-netwerken of aan het openbare Internet-IP-adresbereiken.|
|Het gebruik van aangepaste RBAC-regels controleren|Controle van ingebouwde rollen, zoals eigenaar, rechten voor bijdragers, lezer in plaats van een aangepaste RBAC-rollen, die foutgevoelig zijn. Met behulp van aangepaste rollen wordt beschouwd als een uitzondering en vereist een strenge controleren en een risicomodel.|
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
|[Preview]: Niet-versleutelde SQL-database bewaken in Azure Security Center|Niet-versleutelde SQL-servers of databases wordt bewaakt door Azure Security Center als aanbevelingen.| 
|[Preview]: Niet-gecontroleerde SQL-database bewaken in Azure Security Center|SQL-servers en databases waarvoor geen SQL-controle ingeschakeld zal worden bewaakt door Azure Security Center als aanbevelingen.| 
|[Preview]: Ontbrekende systeemupdates bewaken in Azure Security Center|Ontbrekende beveiligingssysteemupdates op uw servers zullen worden bewaakt door Azure Security Center als aanbevelingen.| 
|[Preview-versie]: ontbrekende blob-versleuteling voor opslagaccounts controleren|Dit beleid controleert opslagaccounts zonder blobversleuteling. Dit is alleen van toepassing op Microsoft.Storage-resourcetypen, niet op andere opslagproviders. Mogelijk netwerk Just In Time-toegang wordt worden als aanbevelingen bewaakt door Azure Security Center.| 
|[Preview]: Mogelijke Just In Time-netwerktoegang (JIT) bewaken in Azure Security Center|Mogelijke netwerktoegang alleen In tijd (JIT) worden door Azure Security Center als aanbevelingen bewaakt.| 
|[Preview]: Mogelijkheid voor de lijst met goedgekeurde apps bewaken in Azure Security Center|Mogelijke whitelistconfiguratie voor toepassingen wordt bewaakt door Azure Security Center.| 
|[Preview]: Ruime netwerktoegang bewaken in Azure Security Center|Netwerkbeveiligingsgroepen met te ruime regels worden door Azure Security Center als aanbevelingen bewaakt.| 
|[Preview]: Beveiligingsproblemen van besturingssystemen bewaken in Azure Security Center|Servers die niet voldoen aan de ingestelde basislijn worden bewaakt door Azure Security Center als aanbevelingen.| 
|[Preview]: Ontbrekende Endpoint Protection bewaken in Azure Security Center|Servers zonder een geïnstalleerde Endpoint Protection-agent worden bewaakt door Azure Security Center als aanbevelingen.| 
|[Preview]: Niet-versleutelde VM-schijven bewaken in Azure Security Center|Virtuele machines zonder ingeschakelde schijfversleuteling worden door Azure Security Center als aanbevelingen bewaakt.| 
|[Preview]: Beveiligingsproblemen van virtuele machines bewaken in Azure Security Center|Hiermee worden beveiligingsproblemen die door de oplossing voor evaluatie van beveiligingsproblemen zijn gedetecteerd en virtuele machines zonder oplossing voor evaluatie van beveiligingsproblemen bewaakt als aanbevelingen in Azure Security Center.| 
|[Preview]: Niet-beveiligde webtoepassing bewaken in Azure Security Center|Webtoepassingen zonder Web Application Firewall bescherming wordt worden als aanbevelingen bewaakt door Azure Security Center.| 
|[Preview]: Niet-beveiligde netwerkeindpunten bewaken in Azure Security Center|Netwerkeindpunten zonder een Next Generation Firewall beveiliging wordt worden als aanbevelingen bewaakt door Azure Security Center.| 
|[Preview]: Resultaten van SQL-beveiligingsevaluatie bewaken in Azure Security Center|Bewaken van de scanresultaten en aanbevelingen van de beveiligingsevaluatie over het herstellen van beveiligingsproblemen in een database.| 
|[Preview-versie]: controle maximale aantal eigenaars voor een abonnement|Het wordt aanbevolen maximaal drie abonnementseigenaren toe te wijzen om het risico dat een gecomprimeerde eigenaar inbreuk kan plegen te beperken.| 
|[Preview-versie]: controle minimale aantal eigenaars voor een abonnement|We raden u aan meer dan één abonnementseigenaar toe te wijzen voor toegangsredundantie voor beheerders.| 
|[Preview-versie]: Accounts met eigenaarsmachtigingen controleren waarbij MFA niet is ingeschakeld in een abonnement|Schakel meervoudige verificatie (MFA) in voor alle abonnementsaccounts met eigenaarsmachtigingen om te voorkomen dat er inbreuk wordt gepleegd op accounts of resources.| 
|[Preview-versie]: controle accounts met schrijfmachtigingen waarbij MFA niet is ingeschakeld in een abonnement|Schakel meervoudige verificatie (MFA) in voor alle abonnementsaccounts met schrijfmachtigingen om te voorkomen dat er inbreuk wordt gepleegd op accounts of resources.| 
|[Preview-versie]: controle accounts met leesmachtigingen waarbij MFA niet is ingeschakeld in een abonnement|Schakel meervoudige verificatie (MFA) in voor alle abonnementsaccounts met leesmachtigingen om te voorkomen dat er inbreuk wordt gepleegd op accounts of resources.| 
|[Preview-versie]: controle afgeschafte accounts met eigenaarsmachtigingen in een abonnement|Afgeschafte accounts met eigenaarsmachtigingen moeten worden verwijderd uit uw abonnement. Afgeschafte accounts zijn accounts die zich zijn geblokkeerd.| 
|[Preview-versie]: controle afgeschafte accounts in een abonnement|Afgeschafte accounts moeten worden verwijderd van uw abonnementen. Afgeschafte accounts zijn accounts die zich zijn geblokkeerd.| 
|[Preview-versie]: controle externe accounts met eigenaarsmachtigingen in een abonnement|Externe accounts met eigenaarsmachtigingen moeten worden verwijderd uit uw abonnement om onbewaakte toegang te voorkomen.| 
|[Preview-versie]: controle externe accounts met schrijfmachtigingen in een abonnement|Externe accounts met schrijfmachtigingen moeten worden verwijderd van uw abonnement om onbewaakte toegang te voorkomen.| 
|[Preview-versie]: controle externe accounts met leesmachtigingen in een abonnement|Externe accounts met leesmachtigingen moeten worden verwijderd van uw abonnement om onbewaakte toegang te voorkomen.| 




## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u kunnen lezen hoe u het beveiligingsbeleid in Security Center configureert. Zie de volgende artikelen voor meer informatie over Security Center:

* [Gids voor de planning en werking van Azure Security Center](security-center-planning-and-operations-guide.md): leer de ontwerpoverwegingen kennen en leer hiervoor te plannen voor Azure Security Center.
* [Beveiligingsstatus controleren in Azure Security Center](security-center-monitoring.md): meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md): leer hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.

Zie [Wat is Azure Policy?](../azure-policy/azure-policy-introduction.md) voor meer informatie over Azure Policy.
