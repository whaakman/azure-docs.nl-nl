---
title: Een Azure Automation-runbook beginnen met een webhook
description: Een webhook waarmee een client een runbook in Azure Automation starten vanuit een aanroep van HTTP.  Dit artikel wordt beschreven voor het maken van een webhook en het aanroepen van een voor het starten van een runbook.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 06/04/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: e047dffa86915b0cd6e8829ea27e0335e7f88cb2
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34757153"
---
# <a name="starting-an-azure-automation-runbook-with-a-webhook"></a>Een Azure Automation-runbook beginnen met een webhook

Een *webhook* kunt u een bepaald runbook te starten in Azure Automation via één HTTP-aanvraag. Hierdoor kan externe services, zoals Visual Studio Team Services, GitHub, Azure-logboekanalyse of aangepaste toepassingen runbooks starten zonder het implementeren van een volledige oplossing met de Azure Automation-API.  
![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)

U kunt vergelijken met webhooks aan andere methoden van een runbook starten [een runbook starten in Azure Automation](automation-starting-a-runbook.md)

## <a name="details-of-a-webhook"></a>Details van een webhook

De volgende tabel beschrijft de eigenschappen die u voor een webhook configureren moet.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Naam |U kunt elke gewenste naam voor een webhook omdat dit geen toegang heeft tot de client opgeven. Dit wordt alleen gebruikt voor u te identificeren van het runbook in Azure Automation. <br> Als een best practice moet u de webhook geeft een naam die betrekking hebben op de client die wordt gebruikt. |
| URL |De URL van de webhook is het unieke adres waarmee een client wordt aangeroepen met een HTTP POST naar het runbook dat is gekoppeld aan de webhook starten. Bij het maken van de webhook wordt automatisch gegenereerd. U kunt een aangepaste URL niet opgeven. <br> <br> De URL bevat een beveiligingstoken waarmee het runbook kan worden aangeroepen door een systeem van derden zonder verdere verificatie. Daarom moet het worden behandeld als een wachtwoord. Uit veiligheidsoverwegingen kunt u alleen de URL in de Azure portal weergeven op het moment dat de webhook is gemaakt. Let op de URL op een veilige locatie voor toekomstig gebruik. |
| Verloopdatum |Zoals een certificaat heeft elke webhook een vervaldatum op dat moment kan niet meer worden gebruikt. Deze vervaldatum kan worden gewijzigd nadat de webhook is gemaakt. |
| Ingeschakeld |Een webhook is standaard ingeschakeld wanneer deze wordt gemaakt. Als u deze op uitgeschakeld instellen, zijn geen client is kunnen gebruiken. U kunt instellen de **ingeschakeld** eigenschap bij het maken van de webhook of op elk gewenst moment eenmaal is gemaakt. |

### <a name="parameters"></a>Parameters

Een webhook kunt waarden voor runbookparameters die worden gebruikt wanneer het runbook wordt gestart door die webhook definiëren. De webhook waarden voor de verplichte parameters van het runbook moet bevatten en waarden voor de volgende optionele parameters kan bevatten. Een parameterwaarde die is geconfigureerd voor een webhook kan zelfs na het maken van de webhook worden gewijzigd. Meerdere webhooks die zijn gekoppeld aan één runbook kunt elke andere parameterwaarden gebruiken.

Wanneer een client wordt gestart van een runbook met behulp van een webhook, kan zij de parameterwaarden die zijn gedefinieerd in de webhook niet overschrijven. Om gegevens te ontvangen van de client, kan het runbook aangeroepen één parameter accepteren. **$WebhookData** van het type [object] die gegevens bevat die de client in de POST-aanvraag bevat.

![Webhookdata-eigenschappen](media/automation-webhooks/webhook-data-properties.png)

De **$WebhookData** object heeft de volgende eigenschappen:

| Eigenschap | Beschrijving |
|:--- |:--- |
| WebhookName |De naam van de webhook. |
| RequestHeader |Hash-tabel met de headers van de binnenkomende POST-aanvraag. |
| requestBody |De hoofdtekst van de binnenkomende POST-aanvraag. Hiermee behoudt alle opmaak zoals tekenreeks, JSON, XML, of formulier gecodeerde gegevens. Het runbook moet zijn geschreven met de indeling van gegevens die naar verwachting werkt. |

Er is geen configuratie van de webhook vereist ter ondersteuning van de **$WebhookData** parameter en het runbook is niet vereist te accepteren. Als het runbook wordt niet gedefinieerd voor de parameter, worden er details van de aanvraag is verzonden vanaf de client genegeerd.

Als u een waarde voor $WebhookData opgeven wanneer u de webhook maakt, dat wordt genegeerd wanneer u de webhook start het runbook met de gegevens van de client POST-aanvraag zelfs als de client geen gegevens in de aanvraagtekst bevatten. Als u een runbook met een andere methode dan een webhook met $WebhookData start, kunt u een waarde opgeven voor $Webhookdata die wordt herkend door het runbook. Deze waarde moet een object met dezelfde [eigenschappen](#details-of-a-webhook) als $Webhookdata zodat het runbook correct ermee werken kunt alsof het was in combinatie met de werkelijke WebhookData doorgegeven door een webhook.

Bijvoorbeeld, als u de volgende runbook vanuit de Azure-portal starten zijn en doorgeven aantal voorbeelden WebhookData wilt voor testdoeleinden, aangezien WebhookData een object is, moet deze worden doorgegeven als JSON in de gebruikersinterface.

![De parameter WebhookData door de gebruikersinterface](media/automation-webhooks/WebhookData-parameter-from-UI.png)

Voor de volgende runbook, hebt u de volgende eigenschappen voor de parameter WebhookData:

* WebhookName: *MyWebhook*
* RequestBody: *[{'ResourceGroup': 'myResourceGroup', 'Name': 'vm01'}, {'ResourceGroup': 'myResourceGroup', 'Name': 'vm02'}]*

Vervolgens wilt u de volgende JSON-waarde in de gebruikersinterface voor de parameter WebhookData doorgeven. Retourneert het volgende voorbeeld met het vervoer en regeltekens overeenkomt met de indeling die is doorgegeven vanuit een webhook.

```json
{"WebhookName":"mywebhook","RequestBody":"[\r\n {\r\n \"ResourceGroup\": \"vm01\",\r\n \"Name\": \"vm01\"\r\n },\r\n {\r\n \"ResourceGroup\": \"vm02\",\r\n \"Name\": \"vm02\"\r\n }\r\n]"}
```

![Parameter WebhookData starten door de gebruikersinterface](media/automation-webhooks/Start-WebhookData-parameter-from-UI.png)

> [!NOTE]
> De waarden van alle invoerparameters worden geregistreerd met de runbooktaak. Dit betekent dat een opgegeven door de client in de aanvraag webhook invoer worden vastgelegd en toegankelijk voor iedereen met toegang tot de automation-taak.  Daarom moet u voorzichtig met gevoelige informatie in de webhook aanroepen.

## <a name="security"></a>Beveiliging

De beveiliging van een webhook is afhankelijk van de privacy van de URL die een beveiligingstoken dat toestaat dat deze bevat kan worden aangeroepen. Azure Automation biedt verificatie niet uitvoeren op de aanvraag zo lang wordt gemaakt aan de juiste URL. Om deze reden moet webhooks niet worden gebruikt voor runbooks die uiterst gevoelige functies uitvoeren zonder gebruik van een alternatieve methode voor de aanvraag wordt gevalideerd.

U kunt opnemen logica binnen het runbook om te bepalen of deze door een webhook is aangeroepen door het controleren van de **WebhookName** eigenschap van de parameter $WebhookData. Het runbook kan verder validatie uitvoeren door te zoeken naar specifieke informatie in de **RequestHeader** of **RequestBody** eigenschappen.

Een andere strategie is om het runbook sommige validatie van een externe voorwaarde uitvoeren wanneer het een webhook-aanvraag ontvangen. Neem bijvoorbeeld een runbook dat wordt aangeroepen door GitHub wanneer er een nieuwe doorvoeren naar een GitHub-opslagplaats. Het runbook mogelijk verbinding met GitHub te valideren dat een nieuwe doorvoer voordat u doorgaat opgetreden.

## <a name="creating-a-webhook"></a>Maken van een webhook

Gebruik de volgende procedure voor het maken van een nieuwe webhook gekoppeld aan een runbook in de Azure-portal.

1. Van de **Runbooks pagina** in de Azure-portal klikt u op het runbook waarmee de webhook wordt gestart om de detailpagina ervan weer te geven.
1. Klik op **Webhook** boven aan de pagina opent de **Webhook toevoegen** pagina.
1. Klik op **maken van nieuwe webhook** openen de **pagina van de webhook maken**.
1. Geef een **naam**, **vervaldatum** voor de webhook en Hiermee wordt aangegeven of moet worden ingeschakeld. Zie [Details van een webhook](#details-of-a-webhook) voor meer informatie deze eigenschappen.
1. Klik op het pictogram kopiëren en druk op Ctrl + C om de URL van de webhook kopiëren. Noteer de op een veilige plaats. **Als u de webhook gemaakt, kan u de URL opnieuw niet ophalen.**

   ![Webhook-URL](media/automation-webhooks/copy-webhook-url.png)

1. Klik op **Parameters** waarden opgeven voor de runbookparameters. Als het runbook verplichte parameters heeft, klikt u vervolgens zich u niet voor het maken van de webhook tenzij waarden zijn opgegeven.
1. Klik op **maken** voor het maken van de webhook.

## <a name="using-a-webhook"></a>Met behulp van een webhook

Voor het gebruik van een webhook nadat deze is gemaakt, moet u de clienttoepassing een HTTP POST door de URL van de webhook verlenen. De syntaxis van de webhook is in de volgende indeling:

```http
http://<Webhook Server>/token?=<Token Value>
```

De client ontvangt een van de volgende retourcodes van de POST-aanvraag.

| Code | Tekst | Beschrijving |
|:--- |:--- |:--- |
| 202 |Geaccepteerd |De aanvraag is geaccepteerd en het runbook is in de wachtrij geplaatst. |
| 400 |Ongeldig verzoek |De aanvraag is niet geaccepteerd voor een van de volgende redenen: <ul> <li>De webhook is verlopen.</li> <li>De webhook is uitgeschakeld.</li> <li>Het token in de URL is ongeldig.</li>  </ul> |
| 404 |Niet gevonden |De aanvraag is niet geaccepteerd voor een van de volgende redenen: <ul> <li>De webhook is niet gevonden.</li> <li>Het runbook is niet gevonden.</li> <li>Het account is niet gevonden.</li>  </ul> |
| 500 |Interne serverfout |De URL is geldig, maar er is een fout opgetreden. Verzend de aanvraag. |

Ervan uitgaande dat de aanvraag is voltooid, bevat het antwoord van de webhook de taak-id in JSON-indeling als volgt. Bevat een enkele taak-id, maar de JSON-indeling kunt u mogelijke toekomstige verbeteringen.

```json
{"JobIds":["<JobId>"]}
```

De client kan wanneer de runbooktaak is voltooid of de voltooiingsstatus ervan van de webhook niet vaststellen. Dit kan bepalen dat deze gegevens met behulp van de taak-id met een andere methode, zoals [Windows PowerShell](http://msdn.microsoft.com/library/azure/dn690263.aspx) of de [Azure Automation-API](/rest/api/automation/job).

## <a name="sample-runbook"></a>Voorbeeldrunbook

Het volgende voorbeeldrunbook accepteert de webhook gegevens accepteert en start de virtuele machines dat is opgegeven in de aanvraagtekst. Dit runbook testen in uw Automation-Account onder **Runbooks**, klikt u op **+ een runbook toevoegen**. Als u niet hoe een runbook te maken weet, Zie [maken van een runbook](automation-quickstart-create-runbook.md).

```powershell
param
(
    [Parameter (Mandatory = $false)]
    [object] $WebhookData
)

# If runbook was called from Webhook, WebhookData will not be null.
if ($WebhookData) {

    # Retrieve VM's from Webhook request body
    $vms = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Authenticate to Azure by using the service principal and certificate. Then, set the subscription.

    Write-Output "Authenticating to Azure with service principal and certificate"
    $ConnectionAssetName = "AzureRunAsConnection"
    Write-Output "Get connection asset: $ConnectionAssetName" 

    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null)
            {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
            }
            Write-Output "Authenticating to Azure with service principal." 
            Add-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Output

        # Start each virtual machine
        foreach ($vm in $vms)
        {
            $vmName = $vm.Name
            Write-Output "Starting $vmName"
            Start-AzureRMVM -Name $vm.Name -ResourceGroup $vm.ResourceGroup
        }
}
else {
    # Error
    write-Error "This runbook is meant to be started from an Azure alert webhook only."
}
```

## <a name="test-the-example"></a>Voorbeeld testen

Windows PowerShell wordt het volgende voorbeeld een runbook starten met een webhook. Elke taal die u van een HTTP-aanvraag maken kunt kunt een webhook; Windows PowerShell wordt gebruikt als voorbeeld hier.

Het runbook is er een lijst met virtuele machines die zijn opgemaakt in JSON in de hoofdtekst van de aanvraag wordt verwacht.

```azurepowershell-interactive
$uri = "<webHook Uri>"

$vms  = @(
            @{ Name="vm01";ResourceGroup="vm01"},
            @{ Name="vm02";ResourceGroup="vm02"}
        )
$body = ConvertTo-Json -InputObject $vms

$response = Invoke-RestMethod -Method Post -Uri $uri -Body $body
$jobid = (ConvertFrom-Json ($response.Content)).jobids[0]
```

Het volgende voorbeeld ziet u de hoofdtekst van de aanvraag die beschikbaar is voor het runbook in de **RequestBody** eigenschap van **WebhookData**. Dit is opgemaakt als JSON omdat die de indeling die is opgenomen in de hoofdtekst van de aanvraag is.

```json
[
    {
        "Name":  "vm01",
        "ResourceGroup":  "myResourceGroup"
    },
    {
        "Name":  "vm02",
        "ResourceGroup":  "myResourceGroup"
    }
]
```

De volgende afbeelding toont de aanvraag worden verzonden vanaf de Windows PowerShell en het resulterende antwoord. De taak-id is opgehaald uit het antwoord en geconverteerd naar een tekenreeks.

![Knop webhooks.](media/automation-webhooks/webhook-request-response.png)

## <a name="next-steps"></a>Volgende stappen

* Zie voor informatie over het gebruik van Azure Automation actie ondernemen op Azure-waarschuwingen, [gebruik van een waarschuwing voor het activeren van een Azure Automation-runbook](automation-create-alert-triggered-runbook.md).
