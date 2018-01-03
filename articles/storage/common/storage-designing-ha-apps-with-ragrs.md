---
title: Het ontwerpen van maximaal beschikbare toepassingen met behulp van Azure leestoegang geografisch redundante opslag (RA-GRS) | Microsoft Docs
description: Klik hier voor meer informatie over het RA-GRS van Azure storage gebruiken voor het bouwen van een maximaal beschikbare toepassing flexibel genoeg is voor het afhandelen van storingen.
services: storage
documentationcenter: .net
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 8f040b0f-8926-4831-ac07-79f646f31926
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/11/2017
ms.author: tamram
ms.openlocfilehash: fe7c6d1f2530b43ac7b10c5b6b0723452452a97a
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="designing-highly-available-applications-using-ra-grs"></a>Maximaal beschikbare toepassingen met behulp van RA-GRS ontwerpen

Een algemene functie van de cloud-gebaseerde infrastructuur zoals Azure Storage is dat ze een maximaal beschikbare platform voor het hosten van toepassingen. Ontwikkelaars van cloud-gebaseerde toepassingen Overweeg zorgvuldig hoe u dit platform voor het leveren van maximaal beschikbare toepassingen voor hun gebruikers. Dit artikel is gericht op hoe ontwikkelaars geografisch redundante opslag met leestoegang (RA-GRS kunt) om ervoor te zorgen dat hun toepassingen met Azure Storage maximaal beschikbaar zijn.

Azure Storage biedt vier opties voor de redundantie voor gegevens in uw opslagaccount:

- LRS (lokaal redundante opslag)
- ZRS (Zone-redundante opslag) 
- GRS (geografisch redundante opslag)
- RA-GRS (geografisch redundante opslag met leestoegang). 

Dit artikel is gericht op GRS en RA-GRS. Met GRS worden drie kopieën van uw gegevens worden opgeslagen in de primaire regio die u hebt geselecteerd bij het instellen van het opslagaccount. Drie extra kopieën worden asynchroon worden bijgehouden in een secundaire regio die is opgegeven door Azure. RA-GRS is hetzelfde als GRS, behalve dat u beschikken over leestoegang tot de secundaire kopie. Zie voor meer informatie over de verschillende opties voor Azure Storage-redundantie [Azure Storage-replicatie](https://docs.microsoft.com/azure/storage/storage-redundancy). De replicatie-artikel ziet ook de koppelingen tussen de primaire en secundaire regio's.

Er zijn codefragmenten die zijn opgenomen in dit artikel en een koppeling naar een compleet codevoorbeeld aan het einde die u kunt downloaden en uitvoeren.

## <a name="key-features-of-ra-grs"></a>Belangrijke functies van RA-GRS

Houd er rekening mee deze sleutel verwijst bij het ontwerpen van uw toepassing voor RA-GRS:

* Azure-opslag onderhoudt een alleen-lezen kopie van de gegevens die u in de primaire regio in een secundaire regio opslaat. Zoals eerder vermeld, bepaalt de storage-service de locatie van de secundaire regio.

* De alleen-lezen kopie is [uiteindelijk consistent](https://en.wikipedia.org/wiki/Eventual_consistency) met de gegevens in de primaire regio.

* Voor blobs, tabellen en wachtrijen, kunt u een query de secundaire regio voor een *tijd van laatste synchronisatie* waarde die aangeeft wanneer de laatste replicatie vanaf de primaire naar de secundaire regio is opgetreden. (Dit wordt niet ondersteund voor Azure-bestanden, wat geen RA-GRS redundantie op dit moment.)

* U kunt de Storage-clientbibliotheek gebruiken om te communiceren met de gegevens in de primaire of secundaire regio. U kunt ook omleiden schijfleesaanvragen automatisch naar de secundaire regio als een aanvraag voor leestoegang tot de primaire regio een optreedt time-out.

* Als er een grote probleem met betrekking tot de toegankelijkheid van de gegevens in de primaire regio, kan het team van Azure een geo-failover, waarna de DNS-vermeldingen die verwijst naar de primaire regio worden gewijzigd om te verwijzen naar de secundaire regio activeren.

* Als een geo-failover optreedt, wordt Azure selecteert u een nieuwe secundaire locatie en de gegevens worden gerepliceerd naar die locatie en de secundaire DNS-vermeldingen wijs. Secundair eindpunt is niet beschikbaar totdat het opslagaccount is voltooid met repliceren. Zie voor meer informatie [wat te doen als een Azure Storage-storing optreedt,](https://docs.microsoft.com/azure/storage/storage-disaster-recovery-guidance).

## <a name="application-design-considerations-when-using-ra-grs"></a>Toepassing Ontwerpoverwegingen bij het gebruik van RA-GRS

Het doel van dit artikel is beschreven hoe u met het ontwerpen van een toepassing die wel blijft werken (maar in een beperkte capaciteit), zelfs in het geval van een noodgeval op het primaire Datacenter. U kunt uw toepassing verwerkt tijdelijke of langdurige problemen met het lezen van de secundaire regio wanneer er een probleem dat bij het lezen van de primaire regio verstoort ontwerpen. Wanneer de primaire regio weer beschikbaar is, kan uw toepassing te lezen van de primaire regio geretourneerd.

### <a name="using-eventually-consistent-data"></a>Met behulp van uiteindelijk consistent gegevens

De voorgestelde oplossing wordt ervan uitgegaan dat het is acceptabel mogelijk verouderde gegevens terugkeren naar de aanroepende toepassing. Omdat gegevens in de secundaire regio uiteindelijk consistent is, is het mogelijk dat de primaire regio kan ontoegankelijk voordat een update naar de secundaire regio repliceren is voltooid.

Stel bijvoorbeeld dat uw klant een update met succes worden verzonden, maar de primaire regio mislukt voordat de update wordt doorgegeven naar de secundaire regio. Wanneer de klant wordt gevraagd om de gegevens terug te lezen, ontvangt hij de verouderde gegevens van de secundaire regio in plaats van de bijgewerkte gegevens. Bij het ontwerpen van uw toepassing, moet u bepalen of dit acceptabel is, en zo ja, hoe u de klant wordt weergegeven. 

Verderop in dit artikel, laten we zien hoe om te controleren van de tijd van laatste synchronisatie voor de secundaire gegevens om te controleren of de secundaire bijgewerkt is.

### <a name="handling-services-separately-or-all-together"></a>Afhandeling van de services afzonderlijk of Alles samenvoegen

Tijdens het onwaarschijnlijk, is het mogelijk voor een service niet beschikbaar terwijl de andere services nog steeds volledig functioneel zijn. U kunt verwerkt de nieuwe pogingen en de modus alleen-lezen voor elke service afzonderlijk (blobs, wachtrijen, tabellen), of kunt u pogingen algemeen voor de opslagservices tegelijk verwerken.

Bijvoorbeeld, als u wachtrijen en blobs in uw toepassing gebruikt, besluiten u in afzonderlijke code voor het afhandelen van herstelbare fouten voor elk van deze te plaatsen. Klik als u een nieuwe poging van de blob-service krijgt, maar de queue-service is nog steeds werken, worden slechts het deel van uw toepassing die verantwoordelijk is voor blobs beïnvloed. Als u besluit om af te handelen alle opslag service pogingen algemeen en een aanroep van de blob-service een herstelbare fout retourneert, worden aanvragen voor zowel de blob-service en de queue-service worden beïnvloed.

Dit is uiteindelijk afhankelijk van de complexiteit van uw toepassing. U kunt besluiten geen te verwerken van de fouten door service, maar in plaats daarvan omleiden van aanvragen voor alle storage-services naar de secundaire regio voor lezen en de toepassing uitvoert in de modus alleen-lezen wanneer u een probleem met een storage-service in de primaire regio detecteren.

### <a name="other-considerations"></a>Andere overwegingen

Dit zijn de andere overwegingen die worden besproken in de rest van dit artikel.

*   Verwerking van nieuwe pogingen van leesaanvragen met behulp van het patroon Circuitonderbreker

*   Uiteindelijk consistent gegevens en de tijd van laatste synchronisatie

*   Testen

## <a name="running-your-application-in-read-only-mode"></a>Uitvoeren van uw toepassing in de modus alleen-lezen

Voor het gebruik van RA-GRS-opslag, u moet beide mislukte leesaanvragen overweg en mislukte updateaanvragen (met update, wat betekent dat in dit geval voegt, updates en verwijderingen). Als de primaire gegevens mislukt centreren, leest u aanvragen kunnen worden omgeleid naar het secundaire Datacenter. Echter, updateaanvragen kunnen niet worden omgeleid naar de secundaire omdat de secundaire alleen-lezen is. Daarom moet u voor het ontwerpen van uw toepassing uit te voeren in de modus alleen-lezen.

U kunt bijvoorbeeld instellen dat een vlag die wordt gecontroleerd voordat de verzoeken van de update worden verzonden naar Azure Storage. Wanneer een van de updateaanvragen via komt, kunt u dit overslaan en retourneren een juiste reactie aan de klant. U kunt ook bepaalde functies uitschakelen helemaal totdat het probleem is opgelost en laat gebruikers weten dat deze functies zijn tijdelijk niet beschikbaar.

Als u besluit fouten worden verwerkt voor elke service afzonderlijk, moet u ook de mogelijkheid voor het uitvoeren van uw toepassing in de modus alleen-lezen door de service verwerkt. Bijvoorbeeld wellicht alleen-lezen vlaggen voor elke service die kunnen worden ingeschakeld en uitgeschakeld. Vervolgens kunt u de vlag in de juiste plaatsen verwerken in uw code.

Kunnen de toepassing uitvoeren in de modus alleen-lezen heeft een ander voordeel van de zijde – dit biedt u de mogelijkheid om ervoor te zorgen beperkte functionaliteit tijdens een upgrade van de primaire toepassing. U kunt uw toepassing uitvoeren in de modus alleen-lezen en wijs het secundaire Datacenter activeren zodat niemand toegang heeft tot de gegevens in de primaire regio terwijl u upgrades wilt aanbrengen.

## <a name="handling-updates-when-running-in-read-only-mode"></a>Verwerken van wijzigingen in de modus alleen-lezen

Er zijn veel manieren voor het afhandelen van aanvragen voor updates in de modus alleen-lezen. We dit uitvoerig won't verrekend, maar er zijn in het algemeen een aantal patronen waarmee u rekening houden.

1.  U kunt reageren als de gebruiker en laat dat u bent momenteel accepteert geen updates. Bijvoorbeeld, kan een contactpersoon beheersysteem kunnen klanten toegang krijgen tot contactgegevens, maar geen updates.

2.  U kunt de updates in een andere regio in de wachtrij plaatsen. In dit geval zou u schrijfaanvragen voor de update in behandeling aan een wachtrij in een andere regio en hebt een manier die aanvragen verwerkt nadat het primaire Datacenter weer online komt. Laat de klant weten dat de update die is aangevraagd in de wachtrij staat voor het verwerken van later in dit scenario.

3.  U kunt uw updates schrijven naar een opslagaccount in een andere regio. Als het primaire Datacenter weer online komt, kunt u een manier die updates samenvoegen met de primaire gegevens, afhankelijk van de structuur van de gegevens hebben. Als u afzonderlijke bestanden met een datum/tijd-stempel in de naam maakt, kunt u deze bestanden kopiëren terug naar de primaire regio. Dit werkt voor sommige werkbelastingen zoals logboekregistratie en iOT-gegevens.

## <a name="handling-retries"></a>Verwerking van nieuwe pogingen

Hoe wilt u weten welke fouten herstelbare? Dit wordt bepaald door de storage-clientbibliotheek. Een 404-fout (resource niet gevonden) is bijvoorbeeld niet-herstelbare omdat deze opnieuw proberen is niet waarschijnlijk leiden tot succes. Een 500 fout is aan de andere kant herstelbare omdat het een serverfout is opgetreden, en dit eenvoudig een tijdelijk probleem komen kan. Bekijk voor meer informatie de [open source code voor de klasse ExponentialRetry](https://github.com/Azure/azure-storage-net/blob/87b84b3d5ee884c7adc10e494e2c7060956515d0/Lib/Common/RetryPolicies/ExponentialRetry.cs) in de storage-clientbibliotheek voor .NET. (Zoek naar de methode ShouldRetry).

### <a name="read-requests"></a>Alleen aanvragen

Alleen aanvragen kunnen worden omgeleid naar de secundaire opslag als er een probleem met de primaire opslag. Als opgemerkt in [uiteindelijk consistente gegevens met behulp van](#using-eventually-consistent-data), moet deze zijn aanvaardbaar is voor uw toepassing verouderde gegevens mogelijk lezen. Als u de storage-clientbibliotheek voor toegang tot gegevens van de RA-GRS gebruikt, kunt u het gedrag van een leesaanvraag voor het opnieuw door een waarde voor de **LocationMode** eigenschap in op een van de volgende:

*   **PrimaryOnly** (de standaardinstelling)

*   **PrimaryThenSecondary**

*   **SecondaryOnly**

*   **SecondaryThenPrimary**

Tijdens het instellen van de **LocationMode** naar **PrimaryThenSecondary**, als de eerste aanvraag voor leestoegang tot de primaire eindpunt mislukt met een herstelbare fout, de client automatisch een andere leesaanvraag naar het secundaire eindpunt maakt. Als de fout een time-out van de server is, hebben de client moet worden gewacht voor de time-out is verlopen voordat het een herstelbare fout van de service ontvangt.

Er zijn in feite twee scenario's om u te overwegen wanneer u beslist reageren op een herstelbare fout:

*   Dit probleem is en een herstelbare fout niet de volgende aanvragen naar de primaire eindpunt wordt geretourneerd. Een voorbeeld van wanneer dit gebeurt mogelijk is wanneer er een tijdelijke fout.

    In dit scenario wordt er is geen aanzienlijke prestaties voor het hebben van **LocationMode** ingesteld op **PrimaryThenSecondary** als dit alleen zelden gebeurt.

*   Dit is een probleem met ten minste één van de storage-services in de primaire regio en alle volgende aanvragen die service in de primaire regio waarschijnlijk herstelbare fouten retourneren, voor een bepaalde periode. Een voorbeeld hiervan is als de primaire regio volledig niet toegankelijk.

    In dit scenario is het een op de prestaties omdat uw leesaanvragen wordt Probeer eerst het primaire eindpunt, wacht u totdat de time-out is verlopen en schakel over naar het secundaire eindpunt.

Voor deze scenario's, moet u bepalen dat er een actieve probleem met het primaire eindpunt en verzendt alle aanvragen rechtstreeks naar het secundaire eindpunt gelezen door het instellen van de **LocationMode** eigenschap **SecondaryOnly**. Op dit moment moet u ook de toepassing uit te voeren in de modus alleen-lezen te wijzigen. Deze aanpak wordt ook wel de [Circuitonderbreker patroon](https://msdn.microsoft.com/library/dn589784.aspx).

### <a name="update-requests"></a>Verzoeken om te werken

Het patroon Circuitonderbreker kan ook worden toegepast voor het bijwerken van aanvragen. Echter worden niet updateaanvragen omgeleid naar secundaire opslag alleen-lezen is. Voor deze aanvragen, laat u de **LocationMode** eigenschap ingesteld op **PrimaryOnly** (de standaardinstelling). U kunt een waarde van toepassing op deze aanvragen – zoals 10 fouten in een rij- en als de drempelwaarde wordt voldaan, schakelt de toepassing in de modus alleen-lezen voor het afhandelen van deze fouten. U kunt dezelfde methoden gebruiken voor het retourneren van modus als die hieronder wordt beschreven in de volgende sectie over het patroon Circuitonderbreker bijwerken.

## <a name="circuit-breaker-pattern"></a>Patroon voor Circuitonderbreker

Het patroon Circuitonderbreker gebruiken in uw toepassing kunt voorkomen dat deze opnieuw wordt geprobeerd een bewerking die is waarschijnlijk geen herhaaldelijk. Kunt u de toepassing wordt uitgevoerd in plaats van exponentieel inneemt bij de bewerking is geprobeerd. Daarnaast wordt gedetecteerd wanneer het probleem is opgelost, op dat moment kan de toepassing probeer het opnieuw.

### <a name="how-to-implement-the-circuit-breaker-pattern"></a>Het implementeren van het patroon Circuitonderbreker

Om aan te duiden dat er een actieve probleem met een primaire eindpunt is, kunt u controleren hoe vaak de client er een herstelbare fout optreedt. Omdat elk geval niet hetzelfde is, hebt u besluit de drempel die u wilt gebruiken voor de beslissing overschakelen naar het secundaire eindpunt en de toepassing uitvoert in de modus alleen-lezen. U kan bijvoorbeeld besluiten om uit te voeren van de switch als er 10 fouten in een rij met geen uitkomsten. Een ander voorbeeld is om over te schakelen als 90% van de aanvragen in een periode van 2 minuten mislukken.

Voor het eerste scenario kunt u eenvoudig houden van een aantal van de fouten en als er een is voltooid voordat het maximum is bereikt de telling weer instellen op nul. Voor het tweede scenario is een manier om dit te implementeren voor gebruik van het object MemoryCache (in .NET). Voor elke aanvraag een CacheItem toevoegen aan de cache, de waarde instellen op geslaagd (1) of mislukt (0) en de verlooptijd ingesteld op 2 minuten vanaf nu (of wat uw tijdsbeperking is). Wanneer een vermelding verlooptijd is bereikt, wordt de vermelding automatisch verwijderd. Hierdoor krijgt u een venster met rolling 2 minuten. Elke keer dat u een aanvraag naar de service storage u eerst gebruiken een Linq-query over het MemoryCache-object voor het berekenen van het percentage succes door de waarden op te tellen en te delen door het aantal. Als het percentage succes zakt onder sommige drempelwaarde (zoals 10%), stelt de **LocationMode** eigenschap voor lezen aanvragen naar **SecondaryOnly** en schakel over naar de toepassing in de modus alleen-lezen voordat u doorgaat.

De drempelwaarde van fouten die worden gebruikt om te bepalen bij het maken van de switch kan variëren van services in uw toepassing, zodat u kunt overwegen om deze parameters kunnen worden geconfigureerd. Dit is ook waarin u kiest voor het afhandelen van herstelbare fouten van elke service afzonderlijk of als een, zoals eerder besproken.

Andere overweging is hoe meerdere exemplaren van een toepassing verwerkt, en wat te doen wanneer u in elk exemplaar herstelbare fouten worden opgespoord. Bijvoorbeeld wellicht 20 VM's die worden uitgevoerd met dezelfde toepassing geladen. Verwerkt u elke instantie afzonderlijk? Als één exemplaar begint problemen, wilt u het antwoord op slechts één exemplaar beperken of u proberen wilt om alle exemplaren reageren op dezelfde manier als één exemplaar een probleem heeft? Afhandeling van de exemplaren afzonderlijk is veel eenvoudiger dan probeert te coördineren van het antwoord onder te brengen, maar hoe u dit doen is afhankelijk van de architectuur van uw toepassing.

### <a name="options-for-monitoring-the-error-frequency"></a>Opties voor het controleren van de frequentie van de fout

Hebt u drie belangrijkste mogelijkheden voor het controleren van de frequentie van nieuwe pogingen in de primaire regio om te bepalen wanneer u overstapt op de secundaire regio en het wijzigen van de toepassing uit te voeren in de modus alleen-lezen.

*   Toevoegen van een handler voor de [ **opnieuw proberen** ](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.operationcontext.retrying.aspx) gebeurtenis op de [ **OperationContext** ](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.operationcontext.aspx) object die u doorgeeft aan uw opslag aanvragen: dit is de methode weergegeven in dit artikel en gebruikt in de bijbehorende steekproef. Deze gebeurtenis geactiveerd wanneer de client een aanvraag opnieuw, zodat u bijhouden hoe vaak de client er een herstelbare fout op een primaire eindpunt optreedt.

    ```csharp 
    operationContext.Retrying += (sender, arguments) =>
    {
        // Retrying in the primary region
        if (arguments.Request.Host == primaryhostname)
            ...
    };
    ```

*   In de [ **Evaluate** ](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.iextendedretrypolicy.evaluate.aspx) methode in een beleid voor aangepaste opnieuw proberen, kunt u aangepaste code uitvoeren telkens wanneer een nieuwe poging plaatsvindt. Naast de opnemen wanneer een nieuwe poging gebeurt, dit biedt u eveneens de mogelijkheid om uw gedrag voor opnieuw proberen te wijzigen.

    ```csharp 
    public RetryInfo Evaluate(RetryContext retryContext,
    OperationContext operationContext)
    {
        var statusCode = retryContext.LastRequestResult.HttpStatusCode;
        if (retryContext.CurrentRetryCount >= this.maximumAttempts
            || ((statusCode >= 300 && statusCode < 500 && statusCode != 408)
            || statusCode == 501 // Not Implemented
            || statusCode == 505 // Version Not Supported
            ))
        {
            // Do not retry
            return null;
        }

        // Monitor retries in the primary location
        ...

        // Determine RetryInterval and TargetLocation
        RetryInfo info =
            CreateRetryInfo(retryContext.CurrentRetryCount);

        return info;
    }
    ```

*   De derde aanpak is het implementeren van een aangepaste bewakingsonderdeel in uw toepassing waarmee u uw eindpunt primaire opslag voortdurend met dummy schijfleesaanvragen pingt (zoals het lezen van een kleine blob) om de status te bepalen. Deze zou resources, maar een aanzienlijke duren. Wanneer er een probleem is gedetecteerd dat de drempel bereikt, voert u vervolgens het overschakelen naar **SecondaryOnly** en alleen-lezen-modus.

U wilt overschakelen naar het primaire eindpunt gebruikt en het toestaan van updates op een bepaald moment. Als u een van de eerste twee methoden die hierboven worden genoemd, kan u gewoon Ga terug naar de primaire eindpunt en de updatemodus inschakelen nadat een willekeurig geselecteerde hoeveelheid tijd of het aantal bewerkingen is uitgevoerd. Vervolgens kunt u deze opnieuw de Pogingslogica doorlopen. Als het probleem is verholpen, blijft het primaire eindpunt gebruikt en dat de updates. Als er nog steeds een probleem is, zal deze één keer terug naar de secundaire eindpunt en de modus alleen-lezen overschakelen lukt de criteria die u hebt ingesteld.

Voor het derde scenario wanneer het eindpunt van de primaire opslag pingen weer mislukt, kunt u activeren de switch terug naar **PrimaryOnly** en doorgaan updates toestaan.

## <a name="handling-eventually-consistent-data"></a>Uiteindelijk consistente gegevens verwerken

RA-GRS werkt met het repliceren van transacties van de primaire naar de secundaire regio. Dit replicatieproces zorgt ervoor dat de gegevens in de secundaire regio is *uiteindelijk consistent*. Dit betekent dat alle transacties in de primaire regio uiteindelijk wordt weergegeven in de secundaire regio, maar dat er mogelijk een vertraging voordat ze worden weergegeven en of er is geen garantie de transacties in de secundaire regio in dezelfde volgorde als waarin ze oorspronkelijk zijn toegepast in de primaire regio binnenkomen. Als uw transacties in de secundaire regio volgorde plaatsvinden, u *mogelijk* Houd rekening met uw gegevens in de secundaire regio niet in een inconsistente status totdat de service de resultaten.

De volgende tabel toont een voorbeeld van wat er gebeuren kan wanneer u de details van een werknemer haar een lid maken van bijwerken de *beheerders* rol. In dit voorbeeld hiervoor moet u bijwerken de **werknemer** entiteit en update een **beheerdersrol** entiteit met een telling van het totale aantal beheerders. U ziet hoe de updates volgorde in de secundaire regio worden toegepast.

| **Tijd** | **Transactie**                                            | **Replicatie**                       | **Tijd van laatste synchronisatie** | **Resultaat** |
|----------|------------------------------------------------------------|---------------------------------------|--------------------|------------| 
| T0       | Transactie A: <br> Werknemer invoegen <br> entiteit in primaire |                                   |                    | Transactie A ingevoegd op primaire,<br> nog niet gerepliceerd. |
| T1       |                                                            | Een transactie <br> gerepliceerd naar<br> secundair | T1 | Een transactie is gerepliceerd naar de secundaire. <br>Tijd van laatste synchronisatie is bijgewerkt.    |
| T2       | Transactie B:<br>Update<br> werknemer-entiteit<br> in primaire  |                                | T1                 | Transactie geschreven naar de primaire B<br> nog niet gerepliceerd.  |
| T3       | Transactie C:<br> Update <br>Beheerder<br>de entiteit rol in<br>primair |                    | T1                 | Transactie geschreven naar de primaire, C<br> nog niet gerepliceerd.  |
| *T4*     |                                                       | Transactie C <br>gerepliceerd naar<br> secundair | T1         | Transactie C gerepliceerd naar de secundaire.<br>LastSyncTime niet bijgewerkt, omdat <br>transactie B is nog niet gerepliceerd.|
| *T 5*     | Entiteiten lezen <br>secundaire                           |                                  | T1                 | Ophalen van de verouderde waarde voor de werknemer <br> entiteit omdat de transactie B nog niet <br> nog gerepliceerd. Ophalen van de nieuwe waarde voor<br> Administrator-rol entiteit omdat C<br> gerepliceerd. Tijd van laatste synchronisatie is nog steeds niet<br> is bijgewerkt, omdat de transactie B<br> nog niet gerepliceerd. U kunt zien de<br>Administrator-rol entiteit komt niet overeen <br>omdat de entiteit datum/tijd na <br>de tijd van laatste synchronisatie. |
| *T6*     |                                                      | Transactie B<br> gerepliceerd naar<br> secundair | T6                 | *T6* – alle transacties via C hebben <br>zijn gerepliceerd, tijd van laatste synchronisatie<br> is bijgewerkt. |

In dit voorbeeld wordt ervan uitgegaan dat de client overschakelt naar het lezen van de secundaire regio op t 5. Deze kan lezen de **beheerdersrol** entiteit op dit moment, maar de entiteit bevat een waarde op voor het aantal komt niet overeen met het aantal beheerders **werknemer** entiteiten die zijn gemarkeerd als beheerders in de secundaire regio op dit moment. De client kan deze waarde, met het risico dat het inconsistente informatie is gewoon weergeven. U kunt ook de client kan proberen om te bepalen die de **beheerdersrol** is een mogelijk inconsistente status omdat de updates is een ongeldige volgorde, en vervolgens de gebruiker van dit feit informeert.

Voor het herkennen van of deze heeft mogelijk inconsistente gegevens, de client gebruikt de waarde van de *tijd van laatste synchronisatie* dat u op elk gewenst moment ophalen kunt door het opvragen van een storage-service. Weet u de tijd waarop de gegevens in de secundaire regio laatst consistent en wanneer de service alle transacties voorafgaand aan dat punt in tijd waren toegepast. In het voorbeeld hierboven, nadat de service wordt ingevoegd de **werknemer** entiteit in de secundaire regio, de tijd van laatste synchronisatie is ingesteld op *T1*. Blijft *T1* tot de service-updates de **werknemer** entiteit in de secundaire regio wanneer deze is ingesteld op *T6*. Als de client worden opgehaald van de tijd van laatste synchronisatie het lezen van de entiteit op *t 5*, deze kan worden vergeleken met de tijdstempel op de entiteit. Als de tijdstempel op de entiteit later dan de tijd van laatste synchronisatie is, klikt u vervolgens de entiteit is een mogelijk inconsistente status heeft en u kunt nemen wat is de juiste actie voor uw toepassing. In dit veld is vereist dat u weet waarop het laatst is bijgewerkt naar de primaire is voltooid.

## <a name="testing"></a>Testen

Het is belangrijk om te testen dat uw toepassing werkt zoals verwacht wanneer herstelbare fouten worden aangetroffen. Bijvoorbeeld, wilt u testen dat de toepassing aan de secundaire en in de modus alleen-lezen wanneer deze een probleem detecteert en verandert teruggeschakeld wanneer de primaire regio weer beschikbaar. Om dit te doen, moet u een manier om te simuleren herstelbare fouten en beheer op hoe vaak ze voorkomen.

U kunt [Fiddler](http://www.telerik.com/fiddler) onderscheppen en HTTP-antwoorden in een script wijzigen. Dit script kunt identificeren van reacties die afkomstig van uw primaire eindpunt zijn en de HTTP-statuscode wijzigen dat de Opslagclientbibliotheek wordt herkend als een herstelbare fout. Dit codefragment toont een eenvoudig voorbeeld van een Fiddler-script waarmee antwoorden onderschept moeten worden gelezen aanvragen op basis van de **employeedata** tabel de status 502:

```java
static function OnBeforeResponse(oSession: Session) {
    ...
    if ((oSession.hostname == "\[yourstorageaccount\].table.core.windows.net")
      && (oSession.PathAndQuery.StartsWith("/employeedata?$filter"))) {
        oSession.responseCode = 502;
    }
}
```

U kunt dit voorbeeld voor het onderscheppen van een breed scala aan aanvragen en alleen wijzigen uitbreiden de **responseCode** op sommige hiervan een Praktijkscenario beter te simuleren. Zie voor meer informatie over het aanpassen van Fiddler scripts [wijzigen van een aanvraag of antwoord](http://docs.telerik.com/fiddler/KnowledgeBase/FiddlerScript/ModifyRequestOrResponse) in de documentatie van Fiddler.

Als u de drempelwaarden voor het overschakelen van uw toepassing naar de alleen-lezen-modus worden geconfigureerd hebt aangebracht, worden deze eenvoudiger is om het gedrag met niet-productieve transactie volumes te testen.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over leestoegang geografische redundantie, met inbegrip van een ander voorbeeld van hoe de LastSyncTime is ingesteld, neem [Windows Azure-opslagopties redundantie en geografisch redundante opslag met leestoegang](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

* Zie voor een compleet codevoorbeeld waarin wordt getoond hoe u de schakeloptie heen en weer tussen de primaire en secundaire eindpunten, [Azure Samples – het Circuitonderbreker-patroon gebruiken met RA-GRS-storage](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs).
