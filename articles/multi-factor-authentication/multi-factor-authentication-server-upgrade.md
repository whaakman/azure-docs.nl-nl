---
title: Upgrade uitvoeren voor Azure MFA-Server | Microsoft Docs
description: Stappen en richtlijnen voor het upgraden van de Azure multi-factor Authentication-Server naar een nieuwere versie.
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 50bb8ac3-5559-4d8b-a96a-799a74978b14
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 6e4e09f8539aad56f92ad9137f4a6b9eb0d82370
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="upgrade-to-the-latest-azure-multi-factor-authentication-server"></a>Een upgrade uitvoert naar de nieuwste Azure multi-factor Authentication-Server

Dit artikel begeleidt u bij het verwerken van de upgrade van Azure multi-factor Authentication (MFA) Server 6.0 of hoger. Als u een oude versie van de PhoneFactor Agent bijwerken moet, raadpleegt u [de PhoneFactor Agent bijwerken naar Azure multi-factor Authentication-Server](multi-factor-authentication-get-started-server-upgrade.md).

Als u een van versie 6.x of naar v7.x oudere of nieuwere upgrade uitvoert, worden alle onderdelen van .NET 2.0 gewijzigd naar .NET 4.5. Alle onderdelen moeten ook Microsoft Visual C++ Redistributable Update 2015 1 of hoger. Als ze niet al zijn geïnstalleerd, installeert het installatieprogramma MFA-Server de x86- en x64 versies van deze onderdelen. Als de Gebruikersportal en de webservice voor mobiele App op afzonderlijke servers uitvoeren, moet u die pakketten installeren vóór de upgrade van deze onderdelen. U kunt zoeken naar de nieuwste update van Microsoft Visual C++ 2015 Redistributable op de [Microsoft Download Center](https://www.microsoft.com/en-us/download/). 

## <a name="install-the-latest-version-of-azure-mfa-server"></a>Installeer de nieuwste versie van Azure MFA-Server

1. Volg de instructies in [downloaden van de Azure multi-factor Authentication-Server](multi-factor-authentication-get-started-server.md#download-the-azure-multi-factor-authentication-server) ophalen van de nieuwste versie van de Azure MFA-Server.
2. Maak een back-up van het gegevensbestand van MFA-Server zich bevindt op C:\Program Files\Multi-Factor Authentication Server\Data\PhoneFactor.pfdata (ervan uitgaande dat de locatie is standaard geïnstalleerd) op de master MFA-Server.
3. Als u meerdere servers voor hoge beschikbaarheid uitvoeren, wijzigt u de client-systemen om de MFA-Server te verifiëren zodat ze niet meer verkeer kunnen verzenden naar de servers die een upgrade uitvoert. Als u een load balancer, een MFA-Server verwijderen uit de load balancer, de upgrade en voeg vervolgens de server weer in de farm.
4. Het nieuwe installatieprogramma uitvoeren op elke MFA-Server. Ondergeschikte servers eerst upgraden omdat de oude gerepliceerd door de master-gegevensbestand kan worden gelezen. 

  U hoeft niet te verwijderen van uw huidige MFA-Server voordat u het installatieprogramma uitvoert. Het installatieprogramma voert een in-place upgrade. Het installatiepad wordt opgehaald uit het register van de vorige installatie, zodat deze wordt geïnstalleerd op dezelfde locatie (bijvoorbeeld C:\Program Files\Multi-Factor Authentication Server). 
  
5. Als u wordt gevraagd om een Microsoft Visual C++ 2015 Redistributable updatepakket te installeren, moet u de prompt accepteren. X86- en x64 versies van het pakket worden geïnstalleerd.
5. Als u de Web Service SDK gebruikt, wordt u gevraagd de nieuwe webservice-SDK installeren. Wanneer u de nieuwe webservice-SDK installeert, zorg ervoor dat de naam van de virtuele map overeenkomt met de eerder geïnstalleerde virtuele map (bijvoorbeeld MultiFactorAuthWebServiceSdk).
6. Herhaal de stappen op alle onderliggende servers. Promoveer een van de onderliggende niveaus van het nieuwe model worden vervolgens de oude hoofdsleutel server bijwerken. 

## <a name="upgrade-the-user-portal"></a>Upgrade de Gebruikersportal

1. Maak een back-up van het bestand web.config dat zich in de virtuele map van de Gebruikersportal-installatielocatie (bijvoorbeeld C:\inetpub\wwwroot\MultiFactorAuth). Als er wijzigingen zijn aangebracht in het standaardthema, moet u een back-up van de map App_Themes\Default ook. Het is beter voor het maken van een kopie van de standaardmap en een nieuw thema dan het standaardthema wijzigt om te maken.
2. Als de Gebruikersportal op dezelfde server als de andere onderdelen van de MFA-Server wordt uitgevoerd, vraagt de MFA-Server-installatie bij te werken van de Gebruikersportal. De prompt accepteren en installeren van de Gebruikersportal-update. Controleert of de naam van de virtuele map overeenkomt met de eerder geïnstalleerde virtuele map (bijvoorbeeld MultiFactorAuth).
3. Als de Gebruikersportal op een eigen server, het bestand MultiFactorAuthenticationUserPortalSetup64.msi kopiëren vanuit de installatielocatie van een van de MFA-Servers en plaatsen op de webserver van de Gebruikersportal. Het installatieprogramma uitvoert. 

  Als er een fout optreedt vermelden 'Microsoft Visual C++ Redistributable Update 2015 1 of hoger is vereist,' download en installeer de meest recente updatepakket van het [Microsoft Download Center](https://www.microsoft.com/download/). De versies x86- en x64 installeren.

4. Nadat de bijgewerkte Gebruikersportal-software is geïnstalleerd, vergelijkt de web.config back-up die u hebt aangebracht in stap 1 met het nieuwe bestand web.config. Als geen nieuwe kenmerken in het nieuwe bestand web.config aanwezig is, kopieert u uw back-up web.config in de virtuele map naar de nieuwe database overschrijven. Een andere optie is om te kopiëren en plakken de appSettings-waarden en de Web Service SDK-URL van de back-upbestand in het nieuwe bestand web.config.

Als u de Gebruikersportal op meerdere servers hebt, kunt u de installatie voor al deze herhalen. 


## <a name="upgrade-the-mobile-app-web-service"></a>Upgrade van de mobiele Web-App Service

1. Maak een back-up van het bestand web.config dat zich in de virtuele map van de webservice voor mobiele App-installatielocatie (bijvoorbeeld C:\inetpub\wwwroot\app of C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService).
2. Kopieer het bestand MultiFactorAuthenticationMobileAppWebServiceSetup64.msi van de locatie van de MFA-Servers is geïnstalleerd en plaatsen op de webserver voor registratie van mobiele App.
3. Het installatieprogramma uitvoert. 

  Als een fout met de mededeling optreedt dat Microsoft Visual C++ Redistributable Update 2015 1 of hoger vereist is, downloadt en installeert u het meest recente updatepakket van het [Microsoft Download Center](https://www.microsoft.com/download/). De versies x86- en x64 installeren.

4. Nadat de bijgewerkte software van de webservice voor mobiele App is geïnstalleerd, vergelijk het web.config-bestand dat een back-in stap 1 met het nieuwe bestand web.config. Als er geen nieuwe kenmerken in het nieuwe bestand web.config aanwezig is, kunt u uw opgeslagen web.config kopiëren naar de virtuele map en de nieuwe database overschrijven. Een andere optie is om te kopiëren en plakken de appSettings-waarden en de Web Service SDK-URL van de back-upbestand in het nieuwe bestand web.config.

Als u de webservice voor mobiele App op meerdere servers hebt, kunt u de installatie voor al deze herhalen. 

## <a name="upgrade-the-ad-fs-adapters"></a>Upgrade van de AD FS-Adapters


### <a name="if-mfa-runs-on-different-servers-than-ad-fs"></a>Als MFA wordt uitgevoerd op andere servers dan de AD FS

Deze instructies zijn alleen van toepassing als u multi-factor Authentication-Server los van uw AD FS-servers uitvoeren. Als beide services worden uitgevoerd op dezelfde servers, kunt u deze sectie overslaan en gaat u naar de installatiestappen. 

1. Sla een kopie van het bestand MultiFactorAuthenticationAdfsAdapter.config die is geregistreerd in AD FS of de configuratie van de volgende PowerShell-opdracht exporteren: `Export-AdfsAuthenticationProviderConfigurationData -Name [adapter name] -FilePath [path to config file]`. De naam van de netwerkadapter is 'WindowsAzureMultiFactorAuthentication' of 'AzureMfaServerAuthentication' afhankelijk van de eerder geïnstalleerde versie.
2. Kopieer de volgende bestanden vanaf de MFA-Server-installatielocatie naar de AD FS-servers:

  - MultiFactorAuthenticationAdfsAdapterSetup64.msi
  - Register-MultiFactorAuthenticationAdfsAdapter.ps1
  - Unregister-MultiFactorAuthenticationAdfsAdapter.ps1
  - MultiFactorAuthenticationAdfsAdapter.config

3. Het bewerken van het script Register-MultiFactorAuthenticationAdfsAdapter.ps1 door toe te voegen `-ConfigurationFilePath [path]` aan het einde van de `Register-AdfsAuthenticationProvider` opdracht. Vervang *[pad]* met het volledige pad naar de MultiFactorAuthenticationAdfsAdapter.config bestand of het configuratiebestand hebt geëxporteerd in de vorige stap. 

  Controleer de kenmerken in de nieuwe MultiFactorAuthenticationAdfsAdapter.config om te zien als ze overeenkomen met de oude configuratiebestand. Als de kenmerken die zijn toegevoegd of verwijderd in de nieuwe versie, kopieert u de kenmerkwaarden weer uit het oude configuratiebestand naar de nieuwe of wijzig het oude configuratiebestand moet worden gezocht.

### <a name="install-new-ad-fs-adapters"></a>Nieuwe AD FS-adapters installeren

> [!IMPORTANT] 
> Uw gebruikers geen moet verificatie in twee stappen tijdens stap 3-8 van deze sectie uitvoeren. Als u AD FS geconfigureerd in meerdere clusters hebt, kunt u verwijderen, bijwerken en herstellen van ieder cluster in de farm onafhankelijk van de andere clusters on te uitvaltijd te voorkomen.

1. Sommige AD FS-servers verwijderen van de farm. Deze servers bijwerken terwijl de andere worden nog steeds uitgevoerd.
2. De nieuwe AD FS-adapter installeren op elke server die is verwijderd uit de AD FS-farm. Als de MFA-Server op elke AD FS-server is geïnstalleerd, kunt u bijwerken via de MFA-Server-beheerder-UX Anders wordt bijgewerkt door het uitvoeren van de bestanden MultiFactorAuthenticationAdfsAdapterSetup64.msi. 

  Als er een fout optreedt vermelden 'Microsoft Visual C++ Redistributable Update 2015 1 of hoger is vereist,' download en installeer de meest recente updatepakket van het [Microsoft Download Center](https://www.microsoft.com/download/). De versies x86- en x64 installeren.

3. Ga naar **AD FS** > **verificatiebeleid** > **globale MultiFactor-verificatie-beleid bewerken**. Schakel het selectievakje **WindowsAzureMultiFactorAuthentication** of **AzureMFAServerAuthentication** (afhankelijk van de huidige versie is geïnstalleerd). 

  Nadat deze stap voltooid is, is verificatie in twee stappen via MFA-Server niet beschikbaar in dit cluster AD FS totdat u stap 8.

4. Hef de registratie van de oudere versie van de AD FS-adapter de Unregister-MultiFactorAuthenticationAdfsAdapter.ps1 PowerShell-script uit te voeren. Zorg ervoor dat de *-naam* parameter ('WindowsAzureMultiFactorAuthentication' of 'AzureMFAServerAuthentication') die overeenkomt met de naam wordt weergegeven in stap 3. Dit geldt voor alle servers in hetzelfde AD FS-cluster omdat er een centraal configuratie.
5. Registreer de nieuwe AD FS-adapter de Register-MultiFactorAuthenticationAdfsAdapter.ps1 PowerShell-script uit te voeren. Dit geldt voor alle servers in hetzelfde AD FS-cluster omdat er een centraal configuratie.
6. Start de AD FS-service op elke server die is verwijderd uit de AD FS-farm.
7. De bijgewerkte servers toevoegen terug naar de AD FS-farm en verwijder de andere servers van de farm.
8. Ga naar **AD FS** > **verificatiebeleid** > **globale MultiFactor-verificatie-beleid bewerken**. Controleer **AzureMfaServerAuthentication**.
9. Herhaal stap 2 van de servers die nu wordt verwijderd uit de AD FS-farm bijwerken en start de AD FS-service op die servers opnieuw.
10. Deze servers weer in de AD FS-farm toevoegen.

## <a name="next-steps"></a>Volgende stappen

- Voorbeelden van ophalen [geavanceerde scenario's met Azure multi-factor Authentication en VPN's van derden](multi-factor-authentication-advanced-vpn-configurations.md)

- [MFA-Server met Windows Server Active Directory synchroniseren](multi-factor-authentication-get-started-server-dirint.md)

- [Windows-verificatie configureren](multi-factor-authentication-get-started-server-windows.md) voor uw toepassingen
