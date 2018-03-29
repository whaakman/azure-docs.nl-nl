---
title: Een fout simuleren bij het openen van redundante opslag met leestoegang in Azure | Microsoft Docs
description: Een fout simuleren bij het openen van geografisch redundante opslag met leestoegang
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.tgt_pltfrm: na
ms.devlang: ''
ms.topic: tutorial
ms.date: 12/23/2017
ms.author: tamram
ms.openlocfilehash: 57296d828156184aa36532cd649fbec0c81b5e27
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="simulate-a-failure-in-accessing-read-access-redundant-storage"></a>Een fout simuleren bij het openen van redundante opslag met leestoegang

Deze zelfstudie is deel twee van een serie.  In deze zelfstudie kunt u [Fiddler](#simulate-a-failure-with-fiddler) of [statische routering](#simulate-a-failure-with-an-invalid-static-route) gebruiken om een fout te simuleren voor aanvragen voor het primaire eindpunt van uw [geografisch redundante](../common/storage-redundancy.md#read-access-geo-redundant-storage) opslagaccount met leestoegang en de toepassing te laten lezen vanuit het secundaire eindpunt.

![Scenario-app](media/storage-simulate-failure-ragrs-account-app/scenario.png)

Voor het volgen van deze zelfstudie moet u de vorige zelfstudie over opslag: [Uw toepassingsgegevens maximaal beschikbaar maken met Azure Storage][previous-tutorial] hebben afgerond.

In deel twee van de serie leert u het volgende:

> [!div class="checklist"]
> * De toepassing uitvoeren en onderbreken
> * Een fout simuleren met [Fiddler](#simulate-a-failure-with-fiddler) of [een ongeldige statische route](#simulate-a-failure-with-an-invalid-static-route) 
> * Herstel van het primaire eindpunt simuleren


## <a name="prerequisites"></a>Vereisten

Een fout met Fiddler simuleren: 

* [Fiddler downloaden](https://www.telerik.com/download/fiddler) en installeren

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="simulate-a-failure-with-fiddler"></a>Een fout simuleren met Fiddler

Als u een fout wilt simuleren met Fiddler, voert u een mislukte reactie voor aanvragen aan het primaire eindpunt van uw geografisch redundante opslagaccount met leestoegang in om een fout te simuleren.

Voer de volgende stappen uit om een fout en herstel van het primaire eindpunt met Fiddler te simuleren.

### <a name="launch-fiddler"></a>Start Fiddler

Open Fiddler en selecteer **Rules** en **Customize Rules**.

![Fiddler-regels aanpassen](media/storage-simulate-failure-ragrs-account-app/figure1.png)

De Fiddler ScriptEditor wordt gestart en geeft het bestand **SampleRules.js** weer. Dit bestand wordt gebruikt om Fiddler aan te passen. Plak het volgende codevoorbeeld in de `OnBeforeResponse`-functie. De nieuwe code is als opmerking opgenomen om ervoor te zorgen dat de logica die deze maakt niet onmiddellijk wordt geïmplementeerd. Selecteer als u klaar bent **File** en **Save** om uw wijzigingen op te slaan.

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

### <a name="start-and-pause-the-application"></a>De toepassing starten en onderbreken

Voer de toepassing uit in uw IDE- of teksteditor. Wanneer de toepassing van het primaire eindpunt gaat lezen, drukt u op **een willekeurige toets** in het consolevenster om de toepassing te onderbreken.

### <a name="simulate-failure"></a>Fout simuleren

Als de toepassing is onderbroken, kunt u nu de aangepaste regel die in een voorgaande stap in Fiddler is opgeslagen als opmerking verwijderen. Het codevoorbeeld zoekt naar aanvragen voor het geografisch redundante opslagaccount met leestoegang en als het pad de naam van de installatiekopie, `HelloWorld`, bevat, wordt een antwoordcode van `503 - Service Unavailable` geretourneerd.

Navigeer naar Fiddler en selecteer **Rules** -> **Customize Rules...**.  Verwijder de volgende regels als opmerking en vervang `STORAGEACCOUNTNAME` door de naam van uw opslagaccount. Selecteer **File** -> **Save** om uw wijzigingen op te slaan. 

> [!NOTE]
> Als u de voorbeeldtoepassing op Linux uitvoert, moet u Fiddler telkens wanneer u het bestand **CustomRule.js** hebt bewerkt, opnieuw opstarten, zodat Fiddler de aangepaste logica installeert. 
> 
> 


```javascript
         if ((oSession.hostname == "STORAGEACCOUNTNAME.blob.core.windows.net")
         && (oSession.PathAndQuery.Contains("HelloWorld"))) {
         oSession.responseCode = 503;
         }
```

Druk op **een willekeurige toets** om de toepassing te hervatten.

Wanneer de toepassing weer wordt uitgevoerd, gaan de aanvragen naar het primaire eindpunt mislukken. De toepassing probeert 5 keer opnieuw verbinding te maken met het primaire eindpunt. Na de foutdrempelwaarde van vijf pogingen wordt de installatiekopie van het secundaire eindpunt met alleen leestoegang aangevraagd. Nadat de toepassing de installatiekopie 20 keer van het secundaire eindpunt heeft opgehaald, probeert de toepassing verbinding te maken met het primaire eindpunt. Als het primaire eindpunt nog steeds niet bereikbaar is, hervat de toepassing het lezen van het secundaire eindpunt. Dit patroon is het [circuitonderbreker](https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker)-patroon dat in de vorige zelfstudie is beschreven.

![De aangepaste regel plakken](media/storage-simulate-failure-ragrs-account-app/figure3.png)

### <a name="simulate-primary-endpoint-restoration"></a>Herstel van het primaire eindpunt simuleren

Met de aangepaste regelset van Fiddler in de vorige stap, mislukken aanvragen naar het primaire eindpunt. Om te simuleren dat het primaire eindpunt weer werkt, verwijdert u de logica om de fout `503` in te voeren.

Druk op **een willekeurige toets** om de toepassing te onderbreken.

Navigeer naar Fiddler en selecteer **Rules** en **Customize Rules...**.  Neem de aangepaste logica op als opmerking of verwijder de aangepaste logica in de functie `OnBeforeResponse`, en verlaat de standaardfunctie. Selecteer **File** en **Save** om de wijzigingen op te slaan.

![Aangepaste regel verwijderen](media/storage-simulate-failure-ragrs-account-app/figure5.png)

Druk als u klaar bent op **een willekeurige toets** om de toepassing te hervatten. De toepassing blijft lezen van het primaire eindpunt totdat de 999 leesbewerkingen zijn bereikt.

![Toepassing hervatten](media/storage-simulate-failure-ragrs-account-app/figure4.png)


## <a name="simulate-a-failure-with-an-invalid-static-route"></a>Een fout simuleren met een ongeldige statische route 
U kunt een ongeldige statische route maken voor alle aanvragen naar het primaire eindpunt van uw [geografisch redundante](../common/storage-redundancy.md#read-access-geo-redundant-storage) opslagaccount met leestoegang. In deze zelfstudie wordt de lokale host gebruikt als de gateway voor routeringsaanvragen voor de opslagaccount. Het gebruik van de lokale host als de gateway zorgt ervoor dat alle aanvragen naar het primaire eindpunt van uw opslagaccount in een lusvorm terugkeren naar de host, wat vervolgens tot een fout leidt. Voer de volgende stappen uit om een fout en herstel van het primaire eindpunt met een ongeldige statische route te simuleren. 

### <a name="start-and-pause-the-application"></a>De toepassing starten en onderbreken

Voer de toepassing uit in uw IDE- of teksteditor. Wanneer de toepassing van het primaire eindpunt gaat lezen, drukt u op **een willekeurige toets** in het consolevenster om de toepassing te onderbreken. 

### <a name="simulate-failure"></a>Fout simuleren

Start terwijl de toepassing is onderbroken de opdrachtprompt van Windows als een beheerder of voer terminal als root uit op Linux. Voor informatie over het primaire-eindpuntdomein van de opslagaccount voert u de volgende opdracht in bij een opdrachtprompt of terminal.

```
nslookup STORAGEACCOUNTNAME.blob.core.windows.net
``` 
 Vervang `STORAGEACCOUNTNAME` door de naam van uw opslagaccount. Kopieer het IP-adres van uw opslagaccount naar een teksteditor voor later gebruik. Als u het IP-adres van uw lokale host wilt ophalen, typt u `ipconfig` in de Windows-opdrachtprompt of `ifconfig` in de Linux-terminal. 

Als u een statische route voor de doelhost wilt toevoegen, typt u de volgende opdracht in een Windows-opdrachtprompt of Linux-terminal. 


# <a name="linuxtablinux"></a>[Linux](#tab/linux)

  route add <destination_ip> gw <gateway_ip>

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

  route add <destination_ip> <gateway_ip>

---
 
Vervang `<destination_ip>` door het IP-adres van uw opslagaccount en `<gateway_ip>` door het IP-adres van uw lokale host. Druk op **een willekeurige toets** om de toepassing te hervatten.

Wanneer de toepassing weer wordt uitgevoerd, gaan de aanvragen naar het primaire eindpunt mislukken. De toepassing probeert 5 keer opnieuw verbinding te maken met het primaire eindpunt. Na de foutdrempelwaarde van vijf pogingen wordt de installatiekopie van het secundaire eindpunt met alleen leestoegang aangevraagd. Nadat de toepassing de installatiekopie 20 keer van het secundaire eindpunt heeft opgehaald, probeert de toepassing verbinding te maken met het primaire eindpunt. Als het primaire eindpunt nog steeds niet bereikbaar is, hervat de toepassing het lezen van het secundaire eindpunt. Dit patroon is het [circuitonderbreker](/azure/architecture/patterns/circuit-breaker.md)-patroon dat in de vorige zelfstudie is beschreven.

### <a name="simulate-primary-endpoint-restoration"></a>Herstel van het primaire eindpunt simuleren

Om te simuleren dat het primaire eindpunt weer werkt, verwijdert u de statische route van het primaire eindpunt uit de routeringstabel. Hiermee worden alle aanvragen naar het primaire eindpunt via de standaardgateway gerouteerd. 

Als u de statische route van een doelhost wilt verwijderen, typt u de volgende opdracht in een Windows-opdrachtprompt of Linux-terminal. 
 
# <a name="linuxtablinux"></a>[Linux](#tab/linux)

route del <destination_ip> gw <gateway_ip>

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

route delete <destination_ip>

---

Druk op **een willekeurige toets** als u de toepassing wilt hervatten. De toepassing blijft lezen van het primaire eindpunt totdat de 999 leesbewerkingen zijn bereikt.

![Toepassing hervatten](media/storage-simulate-failure-ragrs-account-app/figure4.png)


## <a name="next-steps"></a>Volgende stappen

In deel twee van de serie hebt u geleerd over het simuleren van een fout om geografisch redundante opslag met leestoegang te testen, zoals:

> [!div class="checklist"]
> * De toepassing uitvoeren en onderbreken
> * Een fout simuleren met [Fiddler](#simulate-a-failure-with-fiddler) of [een ongeldige statische route](#simulate-a-failure-with-an-invalid-static-route) 
> * Herstel van het primaire eindpunt simuleren

Volg deze link om vooraf samengestelde opslagvoorbeelden te bekijken.

> [!div class="nextstepaction"]
> [Voorbeelden van Azure Storage-scripts](storage-samples-blobs-cli.md)

[previous-tutorial]: storage-create-geo-redundant-storage.md