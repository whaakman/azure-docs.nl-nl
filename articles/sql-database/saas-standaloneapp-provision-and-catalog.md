---
title: Zelfstudie voor multitenant SaaS - Azure SQL Database | Microsoft Docs
description: Inrichten en catalogus nieuwe tenants met behulp van de zelfstandige toepassing patroon
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: SaaS
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: billgib
ms.openlocfilehash: dd43ede94d6f219f3b551091fc6e4b59f56386d1
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="provision-and-catalog-new-tenants-using-the--application-per-tenant-saas-pattern"></a>Inrichten en catalogus nieuwe tenants met behulp van de toepassing per tenant SaaS-patroon

In dit artikel bevat informatie over de inrichting en catalogiseren van nieuwe tenants met behulp van de zelfstandige app per tenant SaaS-patroon.
In dit artikel bevat twee hoofdonderdelen:
* Conceptuele bespreking van de nieuwe tenants worden gecatalogiseerd en inrichting
* Een zelfstudie die PowerShell voorbeeldcode die kunnen ingericht en catalogiseren worden markeert
    * De zelfstudie maakt gebruik van de Wingtip Tickets SaaS voorbeeldtoepassing, aangepast aan de app zelfstandige per tenant-patroon.

## <a name="standalone-application-per-tenant-pattern"></a>Zelfstandige toepassing per tenant patroon
De zelfstandige app per tenant patroon is een van de verschillende patronen voor multitenant SaaS-toepassingen.  Een zelfstandige app wordt in dit patroon worden ingericht voor elke tenant. De toepassing bestaat uit niveau toepassingsonderdelen en een SQL-database.  Elke tenant-app kan worden geïmplementeerd in het abonnement van de leverancier.  U kunt ook Azure biedt een [beheerde toepassingen programma](https://docs.microsoft.com/azure/managed-applications/overview) in die een app kan worden geïmplementeerd in een tenant-abonnement en beheerd door de leverancier van de tenant namens. 

   ![patroon van de App per tenant](media/saas-standaloneapp-provision-and-catalog/standalone-app-pattern.png)

Wanneer u een toepassing implementeert voor een tenant, worden de app en database ingericht in een nieuwe resourcegroep gemaakt voor de tenant.  Afzonderlijke resourcegroepen gebruiken isoleert toepassingsbronnen voor elke tenant en kan ze onafhankelijk van elkaar worden beheerd. Binnen elke resourcegroep worden elk toepassingsexemplaar is geconfigureerd voor het rechtstreeks toegang hebben tot de bijbehorende database.  Dit model van de verbinding in tegenstelling tot andere patronen die gebruikmaken van een catalogus broker verbindingen tussen de app en de database.  En omdat er geen bronnen delen, elke tenant-database moet worden ingericht met voldoende resources om de piekbelasting te verwerken. Dit patroon vaak worden gebruikt voor SaaS-toepassingen met minder tenants, waarbij er nadruk op tenant isolatie en minder de nadruk op resourcekosten.  

## <a name="using-a-tenant-catalog-with-the-application-per-tenant-pattern"></a>Met behulp van een tenant-catalogus met de toepassing per tenant patroon
Hoewel elke tenant-app en database zijn volledig geïsoleerd, kunnen verschillende management en scenario's met gegevensanalyses via tenants werken.  Bijvoorbeeld: toepassen van een schemawijziging voor een nieuwe release van de toepassing vereist wijzigingen in het schema van de database van elke tenant. Scenario's voor rapportage en analyse wellicht ook toegang tot de tenant-databases ongeacht waar ze zijn geïmplementeerd.

   ![patroon van de App per tenant](media/saas-standaloneapp-provision-and-catalog/standalone-app-pattern-with-catalog.png)

De tenant-catalogus bevat een toewijzing tussen een tenant-id en een tenant-database, zodat een-id die moet worden omgezet naar een server en de databasenaam.  De tenant-id wordt berekend als een hash van de naam van de tenant in de Wingtip SaaS-app, maar andere schema's kunnen worden gebruikt.  Bij zelfstandige toepassingen niet de catalogus verbindingen beheren hoeft, kan de catalogus worden gebruikt als bereik voor andere acties om een set databases van de tenant. Elastische Query kan bijvoorbeeld de catalogus gebruiken om te bepalen welke databases waarvoor query's voor het melden van cross-tenant worden gedistribueerd.

## <a name="elastic-database-client-library"></a>Clientbibliotheek voor Elastic Database
In de voorbeeldtoepassing Wingtip de catalogus wordt geïmplementeerd door de shard-beheerfuncties van de [clientbibliotheek voor elastische Database](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-client-library) (EDCL).  De bibliotheek kunt een toepassing te maken, beheren en gebruiken van een shard-toewijzing die is opgeslagen in een database. In de steekproef Wingtip Tickets de catalogus wordt opgeslagen de *tenant catalogus* database.  De shard wijst een tenantsleutel voor de shard (database) in die tenant gegevens wordt opgeslagen.  EDCL functies beheren een *globale shard-toewijzing* opgeslagen in de tabellen in de *tenant catalogus* database en een *lokale shard-toewijzing* opgeslagen in elk shard.

EDCL functies kunnen worden aangeroepen vanuit toepassingen of PowerShell-scripts maken en beheren van de vermeldingen in de shard-toewijzing. Andere functies EDCL kunnen worden gebruikt voor het ophalen van de reeks shards of verbinding maken met de juiste database voor de gegeven tenantsleutel. 
    
> [!IMPORTANT] 
> Bewerk de gegevens in de catalogusdatabase of de lokale shard-toewijzing in de tenant-databases rechtstreeks. Directe updates worden niet ondersteund vanwege de hoge risico van gegevensbeschadiging. In plaats daarvan de toewijzingsgegevens bewerken met behulp van API's EDCL alleen.

## <a name="tenant-provisioning"></a>Tenant-inrichting 
Elke tenant moet een nieuwe Azure-resourcegroep, die moet worden gemaakt voordat resources binnen deze kunnen worden ingericht. Zodra de resourcegroep bestaat, kan een Azure Resource Management-sjabloon implementeren van de toepassingsonderdelen en de database en configureer vervolgens verbinding met de database worden gebruikt. Als u wilt initialiseren schema van de database, kunt de sjabloon een Bacpac-bestand importeren.  U kunt ook kan de database worden gemaakt als een kopie van een 'template'-database.  De database wordt vervolgens verder bijgewerkt met de eerste plaats gegevens en geregistreerd in de catalogus.

## <a name="tutorial"></a>Zelfstudie

In deze zelfstudie leert u het volgende:
* Een catalogus inrichten
* Registreren van de voorbeeld-tenant-databases die u hebt geïmplementeerd eerder in de catalogus
* Een extra tenant in te richten en deze te registreren in de catalogus

Een Azure Resource Manager-sjabloon wordt te implementeren en configureren van de toepassing, de tenant-database maken en vervolgens een Bacpac-bestand voor het initialiseren van het importeren gebruikt. De aanvraag importeren kan enkele minuten duren voordat deze ondernomen wordt in de wachtrij geplaatst.

Aan het einde van deze zelfstudie hebt u een set zelfstandige tenant toepassingen, met elke database die is geregistreerd in de catalogus.

## <a name="prerequisites"></a>Vereisten
U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan: 
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.
* De drie voorbeeld tenant-apps worden geïmplementeerd. Zie voor het implementeren van deze apps in minder dan vijf minuten, [implementeren en het patroon Wingtip Tickets SaaS zelfstandige toepassing verkennen](https://docs.microsoft.com/azure/sql-database/saas-standaloneapp-get-started-deploy).

## <a name="provision-the-catalog"></a>Inrichten van de catalogus
Informatie over het inrichten van de catalogus gebruikt voor het registreren van alle databases in de tenant in deze taak. U gaat het volgende doen: 
* **Inrichting van de catalogusdatabase** met behulp van een Azure-resource management-sjabloon. De database wordt geïnitialiseerd door een Bacpac-bestand te importeren.  
* **Registreren van de voorbeeld-apps tenant** die u eerder hebt geïmplementeerd.  Elke tenant is geregistreerd met een sleutel die is gemaakt op basis van een hash van de naam van de tenant.  Naam van de tenant wordt ook opgeslagen in een tabel met de extensie in de catalogus.

1. Open in PowerShell ISE *...\Learning Modules\UserConfig.psm* en werk de  **\<gebruiker\>**  in de waarde die u hebt gebruikt bij het implementeren van de drie voorbeeldtoepassingen.  **Sla het bestand**.  
1. Open in PowerShell ISE *...\Learning Modules\ProvisionTenants\Demo-ProvisionAndCatalog.ps1* en stel **$Scenario = 1**. De catalogus tenant implementeren en registreren van de vooraf gedefinieerde tenants.

1. Een onderbrekingspunt toevoegen door de gegevens van de cursor overal op de regel die wordt vermeld, `& $PSScriptRoot\New-Catalog.ps1`, en druk vervolgens op **F9**.

    ![een onderbrekingspunt voor tracering instellen](media/saas-standaloneapp-provision-and-catalog/breakpoint.png)

1. Voer het script door te drukken **F5**. 
1.  Nadat de uitvoering van het script stopt bij het onderbrekingspunt, drukt u op **F11** naar stap in het script New-Catalog.ps1.
1.  Het script worden uitgevoerd met de menuopties foutopsporing F10 en F11 traceren, om meer dan of in stap aangeroepen functies.
    *   Zie voor meer informatie over foutopsporing van PowerShell-scripts [Tips voor het werken met en foutopsporing van PowerShell-scripts](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).

Nadat het script is voltooid, wordt de catalogus wordt bestaan en alle tenants in de steekproef wordt geregistreerd. 

Bekijk nu de resources die u hebt gemaakt.

1. Open de [Azure-portal](https://portal.azure.com/) en blader de resourcegroepen.  Open de **wingtip-sa-catalogus -\<gebruiker\>**  resource groeperen en noteer de GC-server en database.
1. Open de database in de portal en selecteer *Gegevensverkenner* uit in het menu links.  Klik op de opdracht aanmelding en voer het wachtwoord =  **P@ssword1** .


1. Verken het schema van de *tenantcatalog* database.  
   * De objecten in de `__ShardManagement` schema worden geleverd door de elastische Database-clientbibliotheek.
   * De `Tenants` tabel en `TenantsExtended` weergeven zijn uitbreidingen toegevoegd in de steekproef die laten zien hoe de catalogus voor aanvullende waarde kan worden uitgebreid.
1. Voer de query `SELECT * FROM dbo.TenantsExtended`.          

   ![Gegevensverkenner](media/saas-standaloneapp-provision-and-catalog/data-explorer-tenantsextended.png)

    Als alternatief voor het gebruik van de Data Explorer kunt u verbinding met de database van SQL Server Management Studio. U doet dit door verbinding maken met de server wingtip- 

    
    Opmerking dat u beter niet bewerken gegevens rechtstreeks in de catalogus - gebruik altijd de shard-management API's. 

## <a name="provision-a-new-tenant-application"></a>Een nieuwe tenant toepassing inrichten

Informatie over het inrichten van een toepassing voor één tenant in deze taak. U gaat het volgende doen:  
* **Maak een nieuwe resourcegroep** voor de tenant. 
* **Inrichten van de toepassing en de database** in de nieuwe resourcegroep met behulp van een Azure-resource management-sjabloon.  Deze actie bevat tijdens de initialisatie van de database met het algemene schema- en referentiegegevens door een Bacpac-bestand te importeren. 
* **Initialiseren van de database met de basic tenant informatie**. Deze actie kunt u ook het type wilt, waarmee wordt bepaald van de foto gebruikt als de achtergrond op de website van de gebeurtenissen opgeven. 
* **Registreren van de database in de catalogusdatabase**. 

1. Open in PowerShell ISE *...\Learning Modules\ProvisionTenants\Demo-ProvisionAndCatalog.ps1* en stel **$Scenario = 2**. De catalogus tenant implementeren en registreren van de vooraf gedefinieerde tenants

1. Een onderbrekingspunt in het script toevoegen door een willekeurige plaats de cursor in de regel 49, plaatsen `& $PSScriptRoot\New-TenantApp.ps1`, en druk vervolgens op **F9**.
1. Voer het script door te drukken **F5**. 
1.  Nadat de uitvoering van het script stopt bij het onderbrekingspunt, drukt u op **F11** naar stap in het script New-Catalog.ps1.
1.  Het script worden uitgevoerd met de menuopties foutopsporing F10 en F11 traceren, om meer dan of in stap aangeroepen functies.

Nadat de tenant is ingericht, wordt de nieuwe tenant gebeurtenissen website geopend.

   ![Rode esdoorn race](media/saas-standaloneapp-provision-and-catalog/redmapleracing.png)

Vervolgens controleert u de nieuwe resources in de Azure portal gemaakt. 

   ![Rode esdoorn race resources](media/saas-standaloneapp-provision-and-catalog/redmapleracing-resources.png)


## <a name="to-stop-billing-delete-resource-groups"></a>Als u wilt stoppen facturering, resourcegroepen verwijderen ##

Verwijder alle resourcegroepen die u hebt gemaakt om de bijbehorende facturering stoppen wanneer u klaar bent met het verkennen van het voorbeeld.

## <a name="additional-resources"></a>Aanvullende resources

- Zie voor meer informatie over multitenant SaaS-databasetoepassingen, [ontwerppatronen voor multitenant SaaS-toepassingen](saas-tenancy-app-design-patterns.md).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Klik hier voor meer informatie over het implementeren van de zelfstandige Wingtip Tickets SaaS-toepassing.
> * Over de servers en databases die gezamenlijk de app.
> * Klik hier voor meer informatie over het verwijderen van de voorbeeldresources om te stoppen gerelateerde facturering.

Vindt u op hoe de catalogus wordt gebruikt ter ondersteuning van verschillende scenario's voor cross-tenant met behulp van de database per tenant-versie van de [Wingtip Tickets SaaS-toepassing](https://docs.microsoft.com/azure/sql-database/saas-dbpertenant-wingtip-app-overview).  
