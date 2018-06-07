---
title: Een toepassing met Azure IoT centraal beheren | Microsoft Docs
description: Als een beheerder, het beheren van uw Azure IoT centrale toepassing
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 0bdd798f9bc8fdaef54abd721a986c607c6323a5
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628552"
---
# <a name="how-to-administer-your-application"></a>Het beheren van uw toepassing

Nadat u een Microsoft Azure IoT centrale toepassing maakt, kunt u de **beheer** sectie van de gebruikersinterface van Azure IoT Centraal beheer van het. Om te navigeren naar de **beheer** sectie **beheer** in het navigatiemenu links.

De **beheer** sectie kunt u:

- Gebruikers beheren

- Rollen beheren

- Factureringsgegevens weergeven

- Toepassingsinstellingen beheren

- Uitbreiden van een gratis proefversie

In de **beheer** sectie, wordt er een secundaire navigatiemenu met koppelingen naar de verschillende beheertaken.

Voor toegang tot en gebruik de **beheer** sectie, moet u in de **beheerder** rol in de Azure IoT centrale toepassing. Als u een Azure IoT centrale toepassing maakt, wordt u automatisch toegewezen aan de **beheerder** rol voor die toepassing. De *gebruikers beheren* sectie in dit artikel wordt uitgelegd meer over het toewijzen van de **beheerder** rol aan andere gebruikers.

## <a name="change-application-name"></a>Naam van de toepassing wijzigen

U wijzigt de naam van uw toepassing, gebruikt u het secundaire navigatiemenu om te navigeren naar de **toepassingsinstellingen** pagina in de **beheer** sectie.

Op de **toepassingsinstellingen** pagina, voer een naam van uw keuze in de **toepassingsnaam** veld en kies vervolgens **opslaan**.

## <a name="change-the-application-url"></a>De URL van de toepassing wijzigen

Om de URL voor uw toepassing wijzigen, gebruikt u het secundaire navigatiemenu om te navigeren naar de **toepassingsinstellingen** pagina in de **beheer** sectie.

![Pagina met toepassingsinstellingen](media\howto-administer\image0-a.png)

Op de **toepassingsinstellingen** pagina, voer de URL van uw keuze in de **URL** veld en kies vervolgens **opslaan**. De URL mag niet meer dan 200 tekens lang zijn. Als de URL niet beschikbaar is, ziet u een validatiefout

> [!Note]
> Als u de URL van uw wijzigt, kan uw oude URL worden genomen door een andere Azure IoT centrale klant. In dat geval is het niet meer beschikbaar voor gebruik. Wanneer u de URL van uw wijzigt, wordt de oude URL is niet meer werkt en u moet waarschuwen dat uw gebruikers over de nieuwe URL moet worden gebruikt.

## <a name="change-the-application-image"></a>Installatiekopie van de toepassing wijzigen

Zie voor meer informatie over het gebruik van afbeeldingen in een Azure IoT centrale toepassing [installatiekopieën van het voorbereiden en uploaden naar uw Azure IoT centrale toepassing](howto-prepare-images.md).

## <a name="delete-an-application"></a>Een toepassing verwijderen

Als u wilt verwijderen van uw toepassing, gebruikt u het secundaire navigatiemenu om te navigeren naar de **toepassingsinstellingen** pagina in de **beheer** sectie.

Kies **verwijderen**.

> [!Note]
> Een toepassing permanent te verwijderen, verwijdert alle gegevens die zijn gekoppeld aan de toepassing. Als u wilt verwijderen van een toepassing, moet u ook de rechten voor het verwijderen van de resources hebben in de Azure-abonnement u hebt gekozen toen u de toepassing hebt gemaakt. Zie voor meer informatie, [Use Role-Based toegangsbeheer voor het beheren van toegang tot de resources van uw Azure-abonnement](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

## <a name="roles-in-azure-iot-central"></a>Rollen in Azure IoT-centraal

Functies kunt u bepalen wie binnen uw organisatie verschillende Azure IoT centrale taken kunt uitvoeren. Azure IoT centraal heeft drie rollen die u aan gebruikers van uw toepassing toewijzen kunt. Rollen zijn toegewezen door elke toepassing. Dezelfde gebruiker kan verschillende rollen in verschillende toepassingen hebben. U kunt de dezelfde gebruiker kan toewijzen aan meerdere rollen in een toepassing.

### <a name="administrator"></a>Beheerder

Gebruikers in de **beheerder** rol hebben toegang tot alle functies in een Azure IoT centrale toepassing.

De gebruiker maken van een toepassing automatisch wordt toegewezen aan de **beheerder** rol. Altijd moet er ten minste één gebruiker in de **beheerder** rol.

### <a name="application-builder"></a>Application Builder

Gebruikers in de **toepassing Builder** rol kunt doen alles in een Azure IoT centrale toepassing, behalve de toepassing beheren.

### <a name="application-operator"></a>Toepassing Operator

Gebruikers in de **toepassing Operator** rol hebt geen toegang tot de **toepassing Builder** pagina. Ze beheren de toepassing niet.

## <a name="manage-users"></a>Gebruikers beheren

Beheerders kunnen gebruikers toewijzen aan de rollen in de toepassing.

### <a name="add-users"></a>Gebruikers toevoegen

Elke gebruiker moet een gebruikersaccount hebben voordat ze kunnen aanmelden en toegang een Azure IoT centrale toepassing tot. Microsoft-Accounts (MSA's) en Azure Active Directory (AAD) accounts worden in Azure IoT centraal ondersteund. Azure Active Directory-groepen worden momenteel niet ondersteund in Azure IoT centrale.

Zie voor meer informatie, [help voor Microsoft-account](https://support.microsoft.com/products/microsoft-account?category=manage-account) en [Snelstartgids: nieuwe gebruikers toevoegen aan Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).

1. Als u wilt een gebruikersaccount toevoegen aan een Azure IoT centrale toepassing, gebruikt u het secundaire navigatiemenu om te navigeren naar de **gebruikers** pagina in de **beheer** sectie.

    ![Lijst met gebruikers](media\howto-administer\image1.png)

1. Op de **gebruikers** pagina **+ gebruiker toevoegen** een gebruiker toe te voegen.

    ![Gebruiker toevoegen](media\howto-administer\image2.png)

1. Wanneer u een gebruiker aan uw Azure IoT centrale toepassing toevoegt, kiest u een rol voor de gebruiker van de **rol** vervolgkeuzelijst. Meer informatie over de rollen in de *rollen in Azure IoT centrale* sectie van dit artikel.

    ![Selectie van Systeemrol](media\howto-administer\image3.png)

    > [!NOTE]
    >  Als gebruikers bulksgewijs toevoegen, voert u de gebruikers-id's van alle gebruikers die u wilt toevoegen gescheiden door puntkomma's. Kies een functie uit de **rol** vervolgkeuzelijst en kies **opslaan**.

1. Nadat u een gebruiker toevoegt, verschijnt er een vermelding voor die gebruiker op de **gebruikers** pagina.

    ![Gebruikerslijst](media\howto-administer\image4.png)

### <a name="edit-the-roles-assigned-to-users"></a>De rollen die zijn toegewezen aan gebruikers bewerken

Worden rollen kunnen niet gewijzigd als assinged. Verwijder de gebruiker om te wijzigen van de rol die is toegewezen aan een gebruiker, en voeg de gebruiker opnieuw met een andere rol toe.

### <a name="delete-users"></a>Gebruikers verwijderen

Voor het verwijderen van gebruikers, raadpleegt u een of meer checkboxes in op de **gebruikers** pagina en kies vervolgens **verwijderen**.

## <a name="view-your-bill"></a>Uw factuur weergeven

Als u wilt uw factuur weergeven, gaat u naar de **facturering** pagina in de **beheer** sectie en kies **facturering**. De Azure facturering pagina in een nieuw tabblad geopend en ziet u de factuur voor elk van uw Azure IoT centrale toepassingen.

## <a name="convert-your-trial-to-a-paid-application"></a>De evaluatieversie converteren naar een betaald toepassing

Zodra u IoT centraal hebt geëvalueerd, kunt u de evaluatieversie converteren naar een betaald toepassing. Deze procedure te voltooien selfservice, de volgende stappen uit:

1. De secundaire navigatiemenu gebruiken om te navigeren naar de **facturering** pagina in de **beheer** sectie. Als u uw proefabonnement nog niet hebt uitgebreid, ziet de pagina er als volgt uit:

    ![Gratis proefversie status](media/howto-administer/freetrial.png)

2. Klik op **converteren naar betaald**. Als u uw proefabonnement nog niet hebt uitgebreid, is het pop-upvenster ziet er als volgt:

    Selecteer de juiste Azure Active Directory-tenant en het Azure-abonnement dat u wilt gebruiken voor uw toepassing IoT centraal in het pop-upvenster.

    ![Gratis proefversie uitbreiden](media/howto-administer/extend.png)

3. Nadat u op **converteren**, uw proefabonnement wordt omgezet in een betaald toepassing en u begint met het ophalen van kosten in rekening gebracht.

## <a name="extend-your-free-trial"></a>Uitbreiden van uw gratis proefversie

Standaard zijn alle gratis proefversies beschikbaar 7 dagen. Als u uw proefabonnement van 30 dagen verhogen wilt, kunt u deze stappen volgen:

1. De secundaire navigatiemenu gebruiken om te navigeren naar de **facturering** pagina in de **beheer** sectie:

1. Klik op **uitbreiden proefversie**. Selecteer de juiste Azure Active Directory-tenant en vervolgens op het Azure-abonnement moet worden gebruikt voor uw toepassing IoT centraal in het pop-upvenster:

1. Klik vervolgens op **uitbreiden**. Uw proefabonnement is nu geldig voor 30 dagen.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u uw Azure IoT centrale toepassing beheert, wordt hier de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [De apparaat-sjabloon instellen](howto-set-up-template.md)