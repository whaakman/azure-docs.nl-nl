---
title: Microsoft Graph voor beveiliging van Azure Active Directory-identiteit | Microsoft Docs
description: Informatie over het Microsoft Graph zoeken naar een lijst met risico's en bijbehorende gegevens van Azure Active Directory.
services: active-directory
keywords: beveiliging voor Azure active directory-identiteit, risicogebeurtenis, beveiligingsprobleem, beveiligingsbeleid, Microsoft Graph
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: fa109ba7-a914-437b-821d-2bd98e681386
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: markvi
ms.reviewer: nigu
ms.custom: seohack1
ms.openlocfilehash: df0d89fc93f1b9c19d669c29306398a8b25ee425
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Aan de slag met Azure Active Directory: Identity Protection en Microsoft Graph
Microsoft Graph is de Microsoft unified API-eindpunt en het hart van [Azure Active Directory: Identity Protection](active-directory-identityprotection.md) API's. De eerste API **identityRiskEvents**, kunt u Microsoft Graph opvragen voor een lijst met [bestaat de kans dat gebeurtenissen](active-directory-identityprotection-risk-events-types.md) en informatie die is gekoppeld. In dit artikel helpt u op weg met het opvragen van deze API. Zie voor een gedetailleerde inleiding, volledige documentatie en toegang tot de grafiek Explorer, de [Microsoft Graph site](https://graph.microsoft.io/).


Er zijn vier stappen Identity Protection gegevenstoegang via Microsoft Graph:

1. Uw domeinnaam ophalen.
2. Maak een nieuwe app-registratie. 
2. Gebruik dit geheim en enkele andere stukjes informatie om Microsoft Graph, waarbij er geen verificatietoken te verifiëren. 
3. Gebruik dit token aanvragen in de API-eindpunt aanbrengen en terughalen van gegevens voor de beveiliging van de identiteit.

Voordat u begint, hebt u nodig:

* Beheerdersrechten voor de toepassing in Azure AD maken
* De naam van uw tenant-domein (bijvoorbeeld: contoso.onmicrosoft.com)


## <a name="retrieve-your-domain-name"></a>Uw domeinnaam ophalen 

1. [Meld u aan](https://portal.azure.com) naar uw Azure-portal als beheerder. 

2. Klik in het navigatiedeelvenster links op **Active Directory**. 
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/41.png)


3. In de **beheren** sectie, klikt u op **eigenschappen**.

    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/42.png)

4. Kopieer uw domeinnaam.


## <a name="create-a-new-app-registration"></a>Maak een nieuwe app-registratie

1. Op de **Active Directory** pagina in de **beheren** sectie, klikt u op **App registraties**.

    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/42.png)


2. Klik in het menu bovenaan op **registratie van de nieuwe toepassing**.
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/43.png)

3. Op de **maken** pagina, voert u de volgende stappen uit:
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/44.png)

    a. In de **naam** textbox, typ een naam voor uw toepassing (bijvoorbeeld: AADIP risico gebeurtenis API toepassing).
   
    b. Als **Type**, selecteer **webtoepassing en / of Web-API**.
   
    c. In de **aanmeldings-URL** textbox type `http://localhost`.

    d. Klik op **Create**.

4. Openen van de **instellingen** pagina in de lijst met toepassingen, klikt u op de registratie van uw nieuwe app. 

5. Kopieer de **toepassings-ID**.


## <a name="grant-your-application-permission-to-use-the-api"></a>Uw toepassing toestemming de API gebruiken

1. Op de **instellingen** pagina, klikt u op **vereist machtigingen**.
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/15.png)

2. Op de **vereist machtigingen** pagina in de werkbalk bovenaan, klikt u op **toevoegen**.
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/16.png)
   
3. Op de **API toevoegen toegang** pagina, klikt u op **selecteert u een API**.
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/17.png)

4. Op de **selecteert u een API** pagina **Microsoft Graph**, en klik vervolgens op **Selecteer**.
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/18.png)

5. Op de **API toevoegen toegang** pagina, klikt u op **machtigingen selecteren**.
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/19.png)

6. Op de **toegang inschakelen** pagina, klikt u op **lezen van alle gegevens van identiteit risico**, en klik vervolgens op **Selecteer**.
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/20.png)

7. Op de **API toevoegen toegang** pagina, klikt u op **gedaan**.
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/21.png)

8. Op de **Required Permissions** pagina, klikt u op **machtiging verlenen**, en klik vervolgens op **Ja**.
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/22.png)



## <a name="get-an-access-key"></a>Een toegangssleutel opvragen

1. Op de **instellingen** pagina, klikt u op **sleutels**.
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/23.png)

2. Op de **sleutels** pagina, voert u de volgende stappen uit:
   
    ![Maken van een toepassing](./media/active-directory-identityprotection-graph-getting-started/24.png)

    a. In de **sleutel beschrijving** textbox, typ een beschrijving (bijvoorbeeld *AADIP risicogebeurtenis*).
    
    b. Als **duur**, selecteer **In 1 jaar**.

    c. Klik op **Opslaan**.
   
    d. De sleutelwaarde Kopieer en plak deze in een veilige locatie.   
   
   > [!NOTE]
   > Als u deze sleutel kwijtraakt, wordt u terug naar deze sectie en maak een nieuwe sleutel hebben. Een geheim voor deze sleutel behouden: iedereen die deze heeft toegang tot uw gegevens.
   > 
   > 

## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Verifiëren naar Microsoft Graph en de identiteit risico gebeurtenissen API query
Op dit moment dat u hebt:

- De naam van uw tenant-domein

- De client-ID 

- De sleutel 


Om te verifiëren, verzendt u een post-aanvraag voor `https://login.microsoft.com` met de volgende parameters in de hoofdtekst:

- grant_type: '**client_credentials**'

-  resource: '**https://graph.microsoft.com**'

- client_id: \<uw client-ID\>

- client_secret: \<uw sleutel\>


Als dit lukt, retourneert deze geen verificatietoken.  
Voor het aanroepen van de API maakt u een header met de volgende parameter:

    `Authorization`=”<token_type> <access_token>"


Bij de verificatie, vindt u het type token en het toegangstoken in het resulterende token.

Deze header als een aanvraag verzenden naar de volgende API-URL:`https://graph.microsoft.com/beta/identityRiskEvents`

Het antwoord, is als dit lukt, een verzameling van identiteit risico's en bijbehorende gegevens in de OData-JSON-indeling, die kan worden geparseerd en wens naar verwerkt.

Hier volgt een voorbeeld van code voor verificatie en het aanroepen van de API met behulp van PowerShell.  
Voeg toe uw client-ID, de geheime sleutel en het tenant-domein.

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

Zie voor meer informatie:

-  [Azure Active Directory Identity Protection](active-directory-identityprotection.md)

-  [Typen risicogebeurtenissen die worden gedetecteerd door Azure Active Directory: Identity Protection](active-directory-identityprotection-risk-events-types.md)

- [Microsoft Graph](https://graph.microsoft.io/)

- [Overzicht van Microsoft Graph](https://graph.microsoft.io/docs)

- [Azure AD Identity Protection Service hoofdmap](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)

