## <a name="view-device-telemetry-in-the-dashboard"></a>Telemetrie van apparaten weergeven in het dashboard
Via het dashboard van de oplossing voor externe controle kunt u de telemetrie bekijken die uw apparaten naar IoT Hub verzenden.

1. Ga in de browser terug naar het dashboard van de oplossing voor externe controle en klik in het linkerdeelvenster op **Apparaten** om naar de **Lijst met apparaten** te navigeren.
2. In de **Lijst met apparaten** zou de status van uw apparaat nu **Wordt uitgevoerd** moeten zijn. Als dat niet zo is, klikt u in het deelvenster **Apparaatdetails** op **Apparaat inschakelen**.
   
    ![Apparaatstatus weergeven][18]
3. Klik op **Dashboard** om terug te keren naar het dashboard en selecteer het apparaat in de vervolgkeuzelijst **Weer te geven apparaat** om de telemetrie ervan weer te geven. De telemetrie uit de voorbeeldtoepassing is 50 eenheden voor de interne temperatuur, 55 eenheden voor de externe temperatuur en 50 eenheden voor de vochtigheid.
   
    ![Telemetrie van apparaten weergeven][img-telemetry]

## <a name="invoke-a-method-on-your-device"></a>Een methode op het apparaat aanroepen
Via het dashboard van de oplossing voor externe controle kunt u methoden op uw apparaten aanroepen via IoT Hub. U kunt in de oplossing voor externe controle bijvoorbeeld een methode aanroepen om het opnieuw opstarten van een apparaat te simuleren.

1. Klik op het dashboard van de oplossing voor externe controle in het linkerdeelvenster op **Apparaten** om naar de **Lijst met apparaten** te navigeren.
2. Klik in de **Lijst met apparaten** op **Apparaat-id** voor uw apparaat.
3. Klik in het deelvenster **Apparaatdetails** op **Methoden**.
   
    ![Apparaatmethoden][13]
4. Selecteer in de vervolgkeuzelijst **Methode** de methode **InitiateFirmwareUpdate** en voer in **FWPACKAGEURI** een dummy URL in. Klik op **Methode aanroepen** om de methode op het apparaat aan te roepen.
   
    ![Een apparaatmethode aanroepen][14]
   

5. In de console waarin de apparaatcode wordt uitgevoerd, wordt een bericht weergegeven wanneer het apparaat de methode afhandelt. De resultaten van de methode worden toegevoegd aan de geschiedenis in de portal van de oplossing:

    ![Geschiedenis van methoden weergeven][img-method-history]

## <a name="next-steps"></a>Volgende stappen
In het artikel [Customizing preconfigured solutions][lnk-customize] (Vooraf geconfigureerde oplossingen aanpassen) worden enkele manieren beschreven waarop u dit voorbeeld kunt uitbreiden. Mogelijke uitbreidingen zijn het gebruik van echte sensoren en de implementatie van aanvullende opdrachten.

Zie [Permissions on the azureiotsuite.com site][lnk-permissions] (Machtigingen op de site azureiotsuite.com) voor meer informatie.

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[img-method-history]: ./media/iot-suite-visualize-connecting/history.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md
