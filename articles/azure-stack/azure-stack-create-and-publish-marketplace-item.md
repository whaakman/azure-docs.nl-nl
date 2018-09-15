---
title: Maken en publiceren van een Marketplace-item in Azure Stack | Microsoft Docs
description: Maken en publiceren van een Marketplace-item in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2018
ms.author: sethm
ms.reviewer: jeffgo
ms.openlocfilehash: 9e579123124615df83483e244ef11810ca590844
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2018
ms.locfileid: "45633960"
---
# <a name="create-and-publish-a-marketplace-item"></a>Een Marketplace-item maken en publiceren

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

## <a name="create-a-marketplace-item"></a>Een Marketplace-item maken
1. [Download](http://www.aka.ms/azurestackmarketplaceitem) het hulpprogramma Azure-galerie Packager en het voorbeeld van Azure Stack Marketplace-item.
2. Open het voorbeeld Marketplace-item en wijzig de naam van de **SimpleVMTemplate** map. (Gebruik dezelfde naam als uw Marketplace-item, bijvoorbeeld **Contoso.TodoList**.) Deze map bevat:
   
       /Contoso.TodoList/
       /Contoso.TodoList/Manifest.json
       /Contoso.TodoList/UIDefinition.json
       /Contoso.TodoList/Icons/
       /Contoso.TodoList/Strings/
       /Contoso.TodoList/DeploymentTemplates/
3. [Een Azure Resource Manager-sjabloon maken](../azure-resource-manager/resource-group-authoring-templates.md) of kies een sjabloon in GitHub. De Marketplace-item maakt gebruik van deze sjabloon om een resource te maken.

    > [!Note]  
    > Nooit harde code er geheimen, zoals productcodes, wachtwoord of een persoonlijke klantgegevens in de Azure Resource Manager-sjabloon. Sjabloon json-bestanden zijn toegankelijk zonder de noodzaak voor verificatie wanneer gepubliceerd in de galerie.  Alle geheimen in Store [Key Vault](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-keyvault-parameter) en het aanroepen van in de sjabloon.

4. Om ervoor te zorgen dat de resource is kan worden geïmplementeerd, test u de sjabloon met de Microsoft Azure Stack-API's.
5. Als uw sjabloon is afhankelijk van een VM-installatiekopie, volgt u de instructies voor het [installatiekopie van een virtuele machine toevoegen aan Azure Stack](azure-stack-add-vm-image.md).
6. Sla uw Azure Resource Manager-sjabloon in de **/Contoso.TodoList/DeploymentTemplates/** map.
7. Kies de pictogrammen en tekst voor uw Marketplace-item. Toevoegen van pictogrammen op de **pictogrammen** map, en tekst toevoegen aan de **resources** -bestand in de **tekenreeksen** map. Gebruik de klein, middelgroot, groot en Wide naamconventie voor pictogrammen. Zie [Marketplace-item UI-referentie voor](#reference-marketplace-item-ui) voor een gedetailleerde beschrijving.
   
   > [!NOTE]
   > Alle vier pictogram grootten (klein, normaal, groot, breed) zijn vereist voor het bouwen van de Marketplace-item goed.
   > 
   > 
8. In de **manifest.json** bestand, wijzig de **naam** op de naam van uw Marketplace-item. Ook wijzigen **publisher** aan uw naam of het bedrijf.
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
11. Raadpleeg voor alle verdere wijzigingen aan manifest.json [verwijzing: Marketplace-item manifest.json](#reference-marketplace-item-manifestjson).
12. Als u wilt de mappen in een bestand .azpkg inpakken, open een opdrachtprompt en voer de volgende opdracht uit:
    
        AzureGalleryPackager.exe package –m <path to manifest.json> -o <output location for the package>
    
    > [!NOTE]
    > Het volledige pad naar de uitvoer-pakket moet bestaan. Bijvoorbeeld, als het uitvoerpad C:\MarketPlaceItem\yourpackage.azpkg is, moet de map C:\MarketPlaceItem bestaan.
    > 
    > 

## <a name="publish-a-marketplace-item"></a>Een Marketplace-item publiceren
1. PowerShell of Azure Storage Explorer gebruiken om uw Marketplace-item (.azpkg) uploaden naar Azure Blob-opslag. U kunt uploaden naar de lokale opslag van Azure Stack of uploaden naar Azure Storage. (Dit is een tijdelijke locatie voor het pakket.) Zorg ervoor dat de blob openbaar toegankelijk is.
2. Controleer of uw PowerShell-sessie met uw beheerdersreferenties voor de service is ingesteld op de client virtuele machine in de Microsoft Azure Stack-omgeving. U vindt instructies voor het verifiëren van PowerShell in Azure Stack in [implementeren van een sjabloon met PowerShell](user/azure-stack-deploy-template-powershell.md).
3. Bij het gebruik [PowerShell 1.3.0]( azure-stack-powershell-install.md) of hoger, kunt u de **toevoegen AzsGalleryItem** PowerShell-cmdlet op de Marketplace-item publiceren naar Azure Stack. Voordat u met behulp van PowerShell 1.3.0, gebruikt u de cmdlet **toevoegen AzureRMGalleryitem** in plaats van **toevoegen AzsGalleryItem**.  Bijvoorbeeld, wanneer het gebruik van PowerShell 1.3.0 of hoger:
   
       Add-AzsGalleryItem -GalleryItemUri `
       https://sample.blob.core.windows.net/gallerypackages/Microsoft.SimpleTemplate.1.0.0.azpkg –Verbose
   
   | Parameter | Beschrijving |
   | --- | --- |
   | abonnements-id |Beheerder abonnements-ID. U kunt deze ophalen met behulp van PowerShell. Als u liever zodat deze in de portal, gaat u naar het providerabonnement en kopieer de abonnements-ID. |
   | GalleryItemUri |BLOB-URI voor de galerijpakket dat al is geüpload naar de opslag. |
   | Apiversion |Instellen als **2015-04-01**. |
4. Ga naar de portal. U ziet de Marketplace-item in de portal--nu als een operator of als een gebruiker.
   
   > [!NOTE]
   > Het pakket kan worden weergegeven in enkele minuten duren.
   > 
   > 
5. Nu is uw Marketplace-item opgeslagen in de Azure Stack Marketplace. U kunt deze verwijderen uit uw Blob storage-locatie.
    > [!Caution]  
    > Alle standaard galerie artefacten en uw aangepaste galerie artefacten zijn nu toegankelijk zonder verificatie bij de volgende URL's:  
`https://adminportal.[Region].[external FQDN]:30015/artifact/20161101/[Template Name]/DeploymentTemplates/Template.json`  
`https://portal.[Region].[external FQDN]:30015/artifact/20161101/[Template Name]/DeploymentTemplates/Template.json`  
`https://systemgallery.blob.[Region].[external FQDN]/dev20161101-microsoft-windowsazure-gallery/[Template Name]/UiDefinition.json`

6. U kunt een Marketplace-item verwijderen met behulp van de **Remove-AzureRMGalleryItem** cmdlet. Voorbeeld:
   
        Remove-AzsGalleryItem -Name Microsoft.SimpleTemplate.1.0.0  –Verbose
   
   > [!NOTE]
   > De Marketplace-gebruikersinterface kan een fout worden weergegeven na het verwijderen van een item. De fout te verhelpen, klikt u op **instellingen** in de portal. Selecteer **wijzigingen negeren** onder **Portal aanpassing**.
   > 
   > 

## <a name="reference-marketplace-item-manifestjson"></a>Naslaginformatie: Marketplace-item manifest.json
### <a name="identity-information"></a>Gegevens van identiteit
| Naam | Vereist | Type | Beperkingen | Beschrijving |
| --- | --- | --- | --- | --- |
| Naam |X |Reeks |[A-Za-z0-9]+ | |
| Uitgever |X |Reeks |[A-Za-z0-9]+ | |
| Versie |X |Reeks |[SemVer v2](http://semver.org/) | |

### <a name="metadata"></a>Metagegevens
| Naam | Vereist | Type | Beperkingen | Beschrijving |
| --- | --- | --- | --- | --- |
| Weergavenaam |X |Reeks |Aanbeveling van 80 tekens |De portal kan de itemnaam van uw niet correct weergegeven als langer dan 80 tekens is. |
| PublisherDisplayName |X |Reeks |Aanbeveling van 30 tekens bevatten |De portal mogelijk de naam van de uitgever niet zonder problemen weergegeven als het langer dan 30 tekens bevatten. |
| PublisherLegalName |X |Reeks |Maximaal 256 tekens bestaan | |
| Samenvatting |X |Reeks |60 tot 100 tekens | |
| Onderliggend LongSummary |X |Reeks |140 tot 256 tekens |Niet nog van toepassing in Azure Stack. |
| Beschrijving |X |[HTML](https://auxdocs.azurewebsites.net/en-us/documentation/articles/gallery-metadata#html-sanitization) |500 tot 5000 tekens | |

### <a name="images"></a>Installatiekopieën
De Marketplace maakt gebruik van de volgende pictogrammen:

| Naam | Breedte | Hoogte | Opmerkingen |
| --- | --- | --- | --- |
| Breed |255 px |115 px |Altijd vereist |
| Groot |115 px |115 px |Altijd vereist |
| Middelgroot |90 px |90 px |Altijd vereist |
| Klein |40 px |40 px |Altijd vereist |
| Schermopname |533 px |32 px |Optioneel |

### <a name="categories"></a>Categorieën
Elke Marketplace-item moet worden gemarkeerd met een categorie die aangeeft waar het item wordt weergegeven op de gebruikersinterface van de portal. U kunt een van de bestaande categorieën in Azure Stack (Compute, gegevens en opslag, enzovoort) of een nieuw wachtwoord kiezen.

### <a name="links"></a>Koppelingen
Elke Marketplace-item kan verschillende koppelingen naar aanvullende inhoud bevatten. De koppelingen worden opgegeven als een lijst met namen en URI's.

| Naam | Vereist | Type | Beperkingen | Beschrijving |
| --- | --- | --- | --- | --- |
| Weergavenaam |X |Reeks |Maximaal 64 tekens | |
| URI |X |URI | | |

### <a name="additional-properties"></a>Aanvullende eigenschappen
Naast de voorgaande metagegevens krijgt u Marketplace auteurs aangepaste sleutel/waarde-paar gegevens in de volgende notatie:

| Naam | Vereist | Type | Beperkingen | Beschrijving |
| --- | --- | --- | --- | --- |
| Weergavenaam |X |Reeks |Maximum van 25 tekens | |
| Waarde |X |Reeks |Maximaal 30 tekens | |

### <a name="html-sanitization"></a>HTML-opschonen
Voor elk veld waarmee HTML, mogen de volgende elementen en kenmerken:

H1, h2, h3, h4, h5, p, ol, ul, li, een [doel | href], Brazilië, sterk, em, b, ik

## <a name="reference-marketplace-item-ui"></a>Naslaginformatie: Marketplace-item UI
Pictogrammen en tekst voor Marketplace-items zoals te zien is in de Azure Stack-portal zijn als volgt.

### <a name="create-blade"></a>Blade maken
![Blade maken](media/azure-stack-marketplace-item-ui-reference/image1.png)

### <a name="marketplace-item-details-blade"></a>Blade met details Marketplace-item
![Blade met details Marketplace-item](media/azure-stack-marketplace-item-ui-reference/image3.png)

