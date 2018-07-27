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
ms.openlocfilehash: a43febf1e78f80451b6aeed19e095b2c313d3216
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39284850"
---
# <a name="how-to-administer-your-application"></a>Het beheren van uw toepassing

Nadat u een Microsoft Azure IoT Central-toepassing hebt gemaakt, kunt u de **beheer** sectie van de Azure IoT Central gebruikersinterface om te beheren. Om te navigeren naar de **beheer** sectie **beheer** in het navigatiemenu links.

De **beheer** sectie kunt u naar:

- Gebruikers beheren

- Rollen beheren

- Factureringsgegevens weergeven

- Toepassingsinstellingen beheren

- Een gratis proefversie verlengen

In de **beheer** sectie, wordt er een secundaire navigatiemenu met koppelingen naar de verschillende beheertaken.

Voor toegang tot en gebruik de **beheer** sectie die u moet zich in de **beheerder** rol in de Azure IoT Central-toepassing. Als u een Azure IoT Central-toepassing maakt, wordt u automatisch toegewezen aan de **beheerder** rol voor de toepassing. De *gebruikers beheren* sectie in dit artikel wordt uitgelegd meer over het toewijzen de **beheerder** rol aan andere gebruikers.

## <a name="change-application-name"></a>Naam van de toepassing wijzigen

Als u wilt de naam van uw toepassing wijzigen, gebruikt u de secundaire navigatiemenu om te navigeren naar de **toepassingsinstellingen** pagina in de **beheer** sectie.

Op de **toepassingsinstellingen** pagina, voer een naam in van uw keuze in de **toepassingsnaam** veld en kies vervolgens **opslaan**.

## <a name="change-the-application-url"></a>De URL van de toepassing wijzigen

Als u wilt wijzigen van de URL voor uw toepassing, gebruikt u de secundaire navigatiemenu om te navigeren naar de **toepassingsinstellingen** pagina in de **beheer** sectie.

![De instellingenpagina van toepassing](media\howto-administer\image0-a.png)

Op de **toepassingsinstellingen** pagina, typ de URL van uw keuze in de **URL** veld en kies vervolgens **opslaan**. De URL mag maximaal 200 tekens. Als de URL niet beschikbaar is, ziet u een validatiefout

> [!Note]
> Als u de URL van uw wijzigt, kan de URL van uw oude kan worden uitgevoerd door een andere Azure IoT Central klant. In dat geval is het niet meer beschikbaar voor gebruik. Wanneer u de URL van uw wijzigt, wordt de oude URL is niet meer werkt en u moet op de hoogte stellen uw gebruikers over de nieuwe URL te gebruiken.

## <a name="change-the-application-image"></a>Installatiekopie van de toepassing wijzigen

Zie voor meer informatie over het gebruik van afbeeldingen in een Azure IoT Central-toepassing [voorbereiden en uploaden van afbeeldingen aan uw Azure IoT Central toepassing](howto-prepare-images.md).

## <a name="copy-an-application"></a>Kopiëren van een toepassing

U kunt een kopie van een toepassing, verminderd met elk apparaatexemplaren, de geschiedenis van apparaat en de gebruikersgegevens kunt maken. Het exemplaar is een betaalde toepassing die u in rekening gebracht. Kan geen maakt u een proefversie toepassing door een andere toepassing kopiëren.

Als u wilt kopiëren van een toepassing, gaat u naar de **toepassingsinstellingen** pagina en klik op de **kopie** knop.

![De pagina instellingen van toepassing](media\howto-administer\appCopy1.png)

Te klikken op de **kopie** opent een dialoogvenster waarin u een naam, URL, AAD directory, abonnement en Azure-regio voor de nieuwe toepassing die wordt gemaakt door te kopiëren van uw toepassing kunt selecteren. Selecteer waarden voor elk van deze velden en klik vervolgens op de **kopie** knop om te bevestigen dat u wilt doorgaan. U kunt meer informatie over wat u moet invoeren voor deze waarden in het artikel over [over het maken van een toepassing](howto-create-application.md).

![De pagina instellingen van toepassing](media\howto-administer\appCopy2.png)

Zodra de app-kopieerbewerking is geslaagd, kunt u zich om te navigeren naar de nieuwe toepassing die is gemaakt met het kopiëren van uw toepassing door te klikken op de koppeling die wordt weergegeven op de **toepassingsinstellingen** pagina.

![De pagina instellingen van toepassing](media\howto-administer\appCopy3.png)

> [!Note]
> Een toepassing kopiëren kopieert u de definitie van regels of acties. Echter, omdat gebruikers die toegang tot uw oorspronkelijke app hebben worden niet gekopieerd naar de gekopieerde app, hebt u handmatig gebruikers toevoegen aan acties zoals e-mailadres waarvan gebruikers een vereiste zijn.

## <a name="delete-an-application"></a>Een toepassing verwijderen

Als u wilt verwijderen van uw toepassing, gebruikt u de secundaire navigatiemenu om te navigeren naar de **toepassingsinstellingen** pagina in de **beheer** sectie.

Kies **verwijderen**.

> [!Note]
> Als u een toepassing permanent verwijdert, verwijdert u alle gegevens die zijn gekoppeld aan de toepassing. Als u wilt verwijderen van een toepassing, hebt u ook de rechten om resources te verwijderen in het Azure-abonnement u hebt gekozen tijdens het maken van de toepassing. Zie voor meer informatie, [Use Role-Based toegangsbeheer voor het beheren van toegang tot de resources van uw Azure-abonnement](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

## <a name="roles-in-azure-iot-central"></a>Rollen in Azure IoT Central

Met functies kunt u om te bepalen wie binnen uw organisatie verschillende Azure IoT Central taken kunt uitvoeren. Azure IoT Central heeft drie rollen die u aan gebruikers van uw toepassing toewijzen kunt. Rollen zijn toegewezen door elke toepassing. Dezelfde gebruiker kan verschillende rollen hebben in verschillende toepassingen. U kunt de dezelfde gebruiker kan toewijzen aan meerdere rollen in een toepassing.

### <a name="administrator"></a>Beheerder

Gebruikers in de **beheerder** rol hebben toegang tot alle functionaliteit in een Azure IoT Central-toepassing.

De gebruiker die het maken van een toepassing wordt automatisch toegewezen aan de **beheerder** rol. Er moet altijd zijn ten minste één gebruiker in de **beheerder** rol.

### <a name="application-builder"></a>Application Builder

Gebruikers in de **toepassing Builder** rol kunt doen alles in een Azure IoT Central-toepassing, behalve de toepassing beheren.

### <a name="application-operator"></a>Toepassing Operator

Gebruikers in de **toepassing Operator** rol geen toegang hebt tot de **toepassing Builder** pagina. Ze beheren de toepassing niet.

## <a name="manage-users"></a>Gebruikers beheren

Beheerders kunnen gebruikers toewijzen aan de rollen in de toepassing.

### <a name="add-users"></a>Gebruikers toevoegen

Elke gebruiker moet een gebruikersaccount hebben voordat ze kunnen zich aanmelden en toegang een Azure IoT Central-toepassing tot. Microsoft-Accounts (MSA's) en Azure Active Directory (AAD)-accounts worden ondersteund in Azure IoT Central. Azure Active Directory-groepen worden momenteel niet ondersteund in Azure IoT Central.

Zie voor meer informatie, [help voor Microsoft-account](https://support.microsoft.com/products/microsoft-account?category=manage-account) en [Quick Start: nieuwe gebruikers toevoegen aan Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).

1. Een gebruikersaccount toevoegen aan een Azure IoT Central-toepassing, gebruikt u de secundaire navigatiemenu om te navigeren naar de **gebruikers** pagina in de **beheer** sectie.

    ![Lijst met gebruikers](media\howto-administer\image1.png)

1. Op de **gebruikers** pagina, kies **+ gebruiker toevoegen** een gebruiker toe te voegen.

    ![Gebruiker toevoegen](media\howto-administer\image2.png)

1. Wanneer u een gebruiker aan uw toepassing met Azure IoT Central toevoegen, kies een rol voor de gebruiker van de **rol** vervolgkeuzelijst. Meer informatie over functies in de *rollen in Azure IoT Central* sectie van dit artikel.

    ![Een cloudrol kiezen](media\howto-administer\image3.png)

    > [!NOTE]
    >  Gebruikers bulksgewijs toevoegen, typt u de gebruikers-id's van alle gebruikers die u wilt toevoegen van elkaar gescheiden door puntkomma's. Kies een rol van de **rol** vervolgkeuzelijst en kies **opslaan**.

1. Nadat u een gebruiker toevoegt, verschijnt er een vermelding voor die gebruiker op de **gebruikers** pagina.

    ![Lijst met gebruikers](media\howto-administer\image4.png)

### <a name="edit-the-roles-assigned-to-users"></a>De rollen die zijn toegewezen aan gebruikers bewerken

Worden rollen kunnen niet gewijzigd als assinged. Als u wilt wijzigen van de rol die is toegewezen aan een gebruiker, verwijder de gebruiker en de gebruiker opnieuw met een andere rol toevoegen.

### <a name="delete-users"></a>Gebruikers verwijderen

Als u wilt verwijderen van gebruikers, controleren op een of meer selectievakjes in de **gebruikers** pagina en kies vervolgens **verwijderen**.

## <a name="view-your-bill"></a>Uw factuur weergeven

Als u wilt uw factuur bekijken, gaat u naar de **facturering** pagina in de **beheer** sectie en kies **facturering**. De Azure facturering pagina in een nieuw tabblad geopend en ziet u de factuur voor elk van uw Azure IoT Central toepassingen.

## <a name="convert-your-trial-to-a-paid-application"></a>Uw evaluatieversie converteren naar een betaalde toepassing

Nadat u hebt vastgesteld dat IoT Central, kunt u uw evaluatieversie converteren naar een betaalde toepassing. Volg deze stappen voor het voltooien van deze Self-serviceproces:

1. De secundaire navigatiemenu gebruiken om te navigeren naar de **facturering** pagina in de **beheer** sectie. Als u uw proefversie van nog niet hebt uitgebreid, ziet de pagina er als volgt uit:

    ![Gratis proefversie status](media/howto-administer/freetrial.png)

2. Klik op **converteren naar betaalde**. Als u uw proefversie van nog niet hebt uitgebreid, is het pop-upvenster ziet eruit als volgt uit:

    Selecteer de juiste Azure Active Directory-tenant en het Azure-abonnement dat u wilt gebruiken voor uw IoT Central-toepassing in het pop-upvenster.

    ![Gratis proefversie verlengen](media/howto-administer/extend.png)

3. Nadat u op **converteren**, uw proefversie wordt geconverteerd naar een betaalde toepassing en u begint, wordt gefactureerd.

## <a name="extend-your-free-trial"></a>Uw gratis proefversie verlengen

Standaard zijn alle gratis proefversies beschikbaar voor zeven dagen. Als u uw proefversie van 30 dagen verhogen wilt, kunt u deze stappen volgen:

1. De secundaire navigatiemenu gebruiken om te navigeren naar de **facturering** pagina in de **beheer** sectie:

1. Klik op **proefversie verlengen**. Selecteer in het pop-upvenster de juiste Azure Active Directory-tenant en vervolgens op het Azure-abonnement moet worden gebruikt voor uw IoT Central-toepassing:

1. Klik vervolgens op **uitbreiden**. Uw proefversie is nu geldig gedurende 30 dagen.

## <a name="next-steps"></a>Volgende stappen

Nu dat uw Azure IoT Central-toepassing beheren, hebt u geleerd, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [De apparaat-sjabloon instellen](howto-set-up-template.md)