---
title: Diagnostische logboekregistratie in Azure Traffic Manager inschakelen
description: Leer hoe u Diagnostische logboekregistratie inschakelen voor uw Traffic Manager-profiel en toegang tot de logboeken die zijn gemaakt als gevolg hiervan.
services: traffic-manager
author: KumudD
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/25/2019
ms.author: kumud
ms.openlocfilehash: d4d4420e2d38b1418a08ad1ca51dd0f75f3fe7b9
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56001099"
---
# <a name="enable-diagnostic-logging-in-azure-traffic-manager"></a>Diagnostische logboekregistratie in Azure Traffic Manager inschakelen

In dit artikel wordt beschreven hoe u Diagnostische logboekregistratie en toegang logboekgegevens voor een Traffic Manager-profiel inschakelen.

Diagnostische logboeken in Azure Traffic Manager kunnen bieden inzicht in het gedrag van de resource Traffic Manager-profiel. U kunt bijvoorbeeld de logboekgegevens van het profiel gebruiken om te bepalen waarom time-out van afzonderlijke tests op basis van een eindpunt.

## <a name="enable-diagnostic-logging"></a>Bijhouden van diagnostische gegevens inschakelen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

U kunt de opdrachten die volgen in uitvoeren de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. De Azure Cloud Shell is een gratis interactieve shell. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Als u PowerShell vanaf uw computer uitvoeren, moet u de Azure PowerShell-module 1.0.0 of hoger. U kunt uitvoeren `Get-Module -ListAvailable Az` de geïnstalleerde versie te vinden. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). Als u PowerShell lokaal uitvoert, moet u ook om uit te voeren `Login-AzAccount` zich aanmeldt bij Azure.

1. **Traffic Manager-profiel ophalen:**

    Als u wilt vastleggen van diagnostische gegevens inschakelen, moet u de ID van een Traffic Manager-profiel. Ophalen van het Traffic Manager-profiel dat u Diagnostische logboekregistratie voor met wilt inschakelen [Get-AzTrafficManagerProfile](/powershell/module/az.TrafficManager/Get-azTrafficManagerProfile). De uitvoer bevat informatie over Traffic Manager-profiel-ID.

    ```azurepowershell-interactive
    Get-AzTrafficManagerProfile -Name <TrafficManagerprofilename> -ResourceGroupName <resourcegroupname>
    ```

2. **Diagnostische logboekregistratie inschakelen voor Traffic Manager-profiel:**

    Diagnostische logboekregistratie inschakelen voor Traffic Manager-profiel met behulp van de ID die is verkregen in de vorige stap met [Set AzDiagnosticSetting](https://docs.microsoft.com/powershell/module/az.insights/set-azdiagnosticsetting?view=latest). De volgende opdracht slaat uitgebreide logboeken voor het Traffic Manager-profiel aan een opgegeven Azure Storage-account. 

      ```azurepowershell-interactive
    Set-AzDiagnosticSetting -ResourceId <TrafficManagerprofileResourceId> -StorageAccountId <storageAccountId> -Enabled $true
      ``` 
3. **Controleer of de diagnostische instellingen:**

      Controleer of de diagnostische instellingen voor de Traffic Manager-profiel met [Get-AzDiagnosticSetting](https://docs.microsoft.com/powershell/module/az.insights/get-azdiagnosticsetting?view=latest). De volgende opdracht geeft de categorieën die zijn geregistreerd voor een resource.

     ```azurepowershell-interactive
     Get-AzDiagnosticSetting -ResourceId <TrafficManagerprofileResourceId>
     ```  
      Zorg ervoor dat alle categorieën die zijn gekoppeld aan de weergave van Traffic Manager profiel resource logboekbestanden als ingeschakeld. Controleer ook of het opslagaccount correct is ingesteld.

## <a name="access-log-files"></a>Logboekbestanden voor toegang
1. Meld u aan bij [Azure Portal](https://portal.azure.com). 
1. Navigeer naar uw Azure Storage-account in de portal.
2. Op de **overzicht** pagina van uw Azure storage-account onder **Services** Selecteer **Blobs**.
3. Voor **Containers**, selecteer **insights-logs-probehealthstatusevents**, en navigeer naar het bestand PT1H.json en op **downloaden** en sla een kopie van dit logboek het bestand.

    ![Logboekbestanden van de toegang van uw Traffic Manager-profiel van een blob-opslag](./media/traffic-manager-logs/traffic-manager-logs.png)


## <a name="traffic-manager-log-schema"></a>Schema voor Traffic Manager-logboek

Alle logboeken met diagnostische gegevens beschikbaar zijn via Azure Monitor delen een gemeenschappelijk schema van de op het hoogste niveau met flexibiliteit voor elke service op de unieke eigenschappen voor hun eigen gebeurtenissen verzenden. Voor diagnostische logboeken op het hoogste niveau schema, Zie [ondersteunde services, schema's en categorieën voor diagnostische logboeken van Azure](../azure-monitor/platform/tutorial-dashboards.md).

De volgende tabel bevat de logboeken schema die specifiek zijn voor de Azure Traffic Manager-profiel-resource.

|||||
|----|----|---|---|
|**Veldnaam**|**Veldtype**|**Definitie**|**Voorbeeld**|
|EndpointName|String|De naam van het Traffic Manager-eindpunt waarvan de gezondheidsstatus wordt opgenomen.|*myPrimaryEndpoint*|
|Status|String|De status van het Traffic Manager-eindpunt dat is dat wordt aangeduid. De status kan zijn **van** of **omlaag**.|**Omhoog**|
|||||

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Traffic Manager-bewaking](traffic-manager-monitoring.md)

