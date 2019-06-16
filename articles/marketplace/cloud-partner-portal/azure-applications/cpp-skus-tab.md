---
title: SKU's configureren voor een aanbieding van Azure-toepassing | Azure Marketplace
description: Over het configureren van de SKU's voor een Azure beheerde toepassingen en de sjabloon van een Azure-oplossing.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: pabutler
ms.openlocfilehash: ef4ea2419c64d0376023ea5d291460df48a51c63
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64943401"
---
# <a name="azure-application-skus-tab"></a>Azure-toepassing-SKU's tabblad

In dit artikel wordt beschreven hoe u het tabblad van de SKU's gebruiken om te maken van SKU's voor uw Azure-toepassing. 

> [!IMPORTANT]
> De stappen voor het configureren van een SKU zijn verschillend voor de aanbieding van een beheerde toepassing en een aanbieding van de sjabloon oplossing. Deze verschillen worden in dit artikel beschreven. 

## <a name="configure-azure-application-skus"></a>Azure-toepassing-SKU's configureren

### <a name="create-a-new-sku"></a>Maak een nieuwe Voorraadeenheid

Volg deze stappen om te maken van een nieuwe SKU:

1. Selecteer de **SKU's** tabblad.
2. Selecteer onder SKU's, **+ nieuwe Voorraadeenheid**.

    ![Nieuwe SKU-prompt](./media/azureapp-plus-sku.png)

3. Typ in het pop-upvenster van nieuwe Voorraadeenheid een **SKU-ID**. Deze id is beperkt tot 50 tekens bevatten en moet bestaan alleen uit kleine letters, alfanumerieke tekens, streepjes of onderstrepingstekens bevatten. De SKU-ID mag niet eindigen op een streepje.
4. De SKU-ID is zichtbaar voor klanten in URL's op product: Resource Manager-sjablonen (indien van toepassing) en facturering rapporten. U kunt deze id niet wijzigen nadat de aanbieding de gepubliceerd.

### <a name="sku-details-for-a-solution-template"></a>SKU-Details voor een oplossingssjabloon

De volgende schermopname ziet u het formulier SKU-Details voor een oplossingssjabloon.

![Formulier voor SKU-details voor de sjabloon-oplossing](./media/azureapp-sku-details-solutiontemplate.png)

Geef de volgende SKU-waarden.  De velden die zijn toegevoegd met een sterretje zijn vereist.

|    Veld         |       Description                                                            |
|  ---------       |     ---------------                                                          |
|  **Titel\***     | Een titel voor de SKU. Deze titel wordt weergegeven in de galerie voor dit item.   |
| **Samenvatting\***    | Een korte beknopte beschrijving van de SKU. (Maximale lengte is 100 tekens.)  |
| **Beschrijving\*** | Een gedetailleerde beschrijving van de SKU. Eenvoudige HTML wordt ondersteund.                 | 
| **SKU-Type\***   | Type Azure-toepassing-oplossing, selecteer ***oplossingssjabloon** voor dit scenario. |
| **Beschikbaarheid van de cloudservice\*** | De locatie van de SKU. De standaardwaarde is **openbare Azure**.  <b/>   **Openbare Azure** -App worden geïmplementeerd voor klanten in alle openbare Azure-regio's die geïntegreerd met marketplace is.  <b/>   **Azure Government-Cloud** -App wordt geïmplementeerd in de Azure Government-Cloud. Voordat u publiceren naar [Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners), raadt Microsoft aan uitgevers testen en valideren van de oplossing werkt zoals verwacht in deze omgeving. Verzoeken om te zetten en te testen, een [proefaccount](https://azure.microsoft.com/offers/ms-azr-usgov-0044p/).  |
| **Is dit een persoonlijke SKU?\*** | Selecteer **Ja** als deze SKU alleen beschikbaar voor een selecte groep van klanten is. |
|   |   |

  > [!NOTE] 
  > Microsoft Azure Government is een overheidscommunitycloud met beperkte toegang voor klanten van de Amerikaanse federale overheid, status, lokale of Tribale en van in aanmerking voor het beheer van deze entiteiten partners.


### <a name="sku-details-for-managed-application"></a>SKU-Details voor de beheerde toepassing

De volgende schermopname ziet u het formulier SKU-Details voor een beheerde toepassing.

   ![Formulier voor SKU-details voor de beheerde toepassing](./media/azureapp-sku-details-managedapplication.png)

Configureer de volgende instellingen voor de SKU. De velden die zijn toegevoegd met een sterretje zijn vereist.

|    Veld         |       Description                                                            |
|  ---------       |     ---------------                                                          |
|  **Titel\***     | Een titel voor de SKU. Deze titel wordt weergegeven in de galerie voor dit item.   |
| **Samenvatting\***    | Een korte beknopte beschrijving van de SKU. (Maximale lengte is 100 tekens.)  |
| **Beschrijving\*** | Een gedetailleerde beschrijving van de SKU. Eenvoudige HTML wordt ondersteund.                 | 
| **SKU-Type\***   | Type Azure-toepassing-oplossing, selecteer ***beheerde toepassingen** voor dit scenario. 
| **Beschikbaarheid van de cloudservice\*** | De locatie van de SKU. De standaardwaarde is **openbare Azure**.  <b/>   **Openbare Azure** -App worden geïmplementeerd voor klanten in alle openbare Azure-regio's die geïntegreerd met marketplace is.  <b/>   **Azure Government-Cloud** -App wordt geïmplementeerd in de Azure Government-Cloud. Voordat u publiceren naar [Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners), raadt Microsoft aan uitgevers testen en valideren van de oplossing werkt zoals verwacht in deze omgeving. Verzoeken om te zetten en te testen, een [proefaccount](https://azure.microsoft.com/offers/ms-azr-usgov-0044p/).   Microsoft Azure Government is een overheidscommunitycloud met beperkte toegang voor klanten van de Amerikaanse federale overheid, status, lokale of Tribale en van in aanmerking voor het beheer van deze entiteiten partners. |
| **Is dit een persoonlijke SKU?\*** | Selecteer **Ja** als deze SKU alleen beschikbaar voor een selecte groep van klanten is. |
| **Beschikbaarheid van het land/regio\*** | Gebruik **regio's selecteren** om weer te geven van de lijst met landen/regio's die beschikbaar zijn. Controleer elk land/regio, en selecteer vervolgens **OK** om op te slaan, uw uitgelicht.  <b/>   ![Lijst met beschikbaarheid land en regio](./media/azure-app-select-country-region.png)  |
| **Oude prijzen\*** | De prijs voor de SKU, in USD per maand. Prijzen zijn ingesteld in de lokale valuta met behulp van de huidige wisselkoersen van configuratie. Controleer deze omdat u eigenaar zijn van deze instellingen. Instellen of de prijs van elk land/regio afzonderlijk weergeven, de prijzen spreadsheet exporteren en importeren met aangepaste prijzen.  U moet uw prijswijzigingen om in te schakelen exporteren/importeren van prijzen gegevens opslaan.  |
| **Vereenvoudigde valuta prijzen\*** | De prijs voor de SKU, in USD per maand. Dit moet hetzelfde zijn als de oude prijzen. Zie voor meer informatie, [vereenvoudigd valuta prijzen](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-update-existing-offer). |
|  |  |


### <a name="package-details-for-solution-template"></a>Pakketgegevens voor oplossingssjabloon

   ![Pakketgegevens voor oplossingssjabloon](./media/azureapp-sku-pkgdetails-solutiontemplate.png)

Geeft u de volgende **pakketgegevens** waarden.  De velden die zijn toegevoegd met een sterretje zijn vereist.

- **Versie\***  -de versie van het pakket dat u zult uploaden. Versie tags moet van het formulier X.Y.Z, waarbij X, Y en Z gehele getallen zijn.
- **Pakketbestand (.zip)\***  -dit pakket bevat de volgende bestanden, opgeslagen in een ZIP-bestand.
  - MainTemplate.json - de implementatie-sjabloonbestand dat wordt gebruikt voor het implementeren van de oplossing/toepassing en de resources die zijn gedefinieerd voor de oplossing te maken. Zie voor meer informatie, [over het ontwerpen van implementatie-sjabloonbestanden](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template).
  - createUIDefinition.json - dit bestand wordt gebruikt door de Azure-portal voor het genereren van de gebruikersinterface voor het inrichten van deze oplossing/toepassing. Zie voor meer informatie, [maken-Azure portal gebruikersinterface voor uw beheerde toepassing](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview).

  >[!IMPORTANT] 
  >Dit pakket bevat geen geneste sjablonen of scripts die nodig zijn voor het inrichten van deze toepassing. Het bestand MainTemplate.json en createUIDefinition.json bestand moeten zich in de hoofdmap.


### <a name="package-details-for-managed-application"></a>Pakketdetails van het voor de beheerde toepassing

   ![Pakketdetails van het voor de beheerde toepassing](./media/azureapp-sku-pkgdetails-managedapplication.png)

Geef de volgende Details van pakket.  De velden die zijn toegevoegd met een sterretje zijn vereist.

- **Versie\***  -de versie van het pakket dat u zult uploaden. Versie tags moet van het formulier X.Y.Z, waarbij X, Y en Z gehele getallen zijn.
- **Pakketbestand (.zip)\***  -dit pakket bevat de volgende bestanden, opgeslagen in een ZIP-bestand.
  - applianceMainTemplate.json - de implementatie-sjabloonbestand dat wordt gebruikt voor het implementeren van de oplossing/toepassing en maak de resources die zijn gedefinieerd. Zie voor meer informatie [Snelstart: Azure Resource Manager-sjablonen maken en implementeren via Azure Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal). 
  - applianceCreateUIDefinition.json - dit bestand wordt gebruikt door de Azure-portal voor het genereren van de gebruikersinterface voor het inrichten van deze oplossing/toepassing. Zie voor meer informatie, [maken-Azure portal gebruikersinterface voor uw beheerde toepassing](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview).
  - mainTemplate.json - het sjabloonbestand die alleen de bron Microsoft.Solution/appliances bevat. Zie voor meer informatie, [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates). <br>
Houd rekening met de volgende belangrijke eigenschappen van deze resource:
    - "soort" - de waarde moet "Marketplace" in het geval van Marketplace-beheerde toepassing.
    - "ManagedResourceGroupId" - de resourcegroep in het abonnement van de klant waarin alle resources die zijn gedefinieerd in de applianceMainTemplate.json worden geïmplementeerd.
    - "PublisherPackageId": de tekenreeks die een unieke identificatie van het pakket. Deze waarde moet als volgt samengesteld: Er is een samenvoeging van [publisherId]. [OfferId]-[SKUID] preview. [PackageVersion].

  >[!IMPORTANT] 
  >Dit pakket bevat geen geneste sjablonen of scripts die nodig zijn voor het inrichten van deze toepassing. Deze bestanden moeten zich in de hoofdmap:  MainTemplate.json, applianceMainTemplate.json en applianceCreateUIDefinition.json.

- **Tenant-Id\***  -de Azure Active Directory-tenant-id van uw organisatie.
- **Toegang tot JIT inschakelen? \***  – Selecteer **Ja** om in te schakelen van Just-In-Time-beheertoegang voor implementaties van klanten met behulp van deze aanbieding.

  >[!NOTE] 
  >Als u JIT inschakelen, moet u het bestand CreateUiDefinition.json ter ondersteuning van toegang tot JIT bijwerken.

U moet voor een beheerde toepassing autorisatie en de beleidsinstellingen configureren.


#### <a name="authorization"></a>Autorisatie

De Azure Active Directory-id van gebruiker, groep of toepassing die u wilt de machtiging verlenen aan de beheerde resourcegroep toevoegen. De machtiging die wordt verleend wordt aangegeven door de roldefinitie-id. Een eigenaar, bijdrager of een aangepaste rol kan het zijn.


#### <a name="policy-settings"></a>Beleidsinstellingen

Voeg de beleidsregels die voldoen aan de beheerde App. Lees de informatie over het beleid voor Azure-Resource [wat is Azure Policy?](../../../governance/policy/overview.md)

   ![Instellingen voor autorisatie en beleid voor een beheerde toepassing](./media/azureapp-sku-details-managedapp-auth-policy.png)

**Maakt een nieuwe autorisatie:**

1. Onder **autorisatie**, selecteer **+ nieuwe autorisatie**.
2. Voor **Principal-Id**, typt u de Azure Active Directory-id van gebruiker, groep of toepassing die u wilt de machtiging verlenen aan de beheerde resourcegroep. De machtiging die wordt verleend wordt door de definitie van de rol aangegeven.
3. Voor **roldefinitie**, selecteer een van deze opties in de vervolgkeuzelijst:  Eigenaar of Bijdrager. Zie voor meer informatie, [ingebouwde rollen voor Azure-resources](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

>[!NOTE] 
>Meerdere autorisaties kunnen worden toegevoegd. Maar is het raadzaam een Active Directory-gebruikersgroep maken en geef de ID in de 'PrincipalId." Hiermee schakelt u het toevoegen van meer gebruikers aan de groep zonder te hoeven bijwerken van de SKU.

**Een nieuw beleid maken:**

1. Onder **beleidsinstellingen**, selecteer **+ nieuw beleid**.
2. Voor **beleidsnaam**, voer een naam in voor het beleid. De maximale lengte van de naam is 50 tekens.
3. Voor **beleid**, selecteer een van de opties in de vervolgkeuzelijst. Kies het beleid dat de gegevensprovider wil worden ingeschakeld wanneer de toepassing gebruikmaakt van de gegevens. Zie voor meer informatie de [Azure Policy Samples](https://docs.microsoft.com/azure/governance/policy/samples/index).

    ![Beleidsinstellingen voor een beheerde toepassing](./media/azureapp-sku-policy-settings.png)

4. Voor **beleids-SKU**, gratis of standaard selecteren als de SKU-type van het beleid. De standaard-SKU is vereist voor controlebeleid.


## <a name="next-steps"></a>Volgende stappen

U wordt verder uw aanbieding beschrijven en leveren marketing activa in de [Marketplace tabblad](./cpp-marketplace-tab.md). 
