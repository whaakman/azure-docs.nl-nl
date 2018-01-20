---
title: Inrichten van een apparaat met Azure IoT Hub apparaat inrichten Service (.NET) | Microsoft Docs
description: Inrichten van uw apparaat met een enkele iothub met behulp van de Azure IoT Hub apparaat inrichten Service (.NET)
services: iot-dps
keywords: 
author: msebolt
ms.author: v-masebo
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 6919f962d853faa572ee7ad5d0cb9aeacd3bd2b6
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="enroll-the-device-to-an-iot-hub-using-the-azure-iot-hub-provisioning-service-client-net"></a>Registreren van het apparaat naar een iothub met behulp van de Azure IoT Hub inrichting Service Client (.NET)

In de vorige zelfstudie hebt u geleerd hoe u een apparaat instellen verbinding maken met uw service apparaten inrichten. In deze zelfstudie leert u hoe aan deze service gebruiken voor het inrichten van uw apparaat met een enkele iothub met zowel  **_afzonderlijke inschrijving_**  en  **_inschrijving groepen_**. In deze handleiding ontdekt u hoe u:

> [!div class="checklist"]
> * Het apparaat inschrijven
> * Start het apparaat
> * Controleer of dat het apparaat is geregistreerd

## <a name="prerequisites"></a>Vereisten

Voordat u doorgaat, zorg ervoor dat uw apparaat configureren en de bijbehorende *Hardware Security Module* zoals beschreven in de zelfstudie [instellen van een apparaat om in te richten met behulp van Azure IoT Hub apparaat-inrichtingsservice](./tutorial-set-up-device.md).

* Visual Studio 2015 of Visual Studio 2017

> [!NOTE]
> Visual Studio is niet vereist. De installatie van [.NET](https://www.microsoft.com/net) voldoende en ontwikkelaars kunnen hun voorkeur editor gebruiken op Windows of Linux.  

Deze zelfstudie wordt de periode tijdens of meteen na de productie-proces, wanneer gegevens van een apparaat wordt toegevoegd aan de inrichting service hardware gesimuleerd. Deze code wordt doorgaans uitgevoerd op een PC of een factory-apparaat dat .NET-code kan worden uitgevoerd en moet niet worden toegevoegd aan de apparaten zelf.


## <a name="enroll-the-device"></a>Het apparaat inschrijven

Deze stap omvat het unieke beveiligings-artefacten van het apparaat toe te voegen aan de Service voor het inrichten van apparaten. Deze beveiliging artefacten zijn als volgt:

- Voor op basis van TPM-apparaten:
    - De *goedkeuringssleutel* die uniek is voor elke TPM-chip of de simulatie. Lees de [TPM-goedkeuringssleutel begrijpen](https://technet.microsoft.com/library/cc770443.aspx) voor meer informatie.
    - De *registratie-ID* die wordt gebruikt om een apparaat in de naamruimte/scope uniek te identificeren. Of deze mogelijk niet hetzelfde zijn als de apparaat-ID. De ID is verplicht voor elk apparaat. Voor apparaten op basis van TPM kan de registratie-ID van de TPM zelf, bijvoorbeeld een SHA-256-hash van de TPM-goedkeuringssleutel afkomstig zijn.

- Voor X.509 op basis van apparaten:
    - De [X.509-certificaat uitgegeven aan het apparaat](https://msdn.microsoft.com/library/windows/desktop/bb540819.aspx), in de vorm van een *.pem* of een *.cer* bestand. Voor afzonderlijke inschrijving die u wilt gebruiken, de *leaf certificaat* voor uw systeem X.509 terwijl voor de inschrijving van groepen, moet u gebruiken de *basiscertificaat* of een gelijkwaardige *ondertekenaar certificaat*.
    - De *registratie-ID* die wordt gebruikt om een apparaat in de naamruimte/scope uniek te identificeren. Of deze mogelijk niet hetzelfde zijn als de apparaat-ID. De ID is verplicht voor elk apparaat. Voor X.509 op basis van apparaten, is de registratie-ID afgeleid van de algemene naam (CN) het certificaat. Zie voor meer informatie over deze vereisten [apparaat concepten](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-device).

Er zijn twee manieren het apparaat naar de Service voor het inrichten van apparaten te registreren:

- **Afzonderlijke inschrijvingen** Hiermee wordt een vermelding voor één apparaat bij de Service voor het inrichten van apparaten registreren kan. Afzonderlijke inschrijvingen mag x.509-certificaten of SAS-tokens (in een echt of virtueel TPM) gebruiken zoals attestation mechanismen. U wordt aangeraden met afzonderlijke inschrijvingen voor apparaten die unieke initiële configuraties vereisen, of voor apparaten die alleen via TPM SAS-tokens als het mechanisme voor attestation kunnen gebruiken. Afzonderlijke inschrijvingen wellicht de gewenste IoT hub apparaat-ID opgegeven.

- **Inschrijving groepen** dit vertegenwoordigt een groep apparaten die een specifieke attestation-mechanisme delen. Aangeraden wordt een inschrijvingsgroep voor voor een groot aantal apparaten dat een gewenste initiële configuratie delen, of voor apparaten alle gaat dezelfde tenant. Inschrijving groepen zijn X.509 alleen en alle delen van een ondertekend certificaat in de keten van hun x.509-certificaat.

### <a name="enroll-the-device-using-individual-enrollments"></a>Registreren van het apparaat met behulp van afzonderlijke inschrijvingen

1. Maak in Visual Studio een Visual C#-consoletoepassing-project met behulp van de **consoletoepassing** projectsjabloon. Noem het project **DeviceProvisioning**.
    
1. Klik in Solution Explorer met de rechtermuisknop op de **DeviceProvisioning** project en klik vervolgens op **NuGet-pakketten beheren...** .

1. In de **NuGet Package Manager** Selecteer **Bladeren** en zoek naar **microsoft.azure.devices.provisioning.service**. Selecteer de vermelding en klikt u op **installeren** voor het installeren van de **Microsoft.Azure.Devices.Provisioning.Service** Inpakken en accepteer de gebruiksvoorwaarden. Deze procedure downloadt, installeert en voegt u een verwijzing naar de [Azure IoT-device SDK-service inricht](https://www.nuget.org/packages/Microsoft.Azure.Devices.Provisioning.Service/) NuGet-pakket en de bijbehorende afhankelijkheden.

1. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:
   
    ```csharp
    using Microsoft.Azure.Devices.Provisioning.Service;
    ```

1. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingswaarde met de verbindingsreeks van de DP's in de vorige sectie hebt genoteerd.
   
    ```csharp
    static readonly string ServiceConnectionString = "{DPS connection string}";

    private const string SampleRegistrationId = "sample-individual-csharp";
    private const string SampleTpmEndorsementKey =
            "AToAAQALAAMAsgAgg3GXZ0SEs/gakMyNRqXXJP1S124GUgtk8qHaGzMUaaoABgCAAEMAEAgAAAAAAAEAxsj2gUS" +
            "cTk1UjuioeTlfGYZrrimExB+bScH75adUMRIi2UOMxG1kw4y+9RW/IVoMl4e620VxZad0ARX2gUqVjYO7KPVt3d" +
            "yKhZS3dkcvfBisBhP1XH9B33VqHG9SHnbnQXdBUaCgKAfxome8UmBKfe+naTsE5fkvjb/do3/dD6l4sGBwFCnKR" +
            "dln4XpM03zLpoHFao8zOwt8l/uP3qUIxmCYv9A7m69Ms+5/pCkTu/rK4mRDsfhZ0QLfbzVI6zQFOKF/rwsfBtFe" +
            "WlWtcuJMKlXdD8TXWElTzgh7JS4qhFzreL0c1mI0GCj+Aws0usZh7dLIVPnlgZcBhgy1SSDQMQ==";
    private const string OptionalDeviceId = "myCSharpDevice";
    private const ProvisioningStatus OptionalProvisioningStatus = ProvisioningStatus.Enabled;
    ```

1. Voeg de volgende voor het implementeren van de registratie voor het apparaat:

    ```csharp
    static async Task SetRegistrationDataAsync()
    {
        Console.WriteLine("Starting SetRegistrationData");

        Attestation attestation = new TpmAttestation(SampleTpmEndorsementKey);

        IndividualEnrollment individualEnrollment = new IndividualEnrollment(SampleRegistrationId, attestation);

        individualEnrollment.DeviceId = OptionalDeviceId;
        individualEnrollment.ProvisioningStatus = OptionalProvisioningStatus;

        Console.WriteLine("\nAdding new individualEnrollment...");
        var serviceClient = ProvisioningServiceClient.CreateFromConnectionString(ServiceConnectionString);

        IndividualEnrollment individualEnrollmentResult =
            await serviceClient.CreateOrUpdateIndividualEnrollmentAsync(individualEnrollment).ConfigureAwait(false);

        Console.WriteLine("\nIndividualEnrollment created with success.");
        Console.WriteLine(individualEnrollmentResult);
    }
    ```

1. Voeg de volgende code naar de **Main** methode voor het openen van de verbinding met uw IoT-hub en begint de inschrijving:
   
    ```csharp
    try
    {
        Console.WriteLine("IoT Device Provisioning example");

        SetRegistrationDataAsync().GetAwaiter().GetResult();
            
        Console.WriteLine("Done, hit enter to exit.");
        Console.ReadLine();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```
        
1. In de Visual Studio Solution Explorer met de rechtermuisknop op uw oplossing en klik vervolgens op **Opstartprojecten instellen...** . Selecteer **één opstartproject**, en selecteer vervolgens de **DeviceProvisioning** -project in de vervolgkeuzelijst.  

1. Voer de app voor .NET-apparaat **DeviceProvisiong**. Het moet inrichten van het apparaat instellen: 

    ![Individuele registratie uitvoeren](./media/tutorial-net-provision-device-to-hub/individual.png)

Wanneer het apparaat is ingeschreven, ziet u deze in de portal als volgt weergegeven:

   ![Geslaagde inschrijving in de portal](./media/tutorial-net-provision-device-to-hub/individual-portal.png)

### <a name="enroll-the-device-using-enrollment-groups"></a>Registreren van het apparaat met behulp van groepen van de inschrijving

> [!NOTE]
> Het voorbeeld van de groep inschrijving vereist een X.509-certificaat.

1. Open in de Visual Studio Solution Explorer de **DeviceProvisioning** project die eerder is gemaakt. 

1. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:
    
    ```csharp
    using System.Security.Cryptography.X509Certificates;
    ```

1. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingswaarde met de X509 locatie van het certificaat.
   
    ```csharp
    private const string X509RootCertPathVar = "{X509 Certificate Location}";
    private const string SampleEnrollmentGroupId = "sample-group-csharp";
    ```

1. Het volgende toevoegen aan **Program.cs** implementeren van de registratie voor de groep:

    ```csharp
    public static async Task SetGroupRegistrationDataAsync()
    {
        Console.WriteLine("Starting SetGroupRegistrationData");

        using (ProvisioningServiceClient provisioningServiceClient =
                ProvisioningServiceClient.CreateFromConnectionString(ServiceConnectionString))
        {
            Console.WriteLine("\nCreating a new enrollmentGroup...");

            var certificate = new X509Certificate2(X509RootCertPathVar);

            Attestation attestation = X509Attestation.CreateFromRootCertificates(certificate);

            EnrollmentGroup enrollmentGroup = new EnrollmentGroup(SampleEnrollmentGroupId, attestation);

            Console.WriteLine(enrollmentGroup);
            Console.WriteLine("\nAdding new enrollmentGroup...");

            EnrollmentGroup enrollmentGroupResult =
                await provisioningServiceClient.CreateOrUpdateEnrollmentGroupAsync(enrollmentGroup).ConfigureAwait(false);

            Console.WriteLine("\nEnrollmentGroup created with success.");
            Console.WriteLine(enrollmentGroupResult);
        }
    }
    ```

1. Tot slot vervangt de volgende code naar de **Main** methode voor het openen van de verbinding met uw IoT-hub en de groep in te schrijven:
   
    ```csharp
    try
    {
        Console.WriteLine("IoT Device Group Provisioning example");

        SetGroupRegistrationDataAsync().GetAwaiter().GetResult();
            
        Console.WriteLine("Done, hit enter to exit.");
        Console.ReadLine();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```

1. Voer de app voor .NET-apparaat **DeviceProvisiong**. Deze moet groep wordt ingericht voor het apparaat instellen: 

    ![Groep-registratie uitvoeren](./media/tutorial-net-provision-device-to-hub/group.png)

    Wanneer de apparaatgroep voor het apparaat wordt ingeschreven, ziet u deze in de portal als volgt weergegeven:

   ![Registratie van geslaagde groep in de portal](./media/tutorial-net-provision-device-to-hub/group-portal.png)


## <a name="start-the-device"></a>Start het apparaat

De volgende instellingen zijn nu gereed voor apparaatregistratie:

1. Uw apparaat of een groep apparaten zijn geregistreerd met uw Service apparaten inrichten en 
2. Uw apparaat is klaar met de beveiliging geconfigureerd en toegankelijk zijn via de toepassing die gebruikmaakt van de inrichtingsservice apparaat-client-SDK.

Start het apparaat zodat uw clienttoepassing starten van de registratie met uw apparaat van de inrichtingsservice.  


## <a name="verify-the-device-is-registered"></a>Controleer of dat het apparaat is geregistreerd

Eenmaal uw apparaat opnieuw is opgestart, de volgende acties moeten worden uitgevoerd. Zie de voorbeeldtoepassing voor TPM-simulator [dps_client_sample](https://github.com/Azure/azure-iot-device-auth/blob/master/dps_client/samples/dps_client_sample/dps_client_sample.c) voor meer informatie. 

1. Het apparaat verzendt een aanvraag voor functieregistratie met uw service apparaten inrichten.
2. Voor de TPM-apparaten vraagt de Service voor het inrichten van apparaten terug registratie waarop het apparaat reageert. 
3. Op de registratie is voltooid verzendt de Service voor het inrichten van apparaten de URI van de IoT-hub, apparaat-ID en de versleutelde sleutel terug naar het apparaat. 
4. De IoT Hub-clienttoepassing op het apparaat vervolgens verbinding maakt met uw hub. 
5. Geslaagde verbinding met de hub, ziet u het apparaat worden weergegeven in de IoT-hub **apparaat Explorer**. 

    ![Geslaagde verbinding met de hub in de portal](./media/tutorial-net-provision-device-to-hub/hub-connect-success.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Het apparaat inschrijven
> * Start het apparaat
> * Controleer of dat het apparaat is geregistreerd

Ga naar de volgende zelfstudie voor informatie over het inrichten van meerdere apparaten via hubs taakverdeling. 

> [!div class="nextstepaction"]
> [Inrichten van apparaten via IoT hubs Netwerktaakverdeling](./tutorial-provision-multiple-hubs.md)
