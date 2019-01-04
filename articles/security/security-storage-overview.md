---
title: Beveiligingsfuncties die kunnen worden gebruikt met Azure Storage | Microsoft Docs
description: Dit artikel bevat een overzicht van de belangrijkste functies van Azure-beveiliging die kunnen worden gebruikt met Azure Storage.
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
ms.date: 10/29/2018
ms.author: terrylan
ms.openlocfilehash: 55686972979c5ac3015802c340cef7dffebdb9bd
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53973125"
---
# <a name="azure-storage-security-overview"></a>Overzicht van beveiliging van Azure Storage

Azure Storage is de oplossing voor opslag in de cloud voor moderne toepassingen die afhankelijk zijn van duurzaamheid, beschikbaarheid en schaalbaarheid om te voldoen aan de behoeften van klanten. Azure Storage biedt een uitgebreide set met mogelijkheden voor beveiliging. U kunt:

* Het storage-account beveiligen met behulp van Role-Based Access Control (RBAC) en Azure Active Directory.
* Gegevens in transit tussen een toepassing en Azure beveiligen met behulp van versleuteling van de client-side-, HTTPS- of SMB 3.0.
* Gegevens worden automatisch versleuteld wanneer ze worden geschreven naar Azure Storage met behulp van Storage Service Encryption instellen.
* Set besturingssysteem en gegevensschijven die wordt gebruikt door virtuele machines (VM's) moeten worden versleuteld met behulp van Azure Disk Encryption.
* Gedelegeerde toegang verlenen tot de gegevensobjecten in Azure Storage met behulp van handtekeningen voor gedeelde toegang (SASs).
* Analytics gebruiken voor het bijhouden van de verificatiemethode die wordt gebruikt wanneer ze toegang hebben tot opslag.

Zie voor een meer gedetailleerde Kijk op beveiliging in Azure Storage, de [Azure Storage-beveiligingshandleiding](../storage/common/storage-security-guide.md). Deze handleiding biedt een dieper ingaan op de beveiligingsfuncties van Azure Storage. Deze functies omvatten opslagaccountsleutels gegevensversleuteling in-transit en at-rest en opslaganalyse.


Dit artikel bevat een overzicht van Azure-beveiliging-functies die u met Azure Storage gebruiken kunt. Koppelingen naar artikelen Geef details van elke functie zodat u kunt meer informatie.

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

U kunt uw storage-account beveiligen met behulp van Role-Based Access Control. Toegang beperken op basis van de [moet weten](https://en.wikipedia.org/wiki/Need_to_know) en [minimale bevoegdheden](https://en.wikipedia.org/wiki/Principle_of_least_privilege) beveiligingsprincipes is van cruciaal belang voor organisaties die willen beveiligingsbeleid voor toegang tot gegevens afdwingen. Deze toegangsrechten worden verleend door de juiste RBAC-rol toewijzen aan groepen en toepassingen met een bepaald bereik. U kunt [ingebouwde RBAC-rollen](../role-based-access-control/built-in-roles.md), zoals Inzender voor Opslagaccounts, machtigingen toewijzen aan gebruikers.

Meer informatie:

* [Azure Active Directory-rollen gebaseerd toegangsbeheer](../role-based-access-control/role-assignments-portal.md)

## <a name="delegated-access-to-storage-objects"></a>Gedelegeerde toegang tot opslagobjecten

Een shared access signature biedt gedelegeerde toegang tot resources in uw opslagaccount. De SAS betekent dat u een client beperkte machtigingen voor objecten in uw storage-account voor een opgegeven periode en met een opgegeven set machtigingen kunt verlenen. U kunt deze beperkte rechten verlenen zonder dat u hoeft voor het delen van de toegangssleutels van uw account. 

De SAS is een URI die in de queryparameters alle informatie die nodig zijn voor geverifieerde toegang tot een opslagresource omvat. Voor toegang tot de storage-resources met de SAS, moet de client alleen voor de SAS is de juiste constructor of de methode.

Meer informatie:

* [Inzicht in het SAS-model](../storage/common/storage-dotnet-shared-access-signature-part-1.md)
* [Maken en een SAS gebruiken met Blob-opslag](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>Versleuteling tijdens overdracht

Versleuteling tijdens overdracht is een mechanisme om gegevens te beveiligen wanneer deze worden verzonden tussen netwerken. Met Azure Storage, kunt u gegevens beveiligen met behulp van:

* [Versleuteling op transportniveau](../storage/common/storage-security-guide.md#encryption-in-transit), zoals HTTPS, wanneer u gegevens naar of uit Azure Storage overbrengen.
* [Wire-versleuteling](../storage/common/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), zoals SMB 3.0-versleuteling voor Azure-bestandsshares.
* [Clientversleuteling](../storage/common/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage), voor het versleutelen van de gegevens voordat deze wordt overgedragen naar de opslag en de gegevens nadat deze zijn overgebracht geen opslagruimte meer te ontsleutelen.

Meer informatie over client-side-versleuteling:

* [Client-Side Encryption voor Microsoft Azure Storage](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
* [Cloudbeveiliging besturingselementen reeks: Versleutelen van gegevens die onderweg zijn](https://cloudblogs.microsoft.com/microsoftsecure/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Versleuteling 'at rest'

Voor veel organisaties [gegevensversleuteling in rust](https://cloudblogs.microsoft.com/microsoftsecure/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) is een verplichte stap voor privacy, naleving en onafhankelijkheid van gegevens. Drie Azure-functies bieden versleuteling van gegevens die zich in rust:

* [Storage-Serviceversleuteling](../storage/common/storage-security-guide.md#encryption-at-rest) kunt u om aan te vragen die de storage-service gegevens automatisch versleuteld bij het schrijven van deze naar Azure Storage.
* [Clientversleuteling](../storage/common/storage-security-guide.md#client-side-encryption) biedt ook de functie van versleuteling-at-rest.
* [Azure Disk Encryption](../storage/common/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) kunt u de besturingssysteemschijven en gegevensschijven die gebruikmaakt van een IaaS-machine versleutelen.

Meer informatie over Storage Service Encryption:

* [Azure Storage-Serviceversleuteling](https://azure.microsoft.com/services/storage/) is beschikbaar voor [Azure Blob-opslag](https://azure.microsoft.com/services/storage/blobs/). Zie voor meer informatie over andere Azure storage-typen, [Azure Files](https://azure.microsoft.com/services/storage/files/), [(Premium-opslag)](https://azure.microsoft.com/services/storage/premium-storage/), [Table storage](https://azure.microsoft.com/services/storage/tables/), en [Queue storage](https://azure.microsoft.com/services/storage/queues/).
* [Azure Storage-Serviceversleuteling voor Data-at-Rest](../storage/common/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Azure Disk Encryption

Azure Disk Encryption voor virtuele machines kunt u adres organisatie beveiligings- en nalevingsvereisten. Het de VM-schijven (met inbegrip van de opstart- en schijven) versleuteld met sleutels en beleidsregels die u beheert in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).

Schijfversleuteling voor virtuele machines werkt voor Linux en Windows-besturingssystemen. Key Vault worden ook gebruikt om te beveiligen, beheren en het gebruik van uw sleutels controleren. Alle gegevens in de VM-schijven worden in rust versleuteld met behulp van industriestandaard versleutelingstechnologie in Azure storage-accounts. Het Disk Encryption-oplossing voor Windows is gebaseerd op [Microsoft BitLocker-stationsversleuteling](https://technet.microsoft.com/library/cc732774.aspx), en de Linux-oplossing is gebaseerd op [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Meer informatie

* [Azure Disk Encryption voor Windows en Linux IaaS-Machines](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="firewalls-and-virtual-networks"></a>Firewalls en virtuele netwerken

Azure-opslag kunt u firewallregels voor uw storage-accounts inschakelen. Eenmaal ingeschakeld ze binnenkomende aanvragen voor gegevens, inclusief aanvragen van andere Azure-services worden geblokkeerd. U kunt configureren dat uitzonderingen om verkeer te staan. Firewall-regels kunnen worden ingeschakeld op de bestaande opslagaccounts of tijdens het aanmaken.

U moet deze functionaliteit gebruiken voor het beveiligen van uw storage-accounts naar een specifieke set toegestane netwerken.

Lees het artikel voor meer informatie over Azure storage firewalls en virtuele netwerken [en virtuele netwerken voor Azure Storage-Firewalls configureren](../storage/common/storage-network-security.md)

## <a name="azure-data-box"></a>Azure Data Box

Met apparaten met Data Box, Data Box Disk of Data Box Heavy kunt u grote hoeveelheden gegevens overdragen naar Azure als overdracht via uw netwerk geen optie is. Deze apparaten van de overdracht offlinesynchronisatie van gegevens worden verzonden tussen uw organisatie en het Azure-datacentrum. De apparaten maken gebruik van AES-versleuteling om uw gegevens tijdens de overdracht te beveiligen en ondergaan na het uploaden een grondig opschoningsproces om uw gegevens van het apparaat te verwijderen.

Data Box Edge en Data Box Gateway zijn producten voor onlinegegevensoverdracht die fungeren als netwerkopslaggateways om gegevens tussen uw site en Azure te beheren. Met Data Box Edge, een on-premises netwerkapparaat, worden gegevens overdragen van en naar Azure en worden gegevens verwerkt met behulp van Edge-rekenprocessen met artificial intelligence (AI). Data Box Gateway is een virtueel apparaat met opslaggatewaymogelijkheden.

Meer informatie:

* [Azure Data Box](https://azure.microsoft.com/services/storage/databox/)
* [Azure Data Box Edge](../databox-online/data-box-edge-overview.md)
* [Azure Data Box-Gateway](..//databox-online/data-box-gateway-overview.md)

## <a name="advanced-threat-protection"></a>Advanced Threat Protection

Azure Storage biedt Advanced Threat Protection voor een extra laag met beveiligingsinformatie die ongebruikelijke en potentieel schadelijke pogingen om toegang tot of misbruik te maken van uw storage-account wordt gedetecteerd. Advanced Threat Protection monitoren diagnostische logboeken van Azure Storage voor verdachte lezen, schrijven of te verwijderen van aanvragen naar Blob-opslag. 

Geavanceerde Threat Protection-waarschuwingen kunnen worden bekeken via [Azure Security Center](https://azure.microsoft.com/services/security-center/). Azure Security Center biedt meer informatie over elke verdachte activiteit gedetecteerd en ontvangt u aanbevelingen om te onderzoeken en oplossen van de mogelijke bedreiging. 

Meer informatie:

* [Azure Advanced Threat Protection overzicht-opslag](../storage/common/storage-advanced-threat-protection.md)

## <a name="azure-key-vault"></a>Azure Key Vault

Azure Disk Encryption gebruikt [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) om te controleren en beheren van sleutels en geheimen in uw key vault-abonnement. Ook zorgt u ervoor dat alle gegevens in de virtuele-machineschijven zijn versleuteld in rust in Azure Storage. U moet Key Vault gebruiken voor het controleren van sleutels en het gebruik van beleid.

Meer informatie

* [Wat is Azure Key Vault?](../key-vault/key-vault-whatis.md)
* [Aan de slag met Azure Key Vault](../key-vault/key-vault-get-started.md)
