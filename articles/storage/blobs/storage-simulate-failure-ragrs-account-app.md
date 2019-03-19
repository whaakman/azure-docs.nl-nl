---
title: 'Zelfstudie: Een fout simuleren bij het openen van redundante opslag met leestoegang in Azure | Microsoft Docs'
description: Een fout simuleren bij het openen van geografisch redundante opslag met leestoegang
services: storage
author: tamram
ms.service: storage
ms.topic: tutorial
ms.date: 01/03/2019
ms.author: tamram
ms.openlocfilehash: 0144d68ecfdb1cc3309c462d00fa8f30e66bab34
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57441352"
---
# <a name="tutorial-simulate-a-failure-in-accessing-read-access-redundant-storage"></a>Zelfstudie: Een fout simuleren bij het openen van redundante opslag met leestoegang

Deze zelfstudie is deel twee van een serie. Er wordt uitgelegd wat de voordelen zijn van [geografisch redundante opslag met leestoegang](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (RA-GRS) door een fout te simuleren.

Als u wilt een fout simuleren, kunt u een [statische routering](#simulate-a-failure-with-an-invalid-static-route) of [Fiddler](#simulate-a-failure-with-fiddler). Beide methoden kunt u voor het simuleren van mislukte aanvragen naar het primaire eindpunt van uw [geo-redundante](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) opslagaccount (RA-GRS), waardoor de toepassing in plaats daarvan lezen van het secundaire eindpunt. 

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

In deel twee van de serie leert u het volgende:

> [!div class="checklist"]
> * De toepassing uitvoeren en onderbreken
> * Een fout simuleren met [een ongeldige statische route](#simulate-a-failure-with-an-invalid-static-route) of [Fiddler](#simulate-a-failure-with-fiddler) 
> * Herstel van het primaire eindpunt simuleren

## <a name="prerequisites"></a>Vereisten

Begin pas met deze zelfstudie nadat u de vorige zelfstudie hebt voltooid: [Uw toepassingsgegevens maximaal beschikbaar maken met Azure-opslag][previous-tutorial].

Als u wilt een fout simuleren met statische routering, gebruikt u een opdrachtprompt met verhoogde bevoegdheid.

Als u wilt een fout met Fiddler simuleren, downloaden en [Fiddler installeren](https://www.telerik.com/download/fiddler)

## <a name="simulate-a-failure-with-an-invalid-static-route"></a>Een fout simuleren met een ongeldige statische route

U kunt een ongeldige statische route maken voor alle aanvragen naar het primaire eindpunt van uw [geografisch redundante](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) opslagaccount met leestoegang. In deze zelfstudie wordt de lokale host gebruikt als de gateway voor routeringsaanvragen voor de opslagaccount. Het gebruik van de lokale host als de gateway zorgt ervoor dat alle aanvragen naar het primaire eindpunt van uw opslagaccount in een lusvorm terugkeren naar de host, wat vervolgens tot een fout leidt. Voer de volgende stappen uit om een fout en herstel van het primaire eindpunt met een ongeldige statische route te simuleren. 

### <a name="start-and-pause-the-application"></a>De toepassing starten en onderbreken

Volg de instructies in de [vorige zelfstudie] [ previous-tutorial] om te starten van het voorbeeld en downloaden van het testbestand bevestigen dat het afkomstig van de primaire opslag is. Afhankelijk van uw doelplatform, kunt u handmatig het voorbeeld onderbreken of wacht bij de opdrachtprompt. 

### <a name="simulate-failure"></a>Fout simuleren

Terwijl de toepassing wordt onderbroken, open een opdrachtprompt van Windows als een beheerder of voer terminal als root op Linux.

Informatie over de storage-account primaire-eindpuntdomein ophalen met de volgende opdracht op een opdracht vragen of terminal, vervangen `STORAGEACCOUNTNAME` met de naam van uw opslagaccount.

```
nslookup STORAGEACCOUNTNAME.blob.core.windows.net
``` 

Kopieer het IP-adres van uw opslagaccount naar een teksteditor voor later gebruik.

Als u het IP-adres van uw lokale host wilt ophalen, typt u `ipconfig` in de Windows-opdrachtprompt of `ifconfig` in de Linux-terminal. 

Als u wilt toevoegen een statische route voor de doelhost, typt u de volgende opdracht uit op een Windows-opdrachtprompt of Linux-terminal, vervangen `<destination_ip>` met het IP-adres van uw storage-account en `<gateway_ip>` met uw lokale host-IP-adres.

#### <a name="linux"></a>Linux

```
route add <destination_ip> gw <gateway_ip>
```

#### <a name="windows"></a>Windows

```
route add <destination_ip> <gateway_ip>
```

In het venster met het actieve voorbeeld hervat de toepassing of druk op de juiste sleutel te downloaden van het voorbeeldbestand en te bevestigen dat het afkomstig van de secundaire opslag is. U kunt vervolgens het voorbeeld opnieuw onderbreken of wacht bij de prompt. 

### <a name="simulate-primary-endpoint-restoration"></a>Herstel van het primaire eindpunt simuleren

Om te simuleren dat het primaire eindpunt weer functioneel is, de ongeldige statische route te verwijderen uit de routeringstabel. Hiermee worden alle aanvragen naar het primaire eindpunt via de standaardgateway gerouteerd. Typ de volgende opdracht op een Windows-opdrachtprompt of Linux-terminal.

#### <a name="linux"></a>Linux

```
route del <destination_ip> gw <gateway_ip>
```

#### <a name="windows"></a>Windows

```
route delete <destination_ip>
```

U kunt vervolgens de toepassing of druk op de juiste sleutel om het voorbeeld te downloaden bestand opnieuw, dit keer bevestigen dat het nog een keer afkomstig is van primaire opslag hervatten.

## <a name="simulate-a-failure-with-fiddler"></a>Een fout simuleren met Fiddler

Als u een fout wilt simuleren met Fiddler, voert u een mislukte reactie voor aanvragen in op het primaire eindpunt van uw geografisch redundante opslagaccount met leestoegang.

In de volgende gedeelten wordt uitgelegd hoe u een fout simuleert en het primaire eindpunt herstelt met Fiddler.

### <a name="launch-fiddler"></a>Start Fiddler

Open Fiddler en selecteer **Rules** en **Customize Rules**.

![Fiddler-regels aanpassen](media/storage-simulate-failure-ragrs-account-app/figure1.png)

De Fiddler ScriptEditor wordt gestart en u ziet het bestand **SampleRules.js**. Dit bestand wordt gebruikt om Fiddler aan te passen.

Plak het volgende codevoorbeeld in de `OnBeforeResponse` functioneren, vervangen `STORAGEACCOUNTNAME` met de naam van uw opslagaccount. Afhankelijk van het voorbeeld, u moet mogelijk ook vervangen `HelloWorld` met de naam van de testbestand (of een voorvoegsel zoals `sampleFile`) worden gedownload. De nieuwe code is opgenomen als opmerking om ervoor te zorgen dat deze onmiddellijk wordt niet uitgevoerd.

Selecteer als u klaar bent **File** en **Save** om uw wijzigingen op te slaan. Laat het ScriptEditor-venster geopend zodat u in de volgende stappen.

```javascript
    /*
        // Simulate data center failure
        // After it is successfully downloading the blob, pause the code in the sample,
        // uncomment these lines of script, and save the script.
        // It will intercept the (probably successful) responses and send back a 503 error. 
        // When you're ready to stop sending back errors, comment these lines of script out again 
        //     and save the changes.

        if ((oSession.hostname == "STORAGEACCOUNTNAME.blob.core.windows.net") 
            && (oSession.PathAndQuery.Contains("HelloWorld"))) {
            oSession.responseCode = 503;  
        }
    */
```

![De aangepaste regel plakken](media/storage-simulate-failure-ragrs-account-app/figure2.png)

### <a name="start-and-pause-the-application"></a>De toepassing starten en onderbreken

Volg de instructies in de [vorige zelfstudie] [ previous-tutorial] om te starten van het voorbeeld en downloaden van het testbestand bevestigen dat het afkomstig van de primaire opslag is. Afhankelijk van uw doelplatform, kunt u handmatig het voorbeeld onderbreken of wacht bij de opdrachtprompt. 

### <a name="simulate-failure"></a>Fout simuleren

Terwijl de toepassing wordt onderbroken, gaat u terug naar Fiddler en verwijder opmerkingen bij de aangepaste regel die u hebt opgeslagen in de `OnBeforeResponse` functie. Zorg ervoor dat u selecteert **bestand** en **opslaan** uw wijzigingen opslaat, zodat de regel worden pas van kracht. Deze code ziet er uit voor aanvragen voor het RA-GRS-opslagaccount en, als het pad de naam van het voorbeeldbestand bevat retourneert een Antwoordcode van `503 - Service Unavailable`.

In het venster met het actieve voorbeeld hervat de toepassing of druk op de juiste sleutel te downloaden van het voorbeeldbestand en te bevestigen dat het afkomstig van de secundaire opslag is. U kunt vervolgens het voorbeeld opnieuw onderbreken of wacht bij de prompt. 

### <a name="simulate-primary-endpoint-restoration"></a>Herstel van het primaire eindpunt simuleren

In Fiddler, verwijderen of opnieuw opmerkingen bij de aangepaste regel. Selecteer **bestand** en **opslaan** om te controleren of de regel wordt niet meer van kracht worden.

In het venster met het actieve voorbeeld hervat de toepassing of druk op de juiste sleutel te downloaden van het voorbeeldbestand en te bevestigen dat het afkomstig van primaire opslag opnieuw is. Vervolgens kunt u het voorbeeld afsluiten. 

## <a name="next-steps"></a>Volgende stappen

In deel twee van de serie hebt u geleerd over het simuleren van een fout om geografisch redundante opslag met leestoegang te testen.

Lees het volgende artikel voor meer informatie over de werking van RA-GRS-opslag en de bijbehorende risico's:

> [!div class="nextstepaction"]
> [HA-apps met RA-GRS ontwerpen](../common/storage-designing-ha-apps-with-ragrs.md)

[previous-tutorial]: storage-create-geo-redundant-storage.md
