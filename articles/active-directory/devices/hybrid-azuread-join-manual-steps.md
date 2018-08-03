---
title: Hybride Azure Active Directory verbonden apparaten handmatig configureren | Microsoft Docs
description: Informatie over het handmatig configureren van hybride Azure Active Directory verbonden apparaten.
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
ms.topic: article
ms.date: 08/02/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 2ee54ca3d6e787267010736343a570e614c4204d
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39427547"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-joined-devices-manually"></a>Zelfstudie: Hybride Azure Active Directory verbonden apparaten handmatig configureren 

Met Apparaatbeheer in Azure Active Directory (Azure AD), kunt u ervoor zorgen dat uw gebruikers toegang hebben tot de bronnen van apparaten die voldoen aan uw normen voor beveiliging en naleving. Zie voor meer informatie de [Inleiding tot Apparaatbeheer in Azure Active Directory](overview.md).

Als u een on-premises Active Directory-omgeving hebt en u wilt deelnemen aan uw domein apparaten naar Azure AD, kunt u dit doen met hybride Azure AD gekoppelde apparaten configureren. In dit artikel biedt u de bijbehorende stappen. 



> [!TIP]
> Als u met Azure AD Connect is een optie voor u, raadpleegt u [selecteert u uw scenario](hybrid-azuread-join-plan.md#select-your-scenario). Met behulp van Azure AD Connect, kunt u de configuratie van hybride Azure AD join aanzienlijk eenvoudiger.


## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint met het configureren van hybride Azure AD gekoppelde apparaten in uw omgeving, moet u uzelf vertrouwd met de ondersteunde scenario's en de beperkingen.  

Als u de [hulpprogramma voor systeemvoorbereiding (Sysprep)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc721940(v=ws.10)), zorg ervoor dat u installatiekopieën maken van een installatie van Windows die niet nog is geregistreerd bij Azure AD.

Alle domein-apparaten wordt uitgevoerd Windows 10 Verjaardag Update en Windows Server 2016 automatisch wordt geregistreerd bij Azure AD op herstart van het apparaat of gebruiker aanmelden als de hieronder vermelde configuratiestappen voltooid zijn. **Als dit probleem automatisch registreren niet aanbevolen wordt of een beheerde implementatie desgewenst**,. Volg de instructies in de sectie 'Stap 4: beheer implementatie en Rollout' hieronder om selectief in- of uitschakelen van automatische implementatie voordat u de andere configuratiestappen te volgen.  

Ter verbetering van de leesbaarheid van de beschrijvingen wordt in dit artikel de volgende vermelding: 

- **Huidige Windows-apparaten** -deze term heeft betrekking op domein apparaten met Windows 10 of Windows Server 2016.
- **Windows downlevel-apparaten** -deze term heeft betrekking op alle **ondersteund** domein Windows-apparaten die geen actieve Windows 10 of Windows Server 2016.  

### <a name="windows-current-devices"></a>Huidige Windows-apparaten

- Voor apparaten met het besturingssysteem voor Windows-bureaublad, de ondersteunde versie is de Windows 10 Jubileumupdate (versie 1607) of hoger. 
- De registratie van de huidige Windows-apparaten **is** in niet-gefedereerde omgevingen, zoals wachtwoord-hash gesynchroniseerde configuraties ondersteund.  


### <a name="windows-down-level-devices"></a>Windows downlevel-apparaten

- De volgende Windows downlevel-apparaten worden ondersteund:
    - Windows 8.1
    - Windows 7
    - Windows Server 2012 R2
    - Windows Server 2012
    - Windows Server 2008 R2
- De registratie van Windows downlevel-apparaten **is** in omgevingen met niet-gefedereerde via naadloze eenmalige aanmelding ondersteund [Azure Active Directory naadloze eenmalige aanmelding](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start). 
- De registratie van Windows downlevel-apparaten **is niet** ondersteund bij het gebruik van Azure AD Pass-through-verificatie zonder naadloze eenmalige aanmelding.
- De registratie van Windows downlevel-apparaten **is niet** ondersteund voor apparaten die gebruikmaken van zwervende profielen. Als u zwervende profielen of instellingen vertrouwen, gebruikt u Windows 10.



## <a name="prerequisites"></a>Vereisten

Voordat u begint met het inschakelen van hybride Azure AD gekoppelde apparaten in uw organisatie, moet u om ervoor te zorgen dat:

- U gebruikt een actuele versie van Azure AD connect.

- Azure AD connect is de computerobjecten van de apparaten die u wilt worden toegevoegd aan hybrid Azure AD toegevoegd aan Azure AD gesynchroniseerd. Verbinding maken ook de computerobjecten behoren tot specifieke organisatie-eenheden (OE), en vervolgens deze organisatie-eenheden moeten worden geconfigureerd voor synchronisatie in Azure AD.

  

Azure AD Connect:

- Houdt u de koppeling tussen het computeraccount in uw on-premises Active Directory (AD) en het apparaatobject in Azure AD. 
- Hiermee stelt u andere apparaat gerelateerde functies zoals Windows Hello voor bedrijven.

Zorg ervoor dat de volgende URL's toegankelijk vanaf computers in het netwerk van uw organisatie voor de registratie van computers met Azure AD zijn:

- https://enterpriseregistration.windows.net

- https://login.microsoftonline.com Toestaan
- https://device.login.microsoftonline.com

- Uw organisatie STS (federatieve domeinen)

Als u niet hebt gedaan, moeten de STS (voor federatieve domeinen) van uw organisatie worden opgenomen in de gebruikersinstellingen lokaal intranet.

Als uw organisatie is plan te gebruiken van naadloze eenmalige aanmelding, zijn de volgende URL's moeten bereikbaar zijn vanaf de computers binnen uw organisatie en ze moeten ook worden toegevoegd aan de lokale intranetzone van de gebruiker:

- https://autologon.microsoftazuread-sso.com

- Bovendien de volgende instelling in de intranetzone van de gebruiker moet worden ingeschakeld: 'Balk statusupdates via scripts toestaan'.

Als uw organisatie gebruikmaakt van beheerde (niet-gefedereerde)-installatie met behulp van on-premises AD en maakt geen gebruik van ADFS om te federeren met Azure AD en hybride Azure AD join op Windows 10 is afhankelijk van de computerobjecten in AD worden sync'ed naar Azure AD. Zorg ervoor dat een organisatie-eenheden (OE) waarin de computerobjecten die moeten worden toegevoegd aan hybrid Azure AD zijn ingeschakeld voor synchronisatie in de configuratie van de Azure AD Connect-synchronisatie.

Als uw organisatie toegang tot Internet via een uitgaande proxy vereist, moet u voor Windows 10-apparaten met versie 1703 of eerder, Web Proxy Auto-Discovery (WPAD) om in te schakelen om u te registreren bij Azure AD Windows 10-computers implementeren. 

## <a name="configuration-steps"></a>Configuratiestappen

U kunt hybride Azure AD gekoppelde apparaten voor verschillende typen Windows-apparaatplatforms configureren. Dit onderwerp bevat de vereiste stappen voor alle standaardconfiguratie-scenario's.  

Gebruik de volgende tabel voor een overzicht van de stappen die vereist voor uw scenario zijn:  



| Stappen                                      | Windows huidige en het wachtwoord-hash-synchronisatie | Huidige Windows- en Federatie | Oudere versies van Windows |
| :--                                        | :-:                                    | :-:                            | :-:                |
| Het service connection point configureren | ![Selecteren][1]                            | ![Selecteren][1]                    | ![Selecteren][1]        |
| Uitgifte van claims instellen           |                                        | ![Selecteren][1]                    | ![Selecteren][1]        |
| Windows 10-apparaten inschakelen      |                                        |                                | ![Selecteren][1]        |
| Besturingselement voor implementatie     | ![Selecteren][1]                            | ![Selecteren][1]                    | ![Selecteren][1]        |
| Controleer of de gekoppelde apparaten          | ![Selecteren][1]                            | ![Selecteren][1]                    | ![Selecteren][1]        |



## <a name="configure-service-connection-point"></a>Het service connection point configureren

Het service connection point (SCP)-object wordt gebruikt door uw apparaten tijdens de registratie voor het detecteren van informatie over Azure AD-tenant. In uw on-premises Active Directory (AD) moet het SCP-object voor de hybride Azure AD gekoppelde apparaten zich in de configuratie van de naam van de partitie van de context van de forest van de computer. Er is slechts één configuratienaamgevingscontext per forest. In een configuratie met meerdere forests Active Directory, moet de service connection point in alle forests met domein computers bestaan.

U kunt de [ **Get-ADRootDSE** ](https://technet.microsoft.com/library/ee617246.aspx) cmdlet voor het ophalen van de configuratienaamgevingscontext van het forest.  

Voor een forest met de naam van de Active Directory-domein *fabrikam.com*, is de configuratienaamgevingscontext:

`CN=Configuration,DC=fabrikam,DC=com`

In het forest zich het SCP-object voor de automatische registratie van apparaten domein bevindt hier:  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

Afhankelijk van hoe u Azure AD Connect hebt geïmplementeerd, kan het SCP-object al zijn geconfigureerd.
U kunt controleren of er sprake van het object en ophalen van de detectie-waarden met behulp van de volgende Windows PowerShell-script: 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

De **$scp. Trefwoorden** uitvoer toont de Azure AD-tenant-gegevens, bijvoorbeeld:

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Als de service connection point niet bestaat, kunt u dit maken door het uitvoeren van de `Initialize-ADSyncDomainJoinedComputerSync` cmdlet uit op uw Azure AD Connect-server. Referenties voor Enterprise-beheerder is vereist voor deze cmdlet uitvoert.  
De cmdlet:

- Hiermee maakt u de service connection point in Azure AD Connect is verbonden met Active Directory-forest. 
- U moet opgeven de `AdConnectorAccount` parameter. Dit is het account dat is geconfigureerd als Active Directory connector-account in Azure AD connect. 


Het volgende script toont een voorbeeld voor het gebruik van de cmdlet. In dit script `$aadAdminCred = Get-Credential` , moet u een gebruikersnaam typt. U moet de naam van de gebruiker in de indeling van de principal-naam (User Principal Name) gebruiker opgeven (`user@example.com`). 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

De `Initialize-ADSyncDomainJoinedComputerSync` cmdlet:

- Maakt gebruik van de Active Directory PowerShell-module en AD DS-hulpprogramma's, die afhankelijk zijn van Active Directory Web Services die worden uitgevoerd op een domeincontroller. Active Directory Web Services wordt ondersteund op domeincontrollers met Windows Server 2008 R2 en hoger.
- Wordt alleen ondersteund door de **MSOnline PowerShell-moduleversie 1.1.166.0**. Gebruik deze module downloaden, deze [koppeling](https://msconfiggallery.cloudapp.net/packages/MSOnline/1.1.166.0/).   
- Als de AD DS-hulpprogramma's zijn niet geïnstalleerd, de `Initialize-ADSyncDomainJoinedComputerSync` mislukken.  De AD DS-hulpprogramma's kunnen worden geïnstalleerd via Serverbeheer onder functies - Remote Server Administration Tools - rol beheerprogramma's.

Gebruik het onderstaande script te maken van de service connection point voor domeincontrollers met Windows Server 2008 of eerdere versies.

In een configuratie met meerdere forests, moet u het volgende script gebruiken om te maken van de service connection point in elk forest waar computers bestaan:
 
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

In het bovenstaande script

- `$verifiedDomain = "contoso.com"` is een tijdelijke aanduiding die u wilt vervangen door een van de namen van uw geverifieerde domeinnaam in Azure AD. U moet eigenaar van het domein voordat u deze kunt gebruiken.

Zie voor meer informatie over geverifieerde domeinnamen [een aangepaste domeinnaam toevoegen aan Azure Active Directory](../active-directory-domains-add-azure-portal.md).  
Als u een lijst van uw bedrijf met geverifieerde domeinen, kunt u de [Get-AzureADDomain](/powershell/module/Azuread/Get-AzureADDomain?view=azureadps-2.0) cmdlet. 

![Get-AzureADDomain](./media/hybrid-azuread-join-manual-steps/01.png)

## <a name="setup-issuance-of-claims"></a>Uitgifte van claims instellen

In een federatieve Azure AD-configuratie, de apparaten zijn afhankelijk van de Active Directory Federation Services (AD FS) of een 3e partij on-premises federation-service om te verifiëren met Azure AD. Apparaten worden geverifieerd voor een toegangstoken om te registreren op basis van de Azure Active Directory Device Registration Service (Azure DRS).

Windows huidige apparaten worden geverifieerd met behulp van geïntegreerde Windows-verificatie naar een actieve WS-Trust-eindpunt (versie 1.3 of 2005) die door de federation-service voor on-premises worden gehost.

> [!NOTE]
> Bij het gebruik van AD FS, ofwel **adfs/services/trust/13/windowstransport** of **adfs/services/trust/2005/windowstransport** moet zijn ingeschakeld. Als u de webproxy verificatie gebruikt, zorg er ook voor dat dit eindpunt is gepubliceerd via de proxy. U kunt zien welke eindpunten worden ingeschakeld via de AD FS-beheerconsole onder **Service > eindpunten**.
>
>Als u geen AD FS als uw on-premises federation-service, volg de instructies van uw leverancier om te controleren of dat ze ondersteuning voor WS-Trust 1.3 of 2005 eindpunten en deze worden gepubliceerd via de metagegevens van Exchange bestand (MEX).

De volgende claims moeten bestaan in het token dat is ontvangen door Azure DRS voor device Registration service om te voltooien. Azure DRS maakt een apparaatobject in Azure AD met een aantal van deze gegevens die vervolgens worden gebruikt door Azure AD Connect naar de zojuist gemaakte apparaatobject koppelen aan de computer account on-premises.

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

Als u meer dan een geverifieerde domeinnaam hebt, moet u bieden de volgende claim voor computers:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

Als u al een ImmutableID-claim (bijvoorbeeld alternatieve aanmeldings-ID) u moet een overeenkomende claim voor computers opgeven:

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

In de volgende secties vindt u informatie over:
 
- De waarden elke claim moet hebben.
- Hoe een definitie eruit als in AD FS

De definitie helpt u om te controleren of de waarden aanwezig zijn of als u nodig hebt om ze te maken.

> [!NOTE]
> Als u AD FS niet voor uw on-premises-federatieserver gebruikt, instructies van uw leverancier om de juiste configuratie voor het uitgeven van deze claims te maken.

### <a name="issue-account-type-claim"></a>Probleem account type claim

**`http://schemas.microsoft.com/ws/2012/01/accounttype`** -Deze claim moet een waarde van bevatten **DJ**, waarin het apparaat als een domein computer. In AD FS, kunt u een regel voor het transformeren van uitgifte die er als uitzien volgt toevoegen:

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

### <a name="issue-objectguid-of-the-computer-account-on-premises"></a>Probleem objectGUID van de computer account on-premises

**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`** -Deze claim moet bevatten de **objectGUID** waarde van het account lokale computer. In AD FS, kunt u een regel voor het transformeren van uitgifte die er als uitzien volgt toevoegen:

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
 
### <a name="issue-objectsid-of-the-computer-account-on-premises"></a>ObjectSID probleem van de computer account on-premises

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`** -Deze claim moet bevatten de **objectSid** waarde van het account lokale computer. In AD FS, kunt u een regel voor het transformeren van uitgifte die er als uitzien volgt toevoegen:

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

### <a name="issue-issuerid-for-computer-when-multiple-verified-domain-names-in-azure-ad"></a>IssuerID voor computer uitgeven wanneer meerdere domeinnamen in Azure AD geverifieerd

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`** -Deze claim moet de id URI (Uniform Resource) van een van de geverifieerde domeinnamen die verbinding met de lokale federation-service (AD FS of 3e partij maken) bevatten voor het uitgeven van het token. U kunt de transformatieregels voor uitgifte die er zoals hieronder in die specifieke volgorde na de bovenstaande prijzen uitzien toevoegen in AD FS. Houd er rekening mee dat een regel om uit te geven expliciet de regel voor gebruikers nodig is. Een eerste regel voor het identificeren van gebruiker en computerverificatie wordt in de onderstaande regels toegevoegd.

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


In de bovenstaande claim

- `<verified-domain-name>` is een tijdelijke aanduiding die u wilt vervangen door een van de namen van uw geverifieerde domeinnaam in Azure AD. Bijvoorbeeld, Value = "http://contoso.com/adfs/services/trust/"



Zie voor meer informatie over geverifieerde domeinnamen [een aangepaste domeinnaam toevoegen aan Azure Active Directory](../active-directory-domains-add-azure-portal.md).  

Als u een lijst van uw bedrijf met geverifieerde domeinen, kunt u de [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) cmdlet. 

![Get-MsolDomain](./media/hybrid-azuread-join-manual-steps/01.png)

### <a name="issue-immutableid-for-computer-when-one-for-users-exist-eg-alternate-login-id-is-set"></a>ImmutableID uitgeven voor computer wanneer er voor gebruikers bestaat (bijvoorbeeld alternatieve aanmeldings-ID is ingesteld)

**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`** -Deze claim moet een geldige waarde voor computers bevatten. In AD FS, kunt u als volgt een uitgifte-transformatieregel maken:

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

### <a name="helper-script-to-create-the-ad-fs-issuance-transform-rules"></a>Helper-script voor het maken van de AD FS-uitgifte transformatieregels voor

Het volgende script waarmee u met het maken van de uitgifte regels die hierboven worden beschreven.

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

- Met dit script worden de regels aan de bestaande regels toegevoegd. Het script niet twee keer uitgevoerd omdat de verzameling van regels wordt twee keer worden toegevoegd. Zorg ervoor dat er geen overeenkomende regels bestaan voor deze claims (onder de bijbehorende voorwaarden) voordat het script nogmaals uit te voeren.

- Als u meerdere geverifieerde domeinnamen (zoals weergegeven in de Azure AD-portal of via de cmdlet Get-MsolDomains) hebt, stel de waarde van **$multipleVerifiedDomainNames** in het script naar **$true**. Controleer ook of u alle bestaande issuerid-claim die mogelijk zijn gemaakt door Azure AD Connect of via andere middelen. Hier volgt een voorbeeld voor deze regel:


        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- Als u al hebt verzonden een **ImmutableID** claim voor gebruikersaccounts, stel de waarde van **$immutableIDAlreadyIssuedforUsers** in het script naar **$true**.

## <a name="enable-windows-down-level-devices"></a>Windows downlevel-apparaten inschakelen

Als sommige van uw apparaten domein Windows downlevel-apparaten zijn, moet u naar:

- Een beleid instellen in Azure AD zodat gebruikers om apparaten te registreren.
 
- Configureren van uw on-premises federation-service voor het uitgeven van claims voor de ondersteuning van **geïntegreerde Windows-verificatie (IWA)** voor device Registration service.
 
- Het verificatie-eindpunt van de Azure AD-apparaat toevoegen aan het lokale intranetzones om te voorkomen dat certificaat wordt gevraagd wanneer het apparaat te verifiëren.

### <a name="set-policy-in-azure-ad-to-enable-users-to-register-devices"></a>Beleid instellen in Azure AD waarmee gebruikers apparaten registreren

Voor het registreren van Windows downlevel-apparaten, moet u om ervoor te zorgen dat de instelling als u wilt toestaan dat gebruikers voor het registreren van apparaten in Azure AD is ingesteld. U kunt deze instelling onder vinden in de Azure-portal:

`Azure Active Directory > Users and groups > Device settings`
    
Het volgende beleid moet worden ingesteld op **alle**: **gebruikers hun apparaten kunnen registreren bij Azure AD**

![Apparaten registreren](./media/hybrid-azuread-join-manual-steps/23.png)


### <a name="configure-on-premises-federation-service"></a>On-premises federation-service configureren 

Uw on-premises federation-service moet ondersteuning voor het uitgeven de **authenticationmethod** en **wiaormultiauthn** claims bij de ontvangst van een verificatieaanvraag naar de relying party van Azure AD met een resouce_params parameter met een gecodeerde waarde zoals hieronder:

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

Wanneer een dergelijke aanvraag afkomstig is, wordt de gebruiker met behulp van geïntegreerde Windows-verificatie moet worden geverifieerd door de federation-service voor on-premises en implementatie is geslaagd, moet deze de volgende twee claims uitgeven:

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

In AD FS, moet u een regel voor het transformeren van uitgifte die passen via de methode voor netwerkauthenticatie toevoegen.  

**Deze regel toevoegen:**

1. In de AD FS-beheerconsole, gaat u naar `AD FS > Trust Relationships > Relying Party Trusts`.
2. Met de rechtermuisknop op het Identiteitsplatform van Microsoft Office 365 relying party vertrouwensrelatieobject en selecteer vervolgens **Claimregels bewerken**.
3. Op de **uitgifte Transformatieregels** tabblad **regel toevoegen**.
4. In de **claimregel** sjabloon in de lijst met **met behulp van een aangepaste regel Claims verzenden**.
5. Selecteer **Volgende**.
6. In de **naam van Claimregel** in het vak **Claimregel Auth-methode**.
7. In de **claimregel** typt u de volgende regel:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. Op de federatieserver, typt u de PowerShell-opdracht hieronder nadat vervangen **\<RPObjectName\>** met de relying party-objectnaam voor uw Azure AD relying party trust-object. Dit object doorgaans de naam **Identiteitsplatform van Microsoft Office 365**.
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-the-azure-ad-device-authentication-end-point-to-the-local-intranet-zones"></a>Het verificatie-eindpunt van de Azure AD-apparaat toevoegen aan de zones Lokaal Intranet

Om te voorkomen dat certificaat wordt gevraagd wanneer gebruikers van apparaten registreren zich met Azure AD u een beleid pushen naar uw domein apparaten verifiëren kunt naar de volgende URL toevoegen aan de zone Lokaal Intranet in Internet Explorer:

`https://device.login.microsoftonline.com`

## <a name="control-deployment-and-rollout"></a>Besturingselement voor implementatie

Wanneer u de vereiste stappen hebt voltooid, gaan domein apparaten worden automatisch gekoppeld aan Azure AD:

- Alle domein op apparaten met Windows 10 Verjaardag Update en Windows Server 2016 automatisch registreren met Azure AD op het apparaat opnieuw opstarten of aanmelden van gebruikers. 

- Nieuwe apparaten registreren bij Azure AD wanneer het apparaat opnieuw wordt opgestart nadat de domain-join-bewerking is voltooid.

- Apparaten die u eerder Azure AD zijn geregistreerd (bijvoorbeeld voor Intune) worden overgezet naar "*van een domein, geregistreerd bij AAD*"; maar het duurt even voordat dit proces is voltooid voor alle apparaten vanwege de normale stroom van het domein en Gebruikersactiviteit.

### <a name="remarks"></a>Opmerkingen

- U kunt een Group Policy object of System Center Configuration Manager client-instelling voor het beheren van de implementatie van automatische inschrijving van Windows 10 en Windows Server 2016 domein computers gebruiken. **Als u niet wilt dat deze apparaten automatisch registreren bij Azure AD of u wilt voor het beheren van de registratie**, en vervolgens moet u Groepsbeleid eerst uitschakelen van de automatische inschrijving naar al deze apparaten uitgerold of als u van configuratie gebruikmaakt Manager moet u de clientinstelling in Cloud Services configureren -> automatisch nieuwe Windows 10 domein apparaten registreren met Azure Active Directory op "Nee", voordat u begint met een van de configuratiestappen. Als u klaar bent u configureert, en wanneer u klaar om te testen bent, moet u Groepsbeleid inschakelen van de automatische inschrijving alleen voor de testapparaten implementeren en kies vervolgens voor alle andere apparaten als u.

- Implementatie van Windows downlevel-computers, u kunt implementeren een [Windows Installer-pakket](#windows-installer-packages-for-non-windows-10-computers) op computers die u selecteert.

- Als u de Groepsbeleid-object op domein Windows 8.1-apparaten hebt gepusht, wordt een join geprobeerd. maar het wordt aangeraden dat u de [Windows Installer-pakket](#windows-installer-packages-for-non-windows-10-computers) worden toegevoegd aan uw apparaten voor de downlevel Windows. 

### <a name="create-a-group-policy-object"></a>Een groepsbeleidsobject maken 

Als u wilt beheren van de implementatie van de huidige Windows-computers, moet u implementeren de **Domeincomputers registreren als apparaten** Group Policy object naar de apparaten die u wilt registreren. Bijvoorbeeld, kunt u het beleid implementeren naar een organisatie-eenheid of aan een beveiligingsgroep.

**Het beleid instellen:**

1. Open **Serverbeheer**, en ga vervolgens naar `Tools > Group Policy Management`.
2. Ga naar het knooppunt van het domein dat overeenkomt met het domein waar u automatische registratie van de huidige Windows-computers te activeren.
3. Met de rechtermuisknop op **Group Policy Objects**, en selecteer vervolgens **nieuw**.
4. Typ een naam voor de Groepsbeleid-object. Bijvoorbeeld: * Hybrid Azure AD join. 
5. Klik op **OK**.
6. Met de rechtermuisknop op uw nieuwe groepsbeleidsobject en selecteer vervolgens **bewerken**.
7. Ga naar **Computerconfiguratie** > **beleid** > **Beheersjablonen** > **Windows Onderdelen** > **Device Registration service**. 
8. Met de rechtermuisknop op **Domeincomputers registreren als apparaten**, en selecteer vervolgens **bewerken**.
   
   > [!NOTE]
   > Deze sjabloon Groepsbeleid is gewijzigd van eerdere versies van de console Groepsbeleidsbeheer. Als u een eerdere versie van de-console gebruikt, gaat u naar `Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`. 

7. Selecteer **ingeschakeld**, en klik vervolgens op **toepassen**. U moet selecteren **uitgeschakelde** als u wilt dat het beleid moet worden geblokkeerd dat de apparaten die beheerd door dit groepsbeleid automatisch registreren met Azure AD.

8. Klik op **OK**.
9. De Groepsbeleid-object een koppeling naar een locatie van uw keuze. Bijvoorbeeld, kunt u deze koppelen aan een specifieke organisatie-eenheid. U kan ook deze koppelen aan een specifieke beveiligingsgroep met computers die automatisch met Azure AD join. U dit beleid instellen voor alle Windows 10 en Windows Server 2016 Domeincomputers in uw organisatie, de Groepsbeleid-object aan het domein te koppelen.

### <a name="windows-installer-packages-for-non-windows-10-computers"></a>Windows Installer-pakketten voor Windows 10-computers

Als u wilt deelnemen aan domein toegevoegde Windows downlevel-computers in een federatieve omgeving, kunt u downloaden en installeren van deze Windows Installer-pakket (.msi) van Download Center op de [Microsoft Workplace Join voor Windows 10-computers](https://www.microsoft.com/en-us/download/details.aspx?id=53554) de pagina.

U kunt het pakket implementeren met behulp van het systeem in een software-distributie, zoals System Center Configuration Manager. Het pakket biedt ondersteuning voor de opties standaard op de achtergrond installeren met de *stille* parameter. System Center Configuration Manager Current Branch biedt extra voordelen van eerdere versies, zoals de mogelijkheid voor het bijhouden van voltooide registraties. Zie voor meer informatie, [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).

Het installatieprogramma maakt een geplande taak op het systeem dat wordt uitgevoerd in de context van de gebruiker. De taak wordt geactiveerd wanneer de gebruiker zich aanmeldt bij Windows. De taak op de achtergrond lid wordt van het apparaat met Azure AD met de referenties van de gebruiker na verificatie met behulp van geïntegreerde Windows-verificatie. De geplande taak, het apparaat, Ga naar **Microsoft** > **Workplace Join**, en ga vervolgens naar de Task Scheduler-bibliotheek.

## <a name="verify-joined-devices"></a>Controleer of de gekoppelde apparaten

U kunt geslaagde gekoppelde apparaten controleren in uw organisatie met behulp van de [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet in de [Azure Active Directory PowerShell-module](/powershell/azure/install-msonlinev1?view=azureadps-2.0).

De uitvoer van deze cmdlet toont de apparaten die zijn geregistreerd en samengevoegd met Azure AD. Voor alle apparaten, gebruikt de **-alle** parameter, en ze vervolgens te filteren met behulp van de **deviceTrustType** eigenschap. Domein apparaten hebben een waarde van **domein**.



## <a name="troubleshoot-your-implementation"></a>Uw implementatie oplossen

Als u problemen ondervindt bij het invullen van hybride Azure AD join voor domein gekoppelde Windows-apparaten, Zie:

- [Oplossen van problemen met Hybrid Azure AD join voor de huidige Windows-apparaten](troubleshoot-hybrid-join-windows-current.md)
- [Oplossen van problemen met Hybrid Azure AD join voor Windows downlevel-apparaten](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Apparaatbeheer in Azure Active Directory](overview.md)



<!--Image references-->
[1]: ./media/hybrid-azuread-join-manual-steps/12.png
