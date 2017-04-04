---
title: Veilige wachtwoorden in Azure AD en wachtwoorden opnieuw instellen die zijn geblokkeerd met Smart Password Lockout | Microsoft-documenten
description: Hierin wordt uitgelegd wat een Azure AD-tenant is en hoe u Azure beheert via Azure Active Directory
services: active-directory
documentationcenter: 
author: markvi
writer: v-lorisc
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/02/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: 8e625a346c9495d436a99fcf9eadf8ffeffcfdff
ms.lasthandoff: 03/28/2017


---
# <a name="secure-passwords--in-azure-ad-and-reset-passwords-that-get-blocked-by-smart-password-lockout"></a>Veilige wachtwoorden in Azure AD en wachtwoorden opnieuw instellen die zijn geblokkeerd met Smart Password Lockout
In dit artikel vindt u aanbevolen procedures die u als gebruiker of beheerder kunt volgen om uw accounts van Azure Active Directory (Azure AD) en de Microsoft-accountservice te beveiligen. 

 >[!NOTE]
 >Beheerders van Azure AD kunnen gebruikerswachtwoorden opnieuw instellen door op de directorynaam te klikken. Kies in de [Azure-beheerportal](https://manage.windowsazure.com) de pagina Gebruikers, klik op de naam van de gebruiker en klik op Wachtwoord opnieuw instellen. 
 >

In Azure AD worden de volgende algemene benaderingen gebruikt om wachtwoorden te beveiligen:
 *    Vereisten voor de lengte van het wachtwoord
 *    Vereisten voor de complexiteit van het wachtwoord
 *    Gepland en automatisch verlopen van wachtwoorden 

Zie [Manage passwords in Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-manage-passwords) (Wachtwoorden beheren in Azure Active Directory) voor meer informatie over functies voor wachtwoordbeheer. 

## <a name="azure-ad-password-protection"></a>Wachtwoordbeveiliging in Azure AD
Azure AD en het Microsoft-accountsysteem maken gebruik van gangbare methoden uit de branche om de wachtwoorden van gebruikers en beheerders optimaal te beveiligen. 

In dit gedeelte wordt beschreven hoe in Azure AD de volgende methoden worden toegepast om wachtwoorden te beveiligen:
 *    Dynamisch uitsluiten van wachtwoorden
 *    Smart Password Lockout

Zie het Engelstalige document [Password Guidance](http://aka.ms/passwordguidance) voor informatie over wachtwoordbeheer op basis van actueel onderzoek. 

### <a name="dynamically-banned-passwords"></a>Dynamisch uitsluiten van wachtwoorden
Azure AD en het Microsoft-accountsysteem garanderen de veiligheid van wachtwoorden door alle vaak voorkomende wachtwoorden dynamisch uit te sluiten. Het Azure AD Identity Protection-team maakt regelmatig analyses van lijsten met uitgesloten wachtwoorden, om te voorkomen dat gebruikers wachtwoorden selecteren die regelmatig worden gebruikt. Deze service is beschikbaar voor klanten van Azure AD en de Microsoft-accountservice. 

Het is belangrijk dat beheerders hun gebruikers adviseren om bij het maken van wachtwoorden ongebruikelijke wachtwoorden te kiezen die bestaan uit een unieke combinatie van letters, cijfers en tekens. Op deze manier wordt het bijna onmogelijk om gebruikerswachtwoorden te achterhalen. 

**Gegevensschendingen**

Azure AD stelt altijd alles in het werk om cybercriminelen een stap voor te blijven. Dit doen we onder andere door te voorkomen dat gebruikers wachtwoorden maken die in een lijst staan met wachtwoorden die bekend zijn bij cybercriminelen.

Het Azure AD Identity Protection-team analyseert voortdurend wachtwoorden die vaak worden gebruikt. Cybercriminelen maken gebruik van vergelijkbare strategieën voor hun aanvallen, zoals het samenstellen van een [rainbow-tabel](https://en.wikipedia.org/wiki/Rainbow_table) om wachtwoord-hashes te kraken. 

Microsoft analyseert voortdurend [gegevensschendingen](https://www.privacyrights.org/data-breaches) om een dynamisch bijgewerkte lijst met uitgesloten wachtwoorden te onderhouden die ervoor zorgt dat kwetsbare wachtwoorden worden uitgesloten voordat ze een echte bedreiging kunnen vormen voor klanten van Azure AD. Raadpleeg het [Microsoft Security Intelligence Report](https://www.microsoft.com/security/sir/default.aspx) voor meer informatie over onze inspanningen op dit gebied. Dit rapport is alleen in het Engels beschikbaar. 

### <a name="smart-password-lockout"></a>Smart Password Lockout

Als Azure AD vaststelt dat een mogelijke cybercrimineel probeert om het wachtwoord van een gebruiker te achterhalen, wordt het gebruikersaccount vergrendeld met Smart Password Lockout. Azure AD bevat logica om het risico te bepalen dat samenhangt met specifieke aanmeldingssessies. 

Met behulp van de meest recente beveiligingsgegevens wordt er semantiek voor accountvergrendeling toegepast bij bedreigingen door cybercriminelen. Op deze manier worden accounts van gebruikers niet vergrendeld wanneer een cybercrimineel wachtwoorden van gebruikers in het netwerk heeft achterhaald.

Als de toegang van een gebruiker tot Azure AD tijdelijk is geblokkeerd, zien ze een scherm zoals dit:

  ![Toegang tot Azure AD geblokkeerd](./media/active-directory-secure-passwords/locked-out-azuread.png)
  
Voor andere Microsoft-accounts ziet het scherm er dan ongeveer zo uit:

  ![Toegang tot Microsoft-account geblokkeerd](./media/active-directory-secure-passwords/locked-out-ms-accounts.png)

Zie [How password management works](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-how-it-works) (Wachtwoordbeheer in Azure Active Directory) voor meer informatie over wachtwoordbeheer.

  >OPMERKING: Als u een Azure AD-beheerder bent, kunt u [Windows Hello](https://www.microsoft.com/en-us/windows/windows-hello) gebruiken om ervoor te zorgen dat uw gebruikers helemaal geen traditionele wachtwoorden meer hoeven te maken.
  >

## <a name="next-steps"></a>Volgende stappen
[Uw eigen wachtwoord bijwerken](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-update-your-own-password)<br>
[The fundamentals of Azure identity management](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity) (De grondbeginselen van Azure-identiteitsbeheer)<br>
[How to get operational insights with password management reports](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-get-insights#view-password-reset-activity) (Operationeel inzicht verkrijgen met rapporten voor wachtwoordbeheer)



