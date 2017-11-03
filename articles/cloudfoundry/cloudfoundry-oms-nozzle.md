---
title: Azure Log Analytics pijp implementeren voor het bewaken van de Cloud Foundry | Microsoft Docs
description: Stapsgewijze instructies over het implementeren van de Cloud Foundry loggregator pijp voor Azure-logboekanalyse. Gebruik de pijp voor het bewaken van de Cloud Foundry system status en prestaties van metrische gegevens.
services: virtual-machines-linux
documentationcenter: 
author: ningk
manager: timlt
editor: 
tags: Cloud-Foundry
ms.assetid: 00c76c49-3738-494b-b70d-344d8efc0853
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/22/2017
ms.author: ningk
ms.openlocfilehash: 0d13d39d2921c51c537534a5b000564a9df91880
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-azure-log-analytics-nozzle-for-cloud-foundry-system-monitoring"></a>Azure Log Analytics pijp implementeren voor het bewaken van de Cloud Foundry-systeem

[Azure Log Analytics](https://azure.microsoft.com/services/log-analytics/) is een service in Microsoft [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/) (OMS). Dit helpt u bij het verzamelen en analyseren van gegevens die is gegenereerd op basis van uw cloud en on-premises omgevingen.

De Log Analytics pijp (de pijp) is een onderdeel Cloud Foundry (CF), die metrische gegevens van doorstuurt de [Cloud Foundry loggregator](https://docs.cloudfoundry.org/loggregator/architecture.html) firehose met logboekanalyse. Met de pijp kunt u verzamelen, weergeven en analyseren van uw CF system health en prestatiemaatstaven, met meerdere implementaties.

In dit document, informatie over het implementeren van de pijp aan uw omgeving CF en vervolgens toegang tot de gegevens uit de logboekanalyse OMS-console.

## <a name="prerequisites"></a>Vereisten

De volgende stappen zijn de vereisten voor de implementatie van de pijp.

### <a name="1-deploy-a-cf-or-pivotal-cloud-foundry-environment-in-azure"></a>1. Een CF of belangrijke Cloud Foundry-omgeving in Azure implementeren

U kunt de pijp gebruiken met de implementatie van een open-source CF of een belangrijke Cloud Foundry (PCF)-implementatie.

* [Foundry op Azure Cloud implementeren](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md)

* [Belangrijke Cloud Foundry in Azure implementeren](https://docs.pivotal.io/pivotalcf/1-11/customizing/azure.html)

### <a name="2-install-the-cf-command-line-tools-for-deploying-the-nozzle"></a>2. De CF opdrachtregelprogramma's voor het implementeren van de pijp installeren

De pijp uitgevoerd als een toepassing in CF-omgeving. U moet de CLI CF om de toepassing te implementeren.

De pijp moet ook de toegangsmachtigingen voor de firehose loggregator en de Cloud-Controller. Als u wilt maken en configureren van de gebruiker, moet u de gebruikersaccount en verificatie UAA ()-service.

* [Cloud Foundry CLI installeren](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html)

* [Cloud Foundry UAA opdrachtregelprogramma client installeren](https://github.com/cloudfoundry/cf-uaac/blob/master/README.md)

Voordat u de client voor de opdrachtregel UAA instelt, zorg ervoor dat de Rubygems is geïnstalleerd.

### <a name="3-create-an-oms-workspace-in-azure"></a>3. Een OMS-werkruimte maken in Azure

U kunt de OMS-werkruimte maken handmatig of via een sjabloon. De vooraf geconfigureerde OMS-weergaven en waarschuwingen nadat u de implementatie van de pijp worden geladen.

De werkruimte handmatig maken:

1. In de Azure portal zoeken in de lijst van services in Azure Marketplace en selecteer vervolgens logboekanalyse.
2. Selecteer **maken**, en selecteer vervolgens de opties voor de volgende items:

   * **OMS-werkruimte**: Typ een naam voor uw werkruimte.
   * **Abonnement**: als u meerdere abonnementen hebt, kiezen die hetzelfde is als uw implementatie CF.
   * **Resourcegroep**: U kunt een nieuwe resourcegroep maken of gebruiken hetzelfde met uw CF-implementatie.
   * **Locatie**: Geef de locatie.
   * **Prijscategorie**: Selecteer **OK** te voltooien.

Zie voor meer informatie [aan de slag met logboekanalyse](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started).

U kunt ook de OMS-werkruimte via de OMS-sjabloon maken. Met deze methode, de sjabloon de vooraf geconfigureerde OMS-weergaven en waarschuwingen automatisch geladen. Zie voor meer informatie de [Azure OMS Log Analytics-oplossing voor Cloud Foundry](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-cloudfoundry-solution).

## <a name="deploy-the-nozzle"></a>De pijp implementeren

Er zijn een aantal verschillende manieren voor het implementeren van de pijp: als een tegel PCF of als een toepassing CF.

### <a name="deploy-the-nozzle-as-a-pcf-ops-manager-tile"></a>De pijp implementeren als een tegel PCF Ops Manager

Als u PCF hebt geïmplementeerd met behulp van Ops Manager, volgt u de stappen voor het [installeren en configureren van de pijp voor PCF](http://docs.pivotal.io/partners/azure-log-analytics-nozzle/installing.html). De pijp wordt geïnstalleerd als een tegel met Operations Manager.

### <a name="deploy-the-nozzle-as-a-cf-application"></a>De pijp implementeren als een toepassing CF

Als u geen PCF Ops Manager gebruikt, moet u de pijp implementeren als een toepassing. De volgende secties beschrijven dit proces.

#### <a name="sign-in-to-your-cf-deployment-as-an-admin-through-cf-cli"></a>Aanmelden bij uw implementatie CF als een beheerder via CF CLI

Voer de volgende opdracht uit:
```
cf login -a https://api.${SYSTEM_DOMAIN} -u ${CF_USER} --skip-ssl-validation
```

'SYSTEM_DOMAIN' is de domeinnaam van uw CF. U kunt deze ophalen door te zoeken in de 'SYSTEM_DOMAIN' in uw CF deployment manifest-bestand. 

'CF_User' is de naam van de beheerder CF. U kunt de naam en het wachtwoord ophalen door de sectie 'scim' zoeken, zoekt u de naam en de 'cf_admin_password' in uw CF deployment manifest-bestand.

#### <a name="create-a-cf-user-and-grant-required-privileges"></a>Maak een CF-gebruiker en de vereiste machtigingen te verlenen

Voer de volgende opdrachten uit:
```
uaac target https://uaa.${SYSTEM_DOMAIN} --skip-ssl-validation
uaac token client get admin
cf create-user ${FIREHOSE_USER} ${FIREHOSE_USER_PASSWORD}
uaac member add cloud_controller.admin ${FIREHOSE_USER}
uaac member add doppler.firehose ${FIREHOSE_USER}
```

'SYSTEM_DOMAIN' is de domeinnaam van uw CF. U kunt deze ophalen door te zoeken in de 'SYSTEM_DOMAIN' in uw CF deployment manifest-bestand.

#### <a name="download-the-latest-log-analytics-nozzle-release"></a>Download de nieuwste versie van de pijp van Log Analytics

Voer de volgende opdracht uit:
```
git clone https://github.com/Azure/oms-log-analytics-firehose-nozzle.git
cd oms-log-analytics-firehose-nozzle
```

#### <a name="set-environment-variables"></a>De omgevingsvariabelen instellen

U kunt nu omgevingsvariabelen instellen in het bestand manifest.yml in uw huidige map. Hieronder ziet u het app-manifest voor de pijp. Vervang de waarden door uw specifieke gegevens van de OMS-werkruimte.

```
OMS_WORKSPACE             : OMS workspace ID: open OMS portal from your OMS workspace, select Settings, and select connected sources.
OMS_KEY                   : OMS key: open OMS portal from your OMS workspace, select Settings, and select connected sources.
OMS_POST_TIMEOUT          : HTTP post timeout for sending events to OMS Log Analytics. The default is 10 seconds.
OMS_BATCH_TIME            : Interval for posting a batch to OMS Log Analytics. The default is 10 seconds.
OMS_MAX_MSG_NUM_PER_BATCH : The maximum number of messages in a batch to OMS Log Analytics. The default is 1000.
API_ADDR                  : The API URL of the CF environment. For more information, see the preceding section, "Sign in to your CF deployment as an admin through CF CLI."
DOPPLER_ADDR              : Loggregator's traffic controller URL. For more information, see the preceding section, "Sign in to your CF deployment as an admin through CF CLI."
FIREHOSE_USER             : CF user you created in the preceding section, "Create a CF user and grant required privileges." This user has firehose and Cloud Controller admin access.
FIREHOSE_USER_PASSWORD    : Password of the CF user above.
EVENT_FILTER              : Event types to be filtered out. The format is a comma-separated list. Valid event types are METRIC, LOG, and HTTP.
SKIP_SSL_VALIDATION       : If true, allows insecure connections to the UAA and the traffic controller.
CF_ENVIRONMENT            : Enter any string value for identifying logs and metrics from different CF environments.
IDLE_TIMEOUT              : The Keep Alive duration for the firehose consumer. The default is 60 seconds.
LOG_LEVEL                 : The logging level of the Nozzle. Valid levels are DEBUG, INFO, and ERROR.
LOG_EVENT_COUNT           : If true, the total count of events that the Nozzle has received and sent are logged to OMS Log Analytics as CounterEvents.
LOG_EVENT_COUNT_INTERVAL  : The time interval of the logging event count to OMS Log Analytics. The default is 60 seconds.
```

### <a name="push-the-application-from-your-development-computer"></a>De toepassing vanaf de ontwikkelcomputer push

Zorg ervoor dat u in de oms-log-analytics-firehose-pijp-map. Voer de volgende opdracht uit:
```
cf push
```

## <a name="validate-the-nozzle-installation"></a>De installatie van de pijp valideren

### <a name="from-apps-manager-for-pcf"></a>Vanuit Beheer van Apps (voor PCF)

1. Aanmelden bij de Operations Manager en controleer of dat de tegel wordt weergegeven op het dashboard van de installatie.
2. Aanmelden bij Apps Manager, zorg ervoor dat de ruimte die u hebt gemaakt voor de pijp wordt weergegeven op het gebruiksrapport en Bevestig dat de status Normaal is.

### <a name="from-your-development-computer"></a>Vanaf de ontwikkelcomputer

Typ in het venster CF CLI:
```
cf apps
```
Zorg ervoor dat de pijp OMS-toepassing wordt uitgevoerd.

## <a name="view-the-data-in-the-oms-portal"></a>De gegevens weergeven in de OMS-portal

### <a name="1-import-the-oms-view"></a>1. Importeer de OMS-weergave

Vanuit de OMS-portal bladert u naar **ontwerper** > **importeren** > **Bladeren**, en selecteer een van de omsview-bestanden. Selecteer bijvoorbeeld *Cloud Foundry.omsview*, en de weergave op te slaan. Nu u een tegel wordt weergegeven op de OMS **overzicht** pagina. Selecteer het gevisualiseerde metrische gegevens te verzamelen.

U kunt deze weergaven aanpassen of maken van nieuwe weergaven via **ontwerper**.

De *'Cloud Foundry.omsview'* is een preview-versie van de sjabloon Cloud Foundry OMS weergeven. Dit is een volledig geconfigureerde standaardsjabloon. Als u suggesties of feedback over de sjabloon hebt, verzenden naar de [uitgeven sectie](https://github.com/Azure/oms-log-analytics-firehose-nozzle/issues).

### <a name="2-create-alert-rules"></a>2. Waarschuwingsregels maken

U kunt [maken van de waarschuwingen](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts), en pas de query's en drempelwaarden zo nodig. De volgende worden waarschuwingen aanbevolen:

| Zoekopdracht                                                                  | Op basis van de waarschuwing wordt gegenereerd | Beschrijving                                                                       |
| ----------------------------------------------------------------------------- | ----------------------- | --------------------------------------------------------------------------------- |
| Type = CF_ValueMetric_CL Origin_s = bbs Name_s = 'Domain.cf-apps'                   | Aantal resultaten < 1   | **BBS. Domain.cf apps** wordt aangegeven of het domein cf-apps up-to-date. Dit betekent dat CF App aanvragen van Cloud-Controller naar bbs zijn gesynchroniseerd. LRPsDesired (AIs Diego gewenst) voor uitvoering. Er zijn geen gegevens ontvangen betekent dat CF apps domein is niet bijgewerkt in de opgegeven periode. |
| Type = CF_ValueMetric_CL Origin_s = rep Name_s UnhealthyCell Value_d = > 1            | Aantal resultaten > 0   | Voor Diego cellen 0 betekent in orde en 1 betekent dat niet in orde. Stel de waarschuwing als meerdere slecht Diego cellen worden gedetecteerd in de opgegeven periode. |
| Type CF_ValueMetric_CL Origin_s = 'bosh hm doorstuurserver' Name_s="system.healthy =" Value_d = 0 | Aantal resultaten > 0 | 1 betekent dat het systeem in orde is en 0 betekent dat het systeem is niet in orde. |
| Type = CF_ValueMetric_CL Origin_s = route_emitter Name_s ConsulDownMode Value_d = > 0 | Aantal resultaten > 0   | Ge verzendt periodiek de status. 0 betekent dat het systeem is in orde en 1 betekent dat de zender route detecteert dat ge niet actief is. |
| Type = CF_CounterEvent_CL Origin_s Delta_d DopplerServer (Name_s="TruncatingBuffer.DroppedMessages' of Name_s="doppler.shedEnvelopes') = > 0 | Aantal resultaten > 0 | De delta aantal berichten opzettelijk verwijderd door Doppler vanwege een zware belasting op het vorige. |
| Type = CF_LogMessage_CL SourceType_s LGR MessageType_s = ERR =                      | Aantal resultaten > 0   | Loggregator verzendt **LGR** om aan te duiden op problemen met het registratieproces. Een voorbeeld van een dergelijk probleem is wanneer de uitvoer van het bericht te hoog is. |
| Type CF_ValueMetric_CL Name_s = slowConsumerAlert =                               | Aantal resultaten > 0   | Wanneer de pijp wordt een waarschuwing trage consumer van loggregator ontvangt, stuurt de **slowConsumerAlert** ValueMetric met OMS. |
| Type = CF_CounterEvent_CL Job_s = pijp Name_s eventsLost Delta_d = > 0              | Aantal resultaten > 0   | Als de delta-aantal gebeurtenissen dat verloren een drempel bereikt, betekent dit dat de pijp mogelijk een probleem dat wordt uitgevoerd. |

## <a name="scale"></a>Schalen

U kunt de pijp en de loggregator schalen.

### <a name="scale-the-nozzle"></a>Schaal de pijp

U moet beginnen met ten minste twee exemplaren van de pijp. De firehose verdeelt de werkbelasting over alle exemplaren van de pijp.
Om te zorgen dat de pijp kunt blijven van het gegevensverkeer van de firehose, instellen van de **slowConsumerAlert** waarschuwing (vermeld in de vorige sectie, 'Waarschuwingsregels maken'). Nadat u hebt gewaarschuwd, voert u de [richtlijnen voor het trage pijp](https://docs.pivotal.io/pivotalcf/1-11/loggregator/log-ops-guide.html#slow-noz) om te bepalen of schalen nodig is.
Als u wilt schalen van de pijp, gebruik Apps Manager of de CLI CF te verhogen van de getallen exemplaar of de bronnen geheugen of schijfruimte voor de pijp.

### <a name="scale-the-loggregator"></a>Schaal de loggregator

Loggregator verzendt een **LGR** logboekbericht om aan te duiden op problemen met het registratieproces. De waarschuwing om te bepalen of de loggregator moet worden uitgebreid, kunt u bewaken.
Als u wilt de loggregator opschalen, vergroten Doppler buffer of extra Doppler server-exemplaren in het manifest CF toevoegen. Zie voor meer informatie [de richtlijnen voor het schalen van de loggregator](https://docs.cloudfoundry.org/running/managing-cf/logging-config.html#scaling).

## <a name="update"></a>Update

Naar de pijp bijwerken met een nieuwere versie, downloaden van de nieuwe versie van de pijp en volg de stappen in de vorige sectie van de 'De pijp implementeren' push van de toepassing opnieuw.

### <a name="remove-the-nozzle-from-ops-manager"></a>Verwijderen van de pijp van Ops Manager

1. Aanmelden bij de Operations Manager.
2. Zoek de **Microsoft Azure Log Analytics pijp voor PCF** tegel.
3. Selecteer het pictogram garbagecollection en Bevestig de verwijdering.

### <a name="remove-the-nozzle-from-your-development-computer"></a>De pijp verwijderen uit uw ontwikkelcomputer

Typ in het venster CF CLI:
```
cf delete <App Name> -r
```

Als u de pijp verwijdert, wordt de gegevens in de OMS-portal niet automatisch verwijderd. Het is verlopen op basis van de instelling van de retentie OMS-logboekanalyse.

## <a name="support-and-feedback"></a>Ondersteuning en feedback

Azure Log Analytics pijp is brongegevens geopend. Uw vragen en feedback verzenden de [GitHub sectie](https://github.com/Azure/oms-log-analytics-firehose-nozzle/issues). Om te openen in een aanvraag voor de ondersteuning van Azure, kiest u 'Virtuele Machine met Cloud Foundry' als de servicecategorie. 

## <a name="next-step"></a>Volgende stap

Naast de Cloud Foundry metrische gegevens die in de pijp worden behandeld en kunt u de OMS-agent inzicht in de operationele gegevens van de VM-niveau (zoals Syslog, prestaties, waarschuwingen, inventarisatie). De OMS-agent wordt geïnstalleerd als een invoegtoepassing Bosh voor uw CF virtuele machines.

Zie voor meer informatie [implementeren OMS-agent voor uw implementatie Cloud Foundry](https://github.com/Azure/oms-agent-for-linux-boshrelease).
