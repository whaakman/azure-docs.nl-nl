---
title: Serviceoverzicht integratie met System Center Operations Manager | Microsoft Docs
description: Serviceoverzicht is een oplossing in Azure die automatisch toepassingsonderdelen op Windows- en Linux-systemen detecteert en de communicatie tussen services toewijst. In dit artikel beschrijft het gebruik van Serviceoverzicht automatisch diagrammen van de gedistribueerde toepassing maken in Operations Manager.
services: monitoring
documentationcenter: ''
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: e8614a5a-9cf8-4c81-8931-896d358ad2cb
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2017
ms.author: bwren;dairwin
ms.openlocfilehash: 6fc6afa9c0ccbddcfa408556dee92618fe63c8fb
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49407109"
---
# <a name="service-map-integration-with-system-center-operations-manager"></a>Serviceoverzicht integratie met System Center Operations Manager
  > [!NOTE]
  > Deze functie is beschikbaar als openbare preview.
  > 
  
Serviceoverzicht ontdekt automatisch toepassingsonderdelen op Windows- en Linux-systemen en wijst de communicatie tussen services toe. Serviceoverzicht kunt u om de manier waarop u deze zien als onderling verbonden systemen die kritieke services verlenen voor de servers weer te geven. Servicetoewijzing toont verbindingen tussen servers, processen en poorten in alle via TCP verbonden architectuur, zonder configuratie vereist naast de installatie van een agent. Zie voor meer informatie de [Serviceoverzicht documentatie]( monitoring-service-map.md).

Met deze integratie tussen Service Map- en System Center Operations Manager, kunt u automatisch diagrammen van de gedistribueerde toepassing maken in Operations Manager die zijn gebaseerd op de dynamische afhankelijkheid toewijzingen in Serviceoverzicht.

## <a name="prerequisites"></a>Vereisten
* Een beheergroep van Operations Manager (2012 R2 of hoger) die wordt beheerd door een set servers.
* Een Log Analytics-werkruimte met de oplossing Serviceoverzicht is ingeschakeld.
* Een set servers (ten minste één) die worden beheerd door Operations Manager en verzenden van gegevens naar de Service Map. Windows en Linux-servers worden ondersteund.
* Een service-principal die toegang hebben tot de Azure-abonnement dat is gekoppeld aan de Log Analytics-werkruimte. Ga voor meer informatie naar [maken van een service-principal](#create-a-service-principal).

## <a name="install-the-service-map-management-pack"></a>Installeer het managementpack voor Serviceoverzicht
U kunt de integratie tussen Operations Manager en Service Map inschakelen door het importeren van de Microsoft.SystemCenter.ServiceMap management pack-bundel (Microsoft.SystemCenter.ServiceMap.mpb). U kunt het management pack-bundel van downloaden de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=55763). De bundel bevat de volgende management packs:
* Microsoft Service Map-toepassing-weergaven
* Microsoft System Center Service Map interne
* Microsoft System Center Service kaart onderdrukkingen
* Microsoft System Center-Serviceoverzicht

## <a name="configure-the-service-map-integration"></a>De Serviceoverzicht-integratie configureren
Nadat u het Serviceoverzicht management pack, een nieuw knooppunt **Serviceoverzicht**, wordt weergegeven onder **Operations Management Suite** in de **beheer** deelvenster. 

>[!NOTE]
>[Operations Management Suite is een verzameling van services](https://github.com/MicrosoftDocs/azure-docs-pr/pull/azure-monitor/azure-monitor-rebrand.md#retirement-of-operations-management-suite-brand) die opgenomen Log Analytics, die nu deel uitmaakt van [Azure Monitor](https://github.com/MicrosoftDocs/azure-docs-pr/pull/azure-monitor/overview.md).

Serviceoverzicht om integratie te configureren, het volgende doen:

1. De configuratiewizard openen in de **Service kaart overzicht** deelvenster, klikt u op **werkruimte toevoegen**.  

    ![Overzicht van service-kaart deelvenster](media/monitoring-service-map/scom-configuration.png)

2. In de **verbindingsconfiguratie** venster, geef de naam van tenant of -ID, toepassings-ID (ook wel bekend als de gebruikersnaam of clientID) en het wachtwoord van de service-principal, en klik vervolgens op **volgende**. Ga voor meer informatie naar [maken van een service-principal](#creating-a-service-principal).

    ![Het venster verbindingsconfiguratie](media/monitoring-service-map/scom-config-spn.png)

3. In de **Abonnementselectie** venster, selecteert u de Azure-abonnement, de Azure-resourcegroep (het account met de Log Analytics-werkruimte) en de Log Analytics-werkruimte en klik vervolgens op **volgende**.

    ![De werkruimte van Operations Manager-configuratie](media/monitoring-service-map/scom-config-workspace.png)

4. In de **Machine groepsselectie** venster, kiest u welke kaart computergroepen u wilt synchroniseren met Operations Manager. Klik op **computergroepen toevoegen/verwijderen**, kiest u groepen uit de lijst met **beschikbare computergroepen**, en klikt u op **toevoegen**.  Wanneer u klaar bent met groepen te selecteren, klikt u op **Ok** om te voltooien.
    
    ![De Operations Manager-configuratie computergroepen](media/monitoring-service-map/scom-config-machine-groups.png)
    
5. In de **serverselectie** venster, u de groep Service Map-Servers configureren met de servers die u wilt synchroniseren tussen Operations Manager en Service Map. Klik op **Servers toevoegen/verwijderen**.   
    
    Voor de integratie met het bouwen van een diagram van gedistribueerde toepassing voor een server, moet de server:

    * Beheerd door Operations Manager
    * Beheerd door Service Map
    * Vermeld in de groep Servers van Service-kaart

    ![De groep Operations Manager-configuratie](media/monitoring-service-map/scom-config-group.png)

6. Optioneel: Selecteer de resourcegroep van de beheerserver om te communiceren met Log Analytics, en klik vervolgens op **werkruimte toevoegen**.

    ![De Operations Manager-configuratie-resourcegroep](media/monitoring-service-map/scom-config-pool.png)

    Het duurt even om te configureren en registreren van de Log Analytics-werkruimte. Nadat deze is geconfigureerd, wordt de eerste synchronisatie van Serviceoverzicht in Operations Manager gestart.

    ![De Operations Manager-configuratie-resourcegroep](media/monitoring-service-map/scom-config-success.png)


## <a name="monitor-service-map"></a>Monitor Serviceoverzicht
Nadat de verbinding van de Log Analytics-werkruimte is gemaakt, een nieuwe map Service Map, wordt weergegeven in de **bewaking** deelvenster van de Operations Manager-console.

![Het deelvenster controle van Operations Manager](media/monitoring-service-map/scom-monitoring.png)

De Serviceoverzicht-map bevat vier knooppunten:
* **Actieve waarschuwingen**: geeft een lijst van alle actieve waarschuwingen over de communicatie tussen Operations Manager en Service Map.  Houd er rekening mee dat deze waarschuwingen niet zijn van Log Analytics waarschuwingen gesynchroniseerd wordt met Operations Manager. 

* **Servers**: geeft een lijst van de bewaakte servers die zijn geconfigureerd om te synchroniseren van Serviceoverzicht.

    ![Het deelvenster Servers van Operations Manager-bewaking](media/monitoring-service-map/scom-monitoring-servers.png)

* **Groepsweergaven van de afhankelijkheid van de computer**: geeft een lijst van alle computergroepen die vanuit de Service Map zijn gesynchroniseerd. U kunt klikken op een groep om een diagram van de gedistribueerde toepassing weer te geven.

    ![Een diagram van de Operations Manager gedistribueerde toepassing](media/monitoring-service-map/scom-group-dad.png)

* **Server afhankelijkheid weergaven**: geeft een lijst van alle servers die zijn gesynchroniseerd van Serviceoverzicht. U kunt klikken op elke server om een diagram van de gedistribueerde toepassing weer te geven.

    ![Een diagram van de Operations Manager gedistribueerde toepassing](media/monitoring-service-map/scom-dad.png)

## <a name="edit-or-delete-the-workspace"></a>Bewerken of verwijderen van de werkruimte
U kunt bewerken of verwijderen van de geconfigureerde werkruimte via de **Service kaart overzicht** deelvenster (**beheer** deelvenster > **Operations Management Suite**  >  **Service kaart**).

>[!NOTE]
>[Operations Management Suite is een verzameling van services](https://github.com/MicrosoftDocs/azure-docs-pr/pull/azure-monitor/azure-monitor-rebrand.md#retirement-of-operations-management-suite-brand) die opgenomen Log Analytics, die nu deel uitmaakt van [Azure Monitor](https://github.com/MicrosoftDocs/azure-docs-pr/pull/azure-monitor/overview.md).

Nu kunt u slechts één Log Analytics-werkruimte configureren.

![De werkruimte van Operations Manager-bewerken](media/monitoring-service-map/scom-edit-workspace.png)

## <a name="configure-rules-and-overrides"></a>Regels en overschrijvingen configureren
Een regel, _Microsoft.SystemCenter.ServiceMapImport.Rule_, om op te halen regelmatig gegevens van Service Map wordt gemaakt. Als u wilt synchroniseren tijdsinstellingen wijzigen, kunt u onderdrukkingen van de regel (**ontwerpen** deelvenster > **regels** > **Microsoft.SystemCenter.ServiceMapImport.Rule**) .

![Het eigenschappenvenster van Operations Manager-onderdrukkingen](media/monitoring-service-map/scom-overrides.png)

* **Ingeschakeld**: in- of uitschakelen van automatische updates. 
* **IntervalMinutes**: opnieuw instellen van de tijd tussen de updates. Het standaardinterval is één uur. Als u synchroniseren server maps regelmatig wilt, kunt u de waarde wijzigen.
* **TimeoutSeconds**: de hoeveelheid tijd voordat de time-out van de aanvraag opnieuw. 
* **TimeWindowMinutes**: opnieuw instellen van het tijdvenster voor het opvragen van gegevens. Standaard is een venster 60 minuten. De toegestane maximumwaarde door Serviceoverzicht is 60 minuten.

## <a name="known-issues-and-limitations"></a>Bekende problemen en beperkingen

Het huidige ontwerp biedt de volgende problemen en beperkingen:
* U kunt alleen verbinding maken met één Log Analytics-werkruimte.
* Hoewel u servers aan de Service Map groep Servers handmatig via toevoegen kunt de **ontwerpen** in het deelvenster de toewijzingen voor deze servers niet meteen worden gesynchroniseerd.  Ze worden gesynchroniseerd vanuit de Service Map tijdens de volgende synchronisatiecyclus.
* Als u wijzigingen aanbrengt aan de gedistribueerde toepassing diagrammen die zijn gemaakt door het managementpack, worden deze wijzigingen op de volgende synchronisatie met Serviceoverzicht waarschijnlijk worden overschreven.

## <a name="create-a-service-principal"></a>Een service-principal maken
Zie voor de officiële Azure-documentatie over het maken van een service principal en:
* [Een service-principal maken met behulp van PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Een service-principal maken met behulp van Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)
* [Een service-principal maken met behulp van de Azure-portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)

### <a name="feedback"></a>Feedback
Hebt u feedback voor ons over Serviceoverzicht of deze documentatie? Ga naar onze [Uservoice-pagina](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), waar u kunt functies voorstellen en op bestaande suggesties stemmen.
