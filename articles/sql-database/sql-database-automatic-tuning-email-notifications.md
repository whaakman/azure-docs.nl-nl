---
title: Automatische afstemming e-mailmeldingen how-to guide - Azure SQL Database | Microsoft Docs
description: Azure SQL Database SQL-query analyseert en automatisch wordt aangepast aan de werkbelasting van de gebruiker.
services: sql-database
documentationcenter: 
author: danimir
manager: drasumic
ms.reviewer: carlrab
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 02/05/2018
ms.author: v-daljep
ms.openlocfilehash: 611c30639b5fb36bb08ebd3e73c90f8aa2bd09d4
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="email-notifications-for-automatic-tuning"></a>E-mailmeldingen voor automatische afstemming

SQL-Database afstemmen aanbevelingen worden gegenereerd door Azure SQL Database [automatische afstemming](sql-database-automatic-tuning.md). Deze oplossing continu bewaakt en analyseert werkbelastingen van het SQL-Databases bieden aangepast aanbevelingen voor elke afzonderlijke database die betrekking hebben op het maken van een index, verwijderen van een index en optimalisatie van de uitvoering van queryplannen afstemmen.

SQL Database automatische afstemming van aanbevelingen kunnen worden weergegeven in de [Azure-portal](sql-database-advisor-portal.md), opgehaald met [REST-API](https://docs.microsoft.com/rest/api/sql/databaserecommendedactions/listbydatabaseadvisor) aanroept of met behulp van [T-SQL](https://azure.microsoft.com/blog/automatic-tuning-introduces-automatic-plan-correction-and-t-sql-management/) en [ PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabaserecommendedaction) opdrachten. In dit artikel is gebaseerd op een PowerShell-script gebruiken om op te halen van automatische afstemmen aanbevelingen.

## <a name="automate-email-notifications-for-automatic-tuning-recommendations"></a>E-mailmeldingen voor automatische afstemmen aanbevelingen automatiseren

De volgende oplossing automatiseert het verzenden van e-mailmeldingen met automatische afstemmen aanbevelingen. De oplossing beschreven bestaat uit het automatiseren van de uitvoering van een PowerShell-script voor het ophalen van afstemmen aanbevelingen via [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro), en automatisering van het plannen van e-levering met behulp van taak [Microsoft Flow ](https://flow.microsoft.com).

## <a name="create-azure-automation-account"></a>Azure Automation-account maken

Voor het gebruik van Azure Automation, is de eerste stap het maken van een automation-account en configureer dit met Azure-resources voor uitvoering van het PowerShell-script. Zie voor meer informatie over Azure Automation en de mogelijkheden ervan, [aan de slag met Azure automation](https://docs.microsoft.com/azure/automation/automation-offering-get-started).

Volg deze stappen voor het Azure Automation-Account maken met de methode te selecteren en configureren van Automation-app uit de Marketplace:

- Meld u aan bij de Azure-portal
- Klik op '**+ maken van een resource**' in de linkerbovenhoek
- Zoek naar '**Automation**' (druk op enter)
- Klik op de Automation-app in de zoekresultaten

![Toevoegen van Azure automation](./media/sql-database-automatic-tuning-email-notifications/howto-email-01.png)

- Eenmaal in het deelvenster 'Een Automation-Account maken' Klik op '**maken**'
- De vereiste informatie invullen: Voer een naam voor dit automation-account, selecteert u uw Azure-abonnement-ID en het Azure-resources moet worden gebruikt voor de uitvoering van het PowerShell-script
- Voor de '**Azure uitvoeren als-account maken**' optie, selecteer **Ja** voor het configureren van het type van welke PowerShell script wordt uitgevoerd met behulp van Azure Automation-account. Zie voor meer informatie over accounttypen, [Run As-account](https://docs.microsoft.com/azure/automation/automation-create-runas-account)
- Het maken van het automation-account sluiten door te klikken op **maken**

> [!TIP]
> Leg uw Azure Automation-accountnaam, de abonnements-ID en de bronnen (zoals kopiëren en plakken naar een Kladblok) precies zoals ingevoerd tijdens het maken van de Automation-app. U hebt deze informatie later nodig.
>

Als u verschillende Azure-abonnementen waarvoor u wilt maken van de dezelfde automatisering hebt, moet u dit proces herhalen voor uw andere abonnementen.

## <a name="update-azure-automation-modules"></a>Azure Automation-modules bijwerken

Maakt gebruik van het PowerShell-script voor het ophalen van automatische afstemming aanbeveling [Get-AzureRmResource](https://docs.microsoft.com/powershell/module/AzureRM.Resources/Get-AzureRmResource) en [Get-AzureRmSqlDatabaseRecommendedAction](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Get-AzureRmSqlDatabaseRecommendedAction) -opdrachten voor welke bijwerken van de Azure-Modules mogen worden de versie 4 en hoger is vereist.

Volg deze stappen voor het bijwerken van Azure PowerShell-modules:

- Toegang tot het deelvenster Automation-app en selecteer '**Modules**' in het menu links (Schuif naar beneden omdat dit menu-item onder gedeelde bronnen).
- Klik in het deelvenster met Modules op '**Update Azure Modules**' aan de boven- en wacht totdat het bericht 'Azure-modules zijn bijgewerkt' wordt weergegeven. Dit proces kan enkele minuten duren.

![Azure automation-modules bijwerken](./media/sql-database-automatic-tuning-email-notifications/howto-email-02.png)

Vereiste versie van AzureRM.Resources en AzureRM.Sql modules moet versie 4 en hoger.

## <a name="create-azure-automation-runbook"></a>Azure Automation-Runbook maken

De volgende stap is het maken van een Runbook in Azure Automation waarbinnen het PowerShell-script voor het ophalen van het afstemmen van aanbevelingen zich bevindt.

Volg deze stappen om een nieuw Azure Automation-runbook maken:

- Toegang tot de Azure Automation-account dat u in de vorige stap hebt gemaakt
- Eenmaal in het deelvenster automation-account, klikt u op de '**Runbooks**' menu-item aan de linkerkant een nieuw Azure Automation-runbook maken met het PowerShell-script. Zie voor meer informatie over het maken van automation-runbooks, [maken van een nieuw runbook](../automation/automation-creating-importing-runbook.md).
- Als u wilt een nieuw runbook toevoegen, klikt u op de '**+ een runbook toevoegen**' menuoptie en klik vervolgens op de '**snel maken: Maak een nieuw runbook**'.
- Typ in het deelvenster Runbook de naam van uw runbook (in dit voorbeeld '**AutomaticTuningEmailAutomation**' wordt gebruikt), selecteer het type runbook als **PowerShell** en een beschrijving van schrijven Dit runbook te beschrijven van het doel.
- Klik op de **maken** om een nieuw runbook maken voltooien

![Azure automation-runbook toevoegen](./media/sql-database-automatic-tuning-email-notifications/howto-email-03.png)

Volg deze stappen voor het laden van een PowerShell-script binnen het runbook dat is gemaakt:

- In de '**PowerShell-Runbook bewerken**'deelvenster Selecteer'**RUNBOOKS**' in het menu consolestructuur en vouw de weergave tot u de naam van uw runbook (in dit voorbeeld ' **AutomaticTuningEmailAutomation**'). Selecteer dit runbook.
- Op de eerste regel van de 'PowerShell-Runbook bewerken' (te beginnen met het nummer 1), kopiëren en plakken met de volgende code van de PowerShell-script. Deze PowerShell-script wordt geleverd als-moet u op weg. Wijzig het script Suite uw behoeften.

In de koptekst van het opgegeven PowerShell-script, moet u vervangen `<SUBSCRIPTION_ID_WITH_DATABASES>` met uw Azure-abonnement-ID. Zie voor informatie over het ophalen van uw Azure-abonnement-ID, [ophalen van uw Azure-abonnement-GUID](https://blogs.msdn.microsoft.com/mschray/2016/03/18/getting-your-azure-subscription-guid-new-portal/).

In geval van meerdere abonnementen kunt u ze als door komma's gescheiden voor de eigenschap '$subscriptions' in de koptekst van het script toevoegen.

```PowerShell
# PowerShell script to retrieve Azure SQL Database Automatic tuning recommendations.
#
# Provided “as-is” with no implied warranties or support.
# The script is released to the public domain.
#
# Replace <SUBSCRIPTION_ID_WITH_DATABASES> in the header with your Azure subscription ID.
#
# Microsoft Azure SQL Database team, 2018-01-22.

# Set subscriptions : IMPORTANT – REPLACE <SUBSCRIPTION_ID_WITH_DATABASES> WITH YOUR SUBSCRIPTION ID 
$subscriptions = ("<SUBSCRIPTION_ID_WITH_DATABASES>", "<SECOND_SUBSCRIPTION_ID_WITH_DATABASES>", "<THIRD_SUBSCRIPTION_ID_WITH_DATABASES>")

# Get credentials
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Define the resource types
$resourceTypes = ("Microsoft.Sql/servers/databases")
$advisors = ("CreateIndex", "DropIndex");
$results = @()

# Loop through all subscriptions
foreach($subscriptionId in $subscriptions) {    
    Select-AzureRmSubscription -SubscriptionId $subscriptionId    
    $rgs = Get-AzureRmResourceGroup

    # Loop through all resource groups
    foreach($rg in $rgs) {
        $rgname = $rg.ResourceGroupName;

        # Loop through all resource types
        foreach($resourceType in $resourceTypes) {
            $resources = Get-AzureRmResource -ResourceGroupName $rgname -ResourceType $resourceType    

            # Loop through all databases
            # Extract resource groups, servers and databases
            foreach ($resource in $resources) {
                $resourceId = $resource.ResourceId
                if ($resourceId -match ".*RESOURCEGROUPS/(?<content>.*)/PROVIDERS.*") {
                    $ResourceGroupName = $matches['content']
                } else {
                    continue
                }
                if ($resourceId -match ".*SERVERS/(?<content>.*)/DATABASES.*") {
                    $ServerName = $matches['content']
                } else {
                    continue
                }
                if ($resourceId -match ".*/DATABASES/(?<content>.*)") {
                    $DatabaseName = $matches['content']
                } else {
                    continue 
                }

                # Skip if master
                if ($DatabaseName -eq "master") {
                    continue
                }

                # Loop through all Automatic tuning recommendation types
                foreach ($advisor in $advisors) {
                    $recs = Get-AzureRmSqlDatabaseRecommendedAction -ResourceGroupName $ResourceGroupName -ServerName $ServerName  -DatabaseName $DatabaseName -AdvisorName $advisor
                    foreach ($r in $recs) {
                        if ($r.State.CurrentValue -eq "Active") {
                            $object = New-Object -TypeName PSObject
                            $object | Add-Member -Name 'SubscriptionId' -MemberType Noteproperty -Value $subscriptionId
                            $object | Add-Member -Name 'ResourceGroupName' -MemberType Noteproperty -Value $r.ResourceGroupName
                            $object | Add-Member -Name 'ServerName' -MemberType Noteproperty -Value $r.ServerName
                            $object | Add-Member -Name 'DatabaseName' -MemberType Noteproperty -Value $r.DatabaseName
                            $object | Add-Member -Name 'Script' -MemberType Noteproperty -Value $r.ImplementationDetails.Script
                            $results += $object
                        }
                    }
                }                
            }
        }
    }
}

# Format and output results for the email
$table = $results | Format-List
Write-Output $table
```

Klik op de '**opslaan**' knop in de rechterbovenhoek om op te slaan van het script. Wanneer u tevreden met het script bent, klikt u op de '**publiceren**' knop om dit runbook te publiceren. 

In het deelvenster belangrijkste runbook kunt u klikken op de '**Start**' knop **testen** het script. Klik op de '**uitvoer**' om te bekijken van resultaten van het script uitgevoerd. Deze uitvoer is het verstandig om de inhoud van uw e-mailadres. In de volgende schermafbeelding ziet u het voorbeeld van uitvoer van het script.

![Weergave automatische afstemming van aanbevelingen met Azure Automation worden uitgevoerd](./media/sql-database-automatic-tuning-email-notifications/howto-email-04.png)

Ervoor zorgen dat de inhoud door het aanpassen van het PowerShell-script op uw behoeften aan te passen.

Met de bovenstaande stappen, is het PowerShell-script voor het ophalen van automatische afstemmen aanbevelingen in Azure Automation geladen. De volgende stap is om te automatiseren en plannen van de taak van de levering van e-mail.

## <a name="automate-the-email-jobs-with-microsoft-flow"></a>De e-taken met Microsoft-Flow automatiseren

Voor het voltooien van de oplossing voor de laatste stap maakt u een automation-stroom in Microsoft Flow die bestaan uit drie acties (taken): 

1. "**Azure Automation - taak maken**': wordt gebruikt voor het uitvoeren van de PowerShell-script voor automatische afstemming van de aanbevelingen in Azure Automation-runbook ophalen
2. "**Azure Automation - Get-taakuitvoer**': wordt gebruikt voor het ophalen van de uitvoer van het uitgevoerde PowerShell-script
3. "**Outlook in office 365 – stuurt u een e-mailadres**': wordt gebruikt voor het verzenden van e-mail. E-mailberichten worden verzonden met behulp van de Office 365-account van de persoon die het maken van de stroom.

Zie voor meer informatie over de mogelijkheden van Microsoft-Flow, [aan de slag met Microsoft-Flow](https://docs.microsoft.com/flow/getting-started).

Vereiste voor deze stap is dat u zich aanmelden voor [Microsoft Flow](https://flow.microsoft.com) account en aan te melden. Eenmaal in de oplossing als volgt te werk voor het instellen van een **nieuwe stroom**:

- Toegang '**mijn stromen**' menu-item
- In mijn stromen, selecteer de '**+ maken van leeg**' koppeling aan de bovenkant van de pagina
- Klik op de koppeling '**zoekt honderden connectors en triggers**' aan de onderkant van de pagina
- In het veld Zoektype '**terugkeerpatroon**', en selecteer '**schema - terugkeerpatroon**' in de zoekresultaten de levering van e-mail uit te voeren taak plannen.
- Selecteer in het deelvenster terugkeerpatroon in het veld frequentie planning frequentie voor deze overdracht wilt uitvoeren, zoals send automated e-elke minuut, uur, dag, Week, enzovoort.

De volgende stap is drie taken (create, get-uitvoer en verzonden e-mail) toevoegen aan de zojuist gemaakte terugkerende stroom. Als u wilt uitvoeren op de vereiste taken toe te voegen aan de stroom, de volgende stappen uit:

1. Actie voor het uitvoeren van PowerShell-script voor het ophalen van afstemmen aanbevelingen maken
- Selecteer "**+ een nieuwe stap**', gevolgd door'**een actie toevoegen**' in het deelvenster van de stroom terugkeerpatroon
- In het veld Zoektype '**automation**'en selecteer'**Azure Automation-taak maken**' in de zoekresultaten
- Configureer de taakeigenschappen in het deelvenster van de taak maken. Voor deze configuratie moet u de details van uw Azure-abonnement-ID, resourcegroep en de Automation-Account **eerder opgenomen** op de **Automation-Account deelvenster**. Zie voor meer informatie over de opties die beschikbaar zijn in deze sectie, [Azure Automation - taak maken](https://docs.microsoft.com/connectors/azureautomation/#Create_job).
- Voer deze actie wordt gemaakt door te klikken op '**opslaan stroom**'

2. Actie voor het ophalen van de uitvoer van het uitgevoerde PowerShell-script maken
- Selecteer "**+ een nieuwe stap**', gevolgd door'**een actie toevoegen**' in het deelvenster van de stroom terugkeerpatroon
- In de zoekopdracht ingediend type '**automation**'en selecteer'**Azure Automation-Get-taakuitvoer**' in de zoekresultaten. Zie voor meer informatie over de opties die beschikbaar zijn in deze sectie, [Azure Automation-Get-taakuitvoer](https://docs.microsoft.com/connectors/azureautomation/#Get_job_output).
- Vullen velden vereist (vergelijkbaar met het maken van de vorige taak) - invullen van uw Azure-abonnement-ID, resourcegroep, en de Automation-Account (zoals opgegeven in het deelvenster Automation-Account)
- Klik in het veld '**taak-ID**' voor de '**dynamische inhoud**' menu worden weergegeven. In dit menu, selecteer de optie '**taak-ID**'.
- Voer deze actie wordt gemaakt door te klikken op '**opslaan stroom**'

3. Actie voor het verzenden van e-mail via Office 365-integratie maken
- Selecteer "**+ een nieuwe stap**', gevolgd door'**een actie toevoegen**' in het deelvenster van de stroom terugkeerpatroon
- In de zoekopdracht ingediend type '**e-mailbericht verzenden**'en selecteer'**Outlook in Office 365 – stuurt u een e-mailadres**' in de zoekresultaten
- In de '**naar**' veldtype in het e-mailadres waarnaar u moet de e-mailmelding verzenden
- In de '**onderwerp**' in het onderwerp van uw e-mailadres, bijvoorbeeld 'automatische afstemmen aanbevelingen e-mailmeldingen' veldtype
- Klik in het veld '**hoofdtekst**' voor de '**dynamische inhoud**' menu worden weergegeven. Uit binnen dit menu onder '**taak uitvoer**", selecteer"**inhoud**' 
- Voer deze actie wordt gemaakt door te klikken op '**opslaan stroom**'

> [!TIP]
> Geautomatiseerde om e-mails te verzenden naar andere ontvangers, maakt u afzonderlijke stromen. In deze aanvullende stromen de ontvanger van e-mailadres in het veld 'Aan' en de onderwerpregel van e-mailadres in het veld 'Onderwerp' te wijzigen. Maken van nieuwe runbooks in Azure Automation met PowerShell-scripts aangepast (zoals met wijziging van de Azure-abonnement-ID) kunt verder aanpassen van geautomatiseerde scenario's, zoals bijvoorbeeld afzonderlijke ontvangers op automatische afstemming is mailen aanbevelingen voor afzonderlijke abonnementen.
>

De bovenstaande concludeert stappen die nodig zijn voor het configureren van de werkstroom e-mail delivery taak. De volledige stroom die bestaan uit drie acties die zijn ingebouwd wordt in de volgende afbeelding weergegeven.

![Automatische afstemming van e-mailmeldingen stroom weergeven](./media/sql-database-automatic-tuning-email-notifications/howto-email-05.png)

Als u wilt testen van de stroom, klik op '**nu uitvoeren**' in de rechterbovenhoek in het deelvenster stroom.

Statistieken van het uitvoeren van de geautomatiseerde taken, met succes van het e-mailmeldingen verzonden, ziet u in het deelvenster van de analytics stroom.

![Stroom voor automatische afstemmen e-mailmeldingen uitgevoerd](./media/sql-database-automatic-tuning-email-notifications/howto-email-06.png)

De stroom analytics is handig voor het succes van de taak uitvoeringen, bewaking en indien nodig voor het oplossen van problemen.  Bij het oplossen van problemen ook raadzaam om te onderzoeken van de PowerShell-script mobiel uitvoeringslogboek toegankelijk via de Azure Automation-app.

De uiteindelijke uitvoer van de geautomatiseerd e-mailbericht lijkt op het volgende e-mailbericht ontvangen nadat bouwen en uitvoeren van deze oplossing:

![E-mailbericht voorbeelduitvoer van automatische afstemmen e-mailmeldingen](./media/sql-database-automatic-tuning-email-notifications/howto-email-07.png)

Door het PowerShell-script aan te passen, kunt u de uitvoer en de opmaak van de geautomatiseerd e-mailbericht aan uw behoeften kunt aanpassen.

U kunt de oplossing voor het bouwen van e-mailmeldingen op basis van een bepaalde gebeurtenis afstemmen en naar meerdere ontvangers, voor meerdere abonnementen of databases, afhankelijk van uw aangepaste scenario's mogelijk verder aanpassen. 

## <a name="next-steps"></a>Volgende stappen

- Leer meer over hoe automatische afstemming, kunt u verbeteren de prestaties van de database, Zie [automatische afstemming in Azure SQL Database](sql-database-automatic-tuning.md).
- Zie inschakelen van automatische afstemming in Azure SQL Database voor het beheren van uw werkbelasting [inschakelen automatische afstemming](sql-database-automatic-tuning-enable.md).
- Handmatig wilt bekijken en toepassen van automatische afstemming van aanbevelingen, Zie [zoeken en toepassen van aanbevelingen voor prestaties](sql-database-advisor-portal.md).
