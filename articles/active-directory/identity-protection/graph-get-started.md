---
title: Microsoft Graph voor Azure Active Directory Identity Protection | Microsoft Docs
description: Leer hoe u Microsoft Graph op te vragen voor een lijst met risico's en bijbehorende gegevens uit Azure Active Directory.
services: active-directory
keywords: Azure active directory identity protection, risicogebeurtenis, beveiligingsproblemen, beveiligingsbeleid, Microsoft Graph
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: fa109ba7-a914-437b-821d-2bd98e681386
ms.service: active-directory
ms.component: conditional-access
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2018
ms.author: markvi
ms.reviewer: nigu
ms.custom: seohack1
ms.openlocfilehash: a415258b8127bb78d8a8d1b5ef228234b34b3e26
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51287503"
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Aan de slag met Azure Active Directory Identity Protection en Microsoft Graph
Microsoft Graph is de Microsoft unified-API-eindpunt en het hart van [Azure Active Directory Identity Protection](../active-directory-identityprotection.md) API's. De eerste API **identityRiskEvents**, kunt u Microsoft Graph op te vragen voor een lijst met [risicogebeurtenissen](../reports-monitoring/concept-risk-events.md) en informatie die is gekoppeld. In dit artikel helpt u op weg met het uitvoeren van query's deze API. Zie voor een diepgaande inleiding, de volledige documentatie en de toegang tot de Graph Explorer, de [Microsoft Graph-site](https://developer.microsoft.com/graph/).


Er zijn vier stappen voor het verkrijgen van toegang tot Identity Protection gegevens via Microsoft Graph:

1. Naam van het domein worden opgehaald.
2. Maak een nieuwe app-registratie. 
2. Dit geheim en enkele andere stukjes informatie gebruiken om te verifiëren bij Microsoft Graph, waar u een verificatietoken hebt ontvangen. 
3. Gebruik dit token aanvragen versturen naar de API-eindpunt en Identity Protection gegevens terugontvangen.

Voordat u begint, moet u op:

- Een Azure AD P2-tenant

- Beheerdersbevoegdheden voor het maken van de toepassing in Azure AD

- De naam van van uw tenant-domein (bijvoorbeeld contoso.onmicrosoft.com)


## <a name="retrieve-your-domain-name"></a>Uw domeinnaam ophalen 

1. [Meld u aan](https://portal.azure.com) naar uw Azure-portal als beheerder. 

2. Klik in het linkernavigatiedeelvenster op **Active Directory**. 
   
    ![Het maken van een toepassing](./media/graph-get-started/41.png)

3. Klik op **aangepaste-domeinnamen**.

    ![Aangepaste domeinnamen](./media/graph-get-started/71.png)

4. Kopieer de naam van het domein dat is gemarkeerd als primaire in de lijst van domeinnamen.

    ![Aangepaste domeinnamen](./media/graph-get-started/72.png)



## <a name="create-a-new-app-registration"></a>Maak een nieuwe app-registratie

1. Op de **Active Directory** pagina, in de **beheren** sectie, klikt u op **App-registraties**.

    ![Het maken van een toepassing](./media/graph-get-started/42.png)


2. Klik in het menu aan de bovenkant op **nieuwe toepassing registreren**.
   
    ![Het maken van een toepassing](./media/graph-get-started/43.png)

3. Op de **maken** pagina, voert u de volgende stappen uit:
   
    ![Het maken van een toepassing](./media/graph-get-started/44.png)

    a. In de **naam** tekstvak, typ een naam voor uw toepassing (bijvoorbeeld: AADIP risico Event-API-toepassing).
   
    b. Als **toepassingstype**, selecteer **webtoepassing en / of Web-API**.
   
    c. In de **aanmeldings-URL** tekstvak, type `http://localhost`.

    d. Klik op **Create**.

4. Om te openen de **instellingen** pagina in de lijst met toepassingen, klikt u op uw zojuist gemaakte app-registratie. 

5. Kopieer de **toepassings-ID**.


## <a name="grant-your-application-permission-to-use-the-api"></a>Uw toepassing verlenen tot de API gebruiken

1. Op de **instellingen** pagina, klikt u op **vereiste machtigingen**.
   
    ![Het maken van een toepassing](./media/graph-get-started/15.png)

2. Op de **vereiste machtigingen** in de werkbalk bovenaan op de pagina, klikt u op **toevoegen**.
   
    ![Het maken van een toepassing](./media/graph-get-started/16.png)
   
3. Op de **API-toegang toevoegen** pagina, klikt u op **Select an API**.
   
    ![Het maken van een toepassing](./media/graph-get-started/17.png)

4. Op de **Select an API** pagina, selecteert u **Microsoft Graph**, en klik vervolgens op **Selecteer**.
   
    ![Het maken van een toepassing](./media/graph-get-started/18.png)

5. Op de **API-toegang toevoegen** pagina, klikt u op **machtigingen selecteren**.
   
    ![Het maken van een toepassing](./media/graph-get-started/19.png)

6. Op de **toegang inschakelen** pagina, klikt u op **lezen van alle identiteitsgegevens risico**, en klik vervolgens op **Selecteer**.
   
    ![Het maken van een toepassing](./media/graph-get-started/20.png)

7. Op de **API-toegang toevoegen** pagina, klikt u op **gedaan**.
   
    ![Het maken van een toepassing](./media/graph-get-started/21.png)

8. Op de **vereiste machtigingen** pagina, klikt u op **machtigingen verlenen**, en klik vervolgens op **Ja**.
   
    ![Het maken van een toepassing](./media/graph-get-started/22.png)



## <a name="get-an-access-key"></a>Een toegangssleutel opvragen

1. Op de **instellingen** pagina, klikt u op **sleutels**.
   
    ![Het maken van een toepassing](./media/graph-get-started/23.png)

2. Op de **sleutels** pagina, voert u de volgende stappen uit:
   
    ![Het maken van een toepassing](./media/graph-get-started/24.png)

    a. In de **Sleutelbeschrijving** tekstvak, typ een beschrijving (bijvoorbeeld *AADIP risicogebeurtenis*).
    
    b. Als **duur**, selecteer **In 1 jaar**.

    c. Klik op **Opslaan**.
   
    d. Kopieer de sleutelwaarde en plak deze in een veilige locatie.   
   
   > [!NOTE]
   > Als u deze sleutel kwijtraakt, wordt u terug naar deze sectie en maak een nieuwe sleutel hebben. Houd deze sleutel een geheim: iedereen die deze heeft toegang tot uw gegevens.
   > 
   > 

## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Verifiëren bij Microsoft Graph API en er query identiteit risico gebeurtenissen
Op dit moment dat u hebt:

- De naam van het domein van uw tenant

- De client-ID 

- De sleutel 


Als u wilt verifiëren, verzendt u een post-aanvraag voor `https://login.microsoft.com` met de volgende parameters in de hoofdtekst:

- grant_type: "**client_credentials**"

-  resource: '**https://graph.microsoft.com**"

- client_id: \<uw client-ID\>

- waarde voor client_secret: \<uw sleutel\>


Als dit lukt, retourneert deze een verificatietoken.  
Voor het aanroepen van de API, een koptekst te maken met de volgende parameter:

    `Authorization`="<token_type> <access_token>"


Bij het verifiëren, kunt u het type token en het toegangstoken vinden in het geretourneerde token.

Deze header als een aanvraag verzenden naar de volgende API-URL: `https://graph.microsoft.com/beta/identityRiskEvents`

Het antwoord, is als dit lukt, een verzameling van risicogebeurtenissen voor identiteiten en de bijbehorende gegevens in de OData-JSON-indeling, dat kan worden geparseerd en wens naar verwerkt.

Hier volgt een voorbeeld van code voor verificatie en aanroepen van de API met behulp van PowerShell.  
Uw client-ID, de geheime sleutel en het tenantdomein dat alleen toevoegen.

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 


## <a name="next-steps"></a>Volgende stappen

Gefeliciteerd, u de eerste aanroep van Microsoft Graph zojuist hebt gemaakt.  
U kunt nu query risicogebeurtenissen voor identiteiten en gebruikmaken van de gegevens echter naar eigen inzicht.

Bekijk voor meer informatie over Microsoft Graph en over het bouwen van toepassingen die gebruikmaken van de Graph API, de [documentatie](https://developer.microsoft.com/graph/docs) en nog veel meer op de [Microsoft Graph-site](https://developer.microsoft.com/graph/). Zorg er ook voor bladwijzer de [Azure AD Identity Protection API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityprotection_root) pagina met een lijst met alle Identity Protection API's beschikbaar in de grafiek. Als we nieuwe manieren om te werken met Identity Protection via API toevoegen, ziet u ze op die pagina.

Zie voor meer informatie:

-  [Azure Active Directory Identity Protection](../active-directory-identityprotection.md)

-  [Typen risicogebeurtenissen die worden gedetecteerd door Azure Active Directory Identity Protection](../reports-monitoring/concept-risk-events.md)

- [Microsoft Graph](https://developer.microsoft.com/graph/)

- [Overzicht van Microsoft Graph](https://developer.microsoft.com/graph/docs)

- [Azure AD Identity Protection-Service-hoofdmap](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityprotection_root)

