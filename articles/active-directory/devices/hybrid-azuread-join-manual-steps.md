---
title: Hybride Azure Active Directory-gekoppelde apparaten handmatig configureren | Microsoft Docs
description: Leer hoe u hybride Azure Active Directory-gekoppelde apparaten handmatig kunt configureren.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/01/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: c85d3ce6ab3e84d454ddbc2550f430b87705c192
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51622173"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-joined-devices-manually"></a>Zelfstudie: Hybride Azure Active Directory-gekoppelde apparaten handmatig configureren 

Met apparaatbeheer in Azure Active Directory (Azure AD) kunt u ervoor zorgen dat uw gebruikers toegang tot uw bronnen hebben vanaf apparaten die voldoen aan uw normen voor beveiliging en naleving. Zie [Inleiding tot apparaatbeheer in Azure Active Directory](overview.md) voor meer informatie.


> [!TIP]
> Zie [Uw scenario selecteren](hybrid-azuread-join-plan.md#select-your-scenario) als het gebruik van Azure AD Connect een optie is voor u. Met behulp van Azure AD Connect kunt u de configuratie van hybride Azure AD-koppeling aanzienlijk vereenvoudigen.



Als u een on-premises Active Directory-omgeving hebt en u uw domein-gekoppelde apparaten aan Azure AD wilt koppelen, kunt u dit doen door hybride Azure AD-gekoppelde apparaten te configureren. In deze zelfstudie leert u hoe u hybride Azure AD-koppeling voor uw apparaten handmatig kunt configureren.

> [!div class="checklist"]
> * Vereisten
> * Configuratiestappen
> * Serviceverbindingspunt configureren
> * Claimuitgifte instellen
> * Downlevel Windows-apparaten inschakelen
> * Gekoppelde apparaten verifiëren
> * Problemen met uw implementatie oplossen
 




## <a name="prerequisites"></a>Vereisten

In deze zelfstudie wordt ervan uitgegaan dat u bekend bent met:
    
-  [Inleiding tot apparaatbeheer in Azure Active Directory](../device-management-introduction.md)
    
-  [De implementatie van uw hybride Azure Active Directory-deelname plannen](hybrid-azuread-join-plan.md)

-  [De hybride Azure AD-deelname van uw apparaten beheren](hybrid-azuread-join-control.md)


Voordat u hybride Azure AD-gekoppelde apparaten begint in te schakelen in uw organisatie, moet u ervoor zorgen dat:

- U een up-to-date versie van Azure AD Connect uitvoert.

- Azure AD Connect de computerobjecten heeft gesynchroniseerd van de apparaten die u hybride Azure AD-gekoppeld wilt maken. Als de computerobjecten bij specifieke organisatie-eenheden (OE) horen, moeten deze OE’s ook worden geconfigureerd voor synchronisatie in Azure AD Connect.

  

Azure AD Connect:

- Behoudt de koppeling tussen het computeraccount in uw on-premises Active Directory (AD) en het apparaatobject in Azure AD. 
- Schakelt andere apparaatgerelateerde functies in, zoals Windows Hello voor Bedrijven.

Zorg ervoor dat de volgende URL’s toegankelijk zijn vanaf computers in uw organisatienetwerk om computers te kunnen registreren bij Azure AD:

- https://enterpriseregistration.windows.net

- https://login.microsoftonline.com Toestaan
- https://device.login.microsoftonline.com

- De STS van uw organisatie (federatieve domeinen)

Als dit nog niet is gedaan, moet de STS van uw organisatie (voor federatieve domeinen) worden opgenomen in de lokale intranetinstellingen van de gebruiker.

Als uw organisatie van plan is naadloze eenmalige aanmelding te gebruiken, moeten de volgende URL’s toegankelijk zijn vanaf de computers in uw organisatie en moeten ze ook worden toegevoegd aan de zone Lokaal intranet van de gebruiker:

- https://autologon.microsoftazuread-sso.com

- Bovendien moet de volgende instelling zijn ingeschakeld in de zone Lokaal intranet van de gebruiker: ‘Statusbalkupdates via scripts toestaan’.

Als uw organisatie beheerde (niet-federatieve) installatie met on-premises AD gebruikt en niet ADFS gebruikt om te federeren met Azure AD, is hybride Azure AD-koppeling op Windows 10 afhankelijk van de synchronisatie van computerobjecten in AD met Azure AD. Zorg ervoor dat organisatie-eenheden (OE’s) die de computerobjecten bevatten die hybride Azure AD-gekoppeld moeten worden, zijn ingeschakeld voor synchronisatie in de Azure AD Connect-synchronisatieconfiguratie.

Voor Windows 10-apparaten met versie 1703 of eerder: als uw organisatie internettoegang via een uitgaande proxy vereist, moet u Web Proxy Auto-Discovery (WPAD) implementeren om Windows 10-computers in staat te stellen apparaten te registreren met Azure AD. 

Vanaf Windows 10 1803 probeert het apparaat het koppelen van de hybride Azure AD te voltooien via de gesynchroniseerde computer of het gesynchroniseerde apparaat, zelfs als het via AD FS koppelen van de hybride Azure AD wordt uitgevoerd vanaf een apparaat in een federatief domein mislukt en Azure AD Connect is geconfigureerd om de computer-/apparaatobjecten te synchroniseren met Azure AD.

## <a name="configuration-steps"></a>Configuratiestappen

U kunt hybride Azure AD-gekoppelde apparaten configureren voor verschillende soorten Windows-apparaatplatforms. Dit onderwerp biedt de vereiste stappen voor alle gebruikelijke configuratiescenario’s.  

In de volgende tabel staat een overzicht van de stappen die vereist zijn voor uw scenario:  



| Stappen                                      | Actueel Windows en wachtwoord-hashsynchronisatie | Actueel Windows en federatie | Downlevel Windows |
| :--                                        | :-:                                    | :-:                            | :-:                |
| Serviceverbindingspunt configureren | ![Selecteren][1]                            | ![Selecteren][1]                    | ![Selecteren][1]        |
| Claimuitgifte instellen           |                                        | ![Selecteren][1]                    | ![Selecteren][1]        |
| Niet-Windows 10-apparaten inschakelen      |                                        |                                | ![Selecteren][1]        |
| Gekoppelde apparaten verifiëren          | ![Selecteren][1]                            | ![Selecteren][1]                    | ![Selecteren][1]        |



## <a name="configure-service-connection-point"></a>Serviceverbindingspunt configureren

Het SCP-object (serviceverbindingspunt) wordt tijdens de registratie door uw apparaten gebruikt om Azure AD-tenantgegevens te ontdekken. In uw on-premises Active Directory (AD) moet het SCP-object voor de hybride Azure AD-gekoppelde apparaten bestaan in de partitie voor de configuratienaamgevingscontext van de forest van de computer. Er is slechts één configuratienaamgevingscontext per forest. In een Active Directory-configuratie met meerdere forests moet het serviceverbindingspunt bestaan in alle forests die domein-gekoppelde computers bevatten.

U kunt de cmdlet [**Get-ADRootDSE**](https://technet.microsoft.com/library/ee617246.aspx) gebruiken om de configuratienaamgevingscontext van uw forest op te halen.  

Voor een forest met de Active Directory-domeinnaam *fabrikam.com* is de configuratienaamgevingscontext:

`CN=Configuration,DC=fabrikam,DC=com`

In uw forest bevindt het SCP-object voor de automatische registratie van domein-gekoppelde apparaten zich op:  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

Afhankelijk van hoe u Azure AD Connect hebt geïmplementeerd, is het SCP-object mogelijk al geconfigureerd.
U kunt het bestaan van het object verifiëren en de detectiewaarden ophalen met behulp van het volgende Windows PowerShell-script: 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

De uitvoer **$scp.Keywords** toont de Azure AD-tenantgegevens, bijvoorbeeld:

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Als het serviceverbindingspunt niet bestaat, kunt u dit maken door de cmdlet `Initialize-ADSyncDomainJoinedComputerSync` uit te voeren op uw Azure AD Connect-server. Om deze cmdlet te kunnen uitvoeren, zijn referenties van een ondernemingsbeheerder nodig.  
De cmdlet:

- Maakt het serviceverbindingspunt in de Active Directory-forest waar Azure AD Connect mee verbonden is. 
- Vereist dat u de parameter `AdConnectorAccount` opgeeft. Dit is het account dat in Azure AD Connect als Active Directory-connectoraccount is geconfigureerd. 


Het volgende script toont een voorbeeld van hoe de cmdlet kan worden gebruikt. In dit script vereist `$aadAdminCred = Get-Credential` dat u een gebruikersnaam typt. U moet de gebruikersnaam in de UPN-indeling (User Principal Name) opgeven (`user@example.com`). 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

De cmdlet `Initialize-ADSyncDomainJoinedComputerSync`:

- Gebruikt de Active Directory PowerShell-module en AD DS-hulpprogramma’s, die afhankelijk zijn van de uitvoering van Active Directory Web Services op een domeincontroller. Active Directory Web Services wordt ondersteund op domeincontrollers waarop Windows Server 2008 R2 of hoger wordt uitgevoerd.
- Wordt alleen ondersteund door **MSOnline PowerShell-module versie 1.1.166.0**. Gebruik deze [koppeling](https://msconfiggallery.cloudapp.net/packages/MSOnline/1.1.166.0/) om deze module te downloaden.   
- Als de AD DS-hulpprogramma’s niet zijn geïnstalleerd, mislukt de `Initialize-ADSyncDomainJoinedComputerSync`.  De AD DS-hulpprogramma’s kunnen worden geïnstalleerd via Serverbeheer onder Functies > Remote Server Administration Tools > Hulpprogramma's voor functiebeheer.

Voor domeincontrollers waarop Windows Server 2008 of een eerdere versie wordt uitgevoerd, gebruikt u het onderstaande script om het serviceverbindingspunt te maken.

In een configuratie met meerdere forests moet u het volgende script gebruiken om het serviceverbindingspunt te maken in elke forest waarin computers bestaan:
 
    $verifiedDomain = "contoso.com"    # Replace this with any of your verified domain names in Azure AD
    $tenantID = "72f988bf-86f1-41af-91ab-2d7cd011db47"    # Replace this with you tenant ID
    $configNC = "CN=Configuration,DC=corp,DC=contoso,DC=com"    # Replace this with your AD configuration naming context

    $de = New-Object System.DirectoryServices.DirectoryEntry
    $de.Path = "LDAP://CN=Services," + $configNC
    $deDRC = $de.Children.Add("CN=Device Registration Configuration", "container")
    $deDRC.CommitChanges()

    $deSCP = $deDRC.Children.Add("CN=62a0ff2e-97b9-4513-943f-0d221bd30080", "serviceConnectionPoint")
    $deSCP.Properties["keywords"].Add("azureADName:" + $verifiedDomain)
    $deSCP.Properties["keywords"].Add("azureADId:" + $tenantID)

    $deSCP.CommitChanges()

In het bovenstaande script:

- is `$verifiedDomain = "contoso.com"` een tijdelijke aanduiding die u moet vervangen door een van uw geverifieerde domeinnamen in Azure AD. U moet eigenaar van het domein zijn voordat u het kunt gebruiken.

Zie [Een aangepaste domeinnaam toevoegen aan Azure Active Directory](../active-directory-domains-add-azure-portal.md) voor meer informatie over geverifieerde domeinnamen.  
Om een lijst met uw geverifieerde bedrijfsdomeinen op te halen, kunt u de cmdlet [Get-AzureADDomain](/powershell/module/Azuread/Get-AzureADDomain?view=azureadps-2.0) gebruiken. 

![Get-AzureADDomain](./media/hybrid-azuread-join-manual-steps/01.png)

## <a name="setup-issuance-of-claims"></a>Claimuitgifte instellen

In een gefedereerde Azure AD-configuratie zijn apparaten afhankelijk van Active Directory Federation Services (AD FS) of een externe on-premises federatieve service om te verifiëren bij Azure AD. Apparaten verifiëren om een toegangstoken te verkrijgen voor registratie bij de Azure Active Directory Device Registration Service (Azure DRS).

Actuele Windows-apparaten verifiëren met behulp van Geïntegreerde Windows-verificatie bij een actief WS-Trust-eindpunt (versie 1.3 of 2005) dat wordt gehost door de on-premises federatieve service.

> [!NOTE]
> Als u AD FS gebruikt, moet **adfs/services/trust/13/windowstransport** of **adfs/services/trust/2005/windowstransport** worden ingeschakeld. Als u de Web Authentication Proxy gebruikt, moet u er ook voor zorgen dat dit eindpunt via de proxy wordt gepubliceerd. Onder **Service > Eindpunten** in de AD FS-beheerconsole kunt u zien welke eindpunten zijn ingeschakeld.
>
>Als u AD FS niet als uw on-premises federatieve service hebt, volgt u de instructies van uw leverancier om te verzekeren dat ze WS-Trust 1.3- of WS-Trust 2005-eindpunten ondersteunen en dat deze via het MEX-bestand (Metadata Exchange) worden gepubliceerd.

De volgende claims moeten bestaan in de token die door Azure DRS wordt ontvangen om de apparaatregistratie te kunnen voltooien. Azure DRS maakt een apparaatobject in Azure AD met sommige van deze gegevens, die vervolgens door Azure AD Connect worden gebruikt om het zojuist gemaakte apparaatobject aan het on-premises computeraccount te koppelen.

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

Als u meer dan één geverifieerde domeinnaam hebt, moet u de volgende claim voor computers opgeven:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

Als u al een ImmutableID-claim (bijv. een alternatieve aanmeldings-ID) uitgeeft, moet u één overeenkomende claim voor computers opgeven:

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

In de volgende secties vindt u informatie over:
 
- De waarden die elke claim moet hebben
- Hoe een definitie eruit zou zien in AD FS

De definitie helpt u te verifiëren dat de waarden aanwezig zijn of dat u ze moet maken.

> [!NOTE]
> Als u AD FS niet gebruikt voor uw on-premises federatieve service, volgt u de instructies van uw leverancier om de toepasselijke configuratie te maken voor het uitgeven van deze claims.

### <a name="issue-account-type-claim"></a>Accounttypeclaim uitgeven

**`http://schemas.microsoft.com/ws/2012/01/accounttype`** - Deze claim moet een **DJ**-waarde bevatten, die het apparaat als een domein-gekoppelde computer identificeert. In AD FS kunt u een uitgiftetransformatieregel toevoegen die er als volgt uitziet:

    @RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );

### <a name="issue-objectguid-of-the-computer-account-on-premises"></a>objectGUID van het on-premises computeraccount uitgeven

**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`** - Deze claim moet de **objectGUID**-waarde van het on-premises computeraccount bevatten. In AD FS kunt u een uitgiftetransformatieregel toevoegen die er als volgt uitziet:

    @RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );
 
### <a name="issue-objectsid-of-the-computer-account-on-premises"></a>objectSID van het on-premises computeraccount uitgeven

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`** - Deze claim moet de **objectSID**-waarde van het on-premises computeraccount bevatten. In AD FS kunt u een uitgiftetransformatieregel toevoegen die er als volgt uitziet:

    @RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);

### <a name="issue-issuerid-for-computer-when-multiple-verified-domain-names-in-azure-ad"></a>issuerID voor computer uitgeven bij meerdere geverifieerde domeinnamen in Azure AD

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`** - Deze claim moet de URI (Uniform Resource Identifier) bevatten van een van de geverifieerde domeinnamen die verbinding maken met de on-premises federatieve service (AD FS of extern) die de token uitgeeft. In AD FS kunt u uitgiftetransformatieregels die eruitzien als de onderstaande, in die specifieke volgorde toevoegen na de bovenstaande regels. Er is één regel nodig om de regel expliciet voor gebruikers uit te geven. In de onderstaande regels wordt een eerste regel toegevoegd waarmee gebruikersverificatie vs. computerverificatie wordt geïdentificeerd.

    @RuleName = "Issue account type with the value User when its not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue the IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://<verified-domain-name>/adfs/services/trust/"
    );


In de bovenstaande claim:

- is `<verified-domain-name>` een tijdelijke aanduiding die u moet vervangen door een van uw geverifieerde domeinnamen in Azure AD, bijvoorbeeld http://contoso.com/adfs/services/trust/



Zie [Een aangepaste domeinnaam toevoegen aan Azure Active Directory](../active-directory-domains-add-azure-portal.md) voor meer informatie over geverifieerde domeinnamen.  

Om een lijst met uw geverifieerde bedrijfsdomeinen op te halen, kunt u de cmdlet [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) gebruiken. 

![Get-MsolDomain](./media/hybrid-azuread-join-manual-steps/01.png)

### <a name="issue-immutableid-for-computer-when-one-for-users-exist-eg-alternate-login-id-is-set"></a>ImmutableID voor computer uitgeven wanneer er een bestaat voor gebruikers (bijv. alternatieve aanmeldings-ID is ingesteld)

**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`** - Deze claim moet een geldige waarde voor computers bevatten. In AD FS kunt u als volgt een uitgiftetransformatieregel maken:

    @RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );

### <a name="helper-script-to-create-the-ad-fs-issuance-transform-rules"></a>Hulpscript om de AD FS-uitgiftetransformatieregels te maken

Het volgende script helpt u bij het maken van de hierboven beschreven uitgiftetransformatieregels.

    $multipleVerifiedDomainNames = $false
    $immutableIDAlreadyIssuedforUsers = $false
    $oneOfVerifiedDomainNames = 'example.com'   # Replace example.com with one of your verified domains
    
    $rule1 = '@RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );'

    $rule2 = '@RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'

    $rule3 = '@RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);'

    $rule4 = ''
    if ($multipleVerifiedDomainNames -eq $true) {
    $rule4 = '@RuleName = "Issue account type with the value User when it is not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue the IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://' + $oneOfVerifiedDomainNames + '/adfs/services/trust/"
    );'
    }

    $rule5 = ''
    if ($immutableIDAlreadyIssuedforUsers -eq $true) {
    $rule5 = '@RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'
    }

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules 

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules 

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString 

### <a name="remarks"></a>Opmerkingen 

- Dit script voegt de regels aan de bestaande regels toe. Voer het script niet tweemaal uit, anders wordt de verzameling regels tweemaal toegevoegd. Zorg dat er geen overeenkomende regels voor deze claims bestaan (onder de overeenkomende voorwaarden) voordat u het script nogmaals uitvoert.

- Als u meerdere geverifieerde domeinnamen hebt (zoals weergegeven in de Azure AD-portal of via de cmdlet Get-MsolDomains), stelt u **$multipleVerifiedDomainNames** in het script op de waarde **$true** in. Zorg ook dat u een bestaande issuerID-claim verwijdert die mogelijk door Azure AD Connect of via een andere methode is gemaakt. Hier is een voorbeeld voor deze regel:


        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- Als u al een **ImmutableID**-claim voor gebruikersaccounts hebt uitgegeven, stelt u **$immutableIDAlreadyIssuedforUsers** in het script op de waarde **$true** in.

## <a name="enable-windows-down-level-devices"></a>Downlevel Windows-apparaten inschakelen

Als sommige van uw domein-gekoppelde apparaten downlevel Windows-apparaten zijn, moet u:

- Een beleid in Azure AD instellen om gebruikers in staat te stellen apparaten te registreren.
 
- Uw on-premises federatieve service configureren voor claimuitgifte om **Geïntegreerde Windows-verificatie (IWA)** te ondersteunen voor apparaatregistratie.
 
- Het Azure AD-apparaatverificatie-eindpunt aan de zone Lokaal intranet toevoegen om certificaatprompts te vermijden wanneer het apparaat wordt geverifieerd.

- Downlevel Windows-apparaten beheren 


### <a name="set-policy-in-azure-ad-to-enable-users-to-register-devices"></a>Een beleid in Azure AD instellen om gebruikers in staat te stellen apparaten te registreren

Om downlevel Windows-apparaten te kunnen registreren, moet u ervoor zorgen dat de instelling is geconfigureerd die gebruikers in staat stellen apparaten te registreren in Azure AD. In de Azure-portal vindt u deze instelling onder:

`Azure Active Directory > Users and groups > Device settings`
    
Het volgende beleid moet zijn ingesteld op **Alle**: **Gebruikers mogen hun apparaten met Azure AD registreren**

![Apparaten registreren](./media/hybrid-azuread-join-manual-steps/23.png)


### <a name="configure-on-premises-federation-service"></a>On-premises federatieve service configureren 

Uw on-premises federatieve service moet de uitgifte van de claims **authenticationmethod** en **wiaormultiauthn** ondersteunen wanneer een verificatieaanvraag wordt ontvangen bij de Azure AD Relying Party, die een resource_params-parameter met een gecodeerde waarde bevatten, zoals hieronder weergegeven:

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

Wanneer een dergelijke aanvraagt wordt ontvangen, moet de on-premises federatieve service de gebruiker verifiëren met behulp van Geïntegreerde Windows-verificatie, en indien succesvol de volgende twee claims uitgeven:

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

In AD FS moet u een uitgiftetransformatieregel toevoegen die wordt doorgegeven via de verificatiemethode.  

**Ga als volgt te werk om deze regel toe te voegen:**

1. Ga in de AD FS-beheerconsole naar `AD FS > Trust Relationships > Relying Party Trusts`.
2. Klik met de rechtermuisknop op het object Relying Party-vertrouwensrelatie van het identiteitsplatform van Microsoft Office 365 en selecteer vervolgens **Claimregels bewerken**.
3. Selecteer op het tabblad **Uitgiftetransformatieregels** de optie **Regel toevoegen**.
4. Selecteer in de sjabloonlijst **Claimregel** de optie **Claim verzenden met een aangepaste regel**.
5. Selecteer **Volgende**.
6. In het vak **Naam claimregel** typt u **Claimregel voor verificatiemethode**.
7. In het vak **Claimregel** typt u de volgende regel:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. In uw federatieve service typt u de onderstaande PowerShell-opdracht nadat u **\<RPObjectName\>** hebt vervangen door de Relying Party-objectnaam voor uw Azure AD-object Relying Party-vertrouwensrelatie. Dit object heet meestal **Identiteitsplatform van Microsoft Office 365**.
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-the-azure-ad-device-authentication-end-point-to-the-local-intranet-zones"></a>Het Azure AD-apparaatverificatie-eindpunt aan de zone Lokaal intranet toevoegen

Om certificaatprompts te vermijden wanneer gebruikers in registerapparaten verifiëren bij Azure AD, kunt u een beleid naar uw domein-gekoppelde apparaten pushen om de volgende URL toe te voegen aan de zone Lokaal intranet in Internet Explorer:

`https://device.login.microsoftonline.com`


### <a name="control-windows-down-level-devices"></a>Downlevel Windows-apparaten beheren 

Als u downlevel Windows-apparaten wilt registreren, dient u een Windows Installer-pakket (.msi) in het Downloadcentrum te downloaden en installeren. Klik [hier](hybrid-azuread-join-control.md#control-windows-down-level-devices) voor meer informatie. 



## <a name="verify-joined-devices"></a>Gekoppelde apparaten verifiëren

U kunt succesvol gekoppelde apparaten in uw organisatie controleren met behulp van de cmdlet [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) in de [Azure Active Directory PowerShell-module](/powershell/azure/install-msonlinev1?view=azureadps-2.0).

De uitvoer van deze cmdlet toont apparaten die zijn geregistreerd en gekoppeld met Azure AD. Om alle apparaten te bekijken, gebruikt u de parameter **-All** en filtert u ze vervolgens met behulp van de eigenschap **deviceTrustType**. Domein-gekoppelde apparaten hebben de waarde **Toegevoegd aan domein**.



## <a name="troubleshoot-your-implementation"></a>Problemen met uw implementatie oplossen

Als u problemen ondervindt met het voltooien van de hybride Azure AD-koppeling voor domein-gekoppelde Windows-apparaten, raadpleegt u:

- [Problemen met hybride Azure AD-koppeling oplossen voor actuele Windows-apparaten](troubleshoot-hybrid-join-windows-current.md)
- [Problemen met hybride Azure AD-koppeling oplossen voor downlevel Windows-apparaten](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot apparaatbeheer in Azure Active Directory](overview.md)



<!--Image references-->
[1]: ./media/hybrid-azuread-join-manual-steps/12.png
