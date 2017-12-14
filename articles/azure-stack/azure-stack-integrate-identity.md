---
title: Stack datacenter integratie van Azure - identiteit
description: Meer informatie over het Azure-Stack AD FS integreren in uw datacenter AD FS
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: article
ms.date: 12/12/2017
ms.author: mabrigg
keywords: 
ms.openlocfilehash: 642ed3298eec0bab5515df117c0310786358e417
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="azure-stack-datacenter-integration---identity"></a>Stack datacenter integratie van Azure - identiteit

*Van toepassing op: Azure Stack geïntegreerd systemen*

U kunt Azure-Stack met behulp van Azure Active Directory (Azure AD) of Active Directory Federation Services (AD FS) implementeren als de id-providers. Voordat u Azure-Stack implementeert, moet u de keuze maken. Implementatie met behulp van AD FS ook aangeduid als Azure-Stack in de modus zonder verbinding implementeert.

De volgende tabel ziet u de verschillen tussen de twee identiteit keuzes:

||Fysiek losgekoppeld|Fysiek verbonden|
|---------|---------|---------|
|Facturering|Capaciteit moet<br> Enterprise Agreement (EA) alleen|Capaciteit of Pay-as-gebruik<br>EA of Cloud Solution Provider (CSP)|
|Identiteit|Moet de AD FS|Azure AD of AD FS|
|Marketplace-syndicatie|Momenteel niet beschikbaar|Ondersteund<br>BYOL-licentieverlening|
|Registratie|Aanbevolen, vereist een verwisselbaar medium<br> en een afzonderlijke aangesloten apparaat.|Geautomatiseerd|
|Patch en bij te werken|Vereist, vereist een verwisselbaar medium<br> en een afzonderlijke aangesloten apparaat.|Updatepakket kan rechtstreeks worden gedownload.<br> van Internet naar het Azure-Stack.|

> [!IMPORTANT]
> U kunt de id-provider niet wijzigen zonder de volledige Azure-Stack-oplossing opnieuw te implementeren.

## <a name="active-directory-federation-services-and-graph"></a>Active Directory Federatieservices en de grafiek

Met AD FS implementeren, kunt identiteiten in een bestaand Active Directory-forest om te verifiëren met resources in Azure-Stack. Deze bestaande Active Directory-forest moet een implementatie van AD FS waarmee het maken van een AD FS federation-vertrouwensrelatie.

Verificatie is een deel van de identiteit. Als u wilt beheren op rollen gebaseerd toegangsbeheer (RBAC) in Azure-Stack, moet het onderdeel van de grafiek worden geconfigureerd. Wanneer u toegang tot een resource wordt overgedragen, zoekt het onderdeel van de grafiek het gebruikersaccount in het bestaande Active Directory-forest met de LDAP-protocol.

![Azure AD FS-Stack architectuur](media/azure-stack-integrate-identity/Azure-Stack-ADFS-architecture.png)

De bestaande AD FS is de account beveiligingstokenservice (STS) waarmee claims naar de Stack van Azure AD FS (de bron-STS) worden verzonden. In Azure-Stack maakt automation de vertrouwensrelatie met claimproviders met het metagegevenseindpunt voor de bestaande AD FS.

Op de bestaande AD FS, moet u een relying party trust configureren. Deze stap wordt niet uitgevoerd door de automatisering en moet worden geconfigureerd door de operator. Het eindpunt van de metagegevens van Azure-Stack wordt beschreven in het bestand AzureStackStampDeploymentInfo.JSON of via het bevoegde eindpunt met de opdracht `Get-AzureStackInfo`.

De configuratie van de relying party-vertrouwensrelatie moet u de claim transformatie regels configureren die worden geleverd door Microsoft.

Voor de configuratie van de grafiek moet een service-account mits leesmachtiging heeft in de bestaande Active Directory. Dit account is vereist als invoer voor de automatisering RBAC scenario's te maken.

Voor de laatste stap wordt een nieuwe eigenaar geconfigureerd voor het abonnement van de provider standaard. Dit account heeft volledige toegang tot alle resources wanneer aangemeld bij de Azure-Stack-beheerdersportal.

Vereisten:


|Onderdeel|Vereiste|
|---------|---------|
|Graph|Microsoft Active Directory-2012/2012 R2/2016|
|AD FS|Windows Server 2012/2012 R2/2016|

## <a name="setting-up-graph-integration"></a>Instellen van integratie met grafiek

De volgende informatie is vereist als invoer voor de automation-parameters:


|Parameter|Beschrijving|Voorbeeld|
|---------|---------|---------|
|CustomADGlobalCatalog|FQDN-naam van het doel van Active Directory-forest<br>dat u integreren wilt met|Contoso.com|
|CustomADAdminCredentials|Een gebruiker met de machtiging lezen LDAP|YOURDOMAIN\graphservice|

### <a name="create-user-account-in-the-existing-active-directory-optional"></a>Maak gebruikersaccount in de bestaande Active Directory (optioneel)

Desgewenst kunt u een account voor de grafiek service in de bestaande Active Directory. Deze stap uitvoeren als u hebt al een account dat u wilt gebruiken.

1. Maak de volgende gebruikersaccount (aanbevolen) in de bestaande Active Directory:
   - **Gebruikersnaam**: graphservice
   - **Wachtwoord**: gebruik een sterk wachtwoord<br>Configureer het wachtwoord nooit verloopt.

   Er is geen speciale machtigingen of lidmaatschap vereist.

#### <a name="trigger-automation-to-configure-graph"></a>Automatisering van trigger voor het configureren van de grafiek

Voor deze procedure gebruikt u een computer in uw datacenternetwerk die met het bevoorrechte eindpunt in Azure-Stack communiceren kan.

2. Open een verhoogde Windows PowerShell-sessie (als administrator uitvoeren) en verbinding maken met het IP-adres van de bevoegde eindpunt. Gebruik de referenties voor **CloudAdmin** om te verifiëren.

   ```powershell
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

3. Nu dat u met het bevoorrechte eindpunt verbonden bent, voer de volgende opdracht: 

   ```powershell
   Register-DirectoryService -CustomADGlobalCatalog contoso.com
   ```

   Wanneer u wordt gevraagd, geef de referenties voor de gebruikersaccount die u wilt gebruiken voor de grafiek service (zoals graphservice).

   > [!IMPORTANT]
   > Wacht totdat de pop-referenties (Get-Credential wordt niet ondersteund in de beschermde eindpunt) en voer de referenties van het serviceaccount van de grafiek.

#### <a name="graph-protocols-and-ports"></a>Grafiek-protocollen en poorten

Grafiek-service in Azure-Stack maakt gebruik van de volgende protocollen en poorten om te communiceren met het doel van Active Directory:

|Type|Poort|Protocol|
|---------|---------|---------|
|LDAP|389|TCP EN UDP|
|LDAP SSL|636|TCP|
|GLOBALE CATALOGUS LDAP|3268|TCP|
|LDAP GC SSL|3269|TCP|

## <a name="setting-up-ad-fs-integration-by-downloading-federation-metadata"></a>Instellen van integratie met AD FS federatiemetagegevens downloaden

De volgende informatie is vereist als invoer voor de automation-parameters:

|Parameter|Beschrijving|Voorbeeld|
|---------|---------|---------|
|CustomAdfsName|Naam van de claimprovider. <cr>Lijkt op die manier op de startpagina van de AD FS.|Contoso|
|CustomAD<br>FSFederationMetadataEndpointUri|Koppeling van federatieve metagegevens|https://AD01.contoso.com/FederationMetadata/2007-06/federationmetadata.XML|


### <a name="trigger-automation-to-configure-claims-provider-trust-in-azure-stack"></a>Automatisering van trigger voor het configureren van de vertrouwensrelatie met claimproviders in Azure-Stack

Voor deze procedure gebruikt u een computer die kan communiceren met het bevoorrechte eindpunt in Azure-Stack. Er wordt verwacht dat het certificaat wordt gebruikt door het account **STS AD FS** wordt vertrouwd door de Azure-Stack.

1. Open een Windows PowerShell-sessie met verhoogde bevoegdheden en verbinding maken met de bevoorrechte eindpunt.

   ```powershell
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Nu dat u met het bevoorrechte eindpunt verbonden bent, voer de volgende opdracht met de parameters die geschikt is voor uw omgeving:

   ```powershell
   Register-CustomAdfs -CustomAdfsName Contoso -CustomADFSFederationMetadataEndpointUri https://win-SQOOJN70SGL.contoso.com/federationmetadata/2007-06/federationmetadata.xml
   ```

3. Voer de volgende opdracht voor het bijwerken van de eigenaar van het abonnement van de provider standaard met de parameters die geschikt is voor uw omgeving:

   ```powershell
   Set-ServiceAdminOwner -ServiceAdminOwnerUpn "administrator@contoso.com"
   ```

## <a name="setting-up-ad-fs-integration-by-providing-federation-metadata-file"></a>Instellen van integratie met AD FS aan de hand van bestand met federatieve metagegevens

Gebruik deze methode als een van de volgende voorwaarden voldaan wordt:

- De certificaatketen is verschillend voor AD FS in vergelijking met andere eindpunten in Azure-Stack.
- Er is geen netwerkverbinding met de bestaande AD FS-server van AD FS-exemplaar van Azure-Stack.

De volgende informatie is vereist als invoer voor de automation-parameters:


|Parameter|Beschrijving|Voorbeeld|
|---------|---------|---------|
|CustomAdfsName|Naam van de claimprovider. De indeling lijkt op die manier op de startpagina van de AD FS.|Contoso|
|CustomADFSFederationMetadataFile|Bestand met federatieve metagegevens|https://AD01.contoso.com/FederationMetadata/2007-06/federationmetadata.XML|

### <a name="create-federation-metadata-file"></a>Bestand met federatieve metagegevens maken

Voor de volgende procedure moet u een computer die een netwerkverbinding heeft met de bestaande AD FS-implementatie, wordt het account STS. Bovendien moeten de benodigde certificaten zijn geïnstalleerd.

1. Open een Windows PowerShell-sessie met verhoogde bevoegdheden en voer de volgende opdracht, met de parameters die geschikt is voor uw omgeving:

   ```powershell
   [XML]$Metadata = Invoke-WebRequest -URI https://win-SQOOJN70SGL.contoso.com/federationmetadata/2007-06/federationmetadata.xml -UseBasicParsing

   $Metadata.outerxml|out-file c:\metadata.xml
   ```

2. Het metagegevensbestand kopiëren naar een share die toegankelijk is vanaf de bevoorrechte eindpunt.


### <a name="trigger-automation-to-configure-claims-provider-trust-in-azure-stack"></a>Automatisering van trigger voor het configureren van de vertrouwensrelatie met claimproviders in Azure-Stack

Voor deze procedure gebruikt u een computer die kan communiceren met het bevoorrechte eindpunt in Azure-Stack.

1. Open een Windows PowerShell-sessie met verhoogde bevoegdheden en verbinding maken met de bevoorrechte eindpunt.

   ```powershell
   $creds=Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Nu dat u met het bevoorrechte eindpunt verbonden bent, voer de volgende opdracht met de parameters die geschikt is voor uw omgeving:

   ```powershell
   Register-CustomAdfs -CustomAdfsName Contoso – CustomADFSFederationMetadataFile \\share\metadataexample.xml
   ```

3. Voer de volgende opdracht voor het bijwerken van de eigenaar van het abonnement van de provider standaard met de parameters die geschikt is voor uw omgeving:

   ```powershell
   Set-ServiceAdminOwner -ServiceAdminOwnerUpn "administrator@contoso.com"
   ```

## <a name="configure-relying-party-on-existing-ad-fs-deployment-account-sts"></a>Relying party configureren op de bestaande AD FS-implementatie (account STS)

Microsoft biedt een script waarmee de relying party trust, met inbegrip van de transformatie claimregels geconfigureerd. Met het script is optioneel als u handmatig de opdrachten kunt uitvoeren.

U kunt downloaden via het script helper vanaf [Stack-hulpprogramma's van Azure](https://github.com/Azure/AzureStack-Tools/tree/vnext/DatacenterIntegration/Identity) op Github.

Als u besluit de opdrachten handmatig uitvoeren, volg deze stappen:

1. Kopieer de volgende inhoud naar een txt-bestand (bijvoorbeeld opgeslagen als c:\ClaimRules.txt) op uw datacenter AD FS-exemplaar of -farm lid:

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

2. Om verificatie op basis van Windows Forms, open een Windows PowerShell-sessie als een gebruiker met verhoogde bevoegdheid en voer de volgende opdracht:

   ```powershell
   Set-AdfsProperties -WIASupportedUserAgents @("MSAuthHost/1.0/In-Domain","MSIPC","Windows Rights Management Client","Kloud")
   ```

3. Als u wilt de relying party trust toevoegen, de volgende Windows PowerShell-opdracht worden uitgevoerd op uw exemplaar van AD FS of een Farmlid. Zorg ervoor dat het eindpunt van de AD FS worden bijgewerkt en verwijzen naar het bestand is gemaakt in stap 1.

   **Voor AD FS 2016**

   ```powershell
   Add-ADFSRelyingPartyTrust -Name AzureStack -MetadataUrl "https://YourAzureStackADFSEndpoint/FederationMetadata/2007-06/FederationMetadata.xml" -IssuanceTransformRulesFile "C:\ClaimIssuanceRules.txt" -AutoUpdateEnabled:$true -MonitoringEnabled:$true -enabled:$true -AccessControlPolicyName "Permit everyone"
   ```

   **Voor AD FS 2012/2012 R2**

   ```powershell
   Add-ADFSRelyingPartyTrust -Name AzureStack -MetadataUrl "https://YourAzureStackADFSEndpoint/FederationMetadata/2007-06/FederationMetadata.xml" -IssuanceTransformRulesFile "C:\ClaimIssuanceRules.txt" -AutoUpdateEnabled:$true -MonitoringEnabled:$true -enabled:$true
   ```

   > [!IMPORTANT]
   > U moet de AD FS MMC-module gebruiken voor het configureren van de autorisatieregels voor uitgifte bij gebruik van Windows Server 2012 of 2012 R2 AD FS.

4. Wanneer u Internet Explorer of Edge-browser gebruikt voor toegang tot Azure-Stack, moet u token bindingen negeren. Anders worden de aanmeldingspogingen mislukken. Op uw exemplaar van AD FS of een Farmlid, moet u de volgende opdracht uitvoeren:

   ```powershell
   Set-AdfsProperties -IgnoreTokenBinding $true
   ```

5. Als u wilt vernieuwen van tokens, open een Windows PowerShell-sessie met verhoogde bevoegdheden en voer de volgende opdracht:

   ```powershell
   Set-ADFSRelyingPartyTrust -TargetName AzureStack -TokenLifeTime 1440
   ```

## <a name="spn-creation"></a>SPN maken

Er zijn veel scenario's waarvoor het gebruik van een service principal name (SPN) voor verificatie. Hier volgen enkele voorbeelden:

- Gebruik met AD FS-implementatie van de Azure CLI
- System Center Management Pack voor Azure-Stack wanneer geïmplementeerd met AD FS
- Resourceproviders in Azure-Stack wanneer geïmplementeerd met AD FS
- Verschillende toepassingen
- U moet een niet-interactieve aanmelding

Zie voor meer informatie over het maken van een SPN [service-principal maken voor AD FS](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#create-service-principal-for-ad-fs).


## <a name="troubleshooting"></a>Problemen oplossen

### <a name="configuration-rollback"></a>Configuratie terugdraaien

Een optie voor het terugdraaien is beschikbaar als een fout optreedt die verlaat de omgeving in een status waar u niet meer kunnen verifiëren.

1. Open een Windows PowerShell-sessie met verhoogde bevoegdheden en voer de volgende opdrachten:

   ```powershell
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Voer de volgende cmdlet:

   ```powershell
   Reset-DatacenterIntegationConfiguration
   ```

   Na het uitvoeren van de actie terugdraaien worden alle configuratiewijzigingen die hersteld. Alleen verificatie met de ingebouwde **CloudAdmin** gebruiker is mogelijk.

   > [!IMPORTANT]
   > U moet de oorspronkelijke eigenaar van het standaard provider-abonnement configureren

   ```powershell
   Set-ServiceAdminOwner -ServiceAdminOwnerUpn "azurestackadmin@[Internal Domain]"
   ```

### <a name="collecting-additional-logs"></a>Verzamelen van aanvullende logboeken

Als een van de cmdlets mislukt, kunt u aanvullende logboekbestanden verzamelen met behulp van de `Get-Azurestacklogs` cmdlet.

1. Open een Windows PowerShell-sessie met verhoogde bevoegdheden en voer de volgende opdrachten:

   ```powershell
   $creds = Get-Credential
   Enter-pssession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Voer de volgende cmdlet:

   ```powershell
   Get-AzureStackLog -OutputPath \\myworstation\AzureStackLogs -FilterByRole ECE
   ```


## <a name="next-steps"></a>Volgende stappen

[Azure datacenter integratie Stack - eindpunten publiceren](azure-stack-integrate-endpoints.md)
