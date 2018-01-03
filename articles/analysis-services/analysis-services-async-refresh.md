---
title: Asynchrone vernieuwen voor Azure Analysis Services-modellen | Microsoft Docs
description: Informatie over het vernieuwen van asynchrone code met behulp van REST-API.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 12/18/2017
ms.author: owend
ms.openlocfilehash: 06d807b83f700c675c6979998dd8f74372a4845f
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="asynchronous-refresh-with-the-rest-api"></a>Asynchrone vernieuwen met de REST-API
U kunt de gegevensvernieuwing asynchrone bewerkingen op uw Azure Analysis Services-modellen in tabelvorm uitvoeren met behulp van elke programmeertaal die REST-aanroepen ondersteunt. Dit omvat de synchronisatie van alleen-lezen-replica's voor query scale-out. 

Vernieuwen van gegevens kunnen even duren, afhankelijk van een aantal factoren, waaronder gegevensvolume van optimalisatie met partities, enzovoort. Deze bewerkingen hebben oudsher is aangeroepen met bestaande methoden zoals het gebruik van [aangepaste](https://docs.microsoft.com/sql/analysis-services/tabular-model-programming-compatibility-level-1200/introduction-to-the-tabular-object-model-tom-in-analysis-services-amo) (Object Model in tabelvorm), [PowerShell](https://docs.microsoft.com/sql/analysis-services/powershell/analysis-services-powershell-reference) -cmdlets of [TMSL](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference) (Model in tabelvorm Scripttaal). Deze methoden vereisen echter vaak onbetrouwbaar, langlopende HTTP-verbindingen.

De REST-API voor Azure Analysis Services kunt gegevens vernieuwen bewerkingen asynchroon worden uitgevoerd. Met behulp van de REST-API zijn langlopende HTTP-verbindingen van clienttoepassingen niet nodig. Er zijn ook andere ingebouwde functies voor de betrouwbaarheid, zoals automatische nieuwe pogingen en batch doorvoeracties.

## <a name="base-url"></a>Basis-URL

De basis-URL volgt deze indeling:

```
https://<rollout>.asazure.windows.net/servers/<serverName>/models/<resource>/
```

Neem bijvoorbeeld een model met de naam AdventureWorks, op een server met de naam MijnServer, zich in de regio West ons Azure de servernaam is:

```
asazure://westus.asazure.windows.net/myserver 
```

De basis-URL voor deze servernaam is:

```
https://westus.asazure.windows.net/servers/myserver/models/AdventureWorks/ 
```

Met behulp van de basis-URL, kunnen bronnen en bewerkingen worden toegevoegd op basis van de volgende: 

![Asynchrone vernieuwen](./media/analysis-services-async-refresh/aas-async-refresh-flow.png)

- Alles wat eindigt in **s** is een verzameling.
- Alles wat eindigt op **()** is een functie.
- Overige is een bronobject.

Bijvoorbeeld, kunt u de POST-bewerking voor de verzameling wordt vernieuwd een vernieuwingsbewerking als volgt uitvoeren:

```
https://westus.asazure.windows.net/servers/myserver/models/AdventureWorks/refreshes
```

## <a name="authentication"></a>Verificatie

Alle aanroepen moeten worden geverifieerd met een geldig Azure Active Directory (2 OAuth)-token in autorisatie-header en moeten voldoen aan de volgende vereisten:

- Het token moet een token of een service-principal van toepassing.
- De gebruiker of toepassing moet voldoende machtigingen op de server of het model voor het aangevraagde gesprek hebben. Het machtigingsniveau wordt bepaald door rollen binnen het model of de administratorgroep op de server.
- Het token moet hebben tot de juiste doelgroep ingesteld op `https://*.asazure.windows.net`.

## <a name="post-refreshes"></a>POST /refreshes

Als u wilt een vernieuwingsbewerking uitvoeren, gebruikt u de POST-bewerking voor de verzameling /refreshes een nieuw vernieuwen item toevoegen aan de verzameling. De locatie-header in het antwoord bevat de id van de vernieuwing. De clienttoepassing kunt verbreken en controleer de status later indien nodig omdat het asynchrone.

Slechts één vernieuwingsbewerking wordt geaccepteerd op een tijdstip voor een model. Als er een huidige actieve vernieuwingsbewerking en andere wordt verzonden, wordt de 409 Conflict HTTP-statuscode geretourneerd.

De hoofdtekst kan als volgt uit:

```
{
    "Type": "Full",
    "CommitMode": "transactional",
    "MaxParallelism": 2,
    "RetryCount": 2,
    "Objects": [
        {
            "table": "DimCustomer",
            "partition": "DimCustomer"
        },
        {
            "table": "DimDate"
        }
    ]
}
```

### <a name="parameters"></a>Parameters
Het opgeven van parameters is niet vereist. De standaardwaarde is toegepast.

|Naam  |Type  |Beschrijving  |Standaard  |
|---------|---------|---------|---------|
|Type     |  Enum       |  Het type verwerking moet worden uitgevoerd. De typen worden afgestemd op de TMSL [de opdracht refresh](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/refresh-command-tmsl) typen: volledige, clearValues, dataOnly, automatische berekening, toevoegen en defragmenteren.       |   Automatisch      |
|CommitMode     |  Enum       |  Hiermee wordt bepaald of de objecten worden doorgevoerd in batches of alleen als u klaar. Modi omvatten: standaard transactionele, partialBatch.  |  transactionele       |
|MaxParallelism     |   Int      |  Deze waarde bepaalt het maximum aantal threads waarop de verwerking van opdrachten parallel worden uitgevoerd. Dit uitgelijnd met de eigenschap MaxParallelism die kan worden ingesteld in de TMSL [opdracht sequentiëren](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/sequence-command-tmsl) of andere methoden gebruiken.       | 10        |
|retryCount    |    Int     |   Geeft het aantal keren dat de bewerking zal opnieuw worden geprobeerd voordat deze is mislukt.      |     0    |
|Objecten     |   Matrix      |   Een matrix met objecten die moeten worden verwerkt. Elk object bevat: "tabel" bij het verwerken van de gehele tabel of 'tabel' en 'partitie' bij het verwerken van een partitie. Als er geen objecten worden opgegeven, wordt het hele model wordt vernieuwd. |   Het gehele model verwerken      |

CommitMode is gelijk aan partialBatch. Deze wordt gebruikt wanneer een initiële laden van grote gegevenssets dat doet, kan uren duren. Als tijdens het vernieuwen mislukt nadat het vastleggen van een of meer batches is, de doorgevoerd batches blijft toegewezen (dit wordt niet terugdraaien doorgevoerd partijen).

> [!NOTE]
> De batchgrootte is de waarde MaxParallelism op het moment van schrijven, maar deze waarde is gewijzigd.

## <a name="get-refreshesrefreshid"></a>GET-/refreshes/\<refreshId >

Gebruiken om te controleren de status van een vernieuwingsbewerking, de term GET van de refresh-ID. Hier volgt een voorbeeld van de antwoordtekst. Als de bewerking uitgevoerd wordt, **inProgress** status wordt geretourneerd.

```
{
    "startTime": "2017-12-07T02:06:57.1838734Z",
    "endTime": "2017-12-07T02:07:00.4929675Z",
    "type": "full",
    "status": "succeeded",
    "currentRefreshType": "full",
    "objects": [
        {
            "table": "DimCustomer",
            "partition": "DimCustomer",
            "status": "succeeded"
        },
        {
            "table": "DimDate",
            "partition": "DimDate",
            "status": "succeeded"
        }
    ]
}
```

## <a name="get-refreshes"></a>/Refreshes ophalen

Als u een lijst met historische vernieuwingsbewerkingen voor een model, gebruikt u de term GET voor de verzameling /refreshes. Hier volgt een voorbeeld van de antwoordtekst. 

> [!NOTE]
> Op het moment van schrijven, de afgelopen 30 dagen van bewerkingen voor gegevensvernieuwing worden opgeslagen en wordt geretourneerd, maar dit nummer is gewijzigd.

```
[
    {
        "refreshId": "1344a272-7893-4afa-a4b3-3fb87222fdac",
        "startTime": "2017-12-09T01:58:04.76",
        "endTime": "2017-12-09T01:58:12.607",
        "status": "succeeded"
    },
    {
        "refreshId": "474fc5a0-3d69-4c5d-adb4-8a846fa5580b",
        "startTime": "2017-12-07T02:05:48.32",
        "endTime": "2017-12-07T02:05:54.913",
        "status": "succeeded"
    }
]
```

## <a name="delete-refreshesrefreshid"></a>DELETE /refreshes/\<refreshId >

Als u wilt een lopende vernieuwingsbewerking annuleren, gebruikt u de DELETE-bewerking van de refresh-ID.

## <a name="post-sync"></a>POST voorgrondsbeleidstoepassing

Vernieuwingsbewerkingen te hebben uitgevoerd, het mogelijk nodig om te synchroniseren van de nieuwe gegevens met replica's voor query scale-out. Een synchroniseren als bewerking wilt uitvoeren voor een model, de POST-bewerking op de functie voorgrondsbeleidstoepassing te gebruiken. De locatie-header in het antwoord bevat de id van de synchronisatie opnieuw.

## <a name="get-sync-status"></a>Voorgrondsbeleidstoepassing status ophalen

Als u wilt controleren van de status van een synchronisatiebewerking, gebruiken de term GET de bewerkings-ID wordt doorgegeven als parameter. Hier volgt een voorbeeld van de hoofdtekst van antwoord:

```
{
    "operationId": "cd5e16c6-6d4e-4347-86a0-762bdf5b4875",
    "database": "AdventureWorks2",
    "UpdatedAt": "2017-12-09T02:44:26.18",
    "StartedAt": "2017-12-09T02:44:20.743",
    "syncstate": 2,
    "details": null
}
```

Waarden voor de synchronisatiestatus:

- 0: Bezig met repliceren. Databasebestanden worden gerepliceerd naar een doelmap.
- 1: rehydrating. De database is wordt zijn gerehydrateerd op alleen-lezen-server-exemplaren.
- 2: voltooid. De synchronisatiebewerking is voltooid.
- 3: is mislukt. De synchronisatie is mislukt.
- 4: voltooien. De synchronisatiebewerking is voltooid, maar is opschonen stappen uit te voeren.

## <a name="code-sample"></a>Codevoorbeeld

Hier volgt een C#-codevoorbeeld weer om u gestart, [RestApiSample op GitHub](https://github.com/Microsoft/Analysis-Services/tree/master/RestApiSample).

### <a name="to-use-the-code-sample"></a>Gebruik het voorbeeld van code

1.  Klonen of downloaden van de opslagplaats. Open de RestApiSample-oplossing.
2.  Zoek de regel **client. BaseAddress =...** en geef uw [basis-URL](#base-url).

De voorbeeldcode kunt interactieve aanmelding, gebruikersnaam en wachtwoord, of [service-principal](#service-principle).

#### <a name="interactive-login-or-usernamepassword"></a>Interactieve aanmelding of gebruikersnaam en wachtwoord

Deze vorm van verificatie vereist een Azure-toepassing worden gemaakt met de benodigde API-machtigingen toegewezen. 

1.  Klik in de Azure-portal op **nieuw** > **Azure Active Directory** > **App registraties** > **nieuw registratie van toepassing**.

    ![Nieuwe toepassing registreren](./media/analysis-services-async-refresh/aas-async-app-reg.png)


2.  In **maken**, typ een naam, selecteer **systeemeigen** toepassingstype. Voor **omleidings-URI**, voer **urn: ietf:wg:oauth:2.0:oob**, en klik vervolgens op **maken**.

    ![Instellingen](./media/analysis-services-async-refresh/aas-async-app-reg-name.png)

3.  Uw app selecteren en kopieert en sla de **toepassings-ID**.

    ![Kopieer de toepassings-ID](./media/analysis-services-async-refresh/aas-async-app-id.png)

4.  In **instellingen**, klikt u op **vereist machtigingen** > **toevoegen**.

    ![API-toegang toevoegen](./media/analysis-services-async-refresh/aas-async-add.png)

5.  In **selecteert u een API**, type **SQL Server Analysis Services** in het zoekvak en selecteer vervolgens **Azure Analysis Services (SQL Server Analysis Services Azure)**.

    ![API selecteren](./media/analysis-services-async-refresh/aas-async-select-api.png)

6.  Selecteer **lezen en schrijven van alle modellen**, en klik vervolgens op **Selecteer**. Wanneer beide zijn ingeschakeld, klikt u op **gedaan** de machtigingen toevoegen. Het duurt enkele minuten worden doorgegeven.

    ![Selecteer lezen en schrijven van alle modellen](./media/analysis-services-async-refresh/aas-async-select-read.png)

7.  Zoek in het voorbeeld van code de **UpdateToken()** methode. Houd rekening met de inhoud van deze methode.
8.  Zoeken naar **clientID string =...** , en voer vervolgens de **toepassings-ID** u in stap 3 hebt gekopieerd.
9.  Voet het voorbeeld uit.

#### <a name="service-principal"></a>Service-principal

Zie de [automatisering van Azure Analysis Services met Service-Principals en PowerShell](https://azure.microsoft.com/blog/automation-of-azure-analysis-services-with-service-principals-and-powershell/) blogbericht voor het instellen van een service-principal en toewijzen van de benodigde machtiging in Azure Analysis Services. Nadat u de stappen uiteengezet in de blogbericht hebt voltooid, kunt u de volgende aanvullende stappen uitvoeren:

1.  Zoek in het codevoorbeeld **string autoriteit =...** , Vervang **algemene** met uw organisatie tenant-ID.
2.  Opmerking/Opmerking verwijderen zodat het ClientCredential-klasse wordt gebruikt voor het exemplaar van de Ref-object maken. Zorg ervoor dat de \<App-ID > en \<App-sleutel > waarden in een veilige manier worden geopend of verificatie op basis van certificaten gebruiken voor service-principals.
3.  Voet het voorbeeld uit.


## <a name="see-also"></a>Zie ook

[Voorbeelden](analysis-services-samples.md)   
[REST API](https://docs.microsoft.com/rest/api/analysisservices/servers)   


