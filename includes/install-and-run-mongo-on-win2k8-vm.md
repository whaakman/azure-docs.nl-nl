Volg deze stappen voor het installeren en uitvoeren van MongoDB op een virtuele machine met Windows Server.

> [!IMPORTANT]
> Beveiligingsfuncties van MongoDB, zoals verificatie en IP-adresbinding zijn niet standaard ingeschakeld. Beveiligingsfuncties moeten worden ingeschakeld voordat u MongoDB in een productieomgeving implementeert.  Zie voor meer informatie [beveiligings- en](http://www.mongodb.org/display/DOCS/Security+and+Authentication).
>
>

1. Nadat u verbinding hebt gemaakt met de virtuele machine via Extern bureaublad, opent u Internet Explorer vanuit de **Start** menu op de virtuele machine.
2. Selecteer de **extra** knop in de rechterbovenhoek.  In **Internetopties**, selecteer de **beveiliging** tabblad en selecteer vervolgens de **vertrouwde websites** pictogram en klik tot slot de **Sites** knop. Voeg *https://\*. mongodb.org* aan de lijst met vertrouwde sites.
3. Ga naar [Downloads - MongoDB](https://www.mongodb.com/download-center#community).
4. Zoeken naar de **huidige stabiele Release** van **Community Server**, selecteert u de meest recente **64-bits** versie in de Windows-kolom. Download en voer vervolgens het installatieprogramma MSI.
5. MongoDB wordt doorgaans geïnstalleerd in C:\Program Files\MongoDB. Omgevingsvariabelen zoeken op het bureaublad en het pad van de binaire bestanden MongoDB toevoegen aan de variabele PATH. U kunt bijvoorbeeld de binaire bestanden vinden op C:\Program Files\MongoDB\Server\3.4\bin op uw computer.
6. MongoDB-gegevens en logboekbestanden mappen maken in de gegevensschijf (zoals station **F:**) u in de voorgaande stappen hebt gemaakt. Van **Start**, selecteer **opdrachtprompt** een opdrachtpromptvenster te openen.  Type:

        C:\> F:
        F:\> mkdir \MongoData
        F:\> mkdir \MongoLogs
7. Uit het volgende als u de database uit te voeren:

        F:\> C:
        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log

    Alle berichten in het logboek worden omgeleid naar de *F:\MongoLogs\mongolog.log* bestand zoals mongod.exe server wordt gestart en preallocates journaal-bestanden. Het kan enkele minuten voor MongoDB wijzen de journaal-bestanden en beginnen met luisteren voor verbindingen duren. De opdrachtprompt blijft gericht zijn op deze taak terwijl het MongoDB-exemplaar wordt uitgevoerd.
8. Voor het starten van de administratieve MongoDB-shell, opent u een andere opdrachtvenster van **Start** en typ de volgende opdrachten:

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

    De database wordt gemaakt door de insert.
9. U kunt ook mongod.exe installeren als een service:

        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install

    Een service is geïnstalleerd met de naam MongoDB met een beschrijving van "DB met Mongo". De `--logpath` optie moet worden gebruikt om op te geven van een logboekbestand, omdat de service heeft geen een opdrachtvenster om uitvoer weer te geven.  De `--logappend` optie geeft u de uitvoer toe te voegen aan het bestaande bestand zorgt ervoor dat de service opnieuw wordt opgestart.  De `--dbpath` optie geeft u de locatie van de map data. Zie voor meer service-gerelateerde opdrachtregelopties, [opdrachtregelopties voor het Service-gerelateerde][MongoWindowsSvcOptions].

    De service wilt starten, moet u deze opdracht uitvoeren:

        C:\> net start MongoDB
10. Nu dat MongoDB is geïnstalleerd en actief is, u een poort openen in Windows Firewall moet zodat u op afstand verbinding met MongoDB maken kunt.  Van de **Start** selecteert u **Systeembeheer** en vervolgens **Windows Firewall met geavanceerde beveiliging**.
11. een) Selecteer in het linkerdeelvenster **regels voor binnenkomende verbindingen**.  In de **acties** deelvenster op de juiste, selecteer **nieuwe regel...** .

    ![Windows Firewall][Image1]

    b) in de **nieuwe Wizard regel voor binnenkomende**, selecteer **poort** en klik vervolgens op **volgende**.

    ![Windows Firewall][Image2]

    c) Selecteer **TCP** en vervolgens **specifieke lokale poorten**.  Geef een '27017' (de standaardpoort MongoDB luistert op)-poort en klik op **volgende**.

    ![Windows Firewall][Image3]

    d) Selecteer **de verbinding toestaan** en klik op **volgende**.

    ![Windows Firewall][Image4]

    e) Klik op **volgende** opnieuw.

    ![Windows Firewall][Image5]

    f) Geef een naam voor de regel, zoals 'MongoPort', en klik op **voltooien**.

    ![Windows Firewall][Image6]

12. Als u niet een eindpunt voor MongoDB configureren wanneer u de virtuele machine hebt gemaakt, kunt u dit nu doen. U moet zowel de firewallregel en het eindpunt te kunnen op afstand verbinding maken met MongoDB.

  Klik in de Azure-portal op **virtuele Machines (klassiek)**, klik op de naam van uw nieuwe virtuele machine en klik vervolgens op **eindpunten**.

    ![Eindpunten][Image7]

13. Klik op **Add**.

14. Toevoegen van een eindpunt met de naam 'Mongo', protocol **TCP**, en beide **openbare** en **persoonlijke** poorten ingesteld op '27017'. Deze poort opent, kunt MongoDB naar extern toegankelijk is.

    ![Eindpunten][Image9]

> [!NOTE]
> De poort 27017 is de standaardpoort gebruikt door MongoDB. U kunt deze standaardpoort wijzigen door op te geven de `--port` parameter bij het starten van de server mongod.exe. Zorg ervoor dat hetzelfde poortnummer in de firewall en het eindpunt 'Mongo' in de voorgaande instructies te bieden.
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
