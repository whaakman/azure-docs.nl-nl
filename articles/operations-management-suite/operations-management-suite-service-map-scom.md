---
title: Serviceoverzicht integratie met System Center Operations Manager | Microsoft Docs
description: Serviceoverzicht is een Operations Management Suite-oplossing die automatisch de onderdelen van toepassing op Windows- en Linux-systemen worden gedetecteerd en de communicatie tussen services wordt toegewezen. Dit artikel wordt het automatisch maken van gedistribueerde toepassing diagrammen in Operations Manager via een Serviceoverzicht.
services: operations-management-suite
documentationcenter: 
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: e8614a5a-9cf8-4c81-8931-896d358ad2cb
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2017
ms.author: bwren;dairwin
ms.openlocfilehash: af1f683f08ff6b70b23ff265f39b9a76f92f4be2
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="service-map-integration-with-system-center-operations-manager"></a>Serviceoverzicht integratie met System Center Operations Manager
  > [!NOTE]
  > Deze functie is openbare preview.
  > 
  
Operations Management Suite Serviceoverzicht automatisch detecteert de onderdelen van toepassing op Windows- en Linux-systemen en de communicatie tussen services wordt toegewezen. Serviceoverzicht kunt u de manier waarop u deze beschouwen als onderling verbonden systemen die essentiële services bieden voor uw servers weergeven. Service-kaart toont de verbindingen tussen servers, processen en poorten voor elke architectuur TCP-verbinding zonder configuratie vereist naast de installatie van een agent. Zie voor meer informatie de [Serviceoverzicht documentatie](operations-management-suite-service-map.md).

Met deze integratie tussen Serviceoverzicht en System Center Operations Manager, kunt u automatisch diagrammen voor gedistribueerde toepassing maken in Operations Manager die zijn gebaseerd op de dynamische afhankelijkheid toewijzingen in Serviceoverzicht.

## <a name="prerequisites"></a>Vereisten
* Een Operations Manager-beheergroep (2012 R2 of hoger) die wordt beheerd door een reeks servers.
* Een Operations Management Suite-werkruimte met de oplossing Serviceoverzicht is ingeschakeld.
* Een set servers (ten minste één) die worden beheerd door Operations Manager en verzenden van gegevens naar Serviceoverzicht. Windows en Linux-servers worden ondersteund.
* Een service-principal met toegang tot de Azure-abonnement dat is gekoppeld aan de Operations Management Suite-werkruimte. Ga voor meer informatie naar [maken van een service-principal](#creating-a-service-principal).

## <a name="install-the-service-map-management-pack"></a>Installeer het Serviceoverzicht management pack
U inschakelen de integratie tussen Operations Manager en Serviceoverzicht door het importeren van de Microsoft.SystemCenter.ServiceMap management pack-bundel (Microsoft.SystemCenter.ServiceMap.mpb). U kunt downloaden via het management pack-bundel van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=55763). De bundel bevat de volgende management packs:
* Microsoft Service kaart Toepassingweergaven
* Microsoft System Center Serviceoverzicht interne
* Microsoft System Center Service kaart onderdrukkingen
* Microsoft System Center-Serviceoverzicht

## <a name="configure-the-service-map-integration"></a>Configureren van de integratie Serviceoverzicht
Nadat u een nieuw knooppunt van het Serviceoverzicht management pack **Serviceoverzicht**, wordt weergegeven onder **Operations Management Suite** in de **beheer** deelvenster. 

Serviceoverzicht om integratie te configureren, het volgende doen:

1. De configuratiewizard openen in de **kaart Serviceoverzicht** deelvenster, klikt u op **werkruimte toevoegen**.  

    ![Deelvenster Overzicht van de service-kaart](media/oms-service-map/scom-configuration.png)

2. In de **verbindingsconfiguratie** venster, voer de tenantnaam of -ID, toepassings-ID (ook wel bekend als de gebruikersnaam of het clientID) en het wachtwoord van de service-principal en klik vervolgens op **volgende**. Ga voor meer informatie naar [maken van een service-principal](#creating-a-service-principal).

    ![Het venster verbindingsconfiguratie](media/oms-service-map/scom-config-spn.png)

3. In de **abonnement selectie** venster de Azure-abonnement, de Azure-resourcegroep (de knop met de Operations Management Suite-werkruimte) en de Operations Management Suite-werkruimte selecteren en klik vervolgens op **volgende**.

    ![De configuratie-werkruimte van Operations Manager](media/oms-service-map/scom-config-workspace.png)

4. In de **groepsselectie Machine** venster u kiezen welke kaart computergroepen u wilt synchroniseren met Operations Manager. Klik op **computergroepen toevoegen/verwijderen**, kies de groepen uit de lijst met **beschikbaar computergroepen**, en klik op **toevoegen**.  Wanneer u klaar bent met groepen te selecteren, klikt u op **Ok** te voltooien.
    
    ![De Operations-Machine van de Configuration Manager-groepen](media/oms-service-map/scom-config-machine-groups.png)
    
5. In de **serverselectie** venster u de groep Servers-Service kaart configureren met de servers die u wilt synchroniseren tussen Operations Manager en Service-kaart. Klik op **Servers toevoegen of verwijderen**.   
    
    Voor de integratie voor het bouwen van een gedistribueerde toepassing diagram voor een server, moet de server:

    * Beheerd door Operations Manager
    * Beheerd door Serviceoverzicht
    * Vermeld in de groep Servers van Service-kaart

    ![De Operations Manager Configuration-servicegroep](media/oms-service-map/scom-config-group.png)

6. Optioneel: Selecteer de resourcegroep voor de beheerserver om te communiceren met Operations Management Suite en klik vervolgens op **werkruimte toevoegen**.

    ![De Operations Manager configuratie resourcegroep](media/oms-service-map/scom-config-pool.png)

    Het duurt even configureren en registreren van de Operations Management Suite-werkruimte. Nadat deze is geconfigureerd, wordt de eerste synchronisatie Serviceoverzicht van Operations Management Suite gestart in Operations Manager.

    ![De Operations Manager configuratie resourcegroep](media/oms-service-map/scom-config-success.png)


## <a name="monitor-service-map"></a>Monitor Serviceoverzicht
Nadat de Operations Management Suite-werkruimte is aangesloten, een nieuwe map Serviceoverzicht wordt weergegeven in de **bewaking** deelvenster van de Operations Manager-console.

![Het deelvenster controle van Operations Manager](media/oms-service-map/scom-monitoring.png)

De map Serviceoverzicht heeft vier knooppunten:
* **Actieve waarschuwingen**: geeft een lijst van alle actieve waarschuwingen over de communicatie tussen Operations Manager en Service-kaart.  Houd er rekening mee dat deze waarschuwingen niet zijn dat Operations Management Suite waarschuwingen wordt gesynchroniseerd met Operations Manager. 

* **Servers**: geeft een lijst van de bewaakte servers die zijn geconfigureerd om te synchroniseren van Serviceoverzicht.

    ![Het deelvenster bewaking Servers van Operations Manager](media/oms-service-map/scom-monitoring-servers.png)

* **Afhankelijkheid Groepsweergaven machine**: geeft een lijst van alle computergroepen die vanuit Serviceoverzicht worden gesynchroniseerd. U kunt klikken op een groep om de gedistribueerde toepassing diagram weer te geven.

    ![Het diagram van de gedistribueerde toepassing Operations Manager](media/oms-service-map/scom-group-dad.png)

* **Server-weergaven voor afhankelijkheid**: geeft een lijst van alle servers die vanuit Serviceoverzicht worden gesynchroniseerd. U kunt klikken op elke server om de gedistribueerde toepassing diagram weer te geven.

    ![Het diagram van de gedistribueerde toepassing Operations Manager](media/oms-service-map/scom-dad.png)

## <a name="edit-or-delete-the-workspace"></a>Bewerken of verwijderen van de werkruimte
U kunt bewerken of verwijderen van de werkruimte geconfigureerde via de **kaart Serviceoverzicht** deelvenster (**beheer** deelvenster > **Operations Management Suite** > **Serviceoverzicht**). U kunt slechts één Operations Management Suite-werkruimte nu configureren.

![Het deelvenster met Operations Manager bewerken](media/oms-service-map/scom-edit-workspace.png)

## <a name="configure-rules-and-overrides"></a>Regels en overschrijvingen configureren
Een regel _Microsoft.SystemCenter.ServiceMapImport.Rule_, periodiek gegevens ophalen van de Serviceoverzicht is gemaakt. Als u wilt synchroniseren tijdsinstellingen wijzigen, kunt u overschrijvingen van de regel configureren (**ontwerpen** deelvenster > **regels** > **Microsoft.SystemCenter.ServiceMapImport.Rule**).

![Het eigenschappenvenster van Operations Manager-onderdrukkingen](media/oms-service-map/scom-overrides.png)

* **Ingeschakeld**: in- of uitschakelen van automatische updates. 
* **IntervalMinutes**: de tijd tussen de updates. Het interval is standaard een uur. Als u synchroniseren server maps vaker wilt, kunt u de waarde wijzigen.
* **TimeoutSeconds**: opnieuw instellen van de tijdsduur voordat de aanvraag een optreedt time-out. 
* **TimeWindowMinutes**: opnieuw instellen van het tijdvenster voor het opvragen van gegevens. Standaard is een venster 60 minuten. De toegestane maximumwaarde door Serviceoverzicht is 60 minuten.

## <a name="known-issues-and-limitations"></a>Bekende problemen en beperkingen

Het huidige ontwerp biedt de volgende problemen en beperkingen:
* U kunt alleen verbinding met een enkele Operations Management Suite-werkruimte.
* Hoewel u servers aan de Service kaart groep Servers handmatig via toevoegen kunt de **ontwerpen** deelvenster de kaarten voor deze servers niet onmiddellijk worden gesynchroniseerd.  Ze worden gesynchroniseerd vanuit Serviceoverzicht tijdens de volgende synchronisatiecyclus.
* Als u wijzigingen in de gedistribueerde toepassing diagrammen die zijn gemaakt door het management pack aanbrengt, worden deze wijzigingen op de volgende synchronisatie met Serviceoverzicht waarschijnlijk worden overschreven.

## <a name="create-a-service-principal"></a>Een service-principal maken
Zie voor de officiële Azure-documentatie over het maken van een service principal:
* [Een service-principal maken met behulp van PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Een service-principal maken met behulp van Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)
* [Een service-principal maken met behulp van de Azure-portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)

### <a name="feedback"></a>Feedback
Hebt u feedback voor ons over Serviceoverzicht of deze documentatie? Ga naar onze [User Voice pagina](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), kunt u functies aanbevelen of over bestaande suggesties stemmen.
