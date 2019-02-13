---
title: Beveiligen van Azure SQL-service en -gegevens in Azure Security Center | Microsoft Docs
description: Dit document adressen aanbevelingen in Azure Security Center waarmee u Bescherm uw gegevens en Azure SQL-service en blijven in overeenstemming met beveiligingsbeleid.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/15/2019
ms.author: rkarlin
ms.openlocfilehash: d6121a0255b809deccb3c70d56585bab76935900
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56110350"
---
# <a name="protecting-azure-sql-service-and-data-in-azure-security-center"></a>Beveiligen van Azure SQL-service en -gegevens in Azure Security Center
Azure Security Center analyseert de beveiligingsstatus van uw Azure-resources. Wanneer Security Center potentiële beveiligingsproblemen worden geïdentificeerd, worden er aanbevelingen die u bij het proces begeleiden van het configureren van de benodigde besturingselementen.  Aanbevelingen zijn van toepassing op Azure-resource-typen: virtuele machines (VM's), netwerk-, SQL en gegevens en toepassingen.


### <a name="monitor-data-security"></a>Gegevensbeveiliging bewaken

Wanneer u klikt op **Gegevensbeveiliging** in de sectie **Preventie**, wordt **Gegevensbronnen** geopend met aanbevelingen voor SQL en opslag. Ook bevat de blade [aanbevelingen](security-center-sql-service-recommendations.md) voor de algemene integriteitsstatus van de database. Lees voor meer informatie over de versleuteling van opslag [Versleuteling inschakelen voor een Azure-opslagaccount in Azure Security Center](security-center-enable-encryption-for-storage-account.md).

![Gegevensbronnen](./media/security-center-monitoring/security-center-monitoring-fig13-newUI-2017.png)

U kunt onder **SQL-aanbevelingen** op elke aanbeveling klikken voor meer informatie over verdere acties die u kunt ondernemen om een probleem te verhelpen. In het volgende voorbeeld is de aanbeveling **Databasecontrole en detectie van bedreigingen in SQL-databases** uitgevouwen.

![Details over een SQL-aanbeveling](./media/security-center-monitoring/security-center-monitoring-fig14-ga-new.png)

**Controle en detectie van bedreigingen in SQL-databases inschakelen** bevat de volgende informatie:

* Een lijst met SQL-databases
* De server waarop deze zich bevinden
* Informatie over of deze instelling is overgenomen van de server of dat deze uniek is in deze database
* De huidige status
* De ernst van het probleem

Als u op de database klikt om deze aanbeveling op te volgen, wordt **Controle en detectie van bedreigingen** geopend, zoals wordt weergegeven in het volgende scherm.

![Controle en detectie van bedreigingen](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

Als u controle wilt inschakelen, hoeft u alleen maar **AAN** te selecteren onder de optie **Controle**.

## <a name="data-and-storage-recommendations"></a>Aanbevelingen voor gegevens en opslag

|Resourcetype|Beveiligingsscore|Aanbeveling|Description|
|----|----|----|----|
|Storage-account|20|Veilige overdracht naar het opslagaccount vereisen|Veilige overdracht is een optie die ervoor zorgt dat uw storage-account om te accepteren van aanvragen van beveiligde verbindingen (HTTPS). Gebruik van HTTPS-verificatie tussen de server en de service garandeert en gegevens die onderweg zijn beschermd tegen aanvallen op toepassingslagen, zoals man-in-the-middle, niet kan worden afgeluisterd en sessiehijacking netwerk.|
|Redis|20|Alleen beveiligde verbindingen naar uw Azure-Cache voor Redis inschakelen|Schakel alleen verbindingen met SSL-beveiliging met Azure-Cache voor Redis. Gebruik van beveiligde verbindingen zorgt ervoor dat de verificatie tussen de server en de service en gegevens die onderweg zijn beschermd tegen aanvallen op toepassingslagen, zoals man-in-the-middle, niet kan worden afgeluisterd en sessiehijacking netwerk.|
|SQL|15|Transparent Data Encryption inschakelen voor SQL-databases|Transparante gegevensversleuteling inschakelen om data-at-rest te beveiligen en de vereisten na te leven.|
|SQL|15|Controleren voor SQL-servers inschakelen|Controle inschakelen voor Azure SQL-servers. (Alleen voor azure SQL-service. Bevat geen SQL die worden uitgevoerd op uw virtuele machines.)|
|SQL|15|Controleren voor SQL-databases inschakelen|Controle inschakelen voor Azure SQL-databases. (Alleen voor azure SQL-service. Bevat geen SQL die worden uitgevoerd op uw virtuele machines.)|
|Data lake analytics|15|Inschakelen van versleuteling-at-rest van de Data Lake Analytics|Transparent data encryption voor het beveiligen van gegevens in rust in uw Data Lake Analytics inschakelen. Versleuteling-at-rest is transparant, wat betekent dat de Data Lake Analytics automatisch gegevens voordat deze worden opgeslagen, versleutelt en ontsleutelt gegevens voordat ze worden opgehaald. Er zijn geen wijzigingen vereist in de in toepassingen en services die interactie met Data Lake Analytics vanwege versleuteling hebben. Versleuteling van inactieve minimaliseert het risico van gegevensverlies via fysieke diefstal en helpt ook bij het voldoen aan wettelijke vereisten.|
|Data lake store|15|Inschakelen van versleuteling-at-rest voor de Data Lake Store|Transparent data encryption voor het beveiligen van gegevens in rust in uw Data Lake Store inschakelen. Versleuteling-at-rest is transparant, wat betekent dat de Data Lake Store versleutelt automatisch gegevens voordat deze worden opgeslagen, en ontsleutelt gegevens voordat ze worden opgehaald. U hebt geen wijzigingen aanbrengen in de toepassingen en services die interactie met Data Lake Store hebben voor versleuteling. Versleuteling van inactieve minimaliseert het risico van gegevensverlies via fysieke diefstal en helpt ook bij het voldoen aan wettelijke vereisten.|
|Data lake analytics|5|Diagnostische logboeken in Data Lake Analytics inschakelen|Logboeken inschakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd. |
|Data lake store|5|Diagnostische logboeken in Azure Data Lake Store inschakelen|Logboeken inschakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd. |
|SQL|30|Beveiligingsproblemen in uw SQL-databases oplossen|Evaluatie van beveiligingsproblemen SQL scant uw database voor beveiligingsproblemen en wordt aangegeven dat eventuele afwijkingen van aanbevolen procedures, zoals onjuiste configuraties, overmatige machtigingen en niet-beveiligde gevoelige gegevens. Het omzetten van de beveiligingsproblemen gevonden, kan de status van uw database security aanzienlijk verbeteren.|
|SQL|20|Een Azure AD-beheerder voor SQL Server inrichten|Inrichten van een Azure AD-beheerder voor uw SQL-server als Azure AD-verificatie wilt inschakelen. Azure AD-verificatie kunt u beheer van machtigingen vereenvoudigde en gecentraliseerde identiteitsbeheer van databasegebruikers en andere Microsoft-services.|
|Storage-account|15|Onbeperkte netwerktoegang tot een opslagaccount uitschakelen|Audit onbeperkte toegang tot het netwerk in de firewall-instellingen van uw storage-account. In plaats daarvan network-regels configureren, zodat alleen toepassingen van toegestane netwerken toegang het opslagaccount tot hebben. Als u wilt toestaan verbindingen van specifieke Internet of on-premises clients, kan toegang worden verleend aan verkeer van specifieke Azure-netwerken of aan het openbare Internet-IP-adresbereiken.|
|Storage-account|1||Opslagaccounts migreren naar nieuwe Azure RM-resources|Gebruik de nieuwe Azure Resource Manager-v2, voor uw storage-accounts voor verbeterde beveiliging, zoals: sterkere toegangsbeheer (RBAC), betere controle, op basis van Resource Manager-implementatie en beheer, de toegang tot beheerde identiteiten, toegang tot key vault voor geheimen, Azure AD gebaseerde verificatie en ondersteuning voor labels en resourcegroepen voor eenvoudiger beveiligingsbeheer.|



## <a name="see-also"></a>Zie ook
Zie de volgende onderwerpen voor meer informatie over aanbevelingen die betrekking hebben op andere typen Azure-resources:

* [Protecting your virtual machines in Azure Security Center](security-center-virtual-machine-recommendations.md) (Uw virtuele machines beveiligen in Azure Security Center)
* [Protecting your applications in Azure Security Center](security-center-application-recommendations.md) (Uw toepassingen beveiligen in Azure Security Center)
* [Protecting your network in Azure Security Center](security-center-network-recommendations.md) (Uw netwerk beveiligen in Azure Security Center)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](tutorial-security-policy.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
