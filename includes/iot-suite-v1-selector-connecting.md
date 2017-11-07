> [!div class="op_single_selector"]
> * [C op Windows](../articles/iot-suite/iot-suite-v1-connecting-devices.md)
> * [C op Linux](../articles/iot-suite/iot-suite-v1-connecting-devices-linux.md)
> * [Node.js](../articles/iot-suite/iot-suite-v1-connecting-devices-node.md)
> 
> 

## <a name="scenario-overview"></a>Overzicht van scenario's
In dit scenario maakt u een apparaat dat de volgende telemetrie verzendt naar de [vooraf geconfigureerde oplossing][lnk-what-are-preconfig-solutions] voor externe controle:

* Externe temperatuur
* Interne temperatuur
* Vochtigheid

Om het eenvoudig te houden, genereert de code op het apparaat voorbeeldwaarden, maar we moedigen u aan om het voorbeeld uit te breiden door echte sensoren met het apparaat te verbinden en echte telemetrie te verzenden.

Het apparaat kan ook reageren op methoden die zijn aangeroepen vanuit het oplossingsdashboard en op waarden van gewenste eigenschappen die in het oplossingsdashboard zijn ingesteld.

U hebt een actief Azure-account nodig om deze zelfstudie te voltooien. Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure][lnk-free-trial] voor meer informatie.

## <a name="before-you-start"></a>Voordat u begint
Voordat u code voor het apparaat gaat schrijven, moet u de vooraf geconfigureerde oplossing voor externe controle inrichten en een nieuw aangepast apparaat in die oplossing inrichten.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>De vooraf geconfigureerde oplossing voor externe controle inrichten
Het apparaat dat u in deze zelfstudie maakt, verzendt gegevens naar een exemplaar van de vooraf geconfigureerde oplossing voor [externe controle][lnk-remote-monitoring]. Als u de vooraf geconfigureerde oplossing voor externe controle in uw Azure-account nog niet hebt ingericht, voer dan de volgende stappen uit:

1. Klik op de pagina <https://www.azureiotsuite.com/> op **+** om een oplossing te maken.
2. Klik in het deelvenster **Externe controle** op **Selecteren** om de oplossing te maken.
3. Voer op de pagina **Oplossing voor externe controle maken** een zelfgekozen **Oplossingsnaam** in, selecteer de **Regio** waarin u deze wilt implementeren en selecteer het Azure-abonnement dat u wilt gebruiken. Klik vervolgens op **Oplossing maken**.
4. Wacht tot het inrichtingsproces is voltooid.

> [!WARNING]
> De vooraf geconfigureerde oplossingen maken gebruik van factureerbare Azure-services. Zorg dat u de vooraf geconfigureerde oplossing uit uw abonnement verwijdert wanneer u klaar bent, om overbodige kosten te voorkomen. U kunt een vooraf geconfigureerde oplossing volledig uit uw abonnement verwijderen door naar de pagina <https://www.azureiotsuite.com/> te gaan.
> 
> 

Wanneer het inrichtingsproces voor de oplossing voor externe controle is voltooid, klikt u op **Starten** om het dashboard van de oplossing in uw browser te openen.

![Dashboard van de oplossing][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Het apparaat inrichten in de oplossing voor externe controle
> [!NOTE]
> Als u al een apparaat in uw oplossing hebt ingericht, kunt u deze stap overslaan. U moet de referenties van het apparaat weten wanneer u de clienttoepassing maakt.
> 
> 

Een apparaat kan alleen verbinding maken met de vooraf geconfigureerde oplossing als het zichzelf met geldige referenties kan identificeren bij IoT Hub. U kunt de apparaatreferenties ophalen via het dashboard van de oplossing. Verderop in deze zelfstudie neemt u de referenties van het apparaat op in de clienttoepassing.

Als u een apparaat aan de oplossing voor externe controle wilt toevoegen, voert u de volgende stappen uit in het dashboard van de oplossing:

1. Klik in de linkerbenedenhoek van het dashboard op **Een apparaat toevoegen**.
   
   ![Een apparaat toevoegen][1]
2. Klik in het deelvenster **Aangepast apparaat** op **Nieuw toevoegen**.
   
   ![Een aangepast apparaat toevoegen][2]
3. Kies **Laat mij mijn eigen apparaat-id definiëren**. Voer een apparaat-id in, bijvoorbeeld **mijnapparaat**, en klik op **Id controleren** om te controleren of de naam nog niet wordt gebruikt. Klik tot slot op **Maken** om het apparaat in te richten.
   
   ![Apparaat-id toevoegen][3]
4. Noteer de referenties van het apparaat (apparaat-id, hostnaam van IoT Hub en apparaatsleutel). De clienttoepassing heeft deze waarden nodig om verbinding te maken met de oplossing voor externe controle. Klik vervolgens op **Gereed**.
   
    ![Apparaatreferenties weergeven][4]
5. Selecteer uw apparaat in de lijst met apparaten in het oplossingsdashboard. Klik vervolgens in het deelvenster **Apparaatdetails** op **Apparaat inschakelen**. De status van het apparaat is nu **Wordt uitgevoerd**. De oplossing voor externe controle kan nu telemetrie ontvangen van uw apparaat en methoden op het apparaat aanroepen.

[img-dashboard]: ./media/iot-suite-v1-selector-connecting/dashboard.png
[1]: ./media/iot-suite-v1-selector-connecting/suite0.png
[2]: ./media/iot-suite-v1-selector-connecting/suite1.png
[3]: ./media/iot-suite-v1-selector-connecting/suite2.png
[4]: ./media/iot-suite-v1-selector-connecting/suite3.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-v1-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-v1-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/