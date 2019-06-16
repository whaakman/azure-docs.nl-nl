---
title: Versleuteling-at-rest met behulp van de klant beheerde sleutels in Azure Key Vault (preview) - Azure Search
description: Aanvulling op de server-side-codering via indexen en synoniementoewijzingen in Azure Search via sleutels die u maakt en beheert in Azure Key Vault.
author: NatiNimni
manager: jlembicz
ms.author: natinimn
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.custom: ''
ms.openlocfilehash: 567f32cba76aaf2d1657b2476c4d11596d44dec5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66753933"
---
# <a name="azure-search-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Azure Search-versleuteling door de klant beheerde sleutels in Azure Key Vault

> [!Note]
> Versleuteling met de klant beheerde sleutels is in preview en niet bedoeld voor gebruik in productieomgevingen. De [2019 in de REST-API-versie-05-06-Preview](search-api-preview.md) biedt deze functie. U kunt ook de .NET SDK-versie 8.0-Preview-versie gebruiken.
>
> Deze functie is niet beschikbaar voor gratis services. U moet een factureerbare zoekservice gemaakt op of na 2019-01-01. Er is geen portal ondersteuning op dit moment.

Standaard-Azure Search versleutelt gebruikersinhoud at-rest met [service beheerde sleutels](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest#data-encryption-models). U kunt de standaard codering aanvullen met een extra versleutelingslaag met sleutels die u maakt en beheert in Azure Key Vault. In dit artikel leidt u door de stappen.

Server-side-versleuteling wordt ondersteund door de integratie met [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview). U kunt uw eigen versleutelingssleutels maken en op te slaan in een key vault, of u kunt Azure Key Vault-API's gebruiken voor het genereren van sleutels voor gegevenscodering. U kunt ook sleutelgebruik met Azure Key Vault controleren. 

Versleuteling met de klant beheerde sleutels is geconfigureerd op de index of synoniem kaart niveau wanneer deze objecten worden gemaakt en niet op het niveau van de search-service. U kunt inhoud die al bestaat niet coderen. 

U kunt verschillende sleutels uit verschillende sleutelkluizen gebruiken. Dit betekent dat een service voor zoeken op één host kan fungeren voor meerdere versleutelde indexes\synonym maps, elk versleuteld mogelijk met behulp van een andere door de klant beheerde sleutel, samen met indexes\synonym-kaarten die niet zijn versleuteld met behulp van de klant beheerde sleutels. 

## <a name="prerequisites"></a>Vereisten

De volgende services worden gebruikt in dit voorbeeld. 

+ [Maak een Azure Search-service](search-create-service-portal.md) of [vinden van een bestaande service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in uw huidige abonnement. U kunt een gratis service voor deze zelfstudie gebruiken.

+ [Maak een Azure Key Vault-resource](https://docs.microsoft.com/azure/key-vault/quick-create-portal#create-a-vault) of een bestaande kluis onder uw abonnement niet vinden.

+ [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) of [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) wordt gebruikt voor configuratietaken.

+ [Postman](search-fiddler.md), [Azure PowerShell](search-create-index-rest-api.md) en [Azure Search SDK](https://aka.ms/search-sdk-preview) kan worden gebruikt voor het aanroepen van de preview-REST-API. Er is geen portal of de .NET SDK-ondersteuning voor versleuteling door de klant beheerde op dit moment.

## <a name="1---enable-key-recovery"></a>1 - sleutelherstel inschakelen

Deze stap is optioneel maar ten zeerste aanbevolen. Na het maken van de Azure Key Vault-resource, inschakelen **voorlopig verwijderen** en **opschonen Protection** in de geselecteerde Key vault door het uitvoeren van de volgende PowerShell of Azure CLI-opdrachten:   

```powershell
$resource = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName "<vault_name>").ResourceId

$resource.Properties | Add-Member -MemberType NoteProperty -Name "enableSoftDelete" -Value 'true'

$resource.Properties | Add-Member -MemberType NoteProperty -Name "enablePurgeProtection" -Value 'true'

Set-AzResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

```azurecli-interactive
az keyvault update -n <vault_name> -g <resource_group> --enable-soft-delete --enable-purge-protection
```

>[!Note]
> Vanwege de aard van de versleuteling met de functie van de klant beheerde sleutels, is Azure Search niet mogelijk voor het ophalen van uw gegevens, als uw Azure Key vault-sleutel wordt verwijderd. Om te voorkomen van gegevensverlies door onbedoelde verwijderingen voor Key Vault-sleutel, is het raadzaam dat u voorlopig verwijderen en beveiliging verwijderen voor de geselecteerde sleutelkluis inschakelen. Zie voor meer informatie, [Azure Key Vault-functie voor voorlopig verwijderen](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete).   

## <a name="2---create-a-new-key"></a>2 - een nieuwe sleutel maken

Als u van een bestaande sleutel gebruikmaakt voor het versleutelen van Azure Search-inhoud, moet u deze stap overslaan.

1. [Aanmelden bij Azure portal](https://portal.azure.com) en Ga naar de sleutelkluis-dashboard.

1. Selecteer de **sleutels** instellen op basis van het navigatiedeelvenster links en klik op **+ genereren/importeren**.

1. In de **maakt u een sleutel** deelvenster in de lijst van **opties**, kiest u de methode die u wilt gebruiken om een sleutel te maken. U kunt **genereren** een nieuwe sleutel **uploaden** een bestaande sleutel, of gebruik **back-up herstellen** om te selecteren van een back-up van een sleutel.

1. Voer een **naam** voor uw sleutel en eventueel andere eigenschappen van de sleutel selecteren.

1. Klik op de **maken** knop om de implementatie te starten.

Noteer de sleutel-id: dit is samengesteld uit de **sleutelwaarde Uri**, wordt de **sleutelnaam**, en de **sleutelversie**. U moet deze voor het definiëren van een versleutelde index in Azure Search.
 
![Maak een nieuwe key vault-sleutel](./media/search-manage-encryption-keys/create-new-key-vault-key.png "Maak een nieuwe key vault-sleutel")

## <a name="3---create-a-service-identity"></a>3 - een met het maken van een service-identiteit

Een identiteit toewijzen aan uw search-service, kunt u toegangsmachtigingen voor Key Vault om uw search-service te verlenen. Uw search-service gebruikt de identiteit te verifiëren met Azure Key vault.

Azure Search ondersteunt twee manieren voor het toewijzen van identiteit: een beheerde identiteit of een extern beheerde Azure Active Directory-toepassing. 

Gebruik zo mogelijk een beheerde identiteit. Dit is de eenvoudigste manier voor het toewijzen van een identiteit aan uw search-service en werkt in de meeste scenario's. Als u meerdere sleutels voor indexen en synoniementoewijzingen, of als uw oplossing in een gedistribueerde architectuur die vanwege de verificatie op basis van identiteit, gebruikt u de geavanceerde [extern beheerde Azure Active Directory benadering](#aad-app)dat aan het einde van dit artikel wordt beschreven.

 In het algemeen kan een beheerde identiteit uw search-service om te verifiëren naar Azure Key Vault zonder referenties opslaan in de code. De levenscyclus van dit type beheerde identiteit is gekoppeld aan de levenscyclus van uw search-service kan slechts één beheerde identiteit hebben. [Meer informatie over beheerde identiteiten](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

1. Het maken van een beheerde identiteit [toAzure portal aanmelden](https://portal.azure.com) en open het servicedashboard van uw zoekopdracht. 

1. Klik op **identiteit** de status ervan te wijzigen in het navigatiedeelvenster links **op**, en klikt u op **opslaan**.

![Inschakelen van een beheerde identiteit](./media/search-enable-msi/enable-identity-portal.png "een beheerde identiteit inschakelen")

## <a name="4---grant-key-access-permissions"></a>4 - toegang tot de sleutel machtigingen verlenen

Om in te schakelen uw search-service uw Key Vault-sleutel te gebruiken, moet u uw zoekopdracht verlenen bepaalde machtigingen access-service.

Machtigingen voor toegang kunnen op elk moment worden ingetrokken. Als ingetrokken, wordt een search-index of synoniem Serviceoverzicht die gebruikmaakt van die key vault onbruikbaar worden. Key vault-machtigingen op een later tijdstip te herstellen, herstelt u index\synonym kaart toegang. Zie voor meer informatie, [beveiligde toegang tot een key vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).

1. [Aanmelden bij Azure portal](https://portal.azure.com) en open de overzichtspagina voor uw sleutelkluis. 

1. Selecteer de **toegangsbeleid** instellen op basis van het navigatiedeelvenster links en klik op **+ nieuwe toevoegen**.

   ![Nieuwe key vault-toegangsbeleid toevoegen](./media/search-manage-encryption-keys/add-new-key-vault-access-policy.png "nieuwe key vault-toegangsbeleid toevoegen")

1. Klik op **Selecteer principal** en selecteer uw Azure Search-service. U kunt voor het zoeken op naam of de object-ID die werd weergegeven na het inschakelen van beheerde identiteit.

   ![Selecteer sleutelkluis toegang beleid principal](./media/search-manage-encryption-keys/select-key-vault-access-policy-principal.png "Selecteer sleutelkluis toegang beleid principal")

1. Klik op **sleutelmachtigingen** en selecteer *ophalen*, *sleutel uitpakken* en *sleutel verpakken en sleutel*. U kunt de *Azure Data Lake Storage of Azure Storage* sjabloon om snel te selecteren de vereiste machtigingen.

   Azure search moet ook worden toegekend aan de volgende [toegangsmachtigingen](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-operations):

   * *Ophalen* -kunt u uw search-service om op te halen van de openbare gedeelten van de sleutel in een Key Vault
   * *Sleutel inpakken* -kunt u uw search-service uw sleutel te gebruiken om de versleutelingssleutel van de interne te beveiligen
   * *Sleutel uitpakken* -kunt u uw search-service uw sleutel te gebruiken voor het uitpakken van de interne versleutelingssleutel

   ![Selecteer sleutelkluis toegang beleid sleutelmachtigingen](./media/search-manage-encryption-keys/select-key-vault-access-policy-key-permissions.png "sleutelmachtigingen van de sleutelkluis toegang-beleid selecteren")

1. Klik op **OK** en **opslaan** wijzigingen in het beleid toegang.

> [!Important]
> Versleutelde inhoud in Azure search is geconfigureerd voor het gebruik van een specifieke Azure Key Vault-sleutel met een specifieke **versie**. Als u de sleutel of de versie wijzigt, de index of synoniem kaart moet worden bijgewerkt voor het gebruik van de nieuwe key\version **voordat** verwijderen van de vorige key\version. De index niet doet, worden weergegeven of synoniem onbruikbaar worden toegewezen, op u niet de inhoud te ontsleutelen wanneer toegang tot de sleutel verbroken wordt.   

## <a name="5---encrypt-content"></a>5 - inhoud coderen

Het maken van een index of synoniem-kaart die zijn versleuteld met de klant beheerde sleutel is nog niet mogelijk met behulp van Azure portal. Azure Search REST API gebruiken om dergelijke een index of synoniem kaart te maken.

Ondersteuning voor een nieuw op het hoogste niveau worden zowel index als synoniem toegewezen **encryptionKey** eigenschap die wordt gebruikt om op te geven van de sleutel. 

Met behulp van de **key vault Uri**, **sleutelnaam** en de **sleutelversie** van uw Key vault-sleutel, maken we een **encryptionKey** definitie:

```json
{
  "encryptionKey": {
    "keyVaultUri": "https://demokeyvault.vault.azure.net",
    "keyVaultKeyName": "myEncryptionKey",
    "keyVaultKeyVersion": "eaab6a663d59439ebb95ce2fe7d5f660"
  }
}
```
> [!Note] 
> Geen van deze key vault-gegevens worden beschouwd als geheim en eenvoudig kan worden opgehaald door te bladeren naar de relevante sleutel Azure Key Vault-pagina in Azure portal.

Als u een AAD-toepassing voor verificatie van de Key Vault in plaats van een beheerde identiteit, voegt u de AAD-toepassing **toegangsreferenties** tot de versleutelingssleutel: 
```json
{
  "encryptionKey": {
    "keyVaultUri": "https://demokeyvault.vault.azure.net",
    "keyVaultKeyName": "myEncryptionKey",
    "keyVaultKeyVersion": "eaab6a663d59439ebb95ce2fe7d5f660",
    "accessCredentials": {
      "applicationId": "00000000-0000-0000-0000-000000000000",
      "applicationSecret": "myApplicationSecret"
    }
  }
}
```

## <a name="example-index-encryption"></a>Voorbeeld: Index-versleuteling
De details van het maken van een nieuwe index via de REST-API kunnen worden gevonden op [Index maken (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index)waar het enige verschil is hier de details van de versleuteling is opgeven als onderdeel van de definitie van de index: 

```json
{
 "name": "hotels",  
 "fields": [
  {"name": "HotelId", "type": "Edm.String", "key": true, "filterable": true},
  {"name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": true, "facetable": false},
  {"name": "Description", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "en.lucene"},
  {"name": "Description_fr", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
  {"name": "Category", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
  {"name": "Tags", "type": "Collection(Edm.String)", "searchable": true, "filterable": true, "sortable": false, "facetable": true},
  {"name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": true, "sortable": true, "facetable": true},
  {"name": "LastRenovationDate", "type": "Edm.DateTimeOffset", "filterable": true, "sortable": true, "facetable": true},
  {"name": "Rating", "type": "Edm.Double", "filterable": true, "sortable": true, "facetable": true},
  {"name": "Location", "type": "Edm.GeographyPoint", "filterable": true, "sortable": true},
 ],
 "encryptionKey": {
   "keyVaultUri": "https://demokeyvault.vault.azure.net",
   "keyVaultKeyName": "myEncryptionKey",
   "keyVaultKeyVersion": "eaab6a663d59439ebb95ce2fe7d5f660"
 }
}
```
U kunt nu verzenden een aanvraag voor het maken van de index en start vervolgens met behulp van de index normaal.

## <a name="example-synonym-map-encryption"></a>Voorbeeld: Synoniem kaart versleuteling

De details van het maken van een nieuwe synoniemtoewijzing via de REST-API kunnen u vinden op [Synoniemtoewijzing maken (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/create-synonym-map)waar het enige verschil is hier de details van de versleuteling is opgeven als onderdeel van de definitie van de kaart synoniem: 

```json
{   
  "name" : "synonymmap1",  
  "format" : "solr",  
  "synonyms" : "United States, United States of America, USA\n
  Washington, Wash. => WA",
  "encryptionKey": {
    "keyVaultUri": "https://demokeyvault.vault.azure.net",
    "keyVaultKeyName": "myEncryptionKey",
    "keyVaultKeyVersion": "eaab6a663d59439ebb95ce2fe7d5f660"
  }
}
```
U kunt nu verzenden het synoniem-aanvraag voor het maken van kaart en start vervolgens normaal gebruiken.

>[!Important] 
> Terwijl **encryptionKey** kan niet worden toegevoegd aan bestaande Azure Search-index of synoniem wordt toegewezen, kan deze worden bijgewerkt door het opgeven van de verschillende waarden voor het gebruik van de drie key vault-gegevens (bijvoorbeeld, de belangrijkste versie bijwerken). Bij het wijzigen van een nieuwe Key Vault-sleutel of een nieuwe sleutelversie, een Azure Search-index of synoniem kaart die gebruikmaakt van de sleutel eerst moet worden bijgewerkt voor het gebruik van de nieuwe key\version **voordat** verwijderen van de vorige key\version. De index niet doet, worden weergegeven of onbruikbaar als deze is niet mogelijk de toegang tot de inhoud eenmaal sleutel ontsleutelen synoniemtoewijzing is verbroken.   
> Key vault-machtigingen op een later tijdstip te herstellen, herstelt u toegang tot inhoud.

## <a name="aad-app"></a> Geavanceerd: Gebruik van een extern beheerde Azure Active Directory-toepassing

Wanneer een beheerde identiteit is niet mogelijk is, dat u kunt een Azure Active Directory-toepassing met een beveiligings-principal maken voor uw Azure Search-service. Een beheerde identiteit is met name niet haalbaar onder deze omstandigheden:

* U kan niet rechtstreeks uw zoekopdracht service toegangsmachtigingen verlenen voor de Key vault (bijvoorbeeld, als de search-service in een andere Active Directory-tenant dan de Azure Key Vault is).

* Een enkele search-service is vereist voor het hosten van meerdere versleutelde indexes\synonym maps, elk met een andere sleutel uit een andere Key vault, waarbij elke sleutelkluis moet gebruiken **een andere identiteit** voor verificatie. Als verschillende sleutelkluizen beheren met behulp van een andere identiteit geen vereiste is, kunt u overwegen de bovenstaande optie voor beheerde identiteit.  

Om te voldoen aan deze topologieën, zoekt Azure ondersteunt het gebruik van Azure Active Directory (AAD)-toepassingen voor verificatie tussen uw search-service en de Key Vault.    
Een AAD-toepassing maken in de portal:

1. [Een Azure Active Directory-toepassing maken](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-an-azure-active-directory-application).

1. [De sleutel van de toepassing ID en verificatiesleutel ophalen](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) zoals die vereist zijn voor het maken van een versleutelde index. U moet opgeven waarden zijn onder andere **toepassings-ID** en **verificatiesleutel**.

>[!Important]
> Wanneer u bepaalt een AAD-toepassing van verificatie gebruiken in plaats van een beheerde identiteit, houd rekening met het feit dat Azure Search is niet gemachtigd voor het beheren van uw AAD-toepassing uit uw naam en het is aan u voor het beheren van uw AAD-toepassing, zoals periodieke rotatie van de verificatiesleutel van de toepassing.
> Bij het wijzigen van een AAD-toepassing of de verificatiesleutel, een Azure Search-index of synoniem kaart die gebruikmaakt van die toepassing eerst moet worden bijgewerkt voor het gebruik van de nieuwe toepassing ID\key **voordat** verwijderen van de vorige toepassing of de autorisatie sleutel, en voor het intrekken van uw Key Vault toegang toe.
> De index niet doet, worden weergegeven of onbruikbaar als deze is niet mogelijk de toegang tot de inhoud eenmaal sleutel ontsleutelen synoniemtoewijzing is verbroken.   

## <a name="next-steps"></a>Volgende stappen

Als u niet bekend met Azure-beveiliging-architectuur bent, raadpleegt u de [documentatie over beveiliging in Azure](https://docs.microsoft.com/azure/security/), en in het bijzonder, in dit artikel:

> [!div class="nextstepaction"]
> [Gegevensversleuteling in rust](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
