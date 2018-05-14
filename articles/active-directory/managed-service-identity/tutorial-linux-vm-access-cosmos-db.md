---
title: Gebruik van een Linux-VM-MSI voor toegang tot Azure Cosmos-DB
description: Een zelfstudie die u bij het proces helpt van het gebruik van een System-Assigned beheerde Service identiteit (MSI) op een Linux-VM voor toegang tot Azure Cosmos DB.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/09/2018
ms.author: skwan
ms.openlocfilehash: bcbafcb5b72fd156f0d8b4a4ddd52aab1d699996
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="use-a-linux-vm-msi-to-access-azure-cosmos-db"></a>Gebruik van een Linux-VM-MSI voor toegang tot Azure Cosmos-DB 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]


Deze zelfstudie laat zien hoe u maken en gebruiken van een Linux VM MSI. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Maak een Linux-VM met MSI ingeschakeld
> * Cosmos DB-account maken
> * Een verzameling maken in het account van de Cosmos-DB
> * De MSI toegang verlenen tot een exemplaar van Azure DB die Cosmos
> * Ophalen van de `principalID` van de van de Linux-VM MSI
> * Een toegangstoken ophalen en deze gebruiken om aan te roepen Azure Resource Manager
> * Toegangssleutels ophalen uit Azure Resource Manager Cosmos DB aanroepen

## <a name="prerequisites"></a>Vereisten

Als u al een Azure-account niet hebt [aanmelden voor een gratis account](https://azure.microsoft.com) voordat u doorgaat.

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Als u wilt uitvoeren in de CLI scriptvoorbeelden in deze zelfstudie, hebt u twee opties:

- Gebruik [Azure Cloud Shell](~/articles/cloud-shell/overview.md) vanuit de Azure-portal of via de **probeert het** knop in de rechterbovenhoek van elk codeblok.
- [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 of hoger) als u liever een lokale CLI-console te gebruiken.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Een virtuele Linux-machine in een nieuwe resourcegroep maken

Voor deze zelfstudie maakt u een nieuwe MSI Linux VM ingeschakeld.

Maken van een VM MSI ingeschakeld:

1. Als u de Azure CLI in een lokale console, eerst aanmelden bij het gebruik van Azure [az aanmelding](/cli/azure/reference-index#az_login). Gebruik een account dat is gekoppeld aan het Azure-abonnement waarmee u wilt implementeren, de virtuele machine:

   ```azurecli-interactive
   az login
   ```

2. Maak een [resourcegroep](../../azure-resource-manager/resource-group-overview.md#terminology) voor containment en implementatie van uw virtuele machine en de bijbehorende resources, met behulp van [az groep maken](/cli/azure/group/#az_group_create). U kunt deze stap overslaan als u al resourcegroep die u wilt gebruiken in plaats daarvan:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Maak een VM met [az vm maken](/cli/azure/vm/#az_vm_create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* met een MSI-bestand, zoals aangevraagd door de `--assign-identity` parameter. De `--admin-username` en `--admin-password` parameters geeft u het account voor gebruiker met beheerdersrechten en het wachtwoord voor aanmelding bij de virtuele machine. Bijwerken van deze waarden naargelang nodig is voor uw omgeving: 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12
   ```

## <a name="create-a-cosmos-db-account"></a>Cosmos DB-account maken 

Als u dit niet al hebt, maakt u een Cosmos-DB-account. U kunt deze stap overslaan en gebruik een bestaande database van de Cosmos-account. 

1. Klik op de **+/ nieuwe service maken** knop gevonden in de linkerbovenhoek van de Azure portal.
2. Klik op **Databases**, klikt u vervolgens **Azure Cosmos DB**, en een nieuwe 'nieuwe account' geeft het deelvenster.
3. Voer een **ID** voor de account Cosmos DB, waarin u later gebruiken.  
4. **API** moet worden ingesteld op 'SQL'. De methode die wordt beschreven in deze zelfstudie kan worden gebruikt met de andere beschikbare API-typen, maar de stappen in deze zelfstudie zijn voor de SQL-API.
5. Zorg ervoor dat de **abonnement** en **resourcegroep** overeenkomen met de gegevenstypen die u hebt opgegeven toen u uw virtuele machine in de vorige stap hebt gemaakt.  Selecteer een **locatie** waarin Cosmos DB beschikbaar is.
6. Klik op **Create**.

## <a name="create-a-collection-in-the-cosmos-db-account"></a>Een verzameling maken in het account van de Cosmos-DB

Vervolgens voegt u een gegevensverzameling in de Cosmos-DB-account die u kunt een query in latere stappen.

1. Navigeer naar uw nieuwe account Cosmos DB.
2. Op de **overzicht** tabblad klikt u op de **+/ verzameling toevoegen** knop en een 'verzameling toevoegen' dia's uit het deelvenster.
3. Geef de verzameling van een database-ID, verzameling-ID, selecteer een opslagcapaciteit, een partitiesleutel invoeren, voer een waarde van de doorvoer en klik vervolgens op **OK**.  Voor deze zelfstudie is het voldoende zijn voor het gebruik van 'Test' als de database-ID en de verzamelings-ID, een vaste opslagcapaciteit en de laagste doorvoer (400 RU/s) selecteren.  

## <a name="retrieve-the-principalid-of-the-linux-vms-msi"></a>Ophalen van de `principalID` van de Linux-VM MSI

Om toegang te krijgen tot de Cosmos-DB-account toegangssleutels van de Resource Manager in de volgende sectie, moet u voor het ophalen van de `principalID` van de Linux-VM MSI.  Zorg ervoor dat u de `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` (resourcegroep waarin u de VM zich bevindt), en `<VM NAME>` parameterwaarden met uw eigen waarden.

```azurecli-interactive
az resource show --id /subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAMe> --api-version 2017-12-01
```
Het antwoord bevat de details van het systeem toegewezen MSI-bestand (Opmerking: de principalID zoals deze wordt gebruikt in de volgende sectie):

```bash  
{
    "id": "/subscriptions/<SUBSCRIPTION ID>/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAMe>",
  "identity": {
    "principalId": "6891c322-314a-4e85-b129-52cf2daf47bd",
    "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533f8",
    "type": "SystemAssigned"
 }

```
## <a name="grant-your-linux-vm-msi-access-to-the-cosmos-db-account-access-keys"></a>Uw Linux VM MSI toegang verlenen tot de toegangssleutels van de Cosmos-DB-account

Cosmos DB biedt geen systeemeigen ondersteuning voor Azure AD-verificatie. U kunt echter een MSI-bestand gebruiken voor het ophalen van een Cosmos-DB-toegangssleutel van de Resource Manager en vervolgens de sleutel gebruiken voor toegang tot Cosmos DB. In deze stap maakt u uw MSI toegang verlenen tot de sleutels aan het account van de Cosmos-DB.

Werk de waarden voor de MSI identiteit om toegang te verlenen aan de database van de Cosmos-account in Azure Resource Manager met de Azure CLI, `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, en `<COSMOS DB ACCOUNT NAME>` voor uw omgeving. Vervang `<MSI PRINCIPALID>` met de `principalId` eigenschap die is geretourneerd door de `az resource show` opdracht in [ophalen van de principalID van de Linux-VM MSI](#retrieve-the-principalID-of-the-linux-VM's-MSI).  Cosmos DB ondersteunt twee detailniveaus bij gebruik van toegangstoetsen: lezen/schrijven toegang tot het account en alleen-lezen toegang tot het account.  Wijs de `DocumentDB Account Contributor` rol als u wilt lezen/schrijven-sleutels voor het account ophalen of toe te wijzen de `Cosmos DB Account Reader Role` functie, indien u wilt ophalen, alleen-lezen sleutels voor het account:

```azurecli-interactive
az role assignment create --assignee <MSI PRINCIPALID> --role '<ROLE NAME>' --scope "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMODS DB ACCOUNT NAME>"
```

Het antwoord bevat de details voor de roltoewijzing gemaakt:

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

## <a name="get-an-access-token-using-the-linux-vms-msi-and-use-it-to-call-azure-resource-manager"></a>Een toegangstoken met behulp van de Linux-VM MSI ophalen en deze gebruiken om aan te roepen Azure Resource Manager

Werken de virtuele machine eerder hebt gemaakt voor het restant van de zelfstudie.

Deze stappen uit te voeren, moet u een SSH-client. Als u van Windows gebruikmaakt, kunt u de SSH-client in de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/install_guide). Als u informatie over het configureren van de client van uw SSH-sleutels nodig hebt, raadpleegt u [het gebruik van SSH-sleutels met Windows in Azure](../../virtual-machines/linux/ssh-from-windows.md), of [maken en de openbare en persoonlijke sleutelpaar voor een SSH gebruiken voor virtuele Linux-machines in Azure](../../virtual-machines/linux/mac-create-ssh-keys.md).

1. Navigeer in de Azure-portal naar **virtuele Machines**, gaat u naar uw virtuele Linux-machine, klikt u vervolgens vanuit de **overzicht** pagina op **Connect** aan de bovenkant. Kopieer de tekenreeks verbinding maken met uw virtuele machine. 
2. Verbinding maken met uw virtuele machine met behulp van uw SSH-client.  
3. Vervolgens maakt u wordt gevraagd om in te voeren uw **wachtwoord** u toegevoegd bij het maken van de **Linux VM**. U moet vervolgens worden aangemeld.  
4. Gebruik CURL voor een toegangstoken ophalen voor Azure Resource Manager: 
     
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true   
    ```
 
    > [!NOTE]
    > De waarde van de resourceparameter '' moet een exacte overeenkomst voor wat er wordt verwacht door Azure AD, in de vorige aanvraag. Wanneer u de resource-ID van Azure Resource Manager gebruikt, moet u de afsluitende slash op de URI opnemen.
    > In het volgende antwoord, het element access_token zoals ingekort als beknopt alternatief bevat.
    
    ```bash
    {"access_token":"eyJ0eXAiOi...",
     "expires_in":"3599",
     "expires_on":"1518503375",
     "not_before":"1518499475",
     "resource":"https://management.azure.com/",
     "token_type":"Bearer",
     "client_id":"1ef89848-e14b-465f-8780-bf541d325cd5"}
     ```
    
## <a name="get-access-keys-from-azure-resource-manager-to-make-cosmos-db-calls"></a>Toegangssleutels ophalen uit Azure Resource Manager Cosmos DB aanroepen  

CURL nu gebruiken om aan te roepen Resource Manager met behulp van het toegangstoken opgehaald in de vorige sectie voor het ophalen van de toegangssleutel Cosmos DB. Wanneer we de toegangssleutel hebben, kunnen we Cosmos DB opvragen. Zorg ervoor dat u de `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, en `<COSMOS DB ACCOUNT NAME>` parameterwaarden met uw eigen waarden. Vervang de `<ACCESS TOKEN>` waarde met het toegangstoken dat u eerder hebt opgehaald.  Als u wilt lezen/schrijven-sleutels ophalen, gebruikt u type sleutel bewerking `listKeys`.  Als u wilt alleen-lezen-sleutels ophalen, gebruikt u het type sleutel bewerking `readonlykeys`:

```bash 
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT NAME>/<KEY OPERATION TYPE>?api-version=2016-03-31' -X POST -d "" -H "Authorization: Bearer <ACCESS TOKEN>" 
```

> [!NOTE]
> De tekst in de voorgaande URL is hoofdlettergevoelig, dus zorg ervoor dat als u upper-kleine voor uw resourcegroepen gebruikt in overeenstemming met het dienovereenkomstig. Bovendien is het belangrijk te weten dat dit een POST-aanvraag niet op een GET-aanvraag is en zorg ervoor dat u een waarde voor het vastleggen van een lengtelimiet met -d die NULL kunnen worden doorgegeven.  

Het antwoord CURL biedt u de lijst met sleutels.  Bijvoorbeeld, als u krijgt de sleutels alleen-lezen:  

```bash 
{"primaryReadonlyMasterKey":"bWpDxS...dzQ==",
"secondaryReadonlyMasterKey":"38v5ns...7bA=="}
```

Nu dat u de toegangssleutel voor het account van de Cosmos-database kunt u doorgegeven aan een Cosmos-SDK voor Documentdb en aanroepen voor toegang tot het account hebben.  Voor een voorbeeld van een snelle, kunt u de toegangssleutel doorgeven aan de Azure CLI.  U krijgt de <COSMOS DB CONNECTION URL> van de **overzicht** tabblad op de blade van de Cosmos-DB-account in de Azure portal.  Vervang de <ACCESS KEY> met de waarde die u hierboven hebt verkregen:

```bash
az cosmosdb collection show -c <COLLECTION ID> -d <DATABASE ID> --url-connection "<COSMOS DB CONNECTION URL>" --key <ACCESS KEY>
```

Met deze opdracht CLI retourneert details over de verzameling:

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

- Zie voor een overzicht van MSI [beheerde Service identiteit (MSI) voor Azure-resources](overview.md).

