---
title: Azure CLI of PowerShell-opdrachten uitvoeren met Azure AD-referenties voor toegang tot gegevens voor blob of een wachtrij | Microsoft Docs
description: Azure CLI en PowerShell ondersteuning voor aanmelden met Azure AD-referenties opdrachten uitvoeren op Azure Storage-blob en wachtrijen met gegevens. Een toegangstoken is opgegeven voor de sessie en gebruikt voor de aanroepende operations autorisatie. Machtigingen zijn afhankelijk van de RBAC-rol die is toegewezen aan de Azure AD beveiligings-principal.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/19/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 41ca1c5f413e5e15691f336d203edb918f21dc1a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65147298"
---
# <a name="run-azure-cli-or-powershell-commands-with-azure-ad-credentials-to-access-blob-or-queue-data"></a>Azure CLI of PowerShell-opdrachten uitvoeren met Azure AD-referenties voor toegang tot gegevens voor blob of een wachtrij

Azure Storage biedt extensies voor Azure CLI en PowerShell waarmee u zich aanmelden en opdrachten in het script uitvoeren met Azure Active Directory (Azure AD)-referenties. Wanneer u zich aanmeldt bij Azure CLI of PowerShell met Azure AD-referenties, wordt een OAuth 2.0-toegangstoken geretourneerd. Dit token wordt automatisch door de CLI of PowerShell gebruikt om bewerkingen voor het volgende gegevens op basis van storage Blob of een wachtrij. Voor ondersteunde bewerkingen hoeft u niet meer om door te geven van een sleutel of een SAS-token met de opdracht.

U kunt machtigingen toewijzen aan blob- en wachtrijservices gegevens naar een beveiligings-principal voor Azure AD via op rollen gebaseerd toegangsbeheer (RBAC). Zie voor meer informatie over RBAC-rollen in Azure Storage, [beheren rechten voor het Azure Storage-gegevens met RBAC](storage-auth-aad-rbac.md).

## <a name="supported-operations"></a>Ondersteunde bewerkingen

De extensies worden ondersteund voor bewerkingen voor containers en wachtrijen. Welke bewerkingen u kunt aanroepen, is afhankelijk van de machtigingen verleend aan de beveiligings-principal van Azure AD in waarmee u zich aanmelden bij Azure CLI of PowerShell. Machtigingen voor het Azure Storage-containers of wachtrijen worden toegewezen via op rollen gebaseerd toegangsbeheer (RBAC). Bijvoorbeeld, als u zijn toegewezen de **gegevenslezer voor Opslagblob** rol, dan hebt u opdrachten in het script die gegevens uit een container of een wachtrij lezen kunt uitvoeren. Als u zijn toegewezen de **Blob Inzender** rol, dan hebt u opdrachten in het script dat lezen, schrijven of verwijderen van een container of wachtrij of de gegevens die ze bevatten kunt uitvoeren. 

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
- Zie voor meer informatie over het gebruik van beheerde identiteiten voor Azure-resources met Azure Storage, [verifiëren van toegang tot blobs en wachtrijen met Azure Active Directory en beheerde identiteiten voor Azure-Resources](storage-auth-aad-msi.md).
- Zie voor informatie over het toestaan van toegang tot containers en wachtrijen van binnen uw storage-toepassingen, [gebruik Azure AD met opslagtoepassingen](storage-auth-aad-app.md).
