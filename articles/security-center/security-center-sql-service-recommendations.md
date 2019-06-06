---
title: Beveiligen van Azure SQL-service en -gegevens in Azure Security Center | Microsoft Docs
description: Dit document adressen aanbevelingen in Azure Security Center waarmee u Bescherm uw gegevens en Azure SQL-service en blijven in overeenstemming met beveiligingsbeleid.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/27/2019
ms.author: monhaber
ms.openlocfilehash: bbba5f380fddb4fdec43a7414e59778135c4e0ef
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428293"
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
|Storage-account|20|Veilige overdracht naar storage-accounts moet worden ingeschakeld|Veilige overdracht is een optie die ervoor zorgt dat uw storage-account om te accepteren van aanvragen van beveiligde verbindingen (HTTPS). Gebruik van HTTPS-verificatie tussen de server en de service garandeert en gegevens die onderweg zijn beschermd tegen aanvallen op toepassingslagen, zoals man-in-the-middle, niet kan worden afgeluisterd en sessiehijacking netwerk.|
|Redis|20|Alleen beveiligde verbindingen met uw Redis-Cache moeten worden ingeschakeld|Schakel alleen verbindingen met SSL-beveiliging met Azure-Cache voor Redis. Gebruik van beveiligde verbindingen zorgt ervoor dat de verificatie tussen de server en de service en gegevens die onderweg zijn beschermd tegen aanvallen op toepassingslagen, zoals man-in-the-middle, niet kan worden afgeluisterd en sessiehijacking netwerk.|
|SQL|15|De Transparent Data Encryption voor SQL-databases moet zijn ingeschakeld|Transparent data encryption voor het beveiligen van data-at-rest en voldoen aan nalevingsvereisten inschakelen.|
|SQL|15|Controleren voor SQL server moet worden ingeschakeld|Controle inschakelen voor Azure SQL-servers. (Alleen voor azure SQL-service. Bevat geen SQL die worden uitgevoerd op uw virtuele machines.)|
|Data lake analytics|5|Diagnostische logboeken in Data Lake Analytics moeten worden ingeschakeld|Logboeken inschakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd. |
|Data lake store|5|Diagnostische logboeken in Azure Data Lake Store moeten worden ingeschakeld|Logboeken inschakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd. |
|SQL|30|Door beveiligingslekken in uw SQL-databases moeten worden hersteld.|Evaluatie van beveiligingsproblemen SQL scant uw database voor beveiligingsproblemen en wordt aangegeven dat eventuele afwijkingen van aanbevolen procedures, zoals onjuiste configuraties, overmatige machtigingen en niet-beveiligde gevoelige gegevens. Het omzetten van de beveiligingsproblemen gevonden, kan de status van uw database security aanzienlijk verbeteren.|
|SQL|20|Een Azure AD-beheerder voor SQL server inrichten|Inrichten van een Azure AD-beheerder voor uw SQL-server als Azure AD-verificatie wilt inschakelen. Azure AD-verificatie kunt u beheer van machtigingen vereenvoudigde en gecentraliseerde identiteitsbeheer van databasegebruikers en andere Microsoft-services.|
|Storage-account|15|Toegang tot opslagaccounts met firewall en configuraties van het virtuele netwerk moet worden beperkt|Audit onbeperkte toegang tot het netwerk in de firewall-instellingen van uw storage-account. In plaats daarvan network-regels configureren, zodat alleen toepassingen van toegestane netwerken toegang het opslagaccount tot hebben. Als u wilt toestaan verbindingen met specifieke Internet- of on-premises clients, kunt u toegang tot het verkeer van specifieke Azure-netwerken of openbare Internet-IP-adresbereiken verlenen.|
|Storage-account|1|Storage-accounts moeten worden gemigreerd naar nieuwe Azure Resource Manager-resources|Nieuwe Azure Resource Manager-v2 voor uw storage-accounts gebruiken voor verbeterde beveiliging, zoals: sterkere toegangsbeheer (RBAC), betere controle, op basis van Resource Manager-implementatie en beheer, de toegang tot beheerde identiteiten, toegang tot key vault voor geheimen, en Azure AD gebaseerde verificatie en ondersteuning voor labels en resourcegroepen voor eenvoudiger beveiligingsbeheer.|

## <a name="see-also"></a>Zie ook
Zie de volgende onderwerpen voor meer informatie over aanbevelingen die betrekking hebben op andere typen Azure-resources:

* [Protecting your virtual machines in Azure Security Center](security-center-virtual-machine-recommendations.md) (Uw virtuele machines beveiligen in Azure Security Center)
* [Protecting your applications in Azure Security Center](security-center-application-recommendations.md) (Uw toepassingen beveiligen in Azure Security Center)
* [Protecting your network in Azure Security Center](security-center-network-recommendations.md) (Uw netwerk beveiligen in Azure Security Center)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](tutorial-security-policy.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
