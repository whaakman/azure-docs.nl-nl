---
title: 'Zelfstudie: Een fout simuleren bij het openen van redundante opslag met leestoegang in Azure | Microsoft Docs'
description: Een fout simuleren bij het openen van geografisch redundante opslag met leestoegang
services: storage
author: tamram
ms.service: storage
ms.topic: tutorial
ms.date: 01/03/2019
ms.author: tamram
ms.openlocfilehash: 0cbb4d2bc6449dc1cf12a374085b429743224995
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56872876"
---
# <a name="tutorial-simulate-a-failure-in-accessing-read-access-redundant-storage"></a>Zelfstudie: Een fout simuleren bij het openen van redundante opslag met leestoegang

Deze zelfstudie is deel twee van een serie. Er wordt uitgelegd wat de voordelen zijn van [geografisch redundante opslag met leestoegang](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (RA-GRS) door een fout te simuleren.

Voor het simuleren van een fout kunt u [Fiddler](#simulate-a-failure-with-fiddler) of [statische routering](#simulate-a-failure-with-an-invalid-static-route) gebruiken. In beide gevallen kunt u fouten simuleren voor aanvragen naar het primaire eindpunt van uw [geografisch redundante opslagaccount met leestoegang](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (RA-GRS) en de toepassing gegevens laten lezen van het secundaire eindpunt.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

In deel twee van de serie leert u het volgende:

> [!div class="checklist"]
> * De toepassing uitvoeren en onderbreken
> * Een fout simuleren met [Fiddler](#simulate-a-failure-with-fiddler) of [een ongeldige statische route](#simulate-a-failure-with-an-invalid-static-route) 
> * Herstel van het primaire eindpunt simuleren

## <a name="prerequisites"></a>Vereisten

Begin pas met deze zelfstudie nadat u de vorige zelfstudie hebt voltooid: [Uw toepassingsgegevens maximaal beschikbaar maken met Azure-opslag][previous-tutorial].

Een fout met Fiddler simuleren: 

* [Fiddler downloaden](https://www.telerik.com/download/fiddler) en installeren

## <a name="simulate-a-failure-with-fiddler"></a>Een fout simuleren met Fiddler

Als u een fout wilt simuleren met Fiddler, voert u een mislukte reactie voor aanvragen in op het primaire eindpunt van uw geografisch redundante opslagaccount met leestoegang.

In de volgende gedeelten wordt uitgelegd hoe u een fout simuleert en het primaire eindpunt herstelt met Fiddler.

### <a name="launch-fiddler"></a>Start Fiddler

Open Fiddler en selecteer **Rules** en **Customize Rules**.

![Fiddler-regels aanpassen](media/storage-simulate-failure-ragrs-account-app/figure1.png)

De Fiddler ScriptEditor wordt gestart en u ziet het bestand **SampleRules.js**. Dit bestand wordt gebruikt om Fiddler aan te passen.

Plak het volgende codevoorbeeld in de `OnBeforeResponse`-functie. De nieuwe code is als opmerking opgenomen om ervoor te zorgen dat de logica die deze maakt niet onmiddellijk wordt geïmplementeerd.

Selecteer als u klaar bent **File** en **Save** om uw wijzigingen op te slaan.

```javascript
    /*
        // Simulate data center failure
        // After it is successfully downloading the blob, pause the code in the sample,
        // uncomment these lines of script, and save the script.
        // It will intercept the (probably successful) responses and send back a 503 error. 
        // When you're ready to stop sending back errors, comment these lines of script out again 
        //     and save the changes.

        if ((oSession.hostname == "contosoragrs.blob.core.windows.net") 
            && (oSession.PathAndQuery.Contains("HelloWorld"))) {
            oSession.responseCode = 503;  
        }
    */
```

![De aangepaste regel plakken](media/storage-simulate-failure-ragrs-account-app/figure2.png)

### <a name="interrupting-the-application"></a>De toepassing onderbreken

# <a name="net-python-and-java-v7tabdotnet-python-java-v7"></a>[.NET, Python en Java versie 7](#tab/dotnet-python-java-v7)

Voer de toepassing uit in uw IDE of shell.

Wanneer de toepassing van het primaire eindpunt gaat lezen, drukt u op **een willekeurige toets** in het consolevenster om de toepassing te onderbreken.

![Scenario-app](media/storage-simulate-failure-ragrs-account-app/scenario.png)

# <a name="java-v10tabjava-v10"></a>[Java v10](#tab/Java-v10)

Voer de toepassing uit in uw IDE of shell.

Aangezien u het voorbeeld beheert, hoeft u geen onderbreking in te voegen om een fout te simuleren. Zorg ervoor dat het bestand is geüpload naar uw opslagaccount door het voorbeeld uit te voeren en **P** te gebruiken.

![Scenario-app](media/storage-simulate-failure-ragrs-account-app/Java-put-list-output.png)

---

### <a name="simulate-failure"></a>Fout simuleren

Terwijl de toepassing is onderbroken, verwijdert u de commentaartekens voor de aangepaste regel die we in Fiddler hebben opgeslagen.

Het codevoorbeeld zoekt naar aanvragen voor het geografisch redundante opslagaccount met leestoegang en als het pad de naam van het bestand bevat, `HelloWorld`, wordt de antwoordcode `503 - Service Unavailable` geretourneerd.

Navigeer naar Fiddler en selecteer **Rules** -> **Customize Rules...**.

Verwijder de commentaartekens voor de volgende regels en vervang `STORAGEACCOUNTNAME` door de naam van uw opslagaccount. Selecteer **File** -> **Save** om uw wijzigingen op te slaan. 

> [!NOTE]
> Als u de voorbeeldtoepassing op Linux uitvoert, moet u Fiddler telkens wanneer u het bestand **CustomRule.js** hebt bewerkt, opnieuw opstarten, zodat Fiddler de aangepaste logica installeert.

```javascript
         if ((oSession.hostname == "STORAGEACCOUNTNAME.blob.core.windows.net")
         && (oSession.PathAndQuery.Contains("HelloWorld"))) {
         oSession.responseCode = 503;
         }
```

# <a name="net-python-and-java-v7tabdotnet-python-java-v7"></a>[.NET, Python en Java versie 7](#tab/dotnet-python-java-v7)

Druk op **een willekeurige toets** om de toepassing te hervatten.

Wanneer de toepassing weer wordt uitgevoerd, gaan de aanvragen naar het primaire eindpunt mislukken. De toepassing probeert 5 keer opnieuw verbinding te maken met het primaire eindpunt. Na de foutdrempelwaarde van vijf pogingen wordt de installatiekopie van het secundaire eindpunt met alleen leestoegang aangevraagd. Als de toepassing de installatiekopie 20 keer van het secundaire eindpunt heeft opgehaald, probeert de toepassing verbinding te maken met het primaire eindpunt. Als het primaire eindpunt nog steeds niet bereikbaar is, hervat de toepassing het lezen van het secundaire eindpunt.

Dit patroon is het [circuitonderbreker](https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker)-patroon dat in de vorige zelfstudie is beschreven.

![De aangepaste regel plakken](media/storage-simulate-failure-ragrs-account-app/figure3.png)

# <a name="java-v10tabjava-v10"></a>[Java v10](#tab/Java-v10)

De fout is nu geïntroduceerd en dus voert u **G** uit om te testen op de fout.

Er verschijnt een bericht dat de secundaire pijplijn wordt gebruikt in plaats van de primaire pijplijn.

---

### <a name="simulate-primary-endpoint-restoration"></a>Herstel van het primaire eindpunt simuleren

# <a name="net-python-and-java-v7tabdotnet-python-java-v7"></a>[.NET, Python en Java versie 7](#tab/dotnet-python-java-v7)

Met de aangepaste regelset van Fiddler in de vorige stap, mislukken aanvragen naar het primaire eindpunt.

Om te simuleren dat het primaire eindpunt weer werkt, verwijdert u de logica om de fout `503` in te voeren.

Druk op **een willekeurige toets** om de toepassing te onderbreken.

Navigeer naar Fiddler en selecteer **Rules** en **Customize Rules...**. 

Neem de aangepaste logica op als opmerking of verwijder de aangepaste logica in de functie `OnBeforeResponse`, en verlaat de standaardfunctie.

Selecteer **File** en **Save** om de wijzigingen op te slaan.

![Aangepaste regel verwijderen](media/storage-simulate-failure-ragrs-account-app/figure5.png)

Druk als u klaar bent op **een willekeurige toets** om de toepassing te hervatten. De toepassing blijft lezen van het primaire eindpunt totdat de 999 leesbewerkingen zijn bereikt.

![Toepassing hervatten](media/storage-simulate-failure-ragrs-account-app/figure4.png)

# <a name="java-v10tabjava-v10"></a>[Java v10](#tab/Java-v10)

Met de aangepaste regelset van Fiddler in de vorige stap, mislukken aanvragen naar het primaire eindpunt.

Om te simuleren dat het primaire eindpunt weer werkt, verwijdert u de logica om de fout `503` in te voeren.

Navigeer naar Fiddler en selecteer **Rules** en **Customize Rules...**.  Neem de aangepaste logica op als opmerking of verwijder de aangepaste logica in de functie `OnBeforeResponse`, en verlaat de standaardfunctie.

Selecteer **File** en **Save** om de wijzigingen op te slaan.

Als u klaar bent, voert u **G** in om de download te testen. De toepassing meldt dat nu weer de primaire pijplijn wordt gebruikt.

---

## <a name="simulate-a-failure-with-an-invalid-static-route"></a>Een fout simuleren met een ongeldige statische route

U kunt een ongeldige statische route maken voor alle aanvragen naar het primaire eindpunt van uw [geografisch redundante](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) opslagaccount met leestoegang. In deze zelfstudie wordt de lokale host gebruikt als de gateway voor routeringsaanvragen voor de opslagaccount. Het gebruik van de lokale host als de gateway zorgt ervoor dat alle aanvragen naar het primaire eindpunt van uw opslagaccount in een lusvorm terugkeren naar de host, wat vervolgens tot een fout leidt. Voer de volgende stappen uit om een fout en herstel van het primaire eindpunt met een ongeldige statische route te simuleren. 

### <a name="start-and-pause-the-application"></a>De toepassing starten en onderbreken

# <a name="net-python-and-java-v7tabdotnet-python-java-v7"></a>[.NET, Python en Java versie 7](#tab/dotnet-python-java-v7)

Voer de toepassing uit in uw IDE of shell. Wanneer de toepassing van het primaire eindpunt gaat lezen, drukt u op **een willekeurige toets** in het consolevenster om de toepassing te onderbreken.

# <a name="java-v10tabjava-v10"></a>[Java v10](#tab/Java-v10)

Aangezien u het voorbeeld beheert, hoeft u geen onderbreking in te voegen om een fout te testen.

Zorg ervoor dat het bestand is geüpload naar uw opslagaccount door het voorbeeld uit te voeren en **P** te gebruiken.

---

### <a name="simulate-failure"></a>Fout simuleren

Start terwijl de toepassing is onderbroken de opdrachtprompt van Windows als een beheerder of voer terminal als root uit op Linux.

Vraag informatie op over het primaire-eindpuntdomein van het opslagaccount door de volgende opdracht in te voeren bij een opdrachtprompt of in een terminalvenster.

```
nslookup STORAGEACCOUNTNAME.blob.core.windows.net
``` 
 Vervang `STORAGEACCOUNTNAME` door de naam van uw opslagaccount. Kopieer het IP-adres van uw opslagaccount naar een teksteditor voor later gebruik.

Als u het IP-adres van uw lokale host wilt ophalen, typt u `ipconfig` in de Windows-opdrachtprompt of `ifconfig` in de Linux-terminal. 

Als u een statische route voor de doelhost wilt toevoegen, typt u de volgende opdracht in een Windows-opdrachtprompt of Linux-terminal. 

#### <a name="linux"></a>Linux

`route add <destination_ip> gw <gateway_ip>`

#### <a name="windows"></a>Windows

`route add <destination_ip> <gateway_ip>`

Vervang `<destination_ip>` door het IP-adres van uw opslagaccount en `<gateway_ip>` door het IP-adres van uw lokale host.

# <a name="net-python-and-java-v7tabdotnet-python-java-v7"></a>[.NET, Python en Java versie 7](#tab/dotnet-python-java-v7)

Druk op **een willekeurige toets** om de toepassing te hervatten.

Wanneer de toepassing weer wordt uitgevoerd, gaan de aanvragen naar het primaire eindpunt mislukken. De toepassing probeert vijf keer opnieuw verbinding te maken met het primaire eindpunt. Na de foutdrempelwaarde van vijf pogingen wordt de installatiekopie van het secundaire eindpunt met alleen leestoegang aangevraagd. Nadat de toepassing de installatiekopie 20 keer van het secundaire eindpunt heeft opgehaald, probeert de toepassing verbinding te maken met het primaire eindpunt. Als het primaire eindpunt nog steeds niet bereikbaar is, hervat de toepassing het lezen van het secundaire eindpunt. Dit patroon is het [circuitonderbreker](/azure/architecture/patterns/circuit-breaker)-patroon dat in de vorige zelfstudie is beschreven.

# <a name="java-v10tabjava-v10"></a>[Java v10](#tab/Java-v10)

De fout is nu geïntroduceerd en dus voert u **G** uit om te testen op de fout. Er verschijnt een bericht dat de secundaire pijplijn wordt gebruikt in plaats van de primaire pijplijn.

---

### <a name="simulate-primary-endpoint-restoration"></a>Herstel van het primaire eindpunt simuleren

Om te simuleren dat het primaire eindpunt weer werkt, verwijdert u de statische route van het primaire eindpunt uit de routeringstabel. Hiermee worden alle aanvragen naar het primaire eindpunt via de standaardgateway gerouteerd.

Als u de statische route van een doelhost wilt verwijderen, typt u de volgende opdracht in een Windows-opdrachtprompt of Linux-terminal.

#### <a name="linux"></a>Linux

`route del <destination_ip> gw <gateway_ip>`

#### <a name="windows"></a>Windows

`route delete <destination_ip>`

# <a name="net-python-and-java-v7tabdotnet-python-java-v7"></a>[.NET, Python en Java versie 7](#tab/dotnet-python-java-v7)

Druk op **een willekeurige toets** als u de toepassing wilt hervatten. De toepassing blijft lezen van het primaire eindpunt totdat de 999 leesbewerkingen zijn bereikt.

![Toepassing hervatten](media/storage-simulate-failure-ragrs-account-app/figure4.png)


# <a name="java-v10tabjava-v10"></a>[Java v10](#tab/Java-v10)

Voer **G** in om de download te testen. De toepassing meldt dat nu weer de primaire pijplijn wordt gebruikt.

![Toepassing hervatten](media/storage-simulate-failure-ragrs-account-app/java-get-pipeline-example-v10.png)

---

## <a name="next-steps"></a>Volgende stappen

In deel twee van de serie hebt u geleerd over het simuleren van een fout om geografisch redundante opslag met leestoegang te testen.

Lees het volgende artikel voor meer informatie over de werking van RA-GRS-opslag en de bijbehorende risico's:

> [!div class="nextstepaction"]
> [HA-apps met RA-GRS ontwerpen](../common/storage-designing-ha-apps-with-ragrs.md)

[previous-tutorial]: storage-create-geo-redundant-storage.md
