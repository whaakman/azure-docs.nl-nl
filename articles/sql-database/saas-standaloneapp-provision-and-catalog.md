---
title: Multitenant SaaS-zelfstudie - Azure SQL Database | Microsoft Docs
description: Inrichten en catalogiseren van nieuwe tenants met behulp van het patroon voor zelfstandige toepassingen
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: billgib
manager: craigg
ms.date: 09/24/2018
ms.openlocfilehash: 28deb9b7ba15744b9bd3d273d02db4398d2b2ef3
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57896939"
---
# <a name="provision-and-catalog-new-tenants-using-the--application-per-tenant-saas-pattern"></a>Inrichten en catalogiseren van nieuwe tenants met behulp van de toepassing per tenant SaaS-patroon

In dit artikel bevat informatie over het inrichten en catalogiseren van nieuwe tenants met behulp van de zelfstandige app per tenant SaaS-patroon.
In dit artikel heeft twee hoofdonderdelen:
* Algemene discussie van het inrichten en catalogiseren van nieuwe tenants
* Een zelfstudie waarin PowerShell-voorbeeldcode die wordt bereikt van het inrichten en catalogiseren
    * De zelfstudie maakt gebruik van de Wingtip Tickets SaaS-voorbeeldtoepassing, aangepast aan de zelfstandige app per tenant patroon.

## <a name="standalone-application-per-tenant-pattern"></a>Zelfstandige toepassing per tenant-patroon

De zelfstandige app per tenant patroon is een van verschillende patronen voor SaaS-toepassingen met meerdere tenants.  In dit patroon is een zelfstandige app ingericht voor elke tenant. De toepassing bestaat uit niveau toepassingsonderdelen en een SQL-database.  Elke tenant-app kan worden geïmplementeerd in het abonnement van de leverancier.  U kunt ook Azure biedt een [beheerde toepassingen programma](https://docs.microsoft.com/azure/managed-applications/overview) in die een app kan worden geïmplementeerd in het abonnement van een tenant en beheerd door de leverancier voor andere gebruikers van de tenant. 

   ![App-per-tenant-patroon](media/saas-standaloneapp-provision-and-catalog/standalone-app-pattern.png)

Bij het implementeren van een toepassing voor een tenant, worden de app en database ingericht in een nieuwe resourcegroep gemaakt voor de tenant.  Met behulp van afzonderlijke resourcegroepen worden geïsoleerd van elke tenant toepassingsresources en kan ze onafhankelijk worden beheerd. Elk exemplaar is in elke resourcegroep geconfigureerd voor het rechtstreeks toegang hebben tot de bijbehorende database.  Dit model verbinding in tegenstelling tot andere patronen die gebruikmaken van een catalogus broker-verbindingen tussen de app en de database.  En omdat er geen resources te delen, elke tenantdatabase moet worden ingericht met voldoende bronnen zijn voor het verwerken van de piekbelasting. Dit patroon vaak moet worden gebruikt voor SaaS-toepassingen met minder tenants, waarbij er een nadruk op tenant isolatie en minder nadruk op de kosten van resources.  

## <a name="using-a-tenant-catalog-with-the-application-per-tenant-pattern"></a>Met behulp van een tenantcatalogus met de toepassing per tenant-patroon

Hoewel van elke tenant-app en database volledig geïsoleerde zijn, kunnen verschillende beheer- en analysescenario's voor tenants worden uitgevoerd.  Bijvoorbeeld, vereist de schemawijziging van een voor een nieuwe versie van de toepassing toe te passen wijzigingen in het schema van elke tenantdatabase. Scenario's voor rapportage en analyse mogelijk ook toegang tot de tenant-databases, ongeacht waar ze zijn geïmplementeerd.

   ![App-per-tenant-patroon](media/saas-standaloneapp-provision-and-catalog/standalone-app-pattern-with-catalog.png)

De tenantcatalogus bevat een toewijzing tussen een tenant-id en een tenantdatabase zodat u een id die moet worden omgezet naar een servernaam en databasenaam.  De tenant-id wordt berekend als een hash van de naam van de tenant in de Wingtip SaaS-app, maar andere schema's kunnen worden gebruikt.  Terwijl zelfstandige toepassingen niet nodig voor de catalogus hebt voor het beheren van verbindingen, kan de catalogus worden gebruikt als bereik voor andere bewerkingen voor een reeks tenantdatabases. Elastische query's kunnen bijvoorbeeld de catalogus gebruiken om te bepalen welke databases waarvoor query's worden gedistribueerd voor cross-tenant-rapportage.

## <a name="elastic-database-client-library"></a>Clientbibliotheek voor Elastic Database

In de voorbeeldtoepassing Wingtip de catalogus wordt geïmplementeerd door de shard-beheerfuncties van de [elastische Database-clientbibliotheek](sql-database-elastic-database-client-library.md) (EDCL).  De bibliotheek kan een toepassing te maken, beheren en gebruiken van een shard-toewijzing die zijn opgeslagen in een database. In het voorbeeld van Wingtip Tickets, de catalogus wordt opgeslagen de *tenantcatalogus* database.  De shard-kaarten een tenantsleutel voor de shard (database) in die van de tenant-gegevens worden opgeslagen.  EDCL functies beheren een *globale shard-toewijzing* die zijn opgeslagen in tabellen in de *tenantcatalogus* database en een *lokale shard-toewijzing* die zijn opgeslagen in elke shard.

EDCL functies kunnen worden aangeroepen vanuit toepassingen of PowerShell-scripts maken en beheren van de vermeldingen in de shard-toewijzing. Andere functies EDCL kunnen worden gebruikt om de set met shards ophalen of verbinding maken met de juiste database voor de opgegeven tenant-sleutel. 

> [!IMPORTANT]
> Niet de gegevens in de catalogusdatabase of de lokale shard-toewijzing in de tenant-databases rechtstreeks bewerken. Automatische updates worden niet ondersteund vanwege het hoge risico van gegevensbeschadiging. Bewerken in plaats daarvan de toewijzingsgegevens alleen via EDCL API's.

## <a name="tenant-provisioning"></a>Tenants worden ingericht 

Elke tenant moet een nieuwe Azure-resourcegroep, die moet worden gemaakt voordat u resources binnen deze kunnen worden ingericht. Nadat de resourcegroep bestaat, kan een Azure Resource Manager-sjabloon worden gebruikt om de implementatie van de toepassingsonderdelen en de database en configureer vervolgens verbinding met de database. Voor het initialiseren van het databaseschema, kunt de sjabloon een bacpac-bestand importeren.  U kunt ook kan de database worden gemaakt als een kopie van een 'template'-database.  De database is vervolgens meer bijgewerkt met gegevens van de oorspronkelijke locatie en geregistreerd in de catalogus.

## <a name="tutorial"></a>Zelfstudie

In deze zelfstudie leert u het volgende:

* Een catalogus inrichten
* De voorbeeld-tenant-databases die u hebt geïmplementeerd eerder in de catalogus registreren
* Een extra tenant in te richten en deze te registreren in de catalogus

Een Azure Resource Manager-sjabloon wordt te implementeren en configureren van de toepassing, de tenant-database maken en vervolgens een bacpac-bestand voor het initialiseren van het importeren gebruikt. De import-aanvraag kan enkele minuten voordat het mailbericht is in de wachtrij geplaatst.

Aan het einde van deze zelfstudie hebt u een set toepassingen voor zelfstandige tenant, met elke database die is geregistreerd in de catalogus.

## <a name="prerequisites"></a>Vereisten

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan: 

* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.
* De voorbeeld-apps voor drie tenant worden geïmplementeerd. Zie voor het implementeren van deze apps in minder dan vijf minuten [implementeren en verkennen van het patroon Wingtip Tickets SaaS-toepassing voor zelfstandige](saas-standaloneapp-get-started-deploy.md).

## <a name="provision-the-catalog"></a>Inrichten van de catalogus

In deze taak leert u hoe u voor het inrichten van de catalogus gebruikt voor het registreren van alle tenantdatabases. U gaat het volgende doen: 

* **Inrichten van de catalogusdatabase** met een Azure resource Manager-sjabloon. De database is geïnitialiseerd door een bacpac-bestand te importeren.  
* **De voorbeeld-apps voor tenant registreren** die u eerder hebt geïmplementeerd.  Elke tenant is geregistreerd met behulp van een sleutel die is samengesteld uit een hash van de naam van de tenant.  Naam van de tenant wordt ook opgeslagen in een tabel van de extensie in de catalogus.

1. Open in PowerShell ISE, *...\Learning Modules\UserConfig.psm* en werk de **\<gebruiker\>** waarde aan de waarde die u hebt gebruikt bij het implementeren van de drie voorbeeldtoepassingen.  **Sla het bestand**.  
1. Open in PowerShell ISE, *...\Learning Modules\ProvisionTenants\Demo-ProvisionAndCatalog.ps1* en stel **$Scenario = 1**. Implementeren van de tenantcatalogus en registreer de vooraf gedefinieerde tenants.

1. Voeg een onderbrekingspunt toe door de cursor ergens op de regel die `& $PSScriptRoot\New-Catalog.ps1`, en druk vervolgens op **F9**.

    ![instellen van een onderbrekingspunt voor tracering](media/saas-standaloneapp-provision-and-catalog/breakpoint.png)

1. Voer het script uit door te drukken **F5**. 
1.  Nadat de uitvoering van het script wordt gestopt op het onderbrekingspunt, drukt u op **F11** naar stap in het script New-Catalog.ps1.
1.  Uitvoering van het script met de menuopties foutopsporing, F10 en F11 traceren, om meer dan of in functies genoemd.
    *   Zie voor meer informatie over het opsporen van fouten in PowerShell-scripts, [Tips over het werken met en het opsporen van fouten in PowerShell-scripts](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).

Nadat het script is voltooid, wordt de catalogus wordt bestaan en alle voorbeeldtenants wordt geregistreerd. 

Bekijk nu de resources die u hebt gemaakt.

1. Open de [Azure-portal](https://portal.azure.com/) en bladert u de resourcegroepen.  Open de **wingtip-sa-catalog -\<gebruiker\>**  resource groeperen en noteer de catalog-server en database.
1. De database openen in de portal en selecteer *Data explorer* in het menu links.  Klik op de opdracht voor aanmelden uit en voer het wachtwoord = **P\@ssword1**.


1. Verken het schema van de *tenantcatalog* database.  
   * De objecten in de `__ShardManagement` schema zijn opgegeven door de elastische Database-clientbibliotheek.
   * De `Tenants` tabel en `TenantsExtended` weergeven zijn toegevoegd in het voorbeeld extensies die laten zien hoe u de catalogus voor extra waarde kunt uitbreiden.
1. De query uitvoert, `SELECT * FROM dbo.TenantsExtended`.          

   ![Data explorer](media/saas-standaloneapp-provision-and-catalog/data-explorer-tenantsextended.png)

    Als alternatief voor het gebruik van Data Explorer kunt u verbinding met de database van SQL Server Management Studio. U doet dit door de verbinding met de server wingtip- 

    
    Houd er rekening mee dat u moet niet Bewerk de gegevens rechtstreeks in de catalogus - gebruik altijd de shard management API's. 

## <a name="provision-a-new-tenant-application"></a>Een nieuwe toepassing met tenants inrichten

In deze taak leert u hoe u een toepassing met één tenant inricht. U gaat het volgende doen:  

* **Maak een nieuwe resourcegroep** voor de tenant. 
* **Richt de toepassings- en database** in de nieuwe resourcegroep met een Azure resource Manager-sjabloon.  Deze actie bevat tijdens de initialisatie van de database met schema- en referentiegegevens door een bacpac-bestand te importeren. 
* **Initialiseren van de database met informatie over eenvoudige tenant**. Deze actie omvat het type venue, waarmee wordt bepaald hoe de foto gebruikt als de achtergrond op de website van de gebeurtenissen op te geven. 
* **Registreren van de database in de catalogusdatabase**. 

1. Open in PowerShell ISE, *...\Learning Modules\ProvisionTenants\Demo-ProvisionAndCatalog.ps1* en stel **$Scenario = 2**. De tenantcatalogus implementeren en registreren van de vooraf gedefinieerde tenants

1. Voeg een onderbrekingspunt toe in het script door de cursor ergens op regel 49 die `& $PSScriptRoot\New-TenantApp.ps1`, en druk vervolgens op **F9**.
1. Voer het script uit door te drukken **F5**. 
1.  Nadat de uitvoering van het script wordt gestopt op het onderbrekingspunt, drukt u op **F11** naar stap in het script New-Catalog.ps1.
1.  Uitvoering van het script met de menuopties foutopsporing, F10 en F11 traceren, om meer dan of in functies genoemd.

Nadat de tenant is ingericht, wordt de nieuwe tenant gebeurtenissen website wordt geopend.

   ![Red maple racing](media/saas-standaloneapp-provision-and-catalog/redmapleracing.png)

Vervolgens controleert u de nieuwe resources gemaakt in Azure portal. 

   ![Red maple racing resources](media/saas-standaloneapp-provision-and-catalog/redmapleracing-resources.png)


## <a name="to-stop-billing-delete-resource-groups"></a>Als u wilt stoppen met facturering, verwijdert u resourcegroepen

Wanneer u klaar bent met het voorbeeld verkennen, verwijdert alle resourcegroepen die u hebt gemaakt om de bijbehorende facturering te stoppen.

## <a name="additional-resources"></a>Aanvullende resources

- Zie voor meer informatie over multitenant SaaS-databasetoepassingen, [ontwerppatronen voor SaaS-toepassingen met meerdere tenants](saas-tenancy-app-design-patterns.md).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Hoe u de zelfstandige Wingtip Tickets SaaS-toepassing implementeert.
> * Over de servers en databases waaruit de app.
> * Klik hier voor meer informatie over het verwijderen van de voorbeeldresources om gerelateerde facturering te stoppen.

U kunt ontdekken hoe de catalogus wordt gebruikt ter ondersteuning van verschillende scenario's voor alle tenants met behulp van de database-per-tenant-versie van de [Wingtip Tickets SaaS-toepassing](saas-dbpertenant-wingtip-app-overview.md).  
