---
title: Azure IoT Edge-module SKU's | Microsoft Docs
description: SKU's voor een IoT Edge-module maken.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 205fd258ed397f5a9588773549368fc3c4aec058
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51684814"
---
# <a name="iot-edge-module-skus-tab"></a>Tabblad IoT Edge module-SKU 's

De **SKU's** tabblad van de **nieuwe aanbieding** pagina kunt u een of meer SKU's maken en deze koppelt aan uw nieuwe aanbieding.  U kunt verschillende SKU's gebruiken om te onderscheiden van een oplossing door functiesets, factureringsmodellen of andere kenmerken.

## <a name="sku-settings"></a>SKU-instellingen

Wanneer u het maken van een nieuwe aanbieding start, er zijn niet alle SKU's die zijn gekoppeld aan de aanbieding. Volg deze stappen voor het maken van een nieuwe SKU:

- Op de **IoT Edge-Modules > nieuwe aanbieding** weergeeft, schakelt de **SKU's** tabblad.
- Selecteer onder SKU's, **+ nieuwe Voorraadeenheid** om een dialoogvenster te openen.

  ![Nieuwe SKU-knop op het tabblad nieuwe aanbieding voor IoT Edge-modules](./media/iot-edge-module-skus-tab-new-sku.png)

- Op de **nieuwe Voorraadeenheid** dialoogvenster vak, voer een id voor de SKU en selecteer vervolgens **OK**.
(In de volgende tabel geeft de id voor naamgeving.)

De **SKU's** tabblad wordt vernieuwd en worden de velden die u voor het configureren van de SKU bewerken weergegeven. Een sterretje (*) toegevoegd aan de naam van het veld geeft aan dat dit is verplicht.

|  **Veld**       |     **Beschrijving**                                                          |
|  ---------       |     ---------------                                                          |
| **SKU-ID**       | De id voor deze SKU. Deze naam is een maximum van 50 tekens, die bestaan uit kleine letters, alfanumerieke tekens en streepjes (-), maar mag niet eindigen met een streepje. **Opmerking:** u deze naam niet wijzigen nadat de aanbieding de gepubliceerd. De naam is zichtbaar voor iedereen in product URL's. |

## <a name="sku-details"></a>SKU-Details

Configureer de **SKU-Details** om te definiëren hoe de SKU die wordt weergegeven op de websites van Azure Marketplace en Azure-Portal.

![IoT Edge module sku metagegevens](media/iot-edge-module-skus-tab-metadata.png)

De volgende tabel beschrijft de doel-inhoud en opmaak voor velden onder **SKU-Details**.

|  **Veld**       |     **Beschrijving**                                                          |
|  ---------       |     ---------------                                                          |
| **Titel**        | Titel voor deze SKU. Maximale lengte van maximaal 50 tekens. <br/> Deze wordt weergegeven in de Azure-Portal en wordt gebruikt als de naam van een standaard-module (zonder spaties en speciale tekens) wanneer deze geïmplementeerd. Zie de afbeeldingen hieronder om te zien precies waar dit veld wordt weergegeven.|
| **Samenvatting**      | Korte samenvatting van deze SKU. Maximale lengte van 100 tekens. Voer **niet** samenvatten van de aanbieding, alleen de SKU.  Dit overzicht wordt weergegeven in de Azure Marketplace. Zie de afbeeldingen hieronder om te zien precies waar dit veld wordt weergegeven.|
| **Beschrijving**  | Korte beschrijving van deze SKU. Maximale lengte van 3000 tekens. Beschrijven niet het aanbod, maar alleen deze SKU. Deze worden weergegeven in de azure marketplace en in de Azure-portal. In Azure portal, wordt deze toegevoegd aan de Marketplace-beschrijving met een beschrijving van de aanbieding die is gedefinieerd in de Marketplace-tabblad.  Kan het zijn hetzelfde als de SKU-samenvatting. Zie de afbeeldingen hieronder om te zien precies waar dit veld wordt weergegeven.|
| **Deze SKU verbergen** | De standaardinstelling is behouden **Nee**. |

### <a name="sku-example"></a>Voorbeeld van de SKU

 De volgende voorbeelden ziet u hoe de SKU **titel**, **samenvatting**, en **beschrijving** velden worden weergegeven in andere weergaven.
 
#### <a name="on-the-azure-marketplace-website"></a>Op de Azure Marketplace-website:

- Bij het onderzoeken van de SKU-details:

    ![Hoe SKU's worden weergegeven in Azure Marketplace-website](media/iot-edge-module-ampdotcom-pdp-plans.png)

#### <a name="on-the-azure-portal-website"></a>Op de website van Azure Portal:

- Bij het bladeren door SKU's:

    ![Hoe IoT Edge-module wordt weergegeven bij het bladeren door de Azure portal #1](media/iot-edge-module-portal-browse.png)

    ![Hoe IoT Edge-module wordt weergegeven bij het bladeren door de Azure portal #2](media/iot-edge-module-portal-product-picker.png)

- Bij het zoeken naar SKU's:

    ![Hoe IoT Edge-module wordt weergegeven bij het zoeken naar de Azure-portal](media/iot-edge-module-portal-search.png)

- Bij het onderzoeken van de SKU-details:

    ![Hoe IoT Edge-module wordt weergegeven bij het zoeken naar details van het product in de portal](./media/iot-edge-module-portal-pdp.png)

- Bij het implementeren van de module:
    
    ![Hoe IoT Edge-module wordt weergegeven wanneer wordt geïmplementeerd](./media/iot-edge-module-deployment.png)

## <a name="sku-content"></a>SKU-inhoud

Onder **Edge-Module installatiekopieën**, de gegevens die we nodig hebben voor het uploaden van uw IoT Edge-module.

Geef ons de toegang tot uw [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) (ACR) die uw IoT Edge module installatiekopie, zodat we kunnen uploaden en het certificeren bevat. Nadat deze gepubliceerd, wordt uw IoT Edge-module moet worden gekopieerd en gedistribueerd met behulp van een openbare container-register, gehost door de Azure Marketplace.

U kunt richten op meerdere platforms en bieden van verschillende versies tot en met tags. Meer informatie over [tags en versiebeheer in 'voorbereiden uw IoT Edge module technische activa'](./cpp-create-technical-assets.md).

![IoT Edge Module installatiekopieën](./media/iot-edge-module-skus-tab-acr.png)

De volgende tabel beschrijft het doel, inhoud en opmaak van de velden voor:

- **Details van de installatiekopie van-opslagplaats**
- **De versie van installatiekopie**

|  **Veld**       |     **Beschrijving**                                                          |
|  ---------       |     ---------------                                                          |
|  ***Details van de installatiekopie van-opslagplaats***   |  |
| **abonnements-ID**        | De Azure-abonnement-ID van uw ACR.|
| **De naam van resourcegroep**      | Naam van de resourcegroep van uw ACR.|
| **Registernaam**  | De naam van uw ACR-register. Alleen kopiëren de registernaam, niet de naam van server (bijvoorbeeld, zonder de `azurecr.io`.) |
| **Naam van de opslagplaats**  | De naam van de opslagplaats van uw ACR met uw IoT Edge-module. **Opmerking:** nadat de naam is ingesteld, deze later niet meer wijzigen. Gebruik een unieke naam om ervoor te zorgen dat er geen andere aanbieding in uw account dezelfde naam heeft. |
| **Gebruikersnaam** | De gebruikersnaam die is gekoppeld aan uw ACR (admin gebruikersnaam). |
| **Wachtwoord** | Het wachtwoord dat is gekoppeld aan uw ACR. |
|  ***De versie van installatiekopie***   |  |
| **Tag Image- of Samenvattingsupdates** | Het omvat ten minste een `latest` tag en een versietag (bijvoorbeeld beginnen met `xx.xx.xx-` waarbij xx is een getal). Ze moeten [manifest tags](https://github.com/estesp/manifest-tool) om u te richten op meerdere platforms. Alle tags waarnaar wordt verwezen door een manifest tag moeten ook worden toegevoegd, zodat we kunnen het uploaden. U kunt verschillende versies van een IoT Edge-module met behulp van labels toevoegen. Alle tags manifest (met uitzondering van `latest`) moet beginnen met een `X.Y-` of `X.Y.Z-` waarbij X, Y, Z gehele getallen zijn. Meer informatie over [tags en versiebeheer in 'voorbereiden uw IoT Edge module technische activa'](./cpp-create-technical-assets.md). <br/> Bijvoorbeeld, als een `latest` punten die verwijst naar code `1.0.1-linux-x64`, `1.0.1-linux-arm32`,, en `1.0.1-windows-arm32`, deze 6 tags moet hier worden toegevoegd. |

### <a name="help-your-customers-launch-your-iot-edge-module-by-using-default-settings"></a>Help uw klanten Start uw IoT Edge-module met behulp van standaard-instellingen

Definieer de meestgebruikte instellingen voor het implementeren van uw IoT Edge-module. Implementaties van klanten optimaliseren doordat ze start uw IoT Edge module out-of-the-box met deze standaardinstellingen.

![IoT Edge module standaardinstellingen bij de implementatie](./media/iot-edge-module-skus-tab-iot-edge-defaults.png)

De volgende tabel beschrijft het doel, inhoud en opmaak van de velden voor **standaard Routes**, **standaard apparaatdubbel-gewenste eigenschappen**, **standaard omgevingsvariabelen**, en **CreateOptions standaard**.

|  **Veld**       |     **Beschrijving**                                                          |
|  ---------       |     ---------------                                                          |
| **Standaardroutes**        | De standaardnaam van de route en de waarde moet minder dan 512 tekens. U kunt maximaal 5 standaardroutes definiëren. Zorg ervoor dat u een juiste [routeren syntaxis](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes) in de waarde van de route. Om te verwijzen naar de module, gebruikt u de naam van de module standaard, die wordt uw **SKU titel** zonder spaties en speciale tekens. Om te verwijzen naar andere modules die nog niet bekend is, gebruikt u de `<FROM_MODULE_NAME>` naamgeving om aan te zodat uw klanten weten dat ze nodig hebben om bij te werken van deze gegevens. Meer informatie over [IoT Edge routes](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes). <br/> Bijvoorbeeld, als module `ContosoModule` luistert voor invoer `ContosoInput` - en uitvoergegevens op `ContosoOutput`, het verstandig om de volgende 2 standaardroutes definiëren:<br/>-De naam #1: `ToContosoModule`<br/>-Waarde #1:`FROM /messages/modules/<FROM_MODULE_NAME>/outputs/* INTO BrokeredEndpoint("/modules/ContosoModule/inputs/ContosoInput")`<br/>-De naam #2: `FromContosoModuleToCloud`<br/>-Waarde #2: `FROM /messages/modules/ContonsoModule/outputs/ContosoOuput INTO $upstream`<br/>  |
| **Standaard apparaatdubbel-gewenste eigenschappen**      | Elke standaardnaam apparaatdubbel-gewenste eigenschappen en de waarde moet minder dan 512 tekens. U kunt maximaal 5 naam/waarde apparaatdubbel-gewenste eigenschappen definiëren. Waarden van gewenste dubbeleigenschappen moet geldige JSON, niet-escape, met een geneste maximumhiërarchie van 4 en zonder matrices. Meer informatie over [apparaatdubbel-gewenste eigenschappen](https://docs.microsoft.com/azure/iot-edge/module-composition#define-or-update-desired-properties). <br/> Bijvoorbeeld, als een module biedt ondersteuning voor een dynamisch configureerbare vernieuwingsfrequentie via apparaatdubbel-gewenste eigenschappen, verstandig het om de volgende standaard apparaatdubbel-gewenste eigenschap definiëren:<br/> -De naam #1: `RefreshRate`<br/>-Waarde #1: `60`|
| **Standaard-omgevingsvariabelen**  | Elke standaardnaam variabelen van de omgeving en de waarde moet minder dan 512 tekens. U kunt maximaal 5 naam/waarde-omgevingsvariabelen definiëren. <br/>Als een module nodig heeft om te accepteren van de gebruiksrechtovereenkomst voordat het wordt gestart, kunt u bijvoorbeeld de volgende omgevingsvariabele definiëren:<br/> -De naam #1: `ACCEPT_EULA`<br/>-Waarde #1: `Y`|
| **Standaard createOptions**  | De createOptions moet minder dan 512 tekens. Het moet geldige JSON, niet-escape. Meer informatie over [createOptions](https://docs.microsoft.com/azure/iot-edge/module-composition#configure-modules). <br/> Bijvoorbeeld, als een module nodig heeft een poort binden kunt u de volgende createOptions definiëren:<br/>  `"HostConfig":{"PortBindings":{"5012/tcp":[{"HostPort":"5012"}]}`|

<br/> Selecteer **opslaan** uw SKU-instellingen op te slaan. 

## <a name="next-steps"></a>Volgende stappen

Gebruik de [Marketplace tabblad](./cpp-marketplace-tab.md) te maken van een marketplace-beschrijving van uw aanbieding.
