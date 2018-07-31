---
title: Toegang krijgen tot Azure Cosmos DB met een Managed Service Identity voor een Linux-VM
description: Een zelfstudie die u helpt bij het gebruiken van een System-Assigned Managed Service Identity op een Linux-VM voor toegang tot Azure Cosmos DB.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/09/2018
ms.author: daveba
ms.openlocfilehash: af148cd8b3eececb258057a8bf6a78216ec0e50a
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39258327"
---
# <a name="tutorial-use-a-linux-vm-managed-service-identity-to-access-azure-cosmos-db"></a>Zelfstudie: Toegang krijgen tot Azure Cosmos DB met een Managed Service Identity voor een Linux-VM 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]


In deze zelfstudie leert u hoe u een Managed Service Identity voor een Linux-VM kunt gebruiken en maken. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een Linux-VM maken waarop MSI is ingeschakeld
> * Cosmos DB-account maken
> * Een verzameling maken in het Cosmos DB-account
> * De Managed Service Identity toegang verlenen tot een Azure Cosmos DB-exemplaar
> * De `principalID` van de Managed Service Identity voor de Linux-VM ophalen
> * Een toegangstoken ophalen en daarmee Azure Resource Manager aanroepen
> * Toegangssleutels ophalen uit Azure Resource Manager voor Cosmos DB-aanroepen

## <a name="prerequisites"></a>Vereisten

Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com) voordat u verdergaat.

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Als u de CLI-scriptvoorbeelden in deze zelfstudie wilt uitvoeren, hebt u twee opties:

- Gebruik [Azure Cloud Shell](~/articles/cloud-shell/overview.md) vanuit Azure Portal of via de knop **Uitproberen** in de rechterbovenhoek van elk codeblok.
- [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 of later) als u liever een lokale CLI-console gebruikt.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Een virtuele Linux-machine maken in een nieuwe resourcegroep

Voor deze zelfstudie maakt u een nieuwe Linux-VM waarop Managed Service Identity is ingeschakeld.

Ga als volgt te werk om een VM te maken waarop Managed Service Identity is ingeschakeld:

1. Als u de Azure CLI in een lokale console gebruikt, meldt u zich eerst aan bij Azure met [az login](/cli/azure/reference-index#az_login). Gebruik een account dat is gekoppeld aan het Azure-abonnement waaronder u de VM wilt implementeren:

   ```azurecli-interactive
   az login
   ```

2. Maak met [az group create](/cli/azure/group/#az_group_create) een [resourcegroep](../../azure-resource-manager/resource-group-overview.md#terminology) voor insluiting en implementatie van uw VM en de bijbehorende bronnen. U kunt deze stap overslaan als u al een resourcegroep hebt die u in plaats daarvan wilt gebruiken:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Maak een VM met [az vm create](/cli/azure/vm/#az_vm_create). In het volgende voorbeeld wordt een VM genaamd *myVM* gemaakt met een Managed Service Identity, zoals aangevraagd door de parameter `--assign-identity`. Met de parameters `--admin-username` en `--admin-password` worden de naam van de gebruiker met beheerdersrechten en het wachtwoord van het account voor aanmelding bij de virtuele machine opgegeven. Werk deze waarden bij met waarden die geschikt zijn voor uw omgeving: 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12
   ```

## <a name="create-a-cosmos-db-account"></a>Cosmos DB-account maken 

Maak een Cosmos DB-account als u er nog geen hebt. U kunt deze stap overslaan en een bestaand Cosmos DB-account gebruiken. 

1. Klik op de knop **+/Nieuwe service maken** in de linkerbovenhoek van Azure Portal.
2. Klik op **Databases** en vervolgens op **Azure Cosmos DB**. Er verschijnt een nieuw venster 'Nieuw account'.
3. Voer een **Id** in voor het Cosmos DB-account, die u later zult gebruiken.  
4. **API** moet worden ingesteld op 'SQL'. De aanpak die in deze zelfstudie wordt beschreven, kan worden gebruikt met de andere beschikbare API-typen, maar de stappen in deze zelfstudie zijn voor de SQL-API.
5. Zorg ervoor dat de waarden van **Abonnement** en **Resourcegroep** overeenkomen met de waarden die u hebt opgegeven bij het maken van de virtuele machine in de vorige stap.  Selecteer een **Locatie** waar Cosmos DB beschikbaar is.
6. Klik op **Create**.

## <a name="create-a-collection-in-the-cosmos-db-account"></a>Een verzameling maken in het Cosmos DB-account

Voeg vervolgens een gegevensverzameling toe in het Cosmos DB-account, waarop u later query’s kunt uitvoeren.

1. Navigeer naar het zojuist gemaakte Cosmos DB-account.
2. Klik op het tabblad **Overzicht** op de knop **+/Verzameling toevoegen**. Het deelvenster 'Verzameling toevoegen' wordt weergegeven.
3. Geef de verzameling een database-ID en een verzameling-ID, selecteer een opslagcapaciteit, voer een partitiesleutel in, voer een doorvoerwaarde in en klik vervolgens op **OK**.  Voor deze zelfstudie volstaat het om ‘Test’ te gebruiken als database-ID en verzameling-ID en een vaste opslagcapaciteit en de laagste doorvoer (400 RU/s) te selecteren.  

## <a name="retrieve-the-principalid-of-the-linux-vms-managed-service-identity"></a>De `principalID` van de Managed Service Identity van de Linux-VM ophalen

Om in de volgende sectie vanuit Resource Manager toegang te krijgen tot de toegangssleutels van het Cosmos DB-account, moet u de `principalID` van de Managed Service Identity van de Linux-VM ophalen.  Vervang de waarden van de parameters `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` (resourcegroep waar de VM zich bevindt) en `<VM NAME>` door uw eigen waarden.

```azurecli-interactive
az resource show --id /subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAMe> --api-version 2017-12-01
```
Het antwoord bevat de details van de door het systeem toegewezen Managed Service Identity (noteer de principalID, want die wordt gebruikt in de volgende sectie):

```bash  
{
    "id": "/subscriptions/<SUBSCRIPTION ID>/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAMe>",
  "identity": {
    "principalId": "6891c322-314a-4e85-b129-52cf2daf47bd",
    "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533f8",
    "type": "SystemAssigned"
 }

```
## <a name="grant-your-linux-vm-managed-service-identity-access-to-the-cosmos-db-account-access-keys"></a>Uw Managed Service Identity op de Linux-VM toegang verlenen tot de toegangssleutels van het Cosmos DB-account

Cosmos DB biedt geen systeemeigen ondersteuning voor Azure AD-verificatie. U kunt echter Managed Service Identity gebruiken om een Cosmos DB-toegangssleutel op te halen uit Resource Manager, en vervolgens die sleutel gebruiken om toegang tot Cosmos DB te krijgen. In deze stap verleent u Managed Service Identity toegang tot de sleutels voor het Cosmos DB-account.

Werk de waarden voor `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` en `<COSMOS DB ACCOUNT NAME>` bij voor uw omgeving om de Managed Service Identity-identiteit met behulp van de Azure CLI toegang te geven tot het Cosmos DB-account in Azure Resource Manager. Vervang `<MSI PRINCIPALID>` door de `principalId` eigenschap die wordt geretourneerd door de opdracht `az resource show` in [De principalID van de Linux-VM-MSI ophalen](#retrieve-the-principalID-of-the-linux-VM's-MSI).  Cosmos DB ondersteunt twee granulariteitsniveaus bij het gebruik van toegangssleutels: lees-/schrijftoegang tot het account en alleen-lezen toegang tot het account.  Wijs de rol `DocumentDB Account Contributor` toe als u sleutels voor lezen/schrijven voor het account wilt krijgen, de rol `Cosmos DB Account Reader Role` als u alleen-lezensleutels voor het account wilt ophalen:

```azurecli-interactive
az role assignment create --assignee <MSI PRINCIPALID> --role '<ROLE NAME>' --scope "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMODS DB ACCOUNT NAME>"
```

Het antwoord bevat de details voor de gemaakte roltoewijzing:

```
{
  "id": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT>/providers/Microsoft.Authorization/roleAssignments/5b44e628-394e-4e7b-bbc3-d6cd4f28f15b",
  "name": "5b44e628-394e-4e7b-bbc3-d6cd4f28f15b",
  "properties": {
    "principalId": "c0833082-6cc3-4a26-a1b1-c4b5f90a981f",
    "roleDefinitionId": "/subscriptions/<SUBSCRIPTION ID>/providers/Microsoft.Authorization/roleDefinitions/fbdf93bf-df7d-467e-a4d2-9458aa1360c8",
    "scope": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT>"
  },
  "resourceGroup": "<RESOURCE GROUP>",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="get-an-access-token-using-the-linux-vms-managed-service-identity-and-use-it-to-call-azure-resource-manager"></a>Een toegangstoken ophalen met behulp van de Managed Service Identity op de Linux-VM en dit gebruiken om Azure Resource Manager aan te roepen

Werk voor de rest van de zelfstudie op de eerder gemaakte virtuele machine.

U hebt een SSH-client nodig om deze stappen uit te voeren. Als u Windows gebruikt, kunt u de SSH-client in het [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/install_guide) gebruiken. Zie [De sleutels van uw SSH-client gebruiken onder Windows in Azure](../../virtual-machines/linux/ssh-from-windows.md) of [Een sleutelpaar met een openbare SSH-sleutel en een privé-sleutel maken en gebruiken voor virtuele Linux-machines in Azure](../../virtual-machines/linux/mac-create-ssh-keys.md) als u hulp nodig hebt bij het configureren van de sleutels van uw SSH-client.

1. In Azure Portal navigeert u naar **Virtuele machines**, gaat u naar uw virtuele Linux-machine, klikt u vervolgens boven aan de pagina **Overzicht** op **Verbinden**. Kopieer de verbindingsreeks voor uw virtuele machine. 
2. Maak verbinding met uw virtuele machine via de SSH-client.  
3. Vervolgens wordt u gevraagd om het **wachtwoord** in te voeren dat u hebt toegevoegd bij het maken van de **Linux-VM**. Als het goed is, bent u daarna aangemeld.  
4. Gebruik CURL om een toegangstoken voor Azure Resource Manager op te halen: 
     
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true   
    ```
 
    > [!NOTE]
    > In de bovenstaande aanvraag moet de waarde van de parameter 'resource' exact overeenkomen met wat er in Azure AD wordt verwacht. Wanneer u de resource-id van Azure Resource Manager gebruikt, moet u de URI opgeven met een slash op het einde.
    > In het volgende antwoord is het element access_token ingekort.
    
    ```bash
    {"access_token":"eyJ0eXAiOi...",
     "expires_in":"3599",
     "expires_on":"1518503375",
     "not_before":"1518499475",
     "resource":"https://management.azure.com/",
     "token_type":"Bearer",
     "client_id":"1ef89848-e14b-465f-8780-bf541d325cd5"}
     ```
    
## <a name="get-access-keys-from-azure-resource-manager-to-make-cosmos-db-calls"></a>Toegangssleutels ophalen uit Azure Resource Manager voor Cosmos DB-aanroepen  

Gebruik nu CURL voor het aanroepen van Resource Manager met behulp van het toegangstoken dat in de vorige sectie is opgehaald, om de toegangssleutel voor het Cosmos DB-account op te halen. Wanneer we de toegangssleutel hebben, kunnen we query’s uitvoeren op de Cosmos DB. Vervang de parameterwaarden `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` en `<COSMOS DB ACCOUNT NAME>` door uw eigen waarden. Vervang de waarde `<ACCESS TOKEN>` door het toegangstoken dat u eerder hebt opgehaald.  Als u lees-/schrijfsleutels wilt ophalen, gebruikt u sleutelbewerkingstype `listKeys`.  Als u alleen-lezensleutels wilt ophalen, gebruikt u sleutelbewerkingstype `readonlykeys`:

```bash 
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT NAME>/<KEY OPERATION TYPE>?api-version=2016-03-31' -X POST -d "" -H "Authorization: Bearer <ACCESS TOKEN>" 
```

> [!NOTE]
> De tekst in de voorgaande URL is hoofdlettergevoelig, dus gebruik dienovereenkomstige hoofdletters en kleine letters voor de resourcegroepen. Daarnaast is het belangrijk om te weten dat dit een POST-aanvraag is en niet een GET-aanvraag. Zorg ervoor dat u een waarde doorgeeft voor het vastleggen van een maximale lengte met -d. Deze waarde kan NULL zijn.  

In het CURL-antwoord vindt u de lijst met sleutels.  Als u bijvoorbeeld de alleen-lezensleutels ophaalt:  

```bash 
{"primaryReadonlyMasterKey":"bWpDxS...dzQ==",
"secondaryReadonlyMasterKey":"38v5ns...7bA=="}
```

Nu u de toegangssleutel voor het Cosmos DB-account hebt, kunt u deze doorgeven aan een Cosmos DB-SDK en aanroepen doen om toegang tot het account te krijgen.  Voor een snel voorbeeld kunt u de toegangssleutel doorgeven aan de Azure CLI.  U kunt de <COSMOS DB CONNECTION URL> vinden op het tabblad **Overzicht** op de Cosmos DB-accountblade in Azure Portal.  Vervang de <ACCESS KEY> door de waarde die u hierboven hebt opgehaald:

```bash
az cosmosdb collection show -c <COLLECTION ID> -d <DATABASE ID> --url-connection "<COSMOS DB CONNECTION URL>" --key <ACCESS KEY>
```

Deze CLI-opdracht retourneert details over de verzameling:

```bash
{
  "collection": {
    "_conflicts": "conflicts/",
    "_docs": "docs/",
    "_etag": "\"00006700-0000-0000-0000-5a8271e90000\"",
    "_rid": "Es5SAM2FDwA=",
    "_self": "dbs/Es5SAA==/colls/Es5SAM2FDwA=/",
    "_sprocs": "sprocs/",
    "_triggers": "triggers/",
    "_ts": 1518498281,
    "_udfs": "udfs/",
    "id": "Test",
    "indexingPolicy": {
      "automatic": true,
      "excludedPaths": [],
      "includedPaths": [
        {
          "indexes": [
            {
              "dataType": "Number",
              "kind": "Range",
              "precision": -1
            },
            {
              "dataType": "String",
              "kind": "Range",
              "precision": -1
            },
            {
              "dataType": "Point",
              "kind": "Spatial"
            }
          ],
          "path": "/*"
        }
      ],
      "indexingMode": "consistent"
    }
  },
  "offer": {
    "_etag": "\"00006800-0000-0000-0000-5a8271ea0000\"",
    "_rid": "f4V+",
    "_self": "offers/f4V+/",
    "_ts": 1518498282,
    "content": {
      "offerIsRUPerMinuteThroughputEnabled": false,
      "offerThroughput": 400
    },
    "id": "f4V+",
    "offerResourceId": "Es5SAM2FDwA=",
    "offerType": "Invalid",
    "offerVersion": "V2",
    "resource": "dbs/Es5SAA==/colls/Es5SAM2FDwA=/"
  }
}
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd een Managed Service Identity op een virtuele Linux-machine te maken om toegang tot Cosmos DB te krijgen.  Zie voor meer informatie over Cosmos DB:

> [!div class="nextstepaction"]
>[Overzicht van Azure Cosmos DB](/azure/cosmos-db/introduction)

