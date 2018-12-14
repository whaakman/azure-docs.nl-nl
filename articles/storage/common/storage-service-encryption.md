---
title: Azure Storage-Serviceversleuteling voor data-at-rest | Microsoft Docs
description: De functie Azure Storage-Serviceversleuteling gebruiken voor het versleutelen van Azure Managed Disks, Azure Blob-opslag, Azure Files, Azure Queue storage en Azure-tabelopslag aan de servicezijde bij het opslaan van de gegevens en bij het ophalen van de gegevens ontsleutelen.
services: storage
author: lakasa
ms.service: storage
ms.topic: article
ms.date: 08/01/2018
ms.author: lakasa
ms.component: common
ms.openlocfilehash: a19cdac54fff641f72d0628c7b0d47060baba9b4
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53384119"
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Azure Storage-Serviceversleuteling voor data-at-rest
Azure Storage-Serviceversleuteling voor data-at-rest kunt u uw gegevens om te voldoen aan uw organisatie beveiligings- en nalevingsverplichtingen beveiligen. Met deze functie door het platform van Azure storage automatisch uw gegevens worden versleuteld voordat Azure Managed Disks, Azure Blob, wachtrij of tabel storage of Azure-bestanden opgeslagen en ontsleutelt de gegevens voor het ophalen van. De verwerking van versleuteling en versleuteling-at-rest, ontsleuteling en sleutelbeheer in Storage Service-versleuteling is transparant voor gebruikers. Alle gegevens die zijn geschreven naar het Azure storage-platform worden versleuteld met 256-bits [AES-versleuteling](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), een van de krachtigste blokversleutelingsmethoden die er bestaan.

Storage-Serviceversleuteling is ingeschakeld voor alle nieuwe en bestaande opslagaccounts en kan niet worden uitgeschakeld. Omdat uw gegevens beveiligd standaard, hoeft u niet te wijzigen van uw code of toepassingen om te profiteren van Storage Service Encryption.

De functie versleutelt automatisch gegevens in:

- Azure storage-services:
    - Azure Managed Disks
    - Azure Blob Storage
    - Azure Files
    - Azure Queue storage
    - Azure-tabelopslag.  
- Beide prestatielagen (standaard en Premium).
- Beide implementatiemodellen (Azure Resource Manager en klassiek).

Storage-Serviceversleuteling heeft geen invloed op de prestaties van Azure storage-services.

U kunt door Microsoft beheerde sleutels gebruiken met Storage Service Encryption of u kunt uw eigen versleutelingssleutels gebruiken. Zie voor meer informatie over het gebruik van uw eigen sleutels [Storage Service Encryption door de klant beheerde sleutels in Azure Key Vault](storage-service-encryption-customer-managed-keys.md).

## <a name="view-encryption-settings-in-the-azure-portal"></a>Versleutelingsinstellingen voor weergave in de Azure-portal
Als u wilt weergeven van instellingen voor versleuteling voor opslagservice, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com) en selecteer een opslagaccount. In de **instellingen** venster de **versleuteling** instelling.

![Portal schermopname die laat zien hoe de versleutelingsinstelling](./media/storage-service-encryption/image1.png)

## <a name="faq-for-storage-service-encryption"></a>Veelgestelde vragen over de Storage-Serviceversleuteling
**Hoe ik de gegevens in een Resource Manager-opslagaccount coderen?**  
Storage-Serviceversleuteling is ingeschakeld voor alle opslagaccounts--klassieke en Resource Manager worden alle bestaande bestanden in de storage-account hebt gemaakt voordat versleuteling werd ingeschakeld wordt met terugwerkende kracht versleuteld met behulp van een achtergrondproces voor versleuteling.

**Storage Service Encryption standaard zijn ingeschakeld wanneer ik een opslagaccount maken?**  
Ja, Storage-Serviceversleuteling is ingeschakeld voor alle opslagaccounts en voor alle Azure storage-services.

**Ik heb een Resource Manager-opslagaccount. Kan ik versleuteling voor opslagservice erop inschakelen?**  
Storage-Serviceversleuteling is standaard ingeschakeld op alle bestaande Resource Manager-opslagaccounts. Dit wordt ondersteund voor Azure Blob storage, Azure-bestanden, Azure Queue storage, Table storage. 

**Kan ik versleuteling uitschakelen op mijn storage-account?**  
Versleuteling is standaard ingeschakeld en er is geen inrichten om versleuteling voor uw opslagaccount te schakelen. 

**Hoe veel meer kost Azure Storage als de Storage-Serviceversleuteling is ingeschakeld?**  
Er is geen extra kosten.

**Kan ik mijn eigen versleutelingssleutels gebruiken?**  
Voor Azure-blobopslag en Azure Files, Ja, kunt u uw eigen versleutelingssleutels. Door de klant beheerde sleutels worden momenteel niet ondersteund door Azure Managed Disks. Zie voor meer informatie, [Storage Service Encryption door de klant beheerde sleutels in Azure Key Vault](storage-service-encryption-customer-managed-keys.md).

**Kan ik toegang tot de versleutelingssleutels intrekken?**  
Ja, als u [gebruik uw eigen versleutelingssleutels](storage-service-encryption-customer-managed-keys.md) in Azure Key Vault.

**Hoe verschilt versleuteling voor opslagservice van Azure Disk Encryption?**  
Azure Disk Encryption biedt integratie tussen OS gebaseerde oplossingen, zoals BitLocker en DM-Crypt en Azure Key Vault. Storage Service-versleuteling biedt versleuteling standaard op de laag van een Azure storage-platform, onder de virtuele machine.

**Ik heb een klassiek opslagaccount. Kan ik versleuteling voor opslagservice erop inschakelen?**  
Storage-Serviceversleuteling is ingeschakeld voor alle opslagaccounts (klassiek en Resource Manager).

**Hoe kan ik gegevens in mijn klassieke opslagaccount coderen?**  
Met versleuteling standaard ingeschakeld, worden alle gegevens die zijn opgeslagen in Azure storage-services automatisch versleuteld. 

**Kan ik storage-accounts maken met de Storage-Serviceversleuteling is ingeschakeld met behulp van Azure PowerShell en Azure CLI**  
Storage-Serviceversleuteling is standaard ingeschakeld op het moment van het maken van een opslagaccount (klassiek of Resource Manager). U kunt de eigenschappen van account controleren met behulp van zowel Azure PowerShell en Azure CLI.

**Mijn storage-account is ingesteld om geo-redundante worden gerepliceerd. Met Storage Service Encryption wordt mijn redundante kopie ook worden versleuteld?**  
Ja, alle exemplaren van het opslagaccount zijn versleuteld. Alle redundantie opties worden ondersteund: lokaal redundante opslag, zone-redundante opslag, geografisch redundante opslag en geografisch redundante opslag met leestoegang.

**Storage-Serviceversleuteling mag alleen in specifieke regio's?**  
Storage-Serviceversleuteling is beschikbaar in alle regio's.

**Is Storage Service Encryption FIPS 140-2 compatibel?**  
Ja, is Storage Service Encryption FIPS 140-2-compatibel. Zie voor meer informatie over de cryptografische modules van Storage Service Encryption onderliggende [Cryptography API: De volgende generatie](https://docs.microsoft.com/windows/desktop/seccng/cng-portal).

**Hoe ik contact opnemen met iemand als ik problemen hebt of feedback wilt geven?**  
Neem contact op met [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) voor eventuele problemen of feedback die betrekking hebben op Storage-Serviceversleuteling.

## <a name="next-steps"></a>Volgende stappen
Azure Storage biedt dat een uitgebreide set mogelijkheden voor beveiliging die ontwikkelaars samen help veilige toepassingen bouwen. Zie voor meer informatie de [Storage-beveiligingshandleiding](../storage-security-guide.md).
