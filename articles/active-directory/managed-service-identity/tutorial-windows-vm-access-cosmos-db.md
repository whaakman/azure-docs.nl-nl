---
title: Gebruik van een Windows-VM-MSI voor toegang tot Azure Cosmos-DB
description: Een zelfstudie die u bij het proces helpt van het gebruik van een System-Assigned beheerde Service identiteit (MSI) op een Windows-VM voor toegang tot Azure Cosmos DB.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2018
ms.author: arluca
ms.openlocfilehash: 0e62e11e0f86714b79cf2bc84bbb505b03df1ba6
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="use-a-windows-vm-msi-to-access-azure-cosmos-db"></a>Gebruik van een Windows-VM-MSI voor toegang tot Azure Cosmos-DB

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Deze zelfstudie laat zien hoe u maken en gebruiken van een Windows-VM-MSI voor toegang tot Cosmos DB. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Maak een MSI ingeschakeld Windows VM 
> * Cosmos DB-account maken
> * Verleen Windows VM MSI-toegang tot de toegangssleutels van de Cosmos-DB-account
> * Ophalen van een toegangstoken met behulp van de MSI van de Windows-VM om aan te roepen Azure Resource Manager
> * Toegangssleutels ophalen uit Azure Resource Manager Cosmos DB aanroepen

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]


## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Een virtuele Windows-machine in een nieuwe resourcegroep maken

Voor deze zelfstudie maken we een nieuwe Windows VM.  U kunt ook MSI op een bestaande virtuele machine inschakelen.

1. Klik op de knop **Een resource maken** in de linkerbovenhoek van Azure Portal.
2. Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. 
3. Geef de informatie van de virtuele machine op. De **gebruikersnaam** en **wachtwoord** gemaakte Hier ziet u de referenties die u kunt aanmelden bij de virtuele machine.
4. Kies de juiste **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5. Selecteer een nieuwe **resourcegroep** waarin u uw virtuele machine maakt, kiest u **nieuw**. Na het voltooien klikt u op **OK**.
6. Selecteer de grootte van de virtuele machine. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. Behoud de standaardinstellingen op de pagina instellingen en klik op **OK**.

   ![De installatiekopie van de alternatieve tekst](../media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>MSI op de virtuele machine inschakelen 

De MSI van een virtuele Machine kunt u toegangstokens ophalen uit Azure AD zonder referenties in uw code te plaatsen. Achter de inschakelen van MSI op een virtuele Machine via de Azure-portal biedt twee dingen: registreert uw virtuele machine met Azure AD voor het maken van een beheerde identiteit en configureert deze identiteit van de op de virtuele machine.

1. Selecteer de **virtuele Machine** dat u inschakelen van MSI wilt op.  
2. Klik op de linkernavigatiebalk **configuratie**. 
3. U ziet **beheerde Service-identiteit**. Als u wilt registreren en inschakelen van het MSI-bestand, selecteer **Ja**, als u wilt uitschakelen, kiest u Nee. 
4. Zorg ervoor dat u klikt op **opslaan** aan de configuratie op te slaan.  
   ![De installatiekopie van de alternatieve tekst](../media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

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

## <a name="grant-windows-vm-msi-access-to-the-cosmos-db-account-access-keys"></a>Verleen Windows VM MSI-toegang tot de toegangssleutels van de Cosmos-DB-account

Cosmos DB biedt geen systeemeigen ondersteuning voor Azure AD-verificatie. U kunt echter een MSI-bestand gebruiken voor het ophalen van een Cosmos-DB-toegangssleutel van de Resource Manager en de sleutel gebruiken voor toegang tot Cosmos DB. In deze stap maakt u uw MSI toegang verlenen tot de sleutels aan het account van de Cosmos-DB.

Werk de waarden voor de MSI identiteit om toegang te verlenen aan de Cosmos-DB-account in Azure Resource Manager met behulp van PowerShell, `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, en `<COSMOS DB ACCOUNT NAME>` voor uw omgeving. Vervang `<MSI PRINCIPALID>` met de `principalId` eigenschap die is geretourneerd door de `az resource show` opdracht in [ophalen van de principalID van de Linux-VM MSI](#retrieve-the-principalID-of-the-linux-VM's-MSI).  Cosmos DB ondersteunt twee detailniveaus bij gebruik van toegangstoetsen: lezen/schrijven toegang tot het account en alleen-lezen toegang tot het account.  Wijs de `DocumentDB Account Contributor` rol als u wilt lezen/schrijven-sleutels voor het account ophalen of toe te wijzen de `Cosmos DB Account Reader Role` functie, indien u wilt ophalen, alleen-lezen sleutels voor het account:

```azurepowershell
$spID = (Get-AzureRMVM -ResourceGroupName myRG -Name myVM).identity.principalid
New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
```

## <a name="get-an-access-token-using-the-windows-vms-msi-to-call-azure-resource-manager"></a>Ophalen van een toegangstoken met behulp van de MSI van de Windows-VM om aan te roepen Azure Resource Manager

Voor het restant van de zelfstudie werken we van de virtuele machine die we eerder hebben gemaakt. 

U moet de Azure Resource Manager PowerShell-cmdlets in dit gedeelte gebruiken.  Als u niet geïnstalleerd hebt, [de nieuwste versie downloaden](https://docs.microsoft.com/powershell/azure/overview) voordat u doorgaat.

U moet ook voor het installeren van de nieuwste versie van [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) op uw Windows-VM.

1. Navigeer in de Azure-portal naar **virtuele Machines**, gaat u naar uw Windows-machine, klikt u vervolgens vanuit de **overzicht** pagina op **Connect** aan de bovenkant. 
2. Voer in uw **gebruikersnaam** en **wachtwoord** voor die u hebt toegevoegd tijdens het maken van de virtuele machine van Windows. 
3. Nu dat u hebt gemaakt een **verbinding met extern bureaublad** met de virtuele machine, opent u PowerShell in de externe sessie.
4. Maak met behulp van Powershell Invoke-WebRequest, een aanvraag naar het lokale eindpunt MSI een access-token ophalen voor Azure Resource Manager.

    ```powershell
        $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Method GET -Headers @{Metadata="true"}
    ```

    > [!NOTE]
    > De waarde van de resourceparameter '' moet een exacte overeenkomst voor door Azure AD wordt verwacht. Wanneer u de resource-ID van Azure Resource Manager gebruikt, moet u de afsluitende slash op de URI opnemen.
    
    Pak vervolgens het element 'Inhoud', dat wordt opgeslagen als een tekenreeks in JSON JavaScript Object Notation () geformatteerd in de $response-object. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```
    Haal het toegangstoken vervolgens uit het antwoord.
    
    ```powershell
    $ArmToken = $content.access_token
    ```

## <a name="get-access-keys-from-azure-resource-manager-to-make-cosmos-db-calls"></a>Toegangssleutels ophalen uit Azure Resource Manager Cosmos DB aanroepen

Nu PowerShell gebruiken om aan te roepen Resource Manager met behulp van het toegangstoken opgehaald in de vorige sectie voor het ophalen van de toegangssleutel Cosmos DB. Wanneer we de toegangssleutel hebben, kunnen we Cosmos DB opvragen. Zorg ervoor dat u de `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, en `<COSMOS DB ACCOUNT NAME>` parameterwaarden met uw eigen waarden. Vervang de `<ACCESS TOKEN>` waarde met het toegangstoken dat u eerder hebt opgehaald.  Als u wilt lezen/schrijven-sleutels ophalen, gebruikt u type sleutel bewerking `listKeys`.  Als u wilt alleen-lezen-sleutels ophalen, gebruikt u het type sleutel bewerking `readonlykeys`:

```powershell
Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.DocumentDb/databaseAccounts/<COSMOS DB ACCOUNT NAME>/listKeys/?api-version=2016-12-01 -Method POST -Headers @{Authorization="Bearer $ARMToken"}
```
Het antwoord geeft de lijst met sleutels.  Bijvoorbeeld, als u krijgt alleen-lezen sleutels:

```powershell
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

- Zie voor een overzicht van MSI [overzicht van de Service-identiteit beheerd](overview.md).
