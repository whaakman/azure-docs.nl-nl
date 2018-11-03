---
title: Verzamelen van metrische gegevens voor Azure PaaS resources met Log Analytics | Microsoft Docs
description: Leer hoe u Azure PaaS resource metrische gegevens te verzamelen met behulp van PowerShell voor het bewaren en analyseren in Log Analytics.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 1cb36eec6dd23d65298b2c011a194c015922d807
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50960987"
---
# <a name="configure-collection-of-azure-paas-resource-metrics-with-log-analytics"></a>Verzamelen van metrische gegevens voor Azure PaaS-resources met Log Analytics configureren

Azure-Platform als een Service (PaaS)-resources, zoals Azure SQL en websites (Web-Apps), kan de prestaties van metrische gegevens systeemeigen naar Log Analytics verzenden. Met dit script kan gebruikers logboekregistratie voor PaaS-resources die al zijn geïmplementeerd in een specifieke resourcegroep of in op een hele abonnement metrische gegevens inschakelen. 

Er is vandaag de dag niet mogelijk om in te schakelen van metrische gegevens over de logboekregistratie voor PaaS-resources via Azure portal. Daarom moet u een PowerShell-script gebruiken. Deze mogelijkheid voor het vastleggen van systeemeigen metrische gegevens samen met Log Analytics, bewaking, kunt u Azure-resources op schaal te volgen. 

## <a name="prerequisites"></a>Vereisten
Controleer of dat u hebt de volgende Azure Resource Manager-modules geïnstalleerd op uw computer voordat u doorgaat:

- AzureRM.Insights
- AzureRM.OperationalInsights
- AzureRM.Resources
- AzureRM.profile

>[!NOTE]
>Het is raadzaam dat alle Azure Resource Manager-modules dezelfde versie zijn om compatibiliteit te controleren of wanneer u Azure Resource Manager-opdrachten vanuit PowerShell uitvoeren.
>
Zie voor informatie over het installeren van de meest recente versie van de Azure Resource Manager-modules op uw computer [Azure PowerShell installeren en configureren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.1#update-azps).  

## <a name="enable-azure-diagnostics"></a>Azure Diagnostics inschakelen  
Azure Diagnostics configureren voor PaaS-resources wordt bereikt door het uitvoeren van het script **inschakelen AzureRMDiagnostics.ps1**, die is beschikbaar via de [PowerShell Gallery](https://www.powershellgallery.com/packages/Enable-AzureRMDiagnostics/2.52).  Het script ondersteunt de volgende scenario's:
  
* Een resource die betrekking hebben op een of meer resourcegroepen in een abonnement op te geven  
* Een resource die betrekking hebben op een specifieke resourcegroep in een abonnement op te geven  
* Een resource te sturen naar een andere werkruimte configureren

Alleen bronnen die ondersteuning bieden verzamelen metrische gegevens met Azure diagnostics en rechtstreeks naar Log Analytics verzenden die worden ondersteund.  Raadpleeg voor een gedetailleerde lijst [Azure verzamelen van Logboeken en metrische gegevens voor gebruik in Log Analytics](log-analytics-azure-storage.md) 

De volgende stappen uitvoeren om te downloaden en uitvoeren van het script.

1.  Typ in het Windows-startscherm **PowerShell** en PowerShell met de rechtermuisknop in de zoekresultaten.  Selecteer in het menu **als Administrator uitvoeren**.   
2. Sla de **inschakelen AzureRMDiagnostics.ps1** scriptbestand lokaal door de volgende opdracht uit en het leveren van een pad voor het opslaan van het script.    

    ```
    PS C:\> save-script -Name Enable-AzureRMDiagnostics -Path "C:\users\<username>\desktop\temp"
    ```

3. Voer `Connect-AzureRmAccount` uit om een verbinding op te zetten met Azure.   
4. Voer het volgende script `.\Enable-AzureRmDiagnostics.ps1` zonder parameters om gegevens te verzamelen van een specifieke bron in uw abonnement of met de parameter `-ResourceGroup <myResourceGroup>` om op te geven van een resource in een specifieke resourcegroep.   
5. Selecteer in de lijst het juiste abonnement hebt u meer dan één, door te voeren van de juiste waarde.<br><br> ![Abonnement selecteren die wordt geretourneerd door het script](./media/log-analytics-collect-azurepass-posh/script-select-subscription.png)<br> Anders wordt automatisch geselecteerd de één abonnement beschikbaar.
6. Vervolgens retourneert dit script een lijst met Log Analytics-werkruimten die zijn geregistreerd in het abonnement.  Selecteer een van de lijst.<br><br> ![Selecteer een werkruimte die wordt geretourneerd door het script](./media/log-analytics-collect-azurepass-posh/script-select-workspace.png)<br> 
7. Selecteer het Azure-resource die u wilt verzamelen van inschakelen. Bijvoorbeeld, als u 5 typt, inschakelen u gegevensverzameling voor SQL Azure-Databases.<br><br> ![Selecteer resourcetype dat wordt geretourneerd door script](./media/log-analytics-collect-azurepass-posh/script-select-resource.png)<br>
   U kunt alleen de resources die ondersteuning bieden voor verzameld metrische gegevens met Azure Diagnostics en rechtstreeks naar een Log Analytics verzenden selecteren.  Het script ziet u een waarde van **waar** onder de **metrische gegevens** kolom voor de lijst met resources in uw abonnement of de opgegeven resourcegroep worden ontdekt.    
8. U wordt gevraagd uw selectie te bevestigen.  Voer **Y** metrische gegevens logboekregistratie inschakelen voor alle geselecteerde resources voor het bereik dat is gedefinieerd, die in ons voorbeeld alle SQL-databases in het abonnement worden.  

Het script wordt uitgevoerd voor elke resource die overeenkomt met de geselecteerde criteria en verzamelen van metrische gegevens voor hen inschakelen. Nadat deze voltooid, ziet u een bericht configuratie is voltooid.  

Kort na voltooiing begint u om gegevens van de Azure PaaS-resource in uw Log Analytics-opslagplaats te bekijken.  Een record met het type `AzureMetrics` wordt gemaakt en analyseren van deze records worden ondersteund door de [Azure SQL Analytics](log-analytics-azure-sql.md) en [Azure Web Apps Analytics](log-analytics-azure-web-apps-analytics.md) beheeroplossingen.   

## <a name="update-a-resource-to-send-data-to-another-workspace"></a>Een resource voor het verzenden van gegevens naar een andere werkruimte bijwerken
Als u een resource die al gegevens naar Log Analytics-werkruimte verzenden is hebt en u later besluit om opnieuw te configureren om te verwijzen naar een andere werkruimte, kunt u het script uitvoeren de `-Update` parameter.  

**Voorbeeld:** 
`PS C:\users\<username>\Desktop\temp> .\Enable-AzureRMDiagnostics.ps1 -Update`

U wordt gevraagd om te beantwoorden van dezelfde informatie als wanneer u het script om uit te voeren van de eerste configuratie hebt uitgevoerd.  

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [zoekopdrachten](log-analytics-log-search.md) om de gegevens die worden verzameld van gegevensbronnen en oplossingen te analyseren. 

* Gebruik [aangepaste velden](log-analytics-custom-fields.md)(voor het parseren van de gebeurtenis legt vast in afzonderlijke velden.

* Beoordeling [maken van een aangepast dashboard voor gebruik in Log Analytics](log-analytics-dashboards.md) wilt weten hoe u voor het visualiseren van uw logboek wordt gezocht op allerlei zinvolle manieren voor de organisatie.
