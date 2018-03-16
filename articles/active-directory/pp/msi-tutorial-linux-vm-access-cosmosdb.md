---
title: Een gebruiker toegewezen beheerde Service identiteit (MSI) op een Linux-VM te gebruiken voor toegang tot Azure Cosmos-DB
description: Een zelfstudie die u bij het proces helpt van het gebruik van een User-Assigned beheerde Service identiteit (MSI) op een Linux-VM voor toegang tot Azure Cosmos DB.
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2018
ms.author: skwan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: dbb5e9e8f9accd618599010ab2bbb4a8760e534f
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="use-a-user-assigned-managed-service-identity-msi-on-a-linux-vm-to-access-azure-cosmos-db"></a>Een gebruiker toegewezen beheerde Service identiteit (MSI) op een Linux-VM te gebruiken voor toegang tot Azure Cosmos-DB 

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Deze zelfstudie laat zien hoe maken en gebruiken van een gebruiker toegewezen beheerde Service identiteit (MSI) van een virtuele Linux-Machine en vervolgens gebruiken voor toegang tot Azure Cosmos DB. Procedures voor:

> [!div class="checklist"]
> * Maak een beheerde Service-identiteit (MSI) toegewezen gebruiker
> * De gebruiker toegewezen MSI toewijzen aan een virtuele Linux-Machine
> * De MSI toegang verlenen tot een exemplaar van Azure DB die Cosmos
> * Ophalen van een toegangstoken met behulp van de identiteit van de gebruiker toegewezen MSI, en deze gebruiken voor toegang tot Azure Cosmos-DB

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Als u wilt uitvoeren in de CLI scriptvoorbeelden in deze zelfstudie, hebt u twee opties:

- Gebruik [Azure Cloud Shell](~/articles/cloud-shell/overview.md) vanuit de Azure-portal of via de **probeert het** knop in de rechterbovenhoek van elk codeblok.
- [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 of hoger) als u liever een lokale CLI-console te gebruiken.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Aanmelden bij de Azure portal op [ https://portal.azure.com ](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Een virtuele Linux-machine in een nieuwe resourcegroep maken

Voor deze zelfstudie maken we een nieuwe Linux VM. U kunt ook MSI op een bestaande virtuele machine inschakelen.

1. Klik op de **+ maken van een resource** gevonden in de linkerbovenhoek van de Azure portal.
2. Selecteer **Compute**, en selecteer vervolgens **Ubuntu Server 16.04 TNS VM**.
3. Geef de informatie van de virtuele machine op. Voor **verificatietype**, selecteer **openbare SSH-sleutel** of **wachtwoord**. De gemaakte referenties kunnen u zich aanmelden bij de virtuele machine.

    ![MSI Linux VM](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Kies een **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5. Onder **resourcegroep**, kies **nieuw** en typ de naam van de resourcegroep voor deze virtuele machine. Na het voltooien klikt u op **OK**.
6. Selecteer de grootte van de virtuele machine. Selecteer om te zien meer grootten, **weergeven van alle** of wijzigen van de ondersteunde schijf type filter. In de **instellingen** blade behoud de standaardinstellingen en klik op **OK**.

## <a name="create-a-user-assigned-msi"></a>Maken van een MSI gebruiker toegewezen

1. Als u gebruikmaakt van de console CLI (in plaats van een Azure-Cloud-Shell-sessie), eerst aanmelden bij Azure. Gebruik een account dat is gekoppeld aan het Azure-abonnement waarmee u wilt maken van de nieuwe MSI:

    ```azurecli
    az login
    ```

2. Maak een gebruiker toegewezen MSI met [az identiteit maken](/cli/azure/identity#az_identity_create). De `-g` parameter geeft u de resourcegroep waar het MSI-bestand is gemaakt, en de `-n` parameter geeft u de naam ervan. Zorg ervoor dat u de `<RESOURCE GROUP>` en `<MSI NAME>` parameterwaarden met uw eigen waarden:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```

    Het antwoord bevat de details voor de gebruiker toegewezen MSI, vergelijkbaar met het volgende voorbeeld gemaakt (Opmerking de `clientId` en `id` waarden voor uw MSI zoals ze worden gebruikt in latere stappen):

    ```json
    {
    "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
    "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=123444643-8088-4d70-9532-c3a0fdc190fz",
    "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
    "location": "westcentralus",
    "name": "<MSI NAME>",
    "principalId": "c0833082-6cc3-4a26-a1b1-c4b5f90a981f",
    "resourceGroup": "<RESOURCE GROUP>",
    "tags": {},
    "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
    "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
    }
    ```

## <a name="assign-your-user-assigned-msi-to-your-linux-vm"></a>De gebruiker toegewezen MSI toewijzen aan uw Linux-VM

In tegenstelling tot een MSI systeem toegewezen kan een gebruiker toegewezen MSI worden gebruikt door clients in meerdere Azure-resources. Voor deze zelfstudie maakt toewijzen u deze aan een enkele virtuele machine. U kunt deze ook toewijzen aan meer dan één virtuele machine.

De gebruiker toegewezen MSI toewijzen aan uw Linux-VM met [az vm toewijzen-identity](/cli/azure/vm#az_vm_assign_identity). Zorg ervoor dat u de `<RESOURCE GROUP>` en `<VM NAME>` parameterwaarden met uw eigen waarden. Gebruik de `id` eigenschap geretourneerd in de vorige stap voor de `--identities` parameterwaarde:

```azurecli-interactive
az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>"
```

## <a name="create-a-cosmos-db-account"></a>Cosmos DB-account maken 

Als u dit niet al hebt, moet u nu een Cosmos-DB-account maken. U kunt ook deze stap overslaan en gebruik een bestaande database van de Cosmos-account, als u liever. 

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

## <a name="grant-your-user-assigned-msi-access-to-the-cosmos-db-account-access-keys"></a>De gebruiker toegewezen MSI toegang verlenen tot de toegangssleutels van de Cosmos-DB-account

Cosmos DB biedt geen systeemeigen ondersteuning voor Azure AD-verificatie.  U kunt echter een MSI-bestand gebruiken voor het ophalen van een Cosmos-DB-toegangssleutel van de Resource Manager en vervolgens de sleutel gebruiken voor toegang tot Cosmos DB.  In deze stap verleent u dat uw gebruiker toegewezen MSI toegang tot de sleutels aan het account van de Cosmos-DB.

Eerst de MSI identiteit toegang verlenen tot de database van de Cosmos-account in Azure Resource Manager met de Azure CLI. Werk de waarden voor `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, en `<COSMOS DB ACCOUNT NAME>` voor uw omgeving. Vervang `<MSI PRINCIPALID>` met de `principalId` eigenschap die is geretourneerd door de `az identity create` opdracht in [maken van een gebruiker toegewezen MSI](#create-a-user-assigned-msi).  Cosmos DB ondersteunt twee detailniveaus bij gebruik van toegangstoetsen: lezen/schrijven toegang tot het account en alleen-lezen toegang tot het account.  Wijs de `DocumentDB Account Contributor` rol als u wilt lezen/schrijven-sleutels voor het account ophalen of toe te wijzen de `Cosmos DB Account Reader Role` functie, indien u wilt ophalen, alleen-lezen sleutels voor het account:

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

## <a name="get-an-access-token-using-the-user-assigned-msi-and-use-it-to-call-azure-resource-manager"></a>Een toegangstoken met behulp van de gebruiker toegewezen MSI ophalen en deze gebruiken om aan te roepen Azure Resource Manager

Werken de virtuele machine eerder hebt gemaakt voor het restant van de zelfstudie.

Deze stappen uit te voeren, moet u een SSH-client. Als u van Windows gebruikmaakt, kunt u de SSH-client in de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/install_guide). Als u informatie over het configureren van de client van uw SSH-sleutels nodig hebt, raadpleegt u [het gebruik van SSH-sleutels met Windows in Azure](../../virtual-machines/linux/ssh-from-windows.md), of [maken en de openbare en persoonlijke sleutelpaar voor een SSH gebruiken voor virtuele Linux-machines in Azure](../../virtual-machines/linux/mac-create-ssh-keys.md).

1. Navigeer in de Azure-portal naar **virtuele Machines**, gaat u naar uw virtuele Linux-machine, klikt u vervolgens vanuit de **overzicht** pagina op **Connect** aan de bovenkant. Kopieer de tekenreeks verbinding maken met uw virtuele machine. 
2. Verbinding maken met uw virtuele machine met behulp van uw SSH-client.  
3. Vervolgens maakt u wordt gevraagd om in te voeren uw **wachtwoord** u toegevoegd bij het maken van de **Linux VM**. U moet vervolgens worden aangemeld.  
4. Gebruik CURL om een toegangstoken ophalen voor Azure Resource Manager.  

    De CURL aanvraag en antwoord voor het toegangstoken is lager dan.  Vervang <CLIENT ID> met de clientId waarde van de gebruiker toegewezen MSI: 
    
    ```bash
    curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com/&client_id=<MSI CLIENT ID>" 
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

CURL nu gebruiken om aan te roepen Resource Manager met behulp van het toegangstoken dat we opgehaald in de vorige sectie voor het ophalen van de toegangssleutel Cosmos DB. Wanneer we de toegangssleutel hebben, kunnen we Cosmos DB opvragen. Zorg ervoor dat u de `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, en `<COSMOS DB ACCOUNT NAME>` parameterwaarden met uw eigen waarden. Vervang de `<ACCESS TOKEN>` waarde met het toegangstoken dat u eerder hebt opgehaald.  Als u wilt lezen/schrijven-sleutels ophalen, gebruikt u type sleutel bewerking `listKeys`.  Als u wilt alleen-lezen-sleutels ophalen, gebruikt u het type sleutel bewerking `readonlykeys`:

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

- Zie voor een overzicht van MSI [beheerde Service identiteit (MSI) voor Azure-resources](msi-overview.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.
