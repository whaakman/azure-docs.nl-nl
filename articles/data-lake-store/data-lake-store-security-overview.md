---
title: Overzicht van de beveiliging in Data Lake Store | Microsoft Docs
description: Begrijpen hoe Azure Data Lake Store is een veiligere big data-archief
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ebd5b2ac-c5cc-46d4-9cfd-1a1ee70024c2
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 52e176711f512e8a3788309a58011c8484821a1e
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="security-in-azure-data-lake-store"></a>Beveiliging in Azure Data Lake Store
Veel bedrijven profiteert van big data-analyses voor zakelijke inzichten zodat beslissingen voor de smartcard. Een organisatie kan een complex en gereglementeerde omgeving met een toenemend aantal diverse gebruikers hebben. Is het essentieel is voor een ondernemings-om ervoor te zorgen dat essentiële bedrijfsgegevens veiliger, wordt opgeslagen met het juiste niveau van toegang te krijgen tot afzonderlijke gebruikers. Azure Data Lake Store is ontworpen om te voldoen aan deze beveiligingsvereisten. In dit artikel meer informatie over de beveiligingsmogelijkheden van Data Lake Store, met inbegrip van:

* Authentication
* Autorisatie
* Netwerkisolatie
* Gegevensbeveiliging
* Controleren

## <a name="authentication-and-identity-management"></a>Authenticatie en identiteit management
Verificatie is het proces waarmee de identiteit van een gebruiker is geverifieerd wanneer de gebruiker werkt met Data Lake Store of met een service die is verbonden met Data Lake Store. Voor identiteits- en verificatie, het gebruik van Data Lake Store [Azure Active Directory](../active-directory/active-directory-whatis.md), een uitgebreide identiteits- en toegangsbeheer cloud beheeroplossing die het beheer van gebruikers en groepen vereenvoudigt.

Elk Azure-abonnement kan worden gekoppeld aan een exemplaar van Azure Active Directory. Alleen gebruikers en service-identiteiten die zijn gedefinieerd in uw Azure Active Directory-service heeft toegang tot uw Data Lake Store-account met behulp van de Azure-portal, opdrachtregelprogramma's of via de clienttoepassingen van uw organisatie worden gebouwd met behulp van Azure Data Lake Store SDK. Belangrijkste voordelen van het gebruik van Azure Active Directory als een gecentraliseerde mechanisme voor toegangsbeheer zijn:

* Identity lifecycle beheer vereenvoudigd. De identiteit van een gebruiker of een service (een service principal identiteit) worden snel gemaakt en snel door gewoon verwijderen of uitschakelen van de account in de map is ingetrokken.
* Meervoudige verificatie. [Multi-factorauthenticatie](../multi-factor-authentication/multi-factor-authentication.md) biedt een extra beveiligingslaag voor gebruikersaanmeldingen en transacties.
* Verificatie van elke client via een open standaardprotocol, zoals OAuth of OpenID.
* Federatie met enterprise directoryservices en cloud-id-providers.

## <a name="authorization-and-access-control"></a>Autorisatie en toegangsbeheer
Nadat u Azure Active Directory wordt een gebruiker geverifieerd, zodat de gebruiker toegang Azure Data Lake Store tot, toegangsmachtigingen autorisatie besturingselementen voor Data Lake Store. Data Lake Store scheidt autorisatie voor activiteiten met betrekking tot account en gerelateerde gegevens in de volgende manier:

* [Toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-what-is.md) (RBAC) opgegeven door de Azure voor accountbeheer
* POSIX ACL voor toegang tot gegevens in het archief

### <a name="rbac-for-account-management"></a>RBAC voor accountbeheer
Vier eenvoudige rollen worden standaard gedefinieerd voor Data Lake Store. De rollen toestaan dat verschillende bewerkingen op een Data Lake Store-account via de Azure-portal, PowerShell-cmdlets en REST-API's. De functies en de eigenaar of bijdrager kunnen tal van functies voor beheer uitvoeren op het account. U kunt de rol Lezer toewijzen aan gebruikers die alleen met gegevens werken.

![RBAC-rollen](./media/data-lake-store-security-overview/rbac-roles.png "RBAC-rollen")

Houd er rekening mee dat hoewel rollen zijn toegewezen voor accountbeheer, sommige rollen gevolgen heeft voor toegang tot gegevens. U moet ACL's gebruiken om toegang aan de bewerkingen die een gebruiker op het bestandssysteem uitvoeren kan te beheren. De volgende tabel bevat een overzicht van de rights management en toegangsrechten van gegevens voor de standaardrollen.

| Rollen | Rights management | Data access-rechten | Uitleg |
| --- | --- | --- | --- |
| Er is geen rol die is toegewezen |Geen |Beheerst door de ACL |De gebruiker niet de Azure portal of Azure PowerShell-cmdlets gebruiken om te bladeren Data Lake Store. De gebruiker kan alleen de opdrachtregelprogramma's gebruiken. |
| Eigenaar |Alle |Alle |De rol van eigenaar is een beheerder. Deze rol kan alles beheren en heeft volledige toegang tot gegevens. |
| Lezer |Alleen-lezen |Beheerst door de ACL |De rol lezer kunt alles met betrekking tot accountbeheer, zoals welke gebruiker is toegewezen aan welke rol weergeven. De rol lezer kunt geen wijzigingen aanbrengen. |
| Inzender |Alles behalve rollen toevoegen en verwijderen |Beheerst door de ACL |De rol Inzender kunt bepaalde aspecten van een account, zoals implementaties en bij het maken en beheren van waarschuwingen beheren. De rol Inzender kunt toevoegen of verwijderen van rollen. |
| Beheerder van gebruikerstoegang |Toevoegen en verwijderen van rollen |Beheerst door de ACL |De rol beheerder voor gebruikerstoegang kunt gebruikerstoegang tot accounts beheren. |

Zie voor instructies [gebruikers of beveiligingsgroepen toewijzen aan Data Lake Store-accounts](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>Met behulp van ACL's voor bewerkingen voor bestandssystemen
Data Lake Store is een hiërarchische bestandssysteem zoals Hadoop Distributed File System (HDFS) en het ondersteunt [POSIX-ACL's](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Hiermee kunt u lezen (r), (b) schrijven en uitvoeren van (machtigingen voor de resources voor de rol van eigenaar, voor de groep eigenaren en voor andere gebruikers en groepen x). In de openbare preview van de Data Lake Store (de huidige release) kunnen ACL's worden ingeschakeld voor de hoofdmap, submappen en afzonderlijke bestanden. Zie voor meer informatie over de werking van ACL's in de context van Data Lake Store [Toegangsbeheer in Data Lake Store](data-lake-store-access-control.md).

Het is raadzaam dat u voor meerdere gebruikers met behulp van ACL's definiëren [beveiligingsgroepen](../active-directory/active-directory-groups-create-azure-portal.md). Gebruikers toevoegen aan een beveiligingsgroep en vervolgens de ACL's voor een bestand of map toewijzen aan die beveiligingsgroep. Dit is handig wanneer u toegang bieden aangepaste, wilt omdat u beperkt tot maximaal negen items voor aangepaste toegang toe te voegen. Zie voor meer informatie over het beter beveiligen van gegevens die zijn opgeslagen in Data Lake Store met Azure Active Directory-beveiligingsgroepen [gebruikers of beveiligingsgroep als ACL's toewijzen aan het bestandssysteem van Azure Data Lake Store](data-lake-store-secure-data.md#filepermissions).

![Standaard- en aangepaste toegang lijst](./media/data-lake-store-security-overview/adl.acl.2.png "lijst standaard en aangepaste toegang")

## <a name="network-isolation"></a>Netwerkisolatie
Gebruik Data Lake Store voor toegang tot uw data store op het netwerkniveau van het te bepalen. U kunt tot stand brengen van firewalls en een IP-adresbereik opgeven voor uw vertrouwde clients. Met een IP-adresbereik kunnen alleen clients die een IP-adres binnen het gedefinieerde bereik hebt verbonden met Data Lake Store.

![Firewall-instellingen en IP-toegang tot](./media/data-lake-store-security-overview/firewall-ip-access.png "Firewall-instellingen en IP-adres")

## <a name="data-protection"></a>Gegevensbeveiliging
Azure Data Lake Store beschermt u uw gegevens tijdens de levenscyclus. Voor gegevens die onderweg, Data Lake Store de industriestandaard-Transport Layer Security (TLS)-protocol gebruikt voor het beveiligen van gegevens via het netwerk.

![Codering in Data Lake Store](./media/data-lake-store-security-overview/adls-encryption.png "versleuteling in Data Lake Store")

Data Lake Store biedt ook versleuteling voor gegevens die zijn opgeslagen in het account. U kunt ervoor kiezen de gegevens te versleutelen of niet te versleutelen. Als u zich voor versleuteling aanmelden, wordt opgeslagen gegevens in Data Lake Store versleuteld vóór het opslaan van de permanente media. In dat geval Data Lake Store automatisch gegevens voordat het behouden blijven gegevens gecodeerd en gedecodeerd voordat ophalen, dus is het volledig transparant voor de client toegang tot de gegevens. Er is geen codewijziging moet aan de clientzijde gegevens versleutelen/ontsleutelen.

Voor sleutelbeheer biedt Data Lake Store twee modi voor het beheren van uw master versleutelingssleutels (MEKs), die nodig zijn voor het ontsleutelen van gegevens die zijn opgeslagen in de Data Lake Store. U kunt ofwel Data Lake Store beheren van de MEKs voor u of eigendom van de MEKs met uw Azure Key Vault bewaart. U opgeven de modus van Sleutelbeheer terwijl tijdens het maken van een Data Lake Store-account. Zie voor meer informatie over het bieden van versleuteling-gerelateerde configuratie [Aan de slag met Azure Data Lake Store met Azure Portal](data-lake-store-get-started-portal.md).

## <a name="auditing-and-diagnostic-logs"></a>Controle en diagnostische logboeken
U kunt controleren of diagnostische logboeken, afhankelijk van of u op zoek naar de logboeken voor incidentbeheer activiteiten of gegevens-gerelateerde activiteiten.

* Management-gerelateerde activiteiten gebruik van Azure Resource Manager-API's en in de Azure portal worden opgehaald via de controlelogboeken.
* Data-gerelateerde activiteiten gebruik van WebHDFS REST-API's en in de Azure portal via Logboeken met diagnostische gegevens worden opgehaald.

### <a name="auditing-logs"></a>Controlelogboeken
Om te voldoen aan de voorschriften, mogelijk een organisatie adequaat audittrails dat deze moeten verdiepen in specifieke incidenten. Data Lake Store is ingebouwde bewaking en controle en registreert alle account beheeractiviteiten.

Voor audittrails account management, weergeven en kies de kolommen die u wilt registreren. U kunt ook controlelogboeken exporteren naar Azure Storage.

![Controlelogboeken](./media/data-lake-store-security-overview/audit-logs.png "Controlelogboeken")

### <a name="diagnostic-logs"></a>Diagnostische logboeken
U kunt toegang tot gegevens audittrails in de Azure portal (in de diagnostische instellingen) en maken van een Azure Blob storage-account waarin de logboeken worden opgeslagen.

![Diagnostische logboeken](./media/data-lake-store-security-overview/diagnostic-logs.png "diagnostische logboeken")

Nadat u de diagnostische instellingen configureert, kunt u de logboeken weergeven op de **diagnostische logboeken** tabblad.

Zie voor meer informatie over het werken met Logboeken met diagnostische gegevens met Azure Data Lake Store [toegang tot diagnoselogboeken voor Data Lake Store](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Samenvatting
Enterprise-klanten vereisen een cloud-platform voor gegevens analytics is veilig en eenvoudig te gebruiken. Azure Data Lake Store is ontworpen om die deze vereisten via identiteitsbeheer en verificatie via Azure Active Directory-integratie, op basis van ACL-autorisatie, netwerkisolatie, gegevensversleuteling onderweg en op de rest-(binnenkort in de toekomst ), en controle.

Als u zien van nieuwe functies in Data Lake Store wilt, stuur ons uw feedback in de [Data Lake Store UserVoice forum](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure Data Lake Store](data-lake-store-overview.md)
* [Aan de slag met Data Lake Store](data-lake-store-get-started-portal.md)
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)

