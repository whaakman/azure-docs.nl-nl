---
title: Voer Azure CLI of PowerShell-opdrachten onder een Azure AD-identiteit voor toegang tot Azure Storage (Preview) | Microsoft Docs
description: Azure CLI en PowerShell ondersteunt aangemeld met een Azure AD-identiteit opdrachten uitvoeren op Azure Storage-containers en -wachtrijen en hun gegevens. Een toegangstoken is opgegeven voor de sessie en gebruikt voor de aanroepende operations autorisatie. Machtigingen zijn afhankelijk van de rol die is toegewezen aan de Azure AD-identiteit.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/20/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: 984185febf770ae10a021d129b0ef6c43da4d0f1
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47032764"
---
# <a name="use-an-azure-ad-identity-to-access-azure-storage-with-cli-or-powershell-preview"></a>Een Azure AD-identiteit gebruiken voor toegang tot Azure Storage met CLI of PowerShell (Preview)

Azure Storage biedt preview-extensies voor Azure CLI en PowerShell waarmee u zich aanmeldt en uitvoeren van opdrachten in het script onder de identiteit van een Azure Active Directory (Azure AD). De identiteit van de Azure AD kan een gebruiker, groep of toepassing service-principal, of kan een [beheerde service-identiteit](../../active-directory/managed-identities-azure-resources/overview.md). U kunt machtigingen voor toegang tot opslagresources naar de Azure AD-identiteit via op rollen gebaseerd toegangsbeheer (RBAC) toewijzen. Zie voor meer informatie over RBAC-rollen in Azure Storage, [beheren rechten voor het Azure Storage-gegevens met RBAC (Preview)](storage-auth-aad-rbac.md).

Wanneer u zich aanmeldt bij Azure CLI of PowerShell met een Azure AD-identiteit, wordt een toegangstoken geretourneerd voor toegang tot Azure Storage onder die identiteit. Dit token wordt vervolgens automatisch gebruikt door de CLI of PowerShell om bewerkingen op Azure Storage. Voor ondersteunde bewerkingen hoeft u niet meer om door te geven van een sleutel of een SAS-token met de opdracht.

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="supported-operations"></a>Ondersteunde bewerkingen

De preview-extensies worden ondersteund voor bewerkingen voor containers en wachtrijen. Welke bewerkingen u kunt aanroepen, is afhankelijk van de machtigingen verleend aan de Azure AD-identiteit waarmee u zich aanmeldt bij Azure CLI of PowerShell. Machtigingen voor het Azure Storage-containers of wachtrijen worden toegewezen via op rollen gebaseerd toegangsbeheer (RBAC). Bijvoorbeeld, als een gegevenslezer-rol wordt toegewezen aan de identiteit, kunt klikt u vervolgens u uitvoeren scripting opdrachten die gegevens uit een container of de wachtrij lezen. Als een gegevens-Registratiemachtiging-rol wordt toegewezen aan de identiteit, kunt u opdrachten in het script dat lezen, schrijven of verwijderen van een container of wachtrij of de gegevens die ze bevatten uitvoeren. 

Zie voor meer informatie over de machtigingen die vereist zijn voor elke Azure Storage-bewerking in een container of een wachtrij [machtigingen voor het aanroepen van REST-bewerkingen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).  

## <a name="call-cli-commands-with-an-azure-ad-identity"></a>Aanroep van CLI-opdrachten met een Azure AD-identiteit

De preview-extensie voor Azure CLI installeren:

1. Zorg ervoor dat u Azure CLI versie 2.0.32 hebt geïnstalleerd of hoger. Voer `az --version` om te controleren of de geïnstalleerde versie.
2. Voer de volgende opdracht voor het installeren van de preview-extensie: 

    ```azurecli
    az extension add -n storage-preview
    ```

De preview-extensie voegt een nieuwe `--auth-mode` parameter ondersteunde opdrachten:

- Stel de `--auth-mode` parameter `login` zich aanmelden met een Azure AD-identiteit.
- Stel de `--auth-mode` parameter naar de oude `key` waarde om te zoeken naar een account-sleutel als geen verificatieparameters voor het account zijn opgegeven. 

Bijvoorbeeld, voor het downloaden van een blob in Azure CLI met behulp van een Azure AD-identiteit voor het eerst uitvoert `az login`, roept u vervolgens de opdracht met `--auth-mode` ingesteld op `login`:

```azurecli
az login
az storage blob download --account-name storagesamples --container sample-container --name myblob.txt --file myfile.txt --auth-mode login 
```

De omgevingsvariabele die is gekoppeld aan de `--auth-mode` parameter `AZURE_STORAGE_AUTH_MODE`.

## <a name="call-powershell-commands-with-an-azure-ad-identity"></a>Aanroepen van de PowerShell-opdrachten met een Azure AD-identiteit

Azure PowerShell ondersteunt aanmelden met een Azure AD-identiteit met een van de volgende preview-modules alleen: 

- 4.4.0-Preview 
- 4.4.1-Preview 

Azure PowerShell gebruiken voor het aanmelden met een Azure AD-identiteit:

1. Verwijder eerdere installaties van Azure PowerShell:

    - Verwijderen van eerdere installaties van Azure PowerShell uit met behulp van Windows de **Apps en functies** bij **instellingen**.
    - Verwijder alle **Azure*** modules van `%Program Files%\WindowsPowerShell\Modules`.

1. Zorg ervoor dat u de nieuwste versie van PowerShellGet geïnstalleerd hebt. Open een Windows PowerShell-venster en voer de volgende opdracht voor het installeren van de meest recente versie:
 
    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```
1. Sluit en Open de PowerShell-venster na de installatie van PowerShellGet. 

1. AzureRM installeren:

    ```powershell
    Install-Module AzureRM –Repository PSGallery –AllowClobber
    ```

1. Een van de preview-modules te installeren:

    ```powershell
    Install-Module Azure.Storage –Repository PSGallery -RequiredVersion 4.4.1-preview  –AllowPrerelease –AllowClobber –Force 
    ```
1. Sluit en Open de PowerShell-venster.
1. Roep de [New-AzureStorageContext](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontext) cmdlet voor het maken van een context en zijn de `-UseConnectedAccount` parameter. 
1. Voor het aanroepen van een cmdlet uit met een Azure AD-identiteit, moet u de zojuist gemaakte context doorgeven aan de cmdlet.

Het volgende voorbeeld ziet hoe u de blobs in een container van Azure PowerShell met behulp van een Azure AD-identiteit. Zorg ervoor dat de namen van de tijdelijke aanduiding voor account- en container vervangen door uw eigen waarden: 

```powershell
$ctx = New-AzureStorageContext -StorageAccountName storagesamples -UseConnectedAccount 
Get-AzureStorageBlob -Container sample-container -Context $ctx 
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over RBAC-rollen voor Azure-opslag, [beheren-toegangsrechten aan opslag van gegevens met RBAC (Preview)](storage-auth-aad-rbac.md).
- Zie voor meer informatie over het gebruik van beheerde identiteiten voor Azure-resources met Azure Storage, [verifiëren toegang tot blobs en wachtrijen met Azure beheerde identiteiten voor Azure-Resources (Preview)](storage-auth-aad-msi.md).
- Zie voor informatie over het toestaan van toegang tot containers en wachtrijen van binnen uw storage-toepassingen, [gebruik Azure AD met opslagtoepassingen](storage-auth-aad-app.md).
- Zie voor meer informatie over Azure AD-integratie voor Azure-Blobs en wachtrijen, de blog van het Azure Storage-team plaatst, [aankondiging van de Preview-versie van Azure AD-verificatie voor Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
