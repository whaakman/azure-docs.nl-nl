---
title: Continue integratie in Azure DevOps-Services met behulp van Azure-resourcegroepprojecten | Microsoft Docs
description: Beschrijft het instellen van continue integratie in Azure DevOps-Services met behulp van Azure Resource Group deployment projecten in Visual Studio.
services: visual-studio-online
documentationcenter: na
author: mlearned
manager: erickson-doug
editor: ''
ms.assetid: b81c172a-be87-4adc-861e-d20b94be9e38
ms.service: azure-resource-manager
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: mlearned
ms.openlocfilehash: a3dfca48b52af39e7a536b3012a3f4cdac4e9a94
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49955164"
---
# <a name="continuous-integration-in-azure-devops-services-using-azure-resource-group-deployment-projects"></a>Continue integratie in Azure DevOps-Services met behulp van Azure Resource Group deployment projecten
Voor het implementeren van een Azure-sjabloon, u taken kunt uitvoeren in verschillende stadia: bouw, Test, kopiëren naar Azure (ook wel "Staging"), en implementeren van sjablonen. Er zijn twee verschillende manieren om sjablonen te implementeren met Azure DevOps-Services. Beide methoden bieden dezelfde resultaten, daarom kiest die het beste past bij uw werkstroom.

1. Één stap toevoegen aan uw build-pijplijn met de PowerShell-script dat opgenomen in het implementatieproject Azure-resourcegroep (implementeren-AzureResourceGroup.ps1) wordt uitgevoerd. Het script artefacten worden gekopieerd en vervolgens de sjabloon wordt geïmplementeerd.
2. Voeg dat meerdere Azure DevOps-Services bouwen stappen, elk ervan een fase-taak uit te voeren.

In dit artikel ziet u beide opties. De eerste optie heeft het voordeel van het gebruik van hetzelfde script gebruikt door ontwikkelaars in Visual Studio en bieden consistentie gedurende de levenscyclus. De tweede optie biedt een alternatief voor het ingebouwde script. Beide procedures wordt ervan uitgegaan dat u hebt al een Visual Studio-implementatieproject ingecheckt in Azure DevOps-Services.

## <a name="copy-artifacts-to-azure"></a>Artefacten kopiëren naar Azure
Ongeacht het scenario, hebt u alle artefacten die nodig zijn voor de sjabloonimplementatie van de moet u Azure Resource Manager toegang verleent tot ze. Deze artefacten kunnen, zoals bestanden opnemen:

* Geneste sjablonen
* Configuratiescripts en DSC-scripts
* Binaire waarden van toepassingen

### <a name="nested-templates-and-configuration-scripts"></a>Geneste sjablonen en configuratiescripts
Wanneer u de sjablonen die worden geleverd door Visual Studio gebruikt (of die zijn gebouwd met Visual Studio-fragmenten), het PowerShell-script bereidt niet alleen de artefacten, deze ook de URI voor de resources voor verschillende implementaties parameterizes. Het script en vervolgens de artefacten worden gekopieerd naar een beveiligde container in Azure, maakt u een SaS-token voor die container, en vervolgens worden doorgegeven die informatie u aan bij de sjabloonimplementatie. Zie [maken van een sjabloonimplementatie](https://msdn.microsoft.com/library/azure/dn790564.aspx) voor meer informatie over geneste sjablonen.  Wanneer u taken in Azure DevOps-Services, moet u de juiste taken selecteren voor de sjabloonimplementatie en indien nodig, parameterwaarden doorgeven in de staging stap om de sjabloonimplementatie van de.

## <a name="set-up-continuous-deployment-in-azure-pipelines"></a>Instellen van continue implementatie in Azure-pijplijnen
Voor het aanroepen van het PowerShell-script in Azure pijplijnen, moet u bijwerken van uw build-pijplijn. In het kort zijn de stappen: 

1. Bewerk de build-pijplijn.
2. Instellen van Azure autorisatie in Azure-pijplijnen.
3. Een stap voor het bouwen van Azure PowerShell die verwijst naar het PowerShell-script in het implementatieproject voor Azure-resourcegroep toevoegen.
4. Stel de waarde van de *- ArtifactsStagingDirectory* parameter om te werken met een project dat is gebouwd in Azure-pijplijnen.

### <a name="detailed-walkthrough-for-option-1"></a>Gedetailleerd overzicht van de optie 1
De volgende procedures helpen u bij de benodigde stappen voor het configureren van continue implementatie in Azure DevOps-Services met behulp van een enkele taak die wordt uitgevoerd het PowerShell-script in uw project. 

1. Bewerk uw build-pijplijn van Azure DevOps-Services en een Azure PowerShell-build-stap toevoegen. Kies de build-pijplijn onder de **pijplijnen bouwen** categorie en kies vervolgens de **bewerken** koppeling.
   
   ![Build-pijplijn bewerken][0]
2. Toevoegen van een nieuwe **Azure PowerShell** build-stap aan het build-pijplijn en kies vervolgens de **build-stap toevoegen...** .
   
   ![Build-stap toevoegen][1]
3. Kies de **implementeren taak** categorie, selecteer de **Azure PowerShell** taak en kies vervolgens de **toevoegen** knop.
   
   ![Taken toevoegen][2]
4. Kies de **Azure PowerShell** build-stap en vul vervolgens de waarden ervan.
   
   1. Als u een Azure-service-eindpunt dat is toegevoegd aan Azure DevOps-Services al hebt, kiest u het abonnement in de **Azure-abonnement** vervolgkeuzelijst vak en vervolgens gaat u verder met de volgende sectie. 
      
      Als u een Azure-service-eindpunt in Azure DevOps-Services hebt, moet u een toe te voegen. Deze subsectie doorloopt u het proces. Als uw Azure-account maakt gebruik van een Microsoft-account (zoals Hotmail), moet u de volgende stappen uit om op te halen van een Service-Principal verificatie uitvoeren.

   2. Kies de **beheren** koppelen naast de **Azure-abonnement** vervolgkeuzelijst met box.
      
      ![Azure-abonnementen beheren][3]
   3. Kies **Azure** in de **nieuwe Service-eindpunt** vervolgkeuzelijst met box.
      
      ![Nieuwe service-eindpunt][4]
   4. In de **Azure-abonnement toevoegen** in het dialoogvenster, selecteer de **Service-Principal** optie.
      
      ![Service-principal-optie][5]
   5. Voeg de informatie over uw Azure-abonnement toe de **Azure-abonnement toevoegen** in het dialoogvenster. U moet opgeven van de volgende items:
      
      * Abonnements-id
      * Abonnementsnaam
      * Service-Principal-Id
      * Service-Principal-sleutel
      * Tenant-id
   6. Toevoegen van een naam van uw keuze op de **abonnement** naamvak. Deze waarde wordt later in de **Azure-abonnement** vervolgkeuzelijst in Azure DevOps-Services. 

   7. Als u uw Azure-abonnement-ID niet weet, kunt u een van de volgende opdrachten te halen.
      
      Gebruik voor PowerShell-scripts:
      
      `Get-AzureRmSubscription`
      
      Gebruik voor Azure CLI:
      
      `azure account show`
   8. Ophalen van een Service-Principal-ID, Service-Principal-sleutel en Tenant-ID, volg de procedure in [een Active Directory-toepassing en serviceprincipal maken met portal](active-directory/develop/howto-create-service-principal-portal.md) of [verifiëren van een service-principal met Azure Resource Manager](active-directory/develop/howto-authenticate-service-principal-powershell.md).
   9. De Service-Principal-ID, Service-Principal-sleutel en Tenant-ID Voeg waarden toe aan de **Azure-abonnement toevoegen** dialoogvenster vak en kies vervolgens de **OK** knop.
      
      U hebt nu een geldige Service-Principal gebruiken de Azure PowerShell-script uit te voeren.
5. Bewerk de build-pijplijn en op de **Azure PowerShell** build-stap. Selecteer het abonnement in de **Azure-abonnement** vervolgkeuzelijst met box. (Als het abonnement niet wordt weergegeven, kiest u de **vernieuwen** knop naast de **beheren** koppeling.) 
   
   ![Azure PowerShell-build-taak configureren][8]
6. Geef een pad naar de implementeren AzureResourceGroup.ps1 PowerShell-script. Om dit te doen, kiest u de knop met het weglatingsteken (...) naast de **scriptpad** vak, gaat u naar de implementeren AzureResourceGroup.ps1 PowerShell-script in de **Scripts** map van uw project, selecteert u deze, en vervolgens Kies de **OK** knop.    
   
   ![Pad naar script selecteren][9]
7. Nadat u het script te selecteren, bijwerken, het pad naar het script zodat deze wordt uitgevoerd vanaf de Build.StagingDirectory (dezelfde map die *ArtifactsLocation* is ingesteld op). U kunt dit doen door toe te voegen "$(Build.StagingDirectory)/ 'aan het begin van het scriptpad.
   
    ![Pad naar script bewerken][10]
8. In de **scriptargumenten** voert u de volgende parameters (op één regel). Wanneer u het script in Visual Studio uitvoeren, kunt u zien hoe Visual Studio maakt gebruik van de parameters in de **uitvoer** venster. U kunt dit gebruiken als uitgangspunt voor het instellen van de parameterwaarden in uw build-stap.
   
   | Parameter | Beschrijving |
   | --- | --- |
   | -ResourceGroupLocation |De waarde van de geo-locatie waar de resourcegroep zich bevindt, zoals **eastus** of **'VS-Oost'**. (Enkele aanhalingstekens toevoegen als er een ruimte in de naam). Zie [Azure-regio's](https://azure.microsoft.com/regions/) voor meer informatie. |
   | -ResourceGroupName |De naam van de resourcegroep die is gebruikt voor deze implementatie. |
   | -UploadArtifacts |Deze parameter, indien aanwezig, geeft aan dat artefacten die moeten worden geüpload naar Azure uit het lokale systeem. U hoeft in te stellen deze switch als uw sjabloonimplementatie vereist extra artefacten die u wilt om te zetten met behulp van de PowerShell-script (zoals configuratiescripts of geneste sjablonen). |
   | -StorageAccountName |De naam van het opslagaccount dat wordt gebruikt voor de fase artefacten voor deze implementatie. Deze parameter wordt alleen gebruikt als u bij het Faseren van artefacten voor implementatie. Als deze parameter is opgegeven, wordt een nieuw opslagaccount gemaakt als het script niet tijdens een eerdere implementatie gemaakt is. Als de parameter is opgegeven, worden de storage-account moet al bestaan. |
   | -StorageAccountResourceGroupName |De naam van de resourcegroep die is gekoppeld aan de storage-account. Deze parameter is vereist, alleen als u een waarde voor de parameter StorageAccountName opgeven. |
   | -Sjabloonbestand |Het pad naar het sjabloonbestand in het implementatieproject voor Azure-resourcegroep. Ter verbetering van de flexibiliteit, moet u een pad gebruiken voor deze parameter die is gebaseerd op de locatie van de PowerShell-script in plaats van een absoluut pad zijn. |
   | -TemplateParametersFile |Het pad naar het parameterbestand in het implementatieproject voor Azure-resourcegroep. Ter verbetering van de flexibiliteit, moet u een pad gebruiken voor deze parameter die is gebaseerd op de locatie van de PowerShell-script in plaats van een absoluut pad zijn. |
   | -ArtifactStagingDirectory |Deze parameter kunt het PowerShell script weet in de map waar binaire bestanden van het project moeten worden gekopieerd. Deze waarde overschrijft de standaardwaarde gebruikt door het PowerShell-script. Voor Azure DevOps-Services gebruikt, stelt u de waarde in op: - ArtifactStagingDirectory $(Build.StagingDirectory) |
   
   Hier volgt een voorbeeld van de script-argumenten (regel uitgesplitst voor de leesbaarheid):
   
   ```    
   -ResourceGroupName 'MyGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\azuredeploy.json' 
   -TemplateParametersFile '..\templates\azuredeploy.parameters.json' -UploadArtifacts -StorageAccountName 'mystorageacct' 
   –StorageAccountResourceGroupName 'Default-Storage-EastUS' -ArtifactStagingDirectory '$(Build.StagingDirectory)'    
   ```
   
   Wanneer u klaar bent, de **scriptargumenten** vak is vergelijkbaar met de volgende lijst:
   
   ![Scriptargumenten][11]
9. Nadat u alle vereiste items aan de Azure PowerShell-build-stap hebt toegevoegd, kiest u de **wachtrij** knop voor het bouwen van het project te bouwen. De **bouwen** scherm ziet u de uitvoer van het PowerShell-script.

### <a name="detailed-walkthrough-for-option-2"></a>Gedetailleerd overzicht voor optie 2
De volgende procedures helpen u bij de benodigde stappen voor het configureren van continue implementatie in Azure DevOps-Services met behulp van de ingebouwde taken.

1. Bewerk uw pijplijn voor het bouwen van Azure DevOps-Services om toe te voegen dat twee nieuwe stappen bouwen. Kies de build-pijplijn onder de **bouwen definities** categorie en kies vervolgens de **bewerken** koppeling.
   
   ![Build-definitie bewerken][12]
2. De nieuwe build-stappen toevoegen aan de build-pijplijn met de **build-stap toevoegen...** .
   
   ![Build-stap toevoegen][13]
3. Kies de **implementeren** taakcategorie, selecteer de **Azure bestandskopie** taak en kies vervolgens de **toevoegen** knop.
   
   ![Azure File Copy taak toevoegen][14]
4. Kies de **Azure Resource Group Deployment** taak, en kies vervolgens de **toevoegen** knop en vervolgens **sluiten** de **taak catalogus**.
   
   ![Azure Resource Group Deployment taak toevoegen][15]
5. Kies de **Azure bestandskopie** taak en vult u de waarden ervan.
   
   Als u een Azure-service-eindpunt dat is toegevoegd aan Azure DevOps-Services al hebt, kiest u het abonnement in de **Azure-abonnement** vervolgkeuzelijst met box. Als u een abonnement hebt, raadpleegt u [optie 1](#detailed-walkthrough-for-option-1) voor instructies over het instellen in Azure DevOps-Services.
   
   * Bron: Voer **$(Build.StagingDirectory)**
   * Type in Azure verbinding - Selecteer **Azure Resource Manager**
   * Azure RM-abonnement: Selecteer het abonnement voor het opslagaccount dat u gebruiken wilt de **Azure-abonnement** vervolgkeuzelijst met box. Als het abonnement niet wordt weergegeven, kiest u de **vernieuwen** knop naast de **beheren** koppeling.
   * Het doeltype - Selecteer **Azure Blob**
   * RM Storage-Account: Selecteer het opslagaccount dat u wilt gebruiken voor het Faseren van artefacten
   * Containernaam van de - Geef de naam van de container die u wilt gebruiken voor fasering; het kan een containernaam zijn geldig, maar gebruikt u een toegewezen aan deze build-pijplijn
   
   De waarden van uitvoer:
   
   * Opslagcontainer URI - Voer **artifactsLocation**
   * Storage-Container SAS-Token - Voer **artifactsLocationSasToken**
   
   ![Azure File Copy taak configureren][16]
6. Kies de **Azure Resource Group Deployment** build-stap en vul vervolgens de waarden ervan.
   
   * Type in Azure verbinding - Selecteer **Azure Resource Manager**
   * Azure RM-abonnement: Selecteer het abonnement voor de implementatie in de **Azure-abonnement** vervolgkeuzelijst met box. Dit is meestal hetzelfde abonnement gebruikt in de vorige stap
   * Actie - Selecteer **maken of bijwerken-resourcegroep**
   * Resourcegroep: Selecteer een resourcegroep of Voer de naam van een nieuwe resourcegroep voor de implementatie
   * Locatie: Selecteer de locatie voor de resourcegroep
   * Sjabloon: Geef het pad en de naam van de sjabloon moet worden geïmplementeerd voorafgaand **$(Build.StagingDirectory)**, bijvoorbeeld: **$(Build.StagingDirectory/DSC-CI/azuredeploy.json)**
   * Sjabloonparameters - Geef het pad en de naam van de parameters moet worden gebruikt: toevoeging **$(Build.StagingDirectory)**, bijvoorbeeld: **$(Build.StagingDirectory/DSC-CI/azuredeploy.parameters.json)**
   * Sjabloonparameters overschrijven - opgeven of kopieer en plak de volgende code:
     
     ```    
     -_artifactsLocation $(artifactsLocation) -_artifactsLocationSasToken (ConvertTo-SecureString -String "$(artifactsLocationSasToken)" -AsPlainText -Force)
     ```
     ![Azure-resourcegroep Implementatietaak configureren][17]
7. Nadat u alle vereiste items hebt toegevoegd, opslaan van de build-pijplijn en kies vervolgens **nieuwe build in de wachtrij** aan de bovenkant.

## <a name="next-steps"></a>Volgende stappen
Lezen [overzicht van Azure Resource Manager](azure-resource-manager/resource-group-overview.md) voor meer informatie over Azure Resource Manager en Azure-resourcegroepen.

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
