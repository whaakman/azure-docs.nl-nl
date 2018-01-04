---
title: Verzamelen van metrische gegevens voor Azure PaaS resources met Log Analytics | Microsoft Docs
description: Informatie over het Azure PaaS resource metrische gegevens verzamelen met behulp van PowerShell voor het bewaren en analyseren in logboekanalyse inschakelen.
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: magoedte
ms.openlocfilehash: 83491c4902dabc6bab1e222551298cfaffbaecf4
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="configure-collection-of-azure-paas-resource-metrics-with-log-analytics"></a>Verzameling van metrische gegevens voor Azure PaaS-resources configureren met Log Analytics

Azure-Platform als een Service (PaaS)-bronnen, zoals Azure SQL en websites (Web-Apps) kunt metrische gegevens van prestatiegegevens met logboekanalyse systeemeigen verzenden. Dit script kan gebruikers metrische gegevens voor PaaS-resources die al is geïmplementeerd in een specifieke resourcegroep of in een hele abonnement logboekregistratie inschakelen. 

Vandaag de dag is er geen manier om in te schakelen van metrische gegevens logboekregistratie voor PaaS-resources via de Azure-portal. Daarom moet u een PowerShell-script gebruiken. Deze systeemeigen metrische gegevens logboekregistratie kunnen samen met logboekanalyse bewaking, kunt u Azure-resources op grote schaal volgen. 

## <a name="prerequisites"></a>Vereisten
Controleer of dat u hebt de volgende Azure Resource Manager-modules geïnstalleerd op uw computer voordat u verdergaat:

- AzureRM.Insights
- AzureRM.OperationalInsights
- AzureRM.Resources
- AzureRM.profile

>[!NOTE]
>We raden aan dat alle Azure Resource Manager-modules dezelfde versie zijn om compatibiliteit te controleren wanneer u Azure Resource Manager-opdrachten met PowerShell uitvoeren.
>
Zie de nieuwste versie van de Azure Resource Manager-modules installeren op uw computer [Azure PowerShell installeren en configureren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.1#update-azps).  

## <a name="enable-azure-diagnostics"></a>Azure diagnostische gegevens inschakelen  
Configureren van Azure Diagnostics voor PaaS-resources wordt bereikt door het uitvoeren van het script **inschakelen AzureRMDiagnostics.ps1**, die is beschikbaar via de [PowerShell Gallery](https://www.powershellgallery.com/packages/Enable-AzureRMDiagnostics/2.52/DisplayScript).  Het script ondersteunt de volgende scenario's:
  
* Een resource die betrekking hebben op een of meer resourcegroepen in een abonnement opgeven  
* Een resource die betrekking hebben op een specifieke resourcegroep in een abonnement opgeven  
* Configureren van een resource te sturen naar een andere werkruimte

Alleen bronnen die ondersteuning bieden bij het verzamelen meetgegevens met Azure diagnostics en rechtstreeks naar logboekanalyse verzenden die worden ondersteund.  Raadpleeg voor een gedetailleerde lijst [verzamelen Azure service-logboeken en metrische gegevens voor gebruik in Log Analytics](log-analytics-azure-storage.md) 

De volgende stappen uitvoeren om te downloaden en uitvoeren van het script.

1.  Typ in het startscherm Windows **PowerShell** en PowerShell met de rechtermuisknop in de zoekresultaten.  Selecteer in het menu **als Administrator uitvoeren**.   
2. Sla de **inschakelen AzureRMDiagnostics.ps1** scriptbestand lokaal door de volgende opdracht uit te voeren en het leveren van een pad voor het opslaan van het script.    

    ```
    PS C:\> save-script -Name Enable-AzureRMDiagnostics -Path "C:\users\<username>\desktop\temp"
    ```

3. Voer `Login-AzureRmAccount` geen verbinding maken met Azure.   
4. Voer het volgende script `.\Enable-AzureRmDiagnostics.ps1` zonder parameters voor het inschakelen van gegevensverzameling van een specifieke bron in uw abonnement of met de parameter `-ResourceGroup <myResourceGroup>` om op te geven van een resource in een specifieke resourcegroep.   
5. Selecteer het juiste abonnement uit de lijst als er meer dan één, door te voeren van de juiste waarde.<br><br> ![Selecteer geretourneerd door de script-abonnement](./media/log-analytics-collect-azurepass-posh/script-select-subscription.png)<br> Anders wordt deze automatisch geselecteerd het één abonnement beschikbaar.
6. Vervolgens wordt retourneert het script een lijst met Log Analytics-werkruimten die zijn geregistreerd in het abonnement.  Selecteer de juiste uit de lijst.<br><br> ![Werkruimte geretourneerd door script selecteren](./media/log-analytics-collect-azurepass-posh/script-select-workspace.png)<br> 
7. Selecteer de Azure resource die u wilt verzamelen van inschakelen. Schakel bijvoorbeeld als u 5 typt, verzamelen van gegevens voor SQL Azure-Databases.<br><br> ![Selecteer brontype geretourneerd door script](./media/log-analytics-collect-azurepass-posh/script-select-resource.png)<br>
   U kunt alleen bronnen die ondersteuning bieden voor het verzamelen meetgegevens met diagnostische Azure-gegevens en verzenden van rechtstreeks naar een Log Analytics selecteren.  Het script wordt een waarde van weergegeven **True** onder de **metrische gegevens** kolom voor de lijst met bronnen die ze in uw abonnement of de opgegeven resourcegroep detecteert.    
8. U wordt gevraagd uw selectie te bevestigen.  Voer **Y** inschakelen van logboekregistratie van metrische gegevens voor alle resources voor de scope die is gedefinieerd, die in ons voorbeeld alle SQL-databases in het abonnement zijn geselecteerd.  

Het script wordt uitgevoerd op elke bron die overeenkomt met de geselecteerde criteria en metrische gegevens verzamelen voor hen inschakelen. Nadat dit voltooid, ziet u een bericht met de configuratie is voltooid.  

Kort na voltooiing gaat u gegevens van de Azure-PaaS-resource in de opslagplaats Log Analytics zien.  Een record met het type `AzureMetrics` wordt gemaakt en analyseren van deze records worden ondersteund door de [Azure SQL Analytics](log-analytics-azure-sql.md) en [Azure Web Apps Analytics](log-analytics-azure-web-apps-analytics.md) oplossingen.   

## <a name="update-a-resource-to-send-data-to-another-workspace"></a>Het bijwerken van een bron voor het verzenden van gegevens naar een andere werkruimte
Als u een resource die al gegevens naar een werkruimte voor logboekanalyse verzenden is hebt en u later besluit opnieuw te configureren om te verwijzen naar een andere werkruimte, kunt u het script uitvoeren de `-Update` parameter.  

**Voorbeeld:** 
`PS C:\users\<username>\Desktop\temp> .\Enable-AzureRMDiagnostics.ps1 -Update`

U wordt gevraagd om dezelfde informatie als bij het uitvoeren van het script uit te voeren van de eerste configuratie te beantwoorden.  

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) om de gegevens verzameld van gegevensbronnen en oplossingen te analyseren. 

* Gebruik [aangepaste velden](log-analytics-custom-fields.md)(voor het parseren van de gebeurtenis legt vast in afzonderlijke velden.

* Bekijk [maken van een aangepast dashboard voor gebruik in logboekanalyse](log-analytics-dashboards.md) om te begrijpen hoe uw logboek visualiseren zoekt op allerlei zinvolle manieren voor de organisatie.