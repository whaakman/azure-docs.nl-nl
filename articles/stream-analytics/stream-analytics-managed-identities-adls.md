---
title: Azure Stream Analytics-taak aan Azure Data Lake Storage Gen1 uitvoer verifiëren
description: In dit artikel wordt beschreven hoe u beheerde identiteiten gebruiken voor het verifiëren van uw Azure Stream Analytics-taak aan Azure Data Lake Storage Gen1 uitvoer.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/18/2019
ms.custom: seodec18
ms.openlocfilehash: 4537d15f88732d4b0c3c3cf514d6b8528af10f81
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56737464"
---
# <a name="authenticate-stream-analytics-to-azure-data-lake-storage-gen1-using-managed-identities-preview"></a>Verifiëren van Stream Analytics voor Azure Data Lake Storage Gen1 met behulp van beheerde identiteiten (Preview)

Azure Stream Analytics biedt ondersteuning voor verificatie met Azure Data Lake Storage (ADLS) Gen1 uitvoer beheerde identiteit. De identiteit is een beheerde toepassing in Azure Active Directory die staat voor een bepaalde Stream Analytics-taak geregistreerd en kan worden gebruikt om een specifieke resource te verifiëren. Beheerde identiteiten elimineert u de beperkingen van methoden voor verificatie op basis van een gebruiker, zoals u dat u hoeft te verifiëren vanwege wachtwoord te wijzigen of token verlopen voor voor een gebruiker die zich na negentig dagen voordoen. Daarnaast beheerde identiteiten helpen bij de automatisering van Stream Analytics-taak implementaties die worden uitgevoerd naar Azure Data Lake Storage Gen1.

Ga naar de [acht nieuwe functies in Azure Stream Analytics](https://azure.microsoft.com/blog/eight-new-features-in-azure-stream-analytics/) blogbericht om u te registreren voor deze Preview-versie en lees meer over nieuwe functies.

In dit artikel ziet u drie manieren om in te schakelen beheerde identiteit voor een Azure Stream Analytics-taak die op een Azure Data Lake Storage Gen1 via de Azure portal, de sjabloonimplementatie van Azure Resource Manager-en de Azure Stream Analytics-hulpprogramma's voor Visual Studio uitvoert.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-portal"></a>Azure Portal

1. Begin met het maken van een nieuwe Stream Analytics-taak of door het openen van een bestaande taak in Azure portal. Selecteer in de menubalk aan de linkerkant van het scherm bevindt, **beheerde identiteit (preview)** bevindt zich onder **configureren**.

   ![Configureren van de Preview-versie van Stream Analytics beheerde identiteit](./media/stream-analytics-managed-identities-adls/stream-analytics-managed-identity-preview.png)

2. Selecteer **gebruik door het systeem toegewezen beheerde identiteit (preview)** in het venster dat wordt weergegeven aan de rechterkant. Klik op **opslaan** aan een service-principal voor de identiteit van de Stream Analytics-taak in Azure Active Directory. De levenscyclus van de identiteit van de zojuist gemaakte worden beheerd door Azure. Wanneer de Stream Analytics-taak wordt verwijderd, wordt de identiteit van de bijbehorende (dat wil zeggen, de service-principal) wordt automatisch verwijderd door Azure.

   Wanneer de configuratie is opgeslagen, wordt de Object-ID (OID) van de service-principal wordt vermeld als de Principal-ID, zoals hieronder weergegeven:

   ![Stream Analytics-service principal-ID](./media/stream-analytics-managed-identities-adls/stream-analytics-principal-id.png)
 
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

   ![Selecteer schrijven en uitvoeren van machtigingen](./media/stream-analytics-managed-identities-adls/stream-analytics-select-permissions.png)
 
9. De service-principal wordt vermeld onder **machtigingen toegewezen** op de **toegang** deelvenster zoals hieronder wordt weergegeven. U kunt nu teruggaan en uw Stream Analytics-taak starten.

   ![Stream Analytics toegang krijgen tot de lijst in de portal](./media/stream-analytics-managed-identities-adls/stream-analytics-access-list.png)

   Zie voor meer informatie over Data Lake Storage Gen1 bestandssysteemmachtigingen [toegangsbeheer in Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-access-control.md).

## <a name="stream-analytics-tools-for-visual-studio"></a>Stream Analytics-hulpprogramma's voor Visual Studio

1. Stel in JobConfig.json, **gebruik door het systeem toegewezen identiteit** te **waar**.

   ![Stream Analytics-taakconfiguratie beheerde identiteiten](./media/stream-analytics-managed-identities-adls/adls-mi-jobconfig-vs.png)

2. Klik op de verificatiemodus vervolgkeuzelijst en selecteer in het venster van de eigenschappen van uitvoer van de uitvoerlocatie ADLS Gen1 **beheerde identiteit (preview)**.

   ![ADLS uitvoer beheerde identiteiten](./media/stream-analytics-managed-identities-adls/adls-mi-output-vs.png)

3. Vul de rest van de eigenschappen, en klikt u op **opslaan**.

4. Klik op **verzenden naar Azure** in de query-editor.

   Wanneer u de taak verzendt, wordt met de hulpprogramma's voor twee dingen doen:

   * Maakt automatisch een service principal voor de identiteit van de Stream Analytics-taak in Azure Active Directory. De levenscyclus van de identiteit van de zojuist gemaakte worden beheerd door Azure. Wanneer de Stream Analytics-taak wordt verwijderd, wordt de identiteit van de bijbehorende (dat wil zeggen, de service-principal) wordt automatisch verwijderd door Azure.

   * Automatisch ingesteld **schrijven** en **Execute** machtigingen voor het ADLS-Gen1 voorvoegsel van pad dat wordt gebruikt in de taak en deze toewijzen aan deze map en alle onderliggende objecten.

5. U kunt de Resource Manager-sjablonen met de volgende eigenschap via genereren [Stream Analytics-CI. CD Nuget-pakket](https://www.nuget.org/packages/Microsoft.Azure.StreamAnalytics.CICD/) versie 1.5.0 of hoger op een machine build (buiten Visual Studio). Volg de Resource Manager sjabloon in de volgende sectie implementatiestappen voor het ophalen van de service principal en Verleen toegang tot de service-principal via PowerShell.

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
   Set-AzDataLakeStoreItemAclEntry -AccountName <accountName> -Path <Path> -AceType User -Id <PrinicpalId> -Permissions <Permissions>
   ```

   De **PrincipalId** is de Object-ID van de service-principal en op het scherm wordt weergegeven wanneer de service-principal is gemaakt. Als u de taak met behulp van de sjabloonimplementatie van een Resource Manager-hebt gemaakt, wordt de Object-ID wordt weergegeven in de identiteitseigenschap van de reactie van de taak.

   **Voorbeeld**

   ```powershell
   PS > Set-AzDataLakeStoreItemAclEntry -AccountName "adlsmsidemo" -Path / -AceType
   User -Id 14c6fd67-d9f5-4680-a394-cd7df1f9bacf -Permissions WriteExecute
   ```

   Raadpleeg voor meer informatie over de bovenstaande PowerShell-opdracht, de [Set AzDataLakeStoreItemAclEntry](https://docs.microsoft.com/powershell/module/az.datalakestore/set-azdatalakestoreitemaclentry#optional-parameters) documentatie.

## <a name="limitations"></a>Beperkingen
Deze functie biedt geen ondersteuning voor het volgende:

1.  **Toegang voor meerdere tenants**: De Service-principal gemaakt voor een bepaalde Stream Analytics-taak wordt opgeslagen op de Azure Active Directory-tenant waarvoor de taak is gemaakt en kan niet worden gebruikt voor een resource die zich op een andere Azure Active Directory-tenant bevinden. U kunt daarom alleen MSI gebruiken voor ADLS Gen 1-resources die zich binnen dezelfde Azure Active Directory-tenant als uw Azure Stream Analytics-taak. 

2.  **[Gebruiker toegewezen identiteit](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka/)**: wordt niet ondersteund dit betekent dat de gebruiker is niet de eigen service-principal moet worden gebruikt door de Stream Analytics-taak invoeren. De service-principal wordt gegenereerd door Azure Stream Analytics. 


## <a name="next-steps"></a>Volgende stappen

* [De uitvoer van een Data lake Store maken met stream analytics](../data-lake-store/data-lake-store-stream-analytics.md)
* [Stream Analytics-query's met Visual Studio lokaal testen](stream-analytics-vs-tools-local-run.md)
* [Testen van live gegevens lokaal met behulp van Azure Stream Analytics-hulpprogramma's voor Visual Studio](stream-analytics-live-data-local-testing.md) 
