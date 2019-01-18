---
title: Gegevens analyseren met Jupyter-Notebook en Kqlmagic
description: In dit onderwerp leert u hoe het analyseren van gegevens met behulp van Jupyter-Notebook en KQLmagic
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 12/19/2018
ms.openlocfilehash: 83c4ae7ff942579d55a5855c24a28809f60fdc62
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54382451"
---
# <a name="analyze-data-using-jupyter-notebook-and-kql-magic"></a>Gegevens analyseren met Jupyter-Notebook en KQL Magic-pakket

Jupyter-Notebook is een open-source-webtoepassing waarmee u het maken en delen van documenten met live code, vergelijkingen, visualisaties en verhalende tekst. Gebruik omvat het opschonen van gegevens en -transformatie, numerieke simulatie, statistische modellen bouwen, gegevensvisualisatie en machine learning.
[Jupyter-Notebook](https://jupyter.org/) ondersteunt magic-functies die de mogelijkheden van de kernel uitbreiden door de ondersteuning van aanvullende opdrachten. KQL Magic-pakket is een opdracht die de mogelijkheden van de Python-kernel in Jupyter-Notebook breidt, zodat u kunt systeemeigen Kusto language-query's uitvoeren. U kunt eenvoudig combineren met Python en Kusto-querytaal om te vragen en visualiseren van gegevens met behulp van uitgebreide Plot.ly bibliotheek geïntegreerd met `render` opdrachten. Gegevensbronnen voor het uitvoeren van query's worden ondersteund. Deze gegevensbronnen zijn Azure Data Explorer, een snelle en schaalbare data exploration service voor logboek- en telemetrie-gegevens, evenals een Log Analytics en Application Insights. KQL magic werkt ook met de Azure-notitieblokken, Jupyter Lab en Visual Studio Code Jupyter-extensie.

## <a name="prerequisites"></a>Vereisten

- Organisatie-e-mailaccount die deel uitmaakt van Azure Active Directory (AAD).
- Jupyter-Notebook op uw lokale computer is geïnstalleerd of gebruik de Azure-notitieblokken en het voorbeeld klonen [Azure Notebook](https://kustomagicsamples-manojraheja.notebooks.azure.com/j/notebooks/Getting%20Started%20with%20kqlmagic%20on%20Azure%20Data%20Explorer.ipynb)

## <a name="install-kql-magic-library"></a>KQL magic bibliotheek installeren

1. KQL Magic-pakket installeren:

    ```python
    !pip install Kqlmagic --no-cache-dir  --upgrade
    ```
    > [!NOTE]
    > Wanneer u Azure-laptops, is deze stap niet vereist.

1. Laden KQL Magic-pakket:

    ```python
    reload_ext Kqlmagic
    ```

## <a name="connect-to-the-azure-data-explorer-help-cluster"></a>Verbinding maken met het cluster Azure Data Explorer Help

Gebruik de volgende opdracht om te verbinden met de *voorbeelden* database die wordt gehost op de *Help* cluster. Niet - Microsoft AAD-gebruikers, vervangen door de naam van de tenant `Microsoft.com` met uw AAD-Tenant.

```python
%kql AzureDataExplorer://tenant="Microsoft.com";code;cluster='help';database='Samples'
```

## <a name="query-and-visualize"></a>Opvragen en visualiseren

Query-gegevens met de [renderen operator](/azure/kusto/query/renderoperator) en Visualiseer gegevens met behulp van de bibliotheek ploy.ly. Biedt een geïntegreerde ervaring die gebruikmaakt van systeemeigen KQL deze query en visualisatie. Kqlmagic ondersteunt de meeste grafieken, behalve `timepivot`, `pivotchart`, en `ladderchart`. Rendering wordt ondersteund met alle kenmerken met uitzondering van `kind`, `ysplit`, en `accumulate`. 

### <a name="query-and-render-piechart"></a>Query's en render piechart

```python
%%kql
StormEvents
| summarize statecount=count() by State
| sort by statecount 
| limit 10
| render piechart title="My Pie Chart by State"
```

### <a name="query-and-render-timechart"></a>Query's en render tijdgrafiek

```python
%%kql
StormEvents
| summarize count() by bin(StartTime,7d)
| render timechart
```

> [!NOTE]
> Deze diagrammen zijn interactief. Selecteer een tijdsbereik kunt inzoomen op in een bepaald tijdstip.

### <a name="customize-the-chart-colors"></a>Kleuren in de grafiek aanpassen

Als u niet het standaardkleurenpalet bevalt, aanpassen van de grafieken met palet opties. Beschikbare daarop vindt u hier: [Kleurenpalet voor uw KQL magic grafiek queryresultaat kiezen](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FColorYourCharts.ipynb)

1. Voor een lijst van palet:

    ```python
    %kql --palettes -popup_window
    ```

1. Selecteer de `cool` kleurenpalet en weer te geven van de query opnieuw uit:

    ```python
    %%kql -palette_name "cool"
    StormEvents
    | summarize statecount=count() by State
    | sort by statecount
    | limit 10
    | render piechart title="My Pie Chart by State"
    ```

## <a name="parameterize-a-query-with-python"></a>Voorzien van een query met Python

KQL magic kunt u eenvoudige uitwisseling tussen Kusto-querytaal en Python. Voor meer informatie: [Parameteriseren uw KQL magic query met Python](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FParametrizeYourQuery.ipynb)

### <a name="use-a-python-variable-in-your-kql-query"></a>Een Python-variabele gebruiken in uw query KQL

U kunt de waarde van een Python-variabele gebruiken in uw query om de gegevens te filteren:

```python
statefilter = ["TEXAS", "KANSAS"]
```

```python
%%kql
let _state = statefilter;
StormEvents 
| where State in (_state) 
| summarize statecount=count() by bin(StartTime,1d), State
| render timechart title = "Trend"
```

### <a name="convert-query-results-to-pandas-dataframe"></a>Queryresultaten converteren naar Pandas DataFrame

U kunt de resultaten van een query KQL in Pandas DataFrame openen. Toegang tot de laatst uitgevoerde queryresultaten door variabele `_kql_raw_result_` en converteer de resultaten in Pandas DataFrame als volgt:

```python
df = _kql_raw_result_.to_dataframe()
df.head(10)
```

### <a name="example"></a>Voorbeeld

In veel scenario's voor analyse, kunt u herbruikbare notitieblokken die veel query's bevatten en feed van de resultaten van een query in de volgende query's maken. Het volgende voorbeeld wordt de variabele Python `statefilter` om de gegevens te filteren.

1. Voer een query voor het weergeven van de top 10 Staten met hoogste `DamageProperty`:

    ```python
    %%kql
    StormEvents
    | summarize max(DamageProperty) by State
    | order by max_DamageProperty desc
    | limit 10
    ```

1. Voer een query voor het uitpakken van de status van de bovenste en stel deze in een Python-variabele:

    ```python
    df = _kql_raw_result_.to_dataframe()
    statefilter =df.loc[0].State
    statefilter
    ```

1. Voer een query met de `let` instructie en de Python-variabele:

    ```python
    %%kql
    let _state = statefilter;
    StormEvents 
    | where State in (_state)
    | summarize statecount=count() by bin(StartTime,1d), State
    | render timechart title = "Trend"
    ```

1. De help-opdracht uitvoeren:

    ```python
    %kql --help "help"
    ```

## <a name="next-steps"></a>Volgende stappen

Voer de opdracht help om te verkennen van de volgende voorbeeldnotitieblokken die alle functies van de ondersteunde bevatten:
- [Aan de slag met KQL magic voor Azure Data Explorer](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FQuickStart.ipynb) 
- [Aan de slag met KQL magic voor Application Insights](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FQuickStartAI.ipynb) 
- [Aan de slag met KQL magic voor Log Analytics](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FQuickStartLA.ipynb) 
- [Parametrize uw KQL magic-query met Python](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FParametrizeYourQuery.ipynb) 
- [Kleurenpalet voor uw KQL magic grafiek queryresultaat kiezen](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FColorYourCharts.ipynb)
