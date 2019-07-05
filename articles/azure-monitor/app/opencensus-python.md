---
title: Python-toepassingen met Azure Application Insights bewaken | Microsoft Docs
description: Vindt u instructies voor het instellen OpenCensus Python met Application Insights
services: application-insights
keywords: ''
author: reyang
ms.author: reyang
ms.date: 07/02/2019
ms.service: application-insights
ms.topic: conceptual
ms.reviewer: mbullwin
manager: carmonm
ms.openlocfilehash: 2c043ad793dcf5e59eaf460d1ec4aa7a3b48810d
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67541439"
---
# <a name="collect-distributed-traces-from-python-preview"></a>Gedistribueerde traceringen verzamelen van Python (Preview)

Application Insights nu ondersteunt tracering van Python-toepassingen door de integratie met gedistribueerde [OpenCensus](https://opencensus.io). In dit artikel begeleidt u stapsgewijs door het proces van het instellen van OpenCensus voor Python en het ophalen van uw gegevens naar Application Insights.

## <a name="prerequisites"></a>Vereisten

- U hebt een Azure-abonnement nodig.
- Python moet worden geïnstalleerd, in dit artikel wordt gebruikgemaakt van [Python 3.7.0](https://www.python.org/downloads/), hoewel eerdere versies waarschijnlijk met kleine aanpassing werkt.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-application-insights-resource"></a>Application Insights-resource maken

U moet eerst maken van een Application Insights-resource, die een key(ikey) instrumentatie wordt gegenereerd. De ikey wordt vervolgens gebruikt voor het configureren van de SDK OpenCensus voor het verzenden van telemetriegegevens naar Application Insights.

1. Selecteer **een resource maken** > **hulpprogramma's voor ontwikkelaars** > **Application Insights**.

   ![Application Insights-resource toevoegen](./media/opencensus-python/0001-create-resource.png)

   Er wordt een configuratiescherm weergegeven. Gebruik de volgende tabel om de invoervelden in te vullen.

    | Instellingen        | Waarde           | Beschrijving  |
   | ------------- |:-------------|:-----|
   | **Name**      | Globaal unieke waarde | Naam die de app beschrijft die u wilt controleren |
   | **Resourcegroep**     | myResourceGroup      | Naam voor de nieuwe resourcegroep waarin App Insights-gegevens worden gehost |
   | **Location** | East US | Kies een locatie in uw buurt of in de buurt van waar de app wordt gehost |

2. Klik op **Create**.

## <a name="install-opencensus-azure-monitor-exporters"></a>OpenCensus Azure Monitor exporteurs installeren

1. De OpenCensus Azure Monitor exporteurs installeren:

    ```console
    python -m pip install opencensus-ext-azure
    ```

    > [!NOTE]
    > `python -m pip install opencensus-ext-azure` wordt ervan uitgegaan dat u een omgevingsvariabele PATH is ingesteld voor uw Python-installatie hebt. Als u dit niet hebt geconfigureerd, moet u het volledige pad om aan te geven waar uw Python uitvoerbare bestand zich bevindt die resulteren in een opdracht wilt: `C:\Users\Administrator\AppData\Local\Programs\Python\Python37-32\python.exe -m pip install opencensus-ext-azure`.

2. Eerst gaan we enkele traceringsgegevens lokaal genereren. In Python niet actief of uw editor naar keuze, voer de volgende code:

    ```python
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer

    tracer = Tracer(sampler=ProbabilitySampler(1.0))

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

3. De code uitvoeren vraagt herhaaldelijk u een waarde in te voeren. De waarde waarbij elk item worden afgedrukt op de shell, en een bijbehorende stukje **SpanData** worden gegenereerd door de OpenCensus Python-Module. Het project OpenCensus definieert een [ _trace als een boomstructuur van reeksen_](https://opencensus.io/core-concepts/tracing/).
    
    ```
    Enter a value: 4
    4
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='15ac5123ac1f6847', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:22.805429Z', end_time='2019-06-27T18:21:44.933405Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 25
    25
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='2e512f846ba342de', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:44.933405Z', end_time='2019-06-27T18:21:46.156787Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 100
    100
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='f3f9f9ee6db4740a', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:46.157732Z', end_time='2019-06-27T18:21:47.269583Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    ```

4. Wanneer dit is handig voor demonstratiedoeleinden te gebruiken, uiteindelijk gaan we de SpanData naar Application Insights verzenden. Wijzig de code uit de vorige stap op basis van het volgende codevoorbeeld:

    ```python
    from opencensus.ext.azure.trace_exporter import AzureExporter
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    tracer = Tracer(
        exporter=AzureExporter(
            instrumentation_key='00000000-0000-0000-0000-000000000000',
        ),
        sampler=ProbabilitySampler(1.0),
    )

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```
5. Nu wanneer u het pythonscript uitvoeren, moet u nog steeds gevraagd waarden op te geven, maar nu alleen de waarde in de shell wordt afgedrukt.

## <a name="start-monitoring-in-the-azure-portal"></a>Beginnen met controleren in Azure Portal

1. U kunt nu opnieuw openen met de Application Insights **overzicht** pagina in de Azure-portal om weer te geven details over de toepassing die momenteel wordt uitgevoerd. Selecteer **metrische Stream Live**.

   ![Schermopname van het overzichtsvenster met live, metrische gegevensstroom geselecteerd in een rood kader](./media/opencensus-python/0005-overview-live-metrics-stream.png)

2. Ga terug naar de **overzicht** pagina en selecteer **Toepassingsoverzicht** voor een visueel overzicht van de afhankelijkheidsrelaties tussen en de timing van de oproep tussen onderdelen van uw toepassing.

    ![Schermafbeelding van de basistoepassing toewijzing](./media/opencensus-python/0007-application-map.png)

    Omdat we alleen een methodeaanroep traceren zijn, is onze overzicht van de toepassing niet zo interessant. Maar overzicht van de toepassing kunt schalen om te visualiseren veel gedistribueerde toepassingen:

   ![Toepassingskaart](media/opencensus-python/application-map.png)

3. Selecteer **prestaties onderzoeken** gedetailleerde prestatieanalyse uitvoeren en de hoofdoorzaak van trage prestaties vast.

    ![Schermafbeelding van deelvenster prestaties](./media/opencensus-python/0008-performance.png)

4. Selecteren **voorbeelden** en vervolgens te klikken op een van de voorbeelden die worden weergegeven in het rechter deelvenster op de ervaring van de details van end-to-end-transactie wordt gestart. Hoewel onze voorbeeld-app ons slechts één gebeurtenis leert, zou een complexere toepassing kunt u de end-to-end-transactie op niveau van een afzonderlijke gebeurtenis aanroepstack verkennen.

     ![Schermafbeelding van end-to-end-transactie-interface](./media/opencensus-python/0009-end-to-end-transaction.png)

## <a name="opencensus-for-python"></a>OpenCensus voor Python

* [Aanpassing](https://github.com/census-instrumentation/opencensus-python/blob/master/README.rst#customization)
* [Flask-integratie](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-flask)
* [Django-integratie](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-django)
* [MySQL-integratie](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-mysql)
* [PostgreSQL](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-postgresql)

## <a name="next-steps"></a>Volgende stappen

* [OpenCensus Python on GitHub](https://github.com/census-instrumentation/opencensus-python)
* [Overzicht van de toepassing](./../../azure-monitor/app/app-map.md)
* [End-to-end-prestaties controleren](./../../azure-monitor/learn/tutorial-performance.md)