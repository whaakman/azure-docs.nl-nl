---
title: Beheren van een toepassing met Azure IoT Central | Microsoft Docs
description: Als een beheerder, het beheren van uw Azure IoT Central-toepassing
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 25b4777be4257933b84d58d0f10cf12571de9590
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50155317"
---
# <a name="administer-your-iot-central-application"></a>Uw IOT Central-toepassing beheren

Nadat u een Microsoft Azure IoT Central-toepassing hebt gemaakt, kunt u de **beheer** sectie van de Azure IoT Central gebruikersinterface om te beheren. Naar de **beheer** sectie, selecteer **beheer** in het navigatiemenu links.

De **beheer** sectie kunt u naar:

- Gebruikers beheren

- Rollen beheren

- Factureringsgegevens weergeven

- Toepassingsinstellingen beheren

- Bieden een gratis proefversie

In de **beheer** sectie, een secundaire navigatiemenu bevat koppelingen naar de verschillende beheertaken.

Voor toegang tot en gebruik de **beheer** sectie die u moet zich in de **beheerder** rol voor een Azure IoT Central-toepassing. Als u een Azure IoT Central-toepassing maakt, wordt u automatisch toegewezen aan de **beheerder** rol voor de toepassing. De [gebruikers beheren](#manage-users) sectie in dit artikel wordt uitgelegd meer over het toewijzen de **beheerder** rol aan andere gebruikers.

## <a name="change-application-name"></a>Naam van de toepassing wijzigen

Als u wilt de naam van uw toepassing wijzigen, gebruikt u de secundaire navigatiemenu naar de **toepassingsinstellingen** pagina in de **beheer** sectie.

Op de **toepassingsinstellingen** pagina, voer een naam in van uw keuze in de **toepassingsnaam** veld. Selecteer vervolgens **Opslaan**.

## <a name="change-the-application-url"></a>De URL van de toepassing wijzigen

Als u wilt wijzigen van de URL voor uw toepassing, gebruikt u de secundaire navigatiemenu om te navigeren naar de **toepassingsinstellingen** pagina in de **beheer** sectie.

![De instellingenpagina van toepassing](media\howto-administer\image0-a.png)

Op de **toepassingsinstellingen** pagina, typ de URL van uw keuze in de **URL** veld en selecteer vervolgens **opslaan**. De URL mag maximaal 200 tekens. Als de URL niet beschikbaar is, ziet u een validatiefout.

> [!Note]
> Als u de URL van uw wijzigt, kan de URL van uw oude kan worden uitgevoerd door een andere Azure IoT Central klant. Als dit gebeurt, is het niet meer beschikbaar voor gebruik. Wanneer u de URL van uw wijzigt, de oude URL is niet meer werkt en u moet op de hoogte stellen uw gebruikers over de nieuwe URL te gebruiken.

## <a name="change-the-application-image"></a>Installatiekopie van de toepassing wijzigen

Zie voor meer informatie over het gebruik van afbeeldingen in een Azure IoT Central-toepassing [voorbereiden en uploaden van afbeeldingen aan uw Azure IoT Central toepassing](howto-prepare-images.md).

## <a name="copy-an-application"></a>Kopiëren van een toepassing

U kunt een kopie van een toepassing, verminderd met elk apparaatexemplaren, de geschiedenis van apparaat en de gebruikersgegevens kunt maken. Het exemplaar is een betaalde toepassing die u voor betaalt. Kan geen maakt u een proefversie toepassing door een andere toepassing kopiëren.

Als u wilt kopiëren van een toepassing, gaat u naar de **toepassingsinstellingen** pagina. Selecteer vervolgens de **kopie** knop.

![De instellingenpagina van toepassing](media\howto-administer\appCopy1.png)

Selecteren van de **kopie** knop opent u een dialoogvenster waarin u een naam, URL, Azure AD-directory, abonnement en Azure-regio voor de nieuwe toepassing die wordt gemaakt door te kopiëren van uw toepassing kunt selecteren. Selecteer waarden voor elk van deze velden. Selecteer vervolgens de **kopie** knop om te bevestigen dat u wilt doorgaan. U kunt meer informatie over wat u moet invoeren voor deze waarden in dit artikel over [over het maken van een toepassing](howto-create-application.md).

![De instellingenpagina van toepassing](media\howto-administer\appCopy2.png)

Nadat de app-kopieerbewerking is geslaagd, gaat u naar de nieuwe toepassing die is gemaakt door te kopiëren van uw toepassing. Als u de app, selecteer de koppeling die wordt weergegeven op de **toepassingsinstellingen** pagina.

![De instellingenpagina van toepassing](media\howto-administer\appCopy3.png)

> [!Note]
> Kopiëren van een toepassing, kopieert ook de definitie van regels of acties worden uitgevoerd. Maar omdat gebruikers die toegang tot uw oorspronkelijke app hebben worden niet gekopieerd naar de gekopieerde app, hebt u handmatig gebruikers toevoegen aan acties zoals e-mailadres waarvan gebruikers een vereiste zijn.

## <a name="delete-an-application"></a>Een toepassing verwijderen

Als u wilt verwijderen van uw toepassing, gebruikt u de secundaire navigatiemenu om te navigeren naar de **toepassingsinstellingen** pagina in de **beheer** sectie.

Kies **verwijderen**.

> [!Note]
> Een toepassing definitief verwijderen, verwijdert u alle gegevens die is gekoppeld aan deze toepassing.  Als u wilt verwijderen van een toepassing, moet u ook machtigingen om resources te verwijderen hebben in de Azure-abonnement u hebt gekozen tijdens het maken van de toepassing. Zie voor meer informatie, [op rollen gebaseerd toegangsbeheer gebruiken voor het beheren van toegang tot de resources van uw Azure-abonnement](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

## <a name="roles-in-azure-iot-central"></a>Rollen in Azure IoT Central

Met functies kunt u om te bepalen wie binnen uw organisatie verschillende Azure IoT Central taken kunt uitvoeren. Azure IoT Central heeft drie rollen die u aan gebruikers van uw toepassing toewijzen kunt. Rollen zijn toegewezen door elke toepassing. Dezelfde gebruiker kan verschillende rollen hebben in verschillende toepassingen. U kunt dezelfde gebruiker toewijzen aan meerdere rollen in een toepassing.

### <a name="administrator"></a>Beheerder

Gebruikers in de **beheerder** rol hebben toegang tot alle functionaliteit in een Azure IoT Central-toepassing.

De gebruiker die een toepassing maakt, wordt automatisch toegewezen aan de **beheerder** rol. Er moet altijd zijn ten minste één gebruiker in de **beheerder** rol.

### <a name="application-builder"></a>Application Builder

Gebruikers in de **toepassing Builder** rol kunt doen alles in een Azure IoT Central-toepassing, behalve de toepassing beheren.

### <a name="application-operator"></a>Toepassing Operator

Gebruikers in de **toepassing Operator** rol geen toegang hebt tot de **toepassing Builder** pagina. Ze beheren de toepassing niet.

## <a name="manage-users"></a>Gebruikers beheren

Beheerders kunnen gebruikers toewijzen aan de rollen in de toepassing.

### <a name="add-users"></a>Gebruikers toevoegen

Elke gebruiker moet een gebruikersaccount hebben voordat ze kunnen zich aanmelden en toegang een Azure IoT Central-toepassing tot. Microsoft-Accounts (MSA's) en Azure Active Directory (Azure AD)-accounts worden ondersteund in Azure IoT Central. Azure Active Directory-groepen worden momenteel niet ondersteund in Azure IoT Central.

Zie voor meer informatie, [help voor Microsoft-account](https://support.microsoft.com/products/microsoft-account?category=manage-account) en [Quick Start: nieuwe gebruikers toevoegen aan Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).

1. Een gebruikersaccount toevoegen aan een Azure IoT Central-toepassing, gebruikt u de secundaire navigatiemenu naar de **gebruikers** pagina in de **beheer** sectie.

    ![Lijst met gebruikers](media\howto-administer\image1.png)

1. Toevoegen van een gebruiker op de **gebruikers** pagina, kies **+ gebruiker toevoegen**.

    ![Gebruiker toevoegen](media\howto-administer\image2.png)

1. Kies een rol voor de gebruiker van de **rol** vervolgkeuzelijst. Meer informatie over functies in de *rollen in Azure IoT Central* sectie van dit artikel.

    ![Een cloudrol kiezen](media\howto-administer\image3.png)

    > [!NOTE]
    >  Gebruikers bulksgewijs toevoegen, typt u de gebruikers-id van alle gebruikers die u wilt toevoegen van elkaar gescheiden door puntkomma's. Kies een rol van de **rol** vervolgkeuzelijst. Selecteer vervolgens **Opslaan**.

1. Nadat u een gebruiker toevoegt, verschijnt er een vermelding voor die gebruiker op de **gebruikers** pagina.

    ![Gebruikerslijst](media\howto-administer\image4.png)

### <a name="edit-the-roles-that-are-assigned-to-users"></a>De functies die zijn toegewezen aan gebruikers bewerken

Rollen kunnen niet worden gewijzigd nadat ze zijn toegewezen. Als u wilt wijzigen van de rol die toegewezen aan een gebruiker, de gebruiker verwijderen en vervolgens de gebruiker opnieuw met een andere rol toevoegen.

### <a name="delete-users"></a>Gebruikers verwijderen

Als u wilt verwijderen van gebruikers, selecteert u een of meer selectievakjes in op de **gebruikers** pagina. Selecteer vervolgens **Verwijderen**.

## <a name="view-your-bill"></a>Uw factuur weergeven

Als u wilt uw factuur bekijken, gaat u naar de **facturering** pagina in de **beheer** sectie. Selecteer vervolgens **facturering**. De Azure-facturering pagina wordt geopend in een nieuw tabblad, waar u de factuur voor elk van uw Azure IoT Central toepassingen kunt zien.

## <a name="convert-your-trial-to-a-paid-application"></a>Uw evaluatieversie converteren naar een betaalde toepassing

Nadat u hebt vastgesteld dat IoT Central, kunt u uw evaluatieversie converteren naar een betaalde toepassing. Volg deze stappen voor het voltooien van deze Self-serviceproces:

1. De secundaire navigatiemenu gebruiken om te gaan naar de **facturering** pagina in de **beheer** sectie. Als u uw proefversie van nog niet hebt uitgebreid, lijkt de pagina met de volgende schermafbeelding:

    ![Gratis proefversie status](media/howto-administer/freetrial.png)

2. Selecteer **converteren naar betaalde**. Als u uw proefversie van nog niet hebt uitgebreid, lijkt het pop-upvenster de volgende schermafbeelding:

    ![Gratis proefversie verlengen](media/howto-administer/extend.png)

3. Selecteer de juiste Azure Active Directory-tenant, en vervolgens het Azure-abonnement te gebruiken om uw IoT Central-toepassing in het pop-upvenster.

3. Nadat u hebt geselecteerd **converteren**, uw proefversie Hashtable naar een betaalde toepassing en u start wordt gefactureerd.

## <a name="extend-your-free-trial"></a>Uw gratis proefversie verlengen

Standaard zijn alle gratis proefversies beschikbaar voor de zeven dagen. Als u uw proefversie van 30 dagen verhogen wilt, volgt u deze stappen:

1. De secundaire navigatiemenu gebruiken om te gaan naar de **facturering** pagina in de **beheer** sectie.

1. Selecteer **proefversie verlengen**. Selecteer de juiste Azure Active Directory-tenant en vervolgens op het Azure-abonnement moet worden gebruikt voor uw IoT Central-toepassing in het pop-upvenster.

1. Selecteer vervolgens **uitbreiden**. Uw proefversie is nu geldig gedurende 30 dagen.

## <a name="use-the-azure-sdks-for-control-plane-operations"></a>Gebruik de Azure SDK's voor bewerkingen voor de controlelaag

IoT Central Azure Resource Manager SDK-pakketten zijn beschikbaar voor Node, Python, C#, Ruby, Java en Go. Deze bibliotheken ondersteuning voor bewerkingen voor de controlelaag voor IoT Central, waarmee u kunt maken, weergeven, bijwerken of verwijderen van IoT Central-toepassingen. Ze bieden ook hulpprogramma's voor het omgaan met verificatie en foutafhandeling die specifiek zijn voor elke taal is. 

Vindt u voorbeelden van hoe u met de Azure Resource Manager-SDK's op [ https://github.com/emgarten/iotcentral-arm-sdk-examples ](https://github.com/emgarten/iotcentral-arm-sdk-examples).

Voor meer informatie, bekijk deze pakketten op GitHub.

| Taal | Opslagplaats | Pakket |
| ---------| ---------- | ------- |
| Knooppunt | [https://github.com/Azure/azure-sdk-for-node](https://github.com/Azure/azure-sdk-for-node) | [https://www.npmjs.com/package/azure-arm-iotcentral](https://www.npmjs.com/package/azure-arm-iotcentral)
| Python |[https://github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python) | [https://pypi.org/project/azure-mgmt-iotcentral](https://pypi.org/project/azure-mgmt-iotcentral)
| C# | [https://github.com/Azure/azure-sdk-for-net](https://github.com/Azure/azure-sdk-for-net) | [https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral](https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral)
| Ruby | [https://github.com/Azure/azure-sdk-for-ruby](https://github.com/Azure/azure-sdk-for-ruby) | [https://rubygems.org/gems/azure_mgmt_iot_central](https://rubygems.org/gems/azure_mgmt_iot_central)
| Java | [https://github.com/Azure/azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java) | [https://search.maven.org/search?q=a:azure-mgmt-iotcentral](https://search.maven.org/search?q=a:azure-mgmt-iotcentral)
| Aan de slag | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go) | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go)

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u uw Azure IoT Central toepassing beheert, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [De apparaat-sjabloon instellen](howto-set-up-template.md)
