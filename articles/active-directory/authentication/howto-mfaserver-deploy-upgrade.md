---
title: Azure MFA Server upgrade
description: Stappen en richtlijnen voor het bijwerken van de Azure multi-factor Authentication-Server naar een nieuwere versie.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: faf04c870ec72f7ff62234b5bcaadde0b8a0a144
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56190072"
---
# <a name="upgrade-to-the-latest-azure-multi-factor-authentication-server"></a>Een upgrade uitvoert naar de meest recente Azure multi-factor Authentication-Server

Dit artikel helpt u bij het verwerken van het upgraden van Azure multi-factor Authentication (MFA)-Server versie 6.0 of hoger. Als u een oude versie van de PhoneFactor-Agent upgraden wilt, raadpleegt u [de PhoneFactor Agent bijwerken naar Azure multi-factor Authentication-Server](howto-mfaserver-deploy-upgrade-pf.md).

Als u een van versie 6.x of naar v7.x oudere of nieuwere upgrade uitvoert, wordt de status van alle onderdelen gewijzigd van .NET 2.0 in .NET 4.5. Alle onderdelen moeten ook Microsoft Visual C++ 2015 Redistributable Update 1 of hoger. Het installatieprogramma van de MFA-Server installeert zowel de x86 x64-versies van deze onderdelen als ze niet al zijn geïnstalleerd. Als de Gebruikersportal en de webservice voor mobiele Apps worden uitgevoerd op afzonderlijke servers, moet u deze om pakketten te installeren vóór de upgrade van deze onderdelen. U kunt zoeken naar de nieuwste update van Microsoft Visual C++ 2015 Redistributable op de [Microsoft Download Center](https://www.microsoft.com/download/). 

Upgradestappen in één oogopslag:

* Upgrade uitvoeren voor Azure MFA-Servers (onderliggende niveaus vervolgens Master)
* Voer een upgrade de Gebruikersportal-instanties
* Voer een upgrade de instanties van de AD FS-Adapter

## <a name="upgrade-azure-mfa-server"></a>Azure MFA-Server upgraden

1. Volg de instructies in [downloaden van de Azure multi-factor Authentication-Server](howto-mfaserver-deploy.md#download-the-mfa-server) naar de meest recente versie van het installatieprogramma voor Azure MFA-Server.
2. Maak een back-up van het bestand met MFA-Server zich bevindt in C:\Program Files\Multi-Factor Authentication Server\Data\PhoneFactor.pfdata (ervan uitgaande dat de locatie is standaard geïnstalleerd) op de master MFA-Server.
3. Als u meerdere servers voor maximale beschikbaarheid uitvoert, wijzigt u de clientsystemen die worden geverifieerd bij de MFA-Server zodat ze stoppen met het verzenden van verkeer naar de servers die een upgrade uitvoert. Als u een load balancer, een onderliggende MFA-Server verwijderen uit de load balancer, de upgrade uit te voeren, en voeg de server weer in de farm.
4. Voer het nieuwe installatieprogramma op elke MFA-Server. Ondergeschikte servers eerst upgraden omdat ze de oude gegevensbestand gerepliceerd door het model kunnen lezen.

   > [!NOTE]
   > Bij het upgraden van een server moet deze worden verwijderd van de functie of verkeer delen met andere MFA-Servers.
   >
   > U hoeft niet te verwijderen van uw huidige MFA-Server voordat u het installatieprogramma uitvoert. Het installatieprogramma voert een in-place upgrade. Het installatiepad wordt opgehaald uit het register van de vorige installatie, zodat het wordt geïnstalleerd op dezelfde locatie (bijvoorbeeld C:\Program Files\Multi-Factor Authentication Server).
  
5. Als u wordt gevraagd om een Microsoft Visual C++ 2015 Redistributable updatepakket te installeren, de prompt te accepteren. Zowel de x86 x64-versies van het pakket worden geïnstalleerd.
6. Als u de webservice-SDK gebruikt, wordt u gevraagd voor het installeren van de nieuwe webservice-SDK. Wanneer u de nieuwe webservice-SDK installeert, zorg ervoor dat de naam van de virtuele map overeenkomt met de eerder geïnstalleerde virtuele map (bijvoorbeeld MultiFactorAuthWebServiceSdk).
7. Herhaal de stappen op alle ondergeschikte servers. Promoveer een van de onderliggende niveaus van het nieuwe model worden vervolgens de oude hoofdsleutel-server bijwerken.

## <a name="upgrade-the-user-portal"></a>Upgrade van de Gebruikersportal

Voltooi de upgrade van uw MFA-Servers voordat u deze sectie worden verplaatst.

1. Maak een back-up van het web.config-bestand dat zich in de virtuele map van de Gebruikersportal-installatielocatie (bijvoorbeeld C:\inetpub\wwwroot\MultiFactorAuth). Als er wijzigingen zijn aangebracht in het standaardthema, moet u een back-up van de map App_Themes\Default ook. Het is beter om een kopie maken van de standaardmap en maak een nieuwe thema dan als u wilt het standaard-thema wijzigen.
2. Als de Gebruikersportal op dezelfde server als de andere onderdelen van de MFA-Server wordt uitgevoerd, wordt de installatie van MFA-Server vraagt u om bij te werken van de Gebruikersportal. De prompt te accepteren en installeren van de update van de Gebruikersportal. Controleer of de naam van de virtuele map overeenkomt met de eerder geïnstalleerde virtuele map (bijvoorbeeld MultiFactorAuth).
3. Als de Gebruikersportal op een eigen server, Kopieer het bestand MultiFactorAuthenticationUserPortalSetup64.msi van de locatie voor de installatie van een van de MFA-Servers en plaatsen op de webserver van de Gebruikersportal. Voer het installatieprogramma.

   Als er een fout optreedt waarin 'Microsoft Visual C++ 2015 Redistributable Update 1 of hoger is vereist,' downloaden en installeren van de meest recente updatepakket van het [Microsoft Download Center](https://www.microsoft.com/download/). Installeer zowel de x86 x64-versies.

4. Nadat de bijgewerkte software van de Gebruikersportal is geïnstalleerd, vergelijkt de web.config back-up die u in stap 1 met het nieuwe bestand web.config hebt gemaakt. Als er zijn geen nieuwe in het nieuwe bestand web.config kenmerken, kopieert u uw back-up web.config in de virtuele map wilt overschrijven van het nieuwe certificaat. Er is een andere optie kopiëren/plakken de appSettings-waarden en de Web Service SDK-URL van het back-upbestand in het nieuwe bestand web.config.

Als u de Gebruikersportal op meerdere servers hebt, kunt u de installatie voor al deze herhalen.

## <a name="upgrade-the-mobile-app-web-service"></a>Upgrade van de webservice voor mobiele App

> [!NOTE]
> Bij een upgrade van een versie van Azure MFA Server die ouder is dan 8.0 naar 8.0 +, kan de webservice voor mobiele apps worden verwijderd na de upgrade

## <a name="upgrade-the-ad-fs-adapters"></a>Upgrade van de AD FS-Adapters

De upgrade van uw MFA-Servers en de Gebruikersportal voltooien voordat u deze sectie worden verplaatst.

### <a name="if-mfa-runs-on-different-servers-than-ad-fs"></a>Als MFA wordt uitgevoerd op verschillende servers dan AD FS

Deze instructies zijn alleen van toepassing als u multi-factor Authentication-Server afzonderlijk van uw AD FS-servers uitvoert. Als beide services worden uitgevoerd op de dezelfde servers, kunt u deze sectie overslaan en gaat u naar de installatiestappen uit. 

1. Sla een kopie van het bestand MultiFactorAuthenticationAdfsAdapter.config die is geregistreerd in AD FS of exporteren van de configuratie met behulp van de volgende PowerShell-opdracht: `Export-AdfsAuthenticationProviderConfigurationData -Name [adapter name] -FilePath [path to config file]`. De naam van de netwerkadapter is "WindowsAzureMultiFactorAuthentication" of "AzureMfaServerAuthentication", afhankelijk van de versie die eerder is geïnstalleerd.
2. Kopieer de volgende bestanden uit de MFA-Server-installatielocatie naar de AD FS-servers:

   * MultiFactorAuthenticationAdfsAdapterSetup64.msi
   * Register-MultiFactorAuthenticationAdfsAdapter.ps1
   * Unregister-MultiFactorAuthenticationAdfsAdapter.ps1
   * MultiFactorAuthenticationAdfsAdapter.config

3. Het bewerken van het script Register-MultiFactorAuthenticationAdfsAdapter.ps1 door toe te voegen `-ConfigurationFilePath [path]` aan het einde van de `Register-AdfsAuthenticationProvider` opdracht. Vervang *[pad]* met het volledige pad naar de MultiFactorAuthenticationAdfsAdapter.config of het configuratiebestand hebt geëxporteerd in de vorige stap.

   Controleer de kenmerken in de nieuwe MultiFactorAuthenticationAdfsAdapter.config om te zien als ze overeenkomen met de oude config-bestand. Als de kenmerken die zijn toegevoegd of verwijderd in de nieuwe versie, kopieert u de kenmerkwaarden van de oude configuratiebestand naar de nieuwe of wijzigt u de oude configuratiebestand zodat deze overeenkomt met.

### <a name="install-new-ad-fs-adapters"></a>Nieuwe AD FS-adapters installeren

> [!IMPORTANT]
> Moeten uw gebruikers niet om uit te voeren van verificatie in twee stappen tijdens stap 3-8 van deze sectie. Als u AD FS geconfigureerd in meerdere clusters hebt, kunt u verwijderen, bijwerken en herstellen van ieder cluster in de farm onafhankelijk van de andere clusters om uitvaltijd te voorkomen.

1. Verwijder enkele AD FS-servers uit de farm. Deze servers bijwerken terwijl de andere nog steeds worden uitgevoerd.
2. De nieuwe AD FS-adapter installeren op elke server die is verwijderd uit de AD FS-farm. Als de MFA-Server op elke AD FS-server is geïnstalleerd, kunt u bijwerken via de MFA-Server-beheerder UX. Anders werken door te voeren MultiFactorAuthenticationAdfsAdapterSetup64.msi.

   Als er een fout optreedt waarin 'Microsoft Visual C++ 2015 Redistributable Update 1 of hoger is vereist,' downloaden en installeren van de meest recente updatepakket van het [Microsoft Download Center](https://www.microsoft.com/download/). Installeer zowel de x86 x64-versies.

3. Ga naar **AD FS** > **verificatiebeleid** > **globale multi-factor Authentication-beleid bewerken**. Schakel het selectievakje **WindowsAzureMultiFactorAuthentication** of **AzureMFAServerAuthentication** (afhankelijk van de huidige versie die is geïnstalleerd).

   Nadat deze stap voltooid is, is verificatie via MFA-Server niet beschikbaar in deze AD FS-cluster totdat u stap 8.

4. Registratie van de oudere versie van de AD FS-adapter verwijderen door het Unregister-MultiFactorAuthenticationAdfsAdapter.ps1 PowerShell-script is uitgevoerd. Zorg ervoor dat de *-naam* parameter ('WindowsAzureMultiFactorAuthentication' of 'AzureMFAServerAuthentication') overeenkomt met de naam die werd weergegeven in stap 3. Dit geldt voor alle servers in hetzelfde AD FS-cluster omdat er een centrale configuratie.
5. Registreer de nieuwe AD FS-adapter door het Register-MultiFactorAuthenticationAdfsAdapter.ps1 PowerShell-script is uitgevoerd. Dit geldt voor alle servers in hetzelfde AD FS-cluster omdat er een centrale configuratie.
6. Start de AD FS-service op elke server die is verwijderd uit de AD FS-farm.
7. De bijgewerkte servers terug naar de AD FS-farm toevoegen en verwijderen van de andere servers in de farm.
8. Ga naar **AD FS** > **verificatiebeleid** > **globale multi-factor Authentication-beleid bewerken**. Controleer **AzureMfaServerAuthentication**.
9. Herhaal stap 2 van de servers die zijn verwijderd uit de AD FS-farm bijwerken en opnieuw opstarten van de AD FS-service op die servers.
10. Deze servers toevoegen in de AD FS-farm.

## <a name="next-steps"></a>Volgende stappen

* Voorbeelden van ophalen [geavanceerde scenario's met Azure multi-factor Authentication en VPN's van derden](howto-mfaserver-nps-vpn.md)

* [MFA-Server met Windows Server Active Directory synchroniseren](howto-mfaserver-dir-ad.md)

* [Windows-verificatie configureren](howto-mfaserver-windows.md) voor uw toepassingen
