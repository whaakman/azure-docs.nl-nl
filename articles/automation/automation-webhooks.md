---
title: Een Azure Automation-runbook beginnen met een webhook | Microsoft Docs
description: Een webhook waarmee een client een runbook in Azure Automation starten vanuit een aanroep van HTTP.  Dit artikel wordt beschreven voor het maken van een webhook en het aanroepen van een voor het starten van een runbook.
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 9b20237c-a593-4299-bbdc-35c47ee9e55d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: magoedte;bwren;sngun
ms.openlocfilehash: 03d1617eb64c48b6a90925ae76e1ab3ce0312ff1
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="starting-an-azure-automation-runbook-with-a-webhook"></a>Een Azure Automation-runbook beginnen met een webhook
Een *webhook* kunt u een bepaald runbook te starten in Azure Automation via één HTTP-aanvraag. Hierdoor kan externe services, zoals Visual Studio Team Services, GitHub, logboekanalyse van Microsoft Operations Management Suite of aangepaste toepassingen runbooks starten zonder het implementeren van een volledige oplossing met de Azure Automation-API.  
![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)

U kunt vergelijken met webhooks aan andere methoden van een runbook starten [een runbook starten in Azure Automation](automation-starting-a-runbook.md)

## <a name="details-of-a-webhook"></a>Details van een webhook
De volgende tabel beschrijft de eigenschappen die u voor een webhook configureren moet.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Naam |U kunt elke gewenste naam voor een webhook omdat dit geen toegang heeft tot de client opgeven.  Dit wordt alleen gebruikt voor u te identificeren van het runbook in Azure Automation. <br>  Als een best practice moet u de webhook geeft een naam die betrekking hebben op de client die wordt gebruikt. |
| URL |De URL van de webhook is het unieke adres waarmee een client wordt aangeroepen met een HTTP POST naar het runbook dat is gekoppeld aan de webhook starten.  Bij het maken van de webhook wordt automatisch gegenereerd.  U kunt een aangepaste URL niet opgeven. <br> <br>  De URL bevat een beveiligingstoken waarmee het runbook kan worden aangeroepen door een systeem van derden zonder verdere verificatie. Daarom moet het worden behandeld als een wachtwoord.  Uit veiligheidsoverwegingen kunt u alleen de URL in de Azure portal weergeven op het moment dat de webhook is gemaakt. Houd er rekening mee de URL in een veilige locatie voor toekomstig gebruik. |
| Verloopdatum |Zoals een certificaat heeft elke webhook een vervaldatum op dat moment kan niet meer worden gebruikt.  Deze vervaldatum kan worden gewijzigd nadat de webhook is gemaakt. |
| Ingeschakeld |Een webhook is standaard ingeschakeld wanneer deze wordt gemaakt.  Als u deze op uitgeschakeld, instellen wordt er geen client kunnen gebruiken.  U kunt instellen de **ingeschakeld** eigenschap bij het maken van de webhook of op elk gewenst moment eenmaal is gemaakt. |

### <a name="parameters"></a>Parameters
Een webhook kunt waarden voor runbookparameters die worden gebruikt wanneer het runbook wordt gestart door die webhook definiëren. De webhook waarden voor de verplichte parameters van het runbook moet bevatten en waarden voor de volgende optionele parameters kan bevatten. Een parameterwaarde die is geconfigureerd voor een webhook kan zelfs na het maken van de webhook worden gewijzigd. Meerdere webhooks die zijn gekoppeld aan één runbook kunt elke andere parameterwaarden gebruiken.

Wanneer een client wordt gestart van een runbook met behulp van een webhook, kan zij de parameterwaarden die zijn gedefinieerd in de webhook niet overschrijven.  Om gegevens te ontvangen van de client, kan het runbook aangeroepen één parameter accepteren. **$WebhookData** van het type [object] die gegevens bevatten die de client in de POST-aanvraag bevat.

![Webhookdata-eigenschappen](media/automation-webhooks/webhook-data-properties.png)

De **$WebhookData** object heeft de volgende eigenschappen:

| Eigenschap | Beschrijving |
|:--- |:--- |
| WebhookName |De naam van de webhook. |
| RequestHeader |Hash-tabel met de headers van de binnenkomende POST-aanvraag. |
| requestBody |De hoofdtekst van de binnenkomende POST-aanvraag.  Hiermee behoudt alle opmaak zoals tekenreeks, JSON, XML, of formulier gecodeerde gegevens. Het runbook moet zijn geschreven met de indeling van gegevens die naar verwachting werkt. |

Er is geen configuratie van de webhook vereist ter ondersteuning van de **$WebhookData** parameter en het runbook is niet vereist te accepteren.  Als het runbook wordt niet gedefinieerd voor de parameter, worden er details van de aanvraag is verzonden vanaf de client genegeerd.

Als u een waarde voor $WebhookData opgeven bij het maken van de webhook die waarde wordt overschreven wanneer u de webhook start het runbook met de gegevens van de client POST-aanvraag, zelfs als de client geen gegevens in de aanvraagtekst bevatten.  Als u een runbook met een andere methode dan een webhook met $WebhookData start, kunt u een waarde opgeven voor $Webhookdata die wordt herkend door het runbook.  Deze waarde moet een object met dezelfde [eigenschappen](#details-of-a-webhook) als $Webhookdata zodat het runbook correct ermee werken kunt alsof het was in combinatie met de werkelijke WebhookData doorgegeven door een webhook.

Bijvoorbeeld, als u de volgende runbook vanuit de Azure-Portal starten zijn en doorgeven aantal voorbeelden WebhookData wilt voor testdoeleinden, aangezien WebhookData een object is, moet deze worden doorgegeven als JSON in de gebruikersinterface.

![De parameter WebhookData door de gebruikersinterface](media/automation-webhooks/WebhookData-parameter-from-UI.png)

Voor het bovenstaande runbook als u de volgende eigenschappen voor de parameter WebhookData hebt:

1. WebhookName: *MyWebhook*
2. RequestHeader: *van de testgebruiker =*
3. RequestBody: *['VM1', 'VM2']*

Vervolgens wilt u de volgende JSON-waarde in de gebruikersinterface voor de WebhookData-parameter doorgeven:  

* {'WebhookName': 'MyWebhook', "RequestHeader": {'Van': 'Test gebruiker'}, "RequestBody": "[\"VM1\",\"VM2\"]"}

![Parameter WebhookData starten door de gebruikersinterface](media/automation-webhooks/Start-WebhookData-parameter-from-UI.png)

> [!NOTE]
> De waarden van alle invoerparameters worden geregistreerd met de runbooktaak.  Dit betekent dat een opgegeven door de client in de aanvraag webhook invoer worden vastgelegd en toegankelijk voor iedereen met toegang tot de automation-taak.  Daarom moet u voorzichtig met gevoelige informatie in de webhook aanroepen.
>

## <a name="security"></a>Beveiliging
De beveiliging van een webhook is afhankelijk van de privacy van de URL die een beveiligingstoken dat toestaat dat deze bevat kan worden aangeroepen. Azure Automation biedt verificatie niet uitvoeren op de aanvraag zo lang wordt gemaakt aan de juiste URL. Om deze reden moet webhooks niet worden gebruikt voor runbooks die uiterst gevoelige functies uitvoeren zonder gebruik van een alternatieve methode voor de aanvraag wordt gevalideerd.

U kunt opnemen logica binnen het runbook om te bepalen of deze door een webhook is aangeroepen door het controleren van de **WebhookName** eigenschap van de parameter $WebhookData. Het runbook kan verder validatie uitvoeren door te zoeken naar specifieke informatie in de **RequestHeader** of **RequestBody** eigenschappen.

Een andere strategie is om het runbook sommige validatie van een externe voorwaarde uitvoeren wanneer het een webhook-aanvraag ontvangen.  Neem bijvoorbeeld een runbook dat wordt aangeroepen door GitHub wanneer er een nieuwe doorvoeren naar een GitHub-opslagplaats.  Het runbook mogelijk verbinding met GitHub te valideren dat een nieuwe doorvoer eigenlijk gewoon voordat u doorgaat opgetreden.

## <a name="creating-a-webhook"></a>Maken van een webhook
Gebruik de volgende procedure voor het maken van een nieuwe webhook gekoppeld aan een runbook in de Azure-portal.

1. Van de **Runbooks pagina** in de Azure-portal klikt u op het runbook waarmee de webhook wordt gestart om de detailpagina ervan weer te geven.
2. Klik op **Webhook** boven aan de pagina opent de **Webhook toevoegen** pagina. <br>
   ![Knop webhooks.](media/automation-webhooks/webhooks-button.png)
3. Klik op **maken van nieuwe webhook** openen de **pagina van de webhook maken**.
4. Geef een **naam**, **vervaldatum** voor de webhook en Hiermee wordt aangegeven of moet worden ingeschakeld. Zie [Details van een webhook](#details-of-a-webhook) voor meer informatie deze eigenschappen.
5. Klik op het pictogram kopiëren en druk op Ctrl + C om de URL van de webhook kopiëren.  Noteer de op een veilige plaats.  **Als u de webhook gemaakt, kan u de URL opnieuw niet ophalen.** <br>
   ![Webhook-URL](media/automation-webhooks/copy-webhook-url.png)
6. Klik op **Parameters** waarden opgeven voor de runbookparameters.  Als het runbook verplichte parameters heeft, klikt zich u niet kunnen maken van de webhook tenzij waarden zijn opgegeven.
7. Klik op **maken** voor het maken van de webhook.

## <a name="using-a-webhook"></a>Met behulp van een webhook
Voor het gebruik van een webhook nadat deze is gemaakt, moet u de clienttoepassing een HTTP POST door de URL van de webhook verlenen.  De syntaxis van de webhook bevindt zich in de volgende indeling.

    http://<Webhook Server>/token?=<Token Value>

De client ontvangt een van de volgende retourcodes van de POST-aanvraag.  

| Code | Tekst | Beschrijving |
|:--- |:--- |:--- |
| 202 |Geaccepteerd |De aanvraag is geaccepteerd en het runbook is in de wachtrij geplaatst. |
| 400 |Ongeldig verzoek |De aanvraag is niet geaccepteerd voor een van de volgende oorzaken hebben. <ul> <li>De webhook is verlopen.</li> <li>De webhook is uitgeschakeld.</li> <li>Het token in de URL is ongeldig.</li>  </ul> |
| 404 |Niet gevonden |De aanvraag is niet geaccepteerd voor een van de volgende oorzaken hebben. <ul> <li>De webhook is niet gevonden.</li> <li>Het runbook is niet gevonden.</li> <li>Het account is niet gevonden.</li>  </ul> |
| 500 |Interne serverfout |De URL is geldig, maar er is een fout opgetreden.  Verzend de aanvraag. |

Ervan uitgaande dat de aanvraag is voltooid, bevat het antwoord van de webhook de taak-id in JSON-indeling als volgt. Bevat een enkele taak-id, maar de JSON-indeling kunt u mogelijke toekomstige verbeteringen.

    {"JobIds":["<JobId>"]}  

De client kan wanneer de runbooktaak is voltooid of de voltooiingsstatus ervan van de webhook niet vaststellen.  Dit kan bepalen dat deze gegevens met behulp van de taak-id met een andere methode, zoals [Windows PowerShell](http://msdn.microsoft.com/library/azure/dn690263.aspx) of de [Azure Automation-API](https://msdn.microsoft.com/library/azure/mt163826.aspx).

### <a name="example"></a>Voorbeeld
Windows PowerShell wordt het volgende voorbeeld een runbook starten met een webhook.  Houd er rekening mee dat elke taal die u van een HTTP-aanvraag maken kunt een webhook; kunt gebruiken Windows PowerShell wordt alleen gebruikt als voorbeeld hier.

Het runbook is er een lijst met virtuele machines die zijn opgemaakt in JSON in de hoofdtekst van de aanvraag wordt verwacht. We zijn ook informatie over die wordt gestart, het runbook en de datum en tijd die wordt gestart in de koptekst van de aanvraag inclusief.      

    $uri = "https://s1events.azure-automation.net/webhooks?token=8ud0dSrSo%2fvHWpYbklW%3c8s0GrOKJZ9Nr7zqcS%2bIQr4c%3d"
    $headers = @{"From"="user@contoso.com";"Date"="05/28/2015 15:47:00"}

    $vms  = @(
                @{ Name="vm01";ServiceName="vm01"},
                @{ Name="vm02";ServiceName="vm02"}
            )
    $body = ConvertTo-Json -InputObject $vms

    $response = Invoke-RestMethod -Method Post -Uri $uri -Headers $headers -Body $body
    $jobid = ConvertFrom-Json $response


De volgende afbeelding toont de berichtkopinformatie (met behulp van een [Fiddler](http://www.telerik.com/fiddler) trace) van deze aanvraag. Dit omvat standaard headers van een HTTP-aanvraag naast de aangepaste datum en van de headers die we hebben toegevoegd.  Elk van deze waarden is beschikbaar voor het runbook in de **RequestHeaders** eigenschap van **WebhookData**.

![Knop webhooks.](media/automation-webhooks/webhook-request-headers.png)

De volgende afbeelding toont de hoofdtekst van de aanvraag (met behulp van een [Fiddler](http://www.telerik.com/fiddler) trace) dat beschikbaar is voor het runbook in de **RequestBody** eigenschap van **WebhookData**. Dit is opgemaakt als JSON omdat die de indeling die is opgenomen in de hoofdtekst van de aanvraag is.     

![Knop webhooks.](media/automation-webhooks/webhook-request-body.png)

De volgende afbeelding toont de aanvraag worden verzonden vanaf de Windows PowerShell en het resulterende antwoord.  De taak-id is opgehaald uit het antwoord en geconverteerd naar een tekenreeks.

![Knop webhooks.](media/automation-webhooks/webhook-request-response.png)

Het volgende voorbeeldrunbook accepteert van het vorige voorbeeldaanvraag en start de virtuele machines dat is opgegeven in de aanvraagtekst.

    workflow Test-StartVirtualMachinesFromWebhook
    {
        param (
            [object]$WebhookData
        )

        # If runbook was called from Webhook, WebhookData will not be null.
        if ($WebhookData -ne $null) {

            # Collect properties of WebhookData
            $WebhookName     =     $WebhookData.WebhookName
            $WebhookHeaders =     $WebhookData.RequestHeader
            $WebhookBody     =     $WebhookData.RequestBody

            # Collect individual headers. VMList converted from JSON.
            $From = $WebhookHeaders.From
            $VMList = ConvertFrom-Json -InputObject $WebhookBody
            Write-Output "Runbook started from webhook $WebhookName by $From."

            # Authenticate to Azure resources
            $Cred = Get-AutomationPSCredential -Name 'MyAzureCredential'
            Add-AzureAccount -Credential $Cred

            # Start each virtual machine
            foreach ($VM in $VMList)
            {
                $VMName = $VM.Name
                Write-Output "Starting $VMName"
                Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName
            }
        }
        else {
            Write-Error "Runbook mean to be started only from webhook."
        }
    }


## <a name="starting-runbooks-in-response-to-azure-alerts"></a>Starten van runbooks in reactie op waarschuwingen van Azure
Webhook ingeschakeld runbooks kunnen worden gebruikt om te reageren op [waarschuwingen van Azure](../monitoring-and-diagnostics/insights-receive-alert-notifications.md). Bronnen in Azure worden gecontroleerd door het verzamelen van de statistieken zoals prestaties, beschikbaarheid en gebruik met behulp van waarschuwingen van Azure. U kunt een waarschuwing op basis van de bewaking van metrische gegevens ontvangen of gebeurtenissen voor uw Azure-resources, momenteel Automation-Accounts ondersteunen alleen metrische gegevens. Wanneer de waarde van een opgegeven waarde groter is dan de drempelwaarde die is toegewezen of als de geconfigureerde gebeurtenis wordt geactiveerd en vervolgens een melding wordt verzonden naar de servicebeheerder of co-beheerders de waarschuwing oplossen, voor meer informatie over metrische gegevens en gebeurtenissen Raadpleeg [waarschuwingen van Azure](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

Naast het gebruik van waarschuwingen van Azure als een waarschuwingssysteem, kunt u ook ere van runbooks in reactie op waarschuwingen. Azure Automation biedt de mogelijkheid om uit te voeren webhook ingeschakeld runbooks met waarschuwingen van Azure. Wanneer een waarde groter is dan de geconfigureerde drempelwaarde vervolgens de waarschuwingsregel actief wordt en de automation-webhook die op zijn beurt wordt uitgevoerd het runbook wordt geactiveerd.

![Webhooks](media/automation-webhooks/webhook-alert.jpg)

### <a name="alert-context"></a>Waarschuwingscontext
Houd rekening met een Azure-resource zoals een virtuele machine, CPU-gebruik van deze machine is een van de prestatie-metriek. Als het CPU-gebruik is 100% of meer dan een bepaald bedrag gedurende lange tijd weergegeven, is het raadzaam om opnieuw te starten van de virtuele machine om het probleem te verhelpen. Dit kan worden opgelost door een waarschuwingsregel aan de virtuele machine configureren en deze regel gaat CPU-percentage als de metriek. Hier CPU-percentage wordt alleen gemaakt als voorbeeld, maar er zijn veel andere metrische gegevens die u voor uw Azure-resources configureren kunt en opnieuw starten van de virtuele machine is een actie die moet worden uitgevoerd om dit probleem te verhelpen, kunt u het runbook om andere acties te configureren.

Wanneer dit de waarschuwingsregel actief en activeert de runbook-webhook is ingeschakeld, verzendt het context van de waarschuwing aan het runbook. [Waarschuwingscontext](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) bevat details, zoals **SubscriptionID**, **ResourceGroupName**, **ResourceName**, **ResourceType**, **ResourceId** en **tijdstempel** die zijn vereist voor het runbook de resource waarop deze actie te identificeren. Waarschuwing context is ingesloten in de hoofdtekst van de **WebhookData** object verzonden naar het runbook en deze kan worden geactiveerd met **Webhook.RequestBody** eigenschap

### <a name="example"></a>Voorbeeld
Maken van een virtuele machine van Azure in uw abonnement en koppelen aan een [waarschuwing voor het bewaken van CPU-percentage metriek](../monitoring-and-diagnostics/insights-receive-alert-notifications.md). Zorg dat u het veld webhook met de URL van de webhook die is gegenereerd tijdens het maken van de webhook vullen tijdens het maken van de waarschuwing.

Het volgende voorbeeldrunbook wordt geactiveerd wanneer de waarschuwingsregel geactiveerd wordt en de context van de waarschuwing parameters die zijn vereist voor het runbook om te identificeren van de resource waarop deze actie te worden verzameld.

    workflow Invoke-RunbookUsingAlerts
    {
        param (      
            [object]$WebhookData
        )

        # If runbook was called from Webhook, WebhookData will not be null.
        if ($WebhookData -ne $null) {   
            # Collect properties of WebhookData.
            $WebhookName    =   $WebhookData.WebhookName
            $WebhookBody    =   $WebhookData.RequestBody
            $WebhookHeaders =   $WebhookData.RequestHeader

            # Outputs information on the webhook name that called This
            Write-Output "This runbook was started from webhook $WebhookName."


            # Obtain the WebhookBody containing the AlertContext
            $WebhookBody = (ConvertFrom-Json -InputObject $WebhookBody)
            Write-Output "`nWEBHOOK BODY"
            Write-Output "============="
            Write-Output $WebhookBody

            # Obtain the AlertContext     
            $AlertContext = [object]$WebhookBody.context

            # Some selected AlertContext information
            Write-Output "`nALERT CONTEXT DATA"
            Write-Output "==================="
            Write-Output $AlertContext.name
            Write-Output $AlertContext.subscriptionId
            Write-Output $AlertContext.resourceGroupName
            Write-Output $AlertContext.resourceName
            Write-Output $AlertContext.resourceType
            Write-Output $AlertContext.resourceId
            Write-Output $AlertContext.timestamp

            # Act on the AlertContext data, in our case restarting the VM.
            # Authenticate to your Azure subscription using Organization ID to be able to restart that Virtual Machine.
            $cred = Get-AutomationPSCredential -Name "MyAzureCredential"
            Add-AzureAccount -Credential $cred
            Select-AzureSubscription -subscriptionName "Visual Studio Ultimate with MSDN"

            #Check the status property of the VM
            Write-Output "Status of VM before taking action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
            Write-Output "Restarting VM"

            # Restart the VM by passing VM name and Service name which are same in this case
            Restart-AzureVM -ServiceName $AlertContext.resourceName -Name $AlertContext.resourceName
            Write-Output "Status of VM after alert is active and takes action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
        }
        else  
        {
            Write-Error "This runbook is meant to only be started from a webhook."  
        }  
    }



## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over verschillende manieren om een runbook te starten [een Runbook starten](automation-starting-a-runbook.md).
* Raadpleeg voor informatie over het weergeven van de Status van een Runbook-Job [uitvoeren van Runbook in Azure Automation](automation-runbook-execution.md).
* Zie voor informatie over het gebruik van Azure Automation actie ondernemen op Azure-waarschuwingen, [Azure VM-waarschuwingen oplossen met Automation-Runbooks](automation-azure-vm-alert-integration.md).
