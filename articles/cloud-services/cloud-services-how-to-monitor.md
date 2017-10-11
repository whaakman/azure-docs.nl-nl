---
title: Het bewaken van een service in de cloud | Microsoft Docs
description: Informatie over het bewaken van cloud-services met behulp van de klassieke Azure portal.
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: 5c48d2fb-b8ea-420f-80df-7aebe2b66b1b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2015
ms.author: adegeo
ms.openlocfilehash: c369b22cf068a473343b006eb1b06fdd350d31db
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-monitor-cloud-services"></a>Cloud Services bewaken
[!INCLUDE [disclaimer](../../includes/disclaimer.md)]

U kunt bewaken `key` maatstaven voor prestaties voor uw cloudservices in de klassieke Azure portal. U kunt het niveau van bewaking aan de minimale en uitgebreide voor elke rol service instellen en de bewaking wordt weergegeven kunt aanpassen. Uitgebreide controle gegevens worden opgeslagen in een opslagaccount dat u toegang hebt tot buiten de portal. 

Controle worden weergegeven in de klassieke Azure portal zijn configureerbaar. U kunt de metrische gegevens die u bewaken in de lijst met metrische gegevens wilt op de **Monitor** , en u kunt kiezen welke metrische gegevens moeten worden getekend in metrics grafieken op de **Monitor** pagina en het dashboard. 

## <a name="concepts"></a>Concepten
Standaard is minimale bewaking opgegeven voor een nieuwe cloudservice met behulp van prestatiemeteritems die afkomstig zijn van het besturingssysteem van de host voor de functies exemplaren (virtuele machines). De minimale metrische gegevens zijn beperkt tot de CPU-Percentage, Data, gegevensuitvoer, doorvoercapaciteit van de schijf lezen en schrijven doorvoercapaciteit van de schijf. Uitgebreide bewaking configureert, kunt u aanvullende gegevens op basis van prestatiegegevens binnen de virtuele machines (rolexemplaren) ontvangen. De uitgebreide metrische gegevens inschakelen dichter analyse van problemen die tijdens de bewerkingen van de toepassing optreden.

Standaard wordt prestatiemeteritemgegevens van rolinstanties door actieve en overgebracht van de rolinstantie om de 3 minuten. Wanneer u uitgebreide bewaking inschakelt, wordt de onbewerkte gegevens van prestatiemeteritems voor elk rolexemplaar en over rolexemplaren voor elke rol geaggregeerd met een interval van 5 minuten, 1 uur en 12 uur. De verzamelde gegevens worden verwijderd na 10 dagen.

Nadat u uitgebreide bewaking ingeschakeld, wordt de geaggregeerde bewakingsgegevens opgeslagen in de tabellen in uw opslagaccount. Als u wilt inschakelen voor uitgebreide bewaking voor een rol, moet u een verbindingsreeks diagnostische gegevens die is gekoppeld aan het storage-account configureren. U kunt verschillende storage-accounts gebruiken voor andere rollen.

Inschakelen van uitgebreide bewaking toeneemt gerelateerde uw opslagkosten voor opslag van gegevens, gegevensoverdracht en opslag transacties. Minimale bewaking is niet vereist voor een opslagaccount. De gegevens voor de metrische gegevens die worden weergegeven op het niveau van de minimale controle worden niet opgeslagen in uw opslagaccount, zelfs als u de bewaking niveau op uitgebreide instellen.

## <a name="how-to-configure-monitoring-for-cloud-services"></a>Procedure: het bewaken configureren voor cloud-services
Gebruik de volgende procedures voor het configureren van uitgebreide of minimale bewaken in de klassieke Azure portal. 

### <a name="before-you-begin"></a>Voordat u begint
* Maak een *klassieke* storage-account voor het opslaan van de bewakingsgegevens. U kunt verschillende storage-accounts gebruiken voor andere rollen. Zie voor meer informatie [het maken van een opslagaccount](../storage/common/storage-create-storage-account.md#create-a-storage-account).
* Schakel diagnostische Azure-gegevens voor rollen van uw cloudservice. Zie [diagnostische gegevens configureren voor Cloudservices](cloud-services-dotnet-diagnostics.md).

Zorg ervoor dat de verbindingsreeks van diagnostische gegevens aanwezig in de configuratie van de functie is. U inschakelen uitgebreide bewaking totdat u Azure diagnostische gegevens inschakelen en een verbindingsreeks diagnostische gegevens in de configuratie van de functie opnemen niet.   

> [!NOTE]
> Projecten die gericht is op Azure SDK 2.5 heeft de verbindingsreeks van diagnostische gegevens niet automatisch opgenomen in de projectsjabloon. Voor deze projecten moet u de verbindingsreeks van diagnostische gegevens handmatig toevoegen aan de configuratie van de functie.
> 
> 

**Verbindingsreeks diagnostische gegevens handmatig toevoegen aan rollen configureren**

1. Open het Cloudservice-project in Visual Studio
2. Dubbelklik op de **rol** openen van de rol ontwerpen en selecteer de **instellingen** tabblad
3. Zoek naar een instelling met de naam **Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString**. 
4. Als deze instelling niet aanwezig is is, klikt u op de **instelling toevoegen** knop toe te voegen aan de configuratie en het wijzigen van het type voor de nieuwe instellingen worden **ConnectionString**
5. Stel de waarde voor de verbindingsreeks de door te klikken op de **...**  knop. Hiermee opent u een dialoogvenster weergegeven zodat u een opslagaccount selecteren.
   
    ![Visual Studio-instellingen](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioDiagnosticsConnectionString.png)

### <a name="to-change-the-monitoring-level-to-verbose-or-minimal"></a>Wijzigen van het niveau van de bewaking in uitgebreide of minimale
1. In de [klassieke Azure-portal](https://manage.windowsazure.com/), open de **configureren** pagina voor de cloud service-implementatie.
2. In **niveau**, klikt u op **uitgebreid** of **minimale**. 
3. Klik op **Opslaan**.

Nadat u ingeschakeld verbose bewaking, moet u eerst de bewakingsgegevens in de klassieke Azure portal ziet binnen het uur.

De onbewerkte gegevens van prestatiemeteritems en geaggregeerde bewakingsgegevens worden opgeslagen in het opslagaccount in tabellen gekwalificeerd door de implementatie-ID voor de rollen. 

## <a name="how-to-receive-alerts-for-cloud-service-metrics"></a>Procedure: waarschuwingen voor cloud service metrische gegevens ontvangen
U kunt waarschuwingen op basis van de cloudservice bewaken metrische gegevens kunt ontvangen. Op de **beheerservices** pagina van de klassieke Azure portal, kunt u een regel voor het activeren van een waarschuwing wanneer de metrische gegevens die u kiest een waarde die u opgeeft bereikt. U kunt ook e-mailbericht verzonden wanneer de waarschuwing wordt geactiveerd. Zie voor meer informatie [hoe: waarschuwingsmeldingen ontvangen en regels voor waarschuwingen beheren in Azure](http://go.microsoft.com/fwlink/?LinkId=309356).

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Hoe: metrische gegevens toevoegen aan de tabel metrische gegevens
1. In de [klassieke Azure-portal](http://manage.windowsazure.com/), open de **Monitor** pagina voor de cloudservice.
   
    Standaard worden in de tabel metrische gegevens een subset van de beschikbare metrische gegevens bevat. De afbeelding ziet u de standaard uitgebreide metrische gegevens voor een cloudservice, die beperkt tot het prestatiemeteritem Geheugen\Beschikbare megabytes (MB) met gegevens geaggregeerd op het rolniveau van de is. Gebruik **toevoegen metrische gegevens** aanvullende statistische en quotumwaarde op gegevens controleren in de klassieke Azure portal te selecteren.
   
    ![Uitgebreide weergave](./media/cloud-services-how-to-monitor/CloudServices_DefaultVerboseDisplay.png)
2. Metrische gegevens toevoegen aan de tabel metrische gegevens:
   
   1. Klik op **toevoegen metrische gegevens** openen **kiezen metrische gegevens**, u hieronder kunt zien.
      
       De eerste beschikbare metriek uitgevouwen zodat de opties die beschikbaar zijn. De eerste optie bevat voor elke metriek geaggregeerde bewakingsgegevens voor alle functies. Bovendien kunt u afzonderlijke rollen om gegevens weer te kiezen.
      
       ![Metrische gegevens toevoegen](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics.png)
   2. Selecteer de metrische gegevens om weer te geven
      
      * Klik op de pijl-omlaag door de metriek uit te breiden, de controle-opties.
      * Schakel het selectievakje in voor elke controleoptie die u wilt weergeven.
        
        U kunt maximaal 50 metrische gegevens weergeven in de tabel metrische gegevens.
        
        > [!TIP]
        > In uitgebreide bewaking, wordt de tientallen metrische gegevens in de lijst van de metrische gegevens kan bevatten. Om weer te geven van een schuifbalk, houd de muis boven aan de rechterkant van het dialoogvenster. Klik op het zoekpictogram om te filteren in de lijst, en voer tekst in het zoekvak, zoals hieronder wordt weergegeven.
        > 
        > 
        
        ![Toevoegen van metrische gegevens zoeken](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics_Search.png)
3. Nadat u metrische gegevens te selecteren, klik op OK (vinkje).
   
    De geselecteerde metrische gegevens worden toegevoegd aan de tabel metrische gegevens, zoals hieronder wordt weergegeven.
   
    ![monitor metrische gegevens](./media/cloud-services-how-to-monitor/CloudServices_Monitor_UpdatedMetrics.png)
4. Als u wilt een metriek verwijderen uit de tabel metrische gegevens, klikt u op de metriek selecteren en klik vervolgens op **metriek verwijderen**. (Alleen zien **metriek verwijderen** wanneer er een geselecteerde waarde.)

### <a name="to-add-custom-metrics-to-the-metrics-table"></a>Aangepaste metrische gegevens toevoegen aan de tabel metrische gegevens
De **uitgebreid** bewaking niveau voorziet in een lijst met standaard metrische gegevens die u kunt controleren op de portal. Naast deze informatie kunt u een aangepaste metrische gegevens of de prestatiemeteritems die zijn gedefinieerd door uw toepassing via de portal bewaken.

De volgende stappen wordt ervan uitgegaan dat u hebt ingeschakeld **uitgebreid** niveau bewaking en uw toepassing voor het verzamelen en aangepaste prestatiemeteritems transfer hebt geconfigureerd. 

Als u wilt weergeven van de aangepaste prestatiemeteritems in de portal moet u de configuratie in af-besturingselement-container bijwerken:

1. Open de blob af-besturingselement-container in uw opslagaccount voor diagnostische gegevens. U kunt Visual Studio of andere Opslagverkenner gebruiken om dit te doen.
   
    ![Visual Studio Server Explorer](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioBlobExplorer.png)
2. De blobpad met behulp van het patroon gaat **RoleName/DeploymentId/RoleInstance** vinden van de configuratie voor uw rolexemplaar. 
   
    ![Visual Studio Opslagverkenner](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioStorage.png)
3. Het configuratiebestand voor de rolinstantie downloaden en werk deze zodanig dat eventuele aangepaste prestatiemeteritems. Zo moeten worden bewaakt *schijf geschreven Bytes per seconde* voor de *station C* Voeg de volgende onder **PerformanceCounters\Subscriptions** knooppunt
   
    ```xml
    <PerformanceCounterConfiguration>
    <CounterSpecifier>\LogicalDisk(C:)\Disk Write Bytes/sec</CounterSpecifier>
    <SampleRateInSeconds>180</SampleRateInSeconds>
    </PerformanceCounterConfiguration>
    ```
4. De wijzigingen opslaan en upload het configuratiebestand terug naar dezelfde locatie als het bestaande bestand in de blob wordt overschreven.
5. Schakelen naar de uitgebreide modus in de configuratie voor Azure classic portal. Als u al in de uitgebreide modus zou moet u minimaal en terug naar de uitgebreide in-of uitschakelen.
6. Het aangepaste prestatiemeteritem is nu beschikbaar in de **toevoegen metrische gegevens** in het dialoogvenster. 

## <a name="how-to-customize-the-metrics-chart"></a>Procedure: de metrische gegevens grafiek aanpassen
1. Selecteer maximaal 6 metrische gegevens om te tekenen op de grafiek metrische gegevens in de tabel metrische gegevens. Als u wilt een metriek selecteert, klikt u op het selectievakje aan de linkerkant. Als u wilt verwijderen een waarde van de grafiek metrische gegevens, schakel het selectievakje in de tabel metrische gegevens.
   
    Als u metrische gegevens in de tabel metrische gegevens selecteert, worden de metrische gegevens worden toegevoegd aan de grafiek metrische gegevens. Op een beperkte weergave een **n meer** vervolgkeuzelijst bevat metrische headers die de weergave niet past.
2. Selecteer voor schakelen tussen het weergeven van relatieve waarden (laatste waarde alleen voor elke metriek) en absolute waarden (Y-as weergegeven), relatief of absoluut boven aan de grafiek.
   
    ![Relatief of absoluut](./media/cloud-services-how-to-monitor/CloudServices_Monitor_RelativeAbsolute.png)
3. Als u wilt wijzigen van het tijdsbereik selecteren de metrische gegevens grafiek wordt weergegeven, 1 uur, 24 uur of zeven dagen aan de bovenkant van de grafiek.
   
    ![Weergaveperiode van de monitor](./media/cloud-services-how-to-monitor/CloudServices_Monitor_DisplayPeriod.png)
   
    In het diagram van de metrische gegevens dashboard verschilt de methode voor het uitzetten van de metrische gegevens. Een standaardset metrische gegevens beschikbaar is en metrische gegevens worden toegevoegd of verwijderd door het selecteren van de metrische header.

### <a name="to-customize-the-metrics-chart-on-the-dashboard"></a>De grafiek metrische gegevens op het dashboard aanpassen
1. Open het dashboard voor de cloudservice.
2. Toevoegen of verwijderen van metrische gegevens van de grafiek:
   
   * Om een nieuwe waarde getekend, selecteer het selectievakje in voor de metrische gegevens in de grafiek headers. Klik op een beperkte weergave op de pijl-omlaag door  ***n* veldnamenrij? metrische gegevens** om te tekenen van een waarde die het grafiekgebied van de header kan niet worden weergegeven.
   * Schakel het selectievakje door de header voor het verwijderen van een waarde die in de grafiek wordt getekend.
   
3. Schakelen tussen **relatieve** en **Absolute** wordt weergegeven.
4. Kies 1 uur, 24 uur of zeven dagen aan gegevens om weer te geven.

## <a name="how-to-access-verbose-monitoring-data-outside-the-azure-classic-portal"></a>Procedure: toegang uitgebreide bewaking gegevens buiten de klassieke Azure portal
Uitgebreide bewakingsgegevens wordt opgeslagen in de tabellen in de storage-accounts die u voor elke rol opgeeft. Zes tabellen worden voor elke cloud service-implementatie gemaakt voor de rol. Twee tabellen worden gemaakt voor elk (5 minuten, 1 uur en 12 uur). Een van deze tabellen slaat quotumwaarde op aggregaties; de andere tabel bevat aggregaties voor rolinstanties. 

De tabelnamen van de hebben de volgende notatie:

```
WAD*deploymentID*PT*aggregation_interval*[R|RI]Table
```

Waarbij:

* *deploymentID* is de GUID die is toegewezen aan de cloud service-implementatie
* *aggregation_interval* = 5 M, 1U of 12 uur
* niveau van de rol aggregaties = R
* aggregaties voor rolinstanties k =

De volgende tabellen bewaren bijvoorbeeld uitgebreide bewakingsgegevens geaggregeerd interval van 1 uur:

```
WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRTable (hourly aggregations for the role)

WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRITable (hourly aggregations for role instances)
```
