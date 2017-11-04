---
title: Doorlopende integratie in VS-Team Services met behulp van Azure Resource Group projecten | Microsoft Docs
description: Beschrijft hoe stel doorlopende integratie in Visual Studio Team Services met behulp van Azure Resource Group implementatieprojecten in Visual Studio.
services: visual-studio-online
documentationcenter: na
author: mlearned
manager: erickson-doug
editor: 
ms.assetid: b81c172a-be87-4adc-861e-d20b94be9e38
ms.service: azure-resource-manager
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: mlearned
ms.openlocfilehash: e7d98ca3fa281a136595c37ed9b7e71de0cf7bff
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="continuous-integration-in-visual-studio-team-services-using-azure-resource-group-deployment-projects"></a>Doorlopende integratie in Visual Studio Team Services met behulp van Azure Resource Group implementatieprojecten
Voor het implementeren van een Azure-sjabloon u taken uitvoeren in verschillende fasen: bouw, Test, kopiëren naar Azure (ook wel 'Fasering'), en de sjabloon implementeert. Er zijn twee verschillende manieren om sjablonen te implementeren voor Visual Studio Team Services (VS Team Services). Beide methoden bieden dezelfde resultaten, dus kiest die het beste past bij uw werkstroom.

1. Één stap toevoegen aan de definitie van de build met de PowerShell-script dat opgenomen in het implementatieproject Azure Resource Group (implementeren AzureResourceGroup.ps1). Het script kopieert artefacten en implementeert vervolgens de sjabloon.
2. Voeg dat meerdere tegenover Team Services bouwen stappen elkaar uitvoeren van een taak fase.

In dit artikel wordt gedemonstreerd beide opties. De eerste optie heeft het voordeel van het gebruik van hetzelfde script gebruikt door ontwikkelaars in Visual Studio en verstrekken consistentie gedurende de levenscyclus. De tweede optie biedt een alternatief voor het ingebouwde script. Beide procedures wordt ervan uitgegaan dat u hebt al een implementatieproject voor Visual Studio in VS-teamservices ingecheckt.

## <a name="copy-artifacts-to-azure"></a>Artefacten kopiëren naar Azure
Ongeacht het scenario, als u alle artefacten die nodig zijn voor de sjabloonimplementatie, hebt moet u Azure Resource Manager toegang geven tot ze. Deze artefacten kunnen u de volgende bestanden:

* Geneste sjablonen
* Configuratiescripts en DSC-scripts
* Binaire bestanden van toepassingen

### <a name="nested-templates-and-configuration-scripts"></a>Geneste sjablonen en configuratiescripts
Wanneer u de sjablonen die worden geleverd door Visual Studio gebruikt (of gebouwd met Visual Studio codefragmenten), het PowerShell-script bereidt niet alleen de artefacten, het ook de URI voor de bronnen voor verschillende implementaties parameterizes. Het script en vervolgens de artefacten kopieert naar een veilige container in Azure, maakt u een SaS-token voor die container en stuurt vervolgens deze informatie doorsturen naar de sjabloonimplementatie. Zie [sjabloonimplementatie met een maakt](https://msdn.microsoft.com/library/azure/dn790564.aspx) voor meer informatie over geneste sjablonen.  Wanneer u taken in de VS Team Services, moet u de juiste taken voor uw sjabloonimplementatie selecteren en indien nodig, parameterwaarden doorgeven van de staging stap voor de sjabloonimplementatie.

## <a name="set-up-continuous-deployment-in-vs-team-services"></a>Doorlopende implementatie in VS-teamservices ingesteld
Voor het aanroepen van het PowerShell-script in VS-Team Services, moet u de definitie van de build bijwerken. In het kort zijn de stappen: 

1. Bewerk de build-definitie.
2. Azure autorisatie in VS-teamservices instellen.
3. De stap van een Azure PowerShell-build die verwijst naar het PowerShell-script in het implementatieproject Azure-resourcegroep toevoegen.
4. Stel de waarde van de *- ArtifactsStagingDirectory* parameter met een ingebouwd in VS-teamservices project werkt.

### <a name="detailed-walkthrough-for-option-1"></a>Gedetailleerd overzicht optie 1
De volgende procedures helpt u stapsgewijs door de stappen die nodig zijn voor het configureren van continue implementatie in VS-Team Services met behulp van een enkele taak waarop de PowerShell-script wordt uitgevoerd in uw project. 

1. Bewerk de definitie van de VS Team Services bouwen en toevoegen van een Azure PowerShell build stap. Kies de definitie van de build onder de **bouwen definities** categorie en kies vervolgens de **bewerken** koppeling.
   
   ![De definitie van de build bewerken][0]
2. Voeg een nieuwe **Azure PowerShell** bouwen stap in de definitie van de build en kies vervolgens de **stap van de build toevoegen...** te klikken.
   
   ![Build stap toevoegen][1]
3. Kies de **implementeren taak** categorie, selecteer de **Azure PowerShell** taak en kies vervolgens de **toevoegen** knop.
   
   ![Taken toevoegen][2]
4. Kies de **Azure PowerShell** stap bouwen en vul vervolgens de waarden.
   
   1. Als u al een Azure-service-eindpunt toegevoegd aan het VS Team Services, kiest u het abonnement in de **Azure-abonnement** keuzelijst met invoervak en ga vervolgens naar de volgende sectie. 
      
      Als u geen een Azure-service-eindpunt in VS-Team Services, moet u een toe te voegen. Deze subsectie gaat u door het proces. Als uw Azure-account gebruikt een Microsoft-account (zoals Hotmail), moet u rekening houden met de volgende stappen om een Service-Principal-verificatie.
   2. Kies de **beheren** koppelen naast de **Azure-abonnement** keuzelijst met invoervak.
      
      ![Azure-abonnementen beheren][3]
   3. Kies **Azure** in de **nieuwe Service-eindpunt** keuzelijst met invoervak.
      
      ![Nieuwe service-eindpunt][4]
   4. In de **Azure-abonnement toevoegen** selecteert u de **Service-Principal** optie.
      
      ![Service-principal optie][5]
   5. Uw Azure-abonnement u informatie toevoegen aan de **Azure-abonnement toevoegen** in het dialoogvenster. U moet bieden de volgende items:
      
      * Abonnements-Id
      * De naam van abonnement
      * Service-Principal-Id
      * Service-Principal-sleutel
      * Tenant-Id
   6. Voeg een naam van uw keuze op de **abonnement** naamvak. Deze waarde wordt weergegeven later in de **Azure-abonnement** vervolgkeuzelijst in VS-Team Services. 
   7. Als u uw Azure-abonnement-ID niet weet, kunt u een van de volgende opdrachten te halen.
      
      Gebruik voor PowerShell-scripts:
      
      `Get-AzureRmSubscription`
      
      Gebruik voor Azure CLI:
      
      `azure account show`
   8. Ophalen van een Service-Principal-ID, Service-Principal-sleutel en het Tenant-ID, volg de procedure in [een Active Directory-toepassing en service-principal maken met portal](resource-group-create-service-principal-portal.md) of [verifiëren van een service-principal met Azure Resource Manager](resource-group-authenticate-service-principal.md).
   9. De Service-Principal-ID, Service-Principal-sleutel en het Tenant-ID Voeg waarden toe aan de **Azure-abonnement toevoegen** dialoogvenster vak en kies vervolgens de **OK** knop.
      
      U hebt nu een geldig Service-Principal gebruik van de Azure PowerShell-script uit te voeren.
5. Bewerk de build-definitie en kies de **Azure PowerShell** stap bouwen. Selecteer het abonnement in de **Azure-abonnement** keuzelijst met invoervak. (Als het abonnement niet wordt weergegeven, selecteert u de **vernieuwen** knop naast de **beheren** koppeling.) 
   
   ![Azure PowerShell opbouwtaak configureren][8]
6. Geef een pad naar de implementeren AzureResourceGroup.ps1 PowerShell-script. Om dit te doen, kiest u de knop met het weglatingsteken (...) naast de **scriptpad** vak, gaat u naar de implementeren AzureResourceGroup.ps1 PowerShell-script in de **Scripts** selecteert u deze map van uw project en kies vervolgens de **OK** knop.    
   
   ![Selecteer het pad naar script][9]
7. Nadat u het script hebt geselecteerd, bijwerken, het pad naar het script zodat deze wordt uitgevoerd vanaf de Build.StagingDirectory (de directory die *ArtifactsLocation* is ingesteld op). U kunt dit doen door toe te voegen "$(Build.StagingDirectory)/ 'aan het begin van het scriptpad.
   
    ![Pad naar script bewerken][10]
8. In de **scriptparameters** Voer de volgende parameters (op één regel). Wanneer u het script in Visual Studio uitvoert, kunt u zien hoe tegenover maakt gebruik van de parameters in de **uitvoer** venster. U kunt dit gebruiken als een beginpunt voor het instellen van de parameterwaarden in uw build-stap.
   
   | Parameter | Beschrijving |
   | --- | --- |
   | -ResourceGroupLocation |De waarde van de geografische locatie waar de resourcegroep zich bevindt, zoals **eastus** of **'VS-Oost'**. (Toevoegen tussen enkele aanhalingstekens als de naam een spatie.) Zie [Azure-gebieden](https://azure.microsoft.com/en-us/regions/) voor meer informatie. |
   | -ResourceGroupName |De naam van de resourcegroep die wordt gebruikt voor deze implementatie. |
   | -UploadArtifacts |Deze parameter, indien aanwezig, geeft aan dat artefacten die moet worden geüpload naar Azure uit het lokale systeem. U hoeft deze switch instellen als uw sjabloonimplementatie vereist extra artefacten die u wilt voorbereiden met de PowerShell-script (zoals configuratiescripts of geneste sjablonen). |
   | -StorageAccountName |De naam van het storage-account gebruikt voor fase artefacten voor deze implementatie. Deze parameter wordt alleen gebruikt als u bij het Faseren van artefacten voor implementatie. Als deze parameter wordt opgegeven, wordt een nieuw opslagaccount gemaakt als het script niet tijdens een eerdere implementatie gemaakt is. Als de parameter wordt opgegeven, wordt het opslagaccount moet al bestaan. |
   | -StorageAccountResourceGroupName |De naam van de resourcegroep die zijn gekoppeld aan het opslagaccount. Deze parameter is alleen vereist als u een waarde voor de parameter StorageAccountName opgeven. |
   | -Sjabloonbestand |Het pad naar het sjabloonbestand in het implementatieproject Azure-resourcegroep. Betere flexibiliteit, moet u een pad gebruiken voor deze parameter die is gebaseerd op de locatie van de PowerShell-script in plaats van een absoluut pad. |
   | -TemplateParametersFile |Het pad naar het parameterbestand in het implementatieproject Azure-resourcegroep. Betere flexibiliteit, moet u een pad gebruiken voor deze parameter die is gebaseerd op de locatie van de PowerShell-script in plaats van een absoluut pad. |
   | -ArtifactStagingDirectory |Deze parameter kunt het PowerShell script weet in de map van waaruit de binaire bestanden van het project moeten worden gekopieerd. De standaardwaarde gebruikt door het PowerShell-script, overschrijft deze waarde. Voor VS Team Services gebruikt, stelt u de waarde in op: - ArtifactStagingDirectory $(Build.StagingDirectory) |
   
   Hier volgt een voorbeeld van een script argumenten (line verbroken voor de leesbaarheid):
   
   ```    
   -ResourceGroupName 'MyGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\azuredeploy.json' 
   -TemplateParametersFile '..\templates\azuredeploy.parameters.json' -UploadArtifacts -StorageAccountName 'mystorageacct' 
   –StorageAccountResourceGroupName 'Default-Storage-EastUS' -ArtifactStagingDirectory '$(Build.StagingDirectory)'    
   ```
   
   Wanneer u klaar bent, de **scriptparameters** vak moet eruitzien als in de volgende lijst:
   
   ![Scriptargumenten][11]
9. Nadat u alle vereiste onderdelen aan de Azure PowerShell build stap hebt toegevoegd, kiest u de **wachtrij** bouwen om het project te bouwen. De **bouwen** scherm ziet u de uitvoer van het PowerShell-script.

### <a name="detailed-walkthrough-for-option-2"></a>Optie 2 gedetailleerd overzicht
De volgende procedures helpt u stapsgewijs door de stappen die nodig zijn voor het configureren van continue implementatie in VS-Team Services met behulp van de ingebouwde taken.

1. Bewerk de definitie van de VS Team Services bouwen om toe te voegen dat twee nieuwe stappen bouwen. Kies de definitie van de build onder de **bouwen definities** categorie en kies vervolgens de **bewerken** koppeling.
   
   ![Build-definition bewerken][12]
2. De nieuwe build-stappen toevoegen aan de build definitie met behulp van de **stap van de build toevoegen...** te klikken.
   
   ![Build stap toevoegen][13]
3. Kies de **implementeren** taakcategorie, selecteer de **Azure bestandskopie** taak en kies vervolgens de **toevoegen** knop.
   
   ![Azure bestandskopie taak toevoegen][14]
4. Kies de **Azure Resource Group Deployment** taak en kies vervolgens de **toevoegen** knop en vervolgens **sluiten** de **taak catalogus**.
   
   ![Azure Resource Group Deployment taak toevoegen][15]
5. Kies de **Azure bestandskopie** taak en de waarden in te vullen.
   
   Als u al een Azure-service-eindpunt toegevoegd aan het VS Team Services, kiest u het abonnement in de **Azure-abonnement** keuzelijst met invoervak. Als u niet een abonnement hebt, raadpleegt u [optie 1](#detailed-walkthrough-for-option-1) voor instructies over het instellen van een van in VS-teamservices.
   
   * Bron - Voer **$(Build.StagingDirectory)**
   * Azure verbindingstype - Selecteer **Azure Resource Manager**
   * Abonnement voor Azure RM - Selecteer het abonnement voor het opslagaccount dat u gebruiken wilt de **Azure-abonnement** keuzelijst met invoervak. Als het abonnement niet wordt weergegeven, selecteert u de **vernieuwen** knop naast de **beheren** koppeling.
   * Doeltype - Selecteer **Azure Blob**
   * RM Storage-Account - Selecteer de storage-account wilt gebruiken voor het Faseren van artefacten
   * Containernaam van de: Voer de naam van de container die u wilt gebruiken voor fasering; elke naam zijn geldige container kan, maar gebruikt u een toegewezen aan deze build-definitie
   
   De waarden van uitvoer:
   
   * Storage-Container URI - Voer **artifactsLocation**
   * Storage-Container SAS-Token - Voer **artifactsLocationSasToken**
   
   ![De taak Azure bestand kopiëren configureren][16]
6. Kies de **Azure Resource Group Deployment** stap bouwen en vul vervolgens de waarden.
   
   * Azure verbindingstype - Selecteer **Azure Resource Manager**
   * Abonnement voor Azure RM - Selecteer het abonnement voor de implementatie in de **Azure-abonnement** keuzelijst met invoervak. Dit is meestal gebruikt in de vorige stap hetzelfde abonnement
   * Actie - Selecteer **maken of bijwerken resourcegroep**
   * Resourcegroep - Selecteer een resourcegroep of Voer de naam van een nieuwe resourcegroep voor de implementatie
   * Locatie - de locatie voor de resourcegroep selecteren
   * Sjabloon: Geef het pad en de naam van de sjabloon worden geïmplementeerd voorafgaand **$(Build.StagingDirectory)**, bijvoorbeeld: **$(Build.StagingDirectory/DSC-CI/azuredeploy.json)**
   * Sjabloonparameters - Geef het pad en de naam van de parameters moet worden gebruikt, voorafgaand **$(Build.StagingDirectory)**, bijvoorbeeld: **$(Build.StagingDirectory/DSC-CI/azuredeploy.parameters.json)**
   * Sjabloonparameters overschrijven: Voer of kopieer en plak de volgende code:
     
     ```    
     -_artifactsLocation $(artifactsLocation) -_artifactsLocationSasToken (ConvertTo-SecureString -String "$(artifactsLocationSasToken)" -AsPlainText -Force)
     ```
     ![Azure-resourcegroep implementatie taak configureren][17]
7. Nadat u alle vereiste onderdelen hebt toegevoegd, slaat u de definitie van de build en kies **wachtrij nieuwe build** aan de bovenkant.

## <a name="next-steps"></a>Volgende stappen
Lees [overzicht van Azure Resource Manager](azure-resource-manager/resource-group-overview.md) voor meer informatie over Azure Resource Manager en Azure-resourcegroepen.

[0]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough1.png
[1]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough2.png
[2]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough3.png
[3]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough4.png
[4]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough5.png
[5]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough6.png
[8]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough9.png
[9]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough10.png
[10]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough11b.png
[11]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough12.png
[12]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough13.png
[13]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough14.png
[14]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough15.png
[15]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough16.png
[16]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough17.png
[17]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough18.png
