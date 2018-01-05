---
title: VPN integreren met Azure MFA met behulp van de extensie Network Policy Server | Microsoft Docs
description: Dit artikel wordt de VPN-infrastructuur integreren met Azure MFA met behulp van de Network Policy Server-uitbreiding voor Microsoft Azure.
services: active-directory
keywords: Azure MFA integreren van VPN-, Azure Active Directory, uitbreiding van Network Policy Server
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.openlocfilehash: 1141245739f86a482bb0b5f550fd3b89d1213ce1
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="integrate-your-vpn-infrastructure-with-azure-mfa-by-using-the-network-policy-server-extension-for-azure"></a>Uw VPN-infrastructuur integreren met Azure MFA met behulp van de Network Policy Server-extensie voor Azure

## <a name="overview"></a>Overzicht

De Network Policy Server (NPS)-extensie voor Azure, waarmee organisaties beschermen Remote Authentication Dial-In User Service (RADIUS)-clientauthenticatie met behulp van cloud-gebaseerde [Azure multi-factor Authentication (MFA)](multi-factor-authentication-get-started-server-rdg.md), waarmee u verificatie in twee stappen.

Dit artikel bevat instructies voor het NPS-infrastructuur integreren met MFA met behulp van de NPS-extensie voor Azure. Dit proces kunnen verificatie van de beveiligde in twee stappen voor gebruikers die verbinding met uw netwerk maakt met behulp van een VPN. 

Network Policy and Access Services krijgt organisaties de mogelijkheid om:

* Toewijzen van een centrale locatie voor het beheer en controle van netwerkaanvragen om op te geven:

    * Wie verbinding kan maken 
    
    * Welke momenten van de dag verbindingen zijn toegestaan. 
    
    * De duur van verbindingen
    
    * Het niveau van beveiliging die clients gebruiken moeten voor verbinding

    In plaats van beleidsregels op elke VPN- of extern bureaublad-Gateway-server opgeeft, doen nadat ze zich in een centrale locatie. De RADIUS-protocol wordt gebruikt voor gecentraliseerde verificatie, autorisatie en Accounting (AAA). 

* Instellen en afdwingen van statusbeleid voor Network Access Protection (NAP) client om te bepalen of apparaten onbeperkte of beperkte toegang krijgen tot netwerkbronnen.

* Geef een manier om af te dwingen, verificatie en autorisatie voor toegang op 802. 1 x-compatibele draadloze toegangspunten en Ethernet-switches.   
Zie voor meer informatie [Network Policy Server](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top). 

Voor betere beveiliging en een hoge mate van naleving bieden, kunnen organisaties NPS geïntegreerd met Azure multi-factor Authentication om ervoor te zorgen dat gebruikers verificatie in twee stappen verbinding maken met de virtuele poort op de VPN-server gebruiken. Voor gebruikers toegang te krijgen, moeten ze bieden hun gebruikersnaam en wachtwoord combinatie en andere informatie die ze beheren. Deze informatie moet worden vertrouwd en niet gemakkelijk worden gedupliceerd. Het kan betekenen dat het nummer van een mobiele telefoon, een vast nummer of een toepassing op een mobiel apparaat.

Voorafgaand aan de beschikbaarheid van de NPS-extensie voor Azure-klanten die willen implementeren verificatie in twee stappen voor geïntegreerde NPS en MFA omgevingen moest configureren en onderhouden van een afzonderlijke MFA-server in een on-premises omgeving. Dit type verificatie wordt aangeboden door extern bureaublad-Gateway en Azure multi-factor Authentication-Server met behulp van RADIUS.

Met de NPS-extensie voor Azure, kunnen organisaties RADIUS-clientverificatie beveiligen door het implementeren van een on-premises op basis van MFA oplossing of een MFA-cloud-gebaseerde oplossing.
 
## <a name="authentication-flow"></a>Verificatiestroom
Wanneer gebruikers verbinding met een virtuele poort op een VPN-server maken, moeten ze eerst worden geverifieerd met behulp van verschillende protocollen. De protocollen voor toestaan het gebruik van een combinatie van gebruikersnaam en wachtwoord en verificatiemethoden op basis van certificaten. 

Naast de verificatie en hun identiteit te verifiëren, moeten gebruikers de juiste machtigingen inbellen hebben. In implementaties van eenvoudige, inbellen machtigingen die toegang geven rechtstreeks op de Active Directory-gebruikersobjecten ingesteld. 

![Gebruikerseigenschappen](./media/nps-extension-vpn/image1.png)

In de eenvoudige implementaties elke VPN-server verleent of weigert op basis van beleidsregels die zijn gedefinieerd op elke lokale VPN-server.

In implementaties van grotere en meer schaalbare, worden de beleidsregels die VPN-toegang verlenen of weigeren gecentraliseerde op RADIUS-servers. In dergelijke gevallen wordt de VPN-server fungeert als een access-server (RADIUS-client) die verbindingsaanvragen en account berichten naar een RADIUS-server stuurt. Maak verbinding met de virtuele poort op de VPN-server door gebruikers moeten worden geverifieerd en voldoen aan de voorwaarden die centraal zijn gedefinieerd op RADIUS-servers. 

Wanneer de NPS-extensie voor Azure is geïntegreerd met de NPS, krijgt een geslaagde verificatiestroom, als volgt:

1. De VPN-server ontvangt een verificatieaanvraag van een VPN-gebruiker met de gebruikersnaam en het wachtwoord voor het verbinden met een resource, zoals een extern bureaublad-sessie. 

2. Fungeert als een RADIUS-client, de VPN-server de aanvraag converteert naar een RADIUS *aanvraag voor toegang tot* bericht en verzendt het (met een versleutelde wachtwoord) naar de RADIUS-server waarop de NPS-uitbreiding is geïnstalleerd. 

3. De combinatie van gebruikersnaam en wachtwoord wordt geverifieerd in Active Directory. Als de gebruikersnaam of het wachtwoord onjuist is, stuurt de RADIUS-Server een *Access Reject* bericht. 

4. Indien wordt voldaan aan alle voorwaarden, zoals opgegeven in de NPS-verbindingsaanvraag en het beleid voor netwerk (bijvoorbeeld de tijd van de dag of groep lidmaatschap beperkingen), de NPS-extensie activeert een aanvraag voor de secundaire verificatie met Azure multi-factor Authentication. 

5. Azure multi-factor Authentication communiceert met Azure Active Directory, haalt de details van de gebruiker en de secundaire verificatie uitvoert met behulp van de methode die geconfigureerd door de gebruiker (cel telefoongesprek, tekstbericht of mobiele app). 

6. Als de MFA-controle geslaagd is, communiceert de Azure multi-factor Authentication het resultaat naar de NPS-extensie.

7. Nadat de verbindingspoging is geverifieerd en geautoriseerd, de NPS waarbij de extensie is geïnstalleerd een RADIUS stuurt *Access Accept* bericht naar de VPN-server (RADIUS-client).

8. De gebruiker krijgt toegang tot de virtuele poort op de VPN-server en er wordt een versleutelde VPN-tunnel.

## <a name="prerequisites"></a>Vereisten
Deze sectie beschrijft de vereisten die moeten worden voltooid voordat u MFA met de extern bureaublad-Gateway integreren kunt. Voordat u begint, moet u de volgende vereisten hebben voldaan:

* VPN-infrastructuur
* Network Policy and Access Services-rol
* Azure multi-factor Authentication-licentie
* Windows Server-software
* Bibliotheken
* Azure Active Directory (Azure AD) gesynchroniseerd met de lokale Active Directory 
* GUID-ID van Azure Active Directory

### <a name="vpn-infrastructure"></a>VPN-infrastructuur
In dit artikel wordt ervan uitgegaan dat u hebt een werkende VPN-infrastructuur die gebruikmaakt van Microsoft Windows Server 2016 en de VPN-server is momenteel niet geconfigureerd voor verbindingsaanvragen wilt doorsturen naar een RADIUS-server. In het artikel configureert u de VPN-infrastructuur voor het gebruik van een centrale RADIUS-server.

Als u een werkende VPN-infrastructuur niet geïmplementeerd hebt, kunt u snel maken volgt u de instructies in talloze zelfstudies van de VPN-instellingen die u kunt vinden op de Microsoft en de sites van derden. 
            
### <a name="the-network-policy-and-access-services-role"></a>De functie Network Policy and Access Services

Network Policy and Access Services biedt de RADIUS-server en client-functionaliteit. In dit artikel wordt ervan uitgegaan dat u de functie Network Policy and Access Services op een lidserver of domeincontroller hebt geïnstalleerd in uw omgeving. In deze handleiding configureert u RADIUS voor een VPN-configuratie. De functie Network Policy and Access Services installeren op een server *dan* de VPN-server.

Voor informatie over het installeren van de functie Network Policy and Access Services service van Windows Server 2012 of later, Zie [installeren van een NAP-statusbeleidsserver](https://technet.microsoft.com/library/dd296890.aspx). NAP is afgeschaft in Windows Server 2016. Zie voor een beschrijving van best practices voor NPS, met inbegrip van de aanbeveling om NPS installeert op een domeincontroller [aanbevolen procedures voor NPS](https://technet.microsoft.com/library/cc771746).

### <a name="azure-mfa-license"></a>Azure MFA-licentie

Een licentie is vereist voor Azure multi-factor Authentication en is beschikbaar in een Azure AD Premium, Enterprise Mobility + Security of een zelfstandige licentie multi-factor Authentication. Op basis van verbruik licenties voor Azure MFA zoals per gebruiker of per authenticatie licenties zijn niet compatibel met de NPS-extensie. Zie voor meer informatie [het ophalen van Azure multi-factor Authentication](multi-factor-authentication-versions-plans.md). Voor testdoeleinden kunt u een proefabonnement.

### <a name="windows-server-software"></a>Windows Server-software

De NPS-uitbreiding vereist Windows Server 2008 R2 SP1 of hoger, met de functie Network Policy and Access Services zijn geïnstalleerd. De stappen in deze handleiding zijn uitgevoerd met Windows Server 2016.

### <a name="libraries"></a>Bibliotheken

De volgende bibliotheken worden automatisch geïnstalleerd met de NPS-extensie:

-   [Visual C++ Redistributable Packages for Visual Studio 2013 (X64)](https://www.microsoft.com/download/details.aspx?id=40784)
-   [Microsoft Azure Active Directory-Module voor Windows PowerShell versie 1.1.166.0](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

Als de Microsoft Azure Active Directory PowerShell-Module niet aanwezig is, wordt deze geïnstalleerd met een configuratiescript dat u als onderdeel van het installatieproces uitvoert. Er is niet nodig voor het installeren van de module tevoren als deze nog niet is geïnstalleerd.

### <a name="azure-active-directory-synced-with-on-premises-active-directory"></a>Azure Active Directory worden gesynchroniseerd met de lokale Active Directory 

Voor het gebruik van de NPS-extensie, zijn on-premises gebruikers gesynchroniseerd met Azure Active Directory en ingeschakeld voor MFA. Deze handleiding wordt ervan uitgegaan dat de on-premises gebruikers zijn gesynchroniseerd met Azure Active Directory via Azure AD Connect. Hieronder vindt u instructies voor het inschakelen van gebruikers voor MFA.

Zie voor meer informatie over Azure AD Connect [uw on-premises adreslijsten integreren met Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md). 

### <a name="azure-active-directory-guid-id"></a>GUID-ID van Azure Active Directory 

U moet weten de GUID van de Azure Active Directory voor het installeren van de NPS-extensie. Instructies voor het vinden van de GUID van de Azure Active Directory zijn beschikbaar in de volgende sectie.

## <a name="configure-radius-for-vpn-connections"></a>RADIUS voor VPN-verbindingen configureren

Als u de functie NPS hebt geïnstalleerd op een lidserver, moet u deze configureren om te verifiëren en autoriseren van de VPN-client die aanvragen VPN-verbindingen. 

Deze sectie wordt ervan uitgegaan dat u de functie Network Policy and Access Services hebt geïnstalleerd, maar hebben geen deze heeft geconfigureerd voor gebruik in uw infrastructuur.

> [!NOTE]
> Als u al een werkende VPN-server die gebruikmaakt van een gecentraliseerde RADIUS-server voor verificatie, kunt u deze sectie overslaan.
>

### <a name="register-server-in-active-directory"></a>Server registreren in Active Directory
De NPS-server een correcte werking in dit scenario moet worden geregistreerd in Active Directory.

1. Open Serverbeheer.

2. Selecteer in Serverbeheer **extra**, en selecteer vervolgens **Network Policy Server**. 

3. In de console Network Policy Server met de rechtermuisknop op **NPS (lokaal)**, en selecteer vervolgens **server registreren in Active Directory**. Selecteer **OK** twee keer.

    ![Network Policy Server](./media/nps-extension-vpn/image2.png)

4. Laat de console geopend voor de volgende procedure.

### <a name="use-wizard-to-configure-the-radius-server"></a>Gebruik de wizard voor het configureren van de RADIUS-server
U kunt een standaard (door de wizard gebaseerd) of geavanceerde configuratieoptie voor het configureren van de RADIUS-server. Deze sectie wordt ervan uitgegaan dat u de standaardconfiguratie op basis van een wizard-optie gebruikt.

1. Selecteer in de beheerconsole Network Policy Server **NPS (lokaal)**.

2. Onder **standaardconfiguratie**, selecteer **RADIUS-Server voor inbel- of VPN-verbindingen**, en selecteer vervolgens **configureren van VPN- of inbelverbindingen**.

    ![VPN configureren](./media/nps-extension-vpn/image3.png)

3. In de **Selecteer inbel- of virtuele particuliere netwerk verbindingen Type** venster Selecteer **VPN-verbindingen**, en selecteer vervolgens **volgende**.

    ![Virtueel particulier netwerk](./media/nps-extension-vpn/image4.png)

4. In de **Geef inbel- of VPN-Server** Selecteer **toevoegen**.

5. In de **nieuwe RADIUS-client** Geef een beschrijvende naam, voer de naam of IP-adres van de VPN-server en voer vervolgens een gedeelde geheime vraag. Controleer het gedeelde geheim wachtwoord lange en complexe. Het opnemen, omdat u hebt deze nodig in de volgende sectie.

    ![Nieuwe RADIUS-client](./media/nps-extension-vpn/image5.png)

6. Selecteer **OK**, en selecteer vervolgens **volgende**.

7. In de **verificatiemethoden configureren** venster, accepteer de standaardinstelling (**Microsoft Encrypted Authentication versie 2 [MS-CHAPv2])** of kies een andere optie en selecteer **volgende** .

    > [!NOTE]
    > Als u Extensible Authentication Protocol (EAP) configureert, moet u Microsoft Challenge Handshake Authentication Protocol (CHAPv2) of Protected Extensible Authentication Protocol (PEAP). Er zijn geen andere EAP wordt ondersteund.
 
8. In de **gebruikersgroepen opgeven** Selecteer **toevoegen**, en selecteer vervolgens een geschikte groep. Als er geen groep is, laat u de selectie leeg om toegang te verlenen voor alle gebruikers.

    ![Het venster gebruikersgroepen opgeven](./media/nps-extension-vpn/image7.png)

9. Selecteer **volgende**.

10. In de **IP-Filters opgeven** Selecteer **volgende**.

11. In de **versleutelingsinstellingen opgeven** venster de standaardinstellingen accepteren en selecteer vervolgens **volgende**.

    ![Het venster versleutelingsinstellingen opgeven](./media/nps-extension-vpn/image8.png)

12. In de **een Realm opgeven** naam van de realm leeg laat, accepteer de standaardinstelling en selecteer vervolgens **volgende**.

    ![Geef een naam van de Realm-venster](./media/nps-extension-vpn/image9.png)

13. In de **nieuwe voltooien inbel- of VPN-verbindingen en RADIUS-clients** Selecteer **voltooien**.

    ![Het venster 'voltooien nieuwe inbel- of de verbindingen van virtueel particulier netwerk en de RADIUS-clients'](./media/nps-extension-vpn/image10.png)

### <a name="verify-the-radius-configuration"></a>Controleer of de RADIUS-configuratie
Deze sectie beschrijft de configuratie die u hebt gemaakt met de wizard.

1. Vouw op de Network Policy Server in de console NPS (lokaal) **RADIUS-Clients**, en selecteer vervolgens **RADIUS-Clients**.

2. In het detailvenster met de rechtermuisknop op de RADIUS-client die u gemaakt en selecteer vervolgens **eigenschappen**. De eigenschappen van de RADIUS-client (de VPN-server) worden zoals hier wordt weergegeven:

    ![VPN-eigenschappen](./media/nps-extension-vpn/image11.png)

3. Selecteer **annuleren**.

4. Vouw op de Network Policy Server in de console NPS (lokaal) **beleid**, en selecteer vervolgens **beleid voor verbindingsaanvragen**. Het beleid voor VPN-verbindingen wordt weergegeven zoals in de volgende afbeelding:

    ![Verbindingsaanvragen](./media/nps-extension-vpn/image12.png)

5. Onder **beleid**, selecteer **netwerkbeleid**. Hier ziet u een beleid met virtuele particuliere netwerk (VPN)-verbindingen dat lijkt op het beleid wordt weergegeven in de volgende afbeelding:

    ![Netwerkbeleid](./media/nps-extension-vpn/image13.png)

## <a name="configure-your-vpn-server-to-use-radius-authentication"></a>De VPN-server voor RADIUS-verificatie configureren
In deze sectie configureert u de VPN-server voor RADIUS-verificatie. De instructies wordt ervan uitgegaan dat u een werkende configuratie van een VPN-server hebt, maar het gebruik van RADIUS-verificatie niet hebt geconfigureerd. Nadat u de VPN-server configureert, moet u controleren of uw configuratie werkt zoals verwacht.

> [!NOTE]
> Als u VPN-serverconfiguratie die gebruikmaakt van RADIUS-verificatie hebt, kunt u deze sectie overslaan.
>

### <a name="configure-authentication-provider"></a>Verificatieprovider configureren
1. Open Serverbeheer op de VPN-server.

2. Selecteer in Serverbeheer **extra**, en selecteer vervolgens **Routing and Remote Access**.

3. In de **Routing and Remote Access** venster met de rechtermuisknop op  **\<servernaam > (lokaal)**, en selecteer vervolgens **eigenschappen**.

    ![Het venster Routering en RAS](./media/nps-extension-vpn/image14.png)
 
4. In de  **\<servernaam > (lokaal) eigenschappen** Selecteer de **beveiliging** tabblad. 

5. Op de **beveiliging** tabblad onder **verificatieprovider**, selecteer **RADIUS-verificatie**, en selecteer vervolgens **configureren**.

    ![RADIUS-verificatie](./media/nps-extension-vpn/image15.png)
 
6. In de **RADIUS-verificatie** Selecteer **toevoegen**.

7. In de **RADIUS-Server toevoegen** venster de volgende handelingen uit:

    a. In de **servernaam** Voer de naam of IP-adres van de RADIUS-server die u hebt geconfigureerd in de vorige sectie.

    b. Voor de **gedeeld geheim**, selecteer **wijziging**, en voer vervolgens het gedeelde geheim wachtwoord dat u hebt gemaakt en u eerder hebt genoteerd.

    c. In de **Time-out (seconden)** Selecteer een waarde van **30** via **60**.  
    De time-outwaarde is nodig om voldoende tijd is voor het voltooien van de tweede verificatiefactor toestaan.
 
    ![Het venster van de RADIUS-Server toevoegen](./media/nps-extension-vpn/image16.png)
 
8. Selecteer **OK**.

### <a name="test-vpn-connectivity"></a>VPN-verbindingen testen
In deze sectie kunt bevestigen u dat de VPN-client is geverifieerd en gemachtigd door de RADIUS-server wanneer u probeert verbinding maken met de VPN-virtuele poort. De instructies wordt ervan uitgegaan dat u van Windows 10 als een VPN-client gebruikmaakt. 

> [!NOTE]
> Als u al een VPN-client om verbinding maken met de VPN-server en de instellingen hebt opgeslagen hebt geconfigureerd, kunt u de stappen voor het configureren en opslaan van een VPN-verbindingsobject overslaan.
>

1. Selecteer op de VPN-client-computer de **Start** knop en selecteer vervolgens de **instellingen** knop.

2. In de **Windows-instellingen** Selecteer **netwerk en Internet**.

3. Selecteer **VPN**.

4. Selecteer **een VPN-verbinding toevoegen**.

5. In de **een VPN-verbinding toevoegen** venster in de **VPN-provider** de optie **Windows (ingebouwd)**, vul de resterende velden, naar gelang van toepassing, en selecteer vervolgens **Opslaan**. 

    ![Het venster 'Een VPN-verbinding toevoegen'](./media/nps-extension-vpn/image17.png)
 
6. Ga naar **Configuratiescherm**, en selecteer vervolgens **Netwerkcentrum**.

7. Selecteer **Adapterinstellingen wijzigen**.

    ![Adapterinstellingen wijzigen](./media/nps-extension-vpn/image18.png)

8. Met de rechtermuisknop op de netwerkverbinding van de VPN- en selecteer vervolgens **eigenschappen**. 

    ![Eigenschappen van VPN-netwerk](./media/nps-extension-vpn/image19.png)

9. Selecteer in het venster van de VPN-eigenschappen, de **beveiliging** tabblad. 

10. Op de **beveiliging** tabblad, moet u zorgen dat alleen **Microsoft CHAP Version 2 (MS-CHAP v2)** is geselecteerd en selecteer vervolgens **OK**.

    ![De optie "Deze protocollen toestaan"](./media/nps-extension-vpn/image20.png)

11. Met de rechtermuisknop op de VPN-verbinding en selecteer vervolgens **Connect**.

12. In de **instellingen** Selecteer **Connect**.  
    Een geslaagde verbinding wordt weergegeven in het beveiligingslogboek op RADIUS-server als gebeurtenis-ID 6272, zoals hier wordt weergegeven:

    ![Het venster Eigenschappen van gebeurtenis](./media/nps-extension-vpn/image21.png)

## <a name="troubleshooting-radius"></a>Het oplossen van RADIUS

Wordt ervan uitgegaan dat uw VPN-configuratie werkte voordat u de VPN-server geconfigureerd voor het gebruik van een gecentraliseerde RADIUS-server voor verificatie en autorisatie. Als de configuratie is werkt, is het waarschijnlijk dat het probleem wordt veroorzaakt door een onjuiste configuratie van de RADIUS-server of het gebruik van een ongeldige gebruikersnaam of wachtwoord. Bijvoorbeeld, als u de alternatieve UPN-achtervoegsel in de gebruikersnaam gebruikt, mislukken de aanmeldingspoging. Gebruik dezelfde accountnaam voor de beste resultaten. 

Raadpleeg de gebeurtenislogboeken voor beveiliging op de RADIUS-server is om deze problemen te een ideaal plaats om te beginnen. Als u wilt opslaan op tijd zoeken naar gebeurtenissen, kunt u de op rollen gebaseerde Network Policy and Access Server aangepaste weergave in Logboeken, zoals hier wordt weergegeven. Gebeurtenis-ID 6273' geeft aan gebeurtenissen waar de NPS toegang geweigerd voor een gebruiker. 

![Logboeken](./media/nps-extension-vpn/image22.png)
 
## <a name="configure-multi-factor-authentication"></a>Multi-factor Authentication configureren

Zie de artikelen voor informatie over het configureren van gebruikers voor multi-factor Authentication [vereisen verificatie in twee stappen voor een gebruiker of groep](multi-factor-authentication-get-started-user-states.md) en [Mijn account voor verificatie in twee stappen instellen](multi-factor-authentication-end-user-first-time.md)

## <a name="install-and-configure-the-nps-extension"></a>Installeren en configureren van de NPS-uitbreiding

Deze sectie geeft instructies voor het configureren van VPN MFA gebruiken voor clientverificatie met de VPN-server.

Na het installeren en configureren van de NPS-uitbreiding, worden alle RADIUS-clientverificatie die wordt verwerkt door deze server is vereist voor MFA gebruiken. Als uw VPN-gebruikers niet zijn geregistreerd in Azure multi-factor Authentication, kunt u het volgende doen:

* Een andere RADIUS-server instellen voor verificatie van gebruikers die niet zijn geconfigureerd voor MFA gebruiken. 

* Maak een registervermelding waarmee gebruikers met een handicap op te geven van een tweede verificatiefactor als ze zijn geregistreerd bij Azure multi-factor Authentication. 

Maak een nieuwe tekenreekswaarde met de naam _REQUIRE_USER_MATCH in HKLM\SOFTWARE\Microsoft\AzureMfa_, en stel de waarde op *True* of *False*. 

![De instelling 'Gebruikersovereenkomst vereist'](./media/nps-extension-vpn/image34.png)
 
Als de waarde is ingesteld op *waar* of is leeg, alle verificatieaanvragen zijn onderworpen aan een challenge MFA. Als de waarde is ingesteld op *False*, MFA uitdagingen alleen voor gebruikers die zijn geregistreerd bij Azure multi-factor Authentication worden uitgegeven. Gebruik de *False* testen of in een productieomgeving gedurende een periode onboarding instellen.

### <a name="obtain-the-azure-active-directory-guid-id"></a>De ID van de Azure Active Directory-GUID verkrijgen

Als onderdeel van de configuratie van de NPS-extensie, kunt u beheerdersreferenties en de ID van uw Azure AD-tenant moet opgeven. De ID verkrijgen als volgt:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als globale beheerder van de Azure-tenant.

2. Selecteer in het linkerdeelvenster de **Azure Active Directory** knop.

3. Selecteer **eigenschappen**.

4. Als uw Azure AD-ID kopiëren, selecteert u de **kopie** knop.
 
    ![De Azure AD-ID](./media/nps-extension-vpn/image35.png)

### <a name="install-the-nps-extension"></a>De NPS-uitbreiding installeren
De NPS-uitbreiding moet worden geïnstalleerd op een server met Network Policy and Access Services-rol geïnstalleerd en die functies als de RADIUS-server in uw ontwerp. Voer *niet* installeren van de NPS-extensie op uw extern bureaublad-server.

1. Download de NPS-extensie van [Microsoft Download Center](https://aka.ms/npsmfa). 

2. Kopieer het setup-bestand (*NpsExtnForAzureMfaInstaller.exe*) naar de NPS-server.

3. Dubbelklik op de NPS-server op **NpsExtnForAzureMfaInstaller.exe** en, als u wordt gevraagd, selecteert u **uitvoeren**.

4. In de **NPS-extensie voor Azure MFA Setup** venster, beoordeling de gebruiksrechtovereenkomst, selecteer de **ik ga akkoord met de licentievoorwaarden en voorwaarden** selectievakje en selecteer vervolgens **installeren**.

    ![Het venster 'NPS-extensie voor Azure MFA Setup'](./media/nps-extension-vpn/image36.png)
 
5. In de **NPS-extensie voor Azure MFA Setup** Selecteer **sluiten**.  

    ![Het 'Setup geslaagd' bevestigingsvenster](./media/nps-extension-vpn/image37.png) 
 
### <a name="configure-certificates-for-use-with-the-nps-extension-by-using-a-powershell-script"></a>Certificaten voor gebruik met de extensie NPS configureren met behulp van een PowerShell-script
Beveiligde communicatie en zekerheid te garanderen, configureert u certificaten voor gebruik met de NPS-extensie. De NPS-onderdelen zijn Windows PowerShell-script waarmee een zelfondertekend certificaat voor gebruik met NPS worden geconfigureerd. 

Het script voert de volgende handelingen uit:

* Een zelfondertekend certificaat gemaakt.
* De openbare sleutel van het certificaat naar de service-principal over Azure AD koppelt.
* Het certificaat wordt opgeslagen in het archief van de lokale computer.
* De gebruiker netwerktoegang tot de persoonlijke sleutel van het certificaat verleent.
* De NPS-service opnieuw is opgestart.

Als u wilt uw eigen certificaten te gebruiken, moet u de openbare sleutel van uw certificaat koppelen aan de service-principal over Azure AD en enzovoort.

Voor het gebruik van het script bieden u de uitbreiding met uw beheerdersreferenties voor de Azure Active Directory en de Azure Active Directory-tenant-ID die u eerder hebt gekopieerd. Het script uitvoeren op elke NPS-server waarop u de extensie NPS installeert.

1. Voer Windows PowerShell als beheerder.

2. Voer de PowerShell-opdrachtprompt **cd c:\Program Files\Microsoft\AzureMfa\Config**, en selecteer vervolgens op Enter.

3. Voer op de volgende opdrachtregel **.\AzureMfsNpsExtnConfigSetup.ps1**, en selecteer vervolgens op Enter. Het script wordt gecontroleerd of de Azure AD PowerShell-module is geïnstalleerd. Als dit niet is geïnstalleerd, installeert u de module met het script voor u.
 
    ![PowerShell](./media/nps-extension-vpn/image38.png)
 
    Nadat het script wordt gecontroleerd door de installatie van de PowerShell-module, wordt de Azure Active Directory PowerShell-module aanmelden venster weergegeven. 

4. Voer uw Azure AD-beheerdersreferenties en het wachtwoord en selecteer vervolgens **aanmelden**. 
 
    ![Het venster PowerShell aanmelden](./media/nps-extension-vpn/image39.png)
 
5. Bij de opdrachtprompt plakken van de tenant-ID die u eerder hebt gekopieerd en schakelt u op Enter. 

    ![Tenant-id](./media/nps-extension-vpn/image40.png)

    Het script maakt een zelfondertekend certificaat en andere configuratiewijzigingen uitvoert. De uitvoer is dat in de volgende afbeelding:

    ![Zelfondertekend certificaat](./media/nps-extension-vpn/image41.png)

6. De server opnieuw opgestart.

### <a name="verify-the-configuration"></a>Controleer de configuratie
Om te controleren of de configuratie, moet u een nieuwe VPN-verbinding met de VPN-server maken. Nadat u uw referenties voor primaire verificatie correct hebt ingevoerd, wordt de VPN-verbinding wordt gewacht op de secundaire verificatie mislukt voordat de verbinding tot stand is gebracht, zoals hieronder wordt weergegeven. 

![Het venster Windows-VPN-instellingen](./media/nps-extension-vpn/image42.png)

Als u verificatie is met de secundaire verificatiemethode die u eerder hebt geconfigureerd in de Azure MFA, kunt u bent verbonden met de resource. Als de secundaire verificatie mislukt is, wordt u toegang tot de bron geweigerd. 

De Microsoft Authenticator-app op een Windows Phone biedt in het volgende voorbeeld wordt de secundaire verificatie:

![Het account verifiëren](./media/nps-extension-vpn/image43.png)

Nadat u hebt geverifieerd met behulp van de secundaire methode, kunt u toegang tot de virtuele poort op de VPN-server zijn verleend. Omdat u een secundaire verificatiemethode gebruiken met behulp van een mobiele app op een vertrouwd apparaat zijn vereist, is het proces aanmelden veiliger dan als tot alleen een combinatie van gebruikersnaam en wachtwoord gebruikten.

### <a name="view-event-viewer-logs-for-successful-sign-in-events"></a>Logboeken voor geslaagde aanmelding gebeurtenissen weergeven
Voor informatie over het weergeven van gebeurtenissen voor geslaagde aanmelden in de logboeken van Windows-Logboeken Meld u query de Windows-beveiliging op de NPS-server met de volgende PowerShell-opdracht:

    _Get-WinEvent -Logname Security_ | where {$_.ID -eq '6272'} | FL 

![PowerShell beveiliging Logboeken](./media/nps-extension-vpn/image44.png)
 
U kunt ook het beveiligingslogboek of de Network Policy and Access Services aangepaste weergave, bekijken, zoals hier wordt weergegeven:

![Network Policy Server-logboek](./media/nps-extension-vpn/image45.png)

Op de server waar u de NPS-extensie voor Azure multi-factor Authentication hebt geïnstalleerd, vindt u Event Viewer-logboeken die specifiek zijn voor de uitbreiding op *toepassingen en Services Logs\Microsoft\AzureMfa*. 

    _Get-WinEvent -Logname Security_ | where {$_.ID -eq '6272'} | FL

![Logboeken 'Het aantal gebeurtenissen' deelvenster](./media/nps-extension-vpn/image46.png)

## <a name="troubleshooting-guide"></a>Handleiding voor het oplossen van problemen
Als de configuratie niet werkt zoals verwacht, probeert op te lossen door te controleren dat de gebruiker is geconfigureerd voor gebruik van MFA. Een gebruiker verbinding maken met de [Azure-portal](https://portal.azure.com). Als de gebruiker wordt gevraagd om secundaire verificatie en kan worden geverifieerd, kunt u een onjuiste configuratie van MFA als een probleem elimineren.

Als MFA voor de gebruiker werkt, raadpleegt u de relevante Event Viewer-Logboeken. De logboeken bevatten de beveiligingsgebeurtenis voor, operationele Gateway en Azure multi-factor Authentication-logboeken die worden beschreven in de vorige sectie. 

Hier ziet u een voorbeeld van een beveiligingslogboek die een mislukte aanmelding gebeurtenis (gebeurtenis-ID 6273) wordt weergegeven:

![Met een mislukte gebeurtenis aanmelden beveiligingslogboek](./media/nps-extension-vpn/image47.png)

Hier ziet u een bijbehorende gebeurtenis uit de Azure multi-factor Authentication-logboek:

![Azure multi-factor Authentication-Logboeken](./media/nps-extension-vpn/image48.png)

Hiertoe geavanceerde probleemoplossing, raadpleegt u de NPS-indeling databaselogboekbestanden waarop de NPS-service is geïnstalleerd. De logboekbestanden worden gemaakt in de _%SystemRoot%\System32\Logs_ map als CSV-bestanden. Zie voor een beschrijving van de logboekbestanden van [interpreteren NPS indeling databaselogboekbestanden](https://technet.microsoft.com/library/cc771748.aspx). 

De items in deze logboekbestanden zijn moeilijk te interpreteren, tenzij u ze naar een spreadsheet of een database exporteert. Hier vindt u veel Internet Authentication Service (IAS) is online hulpprogramma's om u te helpen u bij het interpreteren van de logboekbestanden parseren. De uitvoer van een dergelijke downloadbare [shareware toepassing](http://www.deepsoftware.com/iasviewer) Hier wordt weergegeven: 

![Shareware toepassing](./media/nps-extension-vpn/image49.png)

Hiertoe andere problemen oplossen, kunt u een programma voor protocolanalyse zoals Wireshark of [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx). De volgende afbeelding van Wireshark toont de RADIUS-berichten tussen de VPN-server en de NPS.

![Microsoft Message Analyzer](./media/nps-extension-vpn/image50.png)

Zie voor meer informatie [uw bestaande NPS-infrastructuur integreren met Azure multi-factor Authentication](multi-factor-authentication-nps-extension.md). 

## <a name="next-steps"></a>Volgende stappen
[Ophalen van Azure multi-factor Authentication](multi-factor-authentication-versions-plans.md)

[Extern bureaublad-gateway en Azure Multi-Factor Authentication-server met behulp van RADIUS](multi-factor-authentication-get-started-server-rdg.md)

[Uw on-premises adreslijsten integreren met Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md)

