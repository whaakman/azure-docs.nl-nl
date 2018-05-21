---
title: Beveiligingsfuncties die kunnen worden gebruikt met Azure Storage | Microsoft Docs
description: In dit artikel biedt een overzicht van de kern van het Azure-beveiligingsfuncties die kunnen worden gebruikt met Azure Storage.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: 9558f1ec0d8ccd83da764a0967fa83d93e1e6a02
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="azure-storage-security-overview"></a>Overzicht van Azure Storage-beveiliging
Azure Storage is de oplossing voor opslag in de cloud voor moderne toepassingen die afhankelijk zijn van duurzaamheid, beschikbaarheid en schaalbaarheid om te voldoen aan de behoeften van klanten. Azure Storage biedt een uitgebreide set met beveiligingsmogelijkheden voor. U kunt:

* Het storage-account beveiligen met behulp van op rollen gebaseerde toegangsbeheer (RBAC) en Azure Active Directory.
* De gegevens onderweg tussen een toepassing en Azure beveiligen met behulp van versleuteling van de client-side '-, HTTPS- of SMB 3.0.
* Gegevens worden automatisch versleuteld wanneer deze wordt geschreven naar Azure Storage met behulp van versleuteling van opslag-Service instellen.
* Set besturingssysteem en gegevensschijven die worden gebruikt door virtuele machines (VM's) worden versleuteld met behulp van Azure Disk Encryption.
* Gedelegeerde toegang verlenen tot de gegevensbron-objecten in Azure Storage met behulp van handtekeningen voor gedeelde toegang (SASs).
* Analytics voor het bijhouden van de verificatiemethode die wordt gebruikt wanneer ze toegang hebben tot opslag gebruiken.

Zie voor meer gedetailleerde kijken beveiliging in Azure Storage, de [Azure Storage-beveiligingshandleiding](../storage/common/storage-security-guide.md). Deze handleiding biedt een diepgaand in de beveiligingsfuncties van Azure Storage. Deze functies omvatten toegangscodes voor opslag, gegevensversleuteling onderweg en op rest en storage analytics.

Dit artikel bevat een overzicht van Azure-beveiligingsfuncties die u met Azure Storage gebruiken kunt. Koppelingen naar artikelen geven details van elk onderdeel, zodat u meer informatie.

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer
U kunt uw storage-account beveiligen met behulp van toegangsbeheer op basis van rollen. De toegang beperken op basis van de [moet weten](https://en.wikipedia.org/wiki/Need_to_know) en [minimale bevoegdheden](https://en.wikipedia.org/wiki/Principle_of_least_privilege) beveiligingsprincipes is van cruciaal belang voor organisaties die willen beveiligingsbeleid instellen voor toegang tot gegevens. Deze toegangsrechten worden verleend door de juiste RBAC-rol toewijzen aan groepen en toepassingen op een bepaalde scope. U kunt [ingebouwde RBAC-rollen](../role-based-access-control/built-in-roles.md), zoals opslag Account Inzender rechten toewijzen aan gebruikers.

Meer informatie:

* [Azure Active Directory-rollen gebaseerd toegangsbeheer](../role-based-access-control/role-assignments-portal.md)

## <a name="delegated-access-to-storage-objects"></a>Gedelegeerde toegang tot opslagobjecten
Een shared access signature biedt gedelegeerde toegang tot bronnen in uw opslagaccount. De SAS betekent dat u een client beperkte machtigingen voor objecten in uw storage-account voor een opgegeven periode en met een opgegeven set machtigingen kunt verlenen. U kunt deze beperkte rechten verlenen zonder te delen van de toegangssleutels van uw account. 

De SAS is een URI die de gegevens die zijn vereist voor geverifieerde toegang tot een opslagresource, in de queryparameters omvat. Voor toegang tot de storage-resources met de SA's, moet de client alleen voor de SAS naar de juiste constructor of methode.

Meer informatie:

* [Inzicht in het SAS-model](../storage/common/storage-dotnet-shared-access-signature-part-1.md)
* [Maken en gebruiken van een SAS met Blob-opslag](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>Codering in transit
Versleuteling onderweg is een mechanisme om gegevens te beveiligen wanneer deze worden verzonden via netwerken. Met Azure Storage, kunt u gegevens kunt beveiligen met behulp van:

* [Versleuteling transportniveau](../storage/common/storage-security-guide.md#encryption-in-transit), zoals HTTPS, wanneer u gegevens van of naar Azure Storage overbrengen.
* [Versleuteling Wire](../storage/common/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), zoals SMB 3.0-versleuteling voor Azure-bestandsshares.
* [Versleuteling aan clientzijde](../storage/common/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage), voor het versleutelen van de gegevens voordat deze worden overgedragen naar de opslag en de gegevens te decoderen nadat deze zijn overgebracht buiten-opslag.

Meer informatie over client-side '-versleuteling:

* [Client-Side-versleuteling voor opslag op Microsoft Azure](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
* [Cloud-beveiligingsmaatregelen reeks: versleutelen van gegevens die onderweg](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Versleuteling 'at rest'
Voor veel organisaties [gegevensversleuteling in rust](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) is een verplichte stap naar privacy, naleving en gegevens onafhankelijkheid van gegevens. Drie Azure-functies bieden de versleuteling van gegevens in rust:

* [Versleuteling van de opslagruimte](../storage/common/storage-security-guide.md#encryption-at-rest) kunt u aan te vragen of de storage-service gegevens automatisch coderen wanneer het schrijven naar Azure Storage.
* [Versleuteling aan clientzijde](../storage/common/storage-security-guide.md#client-side-encryption) biedt ook de functie van versleuteling in rust.
* [Azure Disk Encryption](../storage/common/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) kunt u de OS-schijven en gegevensschijven die gebruikmaakt van een virtuele machine voor IaaS versleutelen.

Meer informatie over versleuteling van de opslagruimte:

* [Azure Storage-Service: versleuteling](https://azure.microsoft.com/services/storage/) is beschikbaar voor [Azure Blob storage](https://azure.microsoft.com/services/storage/blobs/). Zie voor informatie over andere typen Azure-opslag, [Azure Files](https://azure.microsoft.com/services/storage/files/), [schijf (Premium-opslag)](https://azure.microsoft.com/services/storage/premium-storage/), [tabel opslag](https://azure.microsoft.com/services/storage/tables/), en [opslag in de wachtrij](https://azure.microsoft.com/services/storage/queues/).
* [Azure Storage-Service: versleuteling voor gegevens in rust](../storage/common/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Azure Disk Encryption
Azure Disk Encryption voor virtuele machines kunt u de beveiliging van de organisatie adres- en nalevingsvereisten. Uw VM-schijven (inclusief opstart- en gegevensschijven) gecodeerd met behulp van sleutels en het beleid dat u beheert in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).

Schijfversleuteling voor virtuele machines werkt voor Linux en Windows-besturingssystemen. Sleutelkluis worden ook gebruikt om te beschermen, beheren en het gebruik van uw schijf versleutelingssleutels controleren. Alle gegevens in uw VM-schijven in rust versleuteld met behulp van technologie voor bestandsversleuteling industriestandaard in Azure storage-accounts. De oplossing schijfversleuteling voor Windows is gebaseerd op [Microsoft BitLocker-stationsversleuteling](https://technet.microsoft.com/library/cc732774.aspx), en de Linux-oplossing is gebaseerd op [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Meer informatie:

* [Azure Disk Encryption for Windows and Linux IaaS virtuele Machines](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="azure-key-vault"></a>Azure Key Vault
Maakt gebruik van Azure Disk Encryption [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) om te controleren en beheren van schijf versleutelingssleutels en geheimen in uw abonnement sleutelkluis. Ook zorgt ervoor dat alle gegevens in de virtuele machine-schijven zijn versleuteld in rust in Azure Storage. Sleutelkluis moet u gebruiken voor het controleren van sleutels en gebruik van beleidsregels.

Meer informatie:

* [Wat is Azure Sleutelkluis?](../key-vault/key-vault-whatis.md)
* [Aan de slag met Azure Sleutelkluis](../key-vault/key-vault-get-started.md)
