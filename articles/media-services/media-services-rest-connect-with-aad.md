---
title: Gebruik Azure AD-verificatie voor toegang tot Azure Media Services-API met REST | Microsoft Docs
description: Informatie over het openen van Azure Media Services-API met Azure Active Directory-verificatie met behulp van de REST.
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/26/2017
ms.author: willzhan;juliako;johndeu
ms.openlocfilehash: ed78d6c6d4c695b841dbfbf917cd1681adc44ee7
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="use-azure-ad-authentication-to-access-the-azure-media-services-api-with-rest"></a>Azure AD-verificatie gebruiken voor toegang tot de Azure Media Services-API met REST

Wanneer u Azure AD-verificatie met Azure Media Services gebruikt, kunt u verifiëren op twee manieren:

- **Gebruikersverificatie** verifieert van een persoon die de app wordt gebruikt om te communiceren met Azure Media Services-resources. De interactieve toepassing moet eerst de gebruiker om referenties gevraagd. Een voorbeeld is een management console-app die wordt gebruikt door gemachtigde gebruikers codering taken bewaken of live streamen. 
- **Verificatie van de service-principal** verifieert u een service. Toepassingen die veel deze verificatiemethode gebruiken zijn apps die worden uitgevoerd daemon-services, middelste laag services of geplande taken, zoals web-apps, apps van de functie, logische apps, API's of microservices.

    Deze zelfstudie leert u hoe u Azure AD **service-principal** verificatie voor toegang tot AMS API met REST. 

    > [!NOTE]
    > **Service-principal** wordt aanbevolen voor de meeste toepassingen verbinding met Azure Media Services. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De verificatie-informatie ophalen van de Azure-portal
> * Het toegangstoken met Postman ophalen
> * Test de **activa** API met behulp van het toegangstoken


> [!IMPORTANT]
> Media Services ondersteunt momenteel, het model van Azure Access Control-services-verificatie. Access Control-verificatie wordt echter 1 juni 2018 afgeschaft. We raden u aan om zo snel mogelijk naar het Azure Active Directory-verificatiemodel te migreren.

## <a name="prerequisites"></a>Vereisten

- Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.
- [Een Azure Media Services-account maken met de Azure-portal](media-services-portal-create-account.md).
- Controleer de [toegang tot Azure Media Services-API met overzicht van de AAD-authenticatie](media-services-use-aad-auth-to-access-ams-api.md) artikel.
- Installeer de [Postman](https://www.getpostman.com/) REST-client wilt uitvoeren van de REST-API's in dit artikel wordt weergegeven. 

    In deze zelfstudie zijn we durende **Postman** maar een REST-hulpmiddel zou geschikt zijn. Andere alternatieven zijn: **Visual Studio Code** met de REST-invoegtoepassing of **Telerik Fiddler**. 

## <a name="get-the-authentication-information-from-the-azure-portal"></a>De verificatie-informatie ophalen van de Azure-portal

### <a name="overview"></a>Overzicht

Voor toegang tot API voor Media Services, moet u de volgende gegevenspunten verzamelen.

|Instelling|Voorbeeld|Beschrijving|
|---|-------|-----|
|Azure Active Directory-tenantdomein|Microsoft.onmicrosoft.com|Azure AD als een Secure Token Service (STS)-eindpunt is gemaakt met de volgende notatie: https://login.microsoftonline.com/ {your-aad-tenant-name.onmicrosoft.com}/oauth2/token. Azure AD geeft een JWT voor toegang tot resources (een toegangstoken).|
|REST API-eindpunt|https://amshelloworld.restv2.westus.Media.Azure.NET/API/|Dit is het eindpunt met welke alle REST API voor Media Services in uw toepassing worden aangeroepen.|
|Client-ID (toepassings-ID)|f7fbbb29-a02d-4d91-bbc6-59a2579259d2|Azure AD (client) toepassings-ID. De client-ID is vereist voor het ophalen van het toegangstoken. |
|Clientgeheim|mUERiNzVMoJGggD6aV1etzFGa1n6KeSlLjIq + Dbim0 =|Azure AD-toepassing-sleutels (clientgeheim). Het clientgeheim is vereist voor het ophalen van het toegangstoken.|

### <a name="get-aad-auth-info-from-the-azure-portal"></a>AAD auth info ophalen uit de Azure-portal

Als u de informatie, de volgende stappen uit:

1. Meld u aan bij [Azure Portal](http://portal.azure.com).
2. Navigeer naar uw AMS-exemplaar.
3. Selecteer **API-toegang**.
4. Klik op **verbinding maken met Azure Media Services-API met service-principal**.

    ![API-toegang](./media/connect-with-rest/connect-with-rest01.png)

5. Selecteer een bestaande **Azure AD-toepassing** of maak een nieuwe (Zie hieronder).

    > [!NOTE]
    > Azure Media REST voor de aanvraag mislukt, de aanvragende gebruiker moet hebben een **Inzender** of **eigenaar** rol voor deze probeert te krijgen tot het Media Services-account. Als u krijgt een uitzondering met de tekst "de externe server heeft een fout geretourneerd: niet geautoriseerd (401), ' Zie [toegangsbeheer](media-services-use-aad-auth-to-access-ams-api.md#access-control).

    Als u wilt maken van een nieuw AD-app, als volgt te werk:
    
    1. Druk op **maken van nieuwe**.
    2. Voer een naam.
    3. Druk op **nieuw** opnieuw.
    4. Druk op **opslaan**.

    ![API-toegang](./media/connect-with-rest/new-app.png)

    De nieuwe app wordt weergegeven op de pagina.

6. Ophalen van de **Client-ID** (toepassings-ID).
    
    1. Selecteer de toepassing.
    2. Ophalen van de **Client-ID** vanuit het venster aan de rechterkant. 

    ![API-toegang](./media/connect-with-rest/existing-client-id.png).

7.  Ophalen van de toepassing **sleutel** (clientgeheim). 

    1. Klik op de **beheren toepassing** knop (u ziet dat de gegevens van de Client-ID onder de ligt **toepassings-ID**). 
    2. Druk op **sleutels**.
    
        ![API-toegang](./media/connect-with-rest/manage-app.png)
    3. Genereren van de app-sleutel (clientgeheim) door in te vullen **beschrijving** en **VERLOOPT** en drukt u op **opslaan**.
    
        Eenmaal de **opslaan** knop wordt ingedrukt, waarde van de sleutel weergegeven. Kopieer de waarde van de sleutel voordat u de blade verlaat.

    ![API-toegang](./media/connect-with-rest/connect-with-rest03.png)

U kunt waarden voor parameters van AD-verbinding toevoegen aan het bestand web.config of app.config voor later gebruik in uw code.

> [!IMPORTANT]
> De **clientsleutel** is een belangrijk geheim en moeten worden goed beveiligd in een sleutelkluis of versleuteld in productie.

## <a name="get-the-access-token-using-postman"></a>Het toegangstoken met Postman ophalen

Deze sectie wordt beschreven hoe u **Postman** uitvoeren van een REST-API die als resultaat een JWT Bearer-Token (toegangstoken geeft). Media Services REST API aanroepen, moet u de koptekst 'Verificatie' toevoegen aan de aanroepen en voeg de waarde van ' Bearer *your_access_token*' voor elke aanroep (zoals weergegeven in de volgende sectie van deze zelfstudie). 

1. Open **Postman**.
2. Selecteer **POST**.
3. Voer de URL in met de tenantnaam van uw met de volgende indeling: naam van de tenant moet eindigen met **. onmicrosoft.com** en de URL moet eindigen met **oauth2/tokens**: 

    https://login.microsoftonline.com/ {your-aad-tenant-name.onmicrosoft.com}/oauth2/token

4. Selecteer de **Headers** tabblad.
5. Voer de **Headers** gegevens door middel van het gegevensraster ' Sleutel/waarde'. 

    ![Gegevensraster](./media/connect-with-rest/headers-data-grid.png)

    U kunt ook op **bulksgewijs bewerken** koppelen aan de rechterkant van het venster Postman en plak de volgende code.

        Content-Type:application/x-www-form-urlencoded
        Keep-Alive:true

6. Druk op de **hoofdtekst** tabblad.
7. Geef de hoofdtekst van gegevens met behulp van het gegevensraster ' Sleutel/waarde' (vervangen de client-ID en geheime waarden). 

    ![Gegevensraster](./media/connect-with-rest/data-grid.png)

    U kunt ook op **bulksgewijs bewerken** aan de rechterkant van het Postman venster en plak de volgende hoofdtekst (vervangen de client-ID en geheime waarden):

        grant_type:client_credentials
        client_id:{Your Client ID that you got from your AAD Application}
        client_secret:{Your client secret that you got from your AAD Application's Keys}
        resource:https://rest.media.azure.net

8. Druk op **Verzenden**.

    ![Token ophalen](./media/connect-with-rest/connect-with-rest04.png)

Het geretourneerde antwoord bevat de **toegangstoken** dat u gebruiken wilt voor toegang tot alle AMS APIs.

## <a name="test-the-assets-api-using-the-access-token"></a>Test de **activa** API met behulp van het toegangstoken

Deze sectie wordt beschreven hoe u toegang tot de **activa** API kunnen doen met **Postman**.

1. Open **Postman**.
2. Selecteer **GET**.
3. Plak het REST-API-eindpunt (bijvoorbeeld https://amshelloworld.restv2.westus.media.azure.net/api/Assets)
4. Selecteer de **autorisatie** tabblad. 
5. Selecteer **Bearer-Token**.
6. Plak het token dat is gemaakt in de vorige sectie.

    ![Token ophalen](./media/connect-with-rest/connect-with-rest05.png)

    > [!NOTE]
    > De UX Postman kan afwijken tussen een Mac en een PC. Als de Mac-versie heeft geen de optie 'Bearer-Token' in de **verificatie** sectie vervolgkeuzelijst toe te voegen de **autorisatie** koptekst handmatig op de Mac-client.

   ![Auth-header](./media/connect-with-rest/auth-header.png)

7. Selecteer **Headers**.
5. Klik op **bulksgewijs bewerken** koppelen aan de rechterkant van het venster Postman.
6. Plak de volgende headers:

        x-ms-version:2.15
        Accept:application/json
        Content-Type:application/json
        DataServiceVersion:3.0
        MaxDataServiceVersion:3.0

7. Druk op **Verzenden**.

Het geretourneerde antwoord bevat de assets die in uw account zijn.

## <a name="next-steps"></a>Volgende stappen

* Probeer deze voorbeeldcode in [Azure AD-verificatie voor Azure Media Services toegang: beide via REST-API](https://github.com/willzhan/WAMSRESTSoln)
* [Uploaden van bestanden met .NET](media-services-dotnet-upload-files.md)
