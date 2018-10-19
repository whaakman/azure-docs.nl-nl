---
title: Asynchrone vernieuwing voor Azure Analysis Services-modellen | Microsoft Docs
description: Leer hoe u asynchrone vernieuwing met behulp van REST-API-code.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: e30f02fb7f250b32cb3c7bb5205f58ca658a8f2a
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49426739"
---
# <a name="asynchronous-refresh-with-the-rest-api"></a>Asynchrone vernieuwing met de REST-API
Met behulp van elke programmeertaal die REST-aanroepen ondersteunt, kunt u gegevensvernieuwing asynchrone bewerkingen op uw modellen in tabelvorm Azure Analysis Services uitvoeren. Dit omvat de synchronisatie van alleen-lezen replica's voor query's worden uitgeschaald. 

Bewerkingen voor gegevensvernieuwing kunnen even duren, afhankelijk van een aantal factoren zoals gegevensvolume, niveau van optimalisatie met partities, enzovoort. Deze bewerkingen hebben traditiegetrouw is aangeroepen met bestaande methoden, zoals het gebruik van [TOM](https://docs.microsoft.com/sql/analysis-services/tabular-model-programming-compatibility-level-1200/introduction-to-the-tabular-object-model-tom-in-analysis-services-amo) (Tabular Object Model), [PowerShell](https://docs.microsoft.com/sql/analysis-services/powershell/analysis-services-powershell-reference) cmdlets, of [TMSL](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference) (Model in tabelvorm Scripting Language). Deze methoden kunnen echter vaak voor onbetrouwbare, langdurige HTTP-verbindingen vereisen.

De REST-API voor Azure Analysis Services kunt bewerkingen voor gegevensvernieuwing worden asynchroon uitgevoerd. Langdurige HTTP-verbindingen van clienttoepassingen zijn niet met behulp van de REST-API, die nodig zijn. Er zijn ook andere ingebouwde functies voor betrouwbaarheid, zoals automatische nieuwe pogingen en batch doorvoeringen.

## <a name="base-url"></a>Basis-URL

De basis-URL met de volgende notatie:

```
https://<rollout>.asazure.windows.net/servers/<serverName>/models/<resource>/
```

Neem bijvoorbeeld een model met de naam AdventureWorks op een server met de naam myserver, zich in de regio West ons Azure. De servernaam van de is:

```
asazure://westus.asazure.windows.net/myserver 
```

De basis-URL voor de naam van deze server is:

```
https://westus.asazure.windows.net/servers/myserver/models/AdventureWorks/ 
```

Met behulp van de basis-URL, kunnen resources en bewerkingen worden toegevoegd op basis van de volgende parameters: 

![Asynchrone vernieuwing](./media/analysis-services-async-refresh/aas-async-refresh-flow.png)

- Alles wat eindigt in **s** is een verzameling.
- Alles wat eindigt op **()** is een functie.
- Iets anders is een resource /-object.

Bijvoorbeeld, kunt u de POST-bewerking op de verzameling wordt vernieuwd een vernieuwingsbewerking uit te voeren:

```
https://westus.asazure.windows.net/servers/myserver/models/AdventureWorks/refreshes
```

## <a name="authentication"></a>Verificatie

Alle aanroepen met een geldig token voor Azure Active Directory (OAuth 2) in de autorisatie-header moeten worden geverifieerd en moeten voldoen aan de volgende vereisten:

- Het token moet een token of een service-principal van toepassing.
- Het token moet de juiste doelgroep ingesteld op `https://*.asazure.windows.net`.
- De gebruiker of toepassing moet voldoende machtigingen op de server of het model om de aangevraagde aanroep te maken hebben. Het machtigingsniveau wordt bepaald door rollen binnen het model of de groep admin op de server.

    > [!IMPORTANT]
    > Op dit moment **serverbeheerder** rolmachtigingen nodig zijn.

## <a name="post-refreshes"></a>POST /refreshes

Als u wilt vernieuwen uitvoeren, gebruikt u de POST-bewerking voor de verzameling /refreshes een nieuwe vernieuwing-item toevoegen aan de verzameling. De Location-header in het antwoord bevat de id vernieuwen. De clienttoepassing kunt verbreken en controleer de status later als vereist, omdat deze asynchroon is.

Van slechts één vernieuwingsbewerking wordt tegelijk voor een model geaccepteerd. Als er een huidige actieve vernieuwingsbewerking en andere wordt ingediend, wordt de 409 Conflict HTTP-statuscode geretourneerd.

De instantie lijkt mogelijk op het volgende:

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
Parameters op te geven is niet vereist. De standaardwaarde is toegepast.

|Naam  |Type  |Beschrijving  |Standaard  |
|---------|---------|---------|---------|
|Type     |  Enum       |  Het type verwerking moet worden uitgevoerd. De typen zijn uitgelijnd met de TMSL [opdracht Vernieuwen](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/refresh-command-tmsl) typen: volledige, clearValues, berekenen, dataOnly, automatische, en defragmenteren. Voeg type wordt niet ondersteund.      |   Automatisch      |
|CommitMode     |  Enum       |  Hiermee bepaalt u als objecten worden vastgelegd in batches of alleen als u klaar bent. Modi opnemen: standaard transactionele partialBatch.  |  transactionele       |
|MaxParallelism     |   Int      |  Deze waarde bepaalt het maximum aantal threads waarop om van verwerkingsopdrachten parallel uit te voeren. Deze waarde wordt uitgelijnd met de eigenschap MaxParallelism die kan worden ingesteld in de TMSL [opdracht sequentiëren](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/sequence-command-tmsl) of andere methoden gebruiken.       | 10        |
|RetryCount    |    Int     |   Geeft het aantal keren dat de bewerking opnieuw wordt geprobeerd voordat deze is mislukt.      |     0    |
|Objecten     |   Matrix      |   Een matrix met objecten moeten worden verwerkt. Elk object bevat: 'tabel' bij het verwerken van de hele tabel of 'tabel' en "partitie" bij het verwerken van een partitie. Als er geen objecten zijn opgegeven, wordt het hele model wordt vernieuwd. |   Het hele model verwerken      |

CommitMode is gelijk aan partialBatch. Deze wordt gebruikt wanneer een initiële laden van grote gegevenssets die hiervoor uren kan duren. Als de vernieuwingsbewerking mislukt na het doorvoeren van een of meer batches is, de doorgevoerd batches blijft toegewezen (dit wordt niet terugdraaien doorgevoerd batches).

> [!NOTE]
> Op het moment van schrijven, de batchgrootte van de is de waarde MaxParallelism, maar deze waarde kan worden gewijzigd.

## <a name="get-refreshesrefreshid"></a>GET-/refreshes/\<refreshId >

Om te controleren of de status van een vernieuwingsbewerking, gebruikt u de GET-bewerking op de id van de vernieuwing. Hier volgt een voorbeeld van de antwoordtekst. Als de bewerking uitgevoerd wordt, **inProgress** in de status wordt geretourneerd.

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

Als u een lijst met historische vernieuwingsbewerkingen voor een model, gebruikt u de GET-bewerking voor de verzameling /refreshes. Hier volgt een voorbeeld van de antwoordtekst. 

> [!NOTE]
> Op het moment van schrijven, de afgelopen 30 dagen van bewerkingen voor gegevensvernieuwing worden opgeslagen en die wordt geretourneerd, maar dit nummer is gewijzigd.

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

## <a name="delete-refreshesrefreshid"></a>VERWIJDEREN /refreshes/\<refreshId >

Als u wilt annuleren van een vernieuwingsbewerking wordt uitgevoerd, gebruikt u de DELETE-bewerking op de id van de vernieuwing.

## <a name="post-sync"></a>POST voorgrondsbeleidstoepassing

Bewerkingen voor gegevensvernieuwing te hebben uitgevoerd, kan het zijn die nodig zijn voor de nieuwe gegevens worden gesynchroniseerd met de replica's voor query's worden uitgeschaald. Een synchroniseren als bewerking wilt uitvoeren voor een model, de POST-bewerking op de functie voorgrondsbeleidstoepassing te gebruiken. De Location-header in het antwoord bevat de synchronisatie bewerking-ID.

## <a name="get-sync-status"></a>Status van voorgrondsbeleidstoepassing ophalen

Om te controleren of de status van een synchronisatiebewerking, gebruikt u de GET-verb de bewerkings-ID wordt doorgegeven als parameter. Hier volgt een voorbeeld van de antwoordtekst:

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

Waarden voor `syncstate`:

- 0: repliceren. Databasebestanden worden gerepliceerd naar een doelmap.
- 1: reactiveren. De database is wordt gereactiveerd op alleen-lezen-server-instantie (s).
- 2: voltooid. De synchronisatiebewerking is voltooid.
- 3: is mislukt. De synchronisatie is mislukt.
- 4: wordt voltooid. De synchronisatiebewerking is voltooid maar opschonen stappen uitvoert.

## <a name="code-sample"></a>Codevoorbeeld

Hier volgt een C#-codevoorbeeld om aan de slag gaat, [RestApiSample op GitHub](https://github.com/Microsoft/Analysis-Services/tree/master/RestApiSample).

### <a name="to-use-the-code-sample"></a>Gebruik de voorbeeldcode

1.  Klonen of downloaden van de opslagplaats. Open de oplossing RestApiSample.
2.  Zoek de regel **client. BaseAddress =...** en geef uw [basis-URL](#base-url).

De voorbeeldcode kunt interactieve aanmelding, gebruikersnaam en wachtwoord, of [service-principal](#service-principal).

#### <a name="interactive-login-or-usernamepassword"></a>Interactieve aanmelding of gebruikersnaam en wachtwoord

Deze vorm van verificatie vereist een Azure-toepassing worden gemaakt met de vereiste API-machtigingen toegewezen. 

1.  Klik in Azure portal op **een resource maken** > **Azure Active Directory** > **App-registraties**  >   **Nieuwe toepassing registreren**.

    ![Nieuwe toepassing registreren](./media/analysis-services-async-refresh/aas-async-app-reg.png)


2.  In **maken**, typ een naam en selecteer **systeemeigen** toepassingstype. Voor **omleidings-URI**, voer **urn: ietf:wg:oauth:2.0:oob**, en klik vervolgens op **maken**.

    ![Instellingen](./media/analysis-services-async-refresh/aas-async-app-reg-name.png)

3.  Selecteer uw app en kopieer en sla de **toepassings-ID**.

    ![Toepassings-ID kopiëren](./media/analysis-services-async-refresh/aas-async-app-id.png)

4.  In **instellingen**, klikt u op **vereiste machtigingen** > **toevoegen**.

    ![API-toegang toevoegen](./media/analysis-services-async-refresh/aas-async-add.png)

5.  In **Select an API**, type **Azure Analysis Services** in het zoekvak en selecteer vervolgens het.

    ![API selecteren](./media/analysis-services-async-refresh/aas-async-select-api.png)

6.  Selecteer **lezen en schrijven van alle modellen**, en klik vervolgens op **Selecteer**. Wanneer beide zijn ingeschakeld, klikt u op **gedaan** om toe te voegen van de machtigingen. Het duurt enkele minuten worden doorgegeven.

    ![Selecteer lezen en schrijven van alle modellen](./media/analysis-services-async-refresh/aas-async-select-read.png)

7.  Zoek in het codevoorbeeld wordt de **UpdateToken()** methode. Bekijk de inhoud van deze methode.
8.  Zoek **tekenreeks clientID =...** , en voer vervolgens de **toepassings-ID** u hebt gekopieerd in stap 3.
9.  Voet het voorbeeld uit.

#### <a name="service-principal"></a>Service-principal

Zie [service-principal - Azure portal maken](../azure-resource-manager/resource-group-create-service-principal-portal.md) en [een service-principal toevoegen aan de serverbeheerdersrol](analysis-services-addservprinc-admins.md) voor meer informatie over het instellen van een service-principal en de benodigde machtiging in Azure als toewijzen . Nadat u de stappen hebt gevolgd, moet u de volgende aanvullende stappen uitvoeren:

1.  Zoek in het codevoorbeeld **tekenreeks autoriteit =...** , Vervang **algemene** met uw organisatie tenant-ID.
2.  Opmerking/Verwijder de opmerking bij, zodat de ClientCredential-klasse wordt gebruikt om de Ref-object te maken. Zorg ervoor dat de \<App-ID > en \<App-sleutel > waarden in een veilige manier worden geopend of verificatie op basis van certificaten gebruikt voor service-principals.
3.  Voet het voorbeeld uit.


## <a name="see-also"></a>Zie ook

[Voorbeelden](analysis-services-samples.md)   
[REST API](https://docs.microsoft.com/rest/api/analysisservices/servers)   


