---
title: Microsoft Graph voor Azure Active Directory Identity Protection | Microsoft Docs
description: Leer hoe u Microsoft Graph op te vragen voor een lijst met risico's en bijbehorende gegevens uit Azure Active Directory.
services: active-directory
keywords: Azure active directory identity protection, risicogebeurtenis, beveiligingsproblemen, beveiligingsbeleid, Microsoft Graph
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: fa109ba7-a914-437b-821d-2bd98e681386
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2019
ms.author: markvi
ms.reviewer: nigu
ms.custom: seohack1
ms.openlocfilehash: b82458de95014d22625a9c8029e064ed21120488
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55158295"
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Aan de slag met Azure Active Directory Identity Protection en Microsoft Graph

Microsoft Graph is de Microsoft unified-API-eindpunt en het hart van [Azure Active Directory Identity Protection](../active-directory-identityprotection.md) API's. Er zijn drie API's die beschikbaar maken van informatie over riskante gebruikers en -aanmeldingen. De eerste API **identityRiskEvents**, kunt u Microsoft Graph op te vragen voor een lijst met [risicogebeurtenissen](../reports-monitoring/concept-risk-events.md) en informatie die is gekoppeld. De tweede API **riskyUsers**, kunt u Microsoft Graph-query voor informatie over gebruikers Identity Protection als risico gedetecteerd. De derde API **signIn**, kunt u Microsoft Graph op te vragen voor meer informatie over Azure AD-aanmeldingen met specifieke eigenschappen met betrekking tot de status van de risico's, informatie en niveau. In dit artikel helpt u op weg met [verbinding te maken met de Microsoft Graph](#Connect-to-Microsoft-Graph) en [uitvoeren van query's deze API's](#Query-the-APIs). Zie voor een diepgaande inleiding, de volledige documentatie en de toegang tot de Graph Explorer, de [Microsoft Graph-site](https://graph.microsoft.io/) of de specifieke documentatie bij deze API's:

* [identityRiskEvents API](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/identityriskevent)
* [riskyUsers API](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/riskyuser)
* [signIn API](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/signin)


## <a name="connect-to-microsoft-graph"></a>Verbinding maken met Microsoft graph

Er zijn vier stappen voor het verkrijgen van toegang tot Identity Protection gegevens via Microsoft Graph:

1. Naam van het domein worden opgehaald.
2. Maak een nieuwe app-registratie. 
3. Dit geheim en enkele andere stukjes informatie gebruiken om te verifiëren bij Microsoft Graph, waar u een verificatietoken hebt ontvangen. 
4. Gebruik dit token aanvragen versturen naar de API-eindpunt en Identity Protection gegevens terugontvangen.

Voordat u begint, moet u op:

* Beheerdersbevoegdheden voor het maken van de toepassing in Azure AD
* De naam van van uw tenant-domein (bijvoorbeeld contoso.onmicrosoft.com)


## <a name="retrieve-your-domain-name"></a>Uw domeinnaam ophalen 

1. [Meld u aan](https://portal.azure.com) naar uw Azure-portal als beheerder. 

2. Klik in het linkernavigatiedeelvenster op **Active Directory**. 
   
    ![Het maken van een toepassing](./media/graph-get-started/41.png)


3. In de **beheren** sectie, klikt u op **eigenschappen**.

    ![Het maken van een toepassing](./media/graph-get-started/42.png)

4. Kopieer de domeinnaam van uw.


## <a name="create-a-new-app-registration"></a>Maak een nieuwe app-registratie

1. Op de **Active Directory** pagina, in de **beheren** sectie, klikt u op **App-registraties**.

    ![Het maken van een toepassing](./media/graph-get-started/42.png)


2. Klik in het menu aan de bovenkant op **nieuwe toepassing registreren**.
   
    ![Het maken van een toepassing](./media/graph-get-started/43.png)

3. Op de **maken** pagina, voert u de volgende stappen uit:
   
    ![Het maken van een toepassing](./media/graph-get-started/44.png)

    a. In de **naam** tekstvak, typ een naam voor uw toepassing (bijvoorbeeld: AADIP risico Event-API toepassing).
   
    b. Als **Type**, selecteer **webtoepassing en / of Web-API**.
   
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

- grant_type: “**client_credentials**”

-  resource: '**https://graph.microsoft.com**"

- client_id: \<uw client-ID\>

- waarde voor client_secret: \<uw sleutel\>


Als dit lukt, retourneert deze een verificatietoken.  
Voor het aanroepen van de API, een koptekst te maken met de volgende parameter:

    `Authorization`=”<token_type> <access_token>"


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

## <a name="query-the-apis"></a>Query uitvoeren op de API 's

Deze drie API's bieden een groot aantal verkoopkansen informatie ophalen over riskante gebruikers en aanmeldingen in uw organisatie. Hieronder vindt u enkele veelvoorkomende use cases voor deze API's en de bijbehorende voorbeeldaanvragen. U kunt deze query's uitvoeren met de voorbeeldcode boven of met behulp van [Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer).

### <a name="get-the-high-risk-and-medium-risk-events-identityriskevents-api"></a>Ophalen van de gebeurtenissen met een hoog risico en gemiddeld risico (identityRiskEvents API)

Gemiddeld en hoog risico gebeurtenissen vertegenwoordigen die mogelijk de mogelijkheid voor het gebruikersrisico beleidsregels of trigger aanmelding Identity Protection. Omdat ze beschikken over een medium of grote kans dat de gebruiker die probeert om aan te melden is niet de eigenaar van de geldige identiteit herstellen van deze gebeurtenissen moeten prioriteit hebben. 

```
GET https://graph.microsoft.com/beta/identityRiskEvents?`$filter=riskLevel eq 'high' or riskLevel eq 'medium'" 
```

### <a name="get-all-of-the-users-who-successfully-passed-an-mfa-challenge-triggered-by-risky-sign-ins-policy-riskyusers-api"></a>Alle van de gebruikers die is doorgegeven een MFA-controle geactiveerd door beleid voor riskante aanmeldingen (riskyUsers API)

U kunt alle gebruikers die is doorgegeven een MFA-controle geactiveerd door een beleid voor riskante aanmeldingen opvragen voor meer informatie over de impact van identiteitsbeveiliging risico's gebaseerd beleid in uw organisatie. Deze informatie kunt u inzicht in welke gebruikers Identity Protection kan hebben om vastgesteld op risico's en welke van uw legitieme gebruikers kunnen worden acties uitvoeren die de AI riskant acht.

```
GET https://graph.microsoft.com/beta/riskyUsers?$filter=riskDetail eq 'userPassedMFADrivenByRiskBasedPolicy'
```

### <a name="get-all-the-risky-sign-ins-for-a-specific-user-signin-api"></a>Alle riskante aanmeldingen voor een specifieke gebruiker (aanmelding API) ophalen

Als u denkt dat een gebruiker mogelijk zijn aangetast dat, kunt u beter beoordelen wat de status van de risico's door op te halen van alle hun riskante aanmeldingen. 
```
https://graph.microsoft.com/beta/identityRiskEvents?`$filter=userID eq '<userID>' and riskState eq 'atRisk'
```




# <a name="next-steps"></a>Volgende stappen

Gefeliciteerd, u de eerste aanroep van Microsoft Graph zojuist hebt gemaakt.  
U kunt nu query risicogebeurtenissen voor identiteiten en gebruikmaken van de gegevens echter naar eigen inzicht.


Bekijk voor meer informatie over Microsoft Graph en over het bouwen van toepassingen die gebruikmaken van de Graph API, de [documentatie](https://docs.microsoft.com/en-us/graph/overview) en nog veel meer op de [Microsoft Graph-site](https://developer.microsoft.com/en-us/graph). 


Zie voor meer informatie:

-  [Azure Active Directory Identity Protection](../active-directory-identityprotection.md)

-  [Typen risicogebeurtenissen die worden gedetecteerd door Azure Active Directory Identity Protection](../reports-monitoring/concept-risk-events.md)

- [Microsoft Graph](https://developer.microsoft.com/graph/)

- [Overzicht van Microsoft Graph](https://developer.microsoft.com/graph/docs)

- [Azure AD Identity Protection-Service-hoofdmap](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityprotection_root)
