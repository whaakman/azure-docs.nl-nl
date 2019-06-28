---
title: bestand opnemen
description: bestand opnemen
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 08/28/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 3e4549a21ec32f1a2c1c869c3b2e0bd8c2e4204e
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446579"
---
## <a name="generate-the-certificate-signing-request-file"></a>Het bestand met de aanvraag ondertekenen van certificaten genereren

De Apple Push Notification Service (APNs) maakt gebruik van certificaten voor verificatie van uw pushmeldingen te verzenden. Volg deze instructies om het pushcertificaat te maken dat vereist is om meldingen te verzenden en te ontvangen. Zie de officiële documentatie van de [Apple Push Notification Service](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html) voor meer informatie over deze concepten.

Genereer het Certificate Signing Request (CSR)-bestand dat gebruikmaakt van Apple voor het genereren van een ondertekend pushcertificaat.

1. Voer op uw Mac het hulpprogramma Sleutelhangertoegang uit. Kan worden geopend vanuit de **hulpprogramma's voor** map of het **andere** map op de Launchpad.

1. Selecteer **toegang tot sleutelhanger**, vouw **Certificaatassistent**, en selecteer vervolgens **een certificaat aanvragen bij een certificeringsinstantie**.

    ![Toegang tot sleutelhanger gebruiken om een nieuw certificaat aan te vragen](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

1. Selecteer uw **e-mailadres gebruiker**, Voer uw **algemene naam** waarde, zorg ervoor dat u opgeeft **opgeslagen op schijf**, en selecteer vervolgens **doorgaan**. Laat **e-mailadres CA** leeg als het is niet vereist.

    ![Vereiste certificaatgegevens](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)

1. Voer een naam voor het CSR-bestand in **OpslaanAls**, selecteert u de locatie in **waar**, en selecteer vervolgens **opslaan**.

    ![Kies een bestandsnaam op voor het certificaat](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)

    Deze actie wordt het CSR-bestand opgeslagen in de geselecteerde locatie. De standaardlocatie is **Desktop**. Onthoud de locatie die u voor het bestand hebt gekozen.

Vervolgens uw app registreren bij Apple, pushmeldingen inschakelen en de geëxporteerde CSR voor het maken van een push-certificaat uploaden.

## <a name="register-your-app-for-push-notifications"></a>Uw app voor pushmeldingen registreren

Als u wilt pushmeldingen verzenden naar een iOS-app, uw toepassing registreren met Apple en zich ook registreren voor pushmeldingen te verzenden.  

1. Als u uw app nog niet hebt geregistreerd, gaat u naar de [iOS-Inrichtingsportal](https://go.microsoft.com/fwlink/p/?LinkId=272456) bij het Apple Developer Center. Hierna, zich aanmelden met uw Apple-ID, selecteer **id's**, selecteer **App-id's**, en selecteer tot slot **+** om een nieuwe app te registreren.

    ![App-id-pagina van iOS-inrichtingsportal](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)

1. Werk de volgende drie waarden voor uw nieuwe app en selecteer vervolgens **doorgaan**:

   * **Naam**: Typ een beschrijvende naam voor uw app in de **naam** vak de **beschrijving App-ID** sectie.

   * **Bundel-id**: In de **expliciete App-ID** sectie, voer een **bundel-id** van het formulier `<Organization Identifier>.<Product Name>` zoals vermeld in de [App Distribution Guide](https://help.apple.com/xcode/mac/current/#/dev91fe7130a). De *organisatie-id* en *Productnaam* waarden moeten overeenkomen met de organisatie-id en product naam die u gebruikt wanneer u uw Xcode-project maakt. In de volgende schermopname is de *NotificationHubs* waarde wordt gebruikt als een organisatie-id en de *GetStarted* waarde wordt gebruikt als naam van het product. Zorg ervoor dat de **bundel-id** waarde komt overeen met de waarde in uw Xcode-project, zodat het juiste publicatieprofiel Xcode gebruikt.

   * **Pushmeldingen verzenden**: Controleer de **Pushmeldingen** optie in de **App Services** sectie.

     ![Formulier voor het registreren van een nieuwe app-id](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

     Deze actie wordt gegenereerd voor uw App-ID en aanvragen dat u bevestigen dat de gegevens. Selecteer **registreren** om te bevestigen van de nieuwe App-ID.

     Nadat u hebt geselecteerd **registreren**, ziet u de **registratie voltooid** scherm, zoals wordt weergegeven in de volgende afbeelding. Selecteer **Done**.

     ![App-id-registratie voltooid met rechten](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-registration-complete.png)

1. In het ontwikkelaarscentrum onder **App-id's**, zoek de app-ID die u hebt gemaakt en selecteer de rij.

    ![Lijst met app-id](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids2.png)

    Selecteer de app-ID moet de app-gegevens weergeven en selecteer vervolgens de **bewerken** knop aan de onderkant.

    ![Pagina app-id bewerken](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)

1. Ga naar de onderkant van het scherm en selecteer de **certificaat maken** knop onder de **ontwikkeling Push-SSL-certificaat** sectie.

    ![Certificaat voor app-id-knop maken](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    U nu Zie de **iOS-certificaat toevoegen** assistent.

    > [!NOTE]
    > In deze zelfstudie wordt een ontwikkelingscertificaat gebruikt. Hetzelfde proces wordt gebruikt bij het registreren van een productiecertificaat. Zorg er wel voor dat u hetzelfde certificaattype gebruikt als u meldingen verzendt.

1. Selecteer **bestand kiezen**, blader naar de locatie waar u het CSR-bestand van de eerste taak hebt opgeslagen, en selecteer vervolgens **genereren**.

    ![Upload-pagina voor gemaakte certificaat-CSR](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

1. Nadat het certificaat is gemaakt in de portal, selecteert u de **downloaden** knop en selecteer vervolgens **gedaan**.

    ![Download-pagina voor gemaakte certificaat](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    Het certificaat is gedownload en opgeslagen op uw computer in uw **Downloads** map.

    ![Certificaatbestand zoeken in de map Downloads](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [!NOTE]
    > Standaard de gedownloade ontwikkelingscertificaat heet **aps_development.cer**.

1. Selecteer het gedownloade pushcertificaat **aps_development.cer**.

    Nu wordt het nieuwe certificaat in de sleutelhanger geïnstalleerd, zoals op de volgende afbeelding wordt weergegeven:

    ![Certificatenlijst in Sleutelhangertoegang geeft nieuw certificaat weer](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [!NOTE]
    > Hoewel de naam in het certificaat afwijken kan, de naam wordt voorafgegaan door **Apple Development iOS Push Services**.

1. In Sleutelhangertoegang klikt u met de rechtermuisknop op het nieuwe pushcertificaat dat u hebt gemaakt in de categorie **Certificaten**. Selecteer **exporteren**, Noem het bestand, selecteer de **.p12** opmaken en selecteer vervolgens **opslaan**.

    ![Certificaat exporteren in p12-indeling](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)

    Noteer de bestandsnaam en locatie van het geëxporteerde .p12-certificaat. Ze worden gebruikt voor het inschakelen van verificatie met APNs.

    > [!NOTE]
    > Deze zelfstudie maakt u een bestand met de naam **QuickStart.p12**. De naam en locatie van uw bestand kunnen afwijken.

## <a name="create-a-provisioning-profile-for-the-app"></a>Een inrichtingsprofiel voor de app maken

1. In de [iOS-Inrichtingsportal](https://go.microsoft.com/fwlink/p/?LinkId=272456), selecteer **Inrichtingsprofielen**, selecteer **alle**, en selecteer vervolgens **+** maken een nieuw profiel. U ziet de **toevoegen van iOS-profiel voor inrichting** wizard.

    ![Lijst met inrichtingsprofielen](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

1. Selecteer **ontwikkeling iOS-App** onder **ontwikkeling** profieltype als de inrichting en selecteer **doorgaan**.

1. Selecteer vervolgens de app-ID die u hebt gemaakt van de **App-ID** vervolgkeuzelijst en selecteer **doorgaan**.

    ![Selecteer de app-id](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)

1. In de **certificaten selecteren** venster, selecteert u het ontwikkelingscertificaat dat u voor ondertekening van programmacode gebruikt en selecteer **doorgaan**. Dit certificaat is niet het pushcertificaat dat u hebt gemaakt.

    ![Het certificaat selecteren](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)

1. Vervolgens selecteert u de apparaten die worden gebruikt voor het testen en selecteer **doorgaan**.

    ![De apparaten selecteren](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)

1. Kies tot slot een naam voor het profiel in **profielnaam**, en selecteer **genereren**.

    ![Een naam kiezen voor het inrichtingsprofiel](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)

1. Wanneer het nieuwe inrichtingsprofiel is gemaakt, kunt u kiezen om te downloaden en installeren op uw ontwikkelcomputer Xcode. Selecteer vervolgens **Done**.

    ![Het inrichtingsprofiel downloaden](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-profile-ready.png)

## <a name="create-a-notification-hub"></a>Een Notification Hub maken

In deze sectie maakt u een notification hub maken en configureren van verificatie met APNs met behulp van het .p12-pushcertificaat dat u eerder hebt gemaakt. Als u gebruiken een notification hub die u al hebt gemaakt wilt, kunt u doorgaan met stap 5.

[!INCLUDE [notification-hubs-portal-create-new-hub](notification-hubs-portal-create-new-hub.md)]

## <a name="configure-your-notification-hub-with-apns-information"></a>Uw notification hub configureren met APNs-informatie

1. Selecteer **Apple (APNS)** onder **Notification Services**.

1. Selecteer **Certificaat**.

1. Selecteer het pictogram van het bestand.

1. Selecteer het .p12-bestand dat u eerder hebt geëxporteerd.

1. Geef het juiste wachtwoord.

1. Selecteer de modus **Sandbox**. Gebruik de modus **Productie** alleen als u pushmeldingen wilt verzenden naar gebruikers die uw app in de Store hebben gekocht.

    ![APNs-certificering in Azure Portal configureren](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-apple-config-cert.png)

U hebt nu uw notification hub geconfigureerd met APNs. U hebt ook de verbindingsreeksen voor uw app registreren en pushmeldingen te verzenden.
