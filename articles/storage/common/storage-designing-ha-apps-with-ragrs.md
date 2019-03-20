---
title: Het ontwerpen van maximaal beschikbare Aaplications met behulp van geo-redundante opslag met leestoegang (RA-GRS) | Microsoft Docs
description: Het gebruik van Azure RA-GRS-opslag te ontwerpen van een maximaal beschikbare toepassing flexibel genoeg is voor het afhandelen van storingen.
services: storage
author: tamram
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 01/17/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 256d709ac976736715f441ecde5eee22a6d86fa6
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58009082"
---
# <a name="designing-highly-available-applications-using-ra-grs"></a>Maximaal beschikbare toepassingen met RA-GRS ontwerpen

Een algemene functie van de cloud gebaseerde infrastructuur, zoals Azure Storage is dat ze een maximaal beschikbare platform bieden voor het hosten van toepassingen. Ontwikkelaars van cloud-gebaseerde toepassingen Overweeg zorgvuldig hoe u dit platform voor het leveren van toepassingen met hoge beschikbaarheid met hun gebruikers. In dit artikel is gericht op hoe ontwikkelaars geografisch redundante opslag met leestoegang (RA-GRS gebruiken kunnen) om ervoor te zorgen dat hun toepassingen met Azure Storage maximaal beschikbaar zijn.

[!INCLUDE [storage-common-redundancy-options](../../../includes/storage-common-redundancy-options.md)]

In dit artikel is gericht op GRS en RA-GRS. Met GRS worden drie kopieën van uw gegevens worden bewaard in de primaire regio die u hebt geselecteerd bij het instellen van het storage-account. Drie extra kopieën worden asynchroon bijgehouden in een secundaire regio die is opgegeven door Azure. RA-GRS biedt geografisch redundante opslag met leestoegang tot de secundaire kopie.

Voor informatie over welke primaire regio's zijn gekoppeld aan welke secundaire regio's, Zie [zakelijke continuïteit en herstel na noodgevallen (BCDR): Gekoppelde Azure-regio's](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

Er zijn codefragmenten die zijn opgenomen in dit artikel en een koppeling naar een compleet voorbeeld aan het einde die u kunt downloaden en uitvoeren.

> [!NOTE]
> Azure Storage biedt nu ondersteuning voor zone-redundante opslag (ZRS) voor het bouwen van toepassingen met hoge beschikbaarheid. ZRS biedt een eenvoudige oplossing voor de behoeften van de redundantie van de vele toepassingen. ZRS biedt bescherming tegen hardwarestoringen of catastrofale rampen in één datacenter. Zie voor meer informatie, [Zone-redundante opslag (ZRS): Maximaal beschikbare toepassingen voor Azure Storage](storage-redundancy-zrs.md).

## <a name="key-features-of-ra-grs"></a>Belangrijke functies van RA-GRS

Houd rekening met deze sleutel bij het ontwerpen van uw toepassing voor RA-GRS:

* Azure-opslag onderhoudt een alleen-lezen kopie van de gegevens die u in de primaire regio in een secundaire regio opslaat. Zoals eerder vermeld, bepaalt de storage-service de locatie van de secundaire regio.

* Het kopiëren van alleen-lezen is [uiteindelijk consistent](https://en.wikipedia.org/wiki/Eventual_consistency) met de gegevens in de primaire regio.

* Voor blobs, tabellen en wachtrijen, kunt u een query de secundaire regio voor een *tijd van laatste synchronisatie* waarde die aangeeft wanneer de laatste replicatie van de primaire naar de secundaire regio is opgetreden. (Dit wordt niet ondersteund voor Azure Files, dat niet beschikt over RA-GRS redundantie op dit moment.)

* U kunt de Storage-clientbibliotheek gebruiken om te communiceren met de gegevens in de primaire of secundaire regio. U kunt ook omleiden lezen aanvragen automatisch naar de secundaire regio als daarom wordt gevraagd naar de primaire regio een optreedt time-out.

* Als de primaire regio niet beschikbaar is, kunt u een account failover starten. Als u een failover naar de secundaire regio, worden de DNS-vermeldingen die verwijst naar de primaire regio gewijzigd om te verwijzen naar de secundaire regio. Nadat de failover voltooid is, wordt toegang voor schrijven is hersteld voor GRS en RA-GRS-accounts. Zie voor meer informatie, [Disaster recovery en storage-account failover (preview) in Azure Storage](storage-disaster-recovery-guidance.md).

## <a name="application-design-considerations-when-using-ra-grs"></a>Ontwerpoverwegingen voor toepassingen bij het gebruik van RA-GRS

Er is het doel van dit artikel leert u hoe u aan het ontwerpen van een toepassing die wel blijft werken (zij het in een beperkte capaciteit), zelfs in het geval van een grote ramp op het primaire Datacenter. U kunt uw toepassing voor het afhandelen van tijdelijke of langdurige problemen met het lezen van de secundaire regio wanneer zich een probleem voordoet die een conflict met het lezen van de primaire regio veroorzaakt ontwerpen. Als de primaire regio weer beschikbaar is, kan uw toepassing te lezen van de primaire regio retourneren.

### <a name="using-eventually-consistent-data"></a>Met behulp van de gegevens uiteindelijk consistent

De voorgestelde oplossing wordt ervan uitgegaan dat het is mogelijk verlopen gegevens terug naar de aanroepende toepassing worden geaccepteerd. Omdat gegevens in de secundaire regio uiteindelijk consistent is, is het mogelijk dat de primaire regio kan ontoegankelijk worden voordat een update naar de secundaire regio repliceren is voltooid.

Stel bijvoorbeeld dat uw klant een update met succes worden verzonden, maar de primaire regio niet voordat de update is doorgegeven aan de secundaire regio. Wanneer de klant wordt gevraagd om de gegevens terug te lezen, ontvangt hij de verouderde gegevens van de secundaire regio in plaats van de bijgewerkte gegevens. Bij het ontwerpen van uw toepassing, moet u bepalen of dit acceptabel is, en als dit het geval is, hoe u de klant wordt weergegeven. 

Verderop in dit artikel laten we zien hoe om te controleren of de tijd van laatste synchronisatie voor de secundaire gegevens om te controleren of de secundaire up-to-date is.

### <a name="handling-services-separately-or-all-together"></a>Verwerking van services afzonderlijk of alles bij elkaar

Hoewel dit onwaarschijnlijk is, is het mogelijk dat een service niet meer beschikbaar zijn terwijl de andere services nog steeds volledig functioneel zijn. U kunt verwerken de nieuwe pogingen en de modus alleen-lezen voor elke service afzonderlijk (blobs, wachtrijen en tabellen), of nieuwe pogingen algemeen voor alle storage-services samen kunnen worden verwerkt.

Bijvoorbeeld, als u wachtrijen en blobs in uw toepassing gebruikt, besluiten u om te zetten in de afzonderlijke code voor het afhandelen van herstelbare fouten voor elk van deze. Als u een nieuwe poging uit de blob-service ophalen, maar de queue-service is nog steeds werken, kunnen alleen het deel van uw toepassing die verantwoordelijk is voor blobs vervolgens zal worden beïnvloed. Als u besluit algemeen alle storage service-nieuwe pogingen afhandelt en een aanroep naar de blob-service een herstelbare fout retourneert, worden aanvragen naar de blobservice en de queue-service worden beïnvloed.

Uiteindelijk, dit is afhankelijk van de complexiteit van uw toepassing. Wilt u misschien niet de fouten worden verwerkt door de service, maar in plaats daarvan omleiden gelezen aanvragen voor alle storage-services naar de secundaire regio en de toepassing uitvoert in de modus alleen-lezen wanneer u een probleem met een storage-service in de primaire regio detecteren.

### <a name="other-considerations"></a>Andere overwegingen

Dit zijn de andere overwegingen die we in de rest van dit artikel wordt besproken.

*   Verwerking van nieuwe pogingen van leesaanvragen met behulp van het circuitonderbrekerpatroon

*   Uiteindelijk consistent gegevens en de tijd van laatste synchronisatie

*   Testen

## <a name="running-your-application-in-read-only-mode"></a>Uw toepassing wordt uitgevoerd in de modus alleen-lezen

Voor het gebruik van RA-GRS-opslag, u moet mogelijk voor het afhandelen van beide mislukte aanvragen voor het lezen en mislukte updateaanvragen (met de update, wat betekent dat in dit geval voegt, updates en verwijderingen). Als het primaire Datacenter mislukt, leest u aanvragen kunnen worden omgeleid naar het secundaire Datacenter. Updateaanvragen niet kunnen echter worden omgeleid naar de secundaire, omdat de secundaire alleen-lezen is. Daarom moet u ontwerp uw toepassing worden uitgevoerd in de modus alleen-lezen.

U kunt bijvoorbeeld een markering waarmee wordt gecontroleerd voordat u een updateaanvragen worden verzonden naar Azure Storage instellen. Wanneer een van de updateaanvragen via komt, kunt u overslaan en retourneren een juiste reactie naar de klant. U kunt ook bepaalde functies uitschakelen helemaal totdat het probleem is opgelost en laat gebruikers weten dat deze functies zijn tijdelijk niet beschikbaar.

Als u besluit voor het afhandelen van fouten voor elke service afzonderlijk, moet u ook de mogelijkheid om uit te voeren van uw toepassing in de modus alleen-lezen door de service worden verwerkt. Bijvoorbeeld, wellicht u alleen-lezen vlaggen voor elke service die kunnen worden ingeschakeld en uitgeschakeld. Vervolgens kunt u de vlag in de juiste plaatsen worden uitgevoerd in uw code verwerken.

De mogelijkheid om uit te voeren van uw toepassing in de modus alleen-lezen is een ander voordeel van kant – dit biedt u de mogelijkheid om te controleren of beperkte functionaliteit tijdens een grote toepassingsupgrade. U kunt uw toepassing uit te voeren in de modus alleen-lezen en verwijzen naar het secundaire Datacenter activeren ervoor te zorgen dat niemand toegang heeft tot de gegevens in de primaire regio terwijl u upgrades wilt aanbrengen.

## <a name="handling-updates-when-running-in-read-only-mode"></a>Verwerken van updates wanneer uitgevoerd in de modus alleen-lezen

Er zijn veel manieren voor het afhandelen van verzoeken om te werken wanneer ze in de modus alleen-lezen. We uitvoerig wordt niet behandeld, maar er zijn enkele patronen waarmee u rekening houden.

1.  U kunt reageren als de gebruiker en vertel dat u geen updates accepteren. Bijvoorbeeld, kan een contact op met beheersysteem kunnen klanten toegang krijgen tot contactgegevens, maar geen updates.

2.  U kunt de updates in een andere regio in de wachtrij plaatsen. In dit geval zou u schrijven van de aanvragen in behandeling bijwerken naar een wachtrij in een andere regio en vervolgens hebben een manier te verwerken nadat het primaire Datacenter weer online komt. In dit scenario, moet u toestaan dat de klant weten dat de update die is aangevraagd in de wachtrij voor latere verwerking wordt geplaatst.

3.  U kunt uw updates schrijven naar een opslagaccount in een andere regio. Als het primaire Datacenter weer online komt, kunt u een manier voor het samenvoegen van deze updates in de primaire gegevens, afhankelijk van de structuur van de gegevens hebben. Als u afzonderlijke bestanden met een datum-/ tijdstempel in de naam maakt, kunt u deze bestanden kopiëren naar de primaire regio. Dit werkt voor sommige werkbelastingen zoals logboekregistratie en iOT-gegevens.

## <a name="handling-retries"></a>Verwerking van nieuwe pogingen

Hoe wilt u weten welke fouten zijn herstelbare? Dit wordt bepaald door de storage-clientbibliotheek. Een 404-fout (resource is niet gevonden) is bijvoorbeeld niet-herstelbare omdat opnieuw wordt geprobeerd deze waarschijnlijk niet leiden tot succes. Een 500 fout is aan de andere kant herstelbare omdat het een serverfout betreft, en dit eenvoudig een tijdelijk probleem komen kan. Bekijk voor meer informatie de [open source-code voor de klasse ExponentialRetry](https://github.com/Azure/azure-storage-net/blob/87b84b3d5ee884c7adc10e494e2c7060956515d0/Lib/Common/RetryPolicies/ExponentialRetry.cs) in de storage-clientbibliotheek voor .NET. (Zoek naar de methode ShouldRetry.)

### <a name="read-requests"></a>Aanvragen lezen

Leesaanvragen kunnen worden omgeleid naar een secundaire opslag als er een probleem met primaire opslag. Hiervoor in als opgemerkt [uiteindelijk Consistent gegevens met behulp van](#using-eventually-consistent-data), moet aanvaardbaar is voor uw toepassing mogelijk verouderde gegevens te lezen. Als u van de storage-clientbibliotheek gebruikmaakt voor toegang tot RA-GRS-gegevens, kunt u het gedrag voor opnieuw proberen van een leesaanvraag opgeven door een waarde voor de **LocationMode** eigenschap in op een van de volgende:

*   **PrimaryOnly** (de standaardinstelling)

*   **PrimaryThenSecondary**

*   **SecondaryOnly**

*   **SecondaryThenPrimary**

Tijdens het instellen van de **LocationMode** naar **PrimaryThenSecondary**, als de eerste lezen aanvraag naar de primaire eindpunt mislukt vanwege een herstelbare fout, de client automatisch een andere aanvraag voor leestoegang tot maakt de secundaire eindpunt. Als de fout een time-out van de server is, klikt u vervolgens hebben de client na afloop van de time-out is verlopen voordat er een herstelbare fout van de service ontvangt.

Er zijn in feite twee scenario's om te overwegen wanneer u beslist het reageren op een herstelbare fout:

*   Dit is een probleem met de geïsoleerde en volgende aanvragen naar het primaire eindpunt niet een herstelbare fout retourneert. Een voorbeeld van waar dit kan gebeuren is wanneer er een tijdelijke fout.

    In dit scenario, er is geen aanzienlijke prestaties nadelig worden beïnvloed in met **LocationMode** ingesteld op **PrimaryThenSecondary** als dit gebeurt alleen af en toe.

*   Dit is een probleem met ten minste één van de storage-services in de primaire regio en alle volgende aanvragen die service in de primaire regio waarschijnlijk herstelbare fouten retourneren voor een bepaalde periode. Een voorbeeld hiervan is als de primaire regio volledig niet toegankelijk is.

    In dit scenario is het een op de prestaties omdat de leesaanvragen wordt Probeer eerst het primaire eindpunt, wacht totdat de time-out is verlopen en schakel over naar het secundaire eindpunt.

Voor deze scenario's, moet u aangeven dat er een lopende probleem met het primaire eindpunt en verzenden aanvragen rechtstreeks naar het secundaire eindpunt lezen door in te stellen de **LocationMode** eigenschap **SecondaryOnly** . Op dit moment moet u ook de toepassing worden uitgevoerd in de modus alleen-lezen te wijzigen. Deze aanpak wordt ook wel de [Circuitonderbrekerpatroon](https://msdn.microsoft.com/library/dn589784.aspx).

### <a name="update-requests"></a>Verzoeken om te werken

Het circuitonderbrekerpatroon kan ook worden toegepast voor het bijwerken van aanvragen. Echter worden niet updateaanvragen omgeleid naar een secundaire opslag, die alleen-lezen is. Voor deze aanvragen, vult u de **LocationMode** eigenschap ingesteld op **PrimaryOnly** (de standaardinstelling). Voor het afhandelen van deze fouten, kunt u een metrische waarde toepassen op deze aanvragen – zoals 10 fouten in een rij- en wanneer de drempelwaarde is bereikt, schakelt u over de toepassing in de modus alleen-lezen. U kunt dezelfde methoden gebruiken voor het retourneren van modus als die hieronder wordt beschreven in de volgende sectie over het circuitonderbrekerpatroon bijwerken.

## <a name="circuit-breaker-pattern"></a>Patroon Circuitonderbreker

Met behulp van het circuitonderbrekerpatroon in uw toepassing kunt voorkomen dat deze opnieuw wordt geprobeerd een bewerking die waarschijnlijk zal mislukken herhaaldelijk. Hierdoor kan de toepassing om door te gaan om uit te voeren in plaats van in beslag terwijl de bewerking wordt opnieuw uitgevoerd exponentieel toeneemt. Daarnaast wordt gedetecteerd wanneer de fout is opgelost, op dat moment de toepassing de bewerking opnieuw kunt proberen.

### <a name="how-to-implement-the-circuit-breaker-pattern"></a>Het implementeren van het circuitonderbrekerpatroon

Om aan te duiden dat er een lopend probleem met een primaire eindpunt is, kunt u controleren hoe vaak de client er een herstelbare fout optreedt. Omdat elk geval verschilt, hebt u om te bepalen wat de drempelwaarde die u wilt gebruiken voor de beslissing om te schakelen naar het secundaire eindpunt en de toepassing uitvoert in de modus alleen-lezen. Bijvoorbeeld, kan u besluit om uit te voeren van de switch als er 10 fouten in een rij met geen hersteltaken zijn gelukt. Een ander voorbeeld is om over te schakelen als 90% van de aanvragen in een periode van 2 minuten mislukken.

Voor het eerste scenario kunt u gewoon een aantal van de fouten te houden en als er een geslaagde pogingen alvorens het maximum, het aantal ingesteld op nul. Voor het tweede scenario wordt één manier om dit te implementeren is het gebruik van het object MemoryCache (in .NET). Voor elke aanvraag een CacheItem toevoegen aan de cache, de waarde instellen op geslaagd (1) of mislukt (0) en de vervaltijd ingesteld op 2 minuten vanaf nu (of wat uw time-beperking is). Wanneer de verlooptijd van een vermelding is bereikt, wordt de vermelding automatisch verwijderd. Hiermee geeft u een rolling 2 minuten-venster. Telkens wanneer die u een aanvraag met de storage-service indienen, u eerst gebruiken een Linq-query voor het object MemoryCache voor het berekenen van het percentage voltooid door de waarden op te tellen en te delen door het aantal. Wanneer het percentage voltooid zakt tot onder een bepaalde drempelwaarde (bijvoorbeeld 10%), stelt de **LocationMode** eigenschap voor lezen aanvragen naar **SecondaryOnly** en schakelt u over de toepassing in de modus alleen-lezen voordat u doorgaat.

De drempelwaarde van fouten die worden gebruikt om te bepalen wanneer u het beste de switch kan variëren van services in uw toepassing, zodat u kunt overwegen om deze configureerbare parameters. Dit is ook waar u besluit om af te handelen herstelbare fouten van elke service afzonderlijk of als een, zoals eerder besproken.

Een andere overweging is hoe u voor het afhandelen van meerdere exemplaren van een toepassing, en wat te doen wanneer u herstelbare fouten in elk exemplaar detecteren. Bijvoorbeeld, wellicht u 20 VM's met dezelfde toepassing geladen. U elk exemplaar afzonderlijk verwerkt? Als één exemplaar wordt gestart problemen, wilt u het antwoord om alleen die één exemplaar te beperken of u proberen wilt om alle exemplaren reageren op dezelfde manier als één exemplaar een probleem heeft? Afzonderlijk verwerken van de exemplaren is veel eenvoudiger dan coördinatie van het antwoord ze, maar doet u als volgt is afhankelijk van de architectuur van uw toepassing.

### <a name="options-for-monitoring-the-error-frequency"></a>Opties voor het controleren van de frequentie van de fout

U hebt drie belangrijkste opties voor het bewaken van de frequentie van nieuwe pogingen in de primaire regio om te bepalen wanneer Schakel over naar de secundaire regio en het wijzigen van de toepassing worden uitgevoerd in de modus alleen-lezen.

*   Toevoegen van een handler voor de [ **opnieuw proberen** ](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.operationcontext.retrying.aspx) gebeurtenis op de [ **OperationContext** ](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.operationcontext.aspx) object die u doorgeeft aan uw opslag aanvragen: dit is de methode in dit artikel wordt weergegeven en gebruikt in het bijbehorende voorbeeld. Deze gebeurtenissen worden geactiveerd wanneer de client probeert om opnieuw een aanvraag, zodat u kunt om bij te houden hoe vaak de client op een primaire eindpunt herstelbare fouten tegenkomt.

    ```csharp 
    operationContext.Retrying += (sender, arguments) =>
    {
        // Retrying in the primary region
        if (arguments.Request.Host == primaryhostname)
            ...
    };
    ```

*   In de [ **evalueren** ](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.iextendedretrypolicy.evaluate.aspx) methode in een beleid voor aangepaste opnieuw proberen, kunt u aangepaste code uitgevoerd wanneer een nieuwe poging plaatsvindt. Naast het op te nemen wanneer een nieuwe poging gebeurt, dit biedt u ook de mogelijkheid om te wijzigen van het gedrag voor opnieuw proberen.

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

*   De derde methode is het implementeren van een aangepaste bewakingsonderdeel in uw toepassing die voortdurend pingt het eindpunt van de primaire opslag met dummy lezen aanvragen (zoals het lezen van een kleine blob) om de status te bepalen. Dit zou aantal resources, maar niet een groot deel duren. Wanneer er een probleem is gedetecteerd dat de drempelwaarde is bereikt, voert u vervolgens het overschakelen naar **SecondaryOnly** en de modus alleen-lezen.

U wilt overschakelen naar het primaire eindpunt gebruikt en updates toestaan op een bepaald moment. Als u een van de eerste twee methoden die hierboven worden vermeld, kan u gewoon Ga terug naar het primaire eindpunt en de modus voor het bijwerken van inschakelen nadat een willekeurig geselecteerde hoeveelheid tijd of het aantal bewerkingen zijn uitgevoerd. U kunt vervolgens deze logica voor opnieuw proberen opnieuw doorlopen. Als het probleem is opgelost, blijft het gebruik van het primaire eindpunt en updates toestaan. Als er nog steeds een probleem is, wordt deze zodra er meer Ga terug naar het secundaire eindpunt en de modus alleen-lezen na het mislukken van de criteria die u hebt ingesteld.

Voor het derde scenario wanneer de primaire opslag-eindpunt te pingen weer geslaagd, u kunt activeren de switch terug naar **PrimaryOnly** en doorgaan met updates toestaan.

## <a name="handling-eventually-consistent-data"></a>Uiteindelijk consistent gegevens verwerken

RA-GRS werkt door transacties te repliceren van de primaire naar de secundaire regio. Dit replicatieproces zorgt ervoor dat de gegevens in de secundaire regio worden *uiteindelijk consistent*. Dit betekent dat alle transacties in de primaire regio uiteindelijk wordt weergegeven in de secundaire regio, maar dat er mogelijk een vertraging voordat ze worden weergegeven en er is geen garantie dat de transacties binnenkomen in de secundaire regio in dezelfde volgorde als waarin ze oorspronkelijk zijn toegepast in de primaire regio. Als uw transacties in de secundaire regio andere volgorde binnenkomen, u *kan* Houd rekening met uw gegevens in de secundaire regio zich in een inconsistente status totdat de service de resultaten.

De volgende tabel ziet u een voorbeeld van wat er gebeuren kan wanneer u de details van een werknemer haar om lid te maken van de *beheerders* rol. Dit voorbeeldscenario hiervoor moet u bijwerken de **werknemer** entiteits- en update een **beheerdersrol** entiteit met een telling van het totale aantal beheerders. U ziet hoe de updates niet de juiste volgorde in de secundaire regio worden toegepast.

| **tijd** | **Transactie**                                            | **Replicatie**                       | **Laatst gesynchroniseerd op** | **Resultaat** |
|----------|------------------------------------------------------------|---------------------------------------|--------------------|------------| 
| T0       | Transactie A: <br> Werknemer invoegen <br> entiteit in primaire |                                   |                    | Transactie een ingevoegd op primaire,<br> nog niet gerepliceerd. |
| T1       |                                                            | Een transactie <br> gerepliceerd naar<br> secundair | T1 | Een transactie is gerepliceerd naar de secundaire. <br>Tijd van laatste synchronisatie bijgewerkt.    |
| T2       | Transactie B:<br>Update<br> Werknemer-entiteit<br> in de primaire  |                                | T1                 | Transactie B naar primair, geschreven<br> nog niet gerepliceerd.  |
| T3       | Transactie C:<br> Update <br>beheerder<br>entiteit in rol<br>primair |                    | T1                 | Transactie geschreven naar primair, C<br> nog niet gerepliceerd.  |
| *T4*     |                                                       | Transactie C <br>gerepliceerd naar<br> secundair | T1         | Transactie C gerepliceerd naar de secundaire.<br>Niet bijgewerkt omdat LastSyncTime <br>transactie B is nog niet gerepliceerd.|
| *T5*     | Entiteiten lezen <br>uit de secundaire regio                           |                                  | T1                 | U krijgt de verouderde waarde voor werknemer <br> entiteit omdat transactie B nog niet <br> nog gerepliceerd. U krijgt de nieuwe waarde voor<br> Administrator-rol entiteit omdat er voor C<br> gerepliceerd. Tijd van laatste synchronisatie is nog niet<br> is bijgewerkt, omdat de transactie B<br> nog niet is gerepliceerd. U kunt zien de<br>Administrator-rol entiteit komt niet overeen <br>omdat de entiteit-datum/tijd na <br>de tijd van laatste synchronisatie. |
| *T6*     |                                                      | Transactie B<br> gerepliceerd naar<br> secundair | T6                 | *T6* – alle transacties via C <br>zijn gerepliceerd, tijd van laatste synchronisatie<br> is bijgewerkt. |

In dit voorbeeld wordt ervan uitgegaan dat de client overschakelt naar het lezen van de secundaire regio op t 5. Deze gegevens kan lezen de **beheerdersrol** entiteit op dit moment, maar de entiteit bevat een waarde op voor het aantal administrators die is niet consistent met het aantal **werknemer** entiteiten die zijn gemarkeerd als beheerders in de secundaire regio op dit moment. De client kan deze waarde, met het risico dat het inconsistente gegevens is gewoon weergeven. U kunt ook de client kan proberen om te bepalen die de **beheerdersrol** heeft de mogelijk inconsistent status omdat de updates zich hebben voorgedaan andere volgorde en vervolgens de gebruiker van dit feit informeert.

Voor het herkennen van of er mogelijk inconsistente gegevens, de client gebruikt de waarde van de *tijd van laatste synchronisatie* dat u op elk gewenst moment krijgen kunt door het opvragen van een storage-service. Weet u de tijd waarop de gegevens in de secundaire regio laatst consistent en wanneer de service alle transacties voorafgaand aan dat punt in tijd had toegepast. In het voorbeeld hierboven, nadat de service voegt de **werknemer** entiteit in de secundaire regio, de tijd van laatste synchronisatie is ingesteld op *T1*. Blijft *T1* totdat de service-updates de **werknemer** entiteit in de secundaire regio als deze is ingesteld op *T6*. Als de client haalt u de tijd van laatste synchronisatie wanneer is het ingesteld op de entiteit op *t 5*, het kunt vergelijken met de tijdstempel van de entiteit. Als de tijdstempel van de entiteit hoger dan de tijd van laatste synchronisatie is, klikt u vervolgens de entiteit is een mogelijk inconsistente status en tilt u wat de juiste actie voor uw toepassing is. Met behulp van dit veld is vereist dat u weet wanneer de laatste update naar de primaire is voltooid.

## <a name="testing"></a>Testen

Het is belangrijk om te testen of uw toepassing werkt zoals verwacht, wanneer het herstelbare fouten tegenkomt. Bijvoorbeeld, moet u testen of de schakelopties voor de toepassing naar de secundaire en in de modus alleen-lezen wanneer deze een probleem detecteert en verandert back-ups maken wanneer de primaire regio weer beschikbaar. Om dit te doen, moet u een manier om herstelbare fouten en hoe vaak ze voorkomen besturingselement te simuleren.

U kunt [Fiddler](https://www.telerik.com/fiddler) te onderscheppen en HTTP-antwoorden in een script wijzigen. Met dit script kunt identificeren antwoorden die afkomstig van uw primaire eindpunt zijn en de HTTP-statuscode wijzigen dat de Opslagclientbibliotheek wordt herkend als een herstelbare fout. Dit codefragment toont een eenvoudig voorbeeld van een Fiddler-script waarmee antwoorden om te lezen aanvragen op basis van de **employeedata** tabel een 502 status moet worden geretourneerd:

```java
static function OnBeforeResponse(oSession: Session) {
    ...
    if ((oSession.hostname == "\[yourstorageaccount\].table.core.windows.net")
      && (oSession.PathAndQuery.StartsWith("/employeedata?$filter"))) {
        oSession.responseCode = 502;
    }
}
```

U kunt dit voorbeeld voor een groot aantal aanvragen worden onderschept en alleen wijzigen uitbreiden de **responseCode** op een aantal ze beter een Praktijkscenario te simuleren. Zie voor meer informatie over het aanpassen van Fiddler scripts [wijzigen van een aanvraag of antwoord](https://docs.telerik.com/fiddler/KnowledgeBase/FiddlerScript/ModifyRequestOrResponse) in de documentatie van Fiddler.

Als u de drempelwaarden voor het schakelen tussen uw toepassing in de modus alleen-lezen configureerbare hebt aangebracht, wordt het gemakkelijker om te testen het gedrag met niet-productie-transactie-volumes zijn.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over leestoegang Geo-redundantie, met inbegrip van een ander voorbeeld van hoe de LastSyncTime is ingesteld, neem [redundantieopties voor Windows Azure-opslag en geografisch redundante opslag met leestoegang](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

* Zie voor een compleet voorbeeld van hoe u de switch heen en weer tussen de primaire en secundaire eindpunten, [Azure-voorbeelden: met behulp van het Circuitonderbrekerpatroon met RA-GRS-opslag](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs).
