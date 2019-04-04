---
title: Bestaande Azure Service Bus Standard-naamruimten migreren naar Premium-laag | Microsoft Docs
description: Handleiding voor het toestaan van de migratie van bestaande Azure Service Bus Standard-naamruimten naar Premium
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
ms.openlocfilehash: 7b153c36e10f1d4e2be2a0cf42f998c31cb6473a
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896424"
---
# <a name="migrate-existing-azure-service-bus-standard-namespaces-to-premium-tier"></a>Bestaande Azure Service Bus Standard-naamruimten migreren naar Premium-laag

Azure Service Bus aangeboden eerder, naamruimten alleen op de Standard-laag. Deze zijn instellingen met meerdere tenants die zijn geoptimaliseerd voor lage doorvoer en ontwikkelaarsomgevingen.

In het verleden recente is Azure Service Bus uitgebreid naar de Premium-laag biedt toegewezen bronnen per naamruimte voor voorspelbare latentie en verbeterde doorvoer tegen een vaste prijs die is geoptimaliseerd voor hoge doorvoer en productie-omgevingen bieden vereist dat extra enterprise-functies.

De onderstaande hulpprogramma's kunnen bestaande Standard-laag de naamruimten worden gemigreerd naar de Premium-laag.

>[!WARNING]
> Migratie is bedoeld voor Service Bus Standard-naamruimte als ***bijgewerkt*** naar de laag Premium.
>
> De migratie-tooling ***niet*** downgrade.
>[!NOTE]
> Deze migratie is bedoeld om u te gebeuren ***erin***.
>
> Dit houdt in dat bestaande afzender en toepassingen van de ontvanger wijzigingen code of configuratie vereisen.
>
> De bestaande verbindingsreeks wordt automatisch verwijzen naar de nieuwe premium-naamruimte.
>
> Bovendien alle entiteiten in de Standard-naamruimte, **gekopieerd** in de Premium-naamruimte tijdens het migratieproces.
>
>
> We bieden ondersteuning voor ***1000 entiteiten per Messaging-eenheid*** op Premium, zodat u nodig hebt om te bepalen hoeveel Messaging-eenheden, start met het aantal entiteiten die u op uw huidige Standard-naamruimte hebt.

## <a name="migration-steps"></a>Migratiestappen

>[!IMPORTANT]
> Er zijn enkele waarschuwingen die zijn gekoppeld aan het migratieproces. We verzoeken u volledig raken met de stappen voor het te verminderen van de mogelijkheden van fouten.

Het migratieproces van concrete stap voor stap wordt beschreven in de onderstaande handleidingen.

De logische stappen zijn:

1. Maak een nieuwe Premium-naamruimte.
2. De standaard en Premium-naamruimte met elkaar worden gekoppeld.
3. Synchronisatie (kopie via)-entiteiten van Standard naar Premium-naamruimte
4. De migratie doorvoeren
5. Entiteiten in de Standard-naamruimte met de naam na de migratie van de naamruimte leegmaken
6. De Standard-naamruimte verwijderen

>[!NOTE]
> Zodra de migratie doorgevoerd is, is het erg belangrijk voor toegang tot de oude Standard-naamruimte en corrigeren van de wachtrijen en abonnementen.
>
> Zodra de berichten zijn is geleegd uit kunnen ze worden verwerkt door de ontvanger toepassingen naar de nieuwe premium-naamruimte worden verzonden.
>
> Zodra de wachtrijen en abonnementen is geleegd, wordt u aangeraden de oude Standard-naamruimte verwijderen. U wordt niet worden hoeven deze!

### <a name="migrate-using-azure-cli-or-powershell"></a>Migreren met behulp van Azure CLI of PowerShell

Als u wilt uw Service Bus Standard-naamruimte migreren naar Premium met het hulpprogramma Azure CLI of PowerShell, raadpleegt u de volgende handleiding.

1. Maak een nieuwe Service Bus Premium-naamruimte. U kunt verwijzen naar de [Azure Resource Manager-sjablonen](service-bus-resource-manager-namespace.md) of [gebruik van Azure portal](service-bus-create-namespace-portal.md). Zorg ervoor dat u "Premium" Selecteer voor de **serviceBusSku** parameter.

2. Stel de volgende omgevingsvariabelen voor het vereenvoudigen van de migratie-opdrachten.
   ```
   resourceGroup = <resource group for the standard namespace>
   standardNamespace = <standard namespace to migrate>
   premiumNamespaceArmId = <Azure Resource Manager ID of the Premium namespace to migrate to>
   postMigrationDnsName = <post migration DNS name entry to access the Standard namespace>
   ```

    >[!IMPORTANT]
    > De naam van de na de migratie (post_migration_dns_name) wordt gebruikt voor toegang tot de oude Standard-naamruimte nadat de migratie. U moet dit gebruiken voor het corrigeren van de wachtrijen en abonnementen en verwijder vervolgens de naamruimte.

3. **Paar** de standaard en Premium-naamruimten en **synchronisatie starten** met behulp van de onderstaande opdracht:

    ```
    az servicebus migration start --resource-group $resourceGroup --name $standardNamespace --target-namespace $premiumNamespaceArmId --post-migration-name $postMigrationDnsName
    ```


4. Controleer de status van het gebruik van de migratie van de onderstaande opdracht:
    ```
    az servicebus migration show --resource-group $resourceGroup --name $standardNamespace
    ```

    De migratie wordt als voltooid beschouwd wanneer
    * MigrationState = 'Actief'
    * pendingReplicationsOperationsCount = 0
    * provisioningState = "Geslaagd"

    Deze opdracht wordt ook weergegeven voor de configuratie voor de migratie. . Controleer om ervoor te zorgen dat de waarden zijn ingesteld als vorige gedeclareerd.

    Daarnaast ook controleren of de Premium-naamruimte in de portal om ervoor te zorgen dat alle wachtrijen en onderwerpen zijn gemaakt en dat ze overeenkomen met wat waren aanwezig op de Standard-naamruimte.

5. Voer de migratie door het uitvoeren van de onderstaande opdracht voltooid
   ```
   az servicebus migration complete --resource-group $resourceGroup --name $standardNamespace
   ```

### <a name="migrate-using-azure-portal"></a>Migreren met behulp van Azure portal

Migreren via Azure portal heeft de dezelfde logische stroom als voor de migratie met behulp van de opdrachten. Raadpleeg de volgende handleiding voor het stapsgewijze proces voor het migreren met behulp van de portal.

1. Kies de **'Migreren naar Premium'** menuoptie in het navigatiemenu in het linkerdeelvenster. Klik op de **'Aan de slag'** om door te gaan naar de volgende pagina.
    ![Landingspagina van migratie][]

2. Volledige **Setup**.
   ![Setup-naamruimte][]
   1. Maken en toewijzen van de Premium-naamruimte voor het migreren van de bestaande Standard-naamruimte.
        ![Instellen van de naamruimte - premium-naamruimte maken][]
   2. Kies de **'Nadat de migratie-name'** voor toegang tot de Standard-naamruimte door nadat de migratie voltooid is.
        ![Instellen van de naamruimte - naam voor na de migratie kiezen][]
   3. Klik op **'Volgende'** om door te gaan.
3. **Synchronisatie** entiteiten tussen de Standard en Premium-naamruimte.
    ![Begin van de naamruimte - entiteiten synchronisatie - instellingen][]

   1. Klik op **synchronisatie starten** om te beginnen met de entiteiten worden gesynchroniseerd.
   2. Klik op **'Ja'** in het pop-upvenster bevestigen voor het starten van de synchronisatie.
   3. Wacht totdat de **synchronisatie** is voltooid. De status is beschikbaar op de statusbalk.
        ![De naamruimte - sync-entiteiten - voortgang van Setup][]
        >[!IMPORTANT]
        > Als u wilt **afbreken** voor een bepaalde reden, raadpleegt u de stroom afbreken in de sectie Veelgestelde vragen van dit document.
   4. Nadat de synchronisatie voltooid is, klikt u op de **'Volgende'** knop aan de onderkant van de pagina.

4. Controleer de wijzigingen op de pagina overzicht.
    ![Switch-namespace - switch-menu][]

5. Klik op **'Volledige migratie'** overschakelen van naamruimten en de migratie voltooien.
    ![Switch-namespace - geslaagd][]

## <a name="faqs"></a>Veelgestelde vragen

### <a name="what-happens-when-the-migration-is-committed"></a>Wat gebeurt er wanneer de migratie doorgevoerd is?

Nadat de migratie doorgevoerd is, wordt de verbindingsreeks die waarnaar wordt verwezen naar de Standard-naamruimte verwijzen naar de Premium-naamruimte.

De afzender en ontvanger-toepassingen wordt de standaard Namespace verbreken en automatisch opnieuw verbinding te maken met de Premium-naamruimte.

### <a name="what-do-i-do-after-the-standard-to-premium-migration-is-complete"></a>Wat moet ik doen als de standaard naar Premium-migratie voltooid is?

De standaard naar Premium migratie zorgt ervoor dat de metagegevens van een entiteit (onderwerpen, abonnementen, filters, markt) zijn gekopieerd van de standaard naar Premium-naamruimte. De berichtgegevens die is toegewezen aan de Standard-naamruimte is niet gekopieerd van de standaard naar Premium-naamruimte.

De Standard-naamruimte mogelijk hiervan enkele berichten die zijn verzonden en vastgelegd tijdens de migratie uitgevoerd is. Deze berichten moeten handmatig uit de Standard Namespace geleegd en via verzonden naar de Premium-Namespace handmatig.

Hiervoor u ***moet*** gebruiken een console-app of een script dat leegmaken van de Standard-naamruimte-entiteiten met behulp van de **na migratie DNS-naam** opgegeven in de migratie-opdrachten uit te voeren en deze vervolgens te verzenden berichten op de Premium-Namespace, zodat ze kunnen worden verwerkt door de ontvangers.

Nadat u hebt de berichten zijn geleegd, gaat u naar de Standard-naamruimte verwijderen.

>[!IMPORTANT]
> Houd er rekening mee dat zodra de berichten uit de Standard-naamruimte hebt zijn geleegd, u **moet** verwijderen van de Standard-naamruimte.
>
> Dit is belangrijk omdat de verbindingsreeks die in eerste instantie waarnaar wordt verwezen naar de Standard-naamruimte nu eigenlijk naar de Premium-naamruimte verwijst. U wordt niet worden hoeven deze standaard Namespace meer.
>
> Verwijderen van de Standard-naamruimte die u helpt gemigreerd verwarring op een later tijdstip. 

### <a name="how-much-downtime-do-i-expect"></a>Hoeveel downtime verwachten ik?
Het migratieproces die hierboven worden beschreven is bedoeld om te beperken van de verwachte uitvaltijd voor de toepassingen. Dit wordt gedaan door het gebruik van de verbindingsreeks die de afzender en ontvanger-toepassingen gebruiken om te verwijzen naar de nieuwe Premium-naamruimte.

De ervaren door de toepassing downtime is beperkt tot de hoeveelheid tijd die nodig is om bij te werken van de DNS-vermelding om te verwijzen naar de Premium-naamruimte.

Dit kan worden aangenomen moet ***ongeveer 5 minuten***.

### <a name="do-i-have-to-make-any-configuration-changes-while-performing-the-migration"></a>Heb ik configuratiewijzigingen aanbrengen tijdens het uitvoeren van de migratie?
Nee, er zijn geen wijzigingen in de code/configuratie die nodig zijn voor het uitvoeren van deze migratie. De verbindingsreeks die de afzender en ontvanger-toepassingen gebruiken voor toegang tot de standaard Namespace automatisch is toegewezen om te fungeren als een **alias** voor de Premium-Namespace.

### <a name="what-happens-when-i-abort-the-migration"></a>Wat gebeurt er wanneer ik de migratie afbreken?
Migratie kunt afgesloten met de opdracht 'Afbreken' of via de Azure portal. 

#### <a name="azure-cli-or-powershell"></a>Azure CLI of PowerShell

    az servicebus migration abort --resource-group $resourceGroup --name $standardNamespace

#### <a name="azure-portal"></a>Azure Portal

![Stroom afbreken - synchronisatie afbreken][]
![afbreken stroom - afbreken voltooid][]

Wanneer het migratieproces is afgebroken, het daadwerkelijk annuleert het proces van het kopiëren van over de entiteiten (onderwerpen, abonnementen en filters) van Standard naar Premium-naamruimte en de koppeling verbroken.

De verbindingsreeks **is niet** bijgewerkt om te verwijzen naar de Premium-naamruimte. Uw bestaande toepassingen blijven werken zoals voordat u de migratie is gestart.

Echter, het **niet** de entiteiten in de Premium-naamruimte of de Premium-naamruimte zelf verwijderen. Dit moet handmatig worden uitgevoerd als u had besloten om niet verdergaat met de migratie nadat alle.

>[!IMPORTANT]
> Als u besluit om af te breken de migratie, verwijder de Premium-Namespace die u had ingericht voor de migratie, zodat u niet in rekening voor de resources gebracht worden.

#### <a name="i-dont-want-to-have-to-drain-the-messages-what-do-i-do"></a>Ik wil niet verwijderen uit de berichten zijn. Wat moet ik doen?

Mogelijk zijn er berichten die worden verzonden door de afzender en toegewezen aan de opslag op de standaard Namespace tijdens de migratie plaatsvindt en precies vóór de migratie is doorgevoerd.

Gezien het feit dat tijdens de migratie, de werkelijke bericht/nettolading is niet gekopieerd van Standard naar Premium, moeten deze worden handmatig geleegd en vervolgens wordt verzonden naar de Premium-naamruimte.

Echter als u tijdens gepland onderhoud/housekeeping migreren kunt en niet wilt dat handmatig leegmaken en de berichten te verzenden, kunt u Volg de onderstaande stappen -

1. Stop de afzender-toepassingen en de ontvangers voor het verwerken van de berichten die zich momenteel in de Standard-naamruimte en corrigeren van de wachtrij staan.
2. Zodra de wachtrijen en abonnementen in de Standard Namespace leeg zijn, volgt u de procedure voor het uitvoeren van de migratie van Standard naar Premium-naamruimte die hierboven worden beschreven.
3. Nadat de migratie voltooid is, kunt u de afzender-toepassingen opnieuw te starten.
4. De afzenders en ontvangers wordt nu automatisch verbinding maken met de Premium-naamruimte.

    >[!NOTE]
    > De ontvanger moet niet worden gestopt voor de migratie.
    >
    > Nadat de migratie voltooid is, wordt de ontvangers loskoppelen van de Standard-naamruimte en wordt automatisch verbinding maken met de Premium-naamruimte.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de [verschillen tussen Standard en Premium Messaging](./service-bus-premium-messaging.md)
* Meer informatie over de hoge beschikbaarheid en Geo-Diaster recovery aspecten voor Service Bus Premium [hier](service-bus-outages-disasters.md#protecting-against-outages-and-disasters---service-bus-premium)

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
