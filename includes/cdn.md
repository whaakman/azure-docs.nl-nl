# <a name="using-cdn-for-azure"></a>Gebruik van CDN voor Azure
Azure inhoud Delivery Network (CDN) biedt ontwikkelaars een globale oplossing voor hoge bandbreedte inhoud leveren door blobs en statische inhoud van de compute-exemplaren op fysieke knooppunten in de Verenigde Staten, Europa, Azië, Australië en Zuid-Amerika cache te plaatsen. Zie voor een huidige lijst van CDN-knooppuntlocaties [Azure CDN-Knooppuntlocaties].

Deze taak omvat de volgende stappen uit:

* [Stap 1: Een opslagaccount maken](#Step1)
* [Stap 2: Maak een nieuw CDN-eindpunt voor uw opslagaccount](#Step2)
* [Stap 3: Toegang tot uw CDN-inhoud](#Step3)
* [Stap 4: Verwijder uw CDN-inhoud](#Step4)

De voordelen van het gebruik van CDN in de cache van de Azure-gegevens zijn:

* Betere prestaties en gebruikerservaring voor eindgebruikers die ver van de inhoudsbron en toepassingen worden gebruikt waar veel internet reizen zijn vereist om inhoud te laden
* Gedistribueerde grootschalige beter afhandelen korte hoge belasting, bijvoorbeeld aan het begin van een gebeurtenis, zoals het starten van een product

Bestaande CDN-klanten kunnen nu gebruikmaken van Azure CDN in de [klassieke Azure-portal]. De CDN is een functie van de invoegtoepassing aan uw abonnement en een afzonderlijke [abonnement].

<a id="Step1"> </a>

<h2>Stap 1: Een opslagaccount maken</h2>

Gebruik de volgende procedure voor het maken van een nieuw opslagaccount voor een Azure-abonnement. Een opslagaccount biedt toegang tot Azure storage-services. Het opslagaccount vertegenwoordigt het hoogste niveau van de naamruimte voor het openen van elk van de onderdelen van de Azure storage-service: Blob-services, wachtrijservices en services van de tabel. Zie voor meer informatie over de Azure storage-services, [met behulp van de Azure Storage-Services](http://msdn.microsoft.com/library/azure/gg433040.aspx).

Voor het maken van een opslagaccount, moet u de servicebeheerder of medebeheerder voor het gekoppelde abonnement.

> [!NOTE]
> Zie voor meer informatie over het uitvoeren van deze bewerking via de Azure Service Management API de [Storage-Account maken](http://msdn.microsoft.com/library/windowsazure/hh264518.aspx) naslagonderwerp.
> 
> 

**Een opslagaccount voor een Azure-abonnement maken**

1. Meld u aan bij de [klassieke Azure-portal].
2. Klik in de linkerbenedenhoek op **nieuw**. In de **nieuw** dialoogvenster **Data Services**, klikt u vervolgens op **opslag**, klikt u vervolgens **snelle invoer**.
   
   De **Storage-Account maken** dialoogvenster wordt weergegeven.
   
   ![Storage-Account maken][create-new-storage-account]
3. In de **URL** veld, typt u de subdomeinnaam. Deze vermelding kan van 3 tot 24 kleine letters en cijfers bevatten.
   
    Deze waarde wordt de hostnaam van de in de URI die wordt gebruikt om Blob, wachtrij of tabel bronnen voor het abonnement. Om een container-bron in de Blob-service op te lossen, zou u een URI gebruikt in de volgende indeling waar  *&lt;StorageAccountLabel&gt;*  verwijst naar de waarde die u hebt opgegeven in **Voer een URL**:
   
    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt;*
   
    **Belangrijk:** de URL label formulieren het subdomein van de URI van het opslagaccount en moet uniek zijn alle gehoste services in Azure.
   
    Deze waarde wordt ook gebruikt als de naam van dit opslagaccount in de portal of bij het openen van dit account via een programma.
4. Van de **regio/Affiniteitsgroep** vervolgkeuzelijst, selecteer een regio of affiniteit groep voor het opslagaccount. Selecteer een affiniteitsgroep in plaats van een regio als u wilt dat uw storage-services zich in hetzelfde datacenter met andere Windows Azure-services die u gebruikt. Kan dit de prestaties te verbeteren en er zijn geen kosten voor uitgaande verbonden zijn.  
   
   **Opmerking:** voor het maken van een affiniteitsgroep, opent u de **instellingen** gebied van de beheerportal, klikt u op **Affiniteitsgroepen**, en klikt u ofwel **een affiniteitsgroeptoevoegen** of **toevoegen**. U kunt ook maken en beheren met de Windows Azure Service Management API affiniteitsgroepen. Zie voor meer informatie [bewerkingen voor Affiniteitsgroepen].
5. Van de **abonnement** vervolgkeuzelijst, selecteert u het abonnement dat het storage-account wordt gebruikt met.
6. Klik op **Opslagaccount maken**. Het proces van het maken van het opslagaccount kan enkele minuten duren.
7. Om te controleren of het opslagaccount is gemaakt, Controleer of het account wordt weergegeven in de items die worden vermeld voor **opslag** met de status van **Online**.

<a id="Step2"> </a>

<h2>Stap 2: Maak een nieuw CDN-eindpunt voor uw opslagaccount</h2>

Zodra u CDN toegang tot een opslagaccount of de gehoste service, zijn alle openbaar beschikbare objecten in aanmerking komen voor CDN edge cache. Als u een object dat momenteel in cache in de CDN wijzigt, de nieuwe inhoud wordt niet meer beschikbaar via de CDN totdat de CDN wordt de inhoud vernieuwd wanneer het in cache opgeslagen inhoud time-to-live-periode is verstreken.

**Een nieuw CDN-eindpunt voor uw opslagaccount maken**

1. In de [klassieke Azure-portal], klik in het navigatievenster op **CDN**.
2. Klik op het lint op **nieuw**. In de **nieuw** dialoogvenster, selecteer **App Services**, klikt u vervolgens **CDN**, klikt u vervolgens **snelle invoer**.
3. In de **brondomein** vervolgkeuzelijst, selecteer de opslag account u hebt gemaakt in de vorige sectie in de lijst van uw accounts beschikbare opslag. 
4. Klik op de **maken** knop om het nieuwe eindpunt te maken.
5. Zodra het eindpunt is gemaakt, wordt deze weergegeven in een lijst met eindpunten voor het abonnement. In de lijstweergave kunt u zien welke URL u moet gebruiken voor toegang tot de content in cache en het brondomein. 
   
    Het brondomein is de locatie van waaruit de CDN inhoud slaat. Het brondomein kan bestaan uit een opslagaccount of een cloudservice; een opslagaccount wordt gebruikt voor de doeleinden van dit voorbeeld. Edge-servers op basis van een cache-control-instelling die u opgeeft, of de standaard methodiek van het netwerk opslaan in cache opgeslagen inhoud opslag in. 

    > [AZURE.NOTE]De configuratie gemaakt voor het eindpunt zal niet onmiddellijk beschikbaar zijn; het kan duren voordat tot 60 minuten voor de registratie worden doorgegeven via het netwerk CDN. Totdat de inhoud beschikbaar via de CDN is, kunnen gebruikers die zich willen gebruiken de naam van het CDN-domein onmiddellijk statuscode 400 (ongeldige aanvraag) ontvangen.

<a id="Step3"> </a>

<h2>Stap 3: Toegang tot CDN-inhoud</h2> 

Gebruik voor toegang tot de inhoud in cache op de CDN moet de CDN-URL in de portal. Het adres van een blob in de cache is vergelijkbaar met het volgende:

http://<*CDNNamespace*\>.vo.msecnd.net/ <*myPublicContainer*\>/<*BlobName*\>

<a id="Step4"> </a>

<h2>Stap 4: Verwijder de inhoud uit CDN</h2>

Als u niet langer in de cache van een object in de Azure Content Delivery Network (CDN) wenst, kunt u een van de volgende stappen uitvoeren:

* U kunt de blob verwijderen uit de openbare container voor een Azure-blob.
* Kunt u de container private in plaats van openbaar. Zie [toegang beperken tot Containers en Blobs](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/#restrict-access-to-containers-and-blobs) voor meer informatie.
* U kunt uitschakelen of verwijderen van het CDN-eindpunt met behulp van de beheerportal.
* U kunt uw gehoste service niet meer reageren op aanvragen voor het object wijzigen.

Een object dat al in de cache in de CDN blijft in de cache, totdat de time-to-live voor het object zijn verstreken. Wanneer de time-to-live-periode is verstreken, wordt de CDN gecontroleerd om te zien of het CDN-eindpunt nog geldig is en het object nog steeds anoniem toegankelijk. Als dit niet het geval is, klikt u vervolgens het object wordt niet langer in de cache opgeslagen.

De mogelijkheid om op te schonen onmiddellijk inhoud wordt momenteel niet ondersteund in Azure Management Portal. Neem contact op met [ondersteuning van Azure](https://azure.microsoft.com/support/options/) als u nodig hebt om op te schonen onmiddellijk inhoud. 

## <a name="additional-resources"></a>Aanvullende bronnen
* [Een Affiniteitsgroep maken in Azure]
* [Het: Storage-Accounts beheren voor een Azure-abonnement]
* [Over de servicebeheer-API]
* [CDN-inhoud toewijzen aan een aangepast domein]

[Create Storage Account]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/
[Azure CDN-Knooppuntlocaties]: http://msdn.microsoft.com/library/windowsazure/gg680302.aspx
[klassieke Azure-portal]: https://manage.windowsazure.com/
[abonnement]: /pricing/calculator/?scenario=full
[Een Affiniteitsgroep maken in Azure]: http://msdn.microsoft.com/library/azure/ee460798.aspx
[Overview of the Azure CDN]: http://msdn.microsoft.com/library/windowsazure/ff919703.aspx
[Over de servicebeheer-API]: http://msdn.microsoft.com/library/windowsazure/ee460807.aspx
[CDN-inhoud toewijzen aan een aangepast domein]: http://msdn.microsoft.com/library/windowsazure/gg680307.aspx


[create-new-storage-account]: ./media/cdn/CDN_CreateNewStorageAcct.png
[Previous Management Portal]: ../../Shared/Media/previous-portal.png
