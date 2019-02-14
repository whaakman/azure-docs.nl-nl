---
title: Azure Log Analytics-pijp voor Cloud Foundry bewaking implementeren | Microsoft Docs
description: Stapsgewijze instructies over het implementeren van de Cloud Foundry-loggregator Nozzle voor Azure Log Analytics. Gebruik de Nozzle voor het bewaken van de Cloud Foundry system health en prestatiemetrieken.
services: virtual-machines-linux
author: ningk
manager: jeconnoc
tags: Cloud-Foundry
ms.assetid: 00c76c49-3738-494b-b70d-344d8efc0853
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/22/2017
ms.author: ningk
ms.openlocfilehash: 6f23b103f1715d567792e162d62d69f13fc08968
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56243872"
---
# <a name="deploy-azure-log-analytics-nozzle-for-cloud-foundry-system-monitoring"></a>Azure Log Analytics-pijp voor Cloud Foundry systeemcontrole implementeren

[Azure Log Analytics](https://azure.microsoft.com/services/log-analytics/) is een service in Azure. Hiermee kunt u het verzamelen en analyseren van gegevens die is gegenereerd op basis van uw cloud en on-premises omgevingen.

De Log Analytics-pijp (Nozzle) is een onderdeel van de Cloud Foundry (CF), die doorstuurt van metrische gegevens van de [Cloud Foundry loggregator](https://docs.cloudfoundry.org/loggregator/architecture.html) firehose naar Log Analytics. Met de Nozzle, kunt u verzamelen, weergeven en analyseren van uw CF system status en prestaties van metrische gegevens, meerdere implementaties.

In dit document leert u hoe u de Nozzle implementeren voor uw omgeving CF en vervolgens toegang tot de gegevens vanuit de Log Analytics-console.

## <a name="prerequisites"></a>Vereisten

De volgende stappen zijn vereisten voor de implementatie van de Nozzle.

### <a name="1-deploy-a-cf-or-pivotal-cloud-foundry-environment-in-azure"></a>1. Een omgeving CF of Pivotal Cloud Foundry in Azure implementeren

U kunt de Nozzle gebruiken met de implementatie van een open-source-CF of een implementatie van Pivotal Cloud Foundry (PCF).

* [Cloud Foundry op Azure implementeren](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md)

* [Pivotal Cloud Foundry op Azure implementeren](https://docs.pivotal.io/pivotalcf/1-11/customizing/azure.html)

### <a name="2-install-the-cf-command-line-tools-for-deploying-the-nozzle"></a>2. De CF opdrachtregelprogramma's voor het implementeren van de Nozzle installeren

De Nozzle wordt uitgevoerd als een toepassing in CF-omgeving. U moet CF CLI om de toepassing te implementeren.

De Nozzle ook toegang heeft toestemming nodig voor de firehose loggregator en de Cloud-Controller. Als u wilt maken en configureren van de gebruiker, moet u de gebruikersaccount en -verificatie (UAA)-service.

* [Cloud Foundry CLI installeren](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html)

* [Cloud Foundry UAA opdrachtregel-client installeren](https://github.com/cloudfoundry/cf-uaac/blob/master/README.md)

Voordat u de opdrachtregelclient UAA instelt, zorg dat Rubygems is geïnstalleerd.

### <a name="3-create-a-log-analytics-workspace-in-azure"></a>3. Een Log Analytics-werkruimte maken in Azure

U kunt de Log Analytics-werkruimte maken handmatig of met behulp van een sjabloon. De sjabloon implementeert een installatie van de vooraf geconfigureerde KPI-weergaven en waarschuwingen voor de Log Analytics-console. 

#### <a name="to-create-the-workspace-manually"></a>De werkruimte handmatig maken:

1. In de Azure-portal zoeken in de lijst met services in de Azure Marketplace en selecteer vervolgens Log Analytics.
2. Selecteer **maken**, en selecteer vervolgens de opties voor de volgende items:

   * **Log Analytics-werkruimte**: Typ een naam voor uw werkruimte.
   * **Abonnement**: Als u meerdere abonnementen hebt, kiest u de optie die gelijk is aan uw CF-implementatie.
   * **Resourcegroep**: U kunt een nieuwe resourcegroep maken of hetzelfde account gebruiken met uw CF-implementatie.
   * **Locatie**: Geef de locatie.
   * **Prijscategorie**: Selecteer **OK** om te voltooien.

Zie voor meer informatie, [aan de slag met Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started).

#### <a name="to-create-the-log-analytics-workspace-through-the-monitoring-template-from-azure-market-place"></a>De Log Analytics-werkruimte via de monitoring-sjabloon maken vanuit Azure Marketplace:

1. Open Azure portal.
2. Klik op het teken '+' of 'Een resource maken' in de linkerbovenhoek.
3. 'Cloud Foundry' typt in het zoekvenster, selecteert u 'Cloud Foundry bewaking oplossing'.
4. De Cloud Foundry oplossing sjabloon front pagina is geladen, klikt u op 'Maken' naar de blade sjabloon geopend.
5. Voer de vereiste parameters:
    * **Abonnement**: Selecteer een Azure-abonnement voor de Log Analytics-werkruimte, meestal hetzelfde met Cloud Foundry-implementatie.
    * **Resourcegroep**: Selecteer een bestaande resourcegroep of maak een nieuw wachtwoord voor de Log Analytics-werkruimte.
    * **Locatie van de resourcegroep**: Selecteer de locatie van de resourcegroep.
    * **OMS_Workspace_Name**: Voer de naam van een werkruimte, als de werkruimte niet bestaat, de sjabloon wordt een nieuwe maken.
    * **OMS_Workspace_Region**: Selecteer de locatie voor de werkruimte.
    * **OMS_Workspace_Pricing_Tier**: Selecteer de SKU van de Log Analytics-werkruimte. Zie de [prijsinformatie](https://azure.microsoft.com/pricing/details/log-analytics/) ter referentie.
    * **Juridische voorwaarden**: Klik op de juridische voorwaarden en klik vervolgens op 'Maken' voor het accepteren van de juridische term.
- Nadat alle parameters zijn opgegeven, klikt u op 'Maken' om de sjabloon te implementeren. Wanneer de implementatie is voltooid, wordt de status op het tabblad melding weergegeven.


## <a name="deploy-the-nozzle"></a>De Nozzle implementeren

Er zijn een aantal verschillende manieren voor het Nozzle implementeren: als een tegel PCF of als een toepassing CF.

### <a name="deploy-the-nozzle-as-a-pcf-ops-manager-tile"></a>De Nozzle implementeren als een tegel PCF Operations Manager

Volg de stappen voor het [installeren en configureren van de Azure Log Analytics-pijp voor PCF](http://docs.pivotal.io/partners/azure-log-analytics-nozzle/installing.html). Dit is de vereenvoudigde benadering, de PCF operations manager-tegel wordt automatisch configureren en de nozzle pushen. 

### <a name="deploy-the-nozzle-manually-as-a-cf-application"></a>Handmatig de Nozzle implementeren als een toepassing CF

Als u niet PCF Operations Manager gebruikt, moet u de Nozzle implementeren als een toepassing. De volgende secties beschrijven deze procedure.

#### <a name="sign-in-to-your-cf-deployment-as-an-admin-through-cf-cli"></a>Aanmelden bij uw implementatie CF als een beheerder via CF CLI

Voer de volgende opdracht uit:
```
cf login -a https://api.${SYSTEM_DOMAIN} -u ${CF_USER} --skip-ssl-validation
```

"SYSTEM_DOMAIN" is de naam van uw CF-domein. U kunt deze door te zoeken naar de "SYSTEM_DOMAIN" ophalen in het manifestbestand van uw CF implementatie. 

"CF_User" is de naam van de beheerder CF. U kunt de naam en het wachtwoord ophalen door te zoeken naar de sectie 'scim', zoekt u de naam en de 'cf_admin_password' in het manifestbestand van uw CF implementatie.

#### <a name="create-a-cf-user-and-grant-required-privileges"></a>Maak een CF-gebruiker en de vereiste machtigingen verlenen

Voer de volgende opdrachten uit:
```
uaac target https://uaa.${SYSTEM_DOMAIN} --skip-ssl-validation
uaac token client get admin
cf create-user ${FIREHOSE_USER} ${FIREHOSE_USER_PASSWORD}
uaac member add cloud_controller.admin ${FIREHOSE_USER}
uaac member add doppler.firehose ${FIREHOSE_USER}
```

"SYSTEM_DOMAIN" is de naam van uw CF-domein. U kunt deze door te zoeken naar de "SYSTEM_DOMAIN" ophalen in het manifestbestand van uw CF implementatie.

#### <a name="download-the-latest-log-analytics-nozzle-release"></a>Download de nieuwste release van Log Analytics-pijp

Voer de volgende opdracht uit:
```
git clone https://github.com/Azure/oms-log-analytics-firehose-nozzle.git
cd oms-log-analytics-firehose-nozzle
```

#### <a name="set-environment-variables"></a>Omgevingsvariabelen instellen

U kunt nu omgevingsvariabelen instellen in het bestand manifest.yml in uw huidige map. Hieronder ziet u het app-manifest voor de Nozzle. Waarden vervangen door uw specifieke gegevens van de Log Analytics-werkruimte.

```
OMS_WORKSPACE             : Log Analytics workspace ID: Open your Log Analytics workspace in the Azure portal, select **Advanced settings**, select **Connected Sources**, and select **Windows Servers**.
OMS_KEY                   : OMS key: Open your Log Analytics workspace in the Azure portal, select **Advanced settings**, select **Connected Sources**, and select **Windows Servers**.
OMS_POST_TIMEOUT          : HTTP post timeout for sending events to Log Analytics. The default is 10 seconds.
OMS_BATCH_TIME            : Interval for posting a batch to Log Analytics. The default is 10 seconds.
OMS_MAX_MSG_NUM_PER_BATCH : The maximum number of messages in a batch to Log Analytics. The default is 1000.
API_ADDR                  : The API URL of the CF environment. For more information, see the preceding section, "Sign in to your CF deployment as an admin through CF CLI."
DOPPLER_ADDR              : Loggregator's traffic controller URL. For more information, see the preceding section, "Sign in to your CF deployment as an admin through CF CLI."
FIREHOSE_USER             : CF user you created in the preceding section, "Create a CF user and grant required privileges." This user has firehose and Cloud Controller admin access.
FIREHOSE_USER_PASSWORD    : Password of the CF user above.
EVENT_FILTER              : Event types to be filtered out. The format is a comma-separated list. Valid event types are METRIC, LOG, and HTTP.
SKIP_SSL_VALIDATION       : If true, allows insecure connections to the UAA and the traffic controller.
CF_ENVIRONMENT            : Enter any string value for identifying logs and metrics from different CF environments.
IDLE_TIMEOUT              : The Keep Alive duration for the firehose consumer. The default is 60 seconds.
LOG_LEVEL                 : The logging level of the Nozzle. Valid levels are DEBUG, INFO, and ERROR.
LOG_EVENT_COUNT           : If true, the total count of events that the Nozzle has received and sent are logged to Log Analytics as CounterEvents.
LOG_EVENT_COUNT_INTERVAL  : The time interval of the logging event count to Log Analytics. The default is 60 seconds.
```

### <a name="push-the-application-from-your-development-computer"></a>De toepassingen op uw ontwikkelcomputer pushen

Zorg ervoor dat u in de oms-log-analytics-firehose-nozzle-map. Voer de volgende opdracht uit:
```
cf push
```

## <a name="validate-the-nozzle-installation"></a>De installatie Nozzle valideren

### <a name="from-apps-manager-for-pcf"></a>Vanuit Beheer van Apps (voor PCF)

1. Meld u aan Operations Manager en controleer of dat de tegel wordt weergegeven op het dashboard van de installatie.
2. Aanmelden bij Apps Manager, zorg ervoor dat de ruimte die u hebt gemaakt voor de Nozzle wordt weergegeven op het gebruiksrapport en Bevestig dat de status Normaal is.

### <a name="from-your-development-computer"></a>Op uw ontwikkelcomputer

Typ in het CF CLI-venster:
```
cf apps
```
Zorg ervoor dat de toepassing OMS Nozzle wordt uitgevoerd.

## <a name="view-the-data-in-the-azure-portal"></a>De gegevens bekijken in de Azure-portal

Als u de oplossing voor controle via de Marketplace-sjabloon hebt geïmplementeerd, gaat u naar Azure portal en zoek de oplossing. U kunt de oplossing vinden in de resourcegroep die u hebt opgegeven in de sjabloon. Klik op de oplossing, blader naar de "Log Analytics-Console', de vooraf geconfigureerde weergaven worden weergegeven, met de hoogste Cloud Foundry system-KPI's, toepassingsgegevens, waarschuwingen en statusmetrieken van de virtuele machine. 

Als u de Log Analytics-werkruimte handmatig hebt gemaakt, volgt u onderstaande stappen voor het maken van de weergaven en waarschuwingen:

### <a name="1-import-the-oms-view"></a>1. Importeren van de OMS-weergave

Via de OMS-portal, blader naar **Weergaveontwerper** > **importeren** > **Bladeren**, en selecteer een van de omsview-bestanden. Selecteer bijvoorbeeld *Cloud Foundry.omsview*, en de weergave opslaan. Nu een tegel wordt weergegeven op de **overzicht** pagina. Selecteer om te zien van de gevisualiseerde metrische gegevens.

U kunt deze weergaven aanpassen of maken van nieuwe weergaven door middel van **Weergaveontwerper**.

De *"Cloud Foundry.omsview"* is een preview-versie van de sjabloon van de Cloud Foundry OMS weergeven. Dit is een volledig geconfigureerde standaardsjabloon. Als u suggesties of feedback over de sjabloon hebt, stuurt u hun naar de [uitgeven sectie](https://github.com/Azure/oms-log-analytics-firehose-nozzle/issues).

### <a name="2-create-alert-rules"></a>2. Waarschuwingsregels maken

U kunt [maken van de waarschuwingen](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts), en aanpassen van de query's en drempelwaarden, indien nodig. De volgende worden waarschuwingen aanbevolen:

| Zoekquery                                                                  | Waarschuwing genereren is gebaseerd op | Description                                                                       |
| ----------------------------------------------------------------------------- | ----------------------- | --------------------------------------------------------------------------------- |
| Type=CF_ValueMetric_CL Origin_s=bbs Name_s="Domain.cf-apps"                   | Aantal resultaten < 1   | **BBS. Domain.cf-apps** geeft aan of het domein cf-apps up-to-date is. Dit betekent dat aanvragen van Cloud-netwerkcontroller CF-App zijn gesynchroniseerd naar bbs. LRPsDesired (AIs Diego gewenst) voor de uitvoering. Er zijn geen gegevens ontvangen betekent dat CF-apps-domein is niet bijgewerkt in de opgegeven periode. |
| Type=CF_ValueMetric_CL Origin_s=rep Name_s=UnhealthyCell Value_d>1            | Aantal resultaten > 0   | Voor cellen Diego, 0 betekent dat in orde en 1 betekent dat niet in orde. Stel de waarschuwing als er meerdere slecht Diego cellen zijn gedetecteerd in de opgegeven periode. |
| Type=CF_ValueMetric_CL Origin_s="bosh-hm-forwarder" Name_s="system.healthy" Value_d=0 | Aantal resultaten > 0 | 1 betekent dat het systeem in orde is en 0 betekent dat het systeem is niet in orde. |
| Type=CF_ValueMetric_CL Origin_s=route_emitter Name_s=ConsulDownMode Value_d>0 | Aantal resultaten > 0   | Ge verzendt regelmatig de status. 0 betekent dat het systeem in orde is, en 1 betekent dat de zender route detecteert dat ge niet actief is. |
| Type=CF_CounterEvent_CL Origin_s=DopplerServer (Name_s="TruncatingBuffer.DroppedMessages" or Name_s="doppler.shedEnvelopes") Delta_d>0 | Aantal resultaten > 0 | De delta-aantal berichten verwijderd doelbewust door Doppler vanwege zwaar wordt belast. |
| Type=CF_LogMessage_CL SourceType_s=LGR MessageType_s=ERR                      | Aantal resultaten > 0   | Loggregator verzendt **LGR** om aan te geven van problemen met het registratieproces. Een voorbeeld van een probleem optreedt, is wanneer de uitvoer van het bericht te hoog is. |
| Type=CF_ValueMetric_CL Name_s=slowConsumerAlert                               | Aantal resultaten > 0   | Wanneer de Nozzle een waarschuwing voor een trage consumenten van loggregator ontvangt, stuurt de **slowConsumerAlert** ValueMetric naar Log Analytics. |
| Type=CF_CounterEvent_CL Job_s=nozzle Name_s=eventsLost Delta_d>0              | Aantal resultaten > 0   | Als een drempelwaarde wordt bereikt door de delta-aantal gebeurtenissen dat verloren, betekent dit dat het Nozzle mogelijk een probleem bij het uitvoeren. |

## <a name="scale"></a>Schalen

U kunt de Nozzle en de loggregator schalen.

### <a name="scale-the-nozzle"></a>Schaal de Nozzle

U moet beginnen met ten minste twee exemplaren van de Nozzle. De firehose verdeelt de werkbelasting over alle exemplaren van de Nozzle.
Als u wilt controleren of de Nozzle kunt houden met het gegevensverkeer van de firehose, instellen van de **slowConsumerAlert** waarschuwing (weergegeven in de vorige sectie, 'Waarschuwingsregels maken'). Nadat u hebt gewaarschuwd, volgt u de [richtlijnen voor trage Nozzle](https://docs.pivotal.io/pivotalcf/1-11/loggregator/log-ops-guide.html#slow-noz) om te bepalen of schalen nodig is.
Om omhoog te schalen de Nozzle, moet u Apps Manager of de CF-CLI gebruiken voor het verhogen van de getallen exemplaar of het geheugen of schijfruimte resources voor de Nozzle.

### <a name="scale-the-loggregator"></a>Schaal de loggregator

Loggregator verzendt een **LGR** logboekbericht om aan te geven van problemen met het registratieproces. U kunt de waarschuwing om te bepalen of de loggregator moet worden opgeschaald bewaken.
Als u wilt de loggregator opschalen, vergroten Doppler buffer of toevoegen van extra Doppler server-exemplaren in het manifest CF. Zie voor meer informatie, [de richtlijnen voor het schalen van de loggregator](https://docs.cloudfoundry.org/running/managing-cf/logging-config.html#scaling).

## <a name="update"></a>Update

Voor het bijwerken van de Nozzle met een nieuwere versie, downloaden van de nieuwe Nozzle release, volg de stappen in de voorgaande 'De Nozzle implementeren'-sectie en pushen van de toepassing opnieuw uit.

### <a name="remove-the-nozzle-from-ops-manager"></a>De Nozzle uit Operations Manager verwijderen

1. Meld u aan Operations Manager.
2. Zoek de **Microsoft Azure Log Analytics-pijp voor PCF** tegel.
3. Selecteer het pictogram voor garbagecollection en Bevestig de verwijdering.

### <a name="remove-the-nozzle-from-your-development-computer"></a>De Nozzle op de ontwikkelcomputer verwijderen

In uw CF CLI-venster, typt u:
```
cf delete <App Name> -r
```

Als u de Nozzle verwijdert, wordt de gegevens in de OMS-portal niet automatisch verwijderd. Het is verlopen op basis van uw instelling voor het bewaren van Log Analytics.

## <a name="support-and-feedback"></a>Ondersteuning en feedback

Azure Log Analytics-pijp is open source. Stuur uw vragen en feedback aan de [GitHub sectie](https://github.com/Azure/oms-log-analytics-firehose-nozzle/issues). Als u wilt een Azure-ondersteuningsaanvraag openen, kiest u 'Virtuele Machine met Cloud Foundry' als de categorie van de service. 

## <a name="next-step"></a>Volgende stap

Maatstaven voor prestaties van virtuele machine zijn van PCF2.0, overgebracht naar Azure log analytics-pijp door systeem metrische gegevens doorstuurserver en geïntegreerd in de Log Analytics-werkruimte. U hoeft niet meer de Log Analytics-agent voor de VM-maatstaven voor prestaties. U kunt echter nog steeds de Log Analytics-agent gebruiken om Syslog-gegevens te verzamelen. De Log Analytics-agent is geïnstalleerd als een invoegtoepassing Bosh voor uw CF VM's. 

Zie voor meer informatie, [implementeren Log Analytics-agent op uw implementatie en Cloud Foundry](https://github.com/Azure/oms-agent-for-linux-boshrelease).
