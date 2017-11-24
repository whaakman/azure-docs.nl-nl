---
title: Zelfstudie voor x.509-beveiliging in Azure IoT Hub | Microsoft Docs
description: Aan de slag op de beveiliging op basis van X.509 in uw Azure-IoT-hub in een gesimuleerde omgeving.
services: iot-hub
documentationcenter: 
author: dsk-2015
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2017
ms.author: dkshir
ms.openlocfilehash: 93f9099d7aef1161f7789e7b21a88a8691cb2a8e
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="set-up-x509-security-in-your-azure-iot-hub"></a>X.509-beveiliging instellen in uw Azure-IoT-hub

Deze zelfstudie simuleert de stappen die u nodig hebt voor het beveiligen van uw Azure IoT hub met de *x.509-certificaatverificatie*. We tonen hoe certificaten lokaal op uw Windows-machine maken met de open-source hulpprogramma OpenSSL omwille van de afbeelding. Het is raadzaam om gebruik te maken van deze zelfstudie voor alleen voor testdoeleinden. Voor de productie-omgeving, moet u de certificaten van koopt een *hoofdmap van de certificeringsinstantie (CA)*. 

## <a name="prerequisites"></a>Vereisten
Deze zelfstudie moet u de volgende bronnen gereed:

- U hebt een IoT-hub gemaakt met uw Azure-abonnement. Zie [maken van een IoT-hub via portal](iot-hub-create-through-portal.md) voor gedetailleerde stappen. 
- U hebt [Visual Studio 2015 of Visual Studio 2017](https://www.visualstudio.com/vs/) op deze computer geïnstalleerd. 

<a id="getcerts"></a>

## <a name="get-x509-ca-certificates"></a>X.509-CA-certificaten ophalen
De x.509-certificaat gebaseerde beveiliging van de IoT-Hub vereist dat u wilt beginnen met een [X.509-certificaatketen](https://en.wikipedia.org/wiki/X.509#Certificate_chains_and_cross-certification), waaronder het basiscertificaat, evenals alle tussenliggende certificaten tot het leaf-certificaat. 

U kunt een van de volgende manieren om uw certificaten:
- Koop x.509-certificaten van een *hoofdmap van de certificeringsinstantie (CA)*. Dit wordt aanbevolen voor productieomgevingen.
OF,
- Maak uw eigen X.509-certificaten met een hulpprogramma van derden, zoals [OpenSSL](https://www.openssl.org/). Dit zijn wel geschikt voor test-en ontwikkelingsdoeleinden. De secties die met de titel *X.509-certificaten maken* en *maken X.509-certificaatketen* in het artikel [PowerShell gebruiken om u te maken van X.509-certificaten](iot-hub-security-x509-create-certificates.md) helpt u stapsgewijs via een PowerShell-voorbeeldscript om de certificaten te maken met behulp van OpenSSL en PowerShell. Als u liever **Bash** in plaats van PowerShell-shell, raadpleegt u de gerelateerde secties van [voorbeeld van het beheren van CA-certificaten](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md). De rest van deze zelfstudie gebruikt de OpenSSL-omgeving instellen in deze *hoe* handleiding u stapsgewijs door de end-to-end X.509-beveiliging in Azure IoT Hub.


<a id="registercerts"></a>

## <a name="register-x509-ca-certificates-to-your-iot-hub"></a>X.509-CA-certificaten naar uw IoT-hub registreren

Deze stappen ziet u hoe u een nieuwe CA toevoegt aan uw IoT-hub via de portal.

1. Navigeer in de Azure-portal naar uw IoT-hub en open de **instellingen** > **certificaten** menu. 
2. Klik op **toevoegen** toevoegen van een nieuw certificaat.
3. Voer een beschrijvende weergavenaam aan uw certificaat. Selecteer het basiscertificaatbestand met de naam *RootCA.cer* gemaakt in de vorige sectie, vanaf uw computer. Klik op **Uploaden**.
4. Als u een melding krijgt dat het certificaat is geüpload, klikt u op **opslaan**.

    ![Certificaat uploaden](./media/iot-hub-security-x509-get-started/add-new-cert.png)  

   Hiermee ziet u uw certificaat in de **certificaat Explorer** lijst. Opmerking de **STATUS** van dit certificaat is *Unverified*.

5. Klik op het certificaat dat u in de vorige stap hebt toegevoegd.

6. In de **certificaatdetails** blade, klikt u op **verificatiecode genereren**.

7. Het maken van een **verificatiecode** het eigendom van het certificaat te valideren. De code naar het Klembord kopiëren. 

   ![Controleer of certificaat](./media/iot-hub-security-x509-get-started/verify-cert.png)  

8. Nu moet u zich aanmelden dit *verificatiecode* de persoonlijke sleutel koppelen met uw CA X.509-certificaat dat wordt gegenereerd voor een handtekening. Er zijn beschikbaar voor het uitvoeren van deze ondertekeningsproces, bijvoorbeeld OpenSSL hulpmiddelen. Dit staat bekend als de [bewijs van eigendom](https://tools.ietf.org/html/rfc5280#section-3.1). Als u de PowerShell-voorbeeldscripts in de vorige sectie hebt gebruikt, voert u het script dat wordt vermeld in het gedeelte [bewijs van eigendom van het x.509-CA-certificaat](iot-hub-security-x509-create-certificates.md#signverificationcode).
 
9. De resulterende handtekening uit stap 8 hierboven naar uw IoT-hub in de portal uploaden. In de **certificaatdetails** blade in de Azure-portal, gaat u naar de **verificatiecertificaat .pem of .cer-bestand**, en selecteer de handtekening, bijvoorbeeld *VerifyCert4.cer*gemaakt met het voorbeeld PowerShell opdracht de _Verkenner_ pictogram naast het.

10. Zodra het certificaat is geüpload, klikt u op **controleren**. De **STATUS** van uw certificaat wijzigingen aan  **_gecontroleerd_**  in de **certificaten** blade. Klik op **vernieuwen** als deze wordt niet automatisch bijgewerkt.

   ![Certificaatverificatie uploaden](./media/iot-hub-security-x509-get-started/upload-cert-verification.png)  


<a id="createdevice"></a>

## <a name="create-an-x509-device-for-your-iot-hub"></a>Maken van een X.509-apparaat voor uw IoT-hub

1. Navigeer in de Azure-portal naar uw IoT-hub **apparaat Explorer**.

2. Klik op **toevoegen** een nieuw apparaat toevoegen. 

3. Geef een beschrijvende weergavenaam voor de **apparaat-ID**, en selecteer  **_X.509 CA ondertekend_**  als de **verificatietype**. Klik op **Opslaan**.

   ![X.509-apparaat in de portal maken](./media/iot-hub-security-x509-get-started/create-x509-device.png)



<a id="authenticatedevice"></a>

## <a name="authenticate-your-x509-device-with-the-x509-certificates"></a>Het X.509-apparaat met de X.509-certificaten verifiëren

Verificatie van het X.509-apparaat, moet u eerst het apparaat met het CA-certificaat ondertekenen. Ondertekening van de leaf-apparaten wordt normaal uitgevoerd op de fabriek waar hulpprogramma's voor productie dienovereenkomstig zijn ingeschakeld. Als het apparaat van een fabrikant naar een andere gaat, wordt elke fabrikant ondertekenen actie wordt vastgelegd als een tussenliggende certificaat in de keten. Het eindresultaat is een certificaatketen van de CA-certificaat van het apparaat leaf-certificaat. Als u eerder hebt gebruikt de PowerShell-scripts in de vorige secties, wordt u kunt het script dat wordt vermeld in het gedeelte uitvoeren *maken leaf X.509-certificaat voor uw apparaat* in het artikel [PowerShell-scripts naar CA ondertekend x.509-certificaten beheren](iot-hub-security-x509-create-certificates.md) dit proces te simuleren.

Vervolgens wordt wordt er beschreven hoe u een C#-toepassing om te simuleren dat het X.509-apparaat is geregistreerd voor uw IoT-hub maken. We sturen temperatuur en vochtigheid waarden van het gesimuleerde apparaat naar de hub. Houd er rekening mee dat in deze zelfstudie, maken we alleen de apparaattoepassing. Deze achterlaat wijze van oefening maakt voor de lezers voor het maken van de IoT Hub-servicetoepassing die antwoord op de gebeurtenissen die worden verzonden door deze gesimuleerde apparaat verzendt. De C#-toepassing wordt ervan uitgegaan dat u de PowerShell-scripts vermeld in het artikel hebt gevolgd [PowerShell-scripts voor het beheren van CA ondertekend X.509-certificaten](iot-hub-security-x509-create-certificates.md)

1. Maak een nieuw Visual C# Classic Windows Desktop-project met behulp van de projectsjabloon consoletoepassing in Visual Studio. Noem het project **SimulateX509Device**.
   ![X.509-device-project in Visual Studio maken](./media/iot-hub-security-x509-get-started/create-device-project.png)

2. Klik in Solution Explorer met de rechtermuisknop op de **SimulateX509Device** project en klik vervolgens op **NuGet-pakketten beheren...** . Selecteer in het venster NuGet Package Manager **Bladeren** en zoek naar **microsoft.azure.devices.client**. Selecteer **installeren** voor het installeren van de **Microsoft.Azure.Devices.Client** Inpakken en accepteer de gebruiksvoorwaarden. Deze procedure downloadt, installeert en een verwijzing naar het Azure IoT device SDK NuGet-pakket en de bijbehorende afhankelijkheden worden toegevoegd.
   ![Apparaat-SDK NuGet-pakket toevoegen in Visual Studio](./media/iot-hub-security-x509-get-started/device-sdk-nuget.png)

3. De volgende regels code toevoegen aan de bovenkant van de *Program.cs* bestand:
    
    ```CSharp
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using System.Security.Cryptography.X509Certificates;
    ```

4. Voeg de volgende regels code in de **programma** klasse:
    
    ```CSharp
        private static int MESSAGE_COUNT = 5;
        private const int TEMPERATURE_THRESHOLD = 30;
        private static String deviceId = "<your-device-id>";
        private static float temperature;
        private static float humidity;
        private static Random rnd = new Random();
    ```
   Gebruik de beschrijvende apparaatnaam die u hebt gebruikt in de vorige sectie in plaats van _< your_device_id >_ tijdelijke aanduiding.

5. De volgende functie als u wilt willekeurige getallen voor temperatuur en vochtigheid maken en deze waarden worden verzonden naar de hub toevoegen:
    ```CSharp
    static async Task SendEvent(DeviceClient deviceClient)
    {
        string dataBuffer;
        Console.WriteLine("Device sending {0} messages to IoTHub...\n", MESSAGE_COUNT);

        for (int count = 0; count < MESSAGE_COUNT; count++)
        {
            temperature = rnd.Next(20, 35);
            humidity = rnd.Next(60, 80);
            dataBuffer = string.Format("{{\"deviceId\":\"{0}\",\"messageId\":{1},\"temperature\":{2},\"humidity\":{3}}}", deviceId, count, temperature, humidity);
            Message eventMessage = new Message(Encoding.UTF8.GetBytes(dataBuffer));
            eventMessage.Properties.Add("temperatureAlert", (temperature > TEMPERATURE_THRESHOLD) ? "true" : "false");
            Console.WriteLine("\t{0}> Sending message: {1}, Data: [{2}]", DateTime.Now.ToLocalTime(), count, dataBuffer);

            await deviceClient.SendEventAsync(eventMessage);
        }
    }
    ```

6. Voeg de volgende regels met code voor de **Main** functie, de tijdelijke aanduidingen vervangt _apparaat-id_, _uw iot-hub-naam_ en  _absolute-Path-to-your-Device-pfx-File_ zoals wordt vereist door uw instellingen.
    ```CSharp
    try
    {
        var cert = new X509Certificate2(@"<absolute-path-to-your-device-pfx-file>", "123");
        var auth = new DeviceAuthenticationWithX509Certificate("<device-id>", cert);
        var deviceClient = DeviceClient.Create("<your-iot-hub-name>.azure-devices.net", auth, TransportType.Amqp_Tcp_Only);

        if (deviceClient == null)
        {
            Console.WriteLine("Failed to create DeviceClient!");
        }
        else
        {
            Console.WriteLine("Successfully created DeviceClient!");
            SendEvent(deviceClient).Wait();
        }

        Console.WriteLine("Exiting...\n");
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```
   Deze code maakt verbinding met uw IoT-hub door de verbindingsreeks voor uw apparaat X.509 maken. Zodra de verbinding tot stand gebracht, vervolgens verzendt temperatuur en vochtigheid gebeurtenissen naar de hub en wordt gewacht op het antwoord. 
7. Omdat deze toepassing toegang heeft tot een *.pfx* -bestand, moet u dit in *Admin* modus. De Visual Studio-oplossing bouwen. Open een nieuw opdrachtvenster als een **beheerder**, en navigeer naar de map met deze oplossing. Navigeer naar de *bin/Debug* pad in de oplossingenmap. Voer de toepassing **SimulateX509Device.exe** van de _Admin_ opdrachtvenster. Hier ziet u uw apparaat is verbinden met de hub en het verzenden van de gebeurtenissen. 
   ![Apparaat-app uitvoeren](./media/iot-hub-security-x509-get-started/device-app-success.png)

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over het beveiligen van uw IoT-oplossing:

* [Aanbevolen beveiligingsprocedures voor IoT][lnk-security-best-practices]
* [IoT-beveiligingsarchitectuur][lnk-security-architecture]
* [Beveiligen van uw IoT-omgeving][lnk-security-deployment]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [AI implementeren op de edge-apparaten met Azure IoT rand][lnk-iotedge]

[lnk-security-best-practices]: iot-hub-security-best-practices.md
[lnk-security-architecture]: iot-hub-security-architecture.md
[lnk-security-deployment]: iot-hub-security-deployment.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
