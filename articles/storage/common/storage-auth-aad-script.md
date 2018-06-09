---
title: Voer Azure CLI of PowerShell-opdrachten onder een Azure AD-identiteit voor toegang tot Azure Storage (Preview) | Microsoft Docs
description: Azure CLI en PowerShell ondersteuning aangemeld met een Azure AD identity opdrachten uitvoeren op Azure Storage-containers en -wachtrijen en hun gegevens. Een toegangstoken is opgegeven voor de sessie en gebruikt voor het autoriseren van aanroepen bewerkingen. Machtigingen zijn afhankelijk van de rol die is toegewezen aan de identiteit van de Azure AD.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/30/2018
ms.author: tamram
ms.openlocfilehash: 98af46707485d1ab49e7d8c6fb1729e6edc6b2ff
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35235862"
---
# <a name="use-an-azure-ad-identity-to-access-azure-storage-with-cli-or-powershell-preview"></a>Een Azure AD-identiteit gebruiken voor toegang tot Azure Storage met CLI of PowerShell (Preview)

Azure Storage biedt preview-extensies voor Azure CLI en PowerShell waarmee u zich aanmeldt en het uitvoeren van opdrachten in het script onder de identiteit van een Azure Active Directory (Azure AD). De identiteit van de Azure AD kan een gebruiker, groep of toepassing service-principal of kan een [beheerde service-identiteit](../../active-directory/managed-service-identity/overview.md). U kunt machtigingen voor toegang tot de opslagbronnen aan de identiteit van de Azure AD via op rollen gebaseerde toegangsbeheer (RBAC). Zie voor meer informatie over RBAC-rollen in Azure Storage [beheren rechten voor het Azure Storage-gegevens met RBAC (Preview)](storage-auth-aad-rbac.md).

Wanneer u bij Azure CLI of PowerShell met de identiteit van een Azure AD aanmelden, wordt een toegangstoken voor toegang tot Azure Storage onder die identiteit geretourneerd. Dat token wordt vervolgens automatisch gebruikt door de CLI of PowerShell te autoriseren bewerkingen op Azure Storage. Voor ondersteunde bewerkingen moet u niet langer een accountsleutel of SAS-token met de opdracht doorgeven.

> [!IMPORTANT]
> Deze preview is bedoeld voor gebruik van niet-productieve alleen. Productie service level agreements (Sla's) worden pas beschikbaar Azure AD-integratie voor Azure Storage is gedeclareerd in het algemeen beschikbaar. Als u Azure AD-integratie is nog niet ondersteund voor uw scenario, blijven de gedeelde sleutel autorisatie of SAS-tokens gebruiken in uw toepassingen. Zie voor meer informatie over de evaluatieversie [verifiëren van toegang tot Azure Storage met Azure Active Directory (Preview)](storage-auth-aad.md).
>
> Tijdens de preview kunnen de RBAC-roltoewijzingen doorgeven maximaal vijf minuten duren.
>
> Azure AD-integratie met Azure Storage vereist het gebruik van HTTPS voor Azure Storage-bewerkingen.

## <a name="supported-operations"></a>Ondersteunde bewerkingen

De preview-extensies worden ondersteund voor bewerkingen op containers en wachtrijen. Welke bewerkingen u kunt aanroepen, is afhankelijk van de machtigingen te krijgen tot de Azure AD-identiteit waarmee u zich voor Azure CLI of PowerShell aanmelden. Azure Storage-containers of wachtrijen machtigingen worden toegewezen via op rollen gebaseerde toegangsbeheer (RBAC). Bijvoorbeeld, als een gegevenslezer-rol is toegewezen aan de identiteit, kunt u uitvoeren scripting opdrachten die gegevens uit een container of de wachtrij lezen. Als een rol gegevens eigenaarsrechten is toegewezen aan de identiteit, kunt u opdrachten in het script dat lezen, schrijven of verwijderen van een container of de wachtrij of de gegevens die ze bevatten kunt uitvoeren. 

Zie voor meer informatie over de machtigingen die vereist zijn voor elke Azure Storage-bewerking op een container of een wachtrij [machtigingen voor het aanroepen van REST-bewerkingen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).  

## <a name="call-cli-commands-with-an-azure-ad-identity"></a>CLI-opdrachten met een Azure AD-identiteit aanroepen

De preview-extensie voor Azure CLI installeren:

1. Controleer of u versie 2.0.32 van Azure CLI hebt geïnstalleerd of hoger. Voer `az --version` om te controleren van de geïnstalleerde versie.
2. Voer de volgende opdracht voor het installeren van de preview-extensie: 

    ```azurecli
    az extension add -n storage-preview
    ```

De extensie voorbeeld voegt u een nieuw `--auth-mode` parameter om ondersteunde opdrachten:

- Stel de `--auth-mode` -parameter voor `login` aan te melden met een Azure AD-identiteit.
- Stel de `--auth-mode` -parameter voor de verouderde `key` waarde om te zoeken naar een sleutel als account geen parameters voor verificatie voor het account worden opgegeven. 

Bijvoorbeeld: voor het downloaden van een blob in Azure CLI met behulp van een Azure AD-identiteit eerst uitvoeren `az login`, roept u vervolgens de opdracht met `--auth-mode` ingesteld op `login`:

```azurecli
az login
az storage blob download --account-name storagesamples --container sample-container --name myblob.txt --file myfile.txt --auth-mode login 
```

De variabele die is gekoppeld aan de `--auth-mode` parameter `AZURE_STORAGE_AUTH_MODE`.

## <a name="call-powershell-commands-with-an-azure-ad-identity"></a>Aanroep van PowerShell-opdrachten met de identiteit van een Azure AD

Azure PowerShell kunnen aanmelden met een Azure AD-identiteit gebruiken:

1. Zorg ervoor dat u de nieuwste versie van PowerShellGet geïnstalleerd hebt. Voer de volgende opdracht voor het installeren van de meest recente:
 
    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```

2. Verwijder eerdere installaties van Azure PowerShell.
3. AzureRM installeren:

    ```powershell
    Install-Module AzureRM –Repository PSGallery –AllowClobber
    ```

4. Installeer de preview-module:

    ```powershell
    Install-Module -Name Azure.Storage -AllowPrerelease –AllowClobber 
    ```

5. Roep de [nieuw AzureStorageContext](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontext) cmdlet voor het maken van een context en omvatten de `-UseConnectedAccount` parameter. 
6. Geven de context aan de cmdlet voor het aanroepen van een cmdlet met een Azure AD-identiteit.

Het volgende voorbeeld ziet u hoe u de blobs in een container van Azure PowerShell met de identiteit van een Azure AD: 

```powershell
$ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -UseConnectedAccount 
Get-AzureStorageBlob -Container $sample-container -Context $ctx 
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over de rollen RBAC voor Azure-opslag, [beheren rechten voor het storage-gegevens met RBAC (Preview)](storage-auth-aad-rbac.md).
- Zie voor meer informatie over het gebruik van de Service-identiteit beheerd met Azure Storage, [verifiëren met Azure AD van een Azure Managed Service-identiteit (Preview)](storage-auth-aad-msi.md).
- Zie voor meer informatie over hoe u toegang verlenen aan containers en wachtrijen van binnen uw toepassingen moeten worden opgeslagen, [gebruik Azure AD met opslagtoepassingen](storage-auth-aad-app.md).
- Zie voor meer informatie over Azure AD-integratie voor Azure Blobs en wachtrijen, het Azure Storage-team blogbericht, [aankondigen van de Preview van Azure AD-verificatie voor Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
