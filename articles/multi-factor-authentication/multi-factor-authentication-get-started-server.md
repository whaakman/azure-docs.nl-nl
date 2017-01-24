---
title: Aan de slag met de Azure Multi-Factor Authentication-server | Microsoft Docs
description: Dit is de Azure Multi-Factor Authentication-pagina waarop wordt beschreven hoe u met de Azure MFA-server aan de slag kunt gaan.
services: multi-factor-authentication
keywords: verificatieserver, app-activeringspagina voor azure multi factor authentication, downloaden bij verificatieserver
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: e94120e4-ed77-44b8-84e4-1c5f7e186a6b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/03/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 0fec7a18e098891374b3b0d7313a72918b630918
ms.openlocfilehash: 7fb107922af9d2316fb7490670002f4255572458

---

# <a name="getting-started-with-the-azure-multi-factor-authentication-server"></a>Aan de slag met de Azure Multi-Factor Authentication-server
<center>![MFA on-premises](./media/multi-factor-authentication-get-started-server/server2.png)</center>

Nu u hebt vastgesteld dat on-premises Multi-Factor Authentication-server moet worden gebruikt, gaat u aan de slag. Op deze pagina vindt u informatie over een nieuwe installatie van de server en leert u deze in te stellen met on-premises Active Directory. Als de PhoneFactor-server al is geïnstalleerd en u een upgrade wilt uitvoeren, leest u het artikel [Upgrading to the Azure Multi-Factor Server](multi-factor-authentication-get-started-server-upgrade.md) (Een upgrade uitvoeren naar de Azure Multi-Factor Authentication-server). Als u op zoek bent naar informatie over het installeren van alleen de webservice, leest u het artikel [Deploying the Azure Multi-Factor Authentication Server Mobile App Web Service](multi-factor-authentication-get-started-server-webservice.md) (Webservice voor mobiele apps van Azure Multi-Factor Authentication-server implementeren).

## <a name="download-the-azure-multi-factor-authentication-server"></a>De Azure Multi-Factor Authentication-server downloaden
U kunt de Azure Multi-Factor Authentication-server op twee verschillende manieren downloaden. Beide downloads verlopen via de Azure-portal. Bij de eerste manier beheert u rechtstreeks de Multi-Factor Authentication-provider. De tweede manier verloopt via de service-instellingen. Voor de tweede manier is een Multi-Factor Authentication-provider of een Azure MFA-, Azure AD Premium- of Enterprise Mobility Suite-licentie vereist.

> [!Important]
> Deze twee opties lijken op elkaar, maar het is belangrijk te weten welke moet worden gebruikt. Als uw gebruikers licenties hebben die worden geleverd met MFA, hoeft u geen Multi-Factor Authentication-provider te maken voor toegang tot de serverdownload. Gebruik in plaats daarvan optie 2 om de server te downloaden van de pagina met service-instellingen. 

### <a name="option-1-download-azure-multi-factor-authentication-server-from-the-azure-classic-portal"></a>Optie 1: de Azure Multi-Factor Authentication-server downloaden via de klassieke Azure-portal

Gebruik deze downloadoptie als u al een Multi-Factor Authentication-provider hebt omdat u voor MFA betaalt per gebruiker of per verificatie. 

1. Meld u aan als beheerder bij de [klassieke Azure Portal](https://manage.windowsazure.com).
2. Selecteer aan de linkerkant **Active Directory**.
3. Klik op de Active Directory-pagina op **Providers voor Multi-Factor Authentication**
    ![Providers voor Multi-Factor Authentication](./media/multi-factor-authentication-get-started-server/authproviders.png)
4. Klik onderaan op **Beheren**. Er wordt een nieuwe pagina geopend.
5. Klik op **Downloads**.
6. Klik boven **Activeringsreferenties** genereren op **Downloaden**.
   ![Downloaden](./media/multi-factor-authentication-get-started-server/download4.png)
7. Sla de download op.

### <a name="option-2-download-azure-multi-factor-authentication-server-from-the-service-settings"></a>Optie 2: de Azure Multi-Factor Authentication-server downloaden via de service-instellingen

Gebruik deze downloadoptie als u Enterprise Mobility Suite-, Azure AD Premium- of Enterprise Cloud Suite-licenties hebt. 

1. Meld u aan als beheerder bij de [klassieke Azure Portal](https://manage.windowsazure.com).
2. Selecteer aan de linkerkant **Active Directory**.
3. Dubbelklik op uw exemplaar van Azure AD.
4. Klik bovenaan op **Configureren**
5. Blader omlaag naar de sectie **Multi-Factor Authentication** en selecteer **Service-instellingen beheren**
6. Klik onderaan op de pagina met service-instellingen op **Naar de portal**. Er wordt een nieuwe pagina geopend.
   ![Downloaden](./media/multi-factor-authentication-get-started-server/servicesettings.png)
7. Klik op **Downloads**.
8. Klik boven **Activeringsreferenties** genereren op **Downloaden**.
    ![Downloaden](./media/multi-factor-authentication-get-started-server/download4.png)
9. Sla de download op.

## <a name="install-and-configure-the-azure-multi-factor-authentication-server"></a>De Azure Multi-Factor Authentication-server installeren en configureren
Nu u de server hebt gedownload, kunt u deze installeren en configureren.  Zorg ervoor dat de server waarop u deze installeert aan de volgende vereisten voldoet.

| Vereisten voor Azure Multi-Factor Authentication-server | Beschrijving |
|:--- |:--- |
| Hardware |<li>200 MB aan vasteschijfruimte</li><li>Voor x32 of x64 geschikte processor</li><li>1 GB of meer RAM-geheugen</li> |
| Software |<li>Windows Server 2008 of een recentere versie als de host een serverbesturingssysteem is</li><li>Windows 7 of een recentere versie als de host een clientbesturingssysteem is</li><li>Microsoft .NET 4.0 Framework</li><li>IIS 7.0 of een recentere versie als de installatie wordt uitgevoerd vanuit de gebruikersportal of de webservice-SDK</li> |

### <a name="azure-multi-factor-authentication-server-firewall-requirements"></a>Firewallvereisten voor Azure Multi-Factor Authentication-server
- - -
Elke MFA-server moet kunnen communiceren op poort 443, uitgaand naar de volgende adressen:

* https://pfd.phonefactor.net
* https://pfd2.phonefactor.net
* https://css.phonefactor.net

Als firewalls voor uitgaand verkeer op poort 443 worden beperkt, moeten de volgende IP-adresbereiken worden geopend:

| IP-subnet | Netmasker | IP-bereik |
|:--- |:--- |:--- |
| 134.170.116.0/25 |255.255.255.128 |134.170.116.1 – 134.170.116.126 |
| 134.170.165.0/25 |255.255.255.128 |134.170.165.1 – 134.170.165.126 |
| 70.37.154.128/25 |255.255.255.128 |70.37.154.129 – 70.37.154.254 |

Als u de functie Gebeurtenisbevestiging niet gebruikt en gebruikers geen mobiele apps gebruiken om in het bedrijfsnetwerk te verifiëren vanaf apparaten, kunt u de IP-adresbereiken beperken tot het volgende:

| IP-subnet | Netmasker | IP-bereik |
|:--- |:--- |:--- |
| 134.170.116.72/29 |255.255.255.248 |134.170.116.72 – 134.170.116.79 |
| 134.170.165.72/29 |255.255.255.248 |134.170.165.72 – 134.170.165.79 |
| 70.37.154.200/29 |255.255.255.248 |70.37.154.201 – 70.37.154.206 |

### <a name="to-install-and-configure-the-azure-multi-factor-authentication-server"></a>De Azure Multi-Factor Authentication-server installeren en configureren

1. Dubbelklik op het uitvoerbare bestand. Hiermee start u de installatie.
2. Zorg ervoor dat in het scherm Installatiemap selecteren de map juist is en klik op **Volgende**.
3. Nadat de installatie is voltooid, klikt u op **Voltooien**.  De configuratiewizard wordt gestart.
4. Schakel in het welkomstscherm van de configuratiewizard het selectievakje **De wizard Verificatieconfiguratie overslaan** en klik op **Volgende**.  Hiermee sluit u de wizard en wordt de server gestart.
    ![Cloud](./media/multi-factor-authentication-get-started-server/skip2.png)
5. Klik op de pagina waarvan we de server hebben gedownload op de knop **Activeringsreferenties genereren**. Kopieer deze informatie naar de Azure MFA-server in de desbetreffende vakken en klik op **Activeren**.

De bovenstaande stappen geven een snelle installatie met behulp van de configuratiewizard weer.  U kunt de verificatiewizard opnieuw uitvoeren door deze te selecteren in het menu Hulpmiddelen op de server.

## <a name="import-users-from-active-directory"></a>Gebruikers uit Active Directory importeren
Nu dat de server is geïnstalleerd en geconfigureerd, kunt u snel gebruikers importeren in de Azure MFA-server.

1. Selecteer links in de Azure MFA-server de optie **Gebruikers**.
2. Selecteer onderaan **Importeren uit Active Directory**.
3. Nu kunt u zoeken naar afzonderlijke gebruikers of in de AD-directory zoeken naar organisatie-eenheden met gebruikers.  In dit geval geeft u de organisatie-eenheid met gebruikers op.
4. Markeer rechts alle gebruikers en klik op **Importeren**.  Normaal verschijnt dan een pop-upvenster met de melding dat het importeren is gelukt.  Sluit het importvenster.

![Cloud](./media/multi-factor-authentication-get-started-server/import2.png)

## <a name="send-users-an-email"></a>E-mail verzenden naar gebruikers
Nu u alle gebruikers hebt geïmporteerd in de MFA-server, wordt aangeraden om een e-mail naar hen te versturen om ze te laten weten dat ze zijn ingeschreven voor verificatie in twee stappen.

U moet uw e-mail aanpassen aan de manier waarop u de gebruikers hebt geconfigureerd voor verificatie in twee stappen. Als u bijvoorbeeld de telefoonnummers van gebruikers vanuit het bedrijfsadresboek importeert, moeten in de e-mail de standaardtelefoonnummers staan. Gebruikers weten dan wat ze kunnen verwachten. Als u de telefoonnummers van gebruikers niet importeert, of als gebruikers zijn geconfigureerd voor het gebruik van hun mobiele app, stuurt u hen een e-mail met een koppeling naar de Azure Multi-Factor Authentication-gebruikersportal om de accountinschrijving te voltooien.

De inhoud van het e-mailbericht is ook afhankelijk van de verificatiemethode die voor de gebruiker is ingesteld (telefoonoproep, sms of mobiele app).  Als de gebruiker bijvoorbeeld bij de verificatie een pincode moet opgeven, staat in het e-mailbericht welke pincode initieel is ingesteld.  Gebruikers moeten hun pincode wijzigen bij de eerste verificatie.


### <a name="configure-email-and-email-templates"></a>E-mailberichten en e-mailsjablonen configureren
Klik op het e-mailpictogram links om de instellingen voor het verzenden van deze e-mailberichten te bepalen. Hier kunt u de SMTP-gegevens van uw e-mailserver invoeren en e-mails verzenden door het selectievakje bij **E-mails versturen naar gebruikers** in te schakelen.

![E-mailinstellingen](./media/multi-factor-authentication-get-started-server/email1.png)

Op het tabblad E-mailinhoud ziet u alle beschikbare e-mailsjablonen waaruit u kunt kiezen. Afhankelijk van hoe u uw gebruikers hebt geconfigureerd voor het uitvoeren van verificatie in twee stappen, kiest u de sjabloon die voor u het meest geschikt is.

![E-mailsjablonen](./media/multi-factor-authentication-get-started-server/email2.png)

## <a name="how-the-azure-multi-factor-authentication-server-handles-user-data"></a>Hoe de Azure Multi-Factor Authentication-server omgaat met gebruikersgegevens
Wanneer u de Multi-Factor Authentication (MFA)-server on-premises gebruikt, worden de gegevens van een gebruiker opgeslagen op de on-premises servers. In de cloud worden geen permanente gebruikersgegevens opgeslagen. Wanneer de gebruiker een verificatie in twee stappen uitvoert, verzendt de MFA-server gegevens naar de Azure MFA-cloudservice om de verificatie uit te voeren. Wanneer deze verificatieaanvragen naar de cloudservice worden verzonden, worden de volgende velden in de aanvraag en logbestanden verzonden, zodat ze beschikbaar zijn in de verificatie-/gebruiksrapporten van de klant. Sommige velden zijn optioneel. Ze kunnen bijgevolg in de Multi-Factor Authentication-server worden in- of uitgeschakeld. De communicatie tussen de MFA-server en de MFA-cloudservice maakt gebruik van SSL/TLS via poort 443 (uitgaand). Deze velden zijn:

* Unieke id - gebruikersnaam of interne MFA-server-id
* Voor- en achternaam (optioneel)
* E-mailadres (optioneel)
* Telefoonnummer - bij het voeren van een telefoongesprek of bij sms-verificatie
* Apparaattoken - bij verificatie met behulp van de mobiele app
* Verificatiemodus
* Verificatieresultaat
* Naam van MFA-server
* IP van MFA-server
* Client-IP – indien beschikbaar

Naast de bovenstaande velden worden ook het verificatieresultaat (geslaagd/geweigerd) en de reden voor eventuele weigeringen opgeslagen met de verificatiegegevens en beschikbaar gesteld in de verificatie-/gebruiksrapporten.

## <a name="next-steps"></a>Volgende stappen
Gebruik de koppelingen in de onderstaande tabel voor meer informatie over geavanceerde instellingen en configuratie-informatie:

| Methode | Beschrijving |
|:--- |:--- |
| [Gebruikersportal](multi-factor-authentication-get-started-portal.md) |Informatie over de installatie en configuratie van de gebruikersportal, inclusief implementatie en selfservice van gebruiker. |
| [Active Directory Federation Service](multi-factor-authentication-get-started-adfs.md) |Informatie over het instellen van Azure Multi-Factor Authentication met AD FS. |
| [RADIUS-verificatie](multi-factor-authentication-get-started-server-radius.md) |Informatie over de installatie en configuratie van de Azure MFA-server met RADIUS. Als u RADIUS gebruikt, kunt u verschillende systemen van derden integreren met de Azure MFA-server. |
| [IIS-authenticatie](multi-factor-authentication-get-started-server-iis.md) |Informatie over de installatie en configuratie van de Azure MFA-server met IIS. Als u IIS gebruikt, kunt u verschillende systemen van derden integreren met de Azure MFA-server. |
| [Windows-verificatie](multi-factor-authentication-get-started-server-windows.md) |Informatie over de installatie en configuratie van de Azure MFA-server met Windows-authenticatie. |
| [LDAP-verificatie](multi-factor-authentication-get-started-server-ldap.md) |Informatie over de installatie en configuratie van de Azure MFA-server met LDAP-authenticatie. Als u LDAP gebruikt, kunt u verschillende systemen van derden integreren met de Azure MFA-server. |
| [Extern bureaublad-gateway en Azure Multi-Factor Authentication-server met behulp van RADIUS](multi-factor-authentication-get-started-server-rdg.md) |Informatie over de installatie en configuratie van de Azure MFA-server met extern bureaublad-gateway en gebruik van RADIUS. |
| [Synchroniseren met Windows Server Active Directory](multi-factor-authentication-get-started-server-dirint.md) |Informatie over de installatie en configuratie van de synchronisatie tussen Active Directory en de Azure MFA-server. |
| [Implementatie van de webservice voor mobiele apps van de Azure Multi-Factor Authentication-server](multi-factor-authentication-get-started-server-webservice.md) |Informatie over de installatie en configuratie van de webservice van de Azure MFA-server. |
| [Geavanceerde scenario's met Azure Multi-Factor Authentication en VPN’s van derden](multi-factor-authentication-advanced-vpn-configurations.md) | Stapsgewijze configuratiehandleidingen voor Cisco-, Citrix- en Juniper VPN-apparaten. |



<!--HONumber=Jan17_HO1-->


