---
title: Overzicht van de beveiliging in Data Lake Store | Microsoft Docs
description: Informatie over hoe Azure Data Lake Store is een veiligere big data-archief
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ebd5b2ac-c5cc-46d4-9cfd-1a1ee70024c2
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: nitinme
ms.openlocfilehash: 3b836859dd2af45e84d0f53db06d2c7ab2828253
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44057050"
---
# <a name="security-in-azure-data-lake-store"></a>Beveiliging in Azure Data Lake Store
Veel ondernemingen profiteren van big data-analyses voor zakelijke inzichten zodat ze slimme besluiten. Een organisatie kan een complexe en gereguleerde omgeving, met een toenemend aantal diverse gebruikers hebben. Het is essentieel voor een onderneming om ervoor te zorgen dat kritieke bedrijfsgegevens veiliger, worden opgeslagen met het juiste niveau van toegang verleend aan individuele gebruikers. Azure Data Lake Store is ontworpen om te voldoen aan deze beveiligingsvereisten. In dit artikel meer informatie over de beveiligingsfuncties van Data Lake Store, met inbegrip van:

* Verificatie
* Autorisatie
* Netwerkisolatie
* Gegevensbeveiliging
* Controleren

## <a name="authentication-and-identity-management"></a>Verificatie en identiteitsbeheer
Verificatie is het proces waarmee de identiteit van een gebruiker is geverifieerd wanneer de gebruiker communiceert met Data Lake Store of met een service die verbinding met Data Lake Store maakt. Voor identiteitsbeheer en authenticatie, het gebruik van Data Lake Store [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md), een uitgebreide identiteits- en toegangsbeheer cloudoplossing die het beheer van gebruikers en groepen vereenvoudigt.

Elk Azure-abonnement kan worden gekoppeld aan een exemplaar van Azure Active Directory. Alleen gebruikers en service-identiteiten die zijn gedefinieerd in uw Azure Active Directory-service hebben toegang tot uw Data Lake Store-account met behulp van Azure portal, opdrachtregelprogramma's, of via de clienttoepassingen uw organisatie worden gebouwd met behulp van Azure Data Lake Store SDK. Belangrijkste voordelen van het gebruik van Azure Active Directory als een gecentraliseerde mechanisme voor toegangsbeheer zijn:

* Vereenvoudigd beheer van identiteitslevenscycli. De identiteit van een gebruiker of een service (een service principal identiteit) worden snel gemaakt en snel door gewoon verwijderen of uitschakelen van de account in de map is ingetrokken.
* Meervoudige verificatie. [Meervoudige verificatie](../active-directory/authentication/multi-factor-authentication.md) biedt een extra beveiligingslaag voor gebruikersaanmeldingen en transacties.
* Verificatie van elke client via een open standaard protocol, zoals OAuth of OpenID.
* Federatie met enterprise directoryservices en cloud-id-providers.

## <a name="authorization-and-access-control"></a>Autorisatie en toegangsbeheer
Nadat u Azure Active Directory wordt een gebruiker geverifieerd, zodat de gebruiker toegang heeft tot Azure Data Lake Store, toegangsmachtigingen besturingselementen voor autorisatie voor Data Lake Store. Data Lake Store scheidt autorisatie voor account-gerelateerd en -gegevens activiteiten in de volgende manier:

* [Op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md) (RBAC) verstrekt door Azure voor accountbeheer
* POSIX ACL voor toegang tot gegevens in de store

### <a name="rbac-for-account-management"></a>RBAC voor accountbeheer
Vier eenvoudige rollen worden standaard gedefinieerd voor Data Lake Store. De rollen kunnen verschillende bewerkingen op een Data Lake Store-account via de Azure portal, PowerShell-cmdlets en REST-API's. De rollen eigenaar en Inzender kunnen u een verscheidenheid aan functies voor beheer uitvoeren op het account. U kunt de rol van lezer toewijzen aan gebruikers die alleen account management gegevens weergeven.

![RBAC-rollen](./media/data-lake-store-security-overview/rbac-roles.png "RBAC-rollen")

Houd er rekening mee dat hoewel rollen zijn toegewezen voor accountbeheer, sommige rollen gevolgen heeft voor toegang tot gegevens. U moet de ACL's gebruiken voor het beheren van toegang tot de bewerkingen die een gebruiker op het bestandssysteem uitvoeren kan. De volgende tabel bevat een overzicht van de rights management en gegevens toegangsrechten voor de standaardrollen.

| Rollen | Rights management | Toegangsrechten voor gegevens | Uitleg |
| --- | --- | --- | --- |
| Er is geen rol die is toegewezen |Geen |Beheerst door de ACL |De gebruiker niet de Azure portal of Azure PowerShell-cmdlets gebruiken voor het Data Lake Store te bladeren. De gebruiker kan alleen de opdrachtregelprogramma's gebruiken. |
| Eigenaar |Alle |Alle |De rol van eigenaar is een supergebruiker. Deze rol kan alles beheren, en heeft volledige toegang tot gegevens. |
| Lezer |Alleen-lezen |Beheerst door de ACL |De rol van lezer kan alles met betrekking tot accountbeheer, zoals welke gebruiker is toegewezen aan welke rol weergeven. De rol van lezer kunt geen wijzigingen aanbrengen. |
| Inzender |Overal behalve in rollen toevoegen en verwijderen |Beheerst door de ACL |De rol Inzender kunt bepaalde aspecten van een account, zoals implementaties en het maken en beheren van waarschuwingen beheren. De rol Inzender geen toevoegen of verwijderen van rollen. |
| Beheerder van gebruikerstoegang |Toevoegen en verwijderen van rollen |Beheerst door de ACL |De rol Administrator voor gebruikerstoegang kunt toegang tot accounts voor gebruikers beheren. |

Zie voor instructies [gebruikers of beveiligingsgroepen toewijzen aan de Data Lake Store-accounts](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>Met behulp van ACL's voor bewerkingen voor bestandssystemen
Data Lake Store is een hiërarchische bestandssysteem, zoals Hadoop Distributed File System (HDFS) en deze ondersteunt [POSIX ACL's](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Hiermee kunt u lezen (r), schrijven (s) en uitvoeren (x) machtigingen voor resources voor de rol van eigenaar, voor de groep eigenaren en voor andere gebruikers en groepen. In Data Lake Store, kunnen de ACL's worden ingeschakeld voor de hoofdmap, submappen en afzonderlijke bestanden. Zie voor meer informatie over de werking van ACL's in de context van Data Lake Store [Toegangsbeheer in Data Lake Store](data-lake-store-access-control.md).

Het is raadzaam dat u ACL's voor meerdere gebruikers met behulp van definiëren [beveiligingsgroepen](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). Gebruikers toevoegen aan een beveiligingsgroep en vervolgens de ACL's voor een bestand of map toewijzen aan die beveiligingsgroep. Dit is handig als u bieden toegewezen machtigingen, wilt omdat u beperkt tot een maximum van 28-vermeldingen voor toegewezen machtigingen bent. Zie voor meer informatie over hoe u gegevens die zijn opgeslagen in Data Lake Store met behulp van Azure Active Directory-beveiligingsgroepen beter kunt beveiligen, [gebruikers of beveiligingsgroep als ACL's toewijzen aan het Azure Data Lake Store-bestandssysteem](data-lake-store-secure-data.md#filepermissions).

![Lijst met machtigingen voor toegang](./media/data-lake-store-security-overview/adl.acl.2.png "toegangsmachtigingen van lijst")

## <a name="network-isolation"></a>Netwerkisolatie
Data Lake Store gebruiken voor toegang tot uw gegevensopslag op het niveau van het netwerk te bepalen. U kunt tot stand brengen firewalls en een IP-adresbereik definiëren voor uw vertrouwde clients. Een IP-adresbereik, alleen clients met een IP-adres binnen het gedefinieerde bereik kunnen verbinding maken met Data Lake Store.

![Firewall-instellingen en IP-toegang hebben tot](./media/data-lake-store-security-overview/firewall-ip-access.png "Firewall-instellingen en IP-adres")

## <a name="data-protection"></a>Gegevensbeveiliging
Azure Data Lake Store beschermt uw gegevens in de gehele levenscyclus. Data Lake Store maakt voor gegevens die onderweg zijn gebruik van het standaardprotocol Transport Layer Security (TLS 1.2) voor het beveiligen van gegevens via het netwerk.

![Versleuteling in Data Lake Store](./media/data-lake-store-security-overview/adls-encryption.png "versleuteling in Data Lake Store")

Data Lake Store biedt ook versleuteling voor gegevens die zijn opgeslagen in het account. U kunt ervoor kiezen de gegevens te versleutelen of niet te versleutelen. Als u zich voor versleuteling aanmelden, worden gegevens die zijn opgeslagen in Data Lake Store is versleuteld vóór het opslaan op permanente media. In dat geval, Data Lake Store versleutelt automatisch gegevens voordat deze worden opgeslagen en ontsleutelt gegevens voordat ze worden opgehaald, dus het is volledig transparant voor de client toegang tot de gegevens. Er is er zijn geen codewijzigingen vereist op de client voor versleutelen/ontsleutelen van gegevens.

Voor sleutelbeheer biedt Data Lake Store twee modi voor het beheren van uw hoofdversleutelingssleutels (MEK's), die vereist zijn voor het ontsleutelen van gegevens die zijn opgeslagen in de Data Lake Store. U kunt ofwel Data Lake Store beheren van de MEK's voor u, of kies eigendom van de MEK's met uw Azure Key Vault-account behouden. U opgeven de modus van Sleutelbeheer terwijl tijdens het maken van een Data Lake Store-account. Zie voor meer informatie over het bieden van versleuteling-gerelateerde configuratie [Aan de slag met Azure Data Lake Store met Azure Portal](data-lake-store-get-started-portal.md).

## <a name="activity-and-diagnostic-logs"></a>Diagnostische logboeken en activiteit
U kunt de activiteit of diagnostische logboeken, afhankelijk van of u op zoek naar de logboeken voor account-gerelateerde activiteiten of gegevens gerelateerde activiteiten.

* Account voor activiteiten met betrekking tot beheer van Azure Resource Manager-API's gebruiken en worden weergegeven in Azure portal via activiteitenlogboeken.
* Activiteiten met betrekking tot gegevens WebHDFS REST-API's gebruiken en in Azure portal via Logboeken met diagnostische gegevens worden opgehaald.

### <a name="activity-log"></a>Activiteitenlogboek
Om te voldoen aan de regelgeving, mogelijk een organisatie adequaat audittrails van beheeractiviteiten account als nodig is om u te verdiepen in specifieke incidenten. Data Lake Store heeft ingebouwde controle en registreert alle activiteiten van de account management.

Voor account management audittrails, weergeven en kies de kolommen die u wilt registreren. U kunt ook activiteitenlogboeken exporteren naar Azure Storage.

![Activiteitenlogboek](./media/data-lake-store-security-overview/activity-logs.png "activiteitenlogboek")

Zie voor meer informatie over het werken met activiteitenlogboeken [activiteitenlogboeken om te controleren van acties op resources bekijken](../azure-resource-manager/resource-group-audit.md).

### <a name="diagnostics-logs"></a>Logboeken met diagnostische gegevens
U kunt de controle van de gegevens en diagnostische gegevens vastleggen in Azure portal inschakelen en de logboeken verzenden naar een Azure Blob storage-account, een event hub of Log Analytics.

![Logboeken met diagnostische gegevens](./media/data-lake-store-security-overview/diagnostic-logs.png "logboeken met diagnostische gegevens")

Zie voor meer informatie over het werken met diagnostische logboeken met Azure Data Lake Store [diagnostische logboeken openen voor Data Lake Store](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Samenvatting
Enterprise-klanten de vraag een data analytics-cloudplatform dat u veilig en eenvoudig te gebruiken. Azure Data Lake Store is ontworpen om u te helpen met het adres van deze vereisten via het identiteitsbeheer en verificatie via Azure Active Directory-integratie, op basis van claims ACL, netwerkisolatie, gegevensversleuteling in-transit en inactieve en controle.

Als u zien van nieuwe functies in Data Lake Store wilt, stuur ons uw feedback in de [Data Lake Store UserVoice-forum](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure Data Lake Store](data-lake-store-overview.md)
* [Aan de slag met Data Lake Store](data-lake-store-get-started-portal.md)
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)

