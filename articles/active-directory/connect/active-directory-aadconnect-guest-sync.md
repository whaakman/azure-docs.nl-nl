---
title: Synchroniseren van gebruikersaccounts van het Gast-gebruikers die e-mail voor aanmelding bij Azure gebruiken | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe Gast-gebruikersaccounts die gebruikmaken van een alternatieve ID aanmelden om toepassingen te synchroniseren.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/19/2018
ms.author: billmath
ms.openlocfilehash: 4e6cf3bb4a691380a5fe22f5afdf749b40f15ef3
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="synchronizing-guest-user-accounts-that-use-email-for-sign-in-preview"></a>Synchroniseren van gebruikersaccounts Gast die e-mail voor aanmelding (Preview gebruiken)

>[!NOTE]
> Deze functie is momenteel in de openbare preview.

Het volgende scenario adres de situatie waar mogelijk hebt u externe gebruikers in uw on-premises AD-omgeving, zoals partners die gebruikmaken van een alternatieve methode voor aanmelden.

In het voorgaande voorbeeld Nina Morin voor Fabrikam werkt en ze heeft de volgende e-mailadres: nmorin@fabrikam.com. Ties is een partner met Contoso en ze nodig heeft toegang tot bepaalde toepassingen die Contoso heeft. Contoso heeft een account voor ties gemaakt en heeft geleid ties haar e-mailadres gebruiken om aan te melden voor toepassingen.

Voor hun on-premises toepassingen heeft dit scenario gewerkt groot. Maar nu Contoso deze toepassingen naar de cloud wordt verplaatst en wil hebben dezelfde ervaring voor hun partners.

Dit scenario lost deze situatie.


## <a name="pre-requisites-and-assumptions"></a>Vereisten en veronderstellingen
Deze sectie bevat een lijst met vereisten en veronderstellingen u moet moet weten voordat u aan voor het instellen van dit scenario.

### <a name="pre-requisites"></a>Vereisten
- Globale beheerdersreferenties voor het configureren van Azure AD Connect domeinen controleren en federation-domeininstellingen configureren
- Azure AD Connect versie 1.1.524.0 of hoger
- Geverifieerde domein voor het instellen van de cloud-UPN van externe gebruikers (voorbeeld: bmcontoso.com).
- Federation-Service om uw externe gebruikers te verifiëren. Als u AD FS gebruikt, moet dit 2012 R2 of hoger
- MSOL PowerShell v1.1 is geïnstalleerd op een machine om te controleren of de federation-instellingen. Zie voor meer informatie [Azure Active Directory (MSOnline)](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-1.0).


### <a name="assumptions"></a>Veronderstellingen 
- Azure AD Connect is al ingesteld en de installatie is voltooid. Zie voor meer informatie over het installeren van Azure AD Connect [Azure AD Connect en Federatie](active-directory-aadconnectfed-whatis.md).
Dit document worden de volgende veronderstellingen:
- u hebt een federation-service instellen en dat deze is met succes verifiëren van gebruikers.
- externe gebruikers kunnen zich verifiëren met het externe e-mailadres.
- - Een alternatieve ID gebruiken voor aanmelding is ingesteld en geconfigureerd. Gebruikers kunnen zich verifiëren met hun alternatieve-ID. Zie voor meer informatie over het instellen van een alternatieve ID met AD FS [Alternate Login ID configureren](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id).

## <a name="task-1--prepare-the-environment"></a>Taak 1: De omgeving voorbereiden
De volgende taak is van een informatieve zodat u klaar om te beginnen met het synchroniseren van uw externe accounts bent, zodat ze kunnen aanmelden met een alternatieve i zoals het e-mailkenmerk.

Definieer de items in de onderstaande tabel voordat u doorgaat naar de tweede taak.

![Architectuur](media/active-directory-aadconnect-guest-sync/guest2.png)

|Aspect omgeving|Wat wordt deze gebruikt?|Implementatie in uw omgeving|
|-----|-----|-----|
|Cloud-UPN-kenmerk|Het kenmerk dat de UPN van de externe gebruikersobjecten in de cloud wordt gevuld. Het UPN-achtervoegsel voor de externe accounts moet worden gedefinieerd in de vereisten. Dit is het geverifieerde domein.|* Voorbeeld: UserPrincipalName (nmorin@bmcontoso.com)|
|Adres van de aanmeldingspagina|Het kenmerk dat externe gebruikers wilt invoeren wanneer u zich aanmeldt. Dit kenmerk moet een e-mailadresindeling en in de meeste gevallen deze samenvalt met de werkelijke e-mailadres van de externe gebruiker.|* Voorbeeld: e-(nmorin@fabrikam.com)|
|Azure AD Connect bereik Filter|De filter waarmee die gericht is op de externe identiteit om het bereik in de synchronisatieregels gedefinieerd verderop in deze handleiding. Typische manieren bereik, bijvoorbeeld: een vooraf gedefinieerde organisatie-eenheid in de organisatie, een bepaalde naamgevingsconventie, een specifiek domein, enzovoort.|* Voorbeeld: Organisatie-eenheid bevat externe sleutels|
|Azure AD-tenant|De naam van de Azure AD-tenant zoals deze wordt weergegeven bij Azure AD Connect.|Voorbeeld: contoso.onmicrosoft.com|

De volgende schermafbeelding heeft drie vakken die worden beschreven.
- De **Externals** organisatie-eenheid die wordt gebruikt in het Azure AD Connect binnen het bereik van Filter en de locatie van de externe gebruikers.
- De **mail** kenmerk wordt gebruikt door de externe gebruikers aanmelden.
- De **userPrincipalName** het geverifieerde domein die de on-premises omgeving is gefedereerd met kenmerk.

![Gebruiker](media/active-directory-aadconnect-guest-sync/guest1.png)

## <a name="task-2--configure-azure-ad-connect"></a>Taak 2: Configureer Azure AD Connect
Zodra u de bovenstaande informatie gedefinieerd hebt, kunnen we voor het instellen van de Azure AD Connect-synchronisatieregels op verplaatsen. Als u de regels instelt, gebruiken de regeleditor voor Azure AD Connect-synchronisatie. Zie voor meer informatie over de editor [declaratieve inrichting](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

### <a name="how-to-configure-the-synchronization-rule"></a>Het configureren van de synchronisatieregel
Gebruik de volgende procedure voor het configureren van Azure AD Connect.

1. Open de regeleditor voor Azure AD Connect-synchronisatie door te gaan naar **Start - Azure AD Connect - synchronisatie regeleditor**.
2. Op de **synchronisatie regeleditor** scherm, controleert u de richting **inkomende** en klik op het recht op **nieuwe regel toevoegen**.
3. Op de **beschrijving** pagina, configureer het volgende en klik op **volgende**.
    - **Naam** -Voer een naam voor de regel 
    - **Verbonden systeem:** -de on-premises AD-omgeving
    - **Verbonden systeem objecttype:** -gebruiker
    - **Metaverse-objecttype:** -persoon
    - **Koppelingstype:** -koppelen
    - **Prioriteit:** - 90
    - 
![](media/active-directory-aadconnect-guest-sync/guest3.png)

4. Op de **bereikfilter** scherm, klikt u op **groep toevoegen**.
5. Gebruik de vervolgkeuzelijsten voor het configureren van het filter. Voer het volgende en klik op **volgende**. Deze actie wordt een filter dat alleen van toepassing op objecten die zich in de externe organisatie-eenheid gemaakt.
    - **Kenmerk** -dn
    - **Operator** -bevat
    - **Waarde** -Externals
 
 ![Filteren](media/active-directory-aadconnect-guest-sync/guest4.png)

6. Op de **Join regels** scherm, klikt u op **volgende**.
7. Op de **transformaties** scherm, klikt u op **transformatie toevoegen**. Voer de volgende informatie in:
    - **FlowType** - constante
    - **Kenmerk doel** -userType
    - **Bron** - Gast
8. Op de **transformaties** scherm, klikt u op **transformatie toevoegen**. Voer de volgende informatie in:
    - **FlowType** - Direct
    - **Kenmerk doel** -onPremisesUserPrincipalName
    - **Bron** -mail
9. Op de **transformaties** scherm, klikt u op **transformatie toevoegen**. Voer de volgende informatie in:
    - **FlowType** - Direct
    - **Kenmerk doel** -userPrincipalName
    - **Bron** -userPrincipalName ![transformatie](media/active-directory-aadconnect-guest-sync/guest5.png)
10. Klik op **Add**. 
11. Op de **synchronisatie regeleditor** scherm, controleert u de richting **uitgaande** en klik op het recht op **nieuwe regel toevoegen**.
12. Op de **beschrijving** pagina Configureer het volgende en klik op **volgende**.
    - **Naam** -Voer een naam voor de regel 
    - **Verbonden systeem:** -Azure AD-tenant
    - **Verbonden systeem objecttype:** -gebruiker
    - **Metaverse-objecttype:** -persoon
    - **Type koppeling** -koppelen
    - **Prioriteit:** - 90
    - 
![Regel](media/active-directory-aadconnect-guest-sync/guest6.png)

13. Op de **Scoping filter** scherm, klikt u op **volgende**.
14. Op de **Join regels** scherm, klikt u op **volgende**.
15. Op de **transformaties** scherm, klikt u op **transformatie toevoegen**.  Voer de volgende informatie in:
    - **FlowType** - Direct
    - **Kenmerk doel** -userType
    - **Bron** -userType
9. Op de **transformaties** scherm, klikt u op **transformatie toevoegen**. Voer de volgende informatie in:
    - **FlowType** - Direct
    - **Kenmerk doel** -onPremisesUserPrincipalName
    - **Bron** -onPremisesUserPrincipalName ![transformatie](media/active-directory-aadconnect-guest-sync/guest7.png)
10. Klik op **Add**. 
11. Wanneer u deze regels hebt geconfigureerd, moet u een volledige synchronisatie uitvoeren. PowerShell gebruiken om een volledige synchronisatie te starten. Nadat de synchronisatie is voltooid, kunt u doorgaan met de volgende stap.

``` powershell
    Start-ADSyncSyncCycle -PolicyType Initial
```

## <a name="task-3--federation"></a>Taak 3: Federation
De volgende taak is een informatief op een aantal dingen die u nodig hebt in-place voor het scenario om te werken.

U kunt uw federation-instellingen verifiëren met Azure met Azure AD PowerShell. Dit document maakt gebruik van de v1.1 van MSOL PowerShell. U kunt deze versie installeren [hier](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-1.0).

### <a name="verify-the-federation-settings"></a>Controleer of de federation-instellingen
Gebruik de volgende procedure om te controleren of de federation-instellingen.
1. Open Windows PowerShell en verbinding maken met Azure AD met behulp van de volgende opdracht:
``` powershell
      Connect-MSOLservice
```
2.  Voer de referenties van de globale beheerder
3.  Voer nu de volgende opdracht:
  ``` powershell
      Get-MSOLDomainFederationSettings
  ```
4.   U ziet dat de federation-informatie moet worden geretourneerd. Opmerking de **ActiveLogonUri** is de URL van de federation-server.

  ![Federatie](media/active-directory-aadconnect-guest-sync/guest8.png)

### <a name="verify-alternate-login-id"></a>Controleer of de alternatieve aanmeldings-ID
Dit document maakt gebruik van AD FS als de id-provider (Idp). Als u een andere Idp gebruikt, kunnen deze stappen zeer.

1. Open Windows PowerShell en voer de volgende opdracht:
   ```powershell
    Get-ADFSClaimsProviderTrust
   ```
2. Hier ziet u de AD FS-informatie.  Opmerking de **AlternateLoginID** en **LookupForests**.

![ADFS](media/active-directory-aadconnect-guest-sync/guest9.png)

## <a name="task-4--testing"></a>Taak 4: testen
Om te controleren of deze correct werkt, moet u aanmelden bij een eindpunt dat is geconfigureerd om te verifiëren met behulp van de Idp. U kunt dit testen we een website in Azure wordt geïmplementeerd en gebruikt de volgende url: contososampapp.azurewebsites.net

### <a name="verify-that-you-can-sign-in-with-the-alternate-id"></a>Controleer of u kunt aanmelden met de alternatieve-ID
1. Aanmelden met het eindpunt.</br>
![Endpoint](media/active-directory-aadconnect-guest-sync/guest10.png)
1. Voer uw gebruikersnaam en u zal worden omgeleid naar de aanmeldingspagina van Federatie.
![Federation](media/active-directory-aadconnect-guest-sync/guest11.png)
1. Geef uw referenties.
2. U moet nu worden aangemeld.
![Geslaagd](media/active-directory-aadconnect-guest-sync/guest12.png)

## <a name="next-steps"></a>Volgende stappen
- [Eigenschappen van een gebruiker met Azure Active Directory B2B-samenwerking](../../active-directory/b2b/user-properties.md#key-properties-of-the-azure-ad-b2b-collaboration-user)
- [Alternatieve aanmeldings-ID configureren](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id)
- [Versiegeschiedenis van Azure AD Connect](active-directory-aadconnect-version-history.md)
