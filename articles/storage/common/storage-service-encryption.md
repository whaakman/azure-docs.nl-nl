---
title: Azure Storage-versleuteling voor data-at-rest | Microsoft Docs
description: Azure Storage beschermt uw gegevens automatisch door deze te coderen voordat opgeslagen in de cloud. Alle gegevens in Azure Storage, in blobs, schijven, bestanden, wachtrijen of tabellen, worden versleuteld en ontsleuteld transparant met 256-bits AES-versleuteling en FIPS 140-2 compatibel is.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/16/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 211cfeb3aba29245e154f4a7db86fb4a3659c36f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60730822"
---
# <a name="azure-storage-encryption-for-data-at-rest"></a>Azure Storage-versleuteling voor data-at-rest

Azure-opslag worden uw gegevens automatisch versleuteld wanneer deze persistent te maken naar de cloud. Versleuteling beschermt uw gegevens en om te helpen u om te voldoen aan de beveiligings- en nalevingsverplichtingen van de organisatie. Gegevens in Azure Storage worden versleuteld en ontsleuteld transparant met 256-bits [AES-versleuteling](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), een van de krachtigste blokversleutelingsmethoden die er bestaan en compatibel is met FIPS 140-2. Azure Storage-versleuteling is vergelijkbaar met BitLocker-versleuteling op Windows.

Azure Storage-versleuteling is ingeschakeld voor alle nieuwe en bestaande opslagaccounts en kan niet worden uitgeschakeld. Omdat uw gegevens beveiligd standaard, hoeft u niet te wijzigen van uw code of toepassingen om te profiteren van Azure Storage-versleuteling. Storage-accounts zijn versleuteld, ongeacht hun prestatielaag (standard of premium) of het implementatiemodel (Azure Resource Manager of klassiek). Redundantieopties voor alle Azure Storage-versleuteling ondersteunen, en alle exemplaren van een storage-account zijn versleuteld. Alle Azure Storage-resources zijn versleuteld, met inbegrip van blobs, schijven, bestanden, wachtrijen en tabellen.

Versleuteling heeft geen invloed op prestaties van Azure Storage. Er is geen extra kosten voor Azure Storage-versleuteling.

Zie voor meer informatie over de cryptografische modules van de onderliggende Azure Storage-versleuteling, [Cryptography API: De volgende generatie](https://docs.microsoft.com/windows/desktop/seccng/cng-portal).

## <a name="key-management"></a>Sleutelbeheer

U kunt vertrouwen op Microsoft beheerde sleutels voor de versleuteling van uw storage-account of u kunt versleuteling beheren met uw eigen sleutels, samen met Azure Key Vault.

### <a name="microsoft-managed-keys"></a>Microsoft beheerde sleutels

Uw storage-account gebruikt standaard, beheerd door Microsoft versleutelingssleutels. U ziet de versleutelingsinstellingen voor uw opslagaccount in de **versleuteling** sectie van de [Azure-portal](https://portal.azure.com), zoals wordt weergegeven in de volgende afbeelding.

![Account is versleuteld met door Microsoft beheerde sleutels weergeven](media/storage-service-encryption/encryption-microsoft-managed-keys.png)

### <a name="customer-managed-keys"></a>Door de klant beheerde sleutels

U kunt Azure Storage-versleuteling beheren met de klant beheerde sleutels. Door de klant beheerde sleutels biedt u meer flexibiliteit om te maken, draaien, uitschakelen en besturingselementen voor toegang intrekken. U kunt ook de versleutelingssleutels gebruikt voor het beveiligen van uw gegevens controleren. 

Azure Key Vault gebruiken voor uw sleutels beheren en controleren van uw gebruik van de sleutel. U kunt uw eigen sleutels maken en deze opslaan in een key vault, of u kunt de Azure Key Vault-API's gebruiken om sleutels te genereren. Het opslagaccount en de key vault moeten zich in dezelfde regio, maar ze kunnen zich in verschillende abonnementen. Zie voor meer informatie over Azure Key Vault [wat is Azure Key Vault?](../../key-vault/key-vault-overview.md).

Als u wilt intrekken van toegang tot de klant beheerde sleutels, Zie [Azure Key Vault PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/) en [Azure Key Vault CLI](https://docs.microsoft.com/cli/azure/keyvault). Effectieve toegang intrekken blokkeert de toegang tot alle gegevens in de storage-account, omdat de versleutelingssleutel niet toegankelijk door Azure Storage.

Zie voor meer informatie over het gebruik van de klant beheerde sleutels met Azure Storage, een van de volgende artikelen:

- [Configureren van de klant beheerde sleutels voor Azure Storage-versleuteling van de Azure-portal](storage-encryption-keys-portal.md)
- [Configureren van de klant beheerde sleutels voor Azure Storage-versleuteling van PowerShell](storage-encryption-keys-powershell.md)
- [Door de klant beheerde sleutels gebruiken met Azure Storage-versleuteling van Azure CLI](storage-encryption-keys-cli.md)

> [!NOTE]  
> Door de klant beheerde sleutels worden niet ondersteund voor [Azure beheerde schijven](../../virtual-machines/windows/managed-disks-overview.md).

## <a name="azure-storage-encryption-versus-disk-encryption"></a>Azure Storage-versleuteling versus schijfversleuteling

Met Azure Storage-versleuteling, zijn alle Azure Storage-accounts en de resources die ze bevatten versleuteld, met inbegrip van de pagina-blobs die back-virtuele-machineschijven van Azure. Virtuele-machineschijven van Azure kunnen ook worden versleuteld met [Azure Disk Encryption](../../security/azure-security-disk-encryption-overview.md). Azure Disk Encryption gebruikt industriestandaard [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) op Windows en [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) op Linux te bieden van versleuteling op basis van een besturingssysteem-oplossingen die kunnen worden geïntegreerd met Azure Key Vault.

## <a name="next-steps"></a>Volgende stappen

- [Wat is Azure Key Vault?](../../key-vault/key-vault-overview.md)
