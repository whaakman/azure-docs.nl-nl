---
title: Maken en publiceren van een Marketplace-item in Azure-Stack | Microsoft Docs
description: Maken en publiceren van een Marketplace-item in Azure-Stack.
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 77e5f60c-a86e-4d54-aa8d-288e9a889386
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: erikje
ms.openlocfilehash: 64203ce186665aada98fbe8daed971164a650399
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-publish-a-marketplace-item"></a>Een Marketplace-item maken en publiceren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

## <a name="create-a-marketplace-item"></a>Een Marketplace-item maken
1. [Download](http://www.aka.ms/azurestackmarketplaceitem) het hulpprogramma Azure-galerie Packager en het voorbeeld Azure Stack Marketplace-item.
2. Open het voorbeeld Marketplace-item en de naam van de **SimpleVMTemplate** map. (Gebruik bijvoorbeeld dezelfde naam als uw Marketplace-item **Contoso.TodoList**.) Deze map bevat:
   
       /Contoso.TodoList/
       /Contoso.TodoList/Manifest.json
       /Contoso.TodoList/UIDefinition.json
       /Contoso.TodoList/Icons/
       /Contoso.TodoList/Strings/
       /Contoso.TodoList/DeploymentTemplates/
3. [Een Azure Resource Manager-sjabloon maken](../azure-resource-manager/resource-group-authoring-templates.md) of kies een sjabloon in GitHub. Het Marketplace-item maakt gebruik van deze sjabloon om een resource te maken.
4. Testen om er zeker van te zijn dat de resource met succes kan worden geïmplementeerd, de sjabloon met de Microsoft Azure-Stack-API's.
5. Als uw sjabloon is afhankelijk van de installatiekopie van een virtuele machine, volgt u de instructies voor het [de installatiekopie van een virtuele machine toevoegen aan Azure-Stack](azure-stack-add-vm-image.md).
6. Opslaan van uw Azure Resource Manager-sjabloon in de **/Contoso.TodoList/DeploymentTemplates/** map.
7. Kies de pictogrammen en tekst voor de Marketplace-item. Toevoegen van pictogrammen op de **pictogrammen** map, en tekst toevoegen aan de **resources** bestand de **tekenreeksen** map. De naamconventie klein, normaal, groot en Wide voor pictogrammen gebruiken. Zie [Marketplace-item UI-naslag](#reference-marketplace-item-ui) voor een gedetailleerde beschrijving.
   
   > [!NOTE]
   > Alle vier pictogram grootten (klein, normaal, groot is, breed) zijn vereist voor het bouwen van de Marketplace-item correct.
   > 
   > 
8. In de **manifest.json** bestand, wijzig **naam** op de naam van uw Marketplace-item. Ook wijzigen **publisher** aan uw naam of het bedrijf.
9. Onder **artefacten**, wijzigen **naam** en **pad** naar de juiste gegevens voor de Azure Resource Manager-sjabloon die u hebt opgenomen.
   
         "artifacts": [
            {
                "name": "Type your template name",
                "type": "Template",
                "path": "DeploymentTemplates\\Type your path",
                "isDefault": true
            }
10. Vervang **mijn Marketplace-Items** met een lijst van de categorieën waar uw Marketplace-item moet worden weergegeven.
    
             "categories":[
                 "My Marketplace Items"
              ],
11. Raadpleeg voor verdere wijzigingen in manifest.json, [verwijzing: Marketplace-item manifest.json](#reference-marketplace-item-manifestjson).
12. Als u wilt de mappen in een bestand .azpkg inpakken, open een opdrachtprompt en voer de volgende opdracht:
    
        AzureGalleryPackager.exe package –m <path to manifest.json> -o <output location for the package>
    
    > [!NOTE]
    > Het volledige pad naar het pakket uitvoer moet bestaan. Bijvoorbeeld, als het uitvoerpad C:\MarketPlaceItem\yourpackage.azpkg is, moet de map C:\MarketPlaceItem bestaan.
    > 
    > 

## <a name="publish-a-marketplace-item"></a>Een Marketplace-item publiceren
1. Gebruik PowerShell of Azure Storage Explorer uw Marketplace-item (.azpkg) uploaden naar Azure Blob-opslag. U kunt uploaden naar de lokale opslag van de Azure-Stack of uploaden naar Azure Storage. (Dit is een tijdelijke locatie voor het pakket.) Zorg ervoor dat de blob openbaar toegankelijk is.
2. Controleer of uw PowerShell-sessie met uw servicebeheerdersreferenties is ingesteld op de client virtuele machine in de Stack van Microsoft Azure-omgeving. U vindt instructies voor het verifiëren van PowerShell in Azure-Stack in [implementeren van een sjabloon met PowerShell](user/azure-stack-deploy-template-powershell.md).
3. Gebruik de **toevoegen AzureRMGalleryItem** PowerShell-cmdlet in de Marketplace-item publiceren naar Azure-Stack. Bijvoorbeeld:
   
       Add-AzureRMGalleryItem -GalleryItemUri `
       https://sample.blob.core.windows.net/gallerypackages/Microsoft.SimpleTemplate.1.0.0.azpkg –Verbose
   
   | Parameter | Beschrijving |
   | --- | --- |
   | Abonnements-id |Beheerder abonnements-ID. U kunt deze ophalen met behulp van PowerShell. Als u liever krijgen in de portal, gaat u naar het provider-abonnement en kopieer de abonnement-ID. |
   | GalleryItemUri |BLOB-URI voor uw gallery-pakket dat al is geüpload naar de opslag. |
   | Apiversion |Instellen als **2015-04-01**. |
4. Ga naar de portal. U ziet nu de Marketplace-item in de portal--als een operator of als een gebruiker.
   
   > [!NOTE]
   > Het pakket kan enige tijd duren om te worden weergegeven.
   > 
   > 
5. Uw Marketplace-item is nu opgeslagen in de Stack Azure Marketplace. U kunt verwijderen uit de Blob-opslaglocatie.
6. U kunt een Marketplace-item verwijderen met behulp van de **verwijderen AzureRMGalleryItem** cmdlet. Voorbeeld:
   
        Remove-AzureRMGalleryItem -Name Microsoft.SimpleTemplate.1.0.0  –Verbose
   
   > [!NOTE]
   > De Marketplace-gebruikersinterface kan een fout worden weergegeven nadat u een item verwijderen. De fout te verhelpen, klikt u op **instellingen** in de portal. Selecteer **wijzigingen negeren** onder **Portal aanpassing**.
   > 
   > 

## <a name="reference-marketplace-item-manifestjson"></a>Naslaginformatie: Marketplace-item manifest.json
### <a name="identity-information"></a>Gegevens van identiteit
| Naam | Vereist | Type | Beperkingen | Beschrijving |
| --- | --- | --- | --- | --- |
| Naam |X |Tekenreeks |[A-Za-z0-9] + | |
| Uitgever |X |Tekenreeks |[A-Za-z0-9] + | |
| Versie |X |Tekenreeks |[SemVer v2](http://semver.org/) | |

### <a name="metadata"></a>Metagegevens
| Naam | Vereist | Type | Beperkingen | Beschrijving |
| --- | --- | --- | --- | --- |
| Weergavenaam |X |Tekenreeks |Aanbeveling 80 tekens |De portal mogelijk de itemnaam van uw niet probleemloos weergegeven als het is langer dan 80 tekens. |
| PublisherDisplayName |X |Tekenreeks |Aanbeveling 30 tekens |De portal mogelijk de naam van de uitgever niet probleemloos weergegeven als het is langer dan 30 tekens. |
| PublisherLegalName |X |Tekenreeks |Maximaal 256 tekens | |
| Samenvatting |X |Tekenreeks |60 en 100 tekens | |
| LongSummary |X |Tekenreeks |140 tot 256 tekens |Niet nog van toepassing in Azure-Stack. |
| Beschrijving |X |[HTML](https://auxdocs.azurewebsites.net/en-us/documentation/articles/gallery-metadata#html-sanitization) |500 tot en met 5000 tekens | |

### <a name="images"></a>Installatiekopieën
De Marketplace maakt gebruik van de volgende pictogrammen:

| Naam | Breedte | Hoogte | Opmerkingen |
| --- | --- | --- | --- |
| Breed |255 px |115 px |Altijd vereist |
| Groot |115 px |115 px |Altijd vereist |
| Middelgroot |90 px |90 px |Altijd vereist |
| Klein |40 px |40 px |Altijd vereist |
| schermopname |533 px |32 px |Optioneel |

### <a name="categories"></a>Categorieën
Elke Marketplace-item moet worden gemarkeerd met een categorie die aangeeft waar het item wordt weergegeven in de gebruikersinterface van de portal. U kunt een van de bestaande categorieën in Azure-Stack (Compute, gegevens en opslag, enz.) of een nieuw wachtwoord kiezen.

### <a name="links"></a>Koppelingen
Elk item Marketplace kan verschillende koppelingen naar aanvullende inhoud bevatten. De koppelingen zijn opgegeven als een lijst met namen en URI's.

| Naam | Vereist | Type | Beperkingen | Beschrijving |
| --- | --- | --- | --- | --- |
| Weergavenaam |X |Tekenreeks |Maximaal 64 tekens | |
| URI |X |URI | | |

### <a name="additional-properties"></a>Aanvullende eigenschappen
Naast de voorgaande metagegevens kunnen Marketplace schrijvers aangepaste sleutel/waarde-paar gegevens in de volgende notatie:

| Naam | Vereist | Type | Beperkingen | Beschrijving |
| --- | --- | --- | --- | --- |
| Weergavenaam |X |Tekenreeks |Maximaal 25 tekens | |
| Waarde |X |Tekenreeks |Maximaal 30 tekens | |

### <a name="html-sanitization"></a>HTML-opschoning
Voor elk veld waarmee HTML, mogen de volgende elementen en kenmerken:

H1, h2, h3, h4, h5, p, ol, ul, li, een [target | href], br, sterk, em, b, ik

## <a name="reference-marketplace-item-ui"></a>Naslaginformatie: Marketplace-item gebruikersinterface
Pictogrammen en tekst voor Marketplace-items zoals te zien is in de Stack van Azure-portal zijn.

### <a name="create-blade"></a>Blade maken
![Blade maken](media/azure-stack-marketplace-item-ui-reference/image1.png)

### <a name="marketplace-item-details-blade"></a>Blade toewijzingdetails van Marketplace-item
![Blade toewijzingdetails van Marketplace-item](media/azure-stack-marketplace-item-ui-reference/image3.png)

