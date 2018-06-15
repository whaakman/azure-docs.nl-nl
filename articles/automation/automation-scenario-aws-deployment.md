---
title: Implementatie van een virtuele machine in Amazon Web Services automatiseren
description: In dit artikel laat zien hoe het gebruik van Azure Automation voor het automatiseren van het maken van een virtuele machine in Amazon Web Service
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 4f49adf006e8d55337220fad9ee84de65209880b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34193478"
---
# <a name="azure-automation-scenario---provision-an-aws-virtual-machine"></a>Azure Automation-scenario: een AWS-virtuele machine inrichten
In dit artikel leert u hoe u kunt gebruikmaken van Azure Automation om een virtuele machine inrichten in uw abonnement Amazon Web Service (AWS) en geef die VM een specifieke naam – AWS wordt naar verwezen als 'tagging' van de virtuele machine.

## <a name="prerequisites"></a>Vereisten
Voor de doeleinden van dit artikel moet u een Azure Automation-account en een AWS-abonnement hebben. Raadpleeg voor meer informatie over een Azure Automation-account instellen en configureren met de referenties van uw AWS-abonnement [verificatie met Amazon Web Services configureren](automation-config-aws-account.md). Dit account moet worden gemaakt of bijgewerkt met de referenties van uw AWS-abonnement voordat u doorgaat, omdat u verwijzen naar dit account in de onderstaande stappen.

## <a name="deploy-amazon-web-services-powershell-module"></a>PowerShell-Module voor Amazon Web Services implementeren
Uw VM-inrichting runbook maakt gebruik van de AWS-PowerShell-module voor het werk. De volgende stappen om de module toevoegen aan uw Automation-account dat is geconfigureerd met de referenties van uw AWS-abonnement uitvoeren.  

1. Open uw webbrowser en navigeer naar de [PowerShell Gallery](http://www.powershellgallery.com/packages/AWSPowerShell/) en klik op de **implementeren in Azure Automation-knop**.<br><br> ![AWS PS Module-Import](./media/automation-scenario-aws-deployment/powershell-gallery-download-awsmodule.png)
2. Gaat u naar de pagina Azure-aanmelding en na verificatie, u wordt doorgestuurd naar de Azure-portal en krijgt de volgende pagina:<br><br> ![Pagina van de Module importeren](./media/automation-scenario-aws-deployment/deploy-aws-powershell-module-parameters.png)
3. Selecteer het Automation-Account en klik op **OK** implementatie te starten.

   > [!NOTE]
   > Terwijl een PowerShell-module in Azure Automation importeert, het is ook het uitpakken van de cmdlets en deze activiteiten niet weergegeven worden tot de module is volledig voltooid importeren en bij het uitpakken van de cmdlets. Dit proces kan enkele minuten duren.  
   > <br>

1. Open uw Automation-account waarnaar wordt verwezen in stap 3 in de Azure-portal.
2. Klik op de **activa** tegel en klik op de **activa** deelvenster, selecteer de **Modules** tegel.
3. Op de **Modules** pagina ziet u de **AWSPowerShell** module in de lijst.

## <a name="create-aws-deploy-vm-runbook"></a>Maak AWS VM runbook implementeren
Zodra de AWS PowerShell-Module is geïmplementeerd, kunt u nu een runbook voor het automatiseren van de inrichting van een virtuele machine in AWS met een PowerShell-script schrijven. De volgende stappen laten zien hoe u gebruikmaken van systeemeigen PowerShell-script in Azure Automation.  

> [!NOTE]
> Voor meer opties en informatie met betrekking tot dit script, gaat u naar de [PowerShell Gallery](https://www.powershellgallery.com/packages/New-AwsVM/DisplayScript).
> 

1. Het PowerShell-script New-AwsVM downloaden van de PowerShell Gallery door een PowerShell-sessie openen en het volgende te typen:<br>
   ```
   Save-Script -Name New-AwsVM -Path <path>
   ```
   <br>
2. Open uw Automation-account van de Azure-portal en selecteer **Runbooks** onder de sectie **procesautomatisering** aan de linkerkant.  
3. Van de **Runbooks** pagina **een runbook toevoegen**.
4. Op de **een runbook toevoegen** deelvenster **snelle invoer** (een nieuw runbook maken).
5. Op de **Runbook** eigenschappendeelvenster, typ een naam in het vak naam voor uw runbook en van de **runbooktype** vervolgkeuzelijst Selecteer **PowerShell**, en klik vervolgens op **Maken**.<br><br> ![Deelvenster runbook maken](./media/automation-scenario-aws-deployment/runbook-quickcreate-properties.png)
6. Wanneer de pagina van de PowerShell-Runbook bewerken wordt weergegeven, kopiëren en plakken van het PowerShell-script in het runbook authoring canvas.<br><br> ![Runbook PowerShell-Script](./media/automation-scenario-aws-deployment/runbook-powershell-script.png)<br>
   
    > [!NOTE]
    > Let op het volgende bij het werken met het voorbeeld PowerShell-script:
    > 
    > * Het runbook bevat een aantal standaardwaarden voor parameters. Alle standaardwaarden evalueren en werk indien nodig.
    > * Als u uw AWS-referenties hebt opgeslagen als een referentieasset met anders dan de naam **AWScred**, moet u het script op regel 57 dienovereenkomstig aan bijwerken.  
    > * Als u werkt met de AWS CLI-opdrachten in PowerShell met name met dit voorbeeldrunbook kunt u de regio AWS moet opgeven. Anders mislukt de de cmdlets. Weergave AWS onderwerp [Geef AWS regio](http://docs.aws.amazon.com/powershell/latest/userguide/pstools-installing-specifying-region.html) in de AWS-hulpprogramma's voor PowerShell-document voor meer informatie.  
    >

7. Voor het ophalen van een lijst met namen van de installatiekopie via uw AWS-abonnement, start PowerShell ISE en importeer de PowerShell-Module (AWS). Verificatie uitvoeren tegen AWS door te vervangen **Get-AutomationPSCredential** in uw omgeving ISE met **AWScred = Get-Credential**. Dit wordt u gevraagd uw referenties en u kunt uw **toegangssleutel-ID** voor de gebruikersnaam en **geheime toegangssleutel** voor het wachtwoord. Zie het voorbeeld hieronder:  

        #Sample to get the AWS VM available images
        #Please provide the path where you have downloaded the AWS PowerShell module
        Import-Module AWSPowerShell
        $AwsRegion = "us-west-2"
        $AwsCred = Get-Credential
        $AwsAccessKeyId = $AwsCred.UserName
        $AwsSecretKey = $AwsCred.GetNetworkCredential().Password
   
        # Set up the environment to access AWS
        Set-AwsCredentials -AccessKey $AwsAccessKeyId -SecretKey $AwsSecretKey -StoreAs AWSProfile
        Set-DefaultAWSRegion -Region $AwsRegion
   
        Get-EC2ImageByName -ProfileName AWSProfile

    De volgende uitvoer wordt geretourneerd:<br><br>
   ![AWS-installatiekopieën ophalen](./media/automation-scenario-aws-deployment/powershell-ise-output.png)<br>  
8. Kopieer en plak een van de namen van de installatiekopie in een Automation-variabele waarnaar wordt verwezen in het runbook als **$InstanceType**. Omdat in dit voorbeeld zijn met behulp van de gratis AWS gelaagde abonnement, gebruikt u **t2.micro** bijvoorbeeld uw runbook.  
9. Sla het runbook en klik vervolgens op **publiceren** om het runbook te publiceren en vervolgens **Ja** wanneer u wordt gevraagd.

### <a name="testing-the-aws-vm-runbook"></a>Testen van het runbook AWS VM
Voordat u doorgaat met het runbook testen, moet u een aantal zaken controleren. Specifiek:  

* Een actief voor het verifiëren met AWS is aangemaakt aangeroepen **AWScred** of het script is bijgewerkt om te verwijzen naar de naam van de referentie-element.    
* De AWS-PowerShell-module is geïmporteerd in Azure Automation  
* Een nieuw runbook gemaakt en de parameterwaarden zijn gecontroleerd en bijgewerkt indien nodig  
* **Logboekregistratie van uitgebreide records** en optioneel **logboekregistratie van voortgangsrecords** onder de runbook-instelling **logboekregistratie en tracering** zijn ingesteld op **op**.<br><br> ![Runbook-logboekregistratie en tracering](./media/automation-scenario-aws-deployment/runbook-settings-logging-and-tracing.png)  

1. U wilt het runbook wordt gestart, dus klik op **Start** en klik vervolgens op **OK** wanneer het deelvenster Runbook starten wordt geopend.
2. Geef in het deelvenster Runbook starten een **VMname**. Accepteer de standaardwaarden voor de parameters die u eerder een vooraf in het script geconfigureerd. Klik op **OK** de runbooktaak starten.<br><br> ![Nieuw AwsVM runbook starten](./media/automation-scenario-aws-deployment/runbook-start-job-parameters.png)
3. Een taakdeelvenster wordt geopend voor de runbooktaak die u hebt gemaakt. Sluit dit venster.
4. U kunt de voortgang van de uitvoer van de taak en de weergave bekijken **Streams** door te selecteren de **alle logboeken** tegel van de pagina van de taak runbook.<br><br> ![Stroomuitvoer](./media/automation-scenario-aws-deployment/runbook-job-streams-output.png)
5. Om te bevestigen op dat de virtuele machine wordt ingericht, meld u aan bij de AWS-beheerconsole als u momenteel niet is aangemeld.<br><br> ![AWS-console geïmplementeerd VM](./media/automation-scenario-aws-deployment/aws-instances-status.png)

## <a name="next-steps"></a>Volgende stappen
* Zie [Mijn eerste grafische runbook](automation-first-runbook-graphical.md) om aan de slag te gaan met grafische runbooks
* Zie [Mijn eerste PowerShell Workflow-runbook](automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks
* Zie [Azure Automation-runbooktypen](automation-runbook-types.md) voor meer informatie over runbooktypen, hun voordelen en beperkingen
* Zie [Systeemeigen PowerShell-scriptondersteuning in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/) voor meer informatie over de functie voor PowerShelll-scriptondersteuning

