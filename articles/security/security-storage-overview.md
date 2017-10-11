---
title: Beveiligingsfuncties die kunnen worden gebruikt met Azure Storage | Microsoft Docs
description: " In dit artikel biedt een overzicht van de kern van het Azure-beveiligingsfuncties die kunnen worden gebruikt met Azure Storage. "
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
ms.date: 01/23/2017
ms.author: terrylan
ms.openlocfilehash: da28cbf5f6f91df1f89114a63bc3f2ebac0f6d73
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="azure-storage-security-overview"></a>Overzicht van Azure-opslag-beveiliging
Azure Storage is de oplossing voor opslag in de cloud voor moderne toepassingen die afhankelijk zijn van duurzaamheid, beschikbaarheid en schaalbaarheid om te voldoen aan de behoeften van klanten. Azure Storage biedt een uitgebreide reeks beveiligingsmogelijkheden:

* Het opslagaccount kan worden beveiligd met op rollen gebaseerde toegangsbeheer en Azure Active Directory.
* Gegevens kunnen worden beveiligd tijdens de overdracht tussen een toepassing en Azure met behulp van versleuteling van de Client-Side-, HTTPS- of SMB 3.0.
* Gegevens kunnen worden ingesteld op automatisch worden versleuteld wanneer geschreven naar Azure Storage met behulp van versleuteling van de opslagruimte.
* Besturingssysteem en gegevensschijven die worden gebruikt door virtuele machines kunnen worden ingesteld om te worden versleuteld met Azure Disk Encryption.
* Gedelegeerde toegang tot de gegevensbron-objecten in Azure Storage kan worden verleend met behulp van handtekeningen voor gedeelde toegang.
* De verificatiemethode die door iemand wordt gebruikt wanneer ze toegang hebben tot opslag kan worden bijgehouden met Storage analytics.

Zie voor meer gedetailleerde kijken beveiliging in Azure Storage, de [Azure Storage-beveiligingshandleiding](../storage/common/storage-security-guide.md). Deze handleiding biedt een diepgaand in de beveiligingsfuncties van Azure Storage zoals toegangscodes voor opslag, gegevensversleuteling onderweg en op rest en storage analytics.

Dit artikel bevat een overzicht van Azure-beveiligingsfuncties die kunnen worden gebruikt met Azure Storage. Koppelingen zijn opgegeven voor artikelen die details van elk onderdeel doet geven voor meer informatie.

Hier volgen de belangrijkste functies in dit artikel aan bod:

* Op rollen gebaseerd toegangsbeheer
* Gedelegeerde toegang tot opslagobjecten
* Codering in transit
* Versleuteling op rest/Storage-Service: versleuteling
* Azure Disk Encryption
* Azure Key Vault

## <a name="role-based-access-control-rbac"></a>RBAC (op rollen gebaseerd toegangsbeheer)
U kunt uw storage-account met op rollen gebaseerde toegangsbeheer (RBAC) beveiligen. De toegang beperken op basis van de [moet weten](https://en.wikipedia.org/wiki/Need_to_know) en [minimale bevoegdheden](https://en.wikipedia.org/wiki/Principle_of_least_privilege) beveiligingsprincipes is van cruciaal belang voor organisaties die willen beveiligingsbeleid instellen voor toegang tot gegevens. Deze toegangsrechten worden verleend door de juiste RBAC-rol toewijzen aan groepen en toepassingen op een bepaalde scope. U kunt [ingebouwde RBAC-rollen](../active-directory/role-based-access-built-in-roles.md), zoals opslag Account Inzender rechten toewijzen aan gebruikers.

Meer informatie:

* [Toegangsbeheer op basis van rollen in Azure Active Directory](../active-directory/role-based-access-control-configure.md)

## <a name="delegated-access-to-storage-objects"></a>Gedelegeerde toegang tot opslagobjecten
Een shared access signature (SAS) biedt gedelegeerde toegang tot bronnen in uw opslagaccount. De SAS betekent dat u een client beperkte machtigingen voor objecten in uw storage-account gedurende een bepaalde tijd en met een opgegeven set machtigingen kunt verlenen. U kunt deze beperkte rechten verlenen zonder te delen van de toegangssleutels van uw account. De SAS is een URI die de gegevens die zijn vereist voor geverifieerde toegang tot een opslagresource, in de queryparameters omvat. Voor toegang tot de storage-resources met de SA's, moet de client alleen voor de SAS naar de juiste constructor of methode.

Meer informatie:

* [Inzicht in het SAS-model](../storage/common/storage-dotnet-shared-access-signature-part-1.md)
* [Maken en gebruiken van een SAS met Blob-opslag](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>Codering in transit
Versleuteling onderweg is een mechanisme om gegevens te beveiligen wanneer deze worden verzonden via netwerken. U kunt beveiligen met behulp van gegevens met Azure Storage:

* [Versleuteling transportniveau](../storage/common/storage-security-guide.md#encryption-in-transit), zoals HTTPS wanneer u gegevens van of naar Azure Storage overbrengen.
* [Versleuteling Wire](../storage/common/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), zoals versleuteling door SMB 3.0 voor Azure-bestandsshares.
* [Versleuteling aan clientzijde](../storage/common/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage), voor het versleutelen van de gegevens voordat deze worden overgedragen naar de opslag en de gegevens te decoderen nadat deze zijn overgebracht buiten-opslag.

Meer informatie over client-side '-versleuteling:

* [Client-Side-versleuteling voor opslag op Microsoft Azure](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
* [Cloud-beveiligingsmaatregelen reeks: versleutelen van gegevens die onderweg](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Versleuteling 'at rest'
Voor veel organisaties [gegevensversleuteling in rust](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) is een verplichte stap naar privacy, naleving en gegevens onafhankelijkheid van gegevens. Er zijn drie Azure-functies waarmee de codering van gegevens die zich 'in rust':

* [Versleuteling van de opslagruimte](../storage/common/storage-security-guide.md#encryption-at-rest) Hiermee kunt u aanvragen dat de storage-service gegevens automatisch coderen wanneer het schrijven naar Azure Storage.
* [Versleuteling aan clientzijde](../storage/common/storage-security-guide.md#client-side-encryption) biedt ook de functie van versleuteling in rust.
* [Azure Disk Encryption](../storage/common/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) kunt u voor het versleutelen van de OS-schijven en gegevensschijven die wordt gebruikt door een virtuele machine voor IaaS.

Meer informatie over versleuteling van de opslagruimte:

* [Azure Storage-Service: versleuteling](https://azure.microsoft.com/services/storage/) is beschikbaar voor [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/). Zie voor informatie over andere typen Azure-opslag, [bestand](https://azure.microsoft.com/services/storage/files/), [schijf (Premium-opslag)](https://azure.microsoft.com/services/storage/premium-storage/), [tabel](https://azure.microsoft.com/services/storage/tables/), en [wachtrij](https://azure.microsoft.com/services/storage/queues/).
* [Azure Storage-Service: versleuteling voor gegevens in rust](../storage/common/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Azure Disk Encryption
Azure Disk Encryption voor virtuele machines (VM's) kunt u bedrijfsbeveiligingsbeleid adres- en nalevingsvereisten door uw VM-schijven (inclusief opstart- en gegevensschijven) te versleutelen met sleutels en beleidsregels die u beheert in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).

Schijfversleuteling voor virtuele machines werkt voor Linux en Windows-besturingssystemen. Sleutelkluis worden ook gebruikt om te beschermen, beheren en het gebruik van uw schijf versleutelingssleutels controleren. Alle gegevens in uw VM-schijven in rust versleuteld met behulp van technologie voor bestandsversleuteling industriestandaard in uw Azure Storage-accounts. De oplossing schijfversleuteling voor Windows is gebaseerd op [Microsoft BitLocker-stationsversleuteling](https://technet.microsoft.com/library/cc732774.aspx), en de Linux-oplossing is gebaseerd op [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Meer informatie:

* [Azure Disk Encryption for Windows and Linux IaaS virtuele Machines](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="azure-key-vault"></a>Azure Key Vault
Maakt gebruik van Azure Disk Encryption [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) om te controleren en beheren van schijf versleutelingssleutels en geheimen in uw abonnement sleutelkluis, terwijl u ervoor te zorgen dat alle gegevens in de virtuele machine-schijven zijn versleuteld in rust in uw Azure Opslag. Sleutelkluis moet u gebruiken voor het controleren van sleutels en gebruik van beleidsregels.

Meer informatie:

* [Wat is Azure Sleutelkluis?](../key-vault/key-vault-whatis.md)
* [Aan de slag met Azure Sleutelkluis](../key-vault/key-vault-get-started.md)
