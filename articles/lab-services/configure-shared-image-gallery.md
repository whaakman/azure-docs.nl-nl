---
title: Een galerie met installatiekopieën van de gedeelde configureren in Azure DevTest Labs | Microsoft Docs
description: Informatie over het configureren van een galerie met installatiekopieën van de gedeelde in Azure DevTest Labs
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2019
ms.author: spelluru
ms.openlocfilehash: 51b394043f88789865edea5be6376ae536f88848
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66420436"
---
# <a name="configure-a-shared-image-gallery-in-azure-devtest-labs"></a>Een galerie met installatiekopieën van de gedeelde configureren in Azure DevTest Labs
DevTest Labs nu ondersteunt de [gedeelde afbeeldingengalerie](/virtual-machines/windows/shared-image-galleries.md) functie. Deze kan labgebruikers toegang krijgen tot afbeeldingen uit een gedeelde locatie tijdens het maken van de labresources. Ook kunt u structuur en de organisatie om uw aangepaste beheerde VM-installatiekopieën. De galerie met installatiekopieën van gedeelde functie ondersteunt:

- Beheerde globale replicatie van afbeeldingen
- Versiebeheer en groepering van beelden voor eenvoudiger beheer
- Uw installatiekopieën maximaal beschikbaar met Zone-redundante opslag (ZRS)-accounts maken in regio's die ondersteuning voor beschikbaarheidszones. ZRS biedt betere tolerantie tegen fouten in de zonegebonden.
- Delen tussen abonnementen en zelfs tussen tenants, met behulp van op rollen gebaseerd toegangsbeheer (RBAC).

Zie voor meer informatie, [gedeelde afbeeldingengalerie documentatie](../virtual-machines/windows/shared-image-galleries.md). 
 
Als u een groot aantal beheerde installatiekopieën die u moet onderhouden en wil graag zodat ze beschikbaar zijn in uw organisatie hebt, kunt u een galerie met installatiekopieën van de gedeelde als een opslagplaats waarmee u eenvoudig om te werken en het delen van uw afbeeldingen. Als de eigenaar van een lab, kunt u een bestaande gedeelde afbeeldingengalerie koppelen aan uw testomgeving. Zodra deze galerie is gekoppeld, worden labgebruikers kunnen machines met deze nieuwste installatiekopieën maken. Een belangrijk voordeel van deze functie is dat de DevTest Labs van nu duren het voordeel voordat kan van het delen van afbeeldingen tussen labs, meerdere abonnementen, en regio's. 

## <a name="considerations"></a>Overwegingen
- U kunt alleen een gedeelde afbeeldingengalerie koppelen aan een lab tegelijk. Als u wilt een andere galerie toevoegen, moet u het bestaande bestand loskoppelen en koppelt een andere. 
- DevTest Labs biedt op dit moment geen ondersteuning van installatiekopieën van het uploaden naar de galerie door in het lab. 
- DevTest Labs gebruikt tijdens het maken van een virtuele machine met behulp van een installatiekopie van de galerie met gedeelde installatiekopie, altijd de meest recente gepubliceerde versie van deze installatiekopie.
- Hoewel met DevTest Labs automatisch een poging wordt om te controleren of de galerie met installatiekopieën van gedeelde afbeeldingen gerepliceerd naar de regio waarin het Lab bestaat, is het niet altijd mogelijk. Om te voorkomen dat gebruikers hebt u problemen bij het maken van virtuele machines van deze installatiekopieën, zorg ervoor dat de installatiekopieën al worden gerepliceerd naar de regio van de testomgeving."

## <a name="use-azure-portal"></a>Azure Portal gebruiken
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer **alle Services** in het linkermenu van de navigatie.
1. Selecteer **DevTest Labs** uit de lijst.
1. Selecteer in de lijst met labs, uw **lab**.
1. Selecteer **configuratie en het beleid** in de **instellingen** sectie in het menu links.
1. Selecteer **gedeelde Afbeeldingsgalerieën** onder **virtuele machine bases** in het menu links.
1. Een bestaande gedeelde afbeeldingengalerie koppelen aan uw testomgeving door te klikken op de **Attach** knop en de galerie selecteren in de vervolgkeuzelijst.
1. Ga naar de galerie met gekoppelde en configureer de galerie in op **in- of uitschakelen** Gedeelde afbeeldingen voor het maken van virtuele machine.
1. Labgebruikers kunnen vervolgens met de ingeschakelde installatiekopieën door te klikken op een virtuele machine maken **+ toevoegen** en het vinden van de afbeelding in de **Kies uw base** pagina.

## <a name="use-azure-resource-manager-template"></a>Azure Resource Manager-sjabloon gebruiken

### <a name="attach-a-shared-image-gallery-to-your-lab"></a>Een galerie met installatiekopieën van de gedeelde koppelen aan uw testomgeving
Als u van een Azure Resource Manager-sjabloon gebruikmaakt een galerie met installatiekopieën van de gedeelde koppelen aan uw testomgeving, moet u deze toevoegen onder de sectie met resources van uw Resource Manager-sjabloon, zoals wordt weergegeven in het volgende voorbeeld:

```json
"resources": [
{
    "apiVersion": "2018-10-15-preview",
    "type": "Microsoft.DevTestLab/labs",
    "name": "[parameters('newLabName')]",
    "location": "[resourceGroup (). location]",
    "resources": [
    {
        "apiVersion": "2018-10-15-preview",
        "name": "[variables('labVirtualNetworkName')]",
        "type": "virtualNetworks",
        "dependsOn": [
            "[resourceId('Microsoft.DevTestLab/labs', parameters('newLabName'))]"
        ]
    },
    {
        "apiVersion":"2018-10-15-preview",
        "name":"myGallery",
        "type":"sharedGalleries",
        "properties": {
            "galleryId":"[parameters('existingSharedGalleryId')]",
            "allowAllImages": "Enabled"
        },
        "dependsOn":[
            "[resourceId('Microsoft.DevTestLab/labs', parameters('newLabName'))]"
        ]
    }
    ]
} 

```

Voor een compleet voorbeeld in de sjabloon Resource Manager, raadpleegt u deze Resource Manager-sjabloon-voorbeelden in onze openbare GitHub-opslagplaats: [Configureren van een galerie met installatiekopieën van de gedeelde tijdens het maken van een lab](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-lab-shared-gallery-configured).

### <a name="create-a-vm-using-an-image-from-the-shared-image-gallery"></a>Een virtuele machine met een installatiekopie uit de galerie met gedeelde installatiekopieën maken
Als u een Azure Resource Manager-sjabloon gebruikt om te maken van een virtuele machine met behulp van een installatiekopie van de galerie met gedeelde installatiekopie, gebruikt u het volgende voorbeeld:

```json

"resources": [
{
    "apiVersion": "2018-10-15-preview",
    "type": "Microsoft.DevTestLab/labs/virtualMachines",
    "name": "[variables('resourceName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "sharedImageId": "[parameters('existingSharedImageId')]",
        "size": "[parameters('newVMSize')]",
        "isAuthenticationWithSshKey": false,
        "userName": "[parameters('userName')]",
        "sshKey": "",
        "password": "[parameters('password')]",
        "labVirtualNetworkId": "[variables('labVirtualNetworkId')]",
        "labSubnetName": "[variables('labSubnetName')]"
    }
}
],

```

Voor meer informatie raadpleegt u deze Resource Manager-sjabloon-voorbeelden op onze openbare GitHub.
[Maak een virtuele machine met behulp van een installatiekopie van de galerie met gedeelde installatiekopie](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-vm-username-pwd-sharedimage).

## <a name="use-api"></a>-API gebruiken

- Gebruik API-versie 2018-10-15-preview.
- Als u wilt koppelen uw galerie, door de aanvraag te verzenden, zoals wordt weergegeven in het volgende codefragment:
    
    ``` 
    PUT [Lab Resource Id]/SharedGalleries/[newGalleryName]
    Body: 
    {
        “properties”:{
            “galleryId”: “[Shared Image Gallery resource Id]”,
            “allowAllImages”:”Enabled”
        }
    }
    ```
- Als u wilt weergeven van alle installatiekopieën in de galerie met installatiekopieën van de gedeelde, u kunt alle gedeelde afbeeldingen, samen met de resource-id's door een lijst

    ```
    GET [Lab Resource Id]/SharedGalleries/mySharedGallery/SharedImages
    ````
- Voor het maken van een virtuele machine met behulp van gedeelde afbeeldingen, kunt u doen een PUT op virtuele machines en in de eigenschappen van de virtuele machine, de ID van de gedeelde afbeeldingen die u hebt verkregen via de vorige aanroep doorgeeft. In de eigenschappen. SharedImageId


## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen op artefacten:

- [Verplichte artefacten voor uw lab opgeven](devtest-lab-mandatory-artifacts.md)
- [Aangepaste artefacten maken](devtest-lab-artifact-author.md)
- [Een artefactopslagplaats toevoegen aan een lab](devtest-lab-artifact-author.md)
- [Problemen met artefacten vaststellen](devtest-lab-troubleshoot-artifact-failure.md)
