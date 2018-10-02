---
title: Gebruik van beheerde identiteiten voor het verifiëren van Azure Stream Analytics-taken op Azure Data Lake Storage Gen1 uitvoer (Preview)
description: ''
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/27/2018
ms.openlocfilehash: 72bf467cc0f2ba195aa4f25228bc9e08605cd4ee
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48018586"
---
# <a name="use-managed-identities-to-authenticate-azure-stream-analytics-jobs-to-azure-data-lake-storage-gen1-output-preview"></a>Gebruik van beheerde identiteiten voor het verifiëren van Azure Stream Analytics-taken op Azure Data Lake Storage Gen1 uitvoer (Preview)

Azure Stream Analytics biedt ondersteuning voor verificatie met Azure Data Lake Storage (ADLS) Gen1 uitvoer beheerde identiteit. De identiteit is een beheerde toepassing in Azure Active Directory die staat voor een bepaalde Stream Analytics-taak geregistreerd en kan worden gebruikt om een specifieke resource te verifiëren. Beheerde identiteiten elimineert u de beperkingen van methoden voor verificatie op basis van een gebruiker, zoals u dat u hoeft te verifiëren vanwege wachtwoord te wijzigen of token verlopen voor voor een gebruiker die zich na negentig dagen voordoen. Daarnaast beheerde identiteiten helpen bij de automatisering van Stream Analytics-taak implementaties die worden uitgevoerd naar Azure Data Lake Storage Gen1.

Ga naar de [acht nieuwe functies in Azure Stream Analytics](https://azure.microsoft.com/en-us/blog/eight-new-features-in-azure-stream-analytics/) blogbericht om u te registreren voor deze Preview-versie en lees meer over nieuwe functies.

Dit artikel ziet u twee manieren om in te schakelen beheerde identiteit voor een Azure Stream Analytics-taak die op een Azure Data Lake Storage Gen1 uitvoert: via de Azure-portal en sjabloonimplementatie van Azure Resource Manager-.

## <a name="enable-managed-identity-with-azure-portal"></a>Beheerde identiteit inschakelen met Azure portal

1. Begin met het maken van een nieuwe Stream Analytics-taak of door het openen van een bestaande taak in Azure portal. Selecteer in de menubalk aan de linkerkant van het scherm bevindt, **beheerde identiteit (preview)** bevindt zich onder **configureren**.

   ![Configureren van de Preview-versie van Stream Analytics beheerde identiteit](./media/stream-analytics-managed-identities-adls/stream-analytics-managed-identity-preview.png)

2. Selecteer **gebruik door het systeem toegewezen beheerde identiteit (preview)** in het venster dat wordt weergegeven aan de rechterkant. Klik op **opslaan** een service-principal voor de identiteit van de Stream Analytics-taak maken in Azure Active Directory. De levenscyclus van de identiteit van de zojuist gemaakte worden beheerd door Azure. Wanneer de Stream Analytics-taak wordt verwijderd, wordt de identiteit van de bijbehorende (dat wil zeggen, de service-principal) wordt automatisch verwijderd door Azure.

   Wanneer de configuratie is opgeslagen, wordt de Object-ID (OID) van de service-principal wordt vermeld als de Principal-ID, zoals hieronder weergegeven:

   ![Stream Analytics-Principal-ID](./media/stream-analytics-managed-identities-adls/stream-analytics-principal-id.png)
 
   De service-principal heeft dezelfde naam als de Stream Analytics-taak. Bijvoorbeeld, als de naam van uw taak is **MyASAJob**, de naam van de service-principal gemaakt is ook **MyASAJob**.

3. Klik op de verificatiemodus vervolgkeuzelijst en selecteer in het venster van de eigenschappen van uitvoer van de uitvoerlocatie ADLS Gen1 **beheerde identiteit (preview)**.

4. Vul de rest van de eigenschappen. Zie voor meer informatie over het maken van een ADLS-uitvoer, [de uitvoer van een Data lake Store maken met stream analytics](../data-lake-store/data-lake-store-stream-analytics.md). Wanneer u klaar bent, klikt u op **opslaan**.

   ![Azure Data Lake-opslag configureren](./media/stream-analytics-managed-identities-adls/stream-analytics-configure-adls.png)
 
5. Navigeer naar de pagina overzicht van uw ADLS-Gen1 en klik op **Data explorer**.

   ![Overzicht van Data Lake-opslag configureren](./media/stream-analytics-managed-identities-adls/stream-analytics-adls-overview.png)

6. Selecteer in het deelvenster Data explorer **toegang** en klikt u op **toevoegen** in het deelvenster toegang.

   ![Data Lake Storage Access configureren](./media/stream-analytics-managed-identities-adls/stream-analytics-adls-access.png)

7. In het tekstvak in op de **gebruiker of groep selecteren** deelvenster, typ de naam van de service-principal. Houd er rekening mee dat de naam van de service-principal ook de naam van de bijbehorende Stream Analytics-taak is. Als u te typen van de principal-naam begint, wordt deze weergegeven onder het tekstvak. Kies de gewenste service principal name en klik op **Selecteer**.

   ![Selecteer de naam van een service-principal](./media/stream-analytics-managed-identities-adls/stream-analytics-service-principal-name.png)
 
8. In de **machtigingen** deelvenster controle de **schrijven** en **Execute** machtigingen en wijs deze toe aan **deze map en alle onderliggende**. Klik vervolgens op **Ok**.

   ![Een machtiging selecteren](./media/stream-analytics-managed-identities-adls/stream-analytics-select-permissions.png)
 
9. De service-principal wordt vermeld onder **machtigingen toegewezen** op de **toegang** deelvenster zoals hieronder wordt weergegeven. U kunt nu teruggaan en uw Stream Analytics-taak starten.

   ![Toegangslijst](./media/stream-analytics-managed-identities-adls/stream-analytics-access-list.png)

   Zie voor meer informatie over Data Lake Storage Gen1 bestandssysteemmachtigingen [toegangsbeheer in Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-access-control.md).

## <a name="resource-manager-template-deployment"></a>Sjabloonimplementatie van Resource Manager

1. U kunt maken een *Microsoft.StreamAnalytics/streamingjobs* resource met een beheerde identiteit door de volgende eigenschap te nemen in de sectie resource van de Resource Manager-sjabloon:

   ```json
   "Identity": {
   "Type": "SystemAssigned",
   },
   ```

   Deze eigenschap vertelt ons Azure Resource Manager maken en beheren van de identiteit voor uw Azure Stream Analytics-taak.

   **Van voorbeeldtaak**

   ```json
   { 
   "Name": "AsaJobWithIdentity", 
   "Type": "Microsoft.StreamAnalytics/streamingjobs", 
   "Location": "West US",
   "Identity": {
     "Type": "SystemAssigned", 
     }, 
   "properties": {
      "sku": {
       "name": "standard"
       },
   "outputs": [
         {
           "name": "string",
           "properties":{
             "datasource": {        
               "type": "Microsoft.DataLake/Accounts",
               "properties": {
                 "accountName": “myDataLakeAccountName",
                 "filePathPrefix": “cluster1/logs/{date}/{time}",
                 "dateFormat": "YYYY/MM/DD",
                 "timeFormat": "HH",
                 "authenticationMode": "Msi"
                 }
                 
   }
   ```
  
   **Taak voorbeeldantwoord**

   ```json
   { 
   "Name": "mySAJob", 
   "Type": "Microsoft.StreamAnalytics/streamingjobs", 
   "Location": "West US",
   "Identity": {
   "Type": "SystemAssigned",
    "principalId": "GUID", 
    "tenantId": "GUID", 
   }, 
   "properties": {
           "sku": {
             "name": "standard"
           },
   }
   ```

   Noteer de Principal-ID uit het antwoord van de taak toegang verlenen tot de vereiste ADLS-resource.

   De **Tenant-ID** is de ID van de Azure Active Directory-tenant waar de service-principal is gemaakt. De service-principal is gemaakt in de Azure-tenant die wordt vertrouwd door het abonnement.

   De **Type** geeft het type beheerde identiteit, zoals wordt beschreven in de typen van beheerde identiteiten. Alleen de door het systeem toegewezen type wordt ondersteund.

2. Biedt toegang tot de service-principal met behulp van PowerShell. Als u wilt toegang verleent tot de service-principal via PowerShell, voert u de volgende opdracht uit:

   ```powershell
   Set-AzureRmDataLakeStoreItemAclEntry -AccountName <accountName> -Path <Path> -AceType User -Id <PrinicpalId> -Permissions <Permissions>
   ```

   De **PrincipalId** is de Object-ID van de service-principal en op het scherm wordt weergegeven wanneer de service-principal is gemaakt. Als u de taak met behulp van de sjabloonimplementatie van een Resource Manager-hebt gemaakt, wordt de Object-ID wordt weergegeven in de identiteitseigenschap van de reactie van de taak.

   **Voorbeeld**

   ```powershell
   PS > Set-AzureRmDataLakeStoreItemAclEntry -AccountName "adlsmsidemo" -Path / -AceType
   User -Id 14c6fd67-d9f5-4680-a394-cd7df1f9bacf -Permissions WriteExecute
   ```

   Raadpleeg voor meer informatie over de bovenstaande PowerShell-opdracht, de [Set AzureRmDataLakeStoreItemAclEntry](https://docs.microsoft.com/powershell/module/azurerm.datalakestore/set-azurermdatalakestoreitemaclentry?view=azurermps-6.8.1&viewFallbackFrom=azurermps-4.2.0#optional-parameters) documentatie.

## <a name="next-steps"></a>Volgende stappen

* [De uitvoer van een Data lake Store maken met stream analytics](../data-lake-store/data-lake-store-stream-analytics.md)
