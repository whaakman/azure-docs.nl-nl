---
title: How Personalizer Works
titleSuffix: Azure Cognitive Services
description: Leer hoe u de container Personalizer uitvoert met de docker-configuratie-opdrachtinstellingen uitvoeren.
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: 85344d432c169ce8c4adffd059059660672df02e
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65158551"
---
# <a name="configure-personalizer-containers"></a>Personalizer containers configureren

De **Personalizer** container runtime-omgeving is geconfigureerd met behulp van de `docker run` opdracht argumenten. Deze container heeft meerdere vereiste instellingen, samen met een aantal optionele instellingen. Verschillende [voorbeelden](#example-docker-run-commands) van de opdracht beschikbaar zijn. De container-specifieke instellingen zijn de instellingen voor facturering. 

## <a name="configuration-settings"></a>Configuratie-instellingen

Deze container heeft de volgende configuratie-instellingen:

|Vereist|Instelling|Doel|
|--|--|--|
|Ja|[ApiKey](#apikey-configuration-setting)|Gebruikt voor het bijhouden van informatie over facturering.|
|Nee|[ApplicationInsights](#applicationinsights-setting)|Hiermee kunt u toevoegen [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) ondersteuning voor telemetrie naar de container.|
|Ja|[Billing](#billing-configuration-setting)|Hiermee wordt de eindpunt-URI van de serviceresource in Azure opgegeven.|
|Ja|[Eula](#eula-setting)| Geeft aan dat u de licentie voor de container hebt geaccepteerd.|
|Nee|[Fluentd](#fluentd-settings)|Logboek schrijven en, optioneel, metrische gegevens naar een Fluentd-server.|
|Nee|[HTTP-Proxy](#http-proxy-credentials-settings)|Configureer een HTTP-proxy voor uitgaande aanvragen.|
|Nee|[Logging](#logging-settings)|Biedt ondersteuning voor ASP.NET Core logboekregistratie voor de container. |
|Nee|[Mounts](#mount-settings)|Gegevens lezen en schrijven van de hostcomputer naar de container en van de container naar de hostcomputer.|

> [!IMPORTANT]
> De [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), en [ `Eula` ](#eula-setting) instellingen samen worden gebruikt en u moet geldige waarden opgeven voor alle drie deze, anders uw container start niet. Zie voor meer informatie over het gebruik van deze configuratie-instellingen voor het starten van een container [facturering](personalizer-container-howto.md#billing).

## <a name="apikey-configuration-setting"></a>ApiKey configuratie-instelling

De `ApiKey` instelling geeft u aan de Azure-resource-sleutel die wordt gebruikt voor het bijhouden van informatie over facturering voor de container. U moet een waarde opgeven voor de ApiKey en de waarde moet een geldige sleutel voor de _Personalizer_ resource die is opgegeven voor de [ `Billing` ](#billing-configuration-setting) configuratie-instelling.

Deze instelling kan worden gevonden in de volgende plaats:

* Azure Portal: **De personalizer** resourcebeheer onder **sleutels**

## <a name="applicationinsights-setting"></a>Application Insights-instelling

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Facturering van configuratie-instelling

De `Billing` instelling geeft u aan de URI van het eindpunt van de _Personalizer_ resource in Azure gebruikt voor het meten van factureringsgegevens voor de container. U moet een waarde voor deze configuratie-instelling opgeven en de waarde moet een geldige URI van het eindpunt voor een _Personalizer_ resource in Azure.

Deze instelling kan worden gevonden in de volgende plaats:

* Azure Portal: **De personalizer** overzicht, met het label `Endpoint`

|Vereist| Name | Gegevenstype | Description |
|--|------|-----------|-------------|
|Ja| `Billing` | Reeks | URI van de facturering-eindpunt<br><br>Voorbeeld:<br>`Billing=https://westus.api.cognitive.microsoft.com/sts/v1.0` |

## <a name="eula-setting"></a>Overeenkomst instelling

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Fluentd-instellingen

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>HTTP-proxy-instellingen voor referenties

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Instellingen voor logboekregistratie
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]


## <a name="mount-settings"></a>Instellingen voor koppelen

Gebruik bind koppelt om te lezen en schrijven van gegevens naar en van de container. U kunt opgeven van een koppelpunt invoer of uitvoer koppelen door op te geven de `--mount` optie in de [docker uitvoeren](https://docs.docker.com/engine/reference/commandline/run/) opdracht.

De containers gebruik geen invoer of uitvoer koppelt training of service-gegevens op te slaan. 

De exacte syntaxis van de locatie van de host koppelen, is afhankelijk van het hostbesturingssysteem. Bovendien de [hostcomputer](personalizer-container-howto.md#the-host-computer)van koppelpunten locatie is mogelijk niet toegankelijk is vanwege een conflict tussen de machtigingen die wordt gebruikt door de docker-service-account en de host koppelen locatie machtigingen. 

|Optioneel| Name | Gegevenstype | Description |
|-------|------|-----------|-------------|
|Niet toegestaan| `Input` | String | Personalizer containers gebruik dit niet.|
|Optioneel| `Output` | String | Het doel van de uitvoer-koppelpunt. De standaardwaarde is `/output`. Dit is de locatie van de logboeken. Dit omvat de logboeken voor containers. <br><br>Voorbeeld:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Voorbeeld van de docker-opdrachten uitvoeren 

De volgende voorbeelden gebruiken de configuratie-instellingen om te laten zien hoe u om te schrijven en gebruik `docker run` opdrachten.  Zodra actief is, de container blijft actief totdat u [stoppen](personalizer-container-howto.md#stop-the-container) deze.

* **Voortzetting van regel tekens**: De Docker-opdrachten in de volgende secties gebruiken de backslash `\`, als een teken van de voortzetting van regel voor een bash-shell. Vervang of verwijder deze op basis van het hostbesturingssysteem vereisten. Het teken van de voortzetting van regel voor windows is bijvoorbeeld een caret-teken, `^`. De backslash vervangen door het caret-teken. 
* **Argument order**: Wijzig de volgorde van de argumenten niet, tenzij u bekend bent met Docker-containers.

Vervang de waarde tussen vierkante haken, `{}`, door uw eigen waarden:

| Tijdelijke aanduiding | Waarde | Indeling of voorbeeld |
|-------------|-------|---|
|{BILLING_KEY} | De eindpuntsleutel van de resource Personalizer. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT_URI} | De facturering eindpuntwaarde inclusief regio.|`https://westus2.api.cognitive.microsoft.com/`|

> [!IMPORTANT]
> De `Eula`, `Billing`, en `ApiKey` opties moeten worden opgegeven voor het uitvoeren van de container; anders wordt de container niet start.  Zie voor meer informatie, [facturering](personalizer-container-howto.md#billing).
> De waarde ApiKey is de **sleutel** op de pagina Azure Personalizer Resource sleutels. 

## <a name="personalizer-container-docker-examples"></a>Personalizer container Docker-voorbeelden

De volgende Docker-voorbeelden zijn voor de container Personalizer. 

### <a name="basic-example"></a>Eenvoudige voorbeeld 

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  mcr.microsoft.com/azure-cognitive-services/[container-name] \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} \
  ApiKey={BILLING_KEY} 
  ```

### <a name="logging-example-with-command-line-arguments"></a>Voorbeeld van de logboekregistratie met opdrachtregelargumenten

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  mcr.microsoft.com/azure-cognitive-services/[container-name] \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} ApiKey={BILLING_KEY} \
  Logging:Console:LogLevel:Default=Information
  ```

## <a name="next-steps"></a>Volgende stappen

[Over het gebruik van de Docker-container](personalizer-container-howto.md)