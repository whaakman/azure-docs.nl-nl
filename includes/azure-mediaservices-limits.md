>[!NOTE]
>Als resources niet vast zijn, kunt u een ondersteuningsticket openen om te vragen of de quota kunnen worden verhoogd. Maak **geen** extra Azure Media Services-accounts in een poging om de limieten te verhogen.

| Resource | Standaardlimiet | 
| --- | --- | 
| AMS-accounts (Azure Media Service) in één abonnement | 25 (vast) |
| Gereserveerde media-eenheden (RU's) per AMS-account |25 (S1)<br/>10 (S2, S3) <sup>(1)</sup> | 
| Taken per AMS-account | 50,000<sup>(2)</sup> |
| Gekoppelde taken per taak | 30 (vast) |
| Activa per AMS-account | 1.000.000|
| Activa per taak | 50 |
| Activa per taak | 100 |
| Unieke locators die aan één activum tegelijk zijn gekoppeld | 5<sup>(4)</sup> |
| Live kanalen per AMS-account |5|
| Programma's met de status Gestopt per kanaal |50|
| Programma's met de status Wordt uitgevoerd per kanaal |3|
| Streaming-eindpunten met de status Wordt uitgevoerd per AMS-account|2|
| Streaming-eenheden per streaming-eindpunt |10 |
| Opslagaccounts | 1.000<sup>(5)</sup> (vast) |
| Beleidsregels | 1,000,000<sup>(6)</sup> |
| Bestandsgrootte| In sommige scenario's, moet u er een limiet is op de maximale bestandsgrootte voor verwerking in Media Services wordt ondersteund. <sup>7</sup> |
  
<sup>1</sup> als u het type (bijvoorbeeld van S2 naar S1) wijzigt, de maximale RU-limieten worden opnieuw ingesteld.

<sup>2</sup> Deze waarde omvat taken in de wachtrij, voltooide taken, actieve taken en geannuleerde taken. Deze bevat geen verwijderde taken. U kunt de oude taken verwijderen met **IJob.Delete** of de HTTP-aanvraag **DELETE**.

Vanaf 1 April 2017 wordt wordt elke taakrecord in uw account die ouder is dan 90 dagen automatisch verwijderd, samen met de bijbehorende onderliggende Taakrecords, zelfs als het totale aantal records lager dan het maximale quotum is. Als u de gegevens van de taak wilt archiveren, kunt u de code gebruiken die [hier](../articles/media-services/previous/media-services-dotnet-manage-entities.md) wordt beschreven.

<sup>3</sup> bij het maken van een aanvraag om taakentiteiten weer te entiteiten, maximaal 1000 taken per aanvraag wordt geretourneerd. Als u alle ingediende taken moet bijhouden, kunt u Populaire/Overslaan gebruiken zoals wordt beschreven in [Queryopties van het OData-systeem](http://msdn.microsoft.com/library/gg309461.aspx).

<sup>4</sup> Locators zijn niet ontworpen om toegangsbeheer per gebruiker te regelen. Gebruik DRM-oplossingen (Digital Rights Management) als u afzonderlijke gebruikers verschillende toegangsrechten wilt geven. Zie [deze](../articles/media-services/previous/media-services-content-protection-overview.md) sectie voor meer informatie.

<sup>5</sup> De opslagaccounts moeten deel uitmaken van hetzelfde Azure-abonnement.

<sup>6</sup> Er geldt een limiet van 1.000.000 beleidsregels voor verschillende AMS-beleidsitems (bijvoorbeeld voor Locator-beleid of ContentKeyAuthorizationPolicy). 

>[!NOTE]
> Gebruik dezelfde beleids-id als u altijd dezelfde dagen/toegangsmachtigingen/enzovoort gebruikt. Zie [dit](../articles/media-services/previous/media-services-dotnet-manage-entities.md#limit-access-policies) gedeelte voor informatie en een voorbeeld.

<sup>7</sup>als u inhoud naar een activum in Azure Media Services uploadt te verwerken met een van de mediaprocessors van de service (dat wil zeggen, zoals encoders zoals Media Encoder Standard en Media Encoder Premium Workflow, of analyse-engines Face Detector), Vervolgens moet u rekening houden met de beperkingen met betrekking tot de maximale bestandsgrootte die wordt ondersteund. 

De maximale grootte voor één blob wordt ondersteund is momenteel maximaal 5 TB in Azure Blob Storage. Aanvullende beperkingen gelden echter in Azure Media Services op basis van de VM-grootten die worden gebruikt door de service. De volgende tabel bevat de limieten op elk van de gereserveerde Media-eenheden (S1, S2, S3) Als de bronbestand groter dan de limieten die zijn gedefinieerd in de tabel is, mislukt de coderingstaak. Als u 4K resolutie bronnen van lange duur van de versleutelt, moet u gereserveerde Media-eenheden S3 gebruiken om de prestaties die nodig zijn. Hebt u 4K-inhoud die groter is dan de limiet van 260 GB van de S3 gereserveerde Media-eenheden, contact met ons op amshelp@microsoft.com voor mogelijke oplossingen ter ondersteuning van uw scenario.

| Type gereserveerde media-eenheid | Maximale invoer grootte (GB)| 
| --- | --- | 
|S1 | 325|
|S2 | 640|
|S3 | 260|
