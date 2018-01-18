---
title: Wachtwoordsynchronisatie met Azure AD Connect-synchronisatie implementeren | Microsoft Docs
description: Bevat informatie over hoe werkt Wachtwoordsynchronisatie en hoe u instelt.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 05f16c3e-9d23-45dc-afca-3d0fa9dbf501
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: billmath
ms.openlocfilehash: 2a28089c85150f1876cd0ade58eb128e2d28f778
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="implement-password-synchronization-with-azure-ad-connect-sync"></a>Wachtwoordsynchronisatie met Azure AD Connect-synchronisatie implementeren
In dit artikel bevat informatie die u wilt synchroniseren van uw wachtwoorden van een lokale Active Directory-exemplaar naar een cloud-gebaseerde Azure Active Directory (Azure AD)-exemplaar.

## <a name="what-is-password-synchronization"></a>Wat is synchronisatie van wachtwoorden
De kans dat u bent geblokkeerd toegang krijgen tot uw werk vanwege een vergeten wachtwoord is gerelateerd aan het aantal verschillende wachtwoorden die u moet onthouden. De wachtwoorden die meer hoeft te onthouden is, hoe groter de kans op een vergeten. Vragen en oproepen over wachtwoorden en andere problemen met wachtwoord vereisen de meeste bronnen van de helpdesk.

Wachtwoordsynchronisatie is een functie die wordt gebruikt voor het synchroniseren van gebruikerswachtwoorden van een lokale Active Directory-exemplaar naar een cloud-gebaseerde Azure AD-exemplaar.
Gebruik deze functie aan te melden bij Azure AD-services zoals Office 365, Microsoft Intune, CRM Online en Azure Active Directory Domain Services (Azure AD DS). U zich aanmeldt bij de service met behulp van hetzelfde wachtwoord dat u aan te melden bij uw lokale Active Directory-exemplaar gebruiken.

![Wat is Azure AD Connect?](./media/active-directory-aadconnectsync-implement-password-synchronization/arch1.png)

Vermindert het aantal wachtwoorden, moeten uw gebruikers tot slechts één onderhouden. Wachtwoordsynchronisatie helpt u bij:

* De productiviteit van uw gebruikers verbeteren.
* Uw helpdesk kosten te verlagen.  

Ook als u besluit om te gebruiken [Federatie met Active Directory Federation Services (AD FS)](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect), kunt u eventueel instellen Wachtwoordsynchronisatie als een back-up als uw AD FS-infrastructuur is mislukt.

Wachtwoordsynchronisatie is een uitbreiding op de functie voor directory-synchronisatie is geïmplementeerd door Azure AD Connect-synchronisatie. Wachtwoordsynchronisatie gebruiken in uw omgeving, moet u naar:

* Azure AD Connect installeren.  
* Adreslijstsynchronisatie configureert tussen uw lokale Active Directory-exemplaar en uw Azure Active Directory-exemplaar.
* Wachtwoordsynchronisatie inschakelen.

Zie [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md) (Uw on-premises identiteiten integreren met Azure Active Directory) voor meer informatie.

> [!NOTE]
> Zie voor meer informatie over Azure Active Directory Domain Services geconfigureerd voor FIPS en Wachtwoordsynchronisatie 'Wachtwoordsynchronisatie en FIPS' verderop in dit artikel.
>
>

## <a name="how-password-synchronization-works"></a>Hoe werkt Wachtwoordsynchronisatie
Wachtwoorden worden opgeslagen in de vorm van een weergave van de hash-waarde van het wachtwoord van de werkelijke gebruiker van de service Active Directory-domein. Een hash-waarde is een resultaat van een wiskundige eenrichtingsfunctie (de *hash-algoritme*). Er is geen methode om het resultaat van een eenrichtingsfunctie naar de versie van de tekst zonder opmaak van een wachtwoord terug te zetten. U kunt een wachtwoord-hash niet gebruiken aan te melden bij uw on-premises netwerk.

Als u wilt synchroniseren van uw wachtwoord extraheert Azure AD Connect-synchronisatie uw wachtwoord-hash van de lokale Active Directory-exemplaar. Verwerking van de extra beveiliging wordt toegepast op de wachtwoordhash voordat deze wordt gesynchroniseerd met de verificatieservice voor Azure Active Directory. Wachtwoorden worden gesynchroniseerd, op basis van een gebruiker en in chronologische volgorde.

De werkelijke gegevensstroom van het synchronisatieproces wachtwoord is vergelijkbaar met de synchronisatie van gebruikersgegevens, zoals DisplayName of e-mailadressen. Wachtwoorden worden echter vaker dan het standaard directory-synchronisatie-venster voor andere kenmerken gesynchroniseerd. Het synchronisatieproces wachtwoord wordt elke twee minuten uitgevoerd. U kunt de frequentie van dit proces niet wijzigen. Wanneer u een wachtwoord synchroniseert, overschrijft het bestaande wachtwoord van de cloud.

De eerste keer dat u de functie van de synchronisatie van wachtwoord inschakelt wordt uitgevoerd een initiële synchronisatie van de wachtwoorden van alle gebruikers die in het bereik. U kunt geen expliciet definiëren een subset van wachtwoorden van gebruikers die u wilt synchroniseren.

Wanneer u een on-premises wachtwoord wijzigt, is het bijgewerkte wachtwoord gesynchroniseerd, meestal binnen een paar minuten.
De synchronisatiefunctie wachtwoord mislukte synchronisatiepogingen wordt automatisch opnieuw geprobeerd. Als er een fout optreedt tijdens een poging om te synchroniseren van een wachtwoord, wordt er een fout vastgelegd in de logboeken.

De synchronisatie van een wachtwoord heeft geen invloed op de gebruiker die momenteel is aangemeld.
De huidige sessie van de cloud-service wordt niet direct beïnvloed door een gesynchroniseerde wachtwoordwijziging die optreedt terwijl u bent aangemeld bij een service in de cloud. Als u de cloudservice moet u opnieuw te verifiëren, moet u uw nieuwe wachtwoord opgeven.

Een gebruiker moet hun zakelijke referenties nogmaals invoeren om te verifiëren met Azure AD, ongeacht of ze bent aangemeld bij het bedrijfsnetwerk. Deze patroon kan echter worden geminimaliseerd als de gebruiker selecteert de mij ondertekend behouden in het selectievakje (KMSI) bij aanmelding in. Deze selectie stelt een sessiecookie dat verificatie voor een korte periode omzeilt. KMSI gedrag kan worden ingeschakeld of uitgeschakeld door de Azure AD-beheerder.

> [!NOTE]
> Wachtwoordsynchronisatie wordt alleen ondersteund voor de gebruiker van het type object in Active Directory. Dit wordt niet ondersteund voor het objecttype iNetOrgPerson.

### <a name="detailed-description-of-how-password-synchronization-works"></a>Gedetailleerde beschrijving van hoe Wachtwoordsynchronisatie werkt
Hieronder wordt beschreven diepgaande hoe werkt Wachtwoordsynchronisatie tussen Active Directory en Azure AD.

![Gedetailleerde wachtwoord stroom](./media/active-directory-aadconnectsync-implement-password-synchronization/arch3.png)


1. Elke twee minuten de agent voor Wachtwoordsynchronisatie op de AD Connect-server aanvragen opgeslagen wachtwoord-hashes (het kenmerk unicodePwd) van een DC via de standaard [MS DRSR](https://msdn.microsoft.com/library/cc228086.aspx) replicatie-protocol gebruikt voor het synchroniseren van gegevens tussen DC's. Het serviceaccount moet een directorywijzigingen repliceren en repliceren Directory wijzigingen alle AD machtigingen (standaard op de installatie) tot de wachtwoord-hashes ophalen.
2. Voordat u verzendt, de domeincontroller de wachtwoordhash MD4 versleuteld met behulp van een sleutel die is een [MD5](http://www.rfc-editor.org/rfc/rfc1321.txt) hash van de sessiesleutel RPC en een salt. Vervolgens verzendt via RPC het resultaat naar de agent voor Wachtwoordsynchronisatie. De DC geeft ook de salt aan de agent voor Wachtwoordsynchronisatie met behulp van het protocol van de DC-replicatie, zodat de agent wordt de envelop ontsleutelen.
3.  Nadat de agent voor Wachtwoordsynchronisatie de versleutelde envelop is, gebruikt [MD5CryptoServiceProvider](https://msdn.microsoft.com/library/System.Security.Cryptography.MD5CryptoServiceProvider.aspx) en de salt voor het genereren van een sleutel voor het ontsleutelen van de ontvangen gegevens terug naar de oorspronkelijke MD4-indeling. Op geen enkel heeft de agent voor Wachtwoordsynchronisatie toegang tot het wachtwoord met ongecodeerde tekst. De agent voor Wachtwoordsynchronisatie van gebruik van MD5 geldt uitsluitend voor replicatie protocol compatibiliteit met de domeincontroller en wordt alleen gebruikt on-premises tussen de domeincontroller en de agent voor Wachtwoordsynchronisatie.
4.  De agent voor Wachtwoordsynchronisatie breidt de 16-bytes binary wachtwoordhash 64 bytes door eerst te converteren back-the-hash naar een hexadecimale tekenreeks van 32-byte, vervolgens deze tekenreeks te converteren naar binair met UTF-16-codering.
5.  De agent voor Wachtwoordsynchronisatie voegt een salt die bestaan uit een lengte in 10 bytes salt, naar de 64-bytes binary naar de oorspronkelijke hash verder te beschermen.
6.  De agent voor Wachtwoordsynchronisatie combineert de MD4-hash plus salt en ingangen in de [PBKDF2](https://www.ietf.org/rfc/rfc2898.txt) functie. 1000 herhalingen van de [HMAC SHA256](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) sleutelhash hash-algoritme wordt gebruikt. 
7.  De agent voor Wachtwoordsynchronisatie de resulterende 32-byte-hash heeft, worden zowel de salt en het aantal iteraties SHA256 ernaar (voor gebruik door Azure AD) en brengt de tekenreeks van Azure AD Connect naar Azure AD via SSL.</br> 
8.  Wanneer een gebruiker wil zich aanmelden bij Azure AD en hun wachtwoord invoert, wordt het wachtwoord uitvoeren via de dezelfde MD4 + salt + PBKDF2 + HMAC SHA256-proces. Als de resulterende hash overeenkomt met de hash die is opgeslagen in Azure AD, wordt de gebruiker het juiste wachtwoord heeft ingevoerd en is geverifieerd. 

>[!Note] 
>De oorspronkelijke MD4-hash is niet verzonden naar Azure AD. In plaats daarvan wordt de SHA256-hash van de oorspronkelijke MD4-hash verzonden. Worden als gevolg hiervan, als de hash die is opgeslagen in Azure AD wordt verkregen, deze kan niet gebruikt in een lokale pass-the-hash-aanval.

### <a name="how-password-synchronization-works-with-azure-active-directory-domain-services"></a>Hoe werkt Wachtwoordsynchronisatie met Azure Active Directory Domain Services
U kunt de functie wachtwoord synchronisatie ook gebruiken om te synchroniseren van uw on-premises wachtwoorden te [Azure Active Directory Domain Services](../../active-directory-domain-services/active-directory-ds-overview.md). In dit scenario verifieert het Azure Active Directory Domain Services-exemplaar uw gebruikers in de cloud met alle methoden die beschikbaar zijn in uw lokale Active Directory-exemplaar. De ervaring van dit scenario is vergelijkbaar met behulp van de Active Directory-migratie (ADMT) in een on-premises omgeving.

### <a name="security-considerations"></a>Beveiligingsoverwegingen
Bij het synchroniseren van wachtwoorden, wordt de tekst zonder opmaak-versie van uw wachtwoord niet blootgesteld aan de functie wachtwoord synchronisatie naar Azure AD of een van de bijbehorende services.

Gebruikersverificatie vindt plaats met Azure AD in plaats van op basis van de Active Directory-exemplaar van het bedrijf. Als uw organisatie opmerkingen over de wachtwoordgegevens in een formulier van het bedrijf verlaten, overweeg dan het feit dat de wachtwoordgegevens SHA256 in Azure AD opgeslagen--is een hash van de oorspronkelijke MD4-hash--veel veiliger is dan wat wordt opgeslagen in Active Directory. Verder, omdat deze SHA256-hash kan niet worden ontsleuteld, het niet kan worden teruggebracht naar Active Directory-omgeving van de organisatie en weergegeven als een geldige gebruiker-wachtwoord in een pass-the-hash-aanval.





### <a name="password-policy-considerations"></a>Overwegingen bij het beleid van wachtwoord
Er zijn twee soorten beleid voor wachtwoorden die worden beïnvloed door Wachtwoordsynchronisatie inschakelen:

* Beleid voor wachtwoordcomplexiteit
* Vervalbeleid voor wachtwoorden

#### <a name="password-complexity-policy"></a>Beleid voor wachtwoordcomplexiteit  
Als Wachtwoordsynchronisatie is ingeschakeld, overschrijven de instellingen voor de complexiteit van wachtwoordbeleid in uw lokale Active Directory-exemplaar complexiteit beleid in de cloud voor gesynchroniseerde gebruikers. U kunt alle geldige wachtwoorden van uw lokale Active Directory-exemplaar gebruiken voor toegang tot Azure AD-services.

> [!NOTE]
> Wachtwoorden voor gebruikers die rechtstreeks in de cloud zijn gemaakt nog steeds worden wachtwoordbeleid zoals gedefinieerd in de cloud.

#### <a name="password-expiration-policy"></a>Vervalbeleid voor wachtwoorden  
Als een gebruiker zich in het bereik van de synchronisatie van wachtwoorden, het wachtwoord voor de cloud-account is ingesteld op *nooit verlopen*.

U kunt blijven aan te melden bij uw cloudservices verkregen met behulp van een gesynchroniseerde wachtwoord dat is verlopen in uw on-premises omgeving. Uw wachtwoord cloud wordt bijgewerkt zodra die u het wachtwoord in de on-premises omgeving wijzigen.

#### <a name="account-expiration"></a>Account verloopt
Als uw organisatie gebruikmaakt van het kenmerk accountExpires als onderdeel van Gebruikersaccountbeheer, let er dan voor dat dit kenmerk is niet gesynchroniseerd naar Azure AD. Als gevolg hiervan wordt verlopen Active Directory-account in een omgeving die is geconfigureerd voor Wachtwoordsynchronisatie nog steeds actief zijn in Azure AD. We raden aan dat als het account is verlopen, een werkstroomactie moet worden geactiveerd door een PowerShell-script waarmee de gebruiker Azure AD-account worden uitgeschakeld. Als u daarentegen wanneer het account is ingeschakeld, moet de Azure AD-exemplaar worden ingeschakeld.

### <a name="overwrite-synchronized-passwords"></a>Gesynchroniseerde wachtwoorden overschrijven
Een beheerder kan handmatig uw wachtwoord opnieuw instellen met behulp van Windows PowerShell.

In dit geval wordt het nieuwe wachtwoord overschrijft uw gesynchroniseerde wachtwoord en alle wachtwoordbeleidsregels gedefinieerd in de cloud worden toegepast op het nieuwe wachtwoord.

Als u uw on-premises wachtwoord wijzigt, het nieuwe wachtwoord is gesynchroniseerd met de cloud en heeft deze voorrang op de handmatig bijgewerkte wachtwoord.

De synchronisatie van een wachtwoord heeft geen invloed op de Azure-gebruiker die is aangemeld. De huidige sessie van de cloud-service wordt niet direct beïnvloed door een gesynchroniseerde wachtwoordwijziging die optreedt terwijl u bent aangemeld bij een service in de cloud. KMSI breidt de duur van dit verschil. Wanneer de cloudservice u opnieuw te verifiëren moet, moet u uw nieuwe wachtwoord op te geven.

### <a name="additional-advantages"></a>Extra voordelen

- Wachtwoordsynchronisatie is over het algemeen eenvoudiger te implementeren dan een federation-service. Deze geen extra servers vereist en elimineert de afhankelijkheid van een maximaal beschikbare federation-service om gebruikers te verifiëren. 
- Wachtwoordsynchronisatie kan ook worden ingeschakeld naast federatieve. Worden kan gebruikt als terugval als een storing optreedt in uw federatieservice.












## <a name="enable-password-synchronization"></a>Wachtwoordsynchronisatie inschakelen
Wanneer u Azure AD Connect installeert met behulp van de **Expresinstellingen** , Wachtwoordsynchronisatie is automatisch ingeschakeld. Zie voor meer informatie [aan de slag met Azure AD Connect met expresinstellingen](active-directory-aadconnect-get-started-express.md).

Als u aangepaste instellingen wanneer u Azure AD Connect installeert, is Wachtwoordsynchronisatie beschikbaar op de aanmeldingspagina van gebruiker. Zie voor meer informatie [aangepaste installatie van Azure AD Connect](active-directory-aadconnect-get-started-custom.md).

![Wachtwoordsynchronisatie inschakelen](./media/active-directory-aadconnectsync-implement-password-synchronization/usersignin.png)

### <a name="password-synchronization-and-fips"></a>Wachtwoordsynchronisatie en FIPS
Als uw server is vergrendeld volgens Federal Information Processing Standard (FIPS), wordt MD5 uitgeschakeld.

**Als u wilt MD5 voor wachtwoordsynchronisatie inschakelen, moet u de volgende stappen uitvoeren:**

1. Ga naar %programfiles%\Azure AD Sync\Bin.
2. Open miiserver.exe.config.
3. Ga naar het knooppunt configuration/runtime aan het einde van het bestand.
4. Het volgende knooppunt toevoegen:`<enforceFIPSPolicy enabled="false"/>`
5. Sla uw wijzigingen op.

Voor een verwijzing naar is dit fragment wat er moet eruitzien als:

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

Zie voor meer informatie over beveiliging en FIPS [AAD Password Sync, versleuteling en FIPS-naleving](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/).

## <a name="troubleshoot-password-synchronization"></a>Probleem met Wachtwoordsynchronisatie oplossen
Als u problemen met synchronisatie van wachtwoorden hebt, raadpleegt u [Wachtwoordsynchronisatie oplossen](active-directory-aadconnectsync-troubleshoot-password-synchronization.md).

## <a name="next-steps"></a>Volgende stappen
* [Azure AD Connect-synchronisatie: Synchronisatieopties voor aanpassen](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
