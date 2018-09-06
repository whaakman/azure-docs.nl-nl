---
title: Zelfstudie voor het x.509-beveiliging in Azure IoT Hub | Microsoft Docs
description: Aan de slag op de beveiliging op basis van X.509 in uw Azure-IoT-hub in een gesimuleerde omgeving.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/10/2017
ms.author: dobett
ms.openlocfilehash: 19f6f5d360981c743d819da81eb2f68db1853c8b
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43783114"
---
# <a name="set-up-x509-security-in-your-azure-iot-hub"></a>Instellen van de x.509-beveiliging in uw Azure-IoT-hub

In deze zelfstudie simuleert de stappen die u nodig hebt voor het beveiligen van uw Azure IoT hub met behulp van de *X.509-certificaatverificatie*. Ten behoeve van de afbeelding, zullen we laten zien hoe de open-source hulpprogramma OpenSSL gebruiken voor het maken van certificaten lokaal op uw Windows-computer. U wordt aangeraden dat u deze zelfstudie voor test-doeleinden gebruiken. Voor productie-omgeving, moet u de certificaten van koopt een *basis-CA (Certificeringsinstantie)*. 

## <a name="prerequisites"></a>Vereisten
In deze zelfstudie moet u de volgende resources gereed hebben:

- U hebt een IoT-hub gemaakt met uw Azure-abonnement. Zie [maken van een IoT-hub via portal](iot-hub-create-through-portal.md) voor gedetailleerde stappen. 
- U hebt [Visual Studio 2015 of Visual Studio 2017](https://www.visualstudio.com/vs/) op uw computer geïnstalleerd. 

<a id="getcerts"></a>

## <a name="get-x509-ca-certificates"></a>X.509-CA-certificaten ophalen
De x.509-certificaat op basis van beveiliging in de IoT-Hub, moet u beginnen met een [X.509-certificaatketen](https://en.wikipedia.org/wiki/X.509#Certificate_chains_and_cross-certification), waaronder het basiscertificaat, evenals alle tussenliggende certificaten accountpagina het leaf-certificaat. 

U kunt een van de volgende manieren om uw certificaten te halen:
- Kopen van x.509-certificaten van een *basis-CA (Certificeringsinstantie)*. Dit wordt aanbevolen voor productie-omgevingen.
OF,
- Maak uw eigen X.509-certificaten met behulp van een hulpprogramma van derden zoals [OpenSSL](https://www.openssl.org/). Dit is prima voor test- en testdoeleinden. Zie [beheren test CA-certificaten voor voorbeelden en zelfstudies](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) testen voor informatie over het genereren van CA-certificaten met behulp van PowerShell- of Bash. De rest van deze zelfstudie maakt gebruik van CA-testcertificaten die zijn gegenereerd door de instructies in [beheren test CA-certificaten voor voorbeelden en zelfstudies](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md).


<a id="registercerts"></a>

## <a name="register-x509-ca-certificates-to-your-iot-hub"></a>Registreren van x.509-CA-certificaten voor uw IoT-hub

Deze stappen laten zien hoe u een nieuwe certificeringsinstantie toevoegen aan uw IoT-hub via de portal.

1. Navigeer in de Azure-portal naar uw IoT-hub en open de **instellingen** > **certificaten** menu. 
2. Klik op **toevoegen** om toe te voegen een nieuw certificaat.
3. Voer een beschrijvende weergavenaam voor uw certificaat. Selecteer bestand van het basiscertificaat met de naam *RootCA.cer* gemaakt in de vorige sectie, vanaf uw computer. Klik op **Uploaden**.
4. Wanneer u een melding dat uw certificaat is geüpload, klikt u op **opslaan**.

    ![Certificaat uploaden](./media/iot-hub-security-x509-get-started/add-new-cert.png)  

   Hiermee ziet u het certificaat in de **Certificatenverkenner** lijst. Houd er rekening mee de **STATUS** van dit certificaat is *niet geverifieerd*.

5. Klik op het certificaat dat u in de vorige stap hebt toegevoegd.

6. In de **certificaatgegevens** blade, klikt u op **verificatiecode genereren**.

7. Maakt een **verificatiecode** het eigendom van het certificaat te valideren. Kopieer de code naar het Klembord. 

   ![Certificaat controleren](./media/iot-hub-security-x509-get-started/verify-cert.png)  

8. Nu moet u zich aanmelden dit *verificatiecode* met de persoonlijke sleutel koppelen met uw X.509-CA-certificaat, dat een handtekening wordt gegenereerd. Er zijn hulpprogramma's die beschikbaar zijn voor het uitvoeren van deze ondertekenen proces, bijvoorbeeld OpenSSL. Dit staat bekend als de [bewijs van eigendom](https://tools.ietf.org/html/rfc5280#section-3.1). Stap 3 in [beheren test CA-certificaten voor voorbeelden en zelfstudies](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) genereert een verificatiecode.
 
9. Upload het resulterende handtekening van stap 8 hierboven naar uw IoT-hub in de portal. In de **certificaatgegevens** blade in de Azure portal, gaat u naar de **verificatiecertificaat PEM- of cer-bestand**, en selecteer de handtekening, bijvoorbeeld *VerifyCert4.cer*die zijn gemaakt door het voorbeeld PowerShell opdracht met de _Verkenner_ pictogram naast het.

10. Nadat het certificaat is geüpload, klikt u op **controleren**. De **STATUS** van uw certificaat wijzigingen **_Verified_** in de **certificaten** blade. Klik op **vernieuwen** als deze wordt niet automatisch bijgewerkt.

   ![Certificaatcontrole uploaden](./media/iot-hub-security-x509-get-started/upload-cert-verification.png)  


<a id="createdevice"></a>

## <a name="create-an-x509-device-for-your-iot-hub"></a>Een X.509-apparaat voor uw IoT-hub maken

1. In de Azure-portal, gaat u naar uw IoT-hub **Device Explorer**.

2. Klik op **toevoegen** naar een nieuw apparaat toevoegen. 

3. Geef een beschrijvende weergavenaam voor de **apparaat-ID**, en selecteer **_x.509-CA ondertekend_** als de **verificatietype**. Klik op **Opslaan**.

   ![X.509-apparaat maken in portal](./media/iot-hub-security-x509-get-started/create-x509-device.png)



<a id="authenticatedevice"></a>

## <a name="authenticate-your-x509-device-with-the-x509-certificates"></a>Het X.509-apparaat met de X.509-certificaten verifiëren

Als u wilt uw X.509-apparaat verifiëren, moet u zich eerst aanmelden het apparaat met het CA-certificaat. Ondertekening van het leaf-apparaten wordt normaal uitgevoerd op de fabriek waar dienovereenkomstig productie-hulpprogramma's zijn ingeschakeld. Als het apparaat van de fabrikant van de ene naar de andere gaat, wordt elke fabrikant ondertekenen actie vastgelegd als een tussenliggende certificaat in de keten. Het eindresultaat is een certificaatketen van de CA-certificaat voor de leaf-certificaat van het apparaat. Stap 4 in [beheren test CA-certificaten voor voorbeelden en zelfstudies](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) genereert een certificaat voor apparaten.

Nu we wordt laten zien hoe u een C#-toepassing voor het simuleren van het X.509-apparaat is geregistreerd voor uw IoT-hub maken. Er wordt temperatuur en vochtigheid waarden van het gesimuleerde apparaat verzonden naar de hub. Houd er rekening mee dat in deze zelfstudie we alleen de apparaattoepassing maken. Het is links bij wijze van oefening naar de lezers van het maken van de IoT Hub-servicetoepassing die antwoord op de gebeurtenissen die worden verzonden door dit gesimuleerde apparaat verzendt. De C#-toepassing wordt ervan uitgegaan dat u de stappen in hebt gevolgd [beheren test CA-certificaten voor voorbeelden en zelfstudies](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md).

1. Maak een nieuw Visual C# Windows Classic Desktop-project met behulp van de projectsjabloon consoletoepassing in Visual Studio. Noem het project **SimulateX509Device**.
   ![X.509-apparaatproject in Visual Studio maken](./media/iot-hub-security-x509-get-started/create-device-project.png)

2. Klik in Solution Explorer met de rechtermuisknop op de **SimulateX509Device** project en klik vervolgens op **NuGet-pakketten beheren...** . Selecteer in het venster NuGet Package Manager **Bladeren** en zoek naar de **microsoft.azure.devices.client**. Selecteer **installeren** voor het installeren van de **Microsoft.Azure.Devices.Client** verpakt en accepteer de gebruiksvoorwaarden. Deze procedure downloadt, installeert en een verwijzing naar de Azure IoT device SDK NuGet-pakket en de bijbehorende afhankelijkheden toegevoegd.
   ![Device SDK NuGet-pakket toevoegen in Visual Studio](./media/iot-hub-security-x509-get-started/device-sdk-nuget.png)

3. Voeg de volgende regels code toe aan de bovenkant van de *Program.cs* bestand:
    
    ```CSharp
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using System.Security.Cryptography.X509Certificates;
    ```

4. Voeg de volgende regels code binnen de **programma** klasse:
    
    ```CSharp
        private static int MESSAGE_COUNT = 5;
        private const int TEMPERATURE_THRESHOLD = 30;
        private static String deviceId = "<your-device-id>";
        private static float temperature;
        private static float humidity;
        private static Random rnd = new Random();
    ```
   Gebruikt u de beschrijvende naam u hebt gebruikt in de voorgaande sectie in plaats van _< your_device_id >_ tijdelijke aanduiding.

5. Voeg de volgende functie voor het maken van willekeurige getallen voor temperatuur en vochtigheid en verzenden van deze waarden naar de hub:
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

6. Voeg de volgende coderegels aan het **Main** functie, de tijdelijke aanduidingen vervangt _apparaat-id_, _uw iot-hub-naam_ en  _absolute-Path-to-your-Device-pfx-File_ zoals wordt vereist door uw configuratie.
    ```CSharp
    try
    {
        var cert = new X509Certificate2(@"<absolute-path-to-your-device-pfx-file>", "1234");
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
   Deze code maakt verbinding met uw IoT-hub met het maken van de verbindingsreeks voor het X.509-apparaat. Zodra er verbinding gemaakt met, vervolgens temperatuur en vochtigheid gebeurtenissen verzonden naar de hub, en wacht tot de reactie. 
7. Omdat deze toepassing toegang heeft tot een *pfx* -bestand, moet u mogelijk om uit te voeren in *Admin* modus. Maak de Visual Studio-oplossing. Open een nieuw opdrachtvenster als een **beheerder**, en navigeer naar de map met deze oplossing. Navigeer naar de *bin/Debug* pad in de oplossingenmap. Voer de toepassing uit **SimulateX509Device.exe** uit de _Admin_ opdrachtvenster. Hier ziet u uw apparaat is verbinding te maken met de hub en de gebeurtenissen te verzenden. 
   ![Apparaat-app uitvoeren](./media/iot-hub-security-x509-get-started/device-app-success.png)

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over het beveiligen van uw IoT-oplossing:

* [Best Practices voor IoT-beveiliging][lnk-security-best-practices]
* [Architectuur voor IoT-beveiliging][lnk-security-architecture]
* [Uw IoT-implementatie beveiligen][lnk-security-deployment]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [AI implementeren op Edge-apparaten met Azure IoT Edge][lnk-iotedge]

[lnk-security-best-practices]: ../iot-fundamentals/iot-security-best-practices.md
[lnk-security-architecture]: ../iot-fundamentals/iot-security-architecture.md
[lnk-security-deployment]: ../iot-fundamentals/iot-security-deployment.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
