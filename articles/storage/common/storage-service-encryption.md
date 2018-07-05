---
title: Azure Storage-Serviceversleuteling voor Data-at-Rest | Microsoft Docs
description: De functie Azure Storage-Serviceversleuteling gebruiken voor het versleutelen van Azure Blob-opslag aan de servicezijde bij het opslaan van de gegevens en bij het ophalen van de gegevens ontsleutelen.
services: storage
author: lakasa
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 06/12/2018
ms.author: lakasa
ms.openlocfilehash: d469dfb5092f1269a6600ee8ee2f81778fd83b96
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37449927"
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Azure Storage Service-versleuteling voor inactieve gegevens

Azure Storage-Serviceversleuteling voor Data-at-Rest kunt u uw gegevens om te voldoen aan uw organisatie beveiligings- en nalevingsverplichtingen beveiligen. Met deze functie wordt door Azure Storage automatisch uw gegevens worden versleuteld voordat deze persistent te maken met Azure Storage en ontsleutelt de gegevens voor het ophalen van. De verwerking van versleuteling en versleuteling-at-rest, ontsleuteling en sleutelbeheer in Storage Service-versleuteling is transparant voor gebruikers. Alle gegevens die zijn geschreven naar Azure Storage worden versleuteld met 256-bits [AES-versleuteling](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), een van de krachtigste blokversleutelingsmethoden die er bestaan.

Storage-Serviceversleuteling is ingeschakeld voor alle nieuwe en bestaande opslagaccounts en kan niet worden uitgeschakeld. Omdat uw gegevens beveiligd standaard, hoeft u niet te wijzigen van uw code of toepassingen om te profiteren van Storage Service Encryption.

De functie versleutelt automatisch gegevens in:

- Beide prestatielagen (standaard en Premium).
- Beide implementatiemodellen (Azure Resource Manager en klassiek).
- Alle Azure Storage-services (Blob storage, Queue storage, Table storage en Azure-bestanden). 

Storage-Serviceversleuteling heeft geen invloed op prestaties van Azure Storage.

U kunt Microsoft beheerde sleutels gebruiken met Storage Service Encryption of kunt u uw eigen versleutelingssleutels. Zie voor meer informatie over het gebruik van uw eigen sleutels [Storage Service Encryption door de klant beheerde sleutels in Azure Key Vault](storage-service-encryption-customer-managed-keys.md).

## <a name="view-encryption-settings-in-the-azure-portal"></a>Versleutelingsinstellingen voor weergave in de Azure-portal

Als u wilt weergeven van instellingen voor versleuteling voor opslagservice, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com) en selecteer een opslagaccount. In de **instellingen** venster de **versleuteling** instelling.

![Portal schermopname die laat zien hoe de versleutelingsinstelling](./media/storage-service-encryption/image1.png)

## <a name="faq-for-storage-service-encryption"></a>Veelgestelde vragen over de Storage-Serviceversleuteling

**V: ik heb een klassiek opslagaccount. Kan ik versleuteling voor opslagservice erop inschakelen?**

A: Storage-Serviceversleuteling is ingeschakeld voor alle opslagaccounts (klassiek en Resource Manager).

**V: hoe kan ik gegevens versleutelen in mijn klassieke storage-account?**

A: met versleuteling standaard ingeschakeld, worden uw nieuwe gegevens automatisch versleuteld door Azure Storage. 

**V: ik heb een Resource Manager-opslagaccount. Kan ik versleuteling voor opslagservice erop inschakelen?**

A: storage Service Encryption is standaard ingeschakeld op alle bestaande Resource Manager-opslagaccounts. Dit wordt ondersteund voor Blob storage, Table storage, Queue storage en Azure Files. 

**V: hoe ik de gegevens in een Resource Manager-opslagaccount versleutelen?**

A: Storage-Serviceversleuteling is ingeschakeld voor alle opslagaccounts--klassieke en Resource Manager worden alle bestaande bestanden in de storage-account hebt gemaakt voordat versleuteling werd ingeschakeld wordt met terugwerkende kracht versleuteld met behulp van een achtergrondproces voor versleuteling.

**V: kan ik storage-accounts maken met de Storage-Serviceversleuteling is ingeschakeld met behulp van Azure PowerShell en Azure CLI**

A: Storage Service Encryption is standaard ingeschakeld op het moment van het maken van een opslagaccount (klassiek of Resource Manager). U kunt de eigenschappen van account controleren met behulp van zowel Azure PowerShell en Azure CLI.

**V: hoe veel meer kost Azure Storage als de Storage-Serviceversleuteling is ingeschakeld?**

Antwoord: Er is geen extra kosten.

**V: kan ik mijn eigen versleutelingssleutels gebruiken?**

A: Ja, kunt u uw eigen versleutelingssleutels. Zie voor meer informatie, [Storage Service Encryption door de klant beheerde sleutels in Azure Key Vault](storage-service-encryption-customer-managed-keys.md).

**V: kan ik toegang tot de versleutelingssleutels intrekken?**

A: Ja, als u [gebruik uw eigen versleutelingssleutels](storage-service-encryption-customer-managed-keys.md) in Azure Key Vault.

**Vraag: is Storage Service Encryption is standaard ingeschakeld wanneer ik een opslagaccount maken?**

A: Ja, Storage-Serviceversleuteling is ingeschakeld voor alle opslagaccounts en voor alle Azure Storage-services.

**V: Wat is dit verschil met Azure Disk Encryption?**

A: azure Disk Encryption wordt gebruikt voor het versleutelen van besturingssysteem en gegevensschijven in IaaS-VM's. Zie voor meer informatie de [Storage-beveiligingshandleiding](../storage-security-guide.md).

**V: Wat gebeurt er als ik Azure Disk Encryption inschakelen op de gegevensschijven van mijn?**

A: deze werkt naadloos. Beide methoden wordt uw gegevens te versleutelen.

**V: Mijn storage-account is ingesteld om geo-redundante worden gerepliceerd. Met Storage Service Encryption wordt mijn redundante kopie ook worden versleuteld?**

A: Ja, alle exemplaren van het opslagaccount zijn versleuteld. Alle redundantie opties worden ondersteund: lokaal redundante opslag, zone-redundante opslag, geografisch redundante opslag en geografisch redundante opslag met leestoegang.

**V: kan ik versleuteling uitschakelen op mijn storage-account?**

A: versleuteling is standaard ingeschakeld en er is geen inrichten om versleuteling voor uw opslagaccount te schakelen. 

**Vraag: is Storage Service Encryption alleen toegestaan in specifieke regio's?**

A: storage Service Encryption is beschikbaar in alle regio's voor alle services.

**V: is Storage Service Encryption FIPS 140-2 compatibel?**

A: Ja, is Storage Service Encryption FIPS 140-2-compatibel.

**V: hoe moet ik contact opnemen iemand als ik problemen hebt of feedback wilt geven?**

A: Neem contact op met [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) voor eventuele problemen of feedback die betrekking hebben op Storage-Serviceversleuteling.

## <a name="next-steps"></a>Volgende stappen
Azure Storage biedt dat een uitgebreide set mogelijkheden voor beveiliging die ontwikkelaars samen help veilige toepassingen bouwen. Zie voor meer informatie de [Storage-beveiligingshandleiding](../storage-security-guide.md).
