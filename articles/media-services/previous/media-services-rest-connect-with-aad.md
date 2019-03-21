---
title: Gebruik Azure AD-verificatie voor toegang tot Azure Media Services API met REST | Microsoft Docs
description: Informatie over het openen van Azure Media Services API met Azure Active Directory-verificatie met behulp van REST.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: willzhan;juliako;johndeu
ms.openlocfilehash: 4b6bd97d7e87832f774f7a09f7e0deeb4047e695
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58294463"
---
# <a name="use-azure-ad-authentication-to-access-the-media-services-api-with-rest"></a>Azure AD-verificatie gebruiken voor toegang tot de Media Services-API met REST

Wanneer u Azure AD-verificatie met Azure Media Services gebruikt, kunt u verificatie in twee manieren:

- **Verificatie van de gebruiker** verifieert van een persoon die de app wordt gebruikt om te communiceren met Azure Media Services-resources. De interactieve toepassing moet eerst de gebruiker om referenties gevraagd. Een voorbeeld is een management console-app die wordt gebruikt door gemachtigde gebruikers om te controleren coderingstaken of live streamen. 
- **Service-principal verificatie** verifieert, een service. Toepassingen die gebruikmaken van deze verificatiemethode vaak zijn apps die daemon-services, services van de middelste laag of geplande taken, zoals web-apps, functie-apps, logic apps, API's of microservices worden uitgevoerd.

    Deze zelfstudie leert u hoe u Azure AD gebruiken **service-principal** verificatie voor toegang tot de AMS-API met REST. 

    > [!NOTE]
    > **Service-principal** is de aanbevolen voor de meeste toepassingen die verbinding maken met Azure Media Services. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De verificatie-informatie ophalen uit de Azure portal
> * Het toegangstoken met Postman
> * Test de **activa** API met behulp van het toegangstoken


> [!IMPORTANT]
> Media Services ondersteunt momenteel het verificatiemodel voor toegangsbeheer van Azure-services. Access Control-verificatie wordt echter vanaf 1 juni 2018 afgeschaft. We raden u aan om zo snel mogelijk naar het Azure Active Directory-verificatiemodel te migreren.

## <a name="prerequisites"></a>Vereisten

- Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.
- [Een Azure Media Services-account maken met de Azure-portal](media-services-portal-create-account.md).
- Controleer de [toegang tot Azure Media Services API met Azure AD-verificatieoverzicht](media-services-use-aad-auth-to-access-ams-api.md) artikel.
- Installeer de [Postman](https://www.getpostman.com/) REST-client voor het uitvoeren van de REST-API's die in dit artikel worden weergegeven. 

    In deze zelfstudie gebruiken we **Postman** maar een REST-hulpprogramma zou geschikt. Enkele andere alternatieven: **Visual Studio Code** met de REST-invoegtoepassing of **Telerik Fiddler**. 

## <a name="get-the-authentication-information-from-the-azure-portal"></a>De verificatie-informatie ophalen uit de Azure portal

### <a name="overview"></a>Overzicht

Voor toegang tot API van Media Services, moet u de volgende gegevenspunten te verzamelen.

|Instelling|Voorbeeld|Description|
|---|-------|-----|
|Azure Active Directory-tenantdomein|microsoft.onmicrosoft.com|Azure AD als een Secure Token Service (STS)-eindpunt is gemaakt met de volgende notatie: <https://login.microsoftonline.com/{your-ad-tenant-name.onmicrosoft.com}/oauth2/token>. Azure AD geeft een JWT voor toegang tot resources (een toegangstoken).|
|REST API-eindpunt|<https://amshelloworld.restv2.westus.media.azure.net/api/>|Dit is het eindpunt waarvoor alle Media Services REST-API-aanroepen in uw toepassing worden gemaakt.|
|Client-ID (toepassings-ID)|f7fbbb29-a02d-4d91-bbc6-59a2579259d2|ID Azure AD-toepassing (client). De client-ID is vereist voor het ophalen van het toegangstoken. |
|Clientgeheim|+mUERiNzVMoJGggD6aV1etzFGa1n6KeSlLjIq+Dbim0=|Azure AD-toepassingssleutels (clientgeheim). Het clientgeheim is vereist voor het ophalen van het toegangstoken.|

### <a name="get-aad-auth-info-from-the-azure-portal"></a>AAD-verificatie-informatie ophalen uit de Azure portal

Als u de informatie, de volgende stappen uit:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Navigeer naar uw AMS-exemplaar.
3. Selecteer **API-toegang**.
4. Klik op **verbinding maken met Azure Media Services API met service-principal**.

    ![API-toegang](./media/connect-with-rest/connect-with-rest01.png)

5. Selecteer een bestaande **Azure AD-toepassing** of maak een nieuwe (Zie hieronder).

    > [!NOTE]
    > Voor de Azure Media REST-aanvraag te voltooien, moet de aanroepende gebruiker hebben een **Inzender** of **eigenaar** rol voor deze probeert te krijgen tot de Media Services-account. Als er een uitzondering met de tekst ' de externe server heeft een fout geretourneerd: (401) niet-geautoriseerd,' Zie [toegangsbeheer](media-services-use-aad-auth-to-access-ams-api.md#access-control).

    Als u nodig hebt voor het maken van een nieuwe AD-app, als volgt te werk:
    
   1. Druk op **Maak een nieuwe**.
   2. Voer een naam in.
   3. Druk op **nieuw** opnieuw.
   4. Klik op **Opslaan**.

      ![API-toegang](./media/connect-with-rest/new-app.png)

      De nieuwe app wordt weergegeven op de pagina.

6. Krijgen de **Client-ID** (toepassings-ID).
    
   1. Selecteer de toepassing.
   2. Krijgen de **Client-ID** in het venster aan de rechterkant. 

      ![API-toegang](./media/connect-with-rest/existing-client-id.png)

7. Ophalen van de toepassing **sleutel** (clientgeheim). 

   1. Klik op de **toepassing beheren** knop (u ziet dat de gegevens van de Client-ID onder de ligt **toepassings-ID**). 
   2. Druk op **sleutels**.
    
       ![API-toegang](./media/connect-with-rest/manage-app.png)
   3. De app-sleutel (clientgeheim) genereren door in te vullen **beschrijving** en **VERLOOPT** en drukt u op **opslaan**.
    
       Zodra de **opslaan** knop wordt gedrukt, wordt de waarde van de sleutel weergegeven. Kopieer de waarde van de sleutel voordat u de blade.

   ![API-toegang](./media/connect-with-rest/connect-with-rest03.png)

U kunt waarden voor parameters voor AD-verbinding toevoegen aan het bestand web.config of app.config voor later gebruik in uw code.

> [!IMPORTANT]
> De **clientsleutel** is een belangrijk geheim en moeten worden goed beveiligd in een key vault of versleuteld in productie.

## <a name="get-the-access-token-using-postman"></a>Het toegangstoken met Postman

In deze sectie ziet u hoe u **Postman** voor het uitvoeren van een REST-API waarmee een JWT Bearer-Token (toegangstoken) wordt geretourneerd. Voor het aanroepen van een Media Services REST API, moet u de "Autorisatie"-header toevoegen aan de aanroepen en voeg de waarde van "Bearer *your_access_token*" aan elke aanroep (zoals weergegeven in de volgende sectie van deze zelfstudie). 

1. Open **Postman**.
2. Selecteer **POST**.
3. Voer de URL in met de naam van uw tenant met de volgende notatie: naam van de tenant moet eindigen met **. onmicrosoft.com** en de URL moet eindigen met **oauth2-verificatietoken**: 

    https://login.microsoftonline.com/{your-aad-tenant-name.onmicrosoft.com}/oauth2/token

4. Selecteer de **Headers** tabblad.
5. Voer de **Headers** informatie met behulp van het gegevensraster "Sleutel/waarde". 

    ![Gegevensraster](./media/connect-with-rest/headers-data-grid.png)

    U kunt ook op **bulksgewijs bewerken** koppelen aan de rechterkant van het venster Postman en plak de volgende code.

        Content-Type:application/x-www-form-urlencoded
        Keep-Alive:true

6. Druk op de **hoofdtekst** tabblad.
7. Geef de hoofdtekst van de gegevens met behulp van het gegevensraster "Sleutel/waarde" (vervangen de client-ID en geheime waarden). 

    ![Gegevensraster](./media/connect-with-rest/data-grid.png)

    U kunt ook op **bulksgewijs bewerken** aan de rechterkant van de Postman-venster en plak de volgende instantie (Vervang de client-ID en geheime waarden):

        grant_type:client_credentials
        client_id:{Your Client ID that you got from your Azure AD Application}
        client_secret:{Your client secret that you got from your Azure AD Application's Keys}
        resource:https://rest.media.azure.net

8. Druk op **Verzenden**.

    ![-token ophalen](./media/connect-with-rest/connect-with-rest04.png)

Het geretourneerde antwoord bevat de **toegangstoken** dat u gebruiken wilt voor toegang tot alle APIs AMS.

## <a name="test-the-assets-api-using-the-access-token"></a>Test de **activa** API met behulp van het toegangstoken

Deze sectie wordt beschreven hoe u toegang tot de **activa** API kunnen doen met **Postman**.

1. Open **Postman**.
2. Selecteer **GET**.
3. Plak de REST API-eindpunt (bijvoorbeeld: https://amshelloworld.restv2.westus.media.azure.net/api/Assets)
4. Selecteer de **autorisatie** tabblad. 
5. Selecteer **Bearer-Token**.
6. Plak het token dat is gemaakt in de vorige sectie.

    ![-token ophalen](./media/connect-with-rest/connect-with-rest05.png)

    > [!NOTE]
    > De Postman-UX kan afwijken tussen een Mac en een PC. Als de Mac-versie beschikt niet over de optie 'Bearer-Token' in de **verificatie** sectie vervolgkeuzelijst die u moet toevoegen de **autorisatie** koptekst handmatig op de Mac-client.

   ![Autorisatie-header](./media/connect-with-rest/auth-header.png)

7. Selecteer **Headers**.
5. Klik op **bulksgewijs bewerken** koppelen aan de rechterkant van de Postman-venster.
6. Plak de volgende headers:

        x-ms-version:2.15
        Accept:application/json
        Content-Type:application/json
        DataServiceVersion:3.0
        MaxDataServiceVersion:3.0

7. Druk op **Verzenden**.

Het geretourneerde antwoord bevat de assets die zich in uw account.

## <a name="next-steps"></a>Volgende stappen

* Probeer deze voorbeeldcode in [Azure AD-verificatie voor toegang tot Azure Media Services: Beide via REST-API](https://github.com/willzhan/WAMSRESTSoln)
* [Bestanden uploaden met .NET](media-services-dotnet-upload-files.md)
