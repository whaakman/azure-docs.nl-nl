---
title: Implementatie van een virtuele machine in Amazon Web Services automatiseren
description: In dit artikel ziet u hoe u Azure Automation gebruiken voor het maken van een virtuele machine van Amazon Web Service automatiseren
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 348c28f6a2d72048e34f117e802abf243597b458
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54425217"
---
# <a name="azure-automation-scenario---provision-an-aws-virtual-machine"></a>Azure Automation-scenario: een AWS-machine inrichten
In dit artikel leert u hoe u kunt gebruikmaken van Azure Automation voor het inrichten van een virtuele machine in uw abonnement Amazon Web Service (AWS) en geef die virtuele machine een specifieke naam – AWS verwijst naar als 'codering' van de virtuele machine.

## <a name="prerequisites"></a>Vereisten
Voor de doeleinden van dit artikel moet u een Azure Automation-account en een AWS-abonnement hebt. Raadpleeg voor meer informatie over het instellen van een Azure Automation-account en configureren met de referenties voor uw AWS-abonnement, [verificatie configureren met Amazon Web Services](automation-config-aws-account.md). Dit account moet worden gemaakt of bijgewerkt met de referenties voor uw AWS-abonnement voordat u doorgaat, zoals u verwijzen naar dit account in de onderstaande stappen.

## <a name="deploy-amazon-web-services-powershell-module"></a>PowerShell-Module van Amazon Web Services implementeren
Uw runbook Inrichtingsstatus van VM's maakt gebruik van de AWS-PowerShell-module voor het werk doen. De volgende stappen uitvoeren om de module toevoegen aan uw Automation-account dat is geconfigureerd met de referenties voor uw AWS-abonnement.  

1. Open uw webbrowser en navigeer naar de [PowerShell Gallery](https://www.powershellgallery.com/packages/AWSPowerShell/) en klik op de **implementeren in Azure Automation-knop**.<br><br> ![AWS PS-Module importeren](./media/automation-scenario-aws-deployment/powershell-gallery-download-awsmodule.png)
2. Gaat u naar de aanmeldingspagina van Azure en na het verifiëren, u wordt doorgestuurd naar de Azure-portal en krijgt de volgende pagina:<br><br> ![Pagina van de Module importeren](./media/automation-scenario-aws-deployment/deploy-aws-powershell-module-parameters.png)
3. Selecteer het Automation-Account wilt gebruiken en op **OK** implementatie te starten.

   > [!NOTE]
   > Tijdens een PowerShell-module in Azure Automation importeert, het ook van de cmdlets extraheren is en worden deze activiteiten niet weergegeven tot de module is volledig voltooid importeren en extraheren van de cmdlets. Dit proces kan enkele minuten duren.  
   > <br>

1. Open uw Automation-account waarnaar wordt verwezen in stap 3 in de Azure-portal.
2. Klik op de **activa** tegel en klik op de **activa** venster de **Modules** tegel.
3. Op de **Modules** pagina, ziet u de **AWSPowerShell** module in de lijst.

## <a name="create-aws-deploy-vm-runbook"></a>Maak AWS VM runbook implementeren
Zodra de AWS-PowerShell-Module is geïmplementeerd, kunt u nu een runbook voor het automatiseren van de inrichting van een virtuele machine in AWS met behulp van een PowerShell-script schrijven. De volgende stappen laten zien hoe u gebruikmaken van systeemeigen PowerShell-script in Azure Automation.  

> [!NOTE]
> Voor meer opties en informatie over dit script, gaat u naar de [PowerShell Gallery](https://www.powershellgallery.com/packages/New-AwsVM/).
> 

1. Het PowerShell-script New-AwsVM downloaden vanuit de PowerShell Gallery door een PowerShell-sessie openen en het volgende te typen:<br>
   ```powershell
   Save-Script -Name New-AwsVM -Path <path>
   ```
   <br>
2. Open uw Automation-account van de Azure-portal en selecteer **Runbooks** onder de sectie **procesautomatisering** aan de linkerkant.  
3. Uit de **Runbooks** weergeeft, schakelt **een runbook toevoegen**.
4. Op de **toevoegen van een runbook** venster **snelle invoer** (een nieuw runbook maken).
5. Op de **Runbook** deelvenster met eigenschappen, typ een naam in het vak naam voor uw runbook en van de **runbooktype** vervolgkeuzelijst selecteren **PowerShell**, en klik vervolgens op **Maken**.<br><br> ![Deelvenster runbook maken](./media/automation-scenario-aws-deployment/runbook-quickcreate-properties.png)
6. Wanneer de pagina van de PowerShell-Runbook bewerken wordt weergegeven, kopieer en plak het PowerShell-script in het runbook canvas voor ontwerpen.<br><br> ![PowerShell-Script voor Runbook](./media/automation-scenario-aws-deployment/runbook-powershell-script.png)<br>
   
    > [!NOTE]
    > Let op het volgende bij het werken met het voorbeeld PowerShell-script:
    > 
    > * Het runbook bevat een aantal standaardwaarden voor parameters. Alle standaardwaarden evalueren en werk indien nodig.
    > * Als u uw AWS-referenties hebt opgeslagen als een referentie-element met de naam anders dan **AWScred**, moet u het script op regel 57 zodat deze overeenkomen met dienovereenkomstig bijwerken.  
    > * Als u werkt met de AWS-CLI-opdrachten in PowerShell, vooral bij dit voorbeeld van een runbook, moet u de AWS-regio opgeven. Anders mislukt de de cmdlets. Weergave AWS onderwerp [AWS regio opgeven](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-installing-specifying-region.html) in de AWS-hulpprogramma's voor PowerShell-document voor meer informatie.  
    >

7. PowerShell ISE starten om op te halen een lijst met namen van afbeeldingen uit uw AWS-abonnement, en importeer de PowerShell-Module van AWS. Verificatie aan de hand van AWS te vervangen **Get-AutomationPSCredential** in uw ISE-omgeving met **AWScred = Get-Credential**. Hiermee vraagt u om uw referenties en u krijgt uw **toegangssleutel-ID** voor de gebruikersnaam en **geheime toegangssleutel** voor het wachtwoord. Zie het voorbeeld hieronder:  

        ```powershell
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
        ```
        
    De volgende uitvoer wordt geretourneerd:<br><br>
   ![AWS-afbeeldingen ophalen](./media/automation-scenario-aws-deployment/powershell-ise-output.png)<br>  
8. Kopieer en plak een van de namen van afbeeldingen in een Automation-variabele waarnaar wordt verwezen in het runbook als **$InstanceType**. Omdat in dit voorbeeld zijn abonnement met behulp van de gratis AWS lagen, gebruikt u **t2.micro** bijvoorbeeld uw runbook.  
9. Sla het runbook en klik vervolgens op **publiceren** om het runbook te publiceren en vervolgens **Ja** wanneer hierom wordt gevraagd.

### <a name="testing-the-aws-vm-runbook"></a>Het AWS-VM-runbook getest.
Voordat u doorgaat met het runbook testen, moet u een aantal zaken controleren. Specifiek:  

* Een asset voor verificatie met AWS is gemaakt met de naam **AWScred** of het script is bijgewerkt om te verwijzen naar de naam van uw referentie-element.    
* De AWS-PowerShell-module is geïmporteerd in Azure Automation  
* Een nieuw runbook is gemaakt en parameterwaarden is geverifieerd en indien nodig bijgewerkt  
* **Logboekregistratie van uitgebreide records** en eventueel **logboekregistratie van voortgangsrecords** onder de instelling van de runbook **logboekregistratie en tracering** zijn ingesteld op **op**.<br><br> ![Runbook-logboekregistratie en tracering](./media/automation-scenario-aws-deployment/runbook-settings-logging-and-tracing.png)  

1. U wilt het runbook starten, dus klik op **Start** en klik vervolgens op **OK** wanneer het deelvenster Runbook starten wordt geopend.
2. Geef in het deelvenster Runbook starten een **VMname**. Accepteer de standaardwaarden voor de andere parameters die u eerder een vooraf in het script geconfigureerd. Klik op **OK** om de runbooktaak te starten.<br><br> ![New-AwsVM runbook starten](./media/automation-scenario-aws-deployment/runbook-start-job-parameters.png)
3. Een taakdeelvenster wordt geopend voor de runbooktaak die u hebt gemaakt. Dit deelvenster sluiten.
4. U kunt de voortgang van de taak en bekijk de uitvoer bekijken **Streams** door het selecteren van de **alle logboeken** tegel van de pagina van de runbook-taak.<br><br> ![Stream-uitvoer](./media/automation-scenario-aws-deployment/runbook-job-streams-output.png)
5. Om te bevestigen dat de virtuele machine wordt ingericht, meld u aan bij de AWS-beheerconsole als u momenteel niet bent aangemeld.<br><br> ![AWS-console VM geïmplementeerd](./media/automation-scenario-aws-deployment/aws-instances-status.png)

## <a name="next-steps"></a>Volgende stappen
* Zie [Mijn eerste grafische runbook](automation-first-runbook-graphical.md) om aan de slag te gaan met grafische runbooks
* Zie [Mijn eerste PowerShell Workflow-runbook](automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks
* Zie [Azure Automation-runbooktypen](automation-runbook-types.md) voor meer informatie over runbooktypen, hun voordelen en beperkingen
* Zie [Systeemeigen PowerShell-scriptondersteuning in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/) voor meer informatie over de functie voor PowerShelll-scriptondersteuning


