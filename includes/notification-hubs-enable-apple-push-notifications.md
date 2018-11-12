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
ms.openlocfilehash: 8c8f3cd67186450fdcf65c177ea0353d297a3b01
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51264251"
---
## <a name="generate-the-certificate-signing-request-file"></a>Het bestand met de aanvraag voor certificaatondertekening genereren

De Apple Push Notification Service (APNS) gebruikt certificaten om uw pushmeldingen te verifiëren. Volg deze instructies om het pushcertificaat te maken dat vereist is om meldingen te verzenden en te ontvangen. Zie de officiële documentatie van de [Apple Push Notification Service](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html) voor meer informatie over deze concepten.

Genereer het bestand met de aanvraag voor certificaatondertekening dat door Apple wordt gebruikt om een ondertekend pushcertificaat te genereren.

1. Voer op uw Mac het hulpprogramma **Sleutelhangertoegang** uit. Dit kan worden gestart vanuit de map **Hulpprogramma's** of de map **Overige** in Launchpad.
2. Klik op **Toegang tot sleutelhanger**, vouw **Certificaatassistent** uit, klik vervolgens op **Een certificaat bij een certificaatautoriteit aanvragen...**.

    ![Toegang tot sleutelhanger gebruiken om een nieuw certificaat aan te vragen](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)
3. Selecteer uw **gebruikers-e-mailadres** en **algemene naam**, controleer of **Opgeslagen op schijf** is geselecteerd en klik vervolgens op **Doorgaan**. Laat het veld **E-mailadres CA** leeg omdat dit niet is vereist.

    ![Vereiste certificaatgegevens](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)

4. Typ een naam voor het bestand met de aanvraag voor certificaatondertekening in **Opslaan als**, selecteer de locatie in **Waar**, klik vervolgens op **Opslaan**.

    ![Een bestandsnaam kiezen voor het certificaat](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)

    Met deze actie wordt het bestand met de aanvraag voor certificaatondertekening opgeslagen op de geselecteerde locatie; de standaardlocatie is op het Bureaublad. Onthoud de locatie die u voor het bestand hebt gekozen.

Vervolgens gaat u uw app registreren bij Apple, pushmeldingen inschakelen en het geëxporteerde bestand met de aanvraag voor certificaatondertekening uploaden om een pushcertificaat te maken.

## <a name="register-your-app-for-push-notifications"></a>Uw app voor pushmeldingen registreren

U moet u app bij Apple registreren en u voor pushmeldingen registeren om pushmeldingen te kunnen verzenden naar een iOS-app.  

1. Als u uw app nog niet hebt geregistreerd, gaat u naar de [iOS-inrichtingsportal](https://go.microsoft.com/fwlink/p/?LinkId=272456), meldt u zich aan bij het Apple Developer Center met uw Apple ID, klikt u op **Id's**, klikt u vervolgens op **App-id's** en ten slotte op het **+**-teken om een nieuwe app te registreren.

    ![App-id-pagina van iOS-inrichtingsportal](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)

2. Werk de volgende drie velden voor uw nieuwe app bij en klik vervolgens op **Doorgaan**:

    * **Naam**: typ een beschrijvende naam voor uw app in het veld **Naam** in de sectie **Beschrijving app-id**.
    * **Bundel-id**: onder de sectie **Expliciete app-id** typt u een **bundel-id** in het formulier `<Organization Identifier>.<Product Name>` zoals vermeld in de [App Distribution Guide](https://help.apple.com/xcode/mac/current/#/dev91fe7130a) (Gids voor apps-distributie). De *organisatie-id* en *productnaam* die u gebruikt, moeten overeenkomen met de organisatie-id en productnaam die u gebruikt als u een XCode-project gaat maken. In de volgende schermopname wordt *NotificationHubs* gebruikt als de organisatie-id en *GetStarted* als de productnaam. Door ervoor te zorgen dat deze waarde overeenkomt met de waarde die u gaat gebruiken in uw XCode-project, is het mogelijk om het juiste publicatieprofiel met XCode te gebruiken.
    * **Pushmeldingen**: selecteer de optie **Pushmeldingen** in de sectie **App Services**.

    ![Formulier voor het registreren van een nieuwe app-id](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

    Met deze actie wordt uw app-id gegenereerd en wordt u gevraagd de gegevens te bevestigen. Klik op **Registreren** om de nieuwe app-id te bevestigen.

    Nadat u op **Registreren** hebt geklikt, wordt het scherm **Registratie voltooid** weergegeven, zoals u op de volgende afbeelding ziet. Klik op **Gereed**.

    ![App-id-registratie voltooid met rechten](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-registration-complete.png)

3. Ga in het Developer Center onder de app-id’s naar de app-id die u hebt gemaakt en klik op de rij van die id.

    ![Lijst met app-id](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids2.png)

    Door op de app-id te klikken, worden de gegevens van de app weergegeven. Klik op de knop **Bewerken** onderaan.

    ![Pagina app-id bewerken](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)

4. Ga naar de onderkant van het scherm en klik op de knop **Certificaat maken** onder de sectie **Ontwikkeling push-SSL-certificaat**.

    ![Certificaat voor app-id-knop maken](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    U ziet de assistent **iOS-certificaat toevoegen**.

    > [!NOTE]
    > In deze zelfstudie wordt een ontwikkelingscertificaat gebruikt. Hetzelfde proces wordt gebruikt bij het registreren van een productiecertificaat. Zorg er wel voor dat u hetzelfde certificaattype gebruikt als u meldingen verzendt.

5. Klik op **Bestand kiezen**, blader naar de locatie waar u het bestand met de aanvraag voor certificaatondertekening hebt opgeslagen dat u in de eerste taak hebt gemaakt, en klik vervolgens op **Genereren**.

    ![Upload-pagina voor gemaakte certificaat-CSR](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

6. Nadat het certificaat met de portal is gemaakt, klikt u op de knop **Downloaden** en klikt u vervolgens op **Gereed**.

    ![Download-pagina voor gemaakte certificaat](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    Het certificaat wordt nu gedownload en op uw computer in de map **Downloads** opgeslagen.

    ![Certificaatbestand zoeken in de map Downloads](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [!NOTE]
    > Standaard krijgt het gedownloade bestand, een ontwikkelingscertificaat, de naam **aps_development.cer**.

7. Dubbelklik op het gedownloade pushcertificaat **aps_development.cer**.

    Nu wordt het nieuwe certificaat in de sleutelhanger geïnstalleerd, zoals op de volgende afbeelding wordt weergegeven:

    ![Certificatenlijst in Sleutelhangertoegang geeft nieuw certificaat weer](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [!NOTE]
    > De naam in uw certificaat kan afwijken, maar wordt voorafgegaan door **Apple Development iOS Push Services**.

8. In Sleutelhangertoegang klikt u met de rechtermuisknop op het nieuwe pushcertificaat dat u hebt gemaakt in de categorie **Certificaten**. Klik op **Exporteer**, geef het bestand een naam, selecteer de **.p12**-indeling en klik vervolgens op **Bewaar**.

    ![Certificaat exporteren in p12-indeling](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)

    Noteer de bestandsnaam en locatie van het geëxporteerde .p12-certificaat. Dit wordt gebruikt om verificatie met APNS mogelijk te maken.

    > [!NOTE]
    > Tijdens deze zelfstudie wordt een QuickStart.p12-bestand gemaakt. De naam en locatie van uw bestand kunnen afwijken.

## <a name="create-a-provisioning-profile-for-the-app"></a>Een inrichtingsprofiel voor de app maken

1. Terug in de [iOS-inrichtingsportal](https://go.microsoft.com/fwlink/p/?LinkId=272456) selecteert u **Inrichtingsprofielen**, selecteert u **Alle** en klikt u vervolgens op de knop met het **+**-teken (plus) om een nieuw profiel te maken. U ziet de wizard **iOS-inrichtingsprofielen toevoegen**:

    ![Lijst met inrichtingsprofielen](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

2. Selecteer **Ontwikkeling iOS-app** onder **Ontwikkeling** als het inrichtingsprofieltype en klik vervolgens op **Doorgaan**.

3. Selecteer vervolgens in de vervolgkeuzelijst **App-id** de app-id die u hebt gemaakt en klik op **Doorgaan**

    ![Selecteer de app-id](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)

4. In het scherm **Certificaten selecteren** selecteert u het ontwikkelingscertificaat dat u doorgaans gebruikt voor het ondertekenen van programmacode en klikt u op **Doorgaan**. Dit certificaat is niet het pushcertificaat dat u hebt gemaakt.

    ![Het certificaat selecteren](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)

5. Selecteer vervolgens de **apparaten** die u voor de tests wilt gebruiken en klik op **Doorgaan**.

    ![De apparaten selecteren](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)

6. Kies tot slot een naam voor het profiel in **Profielnaam** en klik op **Genereren**.

    ![Een naam kiezen voor het inrichtingsprofiel](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)

7. Als het nieuwe inrichtingsprofiel is gemaakt, klikt u erop om het te downloaden en op uw machine met de Xcode-ontwikkelcode te installeren. Klik vervolgens op **Gereed**.

    ![Het inrichtingsprofiel downloaden](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-profile-ready.png)
