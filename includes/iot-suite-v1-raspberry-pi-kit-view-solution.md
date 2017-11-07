## <a name="view-the-solution-dashboard"></a>Het oplossingsdashboard bekijken

Vanaf het dashboard van de oplossing kunt u de geïmplementeerde oplossing beheren. U kunt bijvoorbeeld telemetrie weergeven, apparaten toevoegen en methoden aanroepen.

1. Wanneer het inrichten is voltooid en voor de tegel voor uw vooraf geconfigureerde oplossing de status **Gereed** wordt weergegeven, kiest u **Starten** om uw oplossingsportal voor externe controle te openen op een nieuw tabblad.

    ![De vooraf geconfigureerde oplossing starten][img-launch-solution]

1. Standaard ziet u in de oplossingsportal het *dashboard*. U kunt navigeren naar andere gebieden van de portal van de oplossing met het menu aan de linkerkant van de pagina.

    ![Vooraf geconfigureerd oplossingsdashboard voor externe controle][img-menu]

## <a name="add-a-device"></a>Een apparaat toevoegen

Een apparaat kan alleen verbinding maken met de vooraf geconfigureerde oplossing als het zichzelf met geldige referenties kan identificeren bij IoT Hub. U kunt de apparaatreferenties ophalen via het dashboard van de oplossing. Verderop in deze zelfstudie neemt u de referenties van het apparaat op in de clienttoepassing.

Als u dit nog niet hebt gedaan, moet u een aangepaste apparaat toevoegt aan uw oplossing voor externe controle. De volgende stappen in het dashboard van oplossing:

1. Klik in de linkerbenedenhoek van het dashboard op **Een apparaat toevoegen**.

   ![Een apparaat toevoegen][1]

1. Klik in het deelvenster **Aangepast apparaat** op **Nieuw toevoegen**.

   ![Een aangepast apparaat toevoegen][2]

1. Kies **Laat mij mijn eigen apparaat-id definiëren**. Voer een apparaat-ID zoals **rasppi**, klikt u op **cheque-ID** om te controleren of u nog niet de naam al gebruikt in uw oplossing en klik vervolgens op **maken** voor het inrichten van het apparaat.

   ![Apparaat-id toevoegen][3]

1. Noteer de referenties van het apparaat (**apparaat-ID**, **IoT Hub-hostnaam**, en **apparaatsleutel**). Uw clienttoepassing op de Pi frambozen moet deze waarden verbinding maken met de oplossing voor externe controle. Klik vervolgens op **Gereed**.

    ![Apparaatreferenties weergeven][4]

1. Selecteer uw apparaat in de lijst met apparaten in het oplossingsdashboard. Klik vervolgens in het deelvenster **Apparaatdetails** op **Apparaat inschakelen**. De status van het apparaat is nu **Wordt uitgevoerd**. De oplossing voor externe controle kan nu telemetrie ontvangen van uw apparaat en methoden op het apparaat aanroepen.

[img-launch-solution]: media/iot-suite-v1-raspberry-pi-kit-view-solution/launch.png
[img-menu]: media/iot-suite-v1-raspberry-pi-kit-view-solution/menu.png
[1]: media/iot-suite-v1-raspberry-pi-kit-view-solution/suite0.png
[2]: media/iot-suite-v1-raspberry-pi-kit-view-solution/suite1.png
[3]: media/iot-suite-v1-raspberry-pi-kit-view-solution/suite2.png
[4]: media/iot-suite-v1-raspberry-pi-kit-view-solution/suite3.png