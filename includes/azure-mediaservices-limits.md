>[!NOTE]
>Als resources niet vast zijn, kunt u een ondersteuningsticket openen om te vragen of de quota kunnen worden verhoogd. Maak **geen** extra Azure Media Services-accounts in een poging om de limieten te verhogen.

| Resource | Standaardlimiet | 
| --- | --- | 
| AMS-accounts (Azure Media Service) in één abonnement | 25 (vast) |
| Activa per AMS-account | 1.000.000|
| Gekoppelde taken per taak | 30 (vast) |
| Activa per taak | 50 |
| Activa per taak | 100 |
| Taken per AMS-account | 50.000<sup>2</sup> |
| Unieke locators die aan één activum tegelijk zijn gekoppeld | 5<sup>4</sup> |
| Live kanalen per AMS-account |5|
| Programma's met de status Gestopt per kanaal |50|
| Programma's met de status Wordt uitgevoerd per kanaal |3|
| Streaming-eindpunten met de status Wordt uitgevoerd per AMS-account|2|
| Streaming-eenheden per streaming-eindpunt |10 |
| Gereserveerde media-eenheden (RU's) per AMS-account |25 (S1, S2)<br/>10 (S3) <sup>1</sup> | 
| Opslagaccounts | 1.000<sup>5</sup> (vast) |
| Beleidsregels | 1,000,000<sup>6</sup> |
| Bestandsgrootte| In sommige scenario's is er een limiet voor de maximale bestandsgrootte die wordt ondersteund voor verwerking in Media Services. <sup>7</sup> |
 
<sup>1</sup> S3-RU's zijn niet beschikbaar in India - west.

<sup>2</sup> Deze waarde omvat taken in de wachtrij, voltooide taken, actieve taken en geannuleerde taken. Deze bevat geen verwijderde taken. U kunt de oude taken verwijderen met **IJob.Delete** of de HTTP-aanvraag **DELETE**.

<sup>3</sup> Bij het maken van een aanvraag om taakentiteiten weer te geven, worden maximaal 1000 entiteiten per aanvraag geretourneerd. Als u alle ingediende taken moet bijhouden, kunt u Populaire/Overslaan gebruiken zoals wordt beschreven in [Queryopties van het OData-systeem](http://msdn.microsoft.com/library/gg309461.aspx).

<sup>4</sup> Locators zijn niet ontworpen om toegangsbeheer per gebruiker te regelen. Gebruik DRM-oplossingen (Digital Rights Management) als u afzonderlijke gebruikers verschillende toegangsrechten wilt geven. Zie [deze](../articles/media-services/media-services-content-protection-overview.md) sectie voor meer informatie.

<sup>5</sup> De opslagaccounts moeten deel uitmaken van hetzelfde Azure-abonnement.

<sup>6</sup> Er geldt een limiet van 1.000.000 beleidsregels voor verschillende AMS-beleidsitems (bijvoorbeeld voor Locator-beleid of ContentKeyAuthorizationPolicy). 

>[!NOTE]
> Gebruik dezelfde beleids-id als u altijd dezelfde dagen/toegangsmachtigingen/enzovoort gebruikt.

<sup>7</sup>Als u inhoud uploadt naar een activum in Azure Media Services met de bedoeling het te verwerken met een van de mediaprocessors in onze service (d.w.z. encoders zoals Media Encoding Standard en Media Encoder Premium Workflow, of analyse-engines als Face Detector), dan moet u rekening houden met de volgende limieten. 

| Type gereserveerde media-eenheid | Maximale bestandsgrootte (GB)| 
| --- | --- | 
|S1 | 325|
|S2 | 640|
|S3 | 260|


<!--HONumber=Feb17_HO1-->


