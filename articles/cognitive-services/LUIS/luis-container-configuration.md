---
title: Docker-container-instellingen
titleSuffix: Language Understanding - Azure Cognitive Services
description: De LUIS container runtime-omgeving is geconfigureerd met behulp van de `docker run` opdracht argumenten. LUIS heeft enkele vereiste instellingen, samen met een aantal optionele instellingen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: diberry
ms.openlocfilehash: afd29c1689d6d467a42a7c3c60f9a1dccd1a66f0
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66242613"
---
# <a name="configure-language-understanding-docker-containers"></a>Language Understanding Docker-containers configureren 

De **Language Understanding** (LUIS) container runtime-omgeving is geconfigureerd met behulp van de `docker run` opdracht argumenten. LUIS heeft enkele vereiste instellingen, samen met een aantal optionele instellingen. Verschillende [voorbeelden](#example-docker-run-commands) van de opdracht beschikbaar zijn. De container-specifieke instellingen voor de invoer zijn [instellingen voor koppelen](#mount-settings) en de instellingen voor facturering. 

## <a name="configuration-settings"></a>Configuratie-instellingen

Deze container heeft de volgende configuratie-instellingen:

|Vereist|Instelling|Doel|
|--|--|--|
|Ja|[ApiKey](#apikey-setting)|Gebruikt voor het bijhouden van informatie over facturering.|
|Nee|[ApplicationInsights](#applicationinsights-setting)|Hiermee kunt u toevoegen [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) ondersteuning voor telemetrie naar de container.|
|Ja|[Billing](#billing-setting)|Hiermee wordt de eindpunt-URI van de serviceresource in Azure opgegeven.|
|Ja|[Eula](#eula-setting)| Geeft aan dat u de licentie voor de container hebt geaccepteerd.|
|Nee|[Fluentd](#fluentd-settings)|Logboek schrijven en, optioneel, metrische gegevens naar een Fluentd-server.|
|Nee|[HTTP-Proxy](#http-proxy-credentials-settings)|Configureer een HTTP-proxy voor uitgaande aanvragen.|
|Nee|[Logging](#logging-settings)|Biedt ondersteuning voor ASP.NET Core logboekregistratie voor de container. |
|Ja|[Mounts](#mount-settings)|Gegevens lezen en schrijven van de hostcomputer naar de container en van de container naar de hostcomputer.|

> [!IMPORTANT]
> De [ `ApiKey` ](#apikey-setting), [ `Billing` ](#billing-setting), en [ `Eula` ](#eula-setting) instellingen samen worden gebruikt en u moet geldige waarden opgeven voor alle drie deze, anders uw container start niet. Zie voor meer informatie over het gebruik van deze configuratie-instellingen voor het starten van een container [facturering](luis-container-howto.md#billing).

## <a name="apikey-setting"></a>ApiKey instelling

De `ApiKey` instelling geeft u aan de Azure-resource-sleutel die wordt gebruikt voor het bijhouden van informatie over facturering voor de container. U moet een waarde opgeven voor de ApiKey en de waarde moet een geldige sleutel voor de _Cognitive Services_ resource die is opgegeven voor de [ `Billing` ](#billing-setting) configuratie-instelling.

Deze instelling kan worden gevonden in de volgende locaties:

* Azure Portal: **Cognitive Services** resourcebeheer onder **sleutels**
* LUIS-portal: **Sleutels en eindpuntinstellingen** pagina. 

Gebruik niet de starter-sleutel of de ontwerphandleiding. 

## <a name="applicationinsights-setting"></a>Application Insights-instelling

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-setting"></a>Facturering instellen

De `Billing` instelling geeft u aan de URI van het eindpunt van de _Cognitive Services_ resource in Azure gebruikt voor het meten van factureringsgegevens voor de container. U moet een waarde voor deze configuratie-instelling opgeven en de waarde moet een geldige URI van het eindpunt voor een _Cognitive Services_ resource in Azure. Gebruik de container rapporteert over elke 10 tot 15 minuten.

Deze instelling kan worden gevonden in de volgende locaties:

* Azure Portal: **Cognitive Services** overzicht, met het label `Endpoint`
* LUIS-portal: **Sleutels en eindpuntinstellingen** pagina als onderdeel van de URI van het eindpunt.

Houd er rekening mee te nemen de `luis/v2.0` routering in de URL, zoals wordt weergegeven in de volgende tabel:


|Vereist| Name | Gegevenstype | Description |
|--|------|-----------|-------------|
|Ja| `Billing` | Reeks | URI van de facturering-eindpunt<br><br>Voorbeeld:<br>`Billing=https://westus.api.cognitive.microsoft.com/luis/v2.0` |

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

De container LUIS geen gebruik maakt van invoer of uitvoer koppelt training of service-gegevens op te slaan. 

De exacte syntaxis van de locatie van de host koppelen, is afhankelijk van het hostbesturingssysteem. Bovendien de [hostcomputer](luis-container-howto.md#the-host-computer)van koppelpunten locatie is mogelijk niet toegankelijk is vanwege een conflict tussen de machtigingen die wordt gebruikt door de docker-service-account en de host koppelen locatie machtigingen. 

De volgende tabel beschrijft de instellingen die worden ondersteund.

|Vereist| Name | Gegevenstype | Description |
|-------|------|-----------|-------------|
|Ja| `Input` | Reeks | Het doel van de invoer koppelen. De standaardwaarde is `/input`. Dit is de locatie van de LUIS-pakketbestanden. <br><br>Voorbeeld:<br>`--mount type=bind,src=c:\input,target=/input`|
|Nee| `Output` | Reeks | Het doel van de uitvoer-koppelpunt. De standaardwaarde is `/output`. Dit is de locatie van de logboeken. Dit omvat logboeken van LUIS-query's en logboeken voor containers. <br><br>Voorbeeld:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Voorbeeld van de docker-opdrachten uitvoeren

De volgende voorbeelden gebruiken de configuratie-instellingen om te laten zien hoe u om te schrijven en gebruik `docker run` opdrachten.  Zodra actief is, de container blijft actief totdat u [stoppen](luis-container-howto.md#stop-the-container) deze.

* Deze voorbeelden gebruiken de map uit de `c:` station om te voorkomen van machtigingsconflicten op Windows. Als u gebruiken van een specifieke map als de invoermap wilt, moet u mogelijk de docker verlenen machtiging-service. 
* Wijzig de volgorde van de argumenten niet, tenzij u bekend bent met docker-containers.
* Als u een ander besturingssysteem gebruikt, gebruikt u de juiste console/terminal, de syntaxis van de map voor koppelingen en regel voortzetting teken voor uw systeem. Deze voorbeelden wordt ervan uitgegaan dat een Windows-console met een voortzetting van regel tekens `^`. Omdat de container een Linux-besturingssysteem is, gebruikt het koppelpunt van het doel een Linux-stijl map-syntaxis.

Houd er rekening mee te nemen de `luis/v2.0` routering in de URL, zoals wordt weergegeven in de volgende tabel.

Vervang {_argument_name_} door uw eigen waarden:

| Tijdelijke aanduiding | Waarde | Indeling of voorbeeld |
|-------------|-------|---|
|{ENDPOINT_KEY} | De eindpuntsleutel van het getrainde LUIS-toepassing. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT} | De facturering waarde van het eindpunt is beschikbaar op de Azure `Cognitive Services` pagina overzicht. |https://westus.api.cognitive.microsoft.com/luis/v2.0|

> [!IMPORTANT]
> De `Eula`, `Billing`, en `ApiKey` opties moeten worden opgegeven voor het uitvoeren van de container; anders wordt de container niet start.  Zie voor meer informatie, [facturering](luis-container-howto.md#billing).
> De waarde ApiKey is de **sleutel** pagina van de sleutels en de eindpunten in de portal LUIS en is ook beschikbaar in de Azure `Cognitive Services` resourcepagina sleutels. 

### <a name="basic-example"></a>Eenvoudige voorbeeld

Het volgende voorbeeld is de minst argumenten mogelijk uit te voeren van de container:

```console
docker run --rm -it -p 5000:5000 --memory 4g --cpus 2 ^
--mount type=bind,src=c:\input,target=/input ^
--mount type=bind,src=c:\output,target=/output ^
mcr.microsoft.com/azure-cognitive-services/luis:latest ^
Eula=accept ^
Billing={BILLING_ENDPOINT} ^
ApiKey={ENDPOINT_KEY}
```

### <a name="applicationinsights-example"></a>Voorbeeld van de Application Insights

Het volgende voorbeeld wordt de Application Insights-argument voor het verzenden van telemetrie naar Application Insights, terwijl de container wordt uitgevoerd:

```console
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 ^
--mount type=bind,src=c:\input,target=/input ^
--mount type=bind,src=c:\output,target=/output ^
mcr.microsoft.com/azure-cognitive-services/luis:latest ^
Eula=accept ^
Billing={BILLING_ENDPOINT} ^
ApiKey={ENDPOINT_KEY} ^
InstrumentationKey={INSTRUMENTATION_KEY}
```

### <a name="logging-example"></a>Voorbeeld van de logboekregistratie 

De volgende opdracht stelt het niveau van logboekregistratie, `Logging:Console:LogLevel`, het configureren van het niveau van logboekregistratie voor [ `Information` ](https://msdn.microsoft.com). 

```console
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 ^
--mount type=bind,src=c:\input,target=/input ^
--mount type=bind,src=c:\output,target=/output ^
mcr.microsoft.com/azure-cognitive-services/luis:latest ^
Eula=accept ^
Billing={BILLING_ENDPOINT} ^
ApiKey={ENDPOINT_KEY} ^
Logging:Console:LogLevel:Default=Information
```

## <a name="next-steps"></a>Volgende stappen

* Beoordeling [over het installeren en uitvoeren van containers](luis-container-howto.md)
* Raadpleeg [probleemoplossing](troubleshooting.md) het oplossen van problemen met betrekking tot LUIS-functionaliteit.
* Meer [Cognitive Services-Containers](../cognitive-services-container-support.md)
