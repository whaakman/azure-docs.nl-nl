---
title: Azure CLI of PowerShell-opdrachten in een Azure AD-identiteit voor toegang tot blob- en wachtrijservices gegevens uitvoeren | Microsoft Docs
description: Azure CLI en PowerShell ondersteunt aangemeld met een Azure AD-identiteit opdrachten uitvoeren op Azure Storage-blob en wachtrijen met gegevens. Een toegangstoken is opgegeven voor de sessie en gebruikt voor de aanroepende operations autorisatie. Machtigingen zijn afhankelijk van de RBAC-rol die is toegewezen aan de Azure AD-identiteit.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/26/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: a0972beff48e07b6ce8afdcec10581300f59ed41
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58803575"
---
# <a name="use-an-azure-ad-identity-to-access-blob-and-queue-data-with-cli-or-powershell"></a>Een Azure AD-identiteit voor toegang tot blob- en wachtrijservices gegevens met CLI of PowerShell gebruiken

Azure Storage biedt extensies voor Azure CLI en PowerShell waarmee u zich aanmelden en het uitvoeren van opdrachten in het script onder de identiteit van een Azure Active Directory (Azure AD). De identiteit van de Azure AD kan een gebruiker, groep of toepassing service-principal, of kan een [beheerde identiteit voor de Azure-resources](../../active-directory/managed-identities-azure-resources/overview.md). U kunt machtigingen voor toegang tot blob- en wachtrijservices gegevens naar de Azure AD-identiteit via op rollen gebaseerd toegangsbeheer (RBAC) toewijzen. Zie voor meer informatie over RBAC-rollen in Azure Storage, [beheren rechten voor het Azure Storage-gegevens met RBAC](storage-auth-aad-rbac.md).

Wanneer u zich aanmeldt bij Azure CLI of PowerShell met een Azure AD-identiteit, wordt een toegangstoken voor toegang tot Azure Storage onder die identiteit geretourneerd. Dit token wordt vervolgens automatisch gebruikt door de CLI of PowerShell om bewerkingen op Azure Storage. Voor ondersteunde bewerkingen hoeft u niet meer om door te geven van een sleutel of een SAS-token met de opdracht.

## <a name="supported-operations"></a>Ondersteunde bewerkingen

De extensies worden ondersteund voor bewerkingen voor containers en wachtrijen. Welke bewerkingen u kunt aanroepen, is afhankelijk van de machtigingen verleend aan de Azure AD-identiteit in waarmee u zich aanmelden bij Azure CLI of PowerShell. Machtigingen voor het Azure Storage-containers of wachtrijen worden toegewezen via op rollen gebaseerd toegangsbeheer (RBAC). Bijvoorbeeld, als een gegevenslezer-rol wordt toegewezen aan de identiteit, kunt klikt u vervolgens u uitvoeren scripting opdrachten die gegevens uit een container of de wachtrij lezen. Als een inzender Data-rol wordt toegewezen aan de identiteit, kunt u opdrachten in het script dat lezen, schrijven of verwijderen van een container of wachtrij of de gegevens die ze bevatten uitvoeren. 

Zie voor meer informatie over de machtigingen die vereist zijn voor elke Azure Storage-bewerking in een container of een wachtrij [machtigingen voor het aanroepen van REST-bewerkingen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).  

## <a name="call-cli-commands-using-azure-ad-credentials"></a>Aanroep van CLI-opdrachten met behulp van Azure AD-referenties

Azure CLI ondersteunt de `--auth-mode` parameter voor de blob- en wachtrijservices gegevensbewerkingen:

- Stel de `--auth-mode` parameter `login` zich aanmelden met een beveiligings-principal voor Azure AD.
- Stel de `--auth-mode` parameter naar de oude `key` waarde om te zoeken naar een account-sleutel als geen verificatieparameters voor het account zijn opgegeven. 

Het volgende voorbeeld ziet hoe u een container maken in een nieuw opslagaccount vanuit Azure CLI met behulp van uw Azure AD-referenties. Houd er rekening mee tijdelijke aanduiding voor waarden in de punthaken vervangen door uw eigen waarden: 

1. Zorg ervoor dat u Azure CLI versie 2.0.46 hebt geïnstalleerd of hoger. Voer `az --version` om te controleren of de geïnstalleerde versie.

1. Voer `az login` en in het browservenster te verifiëren: 

    ```azurecli
    az login
    ```
    
1. Geef het gewenste abonnement. Maak een resourcegroep met de opdracht [az group create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create). Maak een opslagaccount in deze resource-groep met [az storage-account maken](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create): 

    ```azurecli
    az account set --subscription <subscription-id>

    az group create \
        --name sample-resource-group-cli \
        --location eastus

    az storage account create \
        --name <storage-account> \
        --resource-group sample-resource-group-cli \
        --location eastus \
        --sku Standard_LRS \
        --encryption-services blob
    ```
    
1. Voordat u de container maken, toewijzen de [Gegevensbijdrager voor Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) rol aan uzelf. Hoewel u eigenaar van het account zijn, moet u expliciete machtigingen voor het uitvoeren van bewerkingen voor het opslagaccount. Zie voor meer informatie over het toewijzen van RBAC-rollen [toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC in Azure portal](storage-auth-aad-rbac.md).

    > [!IMPORTANT]
    > RBAC-roltoewijzingen duurt een paar minuten worden doorgegeven.
    
1. Roep de [az storage container maken](https://docs.microsoft.com/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-create) opdracht met de `--auth-mode` parameter ingesteld op `login` om de container met behulp van uw Azure AD-referenties te maken:

    ```azurecli
    az storage container create \ 
        --account-name <storage-account> \ 
        --name sample-container \
        --auth-mode login
    ```

De omgevingsvariabele die is gekoppeld aan de `--auth-mode` parameter `AZURE_STORAGE_AUTH_MODE`. U kunt de juiste waarde opgeven in de omgevingsvariabele om te voorkomen dat met inbegrip van het bij elke aanroep naar een Azure Storage-gegevens-bewerking.

## <a name="call-powershell-commands-using-azure-ad-credentials"></a>Aanroepen van de PowerShell-opdrachten met behulp van Azure AD-referenties

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Een opslagcontext om te verwijzen naar het storage-account maken voor het gebruik van Azure PowerShell om te melden en de volgende bewerkingen uitvoeren in Azure Storage met behulp van Azure AD-referenties, en met inbegrip van de `-UseConnectedAccount` parameter.

Het volgende voorbeeld ziet hoe u een container maken in een nieuw opslagaccount vanuit Azure PowerShell met behulp van uw Azure AD-referenties. Houd er rekening mee tijdelijke aanduiding voor waarden in de punthaken vervangen door uw eigen waarden:

1. Aanmelden bij uw Azure-abonnement met de `Connect-AzAccount` opdracht en volgt u de op het scherm richtingen uw Azure AD-referenties in te voeren: 

    ```powershell
    Connect-AzAccount
    ```
    
1. Een Azure-resourcegroep maken door het aanroepen van [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). 

    ```powershell
    $resourceGroup = "sample-resource-group-ps"
    $location = "eastus"
    New-AzResourceGroup -Name $resourceGroup -Location $location
    ```

1. Een opslagaccount maken door het aanroepen van [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount).

    ```powershell
    $storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
      -Name "<storage-account>" `
      -SkuName Standard_LRS `
      -Location $location `
    ```

1. Ophalen van de opslagaccountcontext op waarin het nieuwe opslagaccount door het aanroepen van [New-AzStorageContext](/powershell/module/az.storage/new-azstoragecontext). Als u werkt in een storage-account, kunt u verwijzen naar de context in plaats van herhaaldelijk de referenties te geven. Bevatten de `-UseConnectedAccount` parameter om aan te roepen volgende gegevensbewerkingen met behulp van uw Azure AD-referenties:

    ```powershell
    $ctx = New-AzStorageContext -StorageAccountName "<storage-account>" -UseConnectedAccount
    ```

1. Voordat u de container maken, toewijzen de [Gegevensbijdrager voor Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) rol aan uzelf. Hoewel u eigenaar van het account zijn, moet u expliciete machtigingen voor het uitvoeren van bewerkingen voor het opslagaccount. Zie voor meer informatie over het toewijzen van RBAC-rollen [toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC in Azure portal](storage-auth-aad-rbac.md).

    > [!IMPORTANT]
    > RBAC-roltoewijzingen duurt een paar minuten worden doorgegeven.

1. Een container maken door het aanroepen van [New-AzStorageContainer](/powershell/module/az.storage/new-azstoragecontainer). Omdat deze aanroep maakt gebruik van de context in de vorige stappen hebt gemaakt, wordt de container gemaakt met behulp van uw Azure AD-referenties. 

    ```powershell
    $containerName = "sample-container"
    New-AzStorageContainer -Name $containerName -Context $ctx
    ```

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over RBAC-rollen voor Azure-opslag, [beheren-toegangsrechten aan opslag van gegevens met RBAC](storage-auth-aad-rbac.md).
- Zie voor meer informatie over het gebruik van beheerde identiteiten voor Azure-resources met Azure Storage, [verifiëren toegang tot blobs en wachtrijen met Azure beheerde identiteiten voor Azure-Resources](storage-auth-aad-msi.md).
- Zie voor informatie over het toestaan van toegang tot containers en wachtrijen van binnen uw storage-toepassingen, [gebruik Azure AD met opslagtoepassingen](storage-auth-aad-app.md).
