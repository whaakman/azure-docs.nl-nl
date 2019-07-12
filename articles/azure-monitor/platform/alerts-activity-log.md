---
title: Maken, weergeven en beheren van waarschuwingen voor activiteitenlogboeken in Azure Monitor
description: Waarschuwingen voor activiteitenlogboek maken met behulp van de Azure-portal, een Azure Resource Manager-sjabloon en Azure PowerShell.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: vinagara
ms.openlocfilehash: 71e61228fcdbd52abedbc1f1205baa60b1aea923
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705282"
---
# <a name="create-view-and-manage-activity-log-alerts-by-using-azure-monitor"></a>Maken, weergeven en beheren van waarschuwingen voor activiteitenlogboeken via Azure Monitor  

## <a name="overview"></a>Overzicht
Waarschuwingen voor activiteitenlogboeken zijn de waarschuwingen die worden geactiveerd wanneer er een nieuwe activiteit log-gebeurtenis optreedt die overeenkomt met de voorwaarden zijn opgegeven in de waarschuwing.

Deze waarschuwingen zijn voor Azure-resources en kunnen worden gemaakt met behulp van een Azure Resource Manager-sjabloon. Ze kunnen ook worden gemaakt, bijgewerkt of verwijderd in Azure portal. Normaal gesproken u waarschuwingen voor activiteitenlogboek maken om meldingen te ontvangen wanneer bepaalde wijzigingen in de resources in uw Azure-abonnement optreden. Waarschuwingen zijn vaak gericht op bepaalde resourcegroepen of resources. Bijvoorbeeld, u wilt mogelijk een melding wanneer een virtuele machine in de resourcegroep voorbeeld **myProductionResourceGroup** wordt verwijderd. Of u wilt een melding ontvangen als de nieuwe rollen zijn toegewezen aan een gebruiker in uw abonnement.

> [!IMPORTANT]
> Waarschuwingen over service health melding kunnen niet worden gemaakt via de interface voor het maken van activiteit log waarschuwingen. Zie voor meer informatie over het maken en gebruiken van servicestatusmeldingen [ontvangen van waarschuwingen voor activiteitenlogboeken op servicestatusmeldingen](alerts-activity-log-service-notifications.md).

Wanneer u regels voor waarschuwingen maakt, zorg ervoor dat het volgende:

- Het abonnement in het bereik niet af van het abonnement waarin de waarschuwing wordt gemaakt.
- De criteria moet op het niveau, status, aanroeper, resourcegroep, resource-ID of resource type gebeurteniscategorie waarop de waarschuwing is geconfigureerd.
- Er is geen 'dragen' voorwaarde of geneste voorwaarden in de waarschuwingsconfiguratie JSON. In principe wordt slechts één 'allOf' voorwaarde toegestaan zonder verdere "allOf" of 'dragen' voorwaarden.
- Wanneer de categorie 'administratieve' is, moet u ten minste één van de bovenstaande criteria opgeven in de waarschuwing. U kunt een waarschuwing die wordt geactiveerd wanneer een gebeurtenis wordt gemaakt in de activiteitenlogboeken niet maken.


## <a name="azure-portal"></a>Azure Portal

De Azure-portal kunt u maken en wijzigen van de activiteit-logboekwaarschuwingsregels. De ervaring is geïntegreerd met een Azure-activiteitenlogboek om ervoor te zorgen naadloze waarschuwing maken voor specifieke gebeurtenissen die van belang zijn.

### <a name="create-with-the-azure-portal"></a>Maken met de Azure portal

Gebruik de volgende procedure.

1. Selecteer in de Azure portal, **Monitor** > **waarschuwingen**.
2. Selecteer **nieuwe waarschuwingsregel** in de linkerbovenhoek van de **waarschuwingen** venster.

     ![Nieuwe waarschuwingsregel](media/alerts-activity-log/AlertsPreviewOption.png)

     De **maken regel** venster wordt weergegeven.

      ![Opties voor nieuwe waarschuwingsregel](media/alerts-activity-log/create-new-alert-rule-options.png)

3. Onder **waarschuwingsvoorwaarde definiëren**, geef de volgende informatie op en selecteer **gedaan**:

   - **Waarschuwingsdoel:** Als u wilt weergeven en selecteren van het doel voor de nieuwe waarschuwing, gebruikt u **filteren op abonnement** / **filteren op resourcetype**. Selecteer de resource of resourcegroep in de lijst weergegeven.

     > [!NOTE]
     > 
     > U kunt een resource, resourcegroep of een hele abonnement voor een signaal activiteit selecteren.

     **Waarschuwingsdoel voorbeeld weergeven**

     ![Doel selecteren](media/alerts-activity-log/select-target.png)

   - Onder **criteria als doel**, selecteer **criteria toevoegen**. Alle beschikbare signalen voor het doel worden weergegeven, waaronder die uit verschillende categorieën van **activiteitenlogboek**. Naam van de categorie wordt toegevoegd aan de **-controleservice** naam.

   - Selecteer het signaal uit de lijst weergegeven van de verschillende bewerkingen mogelijk voor het type **activiteitenlogboek**.

     U kunt de tijdlijn van de geschiedenis van logboekbestanden en de bijbehorende waarschuwingslogica voor dit doel signaal selecteren:

     **Scherm criteria toevoegen**

     ![criteria toevoegen](media/alerts-activity-log/add-criteria.png)

     - **Tijd van de geschiedenis**: Gebeurtenissen weergegeven die beschikbaar zijn voor de geselecteerde bewerking kunnen worden weergegeven in de laatste 6, 12 of 24 uur of in de afgelopen week.

     - **Waarschuwing logische**:

       - **Gebeurtenisniveau**: De ernst van de gebeurtenis: _Uitgebreide_, _informatief_, _waarschuwing_, _fout_, of _kritieke_.
       - **Status**: De status van de gebeurtenis: _Aan de slag_, _mislukt_, of _geslaagd_.
       - **Gebeurtenis gestart door**: Ook wel bekend als de oproepende functie. De e-mailadres of Azure Active Directory-id van de gebruiker die de bewerking heeft uitgevoerd.

       In dit voorbeeld signaal-grafiek bevat de waarschuwingslogica toegepast:

       ![criteria die zijn geselecteerd](media/alerts-activity-log/criteria-selected.png)

4. Onder **Waarschuwingsdetails definiëren**, geef de volgende informatie op:

    - **Naam waarschuwingsregel**: De naam voor de nieuwe waarschuwingsregel.
    - **Beschrijving**: De beschrijving voor de nieuwe waarschuwingsregel.
    - **Waarschuwing niet opslaan op resourcegroep**: Selecteer de resourcegroep waar u deze nieuwe regel.

5. Onder **actiegroep**, uit de vervolgkeuzelijst, geef de actiegroep die u wilt toewijzen aan deze nieuwe waarschuwingsregel. Of, [maken van een nieuwe actiegroep](../../azure-monitor/platform/action-groups.md) en wijs deze toe aan de nieuwe regel. Voor het maken van een nieuwe groep selecteert **+ nieuwe groep**.

6. Als u de regels nadat u deze hebt gemaakt, schakelt **Ja** voor de **regel inschakelen bij het maken van** optie.
7. Selecteer **waarschuwingsregel maken**.

    De nieuwe waarschuwingsregel voor het activiteitenlogboek wordt gemaakt en er wordt een bevestigingsbericht weergegeven in de rechterbovenhoek van het venster.

    U kunt inschakelen, uitschakelen, bewerken of verwijderen van een regel. Meer informatie over het beheren van de activiteit log regels.


Een eenvoudige vergelijking voor informatie over voorwaarden waarop waarschuwingsregels kunnen worden gemaakt in een activiteitenlogboek is om te verkennen of filteren van gebeurtenissen via de [activiteitenlogboek in Azure portal](activity-log-view.md#azure-portal). In de **Azure Monitor - activiteitenlogboek** scherm, u kunt filteren of zoeken van de gebeurtenis die nodig zijn en vervolgens een waarschuwing te maken met behulp van de **waarschuwing voor activiteitenlogboek toevoegen** knop. Volg de stappen 4 t / m 7 als eerder weergegeven.
    
 ![waarschuwing toevoegen met het activiteitenlogboek](media/alerts-activity-log/add-activity-log.png)
    

### <a name="view-and-manage-in-the-azure-portal"></a>Weergeven en beheren in Azure portal

1. Selecteer in de Azure portal, **Monitor** > **waarschuwingen**. Selecteer **Waarschuwingsregels beheren** in de linkerbovenhoek van het venster.

    ![regels voor waarschuwingen beheren](media/alerts-activity-log/manage-alert-rules.png)

    De lijst met beschikbare regels wordt weergegeven.

2. Zoek de regel voor het logboek van activiteit te wijzigen.

    ![waarschuwingsregels activiteit zoeken](media/alerts-activity-log/searth-activity-log-rule-to-edit.png)

    U kunt de beschikbare filters _abonnement_, _resourcegroep_, _Resource_, _type signaal_, of _Status_ , te vinden van de activiteit regel die u wilt bewerken.

   > [!NOTE]
   > 
   > U kunt alleen bewerken **beschrijving**, **criteria als doel**, en **actiegroepen**.

3. Selecteer de regel en dubbelklik erop om het bewerken van de opties voor de regel. De benodigde wijzigingen aanbrengen en selecteer vervolgens **opslaan**.

   ![regels voor waarschuwingen beheren](media/alerts-activity-log/activity-log-rule-edit-page.png)

4. U kunt inschakelen, uitschakelen of verwijderen van een regel. Selecteer de gewenste optie aan de bovenkant van het venster nadat u de regel hebt geselecteerd, zoals beschreven in stap 2.


## <a name="azure-resource-manager-template"></a>Azure Resource Manager-sjabloon
Als u wilt een waarschuwing voor activiteitenlogboek maken met behulp van een Azure Resource Manager-sjabloon, maken van een resource van het type `microsoft.insights/activityLogAlerts`. U Vul vervolgens alle verwante eigenschappen. Hier volgt een sjabloon die een waarschuwing voor activiteitenlogboek maakt:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "activityLogAlertName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Activity log alert."
      }
    },
    "activityLogAlertEnabled": {
      "type": "bool",
      "defaultValue": true,
      "metadata": {
        "description": "Indicates whether or not the alert is enabled."
      }
    },
    "actionGroupResourceId": {
      "type": "string",
      "metadata": {
        "description": "Resource Id for the Action group."
      }
    }
  },
  "resources": [   
    {
      "type": "Microsoft.Insights/activityLogAlerts",
      "apiVersion": "2017-04-01",
      "name": "[parameters('activityLogAlertName')]",      
      "location": "Global",
      "properties": {
        "enabled": "[parameters('activityLogAlertEnabled')]",
        "scopes": [
            "[subscription().id]"
        ],        
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Administrative"
            },
            {
              "field": "operationName",
              "equals": "Microsoft.Resources/deployments/write"
            },
            {
              "field": "resourceType",
              "equals": "Microsoft.Resources/deployments"
            }
          ]
        },
        "actions": {
          "actionGroups":
          [
            {
              "actionGroupId": "[parameters('actionGroupResourceId')]"
            }
          ]
        }
      }
    }
  ]
}
```
Het vorige voorbeeld JSON als bijvoorbeeld sampleActivityLogAlert.json ten behoeve van deze procedure kan worden opgeslagen en kan worden geïmplementeerd met behulp van [Azure Resource Manager in Azure portal](../../azure-resource-manager/resource-group-template-deploy-portal.md).

> [!NOTE]
> Het duurt tot vijf minuten voor de nieuwe waarschuwingsregel voor activiteitenlogboeken actief.

## <a name="rest-api"></a>REST-API 
De [Azure Monitor activiteit Log waarschuwingen API](https://docs.microsoft.com/rest/api/monitor/activitylogalerts) is een REST-API. Het is volledig compatibel is met de Azure Resource Manager REST API. Het kan worden gebruikt via PowerShell met behulp van de Resource Manager-cmdlet of de Azure CLI.

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

### <a name="deploy-the-resource-manager-template-with-powershell"></a>Implementeer de sjabloon Resource Manager met PowerShell
PowerShell gebruiken voor het implementeren van de Resource Manager-sjabloon van het voorbeeld wordt weergegeven in de vorige [Azure Resource Manager-sjabloon](#azure-resource-manager-template) sectie, gebruikt u de volgende opdracht uit:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile sampleActivityLogAlert.json -TemplateParameterFile sampleActivityLogAlert.parameters.json
```

de sampleActivityLogAlert.parameters.json bevat waarin de waarden voor de parameters die nodig zijn voor het maken van waarschuwingsregel.

### <a name="use-activity-log-powershell-cmdlets"></a>Activiteitenlogboek PowerShell-cmdlets gebruiken

Waarschuwingen voor activiteitenlogboeken zijn speciaal bestemd voor PowerShell-cmdlets beschikbaar:

- [Set-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Set-AzActivityLogAlert): Hiermee maakt u een nieuwe waarschuwing voor activiteitenlogboek gemaakt of een bestaande waarschuwing voor activiteitenlogboek bijgewerkt.
- [Get-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Get-AzActivityLogAlert): Hiermee haalt u een of meer activiteiten waarschuwingsresources log.
- [Enable-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Enable-AzActivityLogAlert): Hiermee kunt een bestaande waarschuwing voor activiteitenlogboek en stelt de labels.
- [Disable-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Disable-AzActivityLogAlert): Schakelt een bestaande waarschuwing voor activiteitenlogboek en stelt de labels.
- [Remove-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Remove-AzActivityLogAlert): Hiermee verwijdert u een waarschuwing voor activiteitenlogboek.

## <a name="azure-cli"></a>Azure-CLI

Azure CLI-opdrachten in de set toegewezen [az monitor activiteitenlogboek waarschuwing](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert) zijn beschikbaar voor het beheren van activiteit-logboekwaarschuwingsregels.

Gebruik voor het maken van een nieuwe waarschuwingsregel voor activiteitenlogboeken, de volgende opdrachten in deze volgorde:

1. [maken van de AZ monitor activiteitenlogboek waarschuwing](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-create): Maak een nieuwe activiteit log waarschuwingsregel resource.
1. [AZ monitor activiteitenlogboek waarschuwing bereik](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert/scope): Bereik voor de activiteit waarschuwingsregel toevoegen.
1. [AZ monitor activiteitenlogboek waarschuwing-actiegroep](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert/action-group): Een actiegroep toevoegen aan de waarschuwingsregel voor activiteitenlogboeken.

Als u wilt één activiteit waarschuwingsregel logboekresource ophalen, gebruikt de Azure CLI-opdracht [az monitor activiteitenlogboek waarschuwing weergeven](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-show
). Als u wilt alle activiteit log waarschuwingsregel resources in een resourcegroep weergeven, gebruikt u [az monitor activiteitenlogboek waarschuwing lijst](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-list).
Activiteit log waarschuwingsregel resources kunnen worden verwijderd met behulp van de Azure CLI-opdracht [az monitor activiteitenlogboek waarschuwing verwijderen](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-delete).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [webhook-schema voor activiteitenlogboeken](../../azure-monitor/platform/activity-log-alerts-webhook.md).
- Lees een [overzicht van activiteitenlogboeken](../../azure-monitor/platform/activity-log-alerts.md).
- Meer informatie over [actiegroepen](../../azure-monitor/platform/action-groups.md).  
- Meer informatie over [health servicemeldingen](../../azure-monitor/platform/service-notifications.md).
