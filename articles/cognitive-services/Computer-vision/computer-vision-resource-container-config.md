---
title: Configureren van de containers - Computer Vision
titlesuffix: Azure Cognitive Services
description: Diverse instellingen voor tekst herkennen containers in Computer Vision configureren.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 4785acacf4749ebddf362fa036b6f6a17cb0056b
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55868375"
---
# <a name="configure-recognize-text-docker-containers"></a>Configureren van herkennen tekst Docker-containers

De **tekst herkennen** container runtime-omgeving is geconfigureerd met behulp van de `docker run` opdracht argumenten. Deze container heeft meerdere vereiste instellingen, samen met een aantal optionele instellingen. Verschillende [voorbeelden](#example-docker-run-commands) van de opdracht beschikbaar zijn. De container-specifieke instellingen zijn de instellingen voor facturering. 

Container-instellingen zijn [hiërarchische](#hierarchical-settings) en kan worden ingesteld met [omgevingsvariabelen](#environment-variable-settings) of docker [opdrachtregelargumenten](#command-line-argument-settings).

## <a name="configuration-settings"></a>Configuratie-instellingen

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> De [ `ApiKey` ](#apikey-setting), [ `Billing` ](#billing-setting), en [ `Eula` ](#eula-setting) instellingen samen worden gebruikt en u moet geldige waarden opgeven voor alle drie deze, anders uw container start niet. Zie voor meer informatie over het gebruik van deze configuratie-instellingen voor het starten van een container [facturering](computer-vision-how-to-install-containers.md#billing).

## <a name="apikey-configuration-setting"></a>ApiKey configuratie-instelling

De `ApiKey` instelling geeft u aan de Azure-resource-sleutel die wordt gebruikt voor het bijhouden van informatie over facturering voor de container. U moet een waarde opgeven voor de ApiKey en de waarde moet een geldige sleutel voor de _Computer Vision_ resource die is opgegeven voor de [ `Billing` ](#billing-setting) configuratie-instelling.

Deze instelling kan worden gevonden in de volgende plaats:

* Azure Portal: **Computer-Vision** resourcebeheer onder **sleutels**

## <a name="applicationinsights-setting"></a>Application Insights-instelling

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Facturering van configuratie-instelling

De `Billing` instelling geeft u aan de URI van het eindpunt van de _Computer Vision_ resource in Azure gebruikt voor het meten van factureringsgegevens voor de container. U moet een waarde voor deze configuratie-instelling opgeven en de waarde moet een geldige URI van het eindpunt voor een _Computer Vision_ resource in Azure.

Deze instelling kan worden gevonden in de volgende plaats:

* Azure Portal: **Computer-Vision** overzicht, met het label `Endpoint`

|Vereist| Name | Gegevenstype | Description |
|--|------|-----------|-------------|
|Ja| `Billing` | Reeks | URI van de facturering-eindpunt<br><br>Voorbeeld:<br>`Billing=https://westcentralus.api.cognitive.microsoft.com/vision/v1.0` |

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

De Computer Vision-containers gebruik geen invoer of uitvoer koppelt training of service-gegevens op te slaan. 

De exacte syntaxis van de locatie van de host koppelen, is afhankelijk van het hostbesturingssysteem. Bovendien de [hostcomputer](computer-vision-how-to-install-containers.md#the-host-computer)van koppelpunten locatie is mogelijk niet toegankelijk is vanwege een conflict tussen de machtigingen die wordt gebruikt door de Docker-service-account en de host koppelen locatie machtigingen. 

|Optioneel| Name | Gegevenstype | Description |
|-------|------|-----------|-------------|
|Niet toegestaan| `Input` | String | Computer Vision-containers gebruik dit niet.|
|Optioneel| `Output` | String | Het doel van de uitvoer-koppelpunt. De standaardwaarde is `/output`. Dit is de locatie van de logboeken. Dit omvat de logboeken voor containers. <br><br>Voorbeeld:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="hierarchical-settings"></a>Hiërarchische instellingen

[!INCLUDE [Container shared configuration hierarchical settings](../../../includes/cognitive-services-containers-configuration-shared-hierarchical-settings.md)]

## <a name="example-docker-run-commands"></a>Voorbeeld van de docker-opdrachten uitvoeren 

De volgende voorbeelden gebruiken de configuratie-instellingen om te laten zien hoe u om te schrijven en gebruik `docker run` opdrachten.  Zodra actief is, de container blijft actief totdat u [stoppen](computer-vision-how-to-install-containers.md#stop-the-container) deze.

* **Voortzetting van regel tekens**: De Docker-opdrachten in de volgende secties gebruiken de backslash `\`, als een voortzetting van regel tekens. Vervang of verwijder deze op basis van het hostbesturingssysteem vereisten. 
* **Argument order**: Wijzig de volgorde van de argumenten niet, tenzij u bekend bent met Docker-containers.

Vervang {_argument_name_} door uw eigen waarden:

| Tijdelijke aanduiding | Waarde | Indeling of voorbeeld |
|-------------|-------|---|
|{BILLING_KEY} | De eindpuntsleutel van de Computer Vision-resource. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT_URI} | De facturering eindpuntwaarde inclusief regio.|`https://westcentralus.api.cognitive.microsoft.com/vision/v1.0`|

> [!IMPORTANT]
> De `Eula`, `Billing`, en `ApiKey` opties moeten worden opgegeven voor het uitvoeren van de container; anders wordt de container niet start.  Zie voor meer informatie, [facturering](computer-vision-how-to-install-containers.md#billing).
> De waarde ApiKey is de **sleutel** van de pagina sleutels in Azure Computer Vision-Resource. 

## <a name="recognize-text-container-docker-examples"></a>Herkennen tekst container Docker-voorbeelden

De volgende Docker-voorbeelden zijn voor de container van de tekst herkennen. 

### <a name="basic-example"></a>Eenvoudige voorbeeld 

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} \
  ApiKey={BILLING_KEY} 
  ```

### <a name="logging-example-with-command-line-arguments"></a>Voorbeeld van de logboekregistratie met opdrachtregelargumenten

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} \
  ApiKey={BILLING_KEY} \
  Logging:Console:LogLevel=Information
  ```

### <a name="logging-example-with-environment-variable"></a>Voorbeeld van de logboekregistratie met omgevingsvariabele

  ```Docker
  SET Logging:Console:LogLevel=Information
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} \
  ApiKey={BILLING_KEY}
  ```

## <a name="next-steps"></a>Volgende stappen

* Beoordeling [over het installeren en uitvoeren van containers](computer-vision-how-to-install-containers.md)
