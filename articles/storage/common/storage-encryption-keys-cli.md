---
title: Configureren van de klant beheerde sleutels voor Azure Storage-versleuteling van Azure CLI
description: Leer hoe u Azure CLI gebruiken voor het configureren van de klant beheerde sleutels voor Azure Storage-versleuteling. Door de klant beheerde sleutels kunnen u maken, draaien, uitschakelen en besturingselementen voor toegang intrekken.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/24/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 925b69e064e260a78a102a068f052ad7d396c380
ms.sourcegitcommit: a7ea412ca4411fc28431cbe7d2cc399900267585
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2019
ms.locfileid: "67357062"
---
# <a name="configure-customer-managed-keys-for-azure-storage-encryption-from-azure-cli"></a>Configureren van de klant beheerde sleutels voor Azure Storage-versleuteling van Azure CLI

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

In dit artikel laat zien hoe het configureren van een key vault met de klant beheerde sleutels met behulp van Azure CLI.

## <a name="assign-an-identity-to-the-storage-account"></a>Een identiteit toewijzen aan de storage-account

Als u wilt inschakelen door de klant beheerde sleutels voor uw storage-account, moet u eerst een beheerde identiteit voor het systeem toegewezen toewijzen aan de storage-account. U gebruikt deze beheerde identiteit de storage-account machtigingen voor toegang tot de sleutelkluis.

Als u wilt toewijzen van een beheerde identiteit met Azure CLI, aanroepen [az storage account update](/cli/azure/storage/account#az-storage-account-update). Vergeet niet om de tijdelijke aanduiding voor waarden tussen vierkante haken vervangen door uw eigen waarden.

```azurecli-interactive
az account set --subscription <subscription-id>

az storage account update \
    --name <storage-account> \
    --resource-group <resource_group> \
    --assign-identity
```

Zie voor meer informatie over het configureren van het systeem toegewezen beheerde identiteiten met Azure CLI [configureren beheerde identiteiten voor een Azure-resources op een Azure-VM met behulp van Azure CLI](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md).

## <a name="create-a-new-key-vault"></a>Een nieuwe sleutelkluis maken

De sleutelkluis die u gebruikt voor het opslaan van de klant beheerde sleutels voor Azure Storage-versleuteling moet twee sleutelbeveiliging instellingen ingeschakeld, hebben **voorlopig verwijderen** en **doen niet leegmaken**. Voor het maken van een nieuwe sleutelkluis met PowerShell of Azure CLI met deze instellingen zijn ingeschakeld, moet u de volgende opdrachten uitvoeren. Vergeet niet om de tijdelijke aanduiding voor waarden tussen vierkante haken vervangen door uw eigen waarden. 

Aanroepen voor het maken van een nieuwe sleutelkluis met behulp van Azure CLI, [az keyvault maken](/cli/azure/keyvault#az-keyvault-create). Vergeet niet om de tijdelijke aanduiding voor waarden tussen vierkante haken vervangen door uw eigen waarden.

```azurecli-interactive
az keyvault create \
    --name <key-vault> \
    --resource-group <resource_group> \
    --location <region> \
    --enable-soft-delete \
    --enable-purge-protection
```

## <a name="configure-the-key-vault-access-policy"></a>Het toegangsbeleid voor key vault configureren

Vervolgens configureert u het toegangsbeleid voor key vault zodat het storage-account heeft machtigingen om deze te openen. In deze stap maakt u de beheerde identiteit die u eerder hebt toegewezen aan de storage-account gebruikt.

Aanroepen om het toegangsbeleid voor key vault, [az keyvault-beleid instellen](/cli/azure/keyvault#az-keyvault-set-policy). Vergeet niet om de tijdelijke aanduiding voor waarden tussen vierkante haken vervangen door uw eigen waarden.

```azurecli-interactive
storage_account_principal=$(az storage account show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --query identity.principalId \
    --output tsv)
az keyvault set-policy \
    --name <key-vault> \
    --resource-group <resource_group>
    --object-id $storage_account_principal \
    --key-permissions get recover unwrapKey wrapKey
```

## <a name="create-a-new-key"></a>Een nieuwe sleutel maken

Maak vervolgens een sleutel in de key vault. Aanroepen voor het maken van een sleutel, [az keyvault key maken](/cli/azure/keyvault/key#az-keyvault-key-create). Vergeet niet om de tijdelijke aanduiding voor waarden tussen vierkante haken vervangen door uw eigen waarden.

```azurecli-interactive
az keyvault key create
    --name <key> \
    --vault-name <key-vault>
```

## <a name="configure-encryption-with-customer-managed-keys"></a>Versleuteling configureren met de klant beheerde sleutels

Azure Storage-versleuteling gebruikt standaard Microsoft beheerde sleutels. Configureer uw Azure Storage-account voor de klant beheerde sleutels en geeft u de code om te koppelen aan de storage-account.

Voor het bijwerken van instellingen voor codering van de storage-account, bellen [az storage account update](/cli/azure/storage/account#az-storage-account-update). In dit voorbeeld is ook query's voor de key vault URI en de sleutelversie, van welke waarden zijn nodig voor het koppelen van de sleutel met de storage-account. Vergeet niet om de tijdelijke aanduiding voor waarden tussen vierkante haken vervangen door uw eigen waarden.

```azurecli-interactive
key_vault_uri=$(az keyvault show \
    --name <key-vault> \
    --resource-group <resource_group> \
    --query properties.vaultUri \
    --output tsv)
key_version=$(az keyvault key list-versions \
    --name <key> \
    --vault-name <key-vault> \
    --query [].kid \
    --output tsv | cut -d '/' -f 6)
az storage account update 
    --name <storage-account> \
    --resource-group <resource_group> \
    --encryption-key-name <key> \
    --encryption-key-version $key_version \
    --encryption-key-source Microsoft.Keyvault \
    --encryption-key-vault $key_vault_uri
```

## <a name="update-the-key-version"></a>De belangrijkste versie bijwerken

Wanneer u een nieuwe versie van een sleutel maakt, moet u het opslagaccount voor het gebruik van de nieuwe versie bijwerken. Eerst op te vragen voor de key vault URI door het aanroepen van [az keyvault show](/cli/azure/keyvault#az-keyvault-show), en voor de belangrijkste versie door het aanroepen van [az keyvault key list-versies](/cli/azure/keyvault/key#az-keyvault-key-list-versions). Roep vervolgens [az storage account update](/cli/azure/storage/account#az-storage-account-update) om bij te werken de versleutelingsinstellingen van de storage-account voor het gebruik van de nieuwe versie van de sleutel, zoals wordt weergegeven in de vorige sectie.

## <a name="next-steps"></a>Volgende stappen

- [Azure Storage-versleuteling voor data-at-rest](storage-service-encryption.md) 
- [Wat is Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)?
