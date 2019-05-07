---
title: Configureren van de klant beheerde sleutels voor Azure Storage-versleuteling van de Azure-portal
description: Leer hoe u de Azure portal gebruiken voor het configureren van de klant beheerde sleutels voor Azure Storage-versleuteling. Door de klant beheerde sleutels kunnen u maken, draaien, uitschakelen en besturingselementen voor toegang intrekken.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/16/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: baabc5a8e1d063cb51a3edea3a7218591e85aa1a
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65154157"
---
# <a name="configure-customer-managed-keys-for-azure-storage-encryption-from-the-azure-portal"></a>Configureren van de klant beheerde sleutels voor Azure Storage-versleuteling van de Azure-portal

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

In dit artikel laat zien hoe het configureren van een key vault met door de klant beheerde sleutels die gebruikmaken van de [Azure-portal](https://portal.azure.com/). Zie voor meer informatie over het maken van een sleutelkluis met behulp van de Azure-portal, [Quick Start: Instellen en ophalen van een geheim uit Azure Key Vault met behulp van de Azure-portal](../../key-vault/quick-create-portal.md). 


> [!IMPORTANT]
> Met behulp van de klant beheerde sleutels met Azure Storage-versleuteling vereist dat de key vault twee vereiste eigenschappen die zijn geconfigureerd, **voorlopig verwijderen** en **doen niet leegmaken**. Deze eigenschappen zijn standaard ingeschakeld wanneer u een nieuwe sleutelkluis in Azure portal maakt. Echter, als u deze eigenschappen in een bestaande sleutelkluis inschakelen wilt, moet u PowerShell of Azure CLI.

## <a name="enable-customer-managed-keys"></a>Inschakelen van de klant beheerde sleutels

Als u wilt inschakelen door de klant beheerde sleutels in Azure portal, de volgende stappen uit:

1. Ga naar uw opslagaccount.
1. Op de **instellingen** blade voor de storage-account, klikt u op **versleuteling**. Selecteer de **uw eigen sleutel gebruiken** optie, zoals wordt weergegeven in de volgende afbeelding.

    ![Schermafbeelding van portal weergeven versleutelingsoptie](./media/storage-encryption-keys-portal/ssecmk1.png)

## <a name="specify-a-key"></a>Een sleutel opgeven

Nadat u de klant beheerde sleutels hebt ingeschakeld, hebt u de mogelijkheid om op te geven van een sleutel wilt koppelen aan de storage-account.

### <a name="specify-a-key-as-a-uri"></a>Een sleutel opgeeft als een URI

Als u een sleutel als een URI, de volgende stappen uit:

1. Als u wilt de sleutel-URI vinden in Azure portal, gaat u naar uw key vault en selecteer de **sleutels** instelling. Selecteer de gewenste sleutel en klik vervolgens op de toets om de instellingen ervan weer te geven. Kopieer de waarde van de **sleutel-id** veld, waarmee u de URI.

    ![Schermopname die laat zien sleutelkluis sleutel-URI](media/storage-encryption-keys-portal/key-uri-portal.png)

1. In de **versleuteling** -instellingen voor uw storage-account, kies de **Enter-toets URI** optie.
1. In de **Key URI** veld, geeft u de URI.

   ![Schermopname die laat zien hoe u sleutel-URI invoeren](./media/storage-encryption-keys-portal/ssecmk2.png)

### <a name="specify-a-key-from-a-key-vault"></a>Geef een sleutel van een key vault

Zorg ervoor dat u een sleutelkluis met een sleutel om op te geven van een sleutel van een key vault. Als u een sleutel van een key vault, de volgende stappen uit:

1. Kies de **selecteren uit Key Vault** optie.
2. Kies de key vault met de sleutel die u wilt gebruiken.
3. Kies de sleutel uit de key vault.

   ![Schermafbeelding van de klant beheerde sleutel optie](./media/storage-encryption-keys-portal/ssecmk3.png)

## <a name="update-the-key-version"></a>De belangrijkste versie bijwerken

Wanneer u een nieuwe versie van een sleutel maakt, moet u het opslagaccount voor het gebruik van de nieuwe versie bijwerken. Volg deze stappen:

1. Ga naar uw opslagaccount en weer te geven de **versleuteling** instellingen.
1. Geef de URI voor de nieuwe sleutelversie. U kunt ook de key vault en de sleutel opnieuw om bij te werken van de versie selecteren.

## <a name="next-steps"></a>Volgende stappen

- [Azure Storage-versleuteling voor data-at-rest](storage-service-encryption.md)
- [Wat is Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)?
