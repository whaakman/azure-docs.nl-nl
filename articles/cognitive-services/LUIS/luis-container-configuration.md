---
title: Docker-container-instellingen
titleSuffix: Language Understanding - Azure Cognitive Services
description: De LUIS container runtime-omgeving is geconfigureerd met behulp van de `docker run` opdracht argumenten. LUIS heeft enkele vereiste instellingen, samen met een aantal optionele instellingen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 12/04/2018
ms.author: diberry
ms.openlocfilehash: 98828589832d69ada11205e471314a153a566766
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53080262"
---
# <a name="configure-containers"></a>Containers configureren

De Language Understanding (LUIS) container runtime-omgeving is geconfigureerd met behulp van de `docker run` opdracht argumenten. LUIS heeft enkele vereiste instellingen, samen met een aantal optionele instellingen. Verschillende [voorbeelden](#example-docker-run-commands) van de opdracht beschikbaar zijn. De container-specifieke instellingen voor de invoer zijn [instellingen voor koppelen](#mount-settings) en de instellingen voor facturering. 

Container-instellingen zijn [hiërarchische](#hierarchical-settings) en kan worden ingesteld met [omgevingsvariabelen](#environment-variable-settings) of docker [opdrachtregelargumenten](#command-line-argument-settings).

## <a name="configuration-settings"></a>Configuratie-instellingen

Deze container heeft de volgende configuratie-instellingen:

|Vereist|Instelling|Doel|
|--|--|--|
|Ja|[ApiKey](#apikey-setting)|Gebruikt voor het bijhouden van informatie over facturering.|
|Nee|[ApplicationInsights](#applicationinsights-setting)|Hiermee kunt u toevoegen [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) ondersteuning voor telemetrie naar de container.|
|Ja|[Facturering](#billing-setting)|Hiermee geeft u de URI van het eindpunt van de _Language Understanding_ resource in Azure.|
|Ja|[Gebruiksrechtovereenkomst](#eula-setting)| Geeft aan dat u de licentie voor de container hebt geaccepteerd.|
|Nee|[Fluentd](#fluentd-settings)|Logboek schrijven en, optioneel, metrische gegevens naar een Fluentd-server.|
|Nee|[Logboekregistratie](#logging-settings)|ASP.NET Core logboekregistratie van ondersteuning voor de container biedt. |
|Ja|[Hiermee wordt gekoppeld](#mount-settings)|Lezen en schrijven van gegevens uit [hostcomputer](luis-container-howto.md#the-host-computer) container en naar de container naar host-computer.|

> [!IMPORTANT]
> De [ `ApiKey` ](#apikey-setting), [ `Billing` ](#billing-setting), en [ `Eula` ](#eula-setting) instellingen samen worden gebruikt en u moet geldige waarden opgeven voor alle drie deze, anders uw container start niet. Zie voor meer informatie over het gebruik van deze configuratie-instellingen voor het starten van een container [facturering](luis-container-howto.md#billing).

## <a name="apikey-setting"></a>ApiKey instelling

De `ApiKey` instelling geeft u aan de Azure-resource-sleutel die wordt gebruikt voor het bijhouden van informatie over facturering voor de container. U moet een waarde opgeven voor de ApiKey en de waarde moet een geldige sleutel voor de _Language Understanding_ resource die is opgegeven voor de [ `Billing` ](#billing-setting) configuratie-instelling.

Deze instelling kan worden gevonden op twee plaatsen:

* Azure-portal: **van Language Understanding** resourcebeheer onder **sleutels**
* LUIS-portal: **sleutels en het eindpunt instellingen** pagina. 

Gebruik niet de starter-sleutel of de ontwerphandleiding. 

## <a name="applicationinsights-setting"></a>Application Insights-instelling

De `ApplicationInsights` instelling kunt u om toe te voegen [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) ondersteuning voor telemetrie naar de container. Application Insights biedt uitgebreide bewaking van de container. U kunt de container voor beschikbaarheid, prestaties en gebruik eenvoudig bewaken. U kunt ook snel identificeren en opsporen van fouten in de container.

De volgende tabel beschrijft de configuratieinstellingen die worden ondersteund onder de `ApplicationInsights` sectie.

|Vereist| Name | Gegevenstype | Description |
|--|------|-----------|-------------|
|Nee| `InstrumentationKey` | Reeks | De instrumentatiesleutel van de Application Insights-exemplaar aan welke telemetrie gegevens voor de container is verzonden. Zie voor meer informatie, [Application Insights voor ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). <br><br>Voorbeeld:<br>`InstrumentationKey=123456789`|


## <a name="billing-setting"></a>Facturering instellen

De `Billing` instelling geeft u aan de URI van het eindpunt van de _Language Understanding_ resource in Azure gebruikt voor het meten van factureringsgegevens voor de container. U moet een waarde voor deze configuratie-instelling opgeven en de waarde moet een geldige URI van het eindpunt voor een _Language Understanding_ resource in Azure.

Deze instelling kan worden gevonden op twee plaatsen:

* Azure-portal: **van Language Understanding** overzicht, met het label `Endpoint`
* LUIS-portal: **sleutels en het eindpunt instellingen** pagina als onderdeel van de URI van het eindpunt.

|Vereist| Name | Gegevenstype | Description |
|--|------|-----------|-------------|
|Ja| `Billing` | Reeks | URI van de facturering-eindpunt<br><br>Voorbeeld:<br>`Billing=https://westus.api.cognitive.microsoft.com/luis/v2.0` |

## <a name="eula-setting"></a>Overeenkomst instelling

De `Eula` instelling geeft aan dat u de licentie voor de container hebt geaccepteerd. U moet een waarde voor deze configuratie-instelling opgeven en de waarde moet worden ingesteld op `accept`.

|Vereist| Name | Gegevenstype | Description |
|--|------|-----------|-------------|
|Ja| `Eula` | Reeks | Instemming met licentie vereisen<br><br>Voorbeeld:<br>`Eula=accept` |

Cognitive Services-containers zijn in licentie gegeven onder [uw overeenkomst](https://go.microsoft.com/fwlink/?linkid=2018657) voor uw gebruik van Azure. Als u een bestaande overeenkomst voor uw gebruik van Azure hebt, gaat u ermee akkoord dat uw overeenkomst voor gebruik van Azure is de [Microsoft Online Subscription overeenkomst](https://go.microsoft.com/fwlink/?linkid=2018755), waarin de [voorwaarden voor onlineservices ](https://go.microsoft.com/fwlink/?linkid=2018760). Voor Preview-versies, gaat u ook akkoord met de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://go.microsoft.com/fwlink/?linkid=2018815). Met behulp van de container gaat u akkoord met deze voorwaarden.

## <a name="fluentd-settings"></a>Fluentd-instellingen

Fluentd is een open-source-gegevensverzamelaar voor centrale logboekregistratie. De `Fluentd` instellingen beheren van de container-verbinding met een [Fluentd](https://www.fluentd.org) server. De LUIS-container voorzien van een provider van de logboekregistratie Fluentd, waardoor uw container om te schrijven logboeken en, optioneel, metrische gegevens naar een Fluentd-server.

De volgende tabel beschrijft de configuratieinstellingen die worden ondersteund onder de `Fluentd` sectie.

| Name | Gegevenstype | Description |
|------|-----------|-------------|
| `Host` | Reeks | De IP-adres of de DNS-hostnaam van de Fluentd-server. |
| `Port` | Geheel getal | De poort van de Fluentd-server.<br/> De standaardwaarde is 24224. |
| `HeartbeatMs` | Geheel getal | De heartbeat-interval in milliseconden. Als er geen verkeer van de gebeurtenis is verzonden voordat dit interval is verstreken, wordt er een heartbeat naar de Fluentd-server verzonden. De standaardwaarde is 60000 milliseconden (1 minuut). |
| `SendBufferSize` | Geheel getal | De bufferruimte netwerk verzendt in bytes, dat wordt toegewezen voor bewerkingen. De standaardwaarde is 32768 bytes (32 kB). |
| `TlsConnectionEstablishmentTimeoutMs` | Geheel getal | De time-out in milliseconden, een SSL/TLS-verbinding met de Fluentd-server tot stand brengen. De standaardwaarde is 10000 milliseconden (10 seconden).<br/> Als `UseTLS` is ingesteld op false, deze waarde wordt genegeerd. |
| `UseTLS` | Booleaans | Geeft aan of SSL/TLS in de container moet worden gebruikt voor communicatie met de Fluentd-server. De standaardwaarde is false. |

## <a name="logging-settings"></a>Instellingen voor logboekregistratie

De `Logging` instellingen beheren van ASP.NET Core logboekregistratie ondersteuning voor de container. U kunt de dezelfde configuratie-instellingen en waarden voor de container die u voor een ASP.NET Core-toepassing gebruikt. 

De volgende logboekregistratie-providers worden ondersteund door de LUIS-container:

|Provider|Doel|
|--|--|
|[Console](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#console-provider)|De ASP.NET Core `Console` provider voor logboekregistratie. Alle configuratie-instellingen voor ASP.NET Core en standaardwaarden voor deze provider voor logboekregistratie worden ondersteund.|
|[Foutopsporing](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#debug-provider)|De ASP.NET Core `Debug` provider voor logboekregistratie. Alle configuratie-instellingen voor ASP.NET Core en standaardwaarden voor deze provider voor logboekregistratie worden ondersteund.|
|[Schijf](#disk-logging)|De provider van de JSON-logboekregistratie. Deze provider voor logboekregistratie schrijft logboekgegevens naar het koppelpunt van de uitvoer.|

### <a name="disk-logging"></a>Logboekregistratie van schijf
  
De `Disk` logboekregistratie-provider ondersteunt de volgende configuratie-instellingen:  

| Name | Gegevenstype | Description |
|------|-----------|-------------|
| `Format` | Reeks | De indeling van de uitvoer voor de logboekbestanden.<br/> **Opmerking:** deze waarde moet worden ingesteld op `json` om in te schakelen van de provider voor logboekregistratie. Als deze waarde is opgegeven zonder ook een koppelpunt uitvoer op te geven bij het instantiëren van een container, wordt er een fout optreedt. |
| `MaxFileSize` | Geheel getal | De maximale grootte in megabytes (MB) van een logboekbestand. Wanneer de grootte van het huidige logboekbestand voldoet aan of deze waarde overschrijdt, wordt een nieuw logboekbestand wordt gestart door de provider voor logboekregistratie. Als -1 is opgegeven, wordt de grootte van het logboekbestand alleen beperkt door de maximale bestandsgrootte, indien aanwezig, voor het koppelen van de uitvoer. De standaardwaarde is 1. |

Zie voor meer informatie over het configureren van ondersteuning voor ASP.NET Core-logboekregistratie [instellingen bestandsconfiguratie](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#settings-file-configuration).

## <a name="mount-settings"></a>Instellingen voor koppelen

Gebruik bind koppelt om te lezen en schrijven van gegevens naar en van de container. U kunt opgeven van een koppelpunt invoer of uitvoer koppelen door op te geven de `--mount` optie in de [docker uitvoeren](https://docs.docker.com/engine/reference/commandline/run/) opdracht. 

De container LUIS geen gebruik maakt van invoer of uitvoer koppelt training of service-gegevens op te slaan. 

De exacte syntaxis van de locatie van de host koppelen, is afhankelijk van het hostbesturingssysteem. Bovendien de [hostcomputer](luis-container-howto.md#the-host-computer)van koppelpunten locatie is mogelijk niet toegankelijk is vanwege een conflict tussen de machtigingen die wordt gebruikt door de docker-service-account en de host koppelen locatie machtigingen. 

De volgende tabel beschrijft de instellingen die worden ondersteund.

|Vereist| Name | Gegevenstype | Description |
|-------|------|-----------|-------------|
|Ja| `Input` | Reeks | Het doel van de invoer koppelen. De standaardwaarde is `/input`. Dit is de locatie van de LUIS-pakketbestanden. <br><br>Voorbeeld:<br>`--mount type=bind,src=c:\input,target=/input`|
|Nee| `Output` | Reeks | Het doel van de uitvoer-koppelpunt. De standaardwaarde is `/output`. Dit is de locatie van de logboeken. Dit omvat logboeken van LUIS-query's en logboeken voor containers. <br><br>Voorbeeld:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="hierarchical-settings"></a>Hiërarchische instellingen

Instellingen voor de container LUIS zijn hiërarchisch, en alle containers in de [hostcomputer](luis-container-howto.md#the-host-computer) gebruiken een gedeelde hiërarchie.

U kunt een van de volgende instellingen opgeven:

* [Omgevingsvariabelen](#environment-variable-settings)
* [Opdrachtregelargumenten](#command-line-argument-settings)

Waarden van omgevingsvariabelen overschrijven opdrachtregelargument waarden, die op zijn beurt de standaardwaarden voor de containerinstallatiekopie te overschrijven. Als u verschillende waarden in een omgevingsvariabele en een opdrachtregelargument voor de dezelfde configuratie-instelling opgeeft, wordt de waarde in de omgevingsvariabele wordt gebruikt door de geïnstantieerde container.

|Prioriteit|Locatie instellen|
|--|--|
|1|Omgevingsvariabele| 
|2|Opdrachtregel|
|3|Standaardwaarde van container-installatiekopie|

### <a name="environment-variable-settings"></a>Instellingen voor de omgevingsvariabelen

De voordelen van het gebruik van omgevingsvariabelen zijn:

* Meerdere instellingen kunnen worden geconfigureerd.
* Meerdere containers kunnen dezelfde instellingen gebruiken.

### <a name="command-line-argument-settings"></a>Opdrachtregelargument-instellingen

Het voordeel van het gebruik van opdrachtregelargumenten is dat elke container verschillende instellingen kunt gebruiken.

## <a name="example-docker-run-commands"></a>Voorbeeld van de docker-opdrachten uitvoeren

De volgende voorbeelden gebruiken de configuratie-instellingen om te laten zien hoe u om te schrijven en gebruik `docker run` opdrachten.  Zodra actief is, de container blijft actief totdat u [stoppen](luis-container-howto.md#stop-the-container) deze.


* **Voortzetting van regel tekens**: de docker-opdrachten in de volgende secties gebruiken de backslash `\`, als een voortzetting van regel tekens. Vervang of verwijder deze op basis van het hostbesturingssysteem vereisten. 
* **Argument order**: de volgorde van de argumenten niet wijzigen, tenzij u bekend bent met docker-containers.

Vervang {_argument_name_} door uw eigen waarden:

| Tijdelijke aanduiding | Waarde | Indeling of voorbeeld |
|-------------|-------|---|
|{ENDPOINT_KEY} | De eindpuntsleutel van het getrainde LUIS-toepassing. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT} | De facturering eindpuntwaarde is beschikbaar op de pagina van de taal begrijpen overzicht van de Azure portal.|https://westus.api.cognitive.microsoft.com/luis/v2.0|

> [!IMPORTANT]
> De `Eula`, `Billing`, en `ApiKey` opties moeten worden opgegeven voor het uitvoeren van de container; anders wordt de container niet start.  Zie voor meer informatie, [facturering](luis-container-howto.md#billing).
> De waarde ApiKey is de **sleutel** pagina van de sleutels en de eindpunten in de portal LUIS en is ook beschikbaar op de pagina sleutels in Azure Language Understanding Resource. 

### <a name="basic-example"></a>Eenvoudige voorbeeld

Het volgende voorbeeld is de minst argumenten mogelijk uit te voeren van de container:

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY}
```

> [!Note] 
> De voorgaande opdracht maakt gebruik van de map uit de `c:` station om te voorkomen van machtigingsconflicten op Windows. Als u gebruiken van een specifieke map als de invoermap wilt, moet u mogelijk de docker verlenen machtiging-service. De voorgaande docker-opdracht maakt gebruik van de backslash `\`, als een voortzetting van regel tekens. Vervang of verwijder deze op basis van uw [hostcomputer](luis-container-howto.md#the-host-computer) vereisten van het besturingssysteem. Wijzig de volgorde van de argumenten niet, tenzij u bekend bent met docker-containers.


### <a name="applicationinsights-example"></a>Voorbeeld van de Application Insights

Het volgende voorbeeld wordt de Application Insights-argument voor het verzenden van telemetrie naar Application Insights, terwijl de container wordt uitgevoerd:

```bash
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY}
InstrumentationKey={INSTRUMENTATION_KEY}
```

### <a name="logging-example-with-command-line-arguments"></a>Voorbeeld van de logboekregistratie met opdrachtregelargumenten

De volgende opdracht stelt het niveau van logboekregistratie, `Logging:Console:LogLevel`, het configureren van het niveau van logboekregistratie voor [ `Information` ](https://msdn.microsoft.com). 

```bash
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY} \
Logging:Console:LogLevel=Information
```

### <a name="logging-example-with-environment-variable"></a>Voorbeeld van de logboekregistratie met omgevingsvariabele

De volgende opdrachten gebruiken een omgevingsvariabele met de naam `Logging:Console:LogLevel` het configureren van het niveau van logboekregistratie voor [ `Information` ](https://msdn.microsoft.com). 

```bash
SET Logging:Console:LogLevel=Information
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={APPLICATION_ID} \
```

## <a name="next-steps"></a>Volgende stappen

* Beoordeling [over het installeren en uitvoeren van containers](luis-container-howto.md)
* Raadpleeg [Veelgestelde vragen (FAQ)](luis-resources-faq.md) het oplossen van problemen met betrekking tot LUIS-functionaliteit.