---
title: Migreren van bestaande Azure Service Bus standard-naamruimten naar de premium-laag | Microsoft Docs
description: Handleiding voor het toestaan van de migratie van bestaande standaard Azure Service Bus-naamruimten naar premium
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: darosa
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/18/2019
ms.author: aschhab
ms.openlocfilehash: d2aa018566695b5b94cd8a7e64931a8b776b151d
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2019
ms.locfileid: "63766295"
---
# <a name="migrate-existing-azure-service-bus-standard-namespaces-to-the-premium-tier"></a>Migreren van bestaande Azure Service Bus standard-naamruimten naar de premium-laag
Azure Service Bus aangeboden eerder, naamruimten alleen op de standard-laag. Naamruimten worden instellingen voor meerdere tenants die zijn geoptimaliseerd voor lage doorvoer en ontwikkelaarsomgevingen. De premium-laag biedt toegewezen bronnen per naamruimte voor voorspelbare latentie en verbeterde doorvoer tegen een vaste prijs. De premium-laag is geoptimaliseerd voor hoge doorvoer en productieomgevingen waarvoor extra enterprise-functies.

Dit artikel wordt beschreven hoe u voor het migreren van bestaande naamruimten voor standard-laag naar de premium-laag.

>[!WARNING]
> Migratie is bedoeld voor Service Bus standard-naamruimten worden bijgewerkt naar de premium-laag. Het hulpprogramma voor migratie ondersteunt geen downgrade uitvoert.

Enkele van de die u moet weten: 
- Deze migratie is bedoeld om u te gebeuren aanwezig is, wat betekent dat bestaande afzender en de ontvanger toepassingen **niet vereist geen wijzigingen in code of configuratie**. De bestaande verbindingsreeks wordt automatisch verwijzen naar de nieuwe premium-naamruimte.
- De **premium** naamruimte moet **geen entiteiten** in voor de migratie te voltooien. 
- Alle **entiteiten** in de standard-naamruimte zijn **gekopieerd** naar de premium-naamruimte tijdens het migratieproces. 
- Migratie ondersteunt **1000 entiteiten per messaging-eenheid** op de premium-laag. Voor het identificeren van het aantal messaging-eenheden u moet beginnen met het aantal entiteiten die u op uw huidige standard-naamruimte hebt. 

## <a name="migration-steps"></a>Migratiestappen
Sommige voorwaarden zijn gekoppeld aan het migratieproces. Maak uzelf bekend met de volgende stappen uit om te verminderen de kans op fouten. Deze stappen wordt beschreven hoe u migratie en de stapsgewijze details worden weergegeven in de volgende secties.

1. Maak een nieuwe premium-naamruimte.
1. De standard en premium-naamruimten met elkaar worden gekoppeld.
1. Synchronisatie (kopie via) entiteiten van de norm voor de premium-naamruimte.
1. De migratie doorvoeren.
1. Leegmaken entiteiten in de standard-naamruimte met behulp van de naam na de migratie van de naamruimte.
1. De standard-naamruimte verwijderen.

>[!IMPORTANT]
> Nadat de migratie doorgevoerd is, toegang tot de oude standard-naamruimte en leegmaken van de wachtrijen en abonnementen. Nadat u hebt de berichten zijn geleegd, kunnen ze naar de nieuwe premium-naamruimte kan worden verwerkt door de toepassingen van de ontvanger verzonden. Nadat de wachtrijen en abonnementen is geleegd, wordt u aangeraden dat u de oude standard-naamruimte verwijdert.

### <a name="migrate-by-using-the-azure-cli-or-powershell"></a>Migreren met behulp van de Azure CLI of PowerShell

Als u wilt uw standard Service Bus-naamruimte migreren naar premium met behulp van de Azure CLI of PowerShell-hulpprogramma, de volgende stappen uit.

1. Maak een nieuwe Service Bus premium-naamruimte. U kunt verwijzen naar de [Azure Resource Manager-sjablonen](service-bus-resource-manager-namespace.md) of [gebruik van Azure portal](service-bus-create-namespace-portal.md). Zorg ervoor dat u selecteert **premium** voor de **serviceBusSku** parameter.

1. Stel de volgende omgevingsvariabelen voor het vereenvoudigen van de migratie-opdrachten.
   ```azurecli
   resourceGroup = <resource group for the standard namespace>
   standardNamespace = <standard namespace to migrate>
   premiumNamespaceArmId = <Azure Resource Manager ID of the premium namespace to migrate to>
   postMigrationDnsName = <post migration DNS name entry to access the standard namespace>
   ```

    >[!IMPORTANT]
    > De naam van de na de migratie (post_migration_dns_name) wordt gebruikt voor toegang tot de oude standard-naamruimte nadat de migratie. Gebruik deze verwijderen uit de wachtrijen en abonnementen, en verwijder vervolgens de naamruimte.

1. De standard en premium-naamruimten worden gekoppeld en de synchronisatie te starten met behulp van de volgende opdracht uit:

    ```azurecli
    az servicebus migration start --resource-group $resourceGroup --name $standardNamespace --target-namespace $premiumNamespaceArmId --post-migration-name $postMigrationDnsName
    ```


1. Controleer de status van de migratie met behulp van de volgende opdracht uit:
    ```azurecli
    az servicebus migration show --resource-group $resourceGroup --name $standardNamespace
    ```

    De migratie als voltooid beschouwd wanneer u de volgende waarden:
    * MigrationState = 'Actief'
    * pendingReplicationsOperationsCount = 0
    * provisioningState = "Geslaagd"

    Deze opdracht wordt ook weergegeven voor de configuratie voor de migratie. Controleer of de waarden correct zijn ingesteld. Controleer ook of de premium-naamruimte in de portal om te controleren of alle wachtrijen en onderwerpen zijn gemaakt en dat ze overeenkomen met wat aanwezig in de standard-naamruimte.

1. Voer de migratie door het uitvoeren van de volgende opdracht voltooid:
   ```azurecli
   az servicebus migration complete --resource-group $resourceGroup --name $standardNamespace
   ```

### <a name="migrate-by-using-the-azure-portal"></a>Migreren met behulp van de Azure-portal

Migratie met behulp van de Azure-portal heeft de dezelfde logische stroom als voor de migratie met behulp van de opdrachten. Volg deze stappen als u wilt migreren met behulp van de Azure-portal.

1. Op de **navigatie** menu in het linkerdeelvenster, selecteer **migreren naar premium**. Klik op de **aan de slag** om door te gaan naar de volgende pagina.
    ![Landingspagina van migratie][]

1. Volledige **Setup**.
   ![Setup-naamruimte][]
   1. Maken en toewijzen van de premium-naamruimte voor het migreren van de bestaande standard-naamruimte.
        ![Instellen van de naamruimte - premium-naamruimte maken][]
   1. Kies een **nadat de migratie naam**. U moet deze naam gebruiken voor toegang tot de standard-naamruimte nadat de migratie voltooid is.
        ![Instellen van de naamruimte - naam voor na de migratie kiezen][]
   1. Selecteer **'Volgende'** om door te gaan.
1. De entiteiten van de synchronisatie tussen de standard en premium-naamruimten.
    ![Begin van de naamruimte - entiteiten synchronisatie - instellingen][]

   1. Selecteer **synchronisatie starten** om te beginnen met de entiteiten worden gesynchroniseerd.
   1. Selecteer **Ja** in het dialoogvenster om te bevestigen en de synchronisatie starten.
   1. Wacht totdat de synchronisatie voltooid is. De status is beschikbaar op de statusbalk.
        ![De naamruimte - sync-entiteiten - voortgang van Setup][]
        >[!IMPORTANT]
        > Als u wilt het Breek de migratie af voor een bepaalde reden, raadpleegt u de stroom afbreken in de sectie Veelgestelde vragen van dit document.
   1. Nadat de synchronisatie voltooid is, selecteert u **volgende** aan de onderkant van de pagina.

1. Controleer de wijzigingen op de pagina overzicht. Selecteer **volledige migratie** om over te schakelen van naamruimten en om de migratie te voltooien.
    ![Overschakelen van de naamruimte - switch menu][] de bevestigingspagina die wordt weergegeven wanneer de migratie voltooid is.
    ![Switch-namespace - geslaagd][]

## <a name="faqs"></a>Veelgestelde vragen

### <a name="what-happens-when-the-migration-is-committed"></a>Wat gebeurt er wanneer de migratie doorgevoerd is?

Nadat de migratie doorgevoerd is, wordt de verbindingsreeks die waarnaar wordt verwezen naar de standard-naamruimte verwijzen naar de premium-naamruimte.

De afzender en ontvanger-toepassingen wordt de standaard Namespace verbreken en automatisch opnieuw verbinding te maken met de premium-naamruimte.

### <a name="what-do-i-do-after-the-standard-to-premium-migration-is-complete"></a>Wat moet ik doen als de standaard naar premium-migratie voltooid is?

De standaard naar premium migratie zorgt ervoor dat de metagegevens van een entiteit, zoals onderwerpen, abonnementen en filters van de standard-naamruimte worden gekopieerd naar de premium-naamruimte. De berichtgegevens die is toegewezen aan de standard-naamruimte is niet van de standard-naamruimte gekopieerd naar de premium-naamruimte.

De standard-naamruimte mogelijk enkele berichten die zijn verzonden en vastgelegd tijdens de migratie uitgevoerd is. Deze berichten van de standaard Namespace handmatig leegmaken en handmatig naar de premium-Namespace te verzenden. Als u wilt leegmaken handmatig de berichten, een console-app of een script dat verkeer naar de standard-naamruimte-entiteiten met behulp van de Post migratie DNS-naam die u hebt opgegeven in de migratie-opdrachten te gebruiken. Deze berichten verzenden naar de premium-naamruimte, zodat ze kunnen worden verwerkt door de ontvangers.

Nadat u hebt de berichten zijn geleegd, verwijdert u de standard-naamruimte.

>[!IMPORTANT]
> Nadat de berichten uit de standard-naamruimte hebt zijn geleegd, verwijdert u de standard-naamruimte. Dit is belangrijk omdat de verbindingsreeks die in eerste instantie waarnaar wordt verwezen naar de standard-naamruimte nu naar de premium-naamruimte verwijst. U hoeft niet de standaard Namespace meer. Verwijderen van de standard-naamruimte die u hebt gemigreerd, kunt voorkoming van verwarring hoger.

### <a name="how-much-downtime-do-i-expect"></a>Hoeveel downtime verwachten ik?
Het migratieproces is bedoeld om te beperken van de verwachte uitvaltijd voor de toepassingen. Uitvaltijd is verlaagd met behulp van de verbindingsreeks die de afzender en ontvanger-toepassingen gebruiken om te verwijzen naar de nieuwe premium-naamruimte.

De downtime die is opgetreden door de toepassing is beperkt tot de tijd die nodig zijn om bij te werken van de DNS-vermelding om te verwijzen naar de premium-naamruimte. De uitvaltijd is ongeveer 5 minuten.

### <a name="do-i-have-to-make-any-configuration-changes-while-doing-the-migration"></a>Heb ik configuratiewijzigingen aanbrengen tijdens de migratie?
Nee, er zijn geen code of configuratiewijzigingen die nodig zijn voor de migratie. De verbindingsreeks die de afzender en ontvanger-toepassingen gebruiken voor toegang tot de standaard Namespace is automatisch om te fungeren als een alias voor de premium-naamruimte toegewezen.

### <a name="what-happens-when-i-abort-the-migration"></a>Wat gebeurt er wanneer ik de migratie afbreken?
De migratie kan worden afgebroken met behulp van de `Abort` opdracht of met behulp van de Azure-portal. 

#### <a name="azure-cli"></a>Azure-CLI

```azurecli
az servicebus migration abort --resource-group $resourceGroup --name $standardNamespace
```

#### <a name="azure-portal"></a>Azure Portal

![Stroom afbreken - synchronisatie afbreken][]
![afbreken stroom - afbreken voltooid][]

Wanneer het migratieproces is afgebroken, annuleert het proces van het kopiëren van de entiteiten (onderwerpen, abonnementen en filters) van de standaard naar de premium-naamruimte en de koppeling verbroken.

De verbindingsreeks wordt niet bijgewerkt om te verwijzen naar de premium-naamruimte. Uw bestaande toepassingen blijven werken zoals voordat u de migratie is gestart.

Echter niet het verwijderen van de entiteiten in de premium-naamruimte of de premium-naamruimte verwijderen. Verwijder handmatig de entiteiten in als u had besloten geen te gaan met de migratie.

>[!IMPORTANT]
> Als u besluit om af te breken de migratie, verwijdert u de premium-Namespace die u voor de migratie heeft ingericht, zodat u niet in rekening voor de resources gebracht worden.

#### <a name="i-dont-want-to-have-to-drain-the-messages-what-do-i-do"></a>Ik wil niet verwijderen uit de berichten zijn. Wat moet ik doen?

Het is mogelijk dat er berichten die worden verzonden door de afzender en toegewezen aan de opslag op de standaard Namespace tijdens de migratie plaatsvindt en net voordat de migratie doorgevoerd is.

Tijdens de migratie is niet de werkelijke bericht/nettolading van de norm gekopieerd naar de premium-naamruimte. De berichten moeten worden handmatig geleegd en vervolgens wordt verzonden naar de premium-naamruimte.

Als u tijdens gepland onderhoud/housekeeping kunt migreren, en u niet wilt handmatig leegmaken en de berichten te verzenden, volgt u deze stappen:

1. Stop de afzender-toepassingen. De ontvanger toepassingen verwerkt de berichten die zich momenteel in de standard-naamruimte en wordt de wachtrij leegmaken.
1. Nadat de wachtrijen en abonnementen in de standard Namespace leeg zijn, volgt u de procedure die eerder is beschreven voor het uitvoeren van de migratie van de norm voor de premium-naamruimte.
1. Nadat de migratie voltooid is, kunt u de afzender-toepassingen opnieuw te starten.
1. De afzenders en ontvangers wordt nu automatisch verbinding maken met de premium-naamruimte.

    >[!NOTE]
    > U hoeft niet te stoppen van de ontvanger toepassingen voor de migratie.
    >
    > Nadat de migratie voltooid is, wordt de ontvanger toepassingen loskoppelen van de standard-naamruimte en wordt automatisch verbinding maken met de premium-naamruimte.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de [verschillen tussen standard en premium-Messaging](./service-bus-premium-messaging.md).
* Meer informatie over de [recovery aspecten van hoge beschikbaarheid en Geo-noodherstel voor Service Bus premium](service-bus-outages-disasters.md#protecting-against-outages-and-disasters---service-bus-premium).

[Landingspagina van migratie]: ./media/service-bus-standard-premium-migration/1.png
[Setup-naamruimte]: ./media/service-bus-standard-premium-migration/2.png
[Instellen van de naamruimte - premium-naamruimte maken]: ./media/service-bus-standard-premium-migration/3.png
[Instellen van de naamruimte - naam voor na de migratie kiezen]: ./media/service-bus-standard-premium-migration/4.png
[Begin van de naamruimte - entiteiten synchronisatie - instellingen]: ./media/service-bus-standard-premium-migration/5.png
[De naamruimte - sync-entiteiten - voortgang van Setup]: ./media/service-bus-standard-premium-migration/8.png
[Switch-namespace - switch-menu]: ./media/service-bus-standard-premium-migration/9.png
[Switch-namespace - geslaagd]: ./media/service-bus-standard-premium-migration/12.png

[Stroom afbreken - synchronisatie annuleren]: ./media/service-bus-standard-premium-migration/abort1.png
[Stroom afbreken - afbreken voltooid]: ./media/service-bus-standard-premium-migration/abort3.png
