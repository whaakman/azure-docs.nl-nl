---
title: 'Aan de slag met Azure Active Directory: Identity Protection en Microsoft Graph | Microsoft Docs'
description: Biedt een inleiding tot Microsoft Graph query voor een lijst met risico's en bijbehorende gegevens van Azure Active Directory.
services: active-directory
keywords: beveiliging voor Azure active directory-identiteit, risicogebeurtenis, beveiligingsprobleem, beveiligingsbeleid, Microsoft Graph
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: fa109ba7-a914-437b-821d-2bd98e681386
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 9b01ff86da6a1fd4a439a6ba59ea15ed6480cdad
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Aan de slag met Azure Active Directory: Identity Protection en Microsoft Graph
Microsoft Graph is de Microsoft unified API-eindpunt en het hart van [Azure Active Directory: Identity Protection](active-directory-identityprotection.md) API's. De eerste API **identityRiskEvents**, kunt u Microsoft Graph opvragen voor een lijst met [bestaat de kans dat gebeurtenissen](active-directory-identityprotection-risk-events-types.md) en informatie die is gekoppeld. In dit artikel helpt u op weg met het opvragen van deze API. Zie voor een gedetailleerde inleiding, volledige documentatie en toegang tot de grafiek Explorer, de [Microsoft Graph site](https://graph.microsoft.io/).

> [!IMPORTANT]
> Microsoft raadt u aan Azure AD te beheren met het [Azure AD-beheercentrum](https://aad.portal.azure.com) in Azure Portal in plaats van de klassieke Azure portal waarnaar in dit artikel wordt verwezen.

Er zijn drie stappen voor het openen van Identity Protection gegevens via Microsoft Graph:

1. Toevoegen van een toepassing met een clientgeheim. 
2. Gebruik dit geheim en enkele andere stukjes informatie om Microsoft Graph, waarbij er geen verificatietoken te verifiëren. 
3. Gebruik dit token aanvragen in de API-eindpunt aanbrengen en terughalen van gegevens voor de beveiliging van de identiteit.

Voordat u begint, hebt u nodig:

* Beheerdersrechten voor de toepassing in Azure AD maken
* De naam van uw tenant-domein (bijvoorbeeld: contoso.onmicrosoft.com)

## <a name="add-an-application-with-a-client-secret"></a>Een toepassing met een clientgeheim toevoegen
1. [Meld u aan](https://manage.windowsazure.com) bij de klassieke Azure portal als beheerder. 
2. Klik op in het navigatiedeelvenster links op **Active Directory**. 
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)
3. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.
4. Klik in het menu bovenaan op **toepassingen**.
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)
5. Klik op **toevoegen** aan de onderkant van de pagina.
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)
6. Op de **wat wilt u doen** dialoogvenster, klikt u op **mijn organisatie ontwikkelt toepassing toevoegen**.
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)
7. Op de **Vertel ons over uw toepassing** dialoogvenster de volgende stappen uitvoeren:
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)
   
    a. In de **naam** textbox, typ een naam voor uw toepassing (bijvoorbeeld: AADIP risico gebeurtenis API toepassing).
   
    b. Als **Type**, selecteer **webtoepassing en / of Web-API**.
   
    c. Klik op **Volgende**.
8. Op de **App-eigenschappen** dialoogvenster de volgende stappen uitvoeren:
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)
   
    a. In de **aanmeldings-URL** textbox type `http://localhost`.
   
    b. In de **App ID URI** textbox type `http://localhost`.
   
    c. Klik op **Voltooien**.

U kunt nu uw toepassing configureren.

![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)

## <a name="grant-your-application-permission-to-use-the-api"></a>Uw toepassing toestemming de API gebruiken
1. Klik op de pagina van uw toepassing in het menu aan de bovenkant op **configureren**. 
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)
2. In de **machtigingen voor andere toepassingen** sectie, klikt u op **toepassing toevoegen**.
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)
3. Op de **machtigingen voor andere toepassingen** dialoogvenster de volgende stappen uitvoeren:
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)
   
    a. Selecteer **Microsoft Graph**.
   
    b. Klik op **Voltooien**.
4. Klik op **Toepassingsmachtigingen: 0**, en selecteer vervolgens **lezen van alle gegevens van identiteit risico gebeurtenis**.
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)
5. Klik op **Opslaan** onder aan de pagina.
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

## <a name="get-an-access-key"></a>Een toegangssleutel opvragen
1. Klik op de pagina van uw toepassing in de **sleutels** sectie, selecteert u 1 jaar als duur.
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)
2. Klik op **Opslaan** onder aan de pagina.
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)
3. Kopieer de waarde van de zojuist gemaakte sleutel in de sectie sleutels en plak deze in een veilige locatie.
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)
   
   > [!NOTE]
   > Als u deze sleutel kwijtraakt, wordt u terug naar deze sectie en maak een nieuwe sleutel hebben. Een geheim voor deze sleutel behouden: iedereen die deze heeft toegang tot uw gegevens.
   > 
   > 
4. In de **eigenschappen** sectie, Kopieer de **Client-ID**, en plak deze in een veilige locatie. 

## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Verifiëren naar Microsoft Graph en de identiteit risico gebeurtenissen API query
Op dit moment dat u hebt:

* De client-ID die u hierboven hebt gekopieerd
* De sleutel die u hierboven hebt gekopieerd
* De naam van uw tenant-domein

Om te verifiëren, verzendt u een post-aanvraag voor `https://login.microsoft.com` met de volgende parameters in de hoofdtekst:

* grant_type: '**client_credentials**'
* resource: '**https://graph.microsoft.com**'
* client_id:<your client ID>
* client_secret:<your key>

> [!NOTE]
> U moet waarden opgeven voor de **client_id** en de **client_secret** parameter.
> 
> 

Als dit lukt, retourneert deze geen verificatietoken.  
Voor het aanroepen van de API maakt u een header met de volgende parameter:

    `Authorization`=”<token_type> <access_token>"


Bij de verificatie, vindt u het type token en het toegangstoken in het resulterende token.

Deze header als een aanvraag verzenden naar de volgende API-URL:`https://graph.microsoft.com/beta/identityRiskEvents`

Het antwoord, is als dit lukt, een verzameling van identiteit risico's en bijbehorende gegevens in de OData-JSON-indeling, die kan worden geparseerd en verwerkt als naar eigen inzicht.

Hier volgt een voorbeeld van code voor verificatie en het aanroepen van de API met behulp van Powershell.  
Alleen toe te voegen uw client-ID sleutel en tenant-domein.

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
Gefeliciteerd, u uw eerste aanroep naar Microsoft Graph zojuist hebt gemaakt.  
U kunt nu query identiteit risico's en de gegevens echter naar eigen inzicht gebruiken.

Bekijk voor meer informatie over Microsoft Graph en het ontwikkelen van toepassingen met behulp van de Graph API de [documentatie](https://graph.microsoft.io/docs) en nog veel meer op de [Microsoft Graph site](https://graph.microsoft.io/). Zorg er ook bladwijzer voor de [Azure AD Identity Protection API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) pagina met een lijst met alle van de identiteit beveiliging beschikbare API's in de grafiek. Als er nieuwe manieren om te werken met Identity Protection via API toevoegt, ziet u ze op die pagina.

## <a name="additional-resources"></a>Aanvullende bronnen
* [Azure Active Directory Identity Protection](active-directory-identityprotection.md)
* [Typen risicogebeurtenissen die worden gedetecteerd door Azure Active Directory: Identity Protection](active-directory-identityprotection-risk-events-types.md)
* [Microsoft Graph](https://graph.microsoft.io/)
* [Overzicht van Microsoft Graph](https://graph.microsoft.io/docs)
* [Azure AD Identity Protection Service hoofdmap](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)

