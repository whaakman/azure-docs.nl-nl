---
title: Datacenter-integratie Azure Stack - identiteit
description: Meer informatie over het integreren van Azure Stack AD FS met uw datacenter AD FS
services: azure-stack
author: jeffgilb
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 08/01/2018
ms.author: jeffgilb
ms.reviewer: wfayed
keywords: ''
ms.openlocfilehash: c22831ae781239f2474720ee9cd1b58f6e14fe41
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39412862"
---
# <a name="azure-stack-datacenter-integration---identity"></a>Datacenter-integratie Azure Stack - identiteit
U kunt Azure Stack met behulp van Azure Active Directory (Azure AD) of Active Directory Federation Services (AD FS) implementeren als de id-providers. Voordat u Azure Stack implementeren, moet u de keuze maken. Implementatie met behulp van AD FS is ook aangeduid als Azure Stack implementeren in de niet-verbonden modus.

De volgende tabel ziet u de verschillen tussen de twee identity-opties:

||Niet verbonden met het internet|Verbonden met internet|
|---------|---------|---------|
|Billing|Capaciteit moet worden<br> Alleen Enterprise Agreement (EA)|Capaciteit of betalen als u-gebruik<br>EA- of Cloud Solution Provider (CSP)|
|Identiteit|Moet de AD FS|Azure AD of AD FS|
|Marketplace |Ondersteund<br>BYOL-licentieverlening|Ondersteund<br>BYOL-licentieverlening|
|Registratie|Aanbevolen, verwisselbare media vereist<br> en een afzonderlijke verbonden apparaten.|Geautomatiseerd|
|Voor patches en updates|Vereist, verwisselbare media vereist<br> en een afzonderlijke verbonden apparaten.|Updatepakket kan rechtstreeks worden gedownload.<br> vanaf het Internet met Azure Stack.|

> [!IMPORTANT]
> U kunt niet de id-provider overstappen zonder de gehele Azure Stack-oplossing opnieuw te implementeren.

## <a name="active-directory-federation-services-and-graph"></a>Active Directory Federatieservices en een graaf

Implementeren met AD FS kunt-id's in een bestaand Active Directory-forest om te verifiëren met resources in Azure Stack. Deze bestaande Active Directory-forest vereist een implementatie van AD FS voor het toestaan van het maken van een AD FS federatieve vertrouwensrelatie.

Verificatie is een deel van de identiteit. Als u wilt beheren op rollen gebaseerd toegangsbeheer (RBAC) in Azure Stack, moet het onderdeel van de grafiek worden geconfigureerd. Wanneer toegang tot een resource wordt overgedragen, wordt de Graph-component zoekt u het gebruikersaccount in het bestaande Active Directory-forest met de LDAP-protocol.

![Azure Stack AD FS-architectuur](media/azure-stack-integrate-identity/Azure-Stack-ADFS-architecture.png)

De bestaande AD FS is de account beveiligingstokenservice (STS) waarmee claims voor de Azure Stack AD FS (de resource STS) worden verzonden. In Azure Stack maakt automation de claims provider vertrouwensrelatie met het eindpunt van de metagegevens voor de bestaande AD FS.

Op de bestaande AD FS, moet een relying party trust worden geconfigureerd. Deze stap niet wordt uitgevoerd door de automatisering en moet worden geconfigureerd door de operator. Het eindpunt van de Azure Stack-metagegevens wordt beschreven in het bestand AzureStackStampDeploymentInfo.JSON of via het eindpunt van de bevoegdheden met de opdracht `Get-AzureStackInfo`.

De configuratie van de relying party trust moet u de claim transformatie regels configureren die worden geleverd door Microsoft.

Voor de configuratie van de grafiek moet een service-account dat is de machtiging lezen in de bestaande Active Directory. Dit account is vereist als invoer voor de automatisering RBAC-scenario's inschakelen.

Voor de laatste stap is een nieuwe eigenaar voor het abonnement van de provider standaard geconfigureerd. Dit account heeft volledige toegang tot alle resources wanneer aangemeld bij de Azure Stack-beheerdersportal.

Vereisten:


|Onderdeel|Vereiste|
|---------|---------|
|Graph|Microsoft Active Directory 2012/2012 R2/2016|
|AD FS|Windows Server 2012/2012 R2/2016|

## <a name="setting-up-graph-integration"></a>Graph-integratie instellen

Grafiek biedt alleen ondersteuning voor integratie met één Active Directory-forest. Als meerdere forests bestaat, wordt alleen het forest die zijn opgegeven in de configuratie worden gebruikt voor het ophalen van gebruikers en groepen.

De volgende informatie is vereist als invoer voor de automation-parameters:


|Parameter|Beschrijving|Voorbeeld|
|---------|---------|---------|
|CustomADGlobalCatalog|FQDN-naam van het doel van Active Directory-forest<br>dat u integreren wilt met|Contoso.com|
|CustomADAdminCredentials|Een gebruiker met de machtiging lezen voor LDAP|YOURDOMAIN\graphservice|

### <a name="create-user-account-in-the-existing-active-directory-optional"></a>Maak gebruikersaccount in de bestaande Active Directory (optioneel)

U kunt eventueel een account voor de Graph-service maken in de bestaande Active Directory. Deze stap uitvoeren als u geen al een account dat u wilt gebruiken.

1. In de bestaande Active Directory, maken de volgende gebruikersaccount (aanbevolen):
   - **Gebruikersnaam**: graphservice
   - **Wachtwoord**: gebruik een sterk wachtwoord<br>Configureer het wachtwoord voor het nooit verloopt.

   Er zijn geen speciale machtigingen of lidmaatschap is vereist.

#### <a name="trigger-automation-to-configure-graph"></a>Trigger automation graph configureren

Voor deze procedure gebruikt u een computer in uw netwerk van datacenters die met de bevoorrechte eindpunt in Azure Stack communiceren kan.

2. Open een verhoogde Windows PowerShell-sessie (als administrator uitvoeren) en verbinding maken met het IP-adres van de bevoegde eindpunt. Gebruik de referenties voor **CloudAdmin** om te verifiëren.

   ```PowerShell  
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

3. Nu dat u met het bevoorrechte eindpunt verbonden bent, voert u de volgende opdracht uit: 

   ```PowerShell  
   Register-DirectoryService -CustomADGlobalCatalog contoso.com
   ```

   Wanneer u hierom wordt gevraagd, geeft u de referentie voor het gebruikersaccount dat u wilt gebruiken voor de Graph-service (zoals graphservice). De invoer voor de cmdlet Register-DirectoryService moet overeenkomen met de forestnaam / domein in het forest in plaats van een ander domein in het forest root.

   > [!IMPORTANT]
   > Wachten op de pop-referenties (Get-Credential wordt niet ondersteund in het privileged eindpunt) en voer de referenties van de Graph-serviceaccount.

#### <a name="graph-protocols-and-ports"></a>Graph-protocollen en poorten

Graph-service in Azure Stack maakt gebruik van de volgende protocollen en poorten om te communiceren met een beschrijfbare Global Catalog Server (GC) en het KDC Key Distribution Center () waarmee aanmeldingsaanvragen in de doel-Active Directory-forest kunnen worden verwerkt.

Graph-service in Azure Stack maakt gebruik van de volgende protocollen en poorten om te communiceren met het doel van Active Directory:

|Type|Poort|Protocol|
|---------|---------|---------|
|LDAP|389|TCP EN UDP|
|LDAP SSL|636|TCP|
|GLOBALE CATALOGUS LDAP|3268|TCP|
|LDAP GC SSL|3269|TCP|

## <a name="setting-up-ad-fs-integration-by-downloading-federation-metadata"></a>AD FS-integratie instellen door te downloaden van federatiemetagegevens

De volgende informatie is vereist als invoer voor de automation-parameters:

|Parameter|Beschrijving|Voorbeeld|
|---------|---------|---------|
|CustomAdfsName|De naam van de claimprovider. <cr>Lijkt op die manier op de startpagina van de AD FS.|Contoso|
|CustomAD<br>FSFederationMetadataEndpointUri|Federatieve metagegevens van koppeling|https://ad01.contoso.com/federationmetadata/2007-06/federationmetadata.xml|


### <a name="trigger-automation-to-configure-claims-provider-trust-in-azure-stack"></a>Trigger automation te configureren van de vertrouwensrelatie met claimproviders in Azure Stack

Voor deze procedure gebruikt u een computer die kan communiceren met het bevoorrechte eindpunt in Azure Stack. Er wordt verwacht dat het certificaat wordt gebruikt door de account **STS AD FS** wordt vertrouwd door Azure Stack.

1. Open een Windows PowerShell-sessie met verhoogde bevoegdheden en verbinding maken met het eindpunt van de bevoegde.

   ```PowerShell  
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Nu dat u met het bevoorrechte eindpunt verbonden bent, voer de volgende opdracht uit met behulp van de parameters die geschikt is voor uw omgeving:

   ```PowerShell  
   Register-CustomAdfs -CustomAdfsName Contoso -CustomADFSFederationMetadataEndpointUri https://win-SQOOJN70SGL.contoso.com/federationmetadata/2007-06/federationmetadata.xml
   ```

3. Voer de volgende opdracht voor het bijwerken van de eigenaar van het abonnement van de provider standaard met de parameters die geschikt is voor uw omgeving:

   ```PowerShell  
   Set-ServiceAdminOwner -ServiceAdminOwnerUpn "administrator@contoso.com"
   ```

## <a name="setting-up-ad-fs-integration-by-providing-federation-metadata-file"></a>AD FS-integratie instellen door op te geven van bestand met federatieve metagegevens

Gebruik deze methode als een van de volgende voorwaarden voldaan wordt:

- De certificaatketen is verschillend voor AD FS in vergelijking met alle andere eindpunten in Azure Stack.
- Er is geen netwerkverbinding met de bestaande AD FS-server van AD FS-exemplaar van Azure Stack.

De volgende informatie is vereist als invoer voor de automation-parameters:


|Parameter|Beschrijving|Voorbeeld|
|---------|---------|---------|
|CustomAdfsName|De naam van de claimprovider. Op die manier op de startpagina van de AD FS wordt deze weergegeven.|Contoso|
|CustomADFSFederationMetadataFileContent|De metagegevens van inhoud|$using: federationMetadataFileContent|

### <a name="create-federation-metadata-file"></a>Bestand met federatieve metagegevens maken

Voor de volgende procedure, moet u een computer die een netwerkverbinding heeft met de bestaande AD FS-implementatie, wordt de STS-account gebruiken. Bovendien moeten de benodigde certificaten zijn geïnstalleerd.

1. Open een Windows PowerShell-sessie met verhoogde bevoegdheden en voer de volgende opdracht uit, met behulp van de parameters die geschikt is voor uw omgeving:

   ```PowerShell  
   [XML]$Metadata = Invoke-WebRequest -URI https://win-SQOOJN70SGL.contoso.com/federationmetadata/2007-06/federationmetadata.xml -UseBasicParsing

   $Metadata.outerxml|out-file c:\metadata.xml
   ```

2. Kopieer het bestand met metagegevens op een computer die met de bevoorrechte eindpunt communiceren kan.

### <a name="trigger-automation-to-configure-claims-provider-trust-in-azure-stack"></a>Trigger automation te configureren van de vertrouwensrelatie met claimproviders in Azure Stack

Voor deze procedure gebruikt u een computer die kan communiceren met de bevoorrechte eindpunt in Azure Stack en heeft toegang tot het bestand met metagegevens dat in een vorige stap hebt gemaakt.

1. Open een Windows PowerShell-sessie met verhoogde bevoegdheden.

   ```PowerShell  
   $federationMetadataFileContent = get-content c:\metadata.cml
   $creds=Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   Register-CustomAdfs -CustomAdfsName Contoso -CustomADFSFederationMetadataFileContent $using:federationMetadataFileContent
   ```

2. Voer de volgende opdracht voor het bijwerken van de eigenaar van het abonnement van de provider standaard met de parameters die geschikt is voor uw omgeving:

   ```PowerShell  
   Set-ServiceAdminOwner -ServiceAdminOwnerUpn "administrator@contoso.com"
   ```

## <a name="configure-relying-party-on-existing-ad-fs-deployment-account-sts"></a>Relying party configureren op de bestaande AD FS-implementatie (account STS)

Microsoft biedt een script waarmee de relying party-trust, met inbegrip van de claimregels transformatie worden geconfigureerd. Gebruik het script is optioneel als u de opdrachten handmatig kunt uitvoeren.

U kunt de helper-script uit downloaden [hulpprogramma's voor Azure Stack](https://github.com/Azure/AzureStack-Tools/tree/vnext/DatacenterIntegration/Identity) op Github.

Als u besluit de opdrachten handmatig uitvoeren, volgt u deze stappen:

1. Kopieer de volgende inhoud in een txt-bestand (bijvoorbeeld opgeslagen als c:\ClaimRules.txt) op van uw datacenter AD FS-exemplaar of van farm lid:

   ```text
   @RuleTemplate = "LdapClaims"
   @RuleName = "Name claim"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
   => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"), query = ";userPrincipalName;{0}", param = c.Value);

   @RuleTemplate = "LdapClaims"
   @RuleName = "UPN claim"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
   => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"), query = ";userPrincipalName;{0}", param = c.Value);

   @RuleTemplate = "LdapClaims"
   @RuleName = "ObjectID claim"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid"]
   => issue(Type = "http://schemas.microsoft.com/identity/claims/objectidentifier", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, Value = c.Value, ValueType = c.ValueType);

   @RuleName = "Family Name and Given claim"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
   => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname", "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"), query = ";sn,givenName;{0}", param = c.Value);

   @RuleTemplate = "PassThroughClaims"
   @RuleName = "Pass through all Group SID claims"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
   => issue(claim = c);

   @RuleTemplate = "PassThroughClaims"
   @RuleName = "Pass through all windows account name claims"
   c:[Type == http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname]
   => issue(claim = c);
   ```

2. Om in te schakelen op basis van formulieren voor Windows-verificatie, open een Windows PowerShell-sessie als een gebruiker met verhoogde bevoegdheid en voer de volgende opdracht uit:

   ```PowerShell  
   Set-AdfsProperties -WIASupportedUserAgents @("MSAuthHost/1.0/In-Domain","MSIPC","Windows Rights Management Client","Kloud")
   ```

3. Als wilt toevoegen de relying party trust, voert u de volgende Windows PowerShell-opdracht op uw exemplaar van AD FS of een Farmlid. Zorg ervoor dat u het bijwerken van de AD FS-eindpunt en verwijzen naar het bestand dat in stap 1 hebt gemaakt.

   **Voor AD FS 2016**

   ```PowerShell  
   Add-ADFSRelyingPartyTrust -Name AzureStack -MetadataUrl "https://YourAzureStackADFSEndpoint/FederationMetadata/2007-06/FederationMetadata.xml" -IssuanceTransformRulesFile "C:\ClaimIssuanceRules.txt" -AutoUpdateEnabled:$true -MonitoringEnabled:$true -enabled:$true -AccessControlPolicyName "Permit everyone"
   ```

   **Voor AD FS 2012/2012 R2**

   ```PowerShell  
   Add-ADFSRelyingPartyTrust -Name AzureStack -MetadataUrl "https://YourAzureStackADFSEndpoint/FederationMetadata/2007-06/FederationMetadata.xml" -IssuanceTransformRulesFile "C:\ClaimIssuanceRules.txt" -AutoUpdateEnabled:$true -MonitoringEnabled:$true -enabled:$true
   ```

   > [!IMPORTANT]
   > De AD FS MMC-module moet u de regels voor Uitgifteautorisatie configureren bij gebruik van Windows Server 2012 of 2012 R2 AD FS.

4. Wanneer u Internet Explorer of de Edge-browser gebruikt voor toegang tot Azure Stack, moet u de token bindingen negeren. Anders mislukt de aanmeldingspogingen. Op uw exemplaar van AD FS of een Farmlid, moet u de volgende opdracht uitvoeren:

   > [!note]  
   > Deze stap is niet van toepassing wanneer u Windows Server 2012 of 2012 R2 AD FS. Is het veilig om deze opdracht overslaan en doorgaan met de integratie.

   ```PowerShell  
   Set-AdfsProperties -IgnoreTokenBinding $true
   ```

5. De Azure Stack-portals en hulpprogramma's (Visual Studio) vereisen vernieuwingstokens. Deze moeten worden geconfigureerd door een vertrouwensrelatie van. Open een Windows PowerShell-sessie met verhoogde bevoegdheden en voer de volgende opdracht uit:

   ```PowerShell  
   Set-ADFSRelyingPartyTrust -TargetName AzureStack -TokenLifeTime 1440
   ```

## <a name="spn-creation"></a>SPN maken

Er zijn veel scenario's waarvoor het gebruik van een service principal name (SPN) voor verificatie. Hier volgen enkele voorbeelden:

- Gebruik CLI met AD FS-implementatie van Azure Stack
- System Center Management Pack voor Azure Stack bij de implementatie met AD FS
- Resourceproviders in Azure Stack bij de implementatie met AD FS
- Verschillende toepassingen
- U moet een niet-interactieve aanmelding

> [!Important]  
> AD FS ondersteunt alleen interactief aanmeldingssessies. Als u een niet-interactieve aanmelding voor een geautomatiseerde scenario vereist, moet u een SPN-naam gebruiken.

Zie voor meer informatie over het maken van een SPN [service-principal maken voor AD FS](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#create-service-principal-for-ad-fs).


## <a name="troubleshooting"></a>Problemen oplossen

### <a name="configuration-rollback"></a>Configuratie terugdraaien

Als er een fout optreedt die blijven van de omgeving in een status waar u niet meer kunt verifiëren, is een optie voor het terugdraaien is beschikbaar.

1. Open een Windows PowerShell-sessie met verhoogde bevoegdheden en voer de volgende opdrachten uit:

   ```PowerShell  
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Voer de volgende cmdlet:

   ```PowerShell  
   Reset-DatacenterIntegationConfiguration
   ```

   Nadat de terugdraaiactie is uitgevoerd, alle wijzigingen teruggedraaid. Alleen verificatie met de ingebouwde **CloudAdmin** gebruiker is mogelijk.

   > [!IMPORTANT]
   > U moet de oorspronkelijke eigenaar van het abonnement van de provider standaard configureren

   ```PowerShell  
   Set-ServiceAdminOwner -ServiceAdminOwnerUpn "azurestackadmin@[Internal Domain]"
   ```

### <a name="collecting-additional-logs"></a>Verzamelen van aanvullende logboeken

Als een van de cmdlets mislukt, kunt u extra logboeken verzamelen met behulp van de `Get-Azurestacklogs` cmdlet.

1. Open een Windows PowerShell-sessie met verhoogde bevoegdheden en voer de volgende opdrachten uit:

   ```PowerShell  
   $creds = Get-Credential
   Enter-pssession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Voer de volgende cmdlet:

   ```PowerShell  
   Get-AzureStackLog -OutputPath \\myworstation\AzureStackLogs -FilterByRole ECE
   ```


## <a name="next-steps"></a>Volgende stappen

[Externe bewakingsoplossingen integreren](azure-stack-integrate-monitor.md)