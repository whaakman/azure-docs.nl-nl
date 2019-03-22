---
title: Gebruik van een beheerde identiteit met Azure Container Instances
description: Informatie over het gebruik van een beheerde identiteit te verifiëren met andere Azure-services van Azure Container Instances.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 10/22/2018
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: ac0a84aa3121c6ebb91860c96c0f6692827c8a3f
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58336521"
---
# <a name="how-to-use-managed-identities-with-azure-container-instances"></a>Het gebruik van beheerde identiteiten met Azure Container Instances

Gebruik [beheerde identiteiten voor een Azure-resources](../active-directory/managed-identities-azure-resources/overview.md) code uitvoeren in Azure Container Instances die met andere Azure-services - communiceert zonder het onderhouden van alle geheimen of referenties in de code. De functie biedt de implementatie van een Azure Container Instances met een automatisch beheerde identiteit in Azure Active Directory.

In dit artikel leert u meer informatie over beheerde identiteiten in Azure Container Instances en:

> [!div class="checklist"]
> * Een gebruiker toegewezen of door het systeem toegewezen identiteit in de containergroep van een inschakelen
> * De identiteit toegang verlenen aan een Azure Key Vault
> * De beheerde identiteit gebruiken voor toegang tot een Key Vault van een container die wordt uitgevoerd

Pas de voorbeelden om te schakelen en gebruiken van identiteiten in Azure Container Instances voor toegang tot andere Azure-services. Deze voorbeelden zijn interactief. In de praktijk uw containerinstallatiekopieën echter uitgevoerd code voor toegang tot Azure-services.

> [!NOTE]
> Momenteel kunt u een beheerde identiteit niet gebruiken in de containergroep van een geïmplementeerd naar een virtueel netwerk.

## <a name="why-use-a-managed-identity"></a>Waarom een beheerde identiteit gebruiken?

Gebruik van een beheerde identiteit in een container die wordt uitgevoerd om te verifiëren met een [die ondersteuning biedt voor Azure AD-verificatie](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication) zonder dat de referenties in de containercode van uw worden beheerd. Voor services die bieden geen ondersteuning voor AD-verificatie, kunt u geheimen in Azure Key Vault opslaan en gebruiken van de beheerde identiteit voor toegang tot Key Vault om op te halen van referenties. Zie voor meer informatie over het gebruik van een beheerde identiteit [wat is beheerde identiteiten voor Azure-resources?](../active-directory/managed-identities-azure-resources/overview.md)

> [!IMPORTANT]
> Deze functie is momenteel beschikbaar als preview-product. Previews worden voor u beschikbaar gesteld op voorwaarde dat u akkoord gaat met de [aanvullende gebruiksvoorwaarden](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Sommige aspecten van deze functie worden mogelijk nog gewijzigd voordat de functie algemeen beschikbaar wordt. Beheerde identiteiten worden op dit moment alleen ondersteund op Linux-container instances.
>  

### <a name="enable-a-managed-identity"></a>Een beheerde identiteit inschakelen

 In Azure Container Instances, zijn beheerde identiteiten voor Azure-resources vanaf de REST API-versie 2018-01-10 en de bijbehorende SDK's en hulpprogramma's ondersteund. Wanneer u een containergroep maakt, een of meer beheerde identiteiten inschakelen door in te stellen een [ContainerGroupIdentity](/rest/api/container-instances/containergroups/createorupdate#containergroupidentity) eigenschap. U kunt ook inschakelen of beheerde identiteiten bijwerken nadat een containergroep wordt uitgevoerd. een actie zorgt ervoor dat de containergroep om opnieuw te starten. Om in te stellen de identiteiten op een nieuwe of bestaande containergroep, de Azure CLI, een Resource Manager-sjabloon of een YAML-bestand te gebruiken. 

Azure Container Instances biedt ondersteuning voor beide typen beheerde Azure-id's: gebruiker toegewezen en het systeem is toegewezen. U kunt een door het systeem toegewezen identiteit, een of meer door de gebruiker toegewezen identiteiten of beide typen identiteiten inschakelen op een containergroep, een. 

* Een **gebruiker toegewezen** beheerde identiteit als zelfstandige Azure-resource in de Azure AD-tenant die wordt vertrouwd door het abonnement in gebruik is gemaakt. Nadat de identiteit is gemaakt, kan de identiteit worden toegewezen aan een of meer Azure-resources (in Azure Container Instances of andere Azure-services). De levenscyclus van een gebruiker toegewezen identiteit wordt afzonderlijk beheerd vanaf de levenscyclus van de containergroepen of andere serviceresources waaraan deze toegewezen. Dit gedrag is vooral nuttig in Azure Container Instances. Omdat de identiteit van de overschrijdt de levensduur van een containergroep, kunt u het opnieuw gebruiken samen met andere standaardinstellingen waarmee uw containerimplementaties groep maximaal herhaalbare.

* Een **systeem toegewezen** beheerde identiteit rechtstreeks op de containergroep van een in Azure Container Instances is ingeschakeld. Wanneer deze ingeschakeld, maakt Azure een identiteit voor de groep in de Azure AD-tenant die wordt vertrouwd door het abonnement van het exemplaar. Nadat de identiteit is gemaakt, worden de referenties zijn ingericht in elke container in de containergroep. De levenscyclus van een systeem toegewezen identiteit is rechtstreeks gekoppeld aan de containergroep die ingeschakeld op. Wanneer de groep wordt verwijderd, opschonen Azure automatisch van de referenties en de identiteit in Azure AD.

### <a name="use-a-managed-identity"></a>Een beheerde identiteit gebruiken

Voor het gebruik van een beheerde identiteit, moet de identiteit in eerste instantie toegangsrechten voor een of meer Azure-serviceresources (zoals een Web-App, een Key Vault of een Storage-Account) in het abonnement. Voor toegang tot de Azure-resources uit een container die wordt uitgevoerd, uw code moet verkrijgen een *toegangstoken* van een Azure AD-eindpunt. Uw code verzendt vervolgens het toegangstoken op een aanroep van een service die ondersteuning biedt voor Azure AD-verificatie. 

Met behulp van een beheerde identiteit in een container die wordt uitgevoerd is in wezen hetzelfde als wanneer u een identiteit in een Azure-VM. Zie de VM-informatie voor het gebruik van een [token](../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md), [Azure PowerShell of Azure CLI](../active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in.md), of de [Azure-SDK's](../active-directory/managed-identities-azure-resources/how-to-use-vm-sdk.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om te installeren en de CLI lokaal gebruikt, in dit artikel is vereist dat u de Azure CLI versie 2.0.49 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="create-an-azure-key-vault"></a>Een Azure Key Vault maken

De voorbeelden in dit artikel voor het gebruik van een beheerde identiteit in Azure Container Instances voor toegang tot een Azure Key Vault-geheim. 

Maak eerst een resourcegroep met de naam *myResourceGroup* in de *eastus* locatie met de volgende [az-groep maken](/cli/azure/group?view=azure-cli-latest#az-group-create) opdracht:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Gebruik de [az keyvault maken](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) opdracht voor het maken van een Key Vault. Zorg ervoor dat Geef een unieke naam voor de Key Vault. 

```azurecli-interactive
az keyvault create --name mykeyvault --resource-group myResourceGroup --location eastus
```

Een voorbeeld-geheim Store in de Key Vault met de [az keyvault secret set](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-set) opdracht:

```azurecli-interactive
az keyvault secret set --name SampleSecret --value "Hello Container Instances!" --description ACIsecret  --vault-name mykeyvault
```

Ga door met de volgende voorbeelden voor toegang tot de Key Vault met behulp van een gebruiker toegewezen of door het systeem toegewezen beheerde identiteit in Azure Container Instances.

## <a name="example-1-use-a-user-assigned-identity-to-access-azure-key-vault"></a>Voorbeeld 1: Een gebruiker toegewezen identiteit gebruiken voor toegang tot Azure Key Vault

### <a name="create-an-identity"></a>Een identiteit maken

Maak eerst een identiteit in uw abonnement met de [az-identiteit maken](/cli/azure/identity?view=azure-cli-latest#az-identity-create) opdracht. U kunt dezelfde resourcegroep gebruikt voor het maken van de Key Vault gebruiken of een ander account gebruiken.

```azurecli-interactive
az identity create --resource-group myResourceGroup --name myACIId
```

U kunt de identiteit in de volgende stappen gebruiken de [az identiteit show](/cli/azure/identity?view=azure-cli-latest#az-identity-show) opdracht voor het opslaan van de service-principal-ID en resource-ID van de identiteit in de variabelen.

```azurecli-interactive
# Get service principal ID of the user-assigned identity
spID=$(az identity show --resource-group myResourceGroup --name myACIId --query principalId --output tsv)

# Get resource ID of the user-assigned identity
resourceID=$(az identity show --resource-group myResourceGroup --name myACIId --query id --output tsv)
```

### <a name="enable-a-user-assigned-identity-on-a-container-group"></a>Een gebruiker toegewezen identiteit voor een containergroep inschakelen

Voer de volgende [az container maken](/cli/azure/container?view=azure-cli-latest#az-container-create) opdracht maakt een containerinstantie op basis van Ubuntu-Server. In dit voorbeeld bevat een enkele container-groep die u gebruiken kunt voor interactieve toegang tot andere Azure-services. De `--assign-identity` parameter uw gebruiker toegewezen beheerde identiteit wordt doorgegeven aan de groep. De opdracht langlopende houdt u de container die wordt uitgevoerd. In dit voorbeeld wordt dezelfde resourcegroep gebruikt voor het maken van de Key Vault, maar u kunt een andere naam opgeven.

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/azure-cli --assign-identity $resourceID --command-line "tail -f /dev/null"
```

Binnen enkele seconden krijgt u een reactie van de Azure-CLI die aangeeft dat de implementatie is voltooid. Controleer de status ervan met de [az container show](/cli/azure/container?view=azure-cli-latest#az-container-show) opdracht.

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer
```

De `identity` sectie in de uitvoer lijkt op het volgende, waarin de identiteit is ingesteld in de containergroep. De `principalID` onder `userAssignedIdentities` is de service-principal van de identiteit die u hebt gemaakt in Azure Active Directory:

```console
...
"identity": {
    "principalId": "null",
    "tenantId": "xxxxxxxx-f292-4e60-9122-xxxxxxxxxxxx",
    "type": "UserAssigned",
    "userAssignedIdentities": {
      "/subscriptions/xxxxxxxx-0903-4b79-a55a-xxxxxxxxxxxx/resourcegroups/danlep1018/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myACIId": {
        "clientId": "xxxxxxxx-5523-45fc-9f49-xxxxxxxxxxxx",
        "principalId": "xxxxxxxx-f25b-4895-b828-xxxxxxxxxxxx"
      }
    }
  },
...
```

### <a name="grant-user-assigned-identity-access-to-the-key-vault"></a>De gebruiker toegewezen identiteit toegang verlenen tot de Key Vault

Voer de volgende [az keyvault-beleid instellen](/cli/azure/keyvault?view=azure-cli-latest) -opdracht uit om een beleid voor toegang in de Key Vault. Het volgende voorbeeld laat de gebruiker toegewezen identiteit geheimen ophalen uit de Key Vault:

```azurecli-interactive
 az keyvault set-policy --name mykeyvault --resource-group myResourceGroup --object-id $spID --secret-permissions get
```

### <a name="use-user-assigned-identity-to-get-secret-from-key-vault"></a>Door de gebruiker toegewezen identiteit gebruiken om op te halen geheim uit Key Vault

U kunt nu de beheerde identiteit gebruiken voor toegang tot de Key Vault in de actieve containerinstantie. In dit voorbeeld moet u eerst een bash-shell in de container starten:

```azurecli-interactive
az container exec --resource-group myResourceGroup --name mycontainer --exec-command "/bin/bash"
```

Voer de volgende opdrachten in de bash-shell in de container. Als u een toegangstoken uit aan Azure Active Directory gebruiken om te verifiëren naar Key Vault, moet u de volgende opdracht uitvoeren:

```bash
curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net%2F' -H Metadata:true -s
```

Uitvoer:

```bash
{"access_token":"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9......xxxxxxxxxxxxxxxxx","refresh_token":"","expires_in":"28799","expires_on":"1539927532","not_before":"1539898432","resource":"https://vault.azure.net/","token_type":"Bearer"}
```

Voor het opslaan van het toegangstoken in een variabele gebruiken in de volgende opdrachten om te verifiëren, moet u de volgende opdracht uitvoeren:

```bash
token=$(curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -H Metadata:true | jq -r '.access_token')

```

Nu het toegangstoken gebruiken om te verifiëren naar Key Vault en een geheim lezen. Zorg ervoor dat u de naam van uw key vault in de URL (*https://mykeyvault.vault.azure.net/...*):

```bash
curl https://mykeyvault.vault.azure.net/secrets/SampleSecret/?api-version=2016-10-01 -H "Authorization: Bearer $token"
```

Het antwoord op de volgende, lijkt de geheime sleutel weergegeven. In uw code zou u deze uitvoer om op te halen van het geheim parseren. Vervolgens gebruikt u het geheim in een latere bewerking voor toegang tot een andere Azure-resource.

```bash
{"value":"Hello Container Instances!","contentType":"ACIsecret","id":"https://mykeyvault.vault.azure.net/secrets/SampleSecret/xxxxxxxxxxxxxxxxxxxx","attributes":{"enabled":true,"created":1539965967,"updated":1539965967,"recoveryLevel":"Purgeable"},"tags":{"file-encoding":"utf-8"}}
```

## <a name="example-2-use-a-system-assigned-identity-to-access-azure-key-vault"></a>Voorbeeld 2: Gebruik van een systeem toegewezen identiteit voor toegang tot Azure Key Vault

### <a name="enable-a-system-assigned-identity-on-a-container-group"></a>Een systeem toegewezen identiteit voor een containergroep inschakelen

Voer de volgende [az container maken](/cli/azure/container?view=azure-cli-latest#az-container-create) opdracht maakt een containerinstantie op basis van Ubuntu-Server. In dit voorbeeld bevat een enkele container-groep die u gebruiken kunt voor interactieve toegang tot andere Azure-services. De `--assign-identity` parameter zonder extra waarde kunt u een systeem toegewezen beheerde identiteit in de groep. De opdracht langlopende houdt u de container die wordt uitgevoerd. In dit voorbeeld wordt dezelfde resourcegroep gebruikt voor het maken van de Key Vault, maar u kunt een andere naam opgeven.

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/azure-cli --assign-identity --command-line "tail -f /dev/null"
```

Binnen enkele seconden krijgt u een reactie van de Azure-CLI die aangeeft dat de implementatie is voltooid. Controleer de status ervan met de [az container show](/cli/azure/container?view=azure-cli-latest#az-container-show) opdracht.

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer
```

De `identity` sectie in de uitvoer lijkt op het volgende weergegeven dat een systeem toegewezen identiteit is gemaakt in Azure Active Directory:

```console
...
"identity": {
    "principalId": "xxxxxxxx-528d-7083-b74c-xxxxxxxxxxxx",
    "tenantId": "xxxxxxxx-f292-4e60-9122-xxxxxxxxxxxx",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
},
...
```

Een variabele instellen op de waarde van `principalId` (de service principal-ID) van de identiteit, voor gebruik in latere stappen.

```azurecli-interactive
spID=$(az container show --resource-group myResourceGroup --name mycontainer --query identity.principalId --out tsv)
```

### <a name="grant-container-group-access-to-the-key-vault"></a>Container groepstoegang verlenen tot de Key Vault

Voer de volgende [az keyvault-beleid instellen](/cli/azure/keyvault?view=azure-cli-latest) -opdracht uit om een beleid voor toegang in de Key Vault. Het volgende voorbeeld kunt de identiteit systeem beheerde geheimen ophalen uit de Key Vault:

```azurecli-interactive
 az keyvault set-policy --name mykeyvault --resource-group myResourceGroup --object-id $spID --secret-permissions get
```

### <a name="use-container-group-identity-to-get-secret-from-key-vault"></a>Container groeps-id gebruiken om op te halen geheim uit Key Vault

U kunt nu de beheerde identiteit gebruiken voor toegang tot de Key Vault in de actieve containerinstantie. In dit voorbeeld moet u eerst een bash-shell in de container starten:

```azurecli-interactive
az container exec --resource-group myResourceGroup --name mycontainer --exec-command "/bin/bash"
```

Voer de volgende opdrachten in de bash-shell in de container. Als u een toegangstoken uit aan Azure Active Directory gebruiken om te verifiëren naar Key Vault, moet u de volgende opdracht uitvoeren:

```bash
curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net%2F' -H Metadata:true -s
```

Uitvoer:

```bash
{"access_token":"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9......xxxxxxxxxxxxxxxxx","refresh_token":"","expires_in":"28799","expires_on":"1539927532","not_before":"1539898432","resource":"https://vault.azure.net/","token_type":"Bearer"}
```

Voor het opslaan van het toegangstoken in een variabele gebruiken in de volgende opdrachten om te verifiëren, moet u de volgende opdracht uitvoeren:

```bash
token=$(curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -H Metadata:true | jq -r '.access_token')

```

Nu het toegangstoken gebruiken om te verifiëren naar Key Vault en een geheim lezen. Zorg ervoor dat u de naam van uw key vault in de URL (*https:\//mykeyvault.vault.azure.net/...* ):

```bash
curl https://mykeyvault.vault.azure.net/secrets/SampleSecret/?api-version=2016-10-01 -H "Authorization: Bearer $token"
```

Het antwoord op de volgende, lijkt de geheime sleutel weergegeven. In uw code zou u deze uitvoer om op te halen van het geheim parseren. Vervolgens gebruikt u het geheim in een latere bewerking voor toegang tot een andere Azure-resource.

```bash
{"value":"Hello Container Instances!","contentType":"ACIsecret","id":"https://mykeyvault.vault.azure.net/secrets/SampleSecret/xxxxxxxxxxxxxxxxxxxx","attributes":{"enabled":true,"created":1539965967,"updated":1539965967,"recoveryLevel":"Purgeable"},"tags":{"file-encoding":"utf-8"}}
```

## <a name="enable-managed-identity-using-resource-manager-template"></a>Beheerde identiteit met behulp van Resource Manager-sjabloon inschakelen

Om in te schakelen van een beheerde identiteit in een container groep met een [Resource Manager-sjabloon](container-instances-multi-container-group.md), stel de `identity` eigenschap van de `Microsoft.ContainerInstance/containerGroups` object met een `ContainerGroupIdentity` object. De volgende fragmenten ziet de `identity` eigenschap geconfigureerd voor verschillende scenario's. Zie de [verwijzing naar het Resource Manager-sjabloon](/azure/templates/microsoft.containerinstance/containergroups). Geef een `apiVersion` van `2018-10-01`.

### <a name="user-assigned-identity"></a>De gebruiker toegewezen identiteit

Een gebruiker toegewezen identiteit is een resource-ID van het formulier:

```
"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{identityName}"
``` 

U kunt een of meer door de gebruiker toegewezen identiteiten inschakelen.

```json
"identity": {
    "type": "UserAssigned",
    "userAssignedIdentities": {
        "myResourceID1": {
            }
        }
    }
```

### <a name="system-assigned-identity"></a>Het systeem toegewezen identiteit

```json
"identity": {
    "type": "SystemAssigned"
    }
```

### <a name="system--and-user-assigned-identities"></a>Systeem - en gebruiker toegewezen identiteiten

U kunt zowel een door het systeem toegewezen identiteit als een of meer door de gebruiker toegewezen identiteiten inschakelen op een containergroep, een.

```json
"identity": {
    "type": "System Assigned, UserAssigned",
    "userAssignedIdentities": {
        "myResourceID1": {
            }
        }
    }
...
```

## <a name="enable-managed-identity-using-yaml-file"></a>Beheerde identiteit YAML-bestand met inschakelen

Om in te schakelen van een beheerde identiteit in een containergroep geïmplementeerd met behulp van een [YAML-bestand](container-instances-multi-container-yaml.md), bevatten de volgende YAML.
Geef een `apiVersion` van `2018-10-01`.

### <a name="user-assigned-identity"></a>De gebruiker toegewezen identiteit

Een gebruiker toegewezen identiteit is een resource-ID van het formulier 

```
'/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{identityName}'
```

U kunt een of meer door de gebruiker toegewezen identiteiten inschakelen.

```YAML
identity:
  type: UserAssigned
  userAssignedIdentities:
    {'myResourceID1':{}}
```

### <a name="system-assigned-identity"></a>Het systeem toegewezen identiteit

```YAML
identity:
  type: SystemAssigned
```

### <a name="system--and-user-assigned-identities"></a>Systeem - en gebruiker toegewezen identiteiten

U kunt zowel een door het systeem toegewezen identiteit als een of meer door de gebruiker toegewezen identiteiten inschakelen op een containergroep, een.

```YAML
identity:
  type: SystemAssigned, UserAssigned
  userAssignedIdentities:
   {'myResourceID1':{}}
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over beheerde identiteiten in Azure Container Instances en hoe u:

> [!div class="checklist"]
> * Een gebruiker toegewezen of door het systeem toegewezen identiteit in de containergroep van een inschakelen
> * De identiteit toegang verlenen aan een Azure Key Vault
> * De beheerde identiteit gebruiken voor toegang tot een Key Vault van een container die wordt uitgevoerd

* Meer informatie over [beheerde identiteiten voor een Azure-resources](/azure/active-directory/managed-identities-azure-resources/).

* Zie een [Azure Go SDK voorbeeld](https://medium.com/@samkreter/c98911206328) van het gebruik van een beheerde identiteit voor toegang tot een Key Vault vanuit Azure Container Instances.
