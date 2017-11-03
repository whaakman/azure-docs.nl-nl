---
title: Gebruik pakketopname voor proactieve netwerkbewaking met waarschuwingen en Azure Functions | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een waarschuwing geactiveerd pakketopname maakt met Azure-netwerk-Watcher
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 75e6e7c4-b3ba-4173-8815-b00d7d824e11
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 1b3da4d6e4593f3c71995ef9331fcea2d5b6ec19
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-packet-capture-for-proactive-network-monitoring-with-alerts-and-azure-functions"></a>Pakketopname voor proactieve netwerkbewaking met waarschuwingen en Azure Functions gebruiken

Netwerk-Watcher pakketopname maakt vastleggen sessies om bij te houden van verkeer van en naar virtuele machines. Het bestand vastleggen hebben een filter dat is gedefinieerd om bij te houden, alleen het verkeer dat u wilt bewaken. Deze gegevens worden vervolgens opgeslagen in een blob storage of lokaal op de gastmachine.

Deze mogelijkheid kan op afstand worden gestart vanuit andere automation-scenario's zoals Azure Functions. Pakketopname biedt u de mogelijkheid om uit te voeren proactieve opnamen op basis van gedefinieerde netwerk afwijkingen. Andere toepassingen zijn onder andere netwerkstatistieken, informatie ophalen over het netwerk beveiligingsrisico's en foutopsporing client-servercommunicaties verzamelen.

Resources die zijn geïmplementeerd in Azure 24/7 worden uitgevoerd. U en uw medewerkers kan niet de status van alle resources 24/7 actief bewaken. Wat gebeurt bijvoorbeeld als er een probleem optreedt op 2 uur?

Met behulp van de netwerk-Watcher, waarschuwingen en functies van binnen het Azure-ecosysteem, kunt u proactief reageren met de gegevens en hulpmiddelen voor het oplossen van problemen in uw netwerk.

![Scenario][scenario]

## <a name="prerequisites"></a>Vereisten

* De nieuwste versie van [Azure PowerShell](/powershell/azure/install-azurerm-ps).
* Een bestaand exemplaar van netwerk-Watcher. Als u nog geen hebt, [geen exemplaar maken van netwerk-Watcher](network-watcher-create.md).
* Een bestaande virtuele machine in dezelfde regio bevinden als de netwerk-Watcher met de [Windows extensie](../virtual-machines/windows/extensions-nwa.md) of [extensie van de virtuele machine Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="scenario"></a>Scenario

In dit voorbeeld wordt de virtuele machine verzendt TCP-segmenten meer dan normaal en u wilt worden gewaarschuwd. TCP-segmenten als voorbeeld hier worden gebruikt, maar u kunt meldingsvoorwaarde.

Wanneer u wordt gewaarschuwd, die u wilt ontvangen pakketniveau gegevens om te begrijpen waarom communicatie is toegenomen. U kunt vervolgens de stappen voor de virtuele machine terug naar normale communicatie nemen.

Dit scenario wordt ervan uitgegaan dat u een bestaand exemplaar van de netwerk-Watcher en een resourcegroep met een geldige virtuele machine hebt.

De volgende lijst bevat een overzicht van de werkstroom die uitgevoerd wordt:

1. Een waarschuwing wordt geactiveerd op de virtuele machine.
1. De waarschuwing aanroepen uw Azure-functie via een webhook.
1. Uw Azure-functie verwerkt de waarschuwing en wordt een netwerk-Watcher pakket capture-sessie gestart.
1. Het vastleggen van het pakket wordt uitgevoerd op de virtuele machine en verzamelt verkeer.
1. Het pakket capture-bestand wordt geüpload naar een opslagaccount voor controle en diagnose.

Als u wilt automatiseren, we maken en verbinding maken met een waarschuwing op onze VM voor het geval van het incident activeren. We ook maken een functie aan te roepen netwerk-Watcher.

Dit scenario doet het volgende:

* Hiermee maakt u een Azure-functie die een pakketopname begint.
* Een waarschuwingsregel maakt op een virtuele machine en configureert u de waarschuwingsregel voor het aanroepen van de Azure-functie.

## <a name="create-an-azure-function"></a>Een Azure-functie maken

De eerste stap is het maken van een Azure-functie voor het verwerken van de waarschuwing en een pakketopname maken.

1. In de [Azure-portal](https://portal.azure.com), selecteer **nieuw** > **Compute** > **functie-App**.

    ![Een functie-app maken][1-1]

2. Op de **functie-App** blade, voer de volgende waarden en selecteer vervolgens **OK** om de app te maken:

    |**Instelling** | **Waarde** | **Details** |
    |---|---|---|
    |**Naam van app**|PacketCaptureExample|De naam van de functie-app.|
    |**Abonnement**|[Uw abonnement] Het abonnement waarvoor u de functie-app maken.||
    |**Resourcegroep**|PacketCaptureRG|De resourcegroep bevat de functie-app.|
    |**Hosting-Plan**|Plan voor verbruik| Het type plan uw app functie gebruikt. Opties zijn verbruik of Azure App Service-abonnement. |
    |**Locatie**|VS - midden| De regio waarin u de functie-app maken.|
    |**Storage-Account**|{automatisch gegenereerde}| Het opslagaccount waarvoor Azure Functions voor opslagaccounts voor algemeen gebruik.|

3. Op de **PacketCaptureExample functie Apps** blade Selecteer **functies** > **aangepaste functie**  >  **+**.

4. Selecteer **HttpTrigger Powershell**, en voer vervolgens de resterende gegevens. Ten slotte selecteren voor het maken van de functie **maken**.

    |**Instelling** | **Waarde** | **Details** |
    |---|---|---|
    |**Scenario**|experimentele|Type van scenario|
    |**Een naam voor de functie opgeven**|AlertPacketCapturePowerShell|Naam van de functie|
    |**Machtigingsniveau**|Functie|Autorisatieniveau voor de functie|

![Voorbeeld van de functies][functions1]

> [!NOTE]
> De PowerShell-sjabloon is experimentele en heeft geen volledige ondersteuning.

Aanpassingen zijn vereist voor dit voorbeeld en worden beschreven in de volgende stappen.

### <a name="add-modules"></a>Modules toevoegen

Voor het gebruik van netwerk-Watcher PowerShell-cmdlets, de meest recente PowerShell-module naar de functie-app te uploaden.

1. Voer de volgende PowerShell-opdracht op uw lokale computer met de meest recente Azure PowerShell-modules geïnstalleerd:

    ```powershell
    (Get-Module AzureRM.Network).Path
    ```

    In dit voorbeeld kunt u het lokale pad naar uw Azure PowerShell-modules. Deze mappen worden gebruikt in een later stadium. De modules die worden gebruikt in dit scenario zijn:

    * AzureRM.Network

    * AzureRM.Profile

    * AzureRM.Resources

    ![PowerShell-mappen][functions5]

1. Selecteer **werken app-instellingen** > **Ga naar App Service Editor**.

    ![De functie app-instellingen][functions2]

1. Met de rechtermuisknop op de **AlertPacketCapturePowershell** map, en maak vervolgens een map met de naam **azuremodules**. 

4. Maak een submap voor elke module die u nodig hebt.

    ![Map en submappen][functions3]

    * AzureRM.Network

    * AzureRM.Profile

    * AzureRM.Resources

1. Met de rechtermuisknop op de **AzureRM.Network** submap en selecteer vervolgens **bestanden uploaden**. 

6. Ga naar uw Azure-modules. In de lokale **AzureRM.Network** map, selecteert u alle bestanden in de map. Selecteer vervolgens **OK**. 

7. Herhaal deze stappen voor **AzureRM.Profile** en **AzureRM.Resources**.

    ![Bestanden uploaden][functions6]

1. Nadat u klaar bent, elke map moet hebben tot de bestanden van de PowerShell-module van uw lokale computer.

    ![Bestanden met PowerShell][functions7]

### <a name="authentication"></a>Authentication

Voor het gebruik van de PowerShell-cmdlets, moet u verifiëren. U configureren verificatie in de functie-app. Als verificatie wilt configureren, moet u omgevingsvariabelen configureren en het bestand met een versleutelde sleutel uploaden naar de functie-app.

> [!NOTE]
> Dit scenario biedt slechts één voorbeeld van hoe u verificatie met Azure Functions implementeert. Er zijn andere manieren om dit te doen.

#### <a name="encrypted-credentials"></a>Versleutelde referenties

De volgende PowerShell-script maakt een sleutelbestand aangeroepen **PassEncryptKey.key**. Het bevat ook een versleutelde versie van het wachtwoord dat wordt meegeleverd. Dit wachtwoord wordt hetzelfde wachtwoord dat is gedefinieerd voor de Azure Active Directory-toepassing die wordt gebruikt voor verificatie.

```powershell
#Variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#Keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#Get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

In de App Service-Editor van de functie-app maken in een map met de naam **sleutels** onder **AlertPacketCapturePowerShell**. Upload de **PassEncryptKey.key** -bestand dat u hebt gemaakt in het vorige voorbeeld met PowerShell.

![Functies sleutel][functions8]

### <a name="retrieve-values-for-environment-variables"></a>Waarden voor omgevingsvariabelen ophalen

De laatste vereiste is voor het instellen van de omgevingsvariabelen die nodig zijn voor toegang tot de waarden voor verificatie. De volgende lijst bevat de omgevingsvariabelen die zijn gemaakt:

* AzureClientID

* AzureTenant

* AzureCredPassword


#### <a name="azureclientid"></a>AzureClientID

De client-ID is de ID van een toepassing in Azure Active Directory.

1. Als u geen al een toepassing te gebruiken, moet u het volgende voorbeeld voor het maken van een toepassing uitvoert.

    ```powershell
    $app = New-AzureRmADApplication -DisplayName "ExampleAutomationAccount_MF" -HomePage "https://exampleapp.com" -IdentifierUris "https://exampleapp1.com/ExampleFunctionsAccount" -Password "<same password as defined earlier>"
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    Start-Sleep 15
    New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $app.ApplicationId
    ```

   > [!NOTE]
   > Het wachtwoord dat u bij het maken van de toepassing moet hetzelfde wachtwoord dat u eerder hebt gemaakt bij het opslaan van het sleutelbestand.

1. Selecteer in de Azure-portal **abonnementen**. Selecteer het abonnement wilt gebruiken en selecteer vervolgens **toegangsbeheer (IAM)**.

    ![De IAM-functies][functions9]

1. Kies het account moet gebruiken en selecteer vervolgens **eigenschappen**. Kopieer de toepassings-ID.

    ![Functies toepassings-ID][functions10]

#### <a name="azuretenant"></a>AzureTenant

De tenant-ID verkrijgen door het uitvoeren van de volgende PowerShell-voorbeeld:

```powershell
(Get-AzureRmSubscription -SubscriptionName "<subscriptionName>").TenantId
```

#### <a name="azurecredpassword"></a>AzureCredPassword

De waarde van de omgevingsvariabele AzureCredPassword is de waarde die u via de volgende PowerShell-voorbeeld uitvoeren. In dit voorbeeld wordt hetzelfde account dat wordt weergegeven in de voorgaande **versleuteld referenties** sectie. De waarde die nodig is, is de uitvoer van de `$Encryptedpassword` variabele.  Dit is de service principal wachtwoord die u versleuteld met behulp van het PowerShell-script.

```powershell
#Variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#Keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#Get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

### <a name="store-the-environment-variables"></a>De omgevingsvariabelen opslaan

1. Ga naar de functie-app. Selecteer vervolgens **werken app-instellingen** > **app-instellingen configureren**.

    ![App-instellingen configureren][functions11]

1. De omgevingsvariabelen en hun waarden toevoegen aan de app-instellingen en selecteer vervolgens **opslaan**.

    ![App-instellingen][functions12]

### <a name="add-powershell-to-the-function"></a>PowerShell toevoegen aan de functie

Het is nu tijd om aanroepen voor netwerk-Watcher uit vanuit de Azure-functie. Afhankelijk van de vereisten, kan de implementatie van deze functie variëren. De algemene stroom van de code is echter als volgt:

1. Proces-invoerparameters.
2. Bestaande pakket query worden vastgelegd om te verifiëren limieten en omzetten van de naam een conflict veroorzaakt.
3. Maak een pakketopname met toepasselijke parameters.
4. Poll-pakket vastleggen periodiek totdat deze is voltooid.
5. De gebruiker waarschuwen dat de opnamesessie pakket voltooid is.

Het volgende voorbeeld is een PowerShell-code die kan worden gebruikt in de functie. Er zijn waarden die worden vervangen moeten voor **subscriptionId**, **resourceGroupName**, en **storageAccountName**.

```powershell
            #Import Azure PowerShell modules required to make calls to Network Watcher
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Profile\AzureRM.Profile.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Network\AzureRM.Network.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Resources\AzureRM.Resources.psd1" -Global

            #Process alert request body
            $requestBody = Get-Content $req -Raw | ConvertFrom-Json

            #Storage account ID to save captures in
            $storageaccountid = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}"

            #Packet capture vars
            $packetcapturename = "PSAzureFunction"
            $packetCaptureLimit = 10
            $packetCaptureDuration = 10

            #Credentials
            $tenant = $env:AzureTenant
            $pw = $env:AzureCredPassword
            $clientid = $env:AzureClientId
            $keypath = "D:\home\site\wwwroot\AlertPacketCapturePowerShell\keys\PassEncryptKey.key"

            #Authentication
            $secpassword = $pw | ConvertTo-SecureString -Key (Get-Content $keypath)
            $credential = New-Object System.Management.Automation.PSCredential ($clientid, $secpassword)
            Add-AzureRMAccount -ServicePrincipal -Tenant $tenant -Credential $credential #-WarningAction SilentlyContinue | out-null


            #Get the VM that fired the alert
            if($requestBody.context.resourceType -eq "Microsoft.Compute/virtualMachines")
            {
                Write-Output ("Subscription ID: {0}" -f $requestBody.context.subscriptionId)
                Write-Output ("Resource Group:  {0}" -f $requestBody.context.resourceGroupName)
                Write-Output ("Resource Name:  {0}" -f $requestBody.context.resourceName)
                Write-Output ("Resource Type:  {0}" -f $requestBody.context.resourceType)

                #Get the Network Watcher in the VM's region
                $nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $requestBody.context.resourceRegion}
                $networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

                #Get existing packetCaptures
                $packetCaptures = Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher

                #Remove existing packet capture created by the function (if it exists)
                $packetCaptures | %{if($_.Name -eq $packetCaptureName)
                { 
                    Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName $packetCaptureName
                }}

                #Initiate packet capture on the VM that fired the alert
                if ((Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher).Count -lt $packetCaptureLimit){
                    echo "Initiating Packet Capture"
                    New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $requestBody.context.resourceId -PacketCaptureName $packetCaptureName -StorageAccountId $storageaccountid -TimeLimitInSeconds $packetCaptureDuration
                    Out-File -Encoding Ascii -FilePath $res -inputObject "Packet Capture created on ${requestBody.context.resourceID}"
                }
            } 
 ``` 
#### <a name="retrieve-the-function-url"></a>De URL van de functie niet ophalen 
1. Nadat u de functie hebt gemaakt, configureert u de waarschuwing voor het aanroepen van de URL die is gekoppeld aan de functie. Als u deze waarde, de URL van de functie van de functie-app te kopiëren.

    ![De URL van de functie zoeken][functions13]

2. Kopieer de URL van de functie voor functie-app.

    ![De URL van de functie kopiëren][2]

Als u aangepaste eigenschappen in de nettolading van de webhook POST-aanvraag nodig hebt, raadpleegt u [een webhook configureren op een Azure metrische waarschuwing](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="configure-an-alert-on-a-vm"></a>Een waarschuwing te configureren op een virtuele machine

Waarschuwingen kunnen worden geconfigureerd om te personen melden wanneer specifieke metrische gegevens overschrijdt de drempelwaarde die toegewezen. In dit voorbeeld wordt de waarschuwing wordt op de TCP-segmenten die worden verzonden, maar de waarschuwing voor veel andere metrische gegevens kan worden geactiveerd. In dit voorbeeld wordt een waarschuwing geconfigureerd voor het aanroepen van een webhook voor het aanroepen van de functie.

### <a name="create-the-alert-rule"></a>De waarschuwingsregel maken

Ga naar een bestaande virtuele machine en vervolgens een waarschuwingsregel toevoegen. Meer gedetailleerde documentatie over het configureren van waarschuwingen kan worden gevonden op [waarschuwingen in de Azure-Monitor maken voor Azure-services - Azure-portal](../monitoring-and-diagnostics/insights-alerts-portal.md). Voer de volgende waarden in de **waarschuwingsregel** blade en selecteer vervolgens **OK**.

  |**Instelling** | **Waarde** | **Details** |
  |---|---|---|
  |**Naam**|TCP_Segments_Sent_Exceeded|Naam van de waarschuwingsregel.|
  |**Beschrijving**|TCP-segmenten verzonden drempelwaarde overschreden|De beschrijving van de waarschuwingsregel.||
  |**Gegevens**|TCP-segmenten die zijn verzonden| De meetwaarde moet worden gebruikt om de waarschuwing te activeren. |
  |**Voorwaarde**|Groter dan| De voorwaarde moet worden gebruikt bij het evalueren van de metrische gegevens.|
  |**Drempelwaarde**|100| De waarde van de metrische gegevens waarmee de waarschuwing wordt geactiveerd. Deze waarde moet worden ingesteld op een geldige waarde voor uw omgeving.|
  |**Periode**|In de afgelopen vijf minuten| Bepaalt de periode waarin er voor de drempelwaarde voor de metriek.|
  |**Webhook**|[webhook-URL van de functie-app]| De webhook-URL van de functie-app die is gemaakt in de vorige stappen.|

> [!NOTE]
> De TCP-segmenten metriek is niet standaard ingeschakeld. Meer informatie over het inschakelen van aanvullende gegevens in via [inschakelen bewaking en diagnostische gegevens](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).

## <a name="review-the-results"></a>De resultaten bekijken

Nadat de criteria voor de waarschuwing triggers wordt een pakketopname gemaakt. Ga naar de netwerk-Watcher en selecteer vervolgens **pakketopname**. Op deze pagina kunt u de koppeling pakket capture-bestand voor het downloaden van het pakket vastleggen.

![Weergave pakketopname][functions14]

Als het bestand vastleggen lokaal is opgeslagen, kunt u deze ophalen door de aanmelding bij de virtuele machine.

Zie voor instructies over het downloaden van bestanden van Azure storage-accounts [aan de slag met Azure Blob storage met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Een ander hulpprogramma kunt u is [Opslagverkenner](http://storageexplorer.com/).

Nadat het vastleggen is gedownload, kunt u deze bekijken met behulp van een hulpprogramma dat gelezen kan een **CAP** bestand. Hieronder volgen koppelingen naar twee van deze hulpprogramma's:

- [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx)
- [WireShark](https://www.wireshark.org/)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het bekijken op de opnamen pakket [pakket netwerkopname-analyse met Wireshark](network-watcher-deep-packet-inspection.md).


[1]: ./media/network-watcher-alert-triggered-packet-capture/figure1.png
[1-1]: ./media/network-watcher-alert-triggered-packet-capture/figure1-1.png
[2]: ./media/network-watcher-alert-triggered-packet-capture/figure2.png
[3]: ./media/network-watcher-alert-triggered-packet-capture/figure3.png
[functions1]:./media/network-watcher-alert-triggered-packet-capture/functions1.png
[functions2]:./media/network-watcher-alert-triggered-packet-capture/functions2.png
[functions3]:./media/network-watcher-alert-triggered-packet-capture/functions3.png
[functions4]:./media/network-watcher-alert-triggered-packet-capture/functions4.png
[functions5]:./media/network-watcher-alert-triggered-packet-capture/functions5.png
[functions6]:./media/network-watcher-alert-triggered-packet-capture/functions6.png
[functions7]:./media/network-watcher-alert-triggered-packet-capture/functions7.png
[functions8]:./media/network-watcher-alert-triggered-packet-capture/functions8.png
[functions9]:./media/network-watcher-alert-triggered-packet-capture/functions9.png
[functions10]:./media/network-watcher-alert-triggered-packet-capture/functions10.png
[functions11]:./media/network-watcher-alert-triggered-packet-capture/functions11.png
[functions12]:./media/network-watcher-alert-triggered-packet-capture/functions12.png
[functions13]:./media/network-watcher-alert-triggered-packet-capture/functions13.png
[functions14]:./media/network-watcher-alert-triggered-packet-capture/functions14.png
[scenario]:./media/network-watcher-alert-triggered-packet-capture/scenario.png
