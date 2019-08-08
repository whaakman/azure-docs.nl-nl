---
title: Controle lijst voor Azure data base Security | Microsoft Docs
description: Dit artikel bevat een set controle lijsten voor Azure data base Security.
services: security
documentationcenter: na
author: unifycloud
manager: barbkess
editor: tomsh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: tomsh
ms.openlocfilehash: bf11d7a9dbaa37db521da67deddbed977cc1d251
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780536"
---
# <a name="azure-database-security-checklist"></a>Controle lijst voor Azure data base-beveiliging

Azure Data Base bevat een aantal ingebouwde beveiligings functies die u kunt gebruiken om de toegang te beperken en te beheren om de beveiliging te verbeteren.

Deze omvatten:

-   Een firewall waarmee u [firewall regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) kunt maken die de connectiviteit beperken op basis van IP-adres,
-   Firewall op server niveau die toegankelijk is vanaf de Azure Portal
-   Firewall regels op database niveau die toegankelijk zijn vanaf SSMS
-   De connectiviteit met uw data base beveiligen met behulp van beveiligde verbindings reeksen
-   Toegangs beheer gebruiken
-   Gegevensversleuteling
-   SQL Database Auditing
-   Detectie van bedreigingen voor SQL Database

## <a name="introduction"></a>Inleiding
Cloud Computing vereist nieuwe beveiligings modellen die niet bekend zijn bij veel toepassings gebruikers, database beheerders en programmeurs. Als gevolg hiervan zijn sommige organisaties cloudhoster voor het implementeren van een Cloud infrastructuur voor gegevens beheer vanwege waargenomen beveiligings Risico's. Veel van deze bezorgdheid kan echter worden verkleind door een beter inzicht te krijgen in de beveiligings functies die zijn ingebouwd in Microsoft Azure en Microsoft Azure SQL Database.

## <a name="checklist"></a>Controlelijst
We raden u aan het artikel over de [Best practices van Azure data base Security](https://docs.microsoft.com/azure/security/fundamentals/database-best-practices) te lezen voordat u deze controle lijst controleert. U kunt deze controle lijst optimaal benutten wanneer u de aanbevolen procedures begrijpt. U kunt deze controle lijst vervolgens gebruiken om ervoor te zorgen dat u de belang rijke problemen in azure data base Security hebt opgelost.


|Controlelijst categorie| Description|
| ------------ | -------- |
|**Gegevens beveiligen**||
| <br> Versleuteling in Motion/Transit| <ul><li>[Transport Layer Security](https://docs.microsoft.com/windows-server/security/tls/transport-layer-security-protocol), voor gegevens versleuteling wanneer gegevens worden verplaatst naar de netwerken.</li><li>Data base vereist beveiligde communicatie van clients op basis van het [TDS-protocol (Tabular Data stream)](https://msdn.microsoft.com/library/dd357628.aspx) via TLS (Transport Layer Security).</li></ul> |
|<br>Versleuteling 'at rest'| <ul><li>[Transparent Data Encryption](https://go.microsoft.com/fwlink/?LinkId=526242), wanneer inactieve gegevens fysiek worden opgeslagen in een digitaal formulier.</li></ul>|
|**Toegang beheren**||  
|<br> Toegang tot de database | <ul><li>[Verificatie](https://docs.microsoft.com/azure/sql-database/sql-database-control-access) (Azure Active Directory-verificatie) AD-verificatie maakt gebruik van identiteiten die worden beheerd door Azure Active Directory.</li><li>[Autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-control-access) verlenen gebruikers de mini maal benodigde bevoegdheden.</li></ul> |
|<br>Toegang tot toepassingen| <ul><li>[Beveiliging op rijniveau](https://msdn.microsoft.com/library/dn765131) (Met behulp van het beveiligings beleid kunt u op hetzelfde moment toegang op rijniveau beperken op basis van de identiteit, rol of uitvoerings context van een gebruiker).</li><li>[Dynamische gegevens maskering](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) (Met behulp van machtiging & beleid, wordt de bloot stelling van gevoelige gegevens beperkt door deze te maskeren voor niet-gemachtigde gebruikers)</li></ul>|
|**Proactieve controle**||  
| <br>Bijhouden & detecteren| <ul><li>[](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) Met auditing worden database gebeurtenissen bijgehouden en naar een audit logboek/activiteiten logboek in uw [Azure Storage-account](https://docs.microsoft.com/azure/storage/storage-create-storage-account)geschreven.</li><li>De Azure data base-status bijhouden met behulp van [Azure monitor activiteiten logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).</li><li>Met [detectie van bedreigingen](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) worden afwijkende database activiteiten gedetecteerd die potentiële beveiligings dreigingen voor de data base aangeven. </li></ul> |
|<br>Azure Security Center| <ul><li>[Gegevens bewaking](https://docs.microsoft.com/azure/security-center/security-center-enable-auditing-on-sql-databases) Gebruik Azure Security Center als een gecentraliseerde oplossing voor beveiligings bewaking voor SQL en andere Azure-Services.</li></ul>|       

## <a name="conclusion"></a>Conclusie
Azure data base is een robuust database platform met een volledig scala aan beveiligings functies die voldoen aan de vereisten voor de organisatorische en reglementaire naleving. U kunt gegevens eenvoudig beveiligen door de fysieke toegang tot uw gegevens te beheren en verschillende opties te gebruiken voor gegevens beveiliging op bestands-, kolom-of rijniveau met Transparent Data Encryption, versleuteling op cel-niveau of beveiliging op rijniveau. Always Encrypted schakelt ook bewerkingen uit op versleutelde gegevens, waardoor het proces van toepassings updates wordt vereenvoudigd. Op zijn beurt krijgt u toegang tot de controle logboeken van SQL Database-activiteiten met de informatie die u nodig hebt, zodat u weet hoe en wanneer gegevens worden geopend.

## <a name="next-steps"></a>Volgende stappen
U hoeft slechts een paar eenvoudige stappen uit te voeren om de beveiliging van uw database tegen kwaadwillende gebruikers of onbevoegde toegang te verbeteren. In deze zelfstudie leert u het volgende:

- Stel [firewall regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) in voor uw server en of Data Base.
- Bescherm uw gegevens met [versleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/sql-server-encryption).
- Schakel [SQL database controle](https://docs.microsoft.com/azure/sql-database/sql-database-auditing)in.

