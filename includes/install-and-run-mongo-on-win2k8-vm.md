Volg deze stappen voor het installeren en uitvoeren van MongoDB op een virtuele machine met Windows Server.

> [!IMPORTANT]
> MongoDB-beveiligingsfuncties, zoals een verificatie- en IP-adresbinding worden niet standaard ingeschakeld. Beveiligingsfuncties die moeten worden ingeschakeld voordat u MongoDB in een productieomgeving implementeert.  Zie voor meer informatie, [beveiligings- en Authenticatieservices](http://www.mongodb.org/display/DOCS/Security+and+Authentication).
>
>

1. Nadat u verbinding hebt gemaakt met de virtuele machine via Extern bureaublad, opent u Internet Explorer vanaf het **Start** menu aan de virtuele machine.
2. Selecteer de **extra** knop in de rechterbovenhoek.  In **Internetopties**, selecteer de **Security** tabblad, en selecteer vervolgens de **vertrouwde Sites** pictogram, en klik tot slot de **Sites** knop. Voeg *https://\*. mongodb.org* aan de lijst met vertrouwde sites.
3. Ga naar [Downloads - MongoDB](https://www.mongodb.com/download-center#community).
4. Zoek de **huidige stabiele versie** van **Community Server**, selecteert u de meest recente **64-bits** versie in de Windows-kolom. Voer het MSI-installatieprogramma vervolgens downloaden.
5. MongoDB wordt doorgaans geïnstalleerd in C:\Program Files\MongoDB. Omgevingsvariabelen zoeken op het bureaublad en het pad voor de MongoDB-binaire bestanden toevoegen aan de variabele PATH. U kunt bijvoorbeeld de binaire bestanden vinden op C:\Program Files\MongoDB\Server\3.4\bin op uw computer.
6. MongoDB-gegevens en logboekbestanden mappen maken in de gegevensschijf (zoals station **F:**) u in de voorgaande stappen hebt gemaakt. Van **Start**, selecteer **opdrachtprompt** om een opdrachtprompt-venster te openen.  Type:

        C:\> F:
        F:\> mkdir \MongoData
        F:\> mkdir \MongoLogs
7. Als u wilt uitvoeren van de database, voert u de volgende uit:

        F:\> C:
        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log

    Alle berichten in het logboek worden doorgestuurd naar de *F:\MongoLogs\mongolog.log* bestand zoals mongod.exe server wordt gestart en preallocates logboekbestanden. Het duurt enkele minuten voor MongoDB vooraf toewijzen van de logboekbestanden en begin met het luisteren naar verbindingen. De opdrachtprompt blijven onze aandacht richten op deze taak terwijl uw MongoDB-exemplaar dat wordt uitgevoerd.
8. Open een nieuw opdrachtvenster uit voor het starten van de administratieve MongoDB-shell, **Start** en typ de volgende opdrachten:

        C:\> cd \my_mongo_dir\bin  
        C:\my_mongo_dir\bin> mongo  
        >db  
        test
        > db.foo.insert( { a : 1 } )  
        > db.foo.find()  
        { _id : ..., a : 1 }  
        > show dbs  
        ...  
        > show collections  
        ...  
        > help  

    De database is gemaakt door de insert.
9. U kunt ook mongod.exe installeren als een service:

        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install

    Een service is geïnstalleerd met de naam MongoDB met een beschrijving van "Mongo DB". De `--logpath` optie moet worden gebruikt om op te geven van een logboekbestand, omdat de service heeft geen een opdrachtvenster om uitvoer weer te geven.  De `--logappend` optie geeft u de uitvoer toe te voegen aan het bestaande bestand zorgt ervoor dat de service opnieuw worden opgestart.  De `--dbpath` optie geeft u op de locatie van de map voor gegevens. Zie voor meer service-gerelateerde opdrachtregelopties, [opdrachtregelopties Service gerelateerde][MongoWindowsSvcOptions].

    De service wilt starten, moet u deze opdracht uitvoeren:

        C:\> net start MongoDB
10. MongoDB is geïnstalleerd en uitgevoerd, moet u een poort in Windows Firewall te openen, zodat u op afstand verbinding met MongoDB maken kunt.  Uit de **Start** in het menu **Systeembeheer** en vervolgens **Windows Firewall met geavanceerde beveiliging**.
11. een) Selecteer in het linkerdeelvenster **regels voor binnenkomende verbindingen**.  In de **acties** deelvenster op de juiste, selecteer **nieuwe regel...** .

    ![Windows Firewall][Image1]

    (b) in de **inkomende Wizard nieuwe regel**, selecteer **poort** en klik vervolgens op **volgende**.

    ![Windows Firewall][Image2]

    c) Selecteer **TCP** en vervolgens **specifieke lokale poorten**.  Geef een poort voor "27017" (de standaardpoort luistert voor MongoDB) en klik op **volgende**.

    ![Windows Firewall][Image3]

    d) Selecteer **de verbinding toestaan** en klikt u op **volgende**.

    ![Windows Firewall][Image4]

    e) Klik op **volgende** opnieuw.

    ![Windows Firewall][Image5]

    f) Geef een naam voor de regel, zoals 'MongoPort', en klikt u op **voltooien**.

    ![Windows Firewall][Image6]

12. Als u niet een eindpunt voor MongoDB configureert tijdens het maken van de virtuele machine, kunt u dit nu doen. Moet u zowel de firewallregel en het eindpunt kunnen op afstand verbinding maken met MongoDB.

  Klik in de Azure-portal op **virtuele Machines (klassiek)**, klikt u op de naam van uw nieuwe virtuele machine en klik vervolgens op **eindpunten**.

    ![Eindpunten][Image7]

13. Klik op **Add**.

14. Toevoegen van een eindpunt met de naam 'Mongo', protocol **TCP**, en beide **openbare** en **persoonlijke** poorten ingesteld op '27017'. Deze poort te openen, kunt MongoDB op afstand worden geopend.

    ![Eindpunten][Image9]

> [!NOTE]
> De poort 27017 is de standaardpoort gebruikt met MongoDB. U kunt deze standaardpoort wijzigen door op te geven de `--port` parameter bij het starten van de mongod.exe-server. Zorg ervoor dat u hetzelfde poortnummer in de firewall en het eindpunt 'Mongo' in de voorgaande instructies te geven.
>
>

[MongoDownloads]: http://www.mongodb.org/downloads

[MongoWindowsSvcOptions]: http://www.mongodb.org/display/DOCS/Windows+Service


[Image1]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall1.png
[Image2]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall2.png
[Image3]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall3.png
[Image4]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall4.png
[Image5]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall5.png
[Image6]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall6.png
[Image7]: ./media/install-and-run-mongo-on-win2k8-vm/menusendpointadd.png
<!-- Removed 03/08/2017. Not in new portal. -->
<!-- [Image8]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint2.png
-->
[Image9]: ./media/install-and-run-mongo-on-win2k8-vm/newendpointdetails.png
