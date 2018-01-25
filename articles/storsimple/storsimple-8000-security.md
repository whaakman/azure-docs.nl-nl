---
title: StorSimple 8000 series beveiliging | Microsoft Docs
description: Beschrijft de beveiliging en privacy-functies die uw StorSimple-service, het apparaat en de on-premises en in de cloud gegevens beveiligen.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/23/2018
ms.author: alkohli
ms.openlocfilehash: c14927f82ca01320206ccec83216777b7d1b8708
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="storsimple-security-and-data-protection"></a>StorSimple-beveiliging en gegevensbescherming

## <a name="overview"></a>Overzicht

Beveiliging is van groot belang voor iedereen die met een nieuwe technologie werken gaat met name wanneer de technologie wordt gebruikt met vertrouwelijke of persoonlijke gegevens. Als u verschillende technologieën evalueren, moet u rekening houden met verhoogde risico's en -kosten voor gegevensbeveiliging. Microsoft Azure StorSimple biedt een beveiligings- en privacy-oplossing voor gegevensbeveiliging, zodat:

* **Vertrouwelijkheid** : alleen geautoriseerde entiteiten kunnen inzien.
* **Integriteit** – alleen geautoriseerde entiteiten kunnen wijzigen of verwijderen van uw gegevens.

De Microsoft Azure StorSimple-oplossing bestaat uit vier belangrijke onderdelen met elkaar communiceren:

* **Apparaatbeheer StorSimple service die wordt gehost in Microsoft Azure** – de management-service die u gebruikt om te configureren en inrichten van het StorSimple-apparaat.
* **StorSimple-apparaat** – een fysiek apparaat geïnstalleerd in uw datacenter. Alle hosts en -clients die gegevens genereren verbinding met het StorSimple-apparaat en het apparaat de gegevens worden beheerd en verplaatst naar de Azure-cloud zo nodig.
* **Clients/hosts die zijn verbonden met het apparaat** – de clients in uw infrastructuur die verbinding maken met het StorSimple-apparaat en het genereren van gegevens die moeten worden beveiligd.
* **Cloudopslag** – de locatie in de Azure-cloud waar gegevens worden opgeslagen.

De volgende secties beschrijven de StorSimple-beveiligingsfuncties die beschermen elk van deze onderdelen en de gegevens die erop zijn opgeslagen. Dit omvat ook een lijst met vragen die u over de Microsoft Azure StorSimple-beveiliging en de bijbehorende antwoorden mogelijk.

## <a name="storsimple-device-manager-service-protection"></a>Beveiliging voor Apparaatbeheer van StorSimple-service

De service Manager van StorSimple-apparaat is een beheerservice gehost in Microsoft Azure en gebruikt voor het beheren van alle StorSimple-apparaten die uw organisatie heeft aangeschaft. U kunt toegang tot de service Manager voor StorSimple-apparaat met behulp van referenties voor uw organisatie aan te melden bij de Azure portal via een webbrowser.

Toegang tot de service Manager voor StorSimple-apparaat vereist dat uw organisatie een Azure-abonnement met inbegrip van StorSimple. Uw abonnement bepaalt de functies die u toegang hebt tot in de Azure portal. Als uw organisatie beschikt niet over een Azure-abonnement en u wilt bekijken voor meer informatie over deze [aanmelden voor Azure als een organisatie](../active-directory/sign-up-organization.md).

Omdat de service Manager voor StorSimple-apparaat wordt gehost in Azure, wordt het beveiligd door de Azure-beveiligingsfuncties. Voor meer informatie over de beveiligingsfuncties die is geleverd door Microsoft Azure gaat u naar de [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/security/).

## <a name="storsimple-device-protection"></a>Beveiliging van StorSimple-apparaat

De StorSimple-apparaat is een on-premises hybride opslagapparaat die Solid-State-schijven (SSD's) en harde schijven (HDD's), samen met automatische failover-mogelijkheden en redundante controllers bevat. De controllers beheren opslag in lagen, het plaatsen van momenteel gebruikte (of hot) gegevens op de lokale opslag (in de StorSimple-apparaat of on-premises servers), tijdens het verplaatsen van minder vaak gebruikte gegevens naar de cloud.

Alleen geautoriseerde StorSimple-apparaten mogen de Apparaatbeheer StorSimple-service die u hebt gemaakt in uw Azure-abonnement toevoegen. Voor het machtigen van een apparaat, moet u deze met de service Manager voor StorSimple-apparaat aan de hand van de serviceregistratiesleutel registreren. De serviceregistratiesleutel is een 128-bits willekeurige sleutel gegenereerd in de Azure-portal.

![serviceregistratiesleutel](./media/storsimple-security/ServiceRegistrationKey.png)

Voor meer informatie over hoe een serviceregistratiesleutel ophalen, gaat u naar [stap 2: de serviceregistratiesleutel ophalen](storsimple-8000-deployment-walkthrough-u2.md#step-2-get-the-service-registration-key).

De serviceregistratiesleutel is een lange sleutel die 100 + tekens bevat. U kunt de sleutel kopiëren en in een tekstbestand op een veilige locatie opslaan zodat u deze gebruiken kunt voor het autoriseren van extra apparaten zo nodig. Als de serviceregistratiesleutel verbroken wordt nadat u uw eerste apparaat hebt geregistreerd, kunt u een nieuwe sleutel genereren van de service Manager voor StorSimple-apparaat. Dit heeft geen invloed op de werking van bestaande apparaten.

Wanneer een apparaat is geregistreerd, gebruikt deze tokens om te communiceren met Microsoft Azure. De serviceregistratiesleutel wordt niet gebruikt na het registreren van apparaten.

> [!NOTE]
> Het is raadzaam dat u de serviceregistratiesleutel na elke toepassing genereren.


## <a name="protect-your-storsimple-solution-via-passwords"></a>Beveiligen van uw StorSimple-oplossing via wachtwoorden

Wachtwoorden zijn een belangrijk aspect van de beveiliging van de computer en grote schaal worden gebruikt in de StorSimple-oplossing om ervoor te zorgen dat uw gegevens voor alleen geautoriseerde gebruikers toegankelijk is. StorSimple kunt u de volgende wachtwoorden configureren:

* Beheerderswachtwoord voor StorSimple-apparaat
* Challenge Handshake Authentication Protocol (CHAP) en doel van de initiator wachtwoorden
* Wachtwoord StorSimple Snapshot Manager

### <a name="windows-powershell-for-storsimple-and-the-storsimple-device-administrator-password"></a>Windows PowerShell voor StorSimple en het beheerderswachtwoord van het StorSimple-apparaat

Windows PowerShell voor StorSimple is een opdrachtregelinterface die u gebruiken kunt voor het beheren van het StorSimple-apparaat. Windows PowerShell voor StorSimple bevat functies waarmee u kunnen u Registreer uw apparaat, de netwerkinterface op het apparaat configureren, bepaalde typen updates installeren, het apparaat oplossen door het openen van de ondersteuningssessie en de apparaatstatus te wijzigen. U kunt Windows PowerShell voor StorSimple openen door verbinding te maken met de seriële console op het apparaat of via Windows PowerShell op afstand.

Externe communicatie van PowerShell kan plaatsvinden via HTTPS of HTTP. Als u extern beheer via HTTPS is ingeschakeld, moet u het certificaat voor extern beheer van het apparaat downloaden en installeren op de externe client. Voor meer informatie over PowerShell voor externe toegang, gaat u naar [op afstand verbinding maken met uw StorSimple-apparaat](storsimple-8000-remote-connect.md).

Nadat u verbinding maken met het apparaat met Windows PowerShell voor StorSimple, moet u het apparaat administrator-wachtwoord voor aanmelding bij het apparaat te bieden.

![Wachtwoord apparaatbeheerder](./media/storsimple-security/DeviceAdminPW.png)

Houd rekening met de volgende aanbevolen procedures:

* Extern beheer is standaard uitgeschakeld. U kunt de service Manager voor StorSimple-apparaat in te schakelen. Als een best practice bij beveiliging externe toegang moet worden ingeschakeld alleen tijdens de periode die daadwerkelijk nodig is.
* Als u het wachtwoord wijzigt, zorg er dan voor dat alle externe gebruikers melden zodat ze zich niet een onverwachte verbinding wordt verbroken.
* De StorSimple-apparaat Manager-service kan de bestaande wachtwoorden niet ophalen: het ze alleen opnieuw kunt instellen. U wordt aangeraden dat u alle wachtwoorden opslaan op een veilige plaats, zodat u niet een wachtwoord opnieuw instellen moet als deze is vergeten. Als u een wachtwoord opnieuw instellen wilt, zorg er dan voor dat alle gebruikers waarschuwen voordat u het opnieuw instellen.

U kunt toegang tot de Windows PowerShell-interface met behulp van een seriële verbinding met het apparaat. U kunt ook toegang tot het op afstand via HTTP of HTTPS, die extra te beveiligen. HTTPS biedt een hoger niveau van beveiliging dan een serie- of HTTP-verbinding. Echter om HTTPS te gebruiken, moet u eerst een certificaat installeren op de clientcomputer die toegang het apparaat tot. U kunt het RAS-certificaat downloaden van de configuratiepagina van het apparaat in de service Manager voor StorSimple-apparaat. Als het certificaat voor externe toegang verbroken wordt, moet u een nieuw certificaat downloaden en deze doorgeven aan alle clients die gemachtigd zijn om het gebruik van extern beheer.

### <a name="challenge-handshake-authentication-protocol-chap-initiator-and-target-passwords"></a>Challenge Handshake Authentication Protocol (CHAP) en doel van de initiator wachtwoorden

CHAP is een verificatieschema dat wordt gebruikt door het StorSimple-apparaat om de identiteit van externe clients te valideren. De verificatie is gebaseerd op een gedeelde wachtwoord. CHAP mag eenzijdige (Unidirectioneel) of onderlinge (bidirectioneel). Met één richting CHAP verifieert het doel (de StorSimple-apparaat) een initiator (host). Wederzijdse of reverse CHAP vereist dat het doel de initiator geverifieerd en vervolgens de initiator het doel te verifiëren. Uw StorSimple kan worden geconfigureerd als een methode wilt gebruiken.

Als u CHAP configureert, worden op de hoogte van de volgende:

* De CHAP-gebruikersnaam moet minder dan 233 tekens bevatten.
* De CHAP-wachtwoord moet tussen 12 en 16 tekens. Bij het gebruik van een langere gebruikersnaam of wachtwoord resulteert in een verificatiefout opgetreden op de Windows-host.
* U kunt hetzelfde wachtwoord niet gebruiken voor zowel de CHAP-initiator en het CHAP-doel.
* Nadat u het wachtwoord hebt ingesteld, kan worden gewijzigd, maar kan niet worden opgehaald. Als het wachtwoord is gewijzigd, moet u alle externe toegang om gebruikers te waarschuwen dat ze een verbinding met het StorSimple-apparaat maken kunnen.

Voor meer informatie over CHAP en hoe u deze wilt configureren voor uw StorSimple-oplossing, gaat u naar [CHAP configureren voor uw StorSimple-apparaat](storsimple-8000-configure-chap.md).

### <a name="storsimple-snapshot-manager-password"></a>Wachtwoord StorSimple Snapshot Manager

StorSimple Snapshot Manager is een module Microsoft Management Console (MMC) dat volume groepen en de Windows Volume Shadow Copy-Service gebruikt voor het genereren van toepassingsconsistente back-ups. Bovendien kunt u StorSimple Snapshot Manager te maken van back-upschema en klonen of volumes te herstellen.

Wanneer u een apparaat gebruiken StorSimple Snapshot Manager configureert, moet u het StorSimple Snapshot Manager-wachtwoord op te geven. Dit wachtwoord wordt eerst ingesteld in Windows PowerShell voor StorSimple tijdens de registratie. Het wachtwoord kan ook worden ingesteld en van de service Manager voor StorSimple-apparaat is gewijzigd. Dit wachtwoord wordt het apparaat met StorSimple Snapshot Manager geverifieerd.

![Wachtwoord StorSimple Snapshot Manager](./media/storsimple-security/SnapshotMgrPassword.png)

Het StorSimple Snapshot Manager-wachtwoord moet 14 of 15 tekens bestaan en moet 3 of meer van een combinatie van hoofdletters, kleine letters, numerieke en speciale tekens bevatten. Nadat u het StorSimple Snapshot Manager-wachtwoord hebt ingesteld, kan worden gewijzigd, maar kan niet worden opgehaald. Als u het wachtwoord wijzigt, zorg er dan voor dat alle externe gebruikers melden.

Ga voor meer informatie over het StorSimple Snapshot Manager naar [wat is er StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md)

### <a name="password-best-practices"></a>Aanbevolen procedures voor wachtwoord

Het is raadzaam om gebruik te maken van de volgende richtlijnen om ervoor te zorgen dat StorSimple-wachtwoorden sterk en goed beveiligd zijn:

* Uw wachtwoord wijzigen om de drie maanden. De wachtwoorden wijzigen wordt jaarlijks afgedwongen.
* Sterke wachtwoorden gebruiken. Ga voor meer informatie naar [maken van sterke wachtwoorden en beveiligt u ze](http://blogs.microsoft.com/cybertrust/2014/08/25/create-stronger-passwords-and-protect-them/).
* Gebruik altijd verschillende wachtwoorden voor verschillende mechanismen voor; elk van de wachtwoorden die u opgeeft moet uniek zijn.
* Wachtwoorden niet delen met iedereen die is niet geautoriseerd voor toegang tot het StorSimple-apparaat.
* Geen uitspreken over een wachtwoord voor anderen of hint de indeling van een wachtwoord.
* Als u vermoedt dat een account of wachtwoord is ingebroken, moet u het incident melden aan uw beveiligingsafdeling informatie.
* Alle wachtwoorden behandelen als gevoelige en vertrouwelijke informatie. 

## <a name="storsimple-data-protection"></a>Gegevensbeveiliging voor StorSimple

Deze sectie beschrijft de StorSimple-beveiligingsfuncties die gegevens onderweg en opgeslagen gegevens te beveiligen.

Zoals beschreven in andere secties, worden wachtwoorden toe te staan en gebruikers worden geverifieerd voordat ze toegang tot uw StorSimple-oplossing krijgen kunnen gebruikt. Een andere veiligheidsoverweging beveiligt gegevens tegen onbevoegde gebruikers terwijl deze wordt overgebracht tussen opslagsystemen en terwijl deze wordt opgeslagen. De volgende secties worden de functies voor gegevensbeveiliging StorSimple voorzien.

> [!NOTE]
> Ontdubbeling biedt extra beveiliging voor gegevens die zijn opgeslagen op het StorSimple-apparaat en in Microsoft Azure storage. Wanneer gegevens is ontdubbeld, de gegevensobjecten afzonderlijk opgeslagen van de metagegevens toewijzen en toegang tot deze worden gebruikt: Er is geen beschikbare opslagniveau context de gegevens op basis van volumestructuur heeft, bestandssysteem of de bestandsnaam van het opnieuw.


## <a name="protect-data-flowing-through-the-service"></a>Beveiligen van gegevens die via de service

Het voornaamste doel van de service Manager voor StorSimple-apparaat is voor het beheren en configureren van het StorSimple-apparaat. De StorSimple-apparaat Manager-service wordt uitgevoerd in Microsoft Azure. Met de Azure-portal voert u de configuratiegegevens apparaat en Microsoft Azure gebruikt vervolgens de service Manager voor StorSimple-apparaat de gegevens te verzenden naar het apparaat. StorSimple maakt gebruik van een systeem van asymmetrische sleutelparen om ervoor te zorgen dat een inbreuk op de Azure-service niet tot een inbreuk op de opgeslagen gegevens leiden.

![Versleuteling van gegevens tijdens de vlucht](./media/storsimple-security/DataEncryption.png)

Het systeem voor asymmetrische sleutel beveiligt de gegevens die via de service als volgt loopt:

1. Een versleutelingscertificaat voor gegevens die gebruikmaakt van een asymmetrische openbare en persoonlijke sleutel paar op het apparaat wordt gegenereerd en wordt gebruikt om de gegevens te beveiligen. De sleutels worden gegenereerd wanneer het eerste apparaat is geregistreerd.
2. De gegevens van het certificaat versleutelingssleutels worden geëxporteerd naar een Personal Information Exchange (.pfx)-bestand dat wordt beveiligd door de service gegevensversleutelingssleutel, namelijk een sterke 128-bits sleutel die wordt willekeurig gegenereerd door de eerste apparaat tijdens de registratie.
3. De openbare sleutel van het certificaat veilig beschikbaar wordt gesteld aan de service Manager voor StorSimple-apparaat en de persoonlijke sleutel blijft het apparaat.
4. Gegevens invoeren van de service is versleuteld met behulp van de openbare sleutel en ontsleuteld met de persoonlijke sleutel die is opgeslagen op het apparaat is ervoor te zorgen dat de Azure-service kan niet worden gebruikt voor het ontsleutelen van de gegevens die op het apparaat.

De gegevensversleutelingssleutel van service is gegenereerd op alleen de eerste apparaat geregistreerd bij de service. Alle latere apparaten die zijn geregistreerd bij de service moeten de dezelfde gegevensversleutelingssleutel van service gebruiken.

> [!IMPORTANT]
> Het is belangrijk om te maken van een kopie van de gegevensversleutelingssleutel van service en opslaan in een veilige locatie. Een kopie van de gegevensversleutelingssleutel van service moet zodanig dat het toegankelijk is voor een specifieke persoon en eenvoudig kan worden doorgegeven aan de apparaatbeheerder worden opgeslagen.
> 
> Als de gegevensversleutelingssleutel van de service verbroken wordt, kunt een ondersteuningsmedewerker van Microsoft u voor het ophalen van het opgegeven dat er ten minste één apparaat in een online status. U wordt aangeraden dat u de gegevensversleutelingssleutel van service wijzigt nadat deze is opgehaald.

Als u wilt wijzigen van de gegevensversleutelingssleutel van service en de bijbehorende gegevens versleutelingscertificaat, volg de stappen in [wijzigen van de gegevensversleutelingssleutel van service voor uw StorSimple-apparaat Manager service](storsimple-8000-manage-service.md#change-the-service-data-encryption-key). Wijzigen van de versleutelingssleutels vereist dat alle apparaten worden bijgewerkt met de nieuwe sleutel. Daarom raden we aan dat u de sleutel wijzigen wanneer alle apparaten online zijn. Als er apparaten zijn offline is, kunnen de sleutels op een andere tijd worden gewijzigd. De apparaten met verouderde sleutels worden nog steeds back-ups uitvoeren, maar ze niet mogelijk om gegevens te herstellen totdat de sleutel wordt bijgewerkt.

De gegevensversleutelingssleutel van service en het certificaat voor de versleuteling verlopen niet. We raden echter aan dat u de gegevensversleutelingssleutel van service jaarlijks ter voorkoming van inbreuk op de sleutel wijzigt.

## <a name="protect-data-at-rest"></a>Beveiligen van gegevens in rust

Gegevens van het StorSimple-apparaat worden beheerd door op te slaan in categorieën, lokaal en in de cloud, afhankelijk van de frequentie van gebruik. Alle hostmachines die zijn verbonden met het apparaat verzenden gegevens naar het apparaat, die vervolgens worden gegevens naar de cloud, naar gelang van toepassing verplaatst. Gegevens worden overgedragen van het apparaat naar de cloud veilig via Internet. Elk apparaat heeft een iSCSI-doel waarmee alle gedeelde volumes voor dat apparaat. Alle gegevens worden versleuteld voordat deze wordt verzonden naar de cloudopslag. 

![coderingssleutel voor cloudopslag](./media/storsimple-security/CloudStorageEncryption.png)

Om te helpen de beveiliging en integriteit van gegevens verplaatst naar de cloud, kunt StorSimple u definiëren versleutelingssleutels voor cloud-opslag als volgt:

* U geeft de versleutelingssleutel voor cloudopslag wanneer u een volumecontainer maken. De sleutel kan niet worden gewijzigd of later worden toegevoegd.
* Alle volumes in een volumecontainer delen de dezelfde versleutelingssleutel. Als u een andere vorm van versleuteling voor een bepaald volume wilt, wordt u aangeraden dat u een nieuwe volumecontainer maakt voor het hosten van dat volume.
* Wanneer u de versleutelingssleutel voor cloudopslag in de service Manager voor StorSimple-apparaat opgeeft, wordt de sleutel is versleuteld met behulp van het openbare deel van de gegevensversleutelingssleutel van service en vervolgens naar het apparaat verzonden.
* De coderingssleutel voor cloudopslag wordt niet opgeslagen in de service en bekend is alleen voor het apparaat.
* Een coderingssleutel voor cloudopslag opgeven is optioneel. U kunt de gegevens die zijn versleuteld op de host naar het apparaat verzenden.

### <a name="additional-security-best-practices"></a>Aanbevolen procedures voor extra beveiliging

* Verkeer wordt verdeeld: uw iSCSI-SAN van gebruikersverkeer op een LAN-netwerk isoleren door de implementatie van een volledig gescheiden netwerk en het gebruik van VLAN's waar fysieke isolatie kan niet worden gebruikt. Een speciaal netwerk voor iSCSI-opslag wordt gegarandeerd dat met de veiligheid en de prestaties van uw bedrijfskritieke gegevens. De combinatie van opslag- en -verkeer via een LAN-netwerk kan wordt niet aanbevolen en latentie vergroten en dat netwerkfouten.
* Gebruik voor netwerkbeveiliging hostzijde netwerkinterfaces die ondersteuning bieden voor TCP/IP-Offload Engine (TOE). CPU-belasting beperkt TOE door TCP verwerking op de netwerkadapter.

## <a name="protect-data-via-storage-accounts"></a>Bescherm gegevens via de storage-accounts

Elke Microsoft Azure-abonnement kunt maken van een of meer opslagaccounts. (Een opslagaccount biedt een unieke naamruimte voor het werken met gegevens die zijn opgeslagen in de Azure-cloud.) Toegang tot een opslagaccount wordt bepaald door het abonnement en toegangssleutels die zijn gekoppeld aan dit opslagaccount.

Wanneer u een opslagaccount maakt, genereert Microsoft Azure twee 512-bits opslagtoegangssleutels, waarvan er één wordt gebruikt voor verificatie wanneer het StorSimple-apparaat opent het opslagaccount. Houd er rekening mee dat er slechts één van deze sleutels gebruikt wordt. De andere sleutel is ondergebracht in reserve, zodat u de sleutels periodiek draaien. De secundaire sleutel actief maken om terug te draaien sleutels, en verwijder vervolgens de primaire sleutel. Vervolgens kunt u een nieuwe sleutel voor gebruik tijdens de volgende draaiing maken. (Om veiligheidsredenen vereist veel datacenters sleutel worden gedraaid.)

U wordt aangeraden dat u deze best practices voor belangrijke rotatie volgt:

* U moet de opslagaccountsleutels regelmatig om ervoor te zorgen dat uw storage-account niet is geopend door onbevoegde gebruikers draaien.
* De Azure-beheerder moet regelmatig wijzigen of de primaire of secundaire sleutel opnieuw genereren met behulp van de sectie van de opslag van de Azure portal rechtstreeks toegang krijgen tot het opslagaccount.

## <a name="protect-data-via-encryption"></a>Bescherm gegevens via versleuteling

StorSimple maakt gebruik van de volgende versleutelingsalgoritmen ter bescherming van gegevens die zijn opgeslagen- of onderweg tussen de onderdelen van uw StorSimple-oplossing.

| Algoritme | Sleutellengte | Toepassingen-protocollen/opmerkingen |
| --- | --- | --- |
| RSA |2048 |1 voor RSA-PKCS-v1.5 wordt gebruikt door de Azure-portal voor het versleutelen van de configuratiegegevens die naar het apparaat wordt verzonden: bijvoorbeeld opslag accountreferenties, configuratie van StorSimple-apparaat, en in de cloud van de versleutelingssleutels voor opslag. |
| AES |256 |AES met CBC wordt gebruikt voor het openbare deel van de versleutelingssleutel van de service-gegevens versleutelen voordat deze wordt verzonden naar de Azure-portal van het StorSimple-apparaat. Dit wordt ook gebruikt door het StorSimple-apparaat om gegevens te versleutelen voordat de gegevens worden verzonden naar de cloud storage-account. |

## <a name="storsimple-cloud-appliance-security"></a>Cloud-toestel StorSimple-beveiliging

[!INCLUDE [storsimple Cloud Appliance security](../../includes/storsimple-virtual-device-security.md)]

## <a name="frequently-asked-questions-faq"></a>Veelgestelde vragen

Hier volgen enkele vragen en antwoorden over de beveiliging en Microsoft Azure StorSimple.

**V:** mijn service wordt aangetast. Wat moet de volgende stappen zijn?

**A:** u onmiddellijk de gegevensversleutelingssleutel van service en de opslagaccountsleutels voor het opslagaccount dat wordt gebruikt voor lagen gegevens moet wijzigen. Voor instructies gaat u naar:

* [De gegevensversleutelingssleutel van service wijzigen](storsimple-8000-manage-service.md#change-the-service-data-encryption-key)
* [Sleutel rotatie van de storage-accounts](storsimple-8000-manage-storage-accounts.md#key-rotation-of-storage-accounts)

**V:** ik heb een nieuwe StorSimple-apparaat die om de serviceregistratiesleutel vraagt. Hoe krijg ik terug?

**A:** deze sleutel is gemaakt tijdens het maken van de service Manager voor StorSimple-apparaat. Wanneer u de service Manager voor StorSimple-apparaat verbinding maken met het apparaat gebruikt, kunt u de pagina snel starten om te bekijken of de service registratie-sleutel opnieuw genereren. Genereren van een nieuwe registratiecode van de service heeft geen invloed op bestaande geregistreerde apparaten. Voor instructies gaat u naar:

* [Weergeven of de service registratie-sleutel opnieuw genereren](storsimple-8000-manage-service.md##regenerate-the-service-registration-key)

**V:** mijn gegevensversleutelingssleutel van service is verbroken. Wat moet ik doen?

**A:** contact op met Microsoft ondersteuning. Ze kunnen zich aanmelden met een ondersteuningssessie van het apparaat en het Help-informatie ophalen van de sleutel (op voorwaarde dat ten minste één apparaat is online). Onmiddellijk nadat u de gegevensversleutelingssleutel van service verkregen, moet u wijzigen om ervoor te zorgen dat de nieuwe sleutel voor u bekend is. Voor instructies gaat u naar:

* [De gegevensversleutelingssleutel van service wijzigen](storsimple-8000-manage-service.md#change-the-service-data-encryption-key)

**V:** gemachtigd een apparaat voor een service data encryption key wijziging, maar is het proces voor sleutel niet gestart. Wat moet ik doen?

**A:** als de time-outperiode is verstreken, moet u het apparaat voor de service gegevens versleuteling belangrijke wijziging opnieuw autoriseren en start het opnieuw.

**V:** ik de gegevensversleutelingssleutel van service gewijzigd, maar ik kon niet voor het bijwerken van de andere apparaten binnen vier uur. Heb ik opnieuw starten?

**A:** de periode van 4 uur is alleen voor het initiëren van de wijziging. Nadat u het updateproces op de geautoriseerde StorSimple-apparaat gestart, is de autorisatie geldig totdat alle apparaten worden bijgewerkt.

**V:** onze StorSimple-beheerder heeft het bedrijf verlaten. Wat moet ik doen?

**A:** wijzigings- en opnieuw instellen van de wachtwoorden die toegang tot het StorSimple-apparaat en het wijzigen van de service-gegevensversleuteling sleutel om ervoor te zorgen dat de nieuwe informatie is niet bekend bij niet-geautoriseerde personeel. Voor instructies gaat u naar:

* [De service StorSimple Apparaatbeheer gebruiken om uw storsimple-wachtwoorden wijzigen](storsimple-8000-change-passwords.md)
* [De gegevensversleutelingssleutel van service wijzigen](storsimple-8000-manage-service.md#change-the-service-data-encryption-key)
* [CHAP configureren voor uw StorSimple-apparaat](storsimple-8000-configure-chap.md)

**V:** ik wil geeft u het wachtwoord StorSimple Snapshot Manager naar een host die is verbonden met het StorSimple-apparaat, maar het wachtwoord is niet beschikbaar. Wat kan ik doen?

**A:** als u het wachtwoord vergeten bent, moet u een nieuwe maken. Vervolgens moet u alle bestaande gebruikers te informeren dat het wachtwoord is gewijzigd en dat ze hun clients voor het gebruik van het nieuwe wachtwoord moeten bijwerken. Voor instructies gaat u naar:

* [Wijzig het wachtwoord voor StorSimple Snapshot Manager](storsimple-8000-change-passwords.md#set-the-storsimple-snapshot-manager-password)
* [Een apparaat verifiëren](storsimple-snapshot-manager-manage-devices.md#authenticate-a-device)

**V:** het certificaat voor externe toegang tot de Windows PowerShell voor StorSimple is gewijzigd op het apparaat. Hoe kan ik mijn RAS-clients bijwerken?

**A:** kunt u het nieuwe certificaat downloaden van de service Manager voor StorSimple-apparaat en vervolgens opgeven in het certificaatarchief van uw RAS-clients moet worden geïnstalleerd. Voor instructies gaat u naar:

* [De cmdlet certificaat importeren](https://technet.microsoft.com/library/hh848630.aspx)

**V:** Is mijn gegevens beveiligd als de service Manager voor StorSimple-apparaat is geknoeid?

**A:** configuratiegegevens van de Service wordt altijd versleuteld met uw openbare sleutel wanneer u deze in een webbrowser bekijken. Omdat de service heeft geen toegang tot de persoonlijke sleutel, kunnen de service zich niet gegevens ziet. Als de service Manager voor StorSimple-apparaat is geknoeid, zijn er geen gevolgen, omdat er geen sleutels zijn opgeslagen in de service Manager voor StorSimple-apparaat.

**V:** als iemand toegang tot het certificaat voor de versleuteling krijgt, wordt mijn gegevens worden getroffen?

**A:** Microsoft Azure gegevensversleutelingssleutel van de klant (PFX-bestand) in een versleutelde indeling opgeslagen. Omdat het .pfx-bestand is versleuteld en het StorSimple-service beschikt niet over de gegevensversleutelingssleutel van service voor het ontsleutelen van het pfx-bestand, wordt er bij het ophalen van gewoon toegang tot het .pfx-bestand niet geen geheimen blootstellen.

**V:** wat er gebeurt als een overheidsinstelling Microsoft om mijn gegevens vraagt?

**A:** omdat alle gegevens worden versleuteld op de service en de persoonlijke sleutel wordt gehouden met het apparaat, de overheidsinstelling moet de klant gegevens opvragen.

## <a name="next-steps"></a>Volgende stappen

[StorSimple-apparaat implementeren](storsimple-8000-deployment-walkthrough-u2.md).

