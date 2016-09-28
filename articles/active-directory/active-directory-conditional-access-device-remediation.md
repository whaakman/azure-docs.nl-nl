<properties
    pageTitle="Problemen oplossen: geen toegang vanaf uw apparaat | Microsoft Azure"
    description="Dit onderwerp helpt u bepalen welke stappen u kunt volgen om toegang te krijgen tot een bepaalde toepassing."
    services="active-directory"
    keywords="voorwaardelijke toegang op basis van een apparaat, apparaatregistratie, apparaatregistratie inschakelen, apparaatregistratie en MDM"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="markvi"/>



# Problemen oplossen: geen toegang vanaf uw apparaat

Een pagina over geweigerde toegang wordt weergegeven wanneer u een toepassing zoals SharePoint Online opent.  
Wat kunt u doen?

Deze gids helpt u bepalen welke stappen u kunt uitvoeren om toegang te krijgen tot de toepassing.



Welk apparaatplatform gebruikt uw apparaat?
Het antwoord op deze vraag bepaalt welke sectie u moet raadplegen:


-   Windows-apparaat
-   iOS-apparaat (iPhone of iPad)
-   Android-apparaat

## Toegang vanaf een Windows-apparaat

Als op het apparaat Windows 10, Windows 8.1, Windows 8.0, Windows 7, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2 wordt uitgevoerd, kiest u de oorzaak die op u van toepassing is door te kijken welke pagina werd weergegeven toen u de toepassing probeerde te openen.

### Apparaat is niet geregistreerd

Als uw apparaat niet is geregistreerd bij Azure Active Directory (Azure AD) en de toepassing is beveiligd met beleid op basis van apparaten, wordt mogelijk een pagina met de volgende inhoud weergegeven:

![Berichten over ontoegankelijke toepassingen voor niet-geregistreerde apparaten](./media/active-directory-conditional-access-device-remediation/01.png "Scenario")



Als het apparaat is toegevoegd aan een Active Directory-domein in uw organisatie, kunt u het volgende proberen:

1.  Zorg ervoor dat u bij Windows bent aangemeld met uw werkaccount (Active Directory-account).
2.  Maak via VPN of DirectAccess verbinding met uw bedrijfsnetwerk.
3.  Nadat u verbinding hebt gemaakt, vergrendelt u uw Windows-sessie door op de Windows-toets + L te drukken.
4.  Ontgrendel de Windows-sessie door de referenties voor uw werkaccount in te voeren.
5.  Wacht even en probeer dan opnieuw of u de toepassing kunt openen.
6.  Als dezelfde pagina wordt weergegeven, neemt u contact op met uw beheerder. Klik op de koppeling **Meer details** en geef de weergegeven details door.

Als uw apparaat niet is verbonden met een domein en Windows 10 op het apparaat wordt uitgevoerd, hebt u twee opties:

- Voer Azure AD Join uit.
- Voeg uw werk- of schoolaccount toe aan Windows.

Zie [Using Windows 10 devices in your workplace](active-directory-azureadjoin-windows10-devices.md) (Windows 10-apparaten gebruiken op uw werkplek) voor informatie over de verschillen tussen de twee.

Ga als volgt te werk als u Azure AD Join wilt uitvoeren (niet beschikbaar voor Windows Phone):

**Windows 10 Jubileumupdate**

1.  Start de app **Instellingen**.
2.  Klik op **Accounts** > **Toegang via werk of school**.
3.  Klik op **Verbinding maken**.
4.  Klik onder aan de pagina op **Dit apparaat toevoegen aan Azure AD**.
5.  Verifieer uzelf bij uw organisatie, gebruik zo nodig meervoudige verificatie en voer alle stappen uit.
6.  Meld u af en meld u weer aan met uw werkaccount.
7.  Probeer opnieuw toegang te krijgen tot de toepassing.




**Windows 10-update van november 2015**


1.  Start de app **Instellingen**.
2.  Klik op **Systeem** > **Info**.
3.  Klik op **Lid worden van Azure AD**.
4.  Verifieer uzelf bij uw organisatie, gebruik zo nodig meervoudige verificatie en voer alle stappen uit.
5.  Meld u af en meld u weer aan met uw werkaccount (Azure AD-account).
6.  Probeer opnieuw toegang te krijgen tot de toepassing.

Ga als volgt te werk om uw werk- of schoolaccount toe te voegen aan Windows:

**Windows 10 Jubileumupdate**

1.  Start de app **Instellingen**.
2.  Klik op **Accounts** > **Toegang via werk of school**.
3.  Klik op **Verbinden**.
4.  Verifieer uzelf bij uw organisatie, gebruik zo nodig meervoudige verificatie en voer alle stappen uit.
5.  Probeer opnieuw toegang te krijgen tot de toepassing.


**Windows 10-update van november 2015**

1.  Start de app **Instellingen**.
2.  Klik op **Accounts** > **Uw accounts**.
3.  Klik op **Werk- of schoolaccount toevoegen**.
4.  Verifieer uzelf bij uw organisatie, gebruik zo nodig meervoudige verificatie en voer alle stappen uit.
5.  Probeer opnieuw toegang te krijgen tot de toepassing.

Als uw apparaat niet in een domein is opgenomen en Windows 8.1 op het apparaat wordt uitgevoerd, kunt u Workplace Join uitvoeren en het apparaat als volgt inschrijven bij Microsoft Intune:

1.  Open **Pc-instellingen**.
2.  Klik op **Netwerk** > **Werkplek**.
3.  Klik op **Deelnemen**.
4.  Verifieer uzelf bij uw organisatie, gebruik zo nodig meervoudige verificatie en voer alle stappen uit.
5.  Klik op **Inschakelen**.
6.  Wachten tot het proces is voltooid.
7.  Probeer opnieuw toegang te krijgen tot de toepassing.


## Niet-ondersteunde browser

Als u de toepassing probeert te openen vanuit een van de volgende browsers, wordt er een pagina weergegeven zoals hierboven:

- Chrome, Firefox of een andere browser die niet Microsoft Edge of Microsoft Internet Explorer in Windows 10 of Windows Server 2016 is.
- Firefox in Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2.

![Berichten over ontoegankelijke toepassingen voor niet-ondersteunde browsers](./media/active-directory-conditional-access-device-remediation/02.png "Scenario")


De enige oplossing is een browser te gebruiken die de toepassing ondersteunt voor uw apparaatplatform.

## Toegang vanaf een iOS-apparaat
Binnenkort vindt u hier instructies voor iPhone en iPad.

## Toegang vanaf een Android-apparaat
Binnenkort vindt u hier instructies voor Android-telefoons en -tablets.

## Volgende stappen

[Voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access.md)



<!--HONumber=Sep16_HO3-->


