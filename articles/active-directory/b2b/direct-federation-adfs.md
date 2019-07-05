---
title: Directe Federatie met een AD FS voor B2B - Azure Active Directory instellen | Microsoft Docs
description: Meer informatie over het instellen van AD FS als een id-provider voor directe Federatie, zodat de gasten kunnen zich aanmelden bij uw Azure AD-apps
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a8f709186f0ef17e037c4203118be07ea2d4f511
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67544320"
---
# <a name="example-direct-federation-with-active-directory-federation-services-ad-fs-preview"></a>Voorbeeld: Directe Federatie met Active Directory Federation Services (AD FS) (preview)
|     |
| --- |
| Direct federation is een openbare preview-functie van Azure Active Directory. Zie [Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.|
|     |

In dit artikel wordt beschreven hoe u voor het instellen van [federation directe](direct-federation.md) met behulp van Active Directory Federation Services (AD FS) als een SAML 2.0 of WS-Federation id-provider. Ter ondersteuning van directe federation, moeten bepaalde kenmerken en claims worden geconfigureerd op de id-provider. Ter illustratie van een id-provider voor directe federatie configureren, gebruiken we Active Directory Federation Services (AD FS) als voorbeeld. We laten zien hoe het instellen van AD FS als SAML-identiteitsprovider en als een id-provider van WS-Federation.

> [!NOTE]
> Dit artikel wordt beschreven hoe u AD FS voor SAML- en WS-Federation instelt voor illustratiedoeleinden. Voor directe federation integraties waar de id-provider AD FS is, raden wij het gebruik van WS-Federation als protocol. 

## <a name="configure-ad-fs-for-saml-20-direct-federation"></a>AD FS voor SAML 2.0 direct federatie configureren
Azure AD B2B kan worden geconfigureerd om te federeren met id-providers die gebruikmaken van het SAML-protocol met specifieke vereisten die hieronder worden vermeld. Ter illustratie van de configuratiestappen SAML, wordt in deze sectie wordt beschreven hoe u AD FS instellen voor SAML 2.0. 

Als u direct federation instelt, moeten de volgende kenmerken worden ontvangen in het SAML 2.0-antwoord van de id-provider. Deze kenmerken kunnen worden geconfigureerd door een koppeling naar de online service voor beveiligingstokens XML-bestand of door ze handmatig invoeren. Stap 12 in [maken van een test AD FS-exemplaar](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed) wordt beschreven hoe u de AD FS-eindpunten vinden of het genereren van uw metagegevens-URL, bijvoorbeeld `https://fs.iga.azure-test.net/federationmetadata/2007-06/federationmetadata.xml`. 

|Kenmerk  |Value  |
|---------|---------|
|AssertionConsumerService     |`https://login.microsoftonline.com/login.srf`         |
|Doelgroep     |`urn:federation:MicrosoftOnline`         |
|Verlener     |De verlener URI van de partner-id-provider, bijvoorbeeld `http://www.example.com/exk10l6w90DHM0yi...`         |

De volgende claims moeten worden geconfigureerd in het SAML 2.0-token dat is uitgegeven door de id-provider:


|Kenmerk  |Value  |
|---------|---------|
|NameID-indeling     |`urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`         |
|emailaddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |


De volgende sectie ziet u hoe u de vereiste kenmerken en claims met AD FS als een voorbeeld van een SAML 2.0-id-provider configureren.

### <a name="before-you-begin"></a>Voordat u begint

Een AD FS-server moet al zijn ingesteld en de werkt voordat u deze procedure begint. Zie voor hulp bij het instellen van een AD FS-server, [maken van een test AD FS-3.0-exemplaar op een Azure-machine](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed).

### <a name="add-the-claim-description"></a>De claimbeschrijving toevoegen

1. Selecteer op uw AD FS-server **extra** > **AD FS-beheer**.
2. Selecteer in het navigatiedeelvenster **Service** > **Claimbeschrijvingen**.
3. Onder **acties**, selecteer **toevoegen Claimbeschrijving**.
4. In de **een Claimbeschrijving toevoegen** venster, geef de volgende waarden:

   - **Weergavenaam**: Permanente id
   - **Id claimen**: `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent` 
   - Schakel het selectievakje voor **de claimbeschrijving van deze publiceren in federatiemetagegevens als claimtype deze federation-service kan accepteren**.
   - Schakel het selectievakje voor **de claimbeschrijving van deze publiceren in federatiemetagegevens als claimtype die deze federation-service kan verzenden**.

5. Klik op **OK**.

### <a name="add-the-relying-party-trust-and-claim-rules"></a>De relying party trust toevoegen en claimregels

1. Op de AD FS-server, gaat u naar **extra** > **AD FS-beheer**.
2. Selecteer in het navigatiedeelvenster **vertrouwensrelaties** > **Relying Party-vertrouwensrelaties**.
3. Onder **acties**, selecteer **Relying Party Trust toevoegen**. 
4. In de relying party vertrouwensrelatie wizard toevoegen voor **gegevensbron selecteren**, gebruikt u de optie **importeren van gegevens over de relying party gepubliceerd online of op een lokaal netwerk**. Geef deze federatiemetagegevens URL - https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml. Laat andere standaardselecties. Selecteer **Sluiten**.
5. De **Claimregels bewerken** wizard wordt geopend.
6. In de **Claimregels bewerken** wizard de optie **regel toevoegen**. In **regeltype kiezen**, selecteer **LDAP-kenmerken als Claims verzenden**. Selecteer **Next**.
7. In **Claimregel configureren**, geef de volgende waarden: 

   - **Naam van claimregel**: Claimregel voor e-mail 
   - **Kenmerkopslag**: Active Directory 
   - **LDAP-kenmerk**: E-Mail-adressen 
   - **Uitgaande claimtype**: E-mailadres

8. Selecteer **Finish**.
9. De **Claimregels bewerken** venster wordt weergegeven van de nieuwe regel. Klik op **Toepassen**. 
10. Klik op **OK**.  

### <a name="create-an-email-transform-rule"></a>Een regel voor het transformeren van e-mailadres maken
1. Ga naar **Claimregels bewerken** en klikt u op **regel toevoegen**. In **regeltype kiezen**, selecteer **een binnenkomende Claim transformeren** en klikt u op **volgende**. 
2. In **Claimregel configureren**, geef de volgende waarden: 

   - **Naam van claimregel**: Regel voor e-transformatie 
   - **Type binnenkomende claim**: E-mailadres 
   - **Uitgaande claimtype**: Naam-ID 
   - **Uitgaande ID indeling**: Permanente id 
   - Selecteer **Alle claimwaarden doorgeven**.

3. Klik op **Voltooien**. 
4. De **Claimregels bewerken** venster wordt weergegeven van de nieuwe regels. Klik op **Toepassen**. 
5. Klik op **OK**. De AD FS-server is nu geconfigureerd voor directe Federatie met behulp van het SAML 2.0-protocol.

## <a name="configure-ad-fs-for-ws-fed-direct-federation"></a>AD FS voor WS-Federation direct federatie configureren 
Azure AD B2B kan worden geconfigureerd om te federeren met id-providers die gebruikmaken van het WS-Federation-protocol met de specifieke vereisten die hieronder worden vermeld. De twee WS-Federation-providers zijn op dit moment getest voor compatibiliteit met Azure AD zijn AD FS en Shibboleth. Hier gebruiken we Active Directory Federation Services (AD FS) als een voorbeeld van de WS-Federation-id-provider. Download de Azure AD Identity Provider compatibiliteit documenten voor meer informatie over het maken van een relying party-vertrouwensrelatie tussen een compatibele WS-Federation-provider met Azure AD.

Als u direct federation instelt, moeten de volgende kenmerken worden ontvangen in de WS-Federation-bericht van de id-provider. Deze kenmerken kunnen worden geconfigureerd door een koppeling naar de online service voor beveiligingstokens XML-bestand of door ze handmatig invoeren. Stap 12 in [maken van een test AD FS-exemplaar](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed) wordt beschreven hoe u de AD FS-eindpunten vinden of het genereren van uw metagegevens-URL, bijvoorbeeld `https://fs.iga.azure-test.net/federationmetadata/2007-06/federationmetadata.xml`.
 
|Kenmerk  |Value  |
|---------|---------|
|PassiveRequestorEndpoint     |`https://login.microsoftonline.com/login.srf`         |
|Doelgroep     |`urn:federation:MicrosoftOnline`         |
|Verlener     |De verlener URI van de partner-id-provider, bijvoorbeeld `http://www.example.com/exk10l6w90DHM0yi...`         |

Vereiste claims voor de WS-Federation-token dat is uitgegeven door de id-provider:

|Kenmerk  |Value  |
|---------|---------|
|ImmutableID     |`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`         |
|emailaddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |

De volgende sectie ziet u hoe u de vereiste kenmerken configureren en claims van AD FS gebruikt als een voorbeeld van een id-provider van WS-Federation.

### <a name="before-you-begin"></a>Voordat u begint
Een AD FS-server moet al zijn ingesteld en de werkt voordat u deze procedure begint. Zie voor hulp bij het instellen van een AD FS-server, [maken van een test AD FS-3.0-exemplaar op een Azure-machine](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed).


### <a name="add-the-relying-party-trust-and-claim-rules"></a>De relying party trust toevoegen en claimregels 
1. Op de AD FS-server, gaat u naar **extra** > **AD FS-beheer**. 
1. Selecteer in het navigatiedeelvenster **vertrouwensrelaties** > **Relying Party-vertrouwensrelaties**. 
1. Onder **acties**, selecteer **Relying Party Trust toevoegen**.  
1. In de relying party vertrouwensrelatie wizard voor toevoegen **gegevensbron selecteren**, gebruikt u de optie **importeren van gegevens over de relying party gepubliceerd online of op een lokaal netwerk**. Geef deze URL voor federatieve metagegevens: `https://nexus.microsoftonline-p.com/federationmetadata/2007-06/federationmetadata.xml`.  Laat andere standaardselecties. Selecteer **Sluiten**.
1. De **Claimregels bewerken** wizard wordt geopend. 
1. In de **Claimregels bewerken** wizard de optie **regel toevoegen**. In **regeltype kiezen**, selecteer **met behulp van een aangepaste regel Claims verzenden**. Selecteer *Next*. 
1. In **Claimregel configureren**, geef de volgende waarden:

   - **Naam van claimregel**: Probleem onveranderbare Id  
   - **Aangepaste regel**: `c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"] => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), query = "samAccountName={0};objectGUID;{1}", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);`

1. Selecteer **Finish**. 
1. De **Claimregels bewerken** venster wordt weergegeven van de nieuwe regel. Klik op **Toepassen**.  
1. In dezelfde **Claimregels bewerken** wizard de optie **regel toevoegen**. In **Cohose regeltype**, selecteer **LDAP-kenmerken als Claims verzenden**. Selecteer **Next**.
1. In **Claimregel configureren**, geef de volgende waarden: 

   - **Naam van claimregel**: Claimregel voor e-mail  
   - **Kenmerkopslag**: Active Directory  
   - **LDAP-kenmerk**: E-Mail-adressen  
   - **Uitgaande claimtype**: E-mailadres 

1.  Selecteer **Finish**. 
1.  De **Claimregels bewerken** venster wordt weergegeven van de nieuwe regel. Klik op **Toepassen**.  
1.  Klik op **OK**. De AD FS-server is nu geconfigureerd voor directe Federatie met behulp van WS-Federation.

## <a name="next-steps"></a>Volgende stappen
Vervolgens gaat u [direct federatie configureren in Azure AD](direct-federation.md#step-2-configure-direct-federation-in-azure-ad) in de Azure AD-portal of met behulp van PowerShell. 
