---
title: Beveiligen van Azure-gegevens en services in Azure Security Center | Microsoft Docs
description: Dit document adressen aanbevelingen in Azure Security Center waarmee u Bescherm uw gegevens en Azure SQL-service en blijven in overeenstemming met beveiligingsbeleid.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/19/2019
ms.author: monhaber
ms.openlocfilehash: 2ac0e4ebaafb8b0c9c79e885cecbefc5a65c1823
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67275305"
---
# <a name="protect-azure-data-and-storage-services-in-azure-security-center"></a>Beveiligen van Azure-gegevens en services in Azure Security Center
Dit onderwerp ziet u hoe u kunt bekijken en aanbevelingen voor beveiliging van gegevens en resources implementeren. Azure Security Center deze aanbevelingen bij het analyseren van de beveiligingsstatus van uw Azure-resources gevonden.

## <a name="view-your-data-security-information"></a>De beveiligingsgegevens van uw gegevens weergeven

1. In de **Resource beveiliging hygiëne** sectie, klikt u op **gegevens- en opslagresources**.

   ![Gegevens en storage-resources](./media/security-center-monitoring/click-data.png)

    De **gegevensbeveiliging** pagina wordt geopend met aanbevelingen voor gegevensbronnen.

     ![Gegevensbronnen](./media/security-center-monitoring/sql-overview.png)

Op deze pagina kunt u het volgende doen:

* Klik op de **overzicht** tabblad geeft een lijst van alle aanbevelingen voor gegevens resources om te worden hersteld. 
* Klik op elk tabblad en weergeven van de aanbevelingen op resourcetype.

    > [!NOTE]
    > Lees voor meer informatie over de versleuteling van opslag [Versleuteling inschakelen voor een Azure-opslagaccount in Azure Security Center](security-center-enable-encryption-for-storage-account.md).


## <a name="remediate-a-recommendation-on-a-data-resource"></a>Een aanbeveling voor een bron van de gegevens herstellen

1. Uit een van de resource-tabbladen, klikt u op een resource. De pagina wordt geopend aanbieding van de aanbevelingen voor het probleem worden opgelost.

    ![Broninformatie](./media/security-center-monitoring/sql-recommendations.png)

2. Klik op een aanbeveling. De aanbeveling-pagina wordt geopend en toont de **herstelstappen** de aanbeveling kunt implementeren.

   ![Herstelstappen](./media/security-center-monitoring/remediate1.png)

3. Klik op **actie ondernemen**. De pagina van de resource-instellingen wordt weergegeven.

    ![Aanbeveling inschakelen](./media/security-center-monitoring/remediate2.png)

4. Ga als volgt de **herstelstappen** en klikt u op **opslaan**.

## <a name="data-and-storage-recommendations"></a>Aanbevelingen voor gegevens en opslag

|Resourcetype|Beveiligingsscore|Aanbeveling|Description|
|----|----|----|----|
|Storage-account|20|Veilige overdracht naar storage-accounts moet worden ingeschakeld|Veilige overdracht is een optie die ervoor zorgt dat uw storage-account om te accepteren van aanvragen van beveiligde verbindingen (HTTPS). HTTPS-verificatie tussen de server en de service garandeert en gegevens die onderweg zijn beschermd tegen netwerklaagaanvallen, zoals man-in-the-middle, niet kan worden afgeluisterd en sessie-hijacking.|
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

Zie de volgende onderwerpen voor meer informatie over Security Center:

* [Setting security policies in Azure Security Center](tutorial-security-policy.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
