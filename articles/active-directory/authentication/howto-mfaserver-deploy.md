---
title: Aan de slag Azure multi-factor Authentication-Server - Azure Active Directory
description: Stapsgewijs aan de slag met Azure MFA-Server on-premises
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 15900758945fd5c97198caf47ff01fcfb5a6a794
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67057401"
---
# <a name="getting-started-with-the-azure-multi-factor-authentication-server"></a>Aan de slag met de Azure Multi-Factor Authentication-server

<center>

![Aan de slag met MFA-Server on-premises](./media/howto-mfaserver-deploy/server2.png)</center>

Op deze pagina vindt u informatie over een nieuwe installatie van de server en leert u deze in te stellen met on-premises Active Directory. Als u de MFA-server hebt geïnstalleerd en op zoek bent naar een upgrade, raadpleegt u [Upgraden naar de meest recente Azure Multi-Factor Authentication-server](howto-mfaserver-deploy-upgrade.md). Zie [De webservice voor de mobiele app van de Azure Multi-Factor Authentication-server implementeren](howto-mfaserver-deploy-mobileapp.md) voor meer informatie over het installeren van de webservice.

> [!IMPORTANT]
> Vanaf 1 juli 2019, zal Microsoft MFA-Server niet meer bieden voor nieuwe implementaties. Nieuwe klanten die willen graag meervoudige verificatie van gebruikers vereisen moeten cloud-gebaseerde Azure multi-factor Authentication gebruiken. Bestaande klanten die vóór 1 juli MFA-Server hebben geactiveerd, worden kunnen de nieuwste versie downloaden door toekomstige updates en zoals gebruikelijk activeringsreferenties genereren.

## <a name="plan-your-deployment"></a>Uw implementatie plannen

> [!WARNING]
> Vanaf maart van de MFA-Server 2019 is downloads alleen beschikbaar voor betaalde tenants. Gratis proefversie/tenants wordt niet langer kunnen downloaden of genereren en gebruiken van activering.

Voordat u de Multi-Factor Authentication-server downloadt, moet u bedenken wat uw laadvereisten en de vereisten voor hoge beschikbaarheid zijn. Gebruik deze informatie om te bepalen hoe en waar u wilt implementeren.

Een goede richtlijn voor de hoeveelheid geheugen die u nodig hebt, is het aantal gebruikers dat u regelmatig verwacht te verifiëren.

| Gebruikers | RAM |
| ----- | --- |
| 1-10.000 | 4 GB |
| 10\.001-50.000 | 8 GB |
| 50\.001-100.000 | 12 GB |
| 100\.000-200.001 | 16 GB |
| 200\.001+ | 32 GB |

Moet u meerdere servers voor hoge beschikbaarheid of taakverdeling instellen? Er zijn een aantal manieren voor het instellen van deze configuratie met Azure MFA-server. Wanneer u uw eerste Azure MFA-server installeert, wordt die uw master. Extra servers worden ondergeschikt en synchroniseren automatisch gebruikers en configuratie met de master. Vervolgens kunt u één primaire server configureren en de rest als back-up laten fungeren, of u kunt een taakverdeling tussen alle servers instellen.

Wanneer een master Azure MFA-server offline gaat, kunnen de ondergeschikte servers nog steeds autorisatieverzoeken in twee stappen verwerken. U kunt echter geen nieuwe gebruikers toevoegen en bestaande gebruikers kunnen hun instellingen pas bijwerken als de master weer online is of een ondergeschikte server wordt gepromoveerd.

### <a name="prepare-your-environment"></a>Uw omgeving voorbereiden

Zorg ervoor dat de server die u voor Azure Multi-Factor Authentication gebruikt, voldoet aan de volgende vereisten:

| Vereisten voor Azure Multi-Factor Authentication-server | Description |
|:--- |:--- |
| Hardware |<li>200 MB aan vasteschijfruimte</li><li>Voor x32 of x64 geschikte processor</li><li>1 GB of meer RAM-geheugen</li> |
| Software |<li>Windows Server 2016</li><li>Windows Server 2012 R2</li><li>Windows Server 2012</li><li>Windows Server 2008 R2</li><li>Windows Server 2008, SP1, SP2</li><li>Windows Server 2003 R2</li><li>Windows Server 2003, SP1, SP2</li><li>Windows 10</li><li>Windows 8.1, alle edities</li><li>Windows 8, alle edities</li><li>Windows 7, alle edities</li><li>Windows Vista, alle edities, SP1, SP2</li><li>Microsoft .NET 4.0 Framework</li><li>IIS 7.0 of een recentere versie als de installatie wordt uitgevoerd vanuit de gebruikersportal of de webservice-SDK</li> |
| Machtigingen | Domeinbeheerder of ondernemingsadministrator-account om u te registreren bij Active Directory |

### <a name="azure-mfa-server-components"></a>Onderdelen van Azure MFA-server

Er zijn drie webonderdelen die gezamenlijk de Azure MFA-server vormen:

* Webservice-SDK: maakt communicatie met de andere onderdelen mogelijk en wordt geïnstalleerd op de Azure MFA-toepassingsserver.
* Gebruikersportal: een IIS-website waar gebruikers zich kunnen registreren bij Azure Multi-Factor Authentication (MFA) en hun account kunnen beheren.
* Webservice voor mobiele apps: maakt het mogelijk om een mobiele app, zoals de Microsoft Authenticator-app, te gebruiken voor verificatie in twee stappen.

Alle drie de onderdelen kunnen worden geïnstalleerd op dezelfde server als deze server internetgericht is. Als de onderdelen worden opgesplitst, wordt de webservice-SDK geïnstalleerd op de Azure MFA-toepassingsserver, en de gebruikersportal en de webservice voor mobiele apps op een internetgerichte server.

### <a name="azure-multi-factor-authentication-server-firewall-requirements"></a>Firewallvereisten voor Azure Multi-Factor Authentication-server

Elke MFA-server moet kunnen communiceren op poort 443, uitgaand naar de volgende adressen:

* https://pfd.phonefactor.net
* https://pfd2.phonefactor.net
* https://css.phonefactor.net

Als firewalls voor uitgaand verkeer op poort 443 worden beperkt, moeten de volgende IP-adresbereiken worden geopend:

| IP-subnet | Netmasker | IP-bereik |
|:---: |:---: |:---: |
| 134.170.116.0/25 |255.255.255.128 |134.170.116.1 – 134.170.116.126 |
| 134.170.165.0/25 |255.255.255.128 |134.170.165.1 – 134.170.165.126 |
| 70.37.154.128/25 |255.255.255.128 |70.37.154.129 – 70.37.154.254 |

Als u de functie Gebeurtenisbevestiging niet gebruikt en gebruikers geen mobiele apps gebruiken om in het bedrijfsnetwerk te verifiëren vanaf apparaten, hebt u alleen de volgende bereiken nodig:

| IP-subnet | Netmasker | IP-bereik |
|:---: |:---: |:---: |
| 134.170.116.72/29 |255.255.255.248 |134.170.116.72 – 134.170.116.79 |
| 134.170.165.72/29 |255.255.255.248 |134.170.165.72 – 134.170.165.79 |
| 70.37.154.200/29 |255.255.255.248 |70.37.154.201 – 70.37.154.206 |

## <a name="download-the-mfa-server"></a>De MFA-server downloaden

> [!WARNING]
> Vanaf maart van de MFA-Server 2019 is downloads alleen beschikbaar voor betaalde tenants. Gratis proefversie/tenants wordt niet langer kunnen downloaden of genereren en gebruiken van activering.

Volg deze stappen om de Azure Multi-Factor Authentication-server te downloaden via Azure Portal:

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Azure Active Directory** > **MFA-server**.
3. Selecteer **Serverinstellingen**.
4. Selecteer **Downloaden** en volg de instructies op de downloadpagina om het installatieprogramma op te slaan. 

   ![MFA-Server downloaden via de Azure-portal](./media/howto-mfaserver-deploy/downloadportal.png)

5. Houd deze pagina open want er wordt naar verwezen na het uitvoeren van het installatieprogramma.

## <a name="install-and-configure-the-mfa-server"></a>De MFA-server installeren en configureren

Nu u de server hebt gedownload, kunt u deze installeren en configureren. Zorg ervoor dat de server waarop u deze installeert aan de vereisten in de planningssectie voldoet.

1. Dubbelklik op het uitvoerbare bestand.
2. Zorg ervoor dat in het scherm Installatiemap selecteren de map juist is en klik op **Volgende**.
3. Nadat de installatie is voltooid, klikt u op **Voltooien**. De configuratiewizard wordt gestart.
4. Schakel in het welkomstscherm van de configuratiewizard het selectievakje **De wizard Verificatieconfiguratie overslaan** en klik op **Volgende**. De wizard wordt gesloten en de server wordt gestart.

   ![De wizard Authenticatieconfiguratie overslaan](./media/howto-mfaserver-deploy/skip2.png)

5. Klik op de pagina waarvan u de server hebt gedownload, op de knop **Activeringsreferenties genereren**. Kopieer deze informatie naar de Azure MFA-server in de desbetreffende vakken en klik op **Activeren**.

## <a name="send-users-an-email"></a>E-mail verzenden naar gebruikers

U kunt de implementatie vereenvoudigen door de MFA-server toestemming te geven om te communiceren met uw gebruikers. U kunt vanaf de MFA-server een e-mail laten versturen om de gebruikers te informeren dat ze zijn ingeschreven voor verificatie in twee stappen.

U moet uw e-mail aanpassen aan de manier waarop u de gebruikers hebt geconfigureerd voor verificatie in twee stappen. Als u bijvoorbeeld de telefoonnummers vanuit het bedrijfsadresboek importeert, moeten in de e-mail de standaardtelefoonnummers staan. Gebruikers weten dan wat ze kunnen verwachten. Als u de telefoonnummers niet importeert of als uw gebruikers de mobiele app gaan gebruiken, stuurt u hun een e-mail met het verzoek om de accountinschrijving te voltooien. Voeg in de e-mail een hyperlink toe naar de portal voor Multi-Factor Authentication via Azure Portal.

De inhoud van het e-mailbericht is ook afhankelijk van de verificatiemethode die voor de gebruiker is ingesteld (telefoonoproep, sms of mobiele app). Als de gebruiker bijvoorbeeld bij de verificatie een pincode moet opgeven, staat in het e-mailbericht welke pincode initieel is ingesteld. Gebruikers moeten hun pincode wijzigen bij de eerste verificatie.

### <a name="configure-email-and-email-templates"></a>E-mailberichten en e-mailsjablonen configureren

Klik op het e-mailpictogram links om de instellingen voor het verzenden van deze e-mailberichten te bepalen. Op deze pagina kunt u de SMTP-gegevens van uw e-mailserver invoeren en e-mails verzenden door het selectievakje bij **E-mails versturen naar gebruikers** in te schakelen.

![E-mailconfiguratie van MFA-server](./media/howto-mfaserver-deploy/email1.png)

Op het tabblad E-mailinhoud ziet u alle beschikbare e-mailsjablonen waaruit u kunt kiezen. Afhankelijk van hoe u uw gebruikers hebt geconfigureerd voor het uitvoeren van verificatie in twee stappen, kiest u de sjabloon die voor u het meest geschikt is.

![MFA Server e-mailsjablonen in de console](./media/howto-mfaserver-deploy/email2.png)

## <a name="import-users-from-active-directory"></a>Gebruikers uit Active Directory importeren

Nu de server is geïnstalleerd, kunt u gebruikers gaan toevoegen. U kunt handmatig gebruikers maken, importeren uit Active Directory of automatische synchronisatie met Active Directory configureren.

### <a name="manual-import-from-active-directory"></a>Handmatig importeren uit Active Directory

1. Selecteer links in de Azure MFA-server de optie **Gebruikers**.
2. Selecteer onderaan **Importeren uit Active Directory**.
3. Nu kunt u zoeken naar afzonderlijke gebruikers of in de AD-directory zoeken naar organisatie-eenheden met gebruikers. In dit geval geeft u de organisatie-eenheid met gebruikers op.
4. Markeer rechts alle gebruikers en klik op **Importeren**. Normaal verschijnt dan een pop-upvenster met de melding dat het importeren is gelukt. Sluit het importvenster.

   ![MFA-Server gebruikers importeren uit Active Directory](./media/howto-mfaserver-deploy/import2.png)

### <a name="automated-synchronization-with-active-directory"></a>Automatische synchronisatie met Active Directory

1. Selecteer links op de Azure MFA-server de optie **Adreslijstintegratie**.
2. Navigeer naar het tabblad **Synchronisatie**.
3. Kies onderaan **Toevoegen**.
4. Kies in het groepsvak **Synchronisatie-item toevoegen** dat wordt weergegeven het domein, de organisatie-eenheid **of** beveiligingsgroep, instellingen, standaardinstellingen voor methode en standaardinstellingen voor taal voor deze synchronisatietaak en klik ten slotte op **Toevoegen**.
5. Schakel het selectievakje **Synchronisatie met Active Directory inschakelen** in en kies een **synchronisatie-interval** tussen één minuut en 24 uur.

## <a name="how-the-azure-multi-factor-authentication-server-handles-user-data"></a>Hoe de Azure Multi-Factor Authentication-server omgaat met gebruikersgegevens

Wanneer u de Multi-Factor Authentication (MFA)-server on-premises gebruikt, worden de gegevens van een gebruiker opgeslagen op de on-premises servers. In de cloud worden geen permanente gebruikersgegevens opgeslagen. Wanneer de gebruiker een verificatie in twee stappen uitvoert, verzendt de MFA-server gegevens naar de Azure MFA-cloudservice om de verificatie uit te voeren. Wanneer deze verificatieaanvragen naar de cloudservice worden verzonden, worden de volgende velden in de aanvraag en logboeken verzonden, zodat ze beschikbaar zijn in de verificatie-/gebruiksrapporten van de klant. Sommige velden zijn optioneel. Ze kunnen bijgevolg in de Multi-Factor Authentication-server worden in- of uitgeschakeld. De communicatie tussen de MFA-server en de MFA-cloudservice maakt gebruik van SSL/TLS via poort 443 (uitgaand). Deze velden zijn:

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

> [!IMPORTANT]
> Vanaf maart van 2019 de opties telefoongesprek zich niet beschikbaar voor gebruikers van de MFA-Server in vrij/proefversie van Azure AD-tenants. SMS-berichten worden niet beïnvloed door deze wijziging. Telefonische oproep blijft beschikbaar voor gebruikers in betaalde Azure AD-tenants. Deze wijziging geldt alleen voor gratis/proefversie van Azure AD-tenants.

## <a name="back-up-and-restore-azure-mfa-server"></a>Back-ups maken van Azure MFA-server en deze terugzetten

Het is voor elk systeem belangrijk dat er altijd een goede back-up beschikbaar is.

Als u een back-up wilt maken van de Azure MFA-server, zorg er dan voor dat u een kopie hebt van de map **C:\Program Files\Multi-Factor Authentication Server\Data**, inclusief het bestand **PhoneFactor.pfdata**. 

Als u de back-up moet terugzetten, voert u de volgende stappen uit:

1. Installeer de Azure MFA-server op een nieuwe server.
2. Activeer de nieuwe Azure MFA-server.
3. Stop de service **MultiFactorAuth**.
4. Overschrijf het bestand **PhoneFactor.pfdata** met de kopie in de back-up.
5. Start de service **MultiFactorAuth**.

De nieuwe server is nu actief en wordt uitgevoerd met de oorspronkelijke configuratie en gebruikersgegevens.

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>Beheer van de TLS/SSL-protocollen en coderingssuites

Nadat u een upgrade naar MFA Server versie 8.x of hoger hebt uitgevoerd of deze hebt geïnstalleerd, is het raadzaam oudere en zwakkere coderingssuites uit te schakelen of te verwijderen, tenzij uw organisatie ze nodig heeft. Informatie over het uitvoeren van deze taak vindt u in het artikel [​​SSL/TLS-protocollen en coderingssuites beheren voor AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/manage-ssl-protocols-in-ad-fs)

## <a name="next-steps"></a>Volgende stappen

- De [Gebruikersportal](howto-mfaserver-deploy-userportal.md) instellen en configureren voor selfservice door gebruikers.
- De Azure MFA-server installeren en configureren met [Active Directory Federation-service](multi-factor-authentication-get-started-adfs.md), [RADIUS-verificatie](howto-mfaserver-dir-radius.md) of [LDAP-authenticatie](howto-mfaserver-dir-ldap.md).
- [Externe bureaublad-gateway en Azure Multi-Factor Authentication-server met behulp van RADIUS](howto-mfaserver-nps-rdg.md) instellen en configureren.
- [Implementatie van de webservice voor mobiele apps van de Azure Multi-Factor Authentication-server](howto-mfaserver-deploy-mobileapp.md).
- [Geavanceerde scenario's met Azure Multi-Factor Authentication en VPN's van derden](howto-mfaserver-nps-vpn.md).
