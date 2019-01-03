---
title: StorSimple 8000-serie beveiliging | Microsoft Docs
description: Beschrijft de functies voor beveiliging en privacy die bescherming van uw StorSimple-service, het apparaat en de gegevens on-premises en in de cloud.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/18/2018
ms.author: alkohli
ms.openlocfilehash: 734b0cf9373ea98ab33c06b45ad53b46a3355dd6
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53976015"
---
# <a name="storsimple-security-and-data-protection"></a>StorSimple-beveiliging en gegevensbescherming

## <a name="overview"></a>Overzicht

Beveiliging is van groot belang voor iedereen die is overstappen op een nieuwe technologie, met name als de technologie wordt gebruikt met vertrouwelijke of geheime gegevens. Als u verschillende technologieën evalueren, moet u rekening houden met verbeterde risico's en de kosten voor de beveiliging van gegevens. Microsoft Azure StorSimple biedt een beveiligings- en privacy-oplossing voor gegevensbeveiliging, helpt om ervoor te zorgen:

* **Vertrouwelijkheid** : alleen geautoriseerde entiteiten kunnen inzien.
* **Integriteit** – alleen geautoriseerde entiteiten kunnen wijzigen of verwijderen van uw gegevens.

De Microsoft Azure StorSimple-oplossing bestaat uit vier hoofdonderdelen die met elkaar communiceren:

* **StorSimple Device Manager-service die wordt gehost in Microsoft Azure** – de management-service die u gebruiken om te configureren en inrichten van het StorSimple-apparaat.
* **StorSimple-apparaat** – een fysiek apparaat is geïnstalleerd in uw datacenter. Alle hosts en -clients die gegevens genereren verbinding maken met de StorSimple-apparaat en het apparaat de gegevens worden beheerd en verplaatst die naar de Azure-cloud indien van toepassing.
* **Clients /-hosts die zijn verbonden met het apparaat** – de clients in uw infrastructuur die verbinding maken met het StorSimple-apparaat en die gegevens genereren die moet worden beveiligd.
* **Cloudopslag** – De locatie in de Azure-cloud waar gegevens worden opgeslagen.

De volgende secties beschrijven de StorSimple-beveiligingsfuncties die helpen beschermen van elk van deze onderdelen en de gegevens die zijn opgeslagen op deze. Dit omvat ook een lijst met vragen die u over de beveiliging van Microsoft Azure StorSimple, en de bijbehorende antwoorden hebt mogelijk.

## <a name="storsimple-device-manager-service-protection"></a>StorSimple Device Manager-service-beveiliging

De StorSimple Device Manager-service is een management-service die wordt gehost in Microsoft Azure en gebruikt voor het beheren van alle StorSimple-apparaten die uw organisatie heeft aangeschaft. U kunt toegang tot de service StorSimple Device Manager met behulp van referenties voor uw organisatie om aan te melden bij Azure portal via een webbrowser.

Toegang tot de service StorSimple Device Manager vereist dat uw organisatie een Azure-abonnement met inbegrip van StorSimple. Uw abonnement bepaalt tot welke functies u toegang hebt in de Azure-portal. Als uw organisatie beschikt niet over een Azure-abonnement en u wilt bekijken voor meer informatie over deze [zich registreren voor Azure als een organisatie](../active-directory/fundamentals/sign-up-organization.md).

Omdat de service StorSimple Device Manager wordt gehost in Azure, wordt deze wordt beveiligd door de functies van Azure-beveiliging. Ga naar het [Vertrouwenscentrum van Microsoft Azure](https://azure.microsoft.com/support/trust-center/security/) voor meer informatie over de beveiligingsfuncties die worden geleverd door Microsoft Azure.

## <a name="storsimple-device-protection"></a>Bescherming van de StorSimple-apparaat

Het StorSimple-apparaat is een on-premises hybride opslagapparaat met (Solid-State drives) en harde schijven (HDD's), samen met redundante controllers en automatisch failover wordt toegepast. De controllers beheren opslag in lagen, het plaatsen van momenteel gebruikt (of ' hot ') gegevens in lokale opslag (in de StorSimple-apparaat of on-premises servers) tijdens het verplaatsen van gegevens die minder vaak gebruikt in de cloud.

Alleen geautoriseerde StorSimple-apparaten kunnen aanmelden bij de StorSimple Device Manager-service die u hebt gemaakt in uw Azure-abonnement. Als u wilt toestaan dat een apparaat, moet u deze registreren bij de StorSimple Device Manager-service door op te geven van de serviceregistratiesleutel. De serviceregistratiesleutel is een 128-bits van een willekeurige sleutel gegenereerd in Azure portal.

![serviceregistratiesleutel](./media/storsimple-security/ServiceRegistrationKey.png)

Voor meer informatie over hoe een serviceregistratiesleutel ophalen, gaat u naar [stap 2: De serviceregistratiesleutel ophalen](storsimple-8000-deployment-walkthrough-u2.md#step-2-get-the-service-registration-key).

De serviceregistratiesleutel is een lange sleutel met meer dan 100 tekens. U kunt de sleutel kopiëren en opslaan in een tekstbestand op een veilige locatie, zodat u deze gebruiken kunt om extra apparaten zo nodig. Als u de serviceregistratiesleutel is verbroken nadat u uw eerste apparaat hebt geregistreerd, kunt u een nieuwe sleutel genereren van de StorSimple Device Manager-service. Dit wordt niet van invloed op de werking van bestaande apparaten.

Nadat een apparaat is geregistreerd, worden tokens gebruikt om te communiceren met Microsoft Azure. De serviceregistratiesleutel wordt niet gebruikt na het registreren van apparaten.

> [!NOTE]
> Het is raadzaam dat u de serviceregistratiesleutel opnieuw na elke gebruik genereren.


## <a name="protect-your-storsimple-solution-via-passwords"></a>Beveiligen van uw StorSimple-oplossing via wachtwoorden

Wachtwoorden zijn een belangrijk aspect van de beveiliging van de computer en veel worden gebruikt in de StorSimple-oplossing om ervoor te zorgen dat uw gegevens toegankelijk voor alleen gemachtigde gebruikers is. StorSimple kunt u de volgende wachtwoorden configureren:

* Beheerderswachtwoord voor StorSimple-apparaat
* Challenge Handshake Authentication Protocol (CHAP) initiator en het doel wachtwoorden
* Wachtwoord StorSimple Snapshot Manager

### <a name="windows-powershell-for-storsimple-and-the-storsimple-device-administrator-password"></a>Windows PowerShell voor StorSimple en het beheerderswachtwoord van de StorSimple-apparaat

Windows PowerShell voor StorSimple is een opdrachtregelinterface die u gebruiken kunt voor het beheren van het StorSimple-apparaat. Windows PowerShell voor StorSimple bevat functies die kunnen u uw apparaat registreren, configureren van de netwerkinterface op uw apparaat, bepaalde typen updates installeren, problemen oplossen van uw apparaat door het openen van de ondersteuningssessie en wijzigen van de apparaatstatus. U kunt Windows PowerShell voor StorSimple openen door verbinding te maken met de seriële console op het apparaat of met behulp van Windows PowerShell voor externe toegang.

PowerShell voor externe toegang kan worden gedaan via HTTPS of HTTP. Als extern beheer via HTTPS is ingeschakeld, moet u het certificaat voor extern beheer van het apparaat downloaden en installeren op de externe client. Voor meer informatie over PowerShell voor externe toegang, gaat u naar [verbinding maken met uw StorSimple-apparaat op afstand](storsimple-8000-remote-connect.md).

Nadat u Windows PowerShell voor StorSimple verbinding maken met het apparaat gebruikt, moet u voor het beheerderswachtwoord voor aanmelding bij het apparaat.

![Wachtwoord apparaatbeheerder](./media/storsimple-security/DeviceAdminPW.png)

Houd rekening met de volgende aanbevolen procedures:

* Extern beheer is standaard uitgeschakeld. U kunt de StorSimple Device Manager-service te kunnen gebruiken. Als een aanbevolen beveiligingsprocedure, externe toegang moet worden ingeschakeld alleen tijdens de periode die daadwerkelijk nodig is.
* Als u het wachtwoord wijzigt, zorg er dan voor dat alle RAS-gebruikers waarschuwen dat ze zich een onverwachte verbindingsverlies niet.
* De StorSimple Device Manager-service kan de bestaande wachtwoorden niet ophalen: het ze alleen opnieuw kunt instellen. Het is raadzaam dat u alle wachtwoorden opslaan op een veilige plaats, zodat u geen hebt een wachtwoord opnieuw instellen als deze is vergeten. Als u een wachtwoord opnieuw instellen moet, zorg er dan voor dat alle gebruikers een melding ontvangen voordat u het opnieuw instellen.

U kunt toegang tot de Windows PowerShell-interface met behulp van een seriële verbinding met het apparaat. U kunt ook toegang tot het op afstand met behulp van HTTP of HTTPS, dit extra te beveiligen. HTTPS biedt een hoger beveiligingsniveau dan een serie- of HTTP-verbinding. Echter, voor het gebruik van HTTPS, moet u eerst een certificaat installeren op de clientcomputer die toegang het apparaat tot. U kunt het certificaat voor externe toegang downloaden via de configuratiepagina van apparaat in de StorSimple Device Manager-service. Als het certificaat voor externe toegang gegaan is, moet u een nieuw certificaat downloaden en deze doorgeven aan alle clients die zijn gemachtigd voor het gebruik van extern beheer.

### <a name="challenge-handshake-authentication-protocol-chap-initiator-and-target-passwords"></a>Challenge Handshake Authentication Protocol (CHAP) initiator en het doel wachtwoorden

CHAP is een verificatiemethode die wordt gebruikt door het StorSimple-apparaat om te valideren van de identiteit van externe clients. De verificatie is gebaseerd op een gedeelde wachtwoord. CHAP is één richting (Unidirectioneel) of wederzijdse (in twee richtingen). Met één richting CHAP verifieert het doel (het StorSimple-apparaat) een initiator (host). Wederzijdse of omgekeerde CHAP vereist dat het doel de initiator geverifieerd en vervolgens de initiator het doel te verifiëren. StorSimple kan worden geconfigureerd voor het gebruik van een van beide methoden.

Houd rekening met het volgende bij het configureren van CHAP:

* De CHAP-gebruikersnaam moet minder dan 233 tekens bevatten.
* De CHAP-wachtwoord moet 12 tot 16 tekens. Bij het gebruik een langer gebruikersnaam of wachtwoord zal leiden tot een verificatiefout opgetreden op de Windows-host.
* U kunt hetzelfde wachtwoord niet gebruiken voor zowel de CHAP-initiator en CHAP-doel.
* Nadat u het wachtwoord hebt ingesteld, kan worden gewijzigd, maar kan niet worden opgehaald. Als het wachtwoord is gewijzigd, moet u aan alle gebruikers die externe toegang zodat ze verbinding met de StorSimple-apparaat maken kunnen.

Voor meer informatie over CHAP en hoe u deze wilt configureren voor uw StorSimple-oplossing, gaat u naar [CHAP configureren voor uw StorSimple-apparaat](storsimple-8000-configure-chap.md).

### <a name="storsimple-snapshot-manager-password"></a>Wachtwoord StorSimple Snapshot Manager

StorSimple Snapshot Manager is een Microsoft Management Console (MMC)-module die gebruikmaakt van volumegroepen en de Windows Volume Shadow Copy-Service voor het genereren van toepassingsconsistente back-ups. Bovendien kunt u StorSimple Snapshot Manager back-upschema's en kloon maken en herstellen van volumes.

Wanneer u een apparaat om te gebruiken via StorSimple Snapshot Manager configureert, moet u het StorSimple Snapshot Manager-wachtwoord op te geven. Dit wachtwoord wordt eerst ingesteld in Windows PowerShell voor StorSimple tijdens de registratie. Het wachtwoord kan ook worden ingesteld en gewijzigd van de StorSimple Device Manager-service. Dit wachtwoord wordt geverifieerd door het apparaat met StorSimple Snapshot Manager.

![Wachtwoord StorSimple Snapshot Manager](./media/storsimple-security/SnapshotMgrPassword.png)

Het StorSimple Snapshot Manager-wachtwoord moet 14 of 15 tekens bestaan en moet 3 of meer van een combinatie van hoofdletters, kleine letters, cijfers en speciale tekens bevatten. Nadat u het StorSimple Snapshot Manager-wachtwoord hebt ingesteld, kan worden gewijzigd, maar kan niet worden opgehaald. Als u het wachtwoord wijzigt, zorg er dan voor dat alle externe gebruikers een melding ontvangen.

Voor meer informatie over StorSimple Snapshot Manager, gaat u naar [wat is StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md)

### <a name="password-best-practices"></a>Aanbevolen procedures voor wachtwoorden

U wordt aangeraden gebruik te maken van de volgende richtlijnen om ervoor te zorgen dat de StorSimple-wachtwoorden sterke en goed beveiligde zijn:

* Elke drie maanden van uw wachtwoord wijzigen. Wijzigen van de wachtwoorden wordt jaarlijks afgedwongen.
* Sterke wachtwoorden gebruiken. Ga voor meer informatie naar [sterke wachtwoorden maken en deze beveiligen](https://cloudblogs.microsoft.com/microsoftsecure/2014/08/25/create-stronger-passwords-and-protect-them/).
* Gebruik altijd verschillende wachtwoorden voor verschillende mechanismen voor; elk van de wachtwoorden die u opgeeft moet uniek zijn.
* Wachtwoorden niet delen met iedereen die is niet gemachtigd voor toegang tot het StorSimple-apparaat.
* Geen spreken over een wachtwoord in het zicht van anderen of hint op de indeling van een wachtwoord.
* Als u vermoedt dat een account of wachtwoord is aangetast, kunt u het incident melden aan uw beveiligingsafdeling informatie.
* Alle wachtwoorden worden behandeld als gevoelige en vertrouwelijke informatie. 

## <a name="storsimple-data-protection"></a>StorSimple-gegevensbeveiliging

Deze sectie beschrijft het StorSimple-beveiligingsfuncties die beschermen van gegevens die onderweg zijn en opgeslagen gegevens.

In andere gedeelten wordt beschreven, worden wachtwoorden gebruikt om te autoriseren en gebruikers te verifiëren voordat ze toegang tot uw StorSimple-oplossing krijgen kunnen. Een andere uit veiligheidsoverwegingen beveiligt gegevens tegen onbevoegde gebruikers terwijl deze worden overgebracht tussen opslagsystemen en terwijl deze worden opgeslagen. De volgende secties beschrijven de functies voor gegevensbescherming met StorSimple worden opgegeven.

> [!NOTE]
> Functie voor gegevensontdubbeling biedt aanvullende bescherming voor gegevens die zijn opgeslagen op het StorSimple-apparaat en in Microsoft Azure storage. Wanneer gegevens is ontdubbeld, de gegevensobjecten van de metagegevens gebruikt voor het toewijzen en toegang tot deze apart zijn opgeslagen: Er is geen beschikbare opslagniveau context om de gegevens op basis van volumestructuur, bestandssysteem of bestandsnaam te reconstrueren.


## <a name="protect-data-flowing-through-the-service"></a>Beveiligen van gegevens die binnenkomen via de service

Het voornaamste doel van de StorSimple Device Manager-service is om te beheren en configureren van het StorSimple-apparaat. De StorSimple Device Manager-service wordt uitgevoerd in Microsoft Azure. Gebruikt u de Azure portal om in te voeren van apparaat-configuratiegegevens en Microsoft Azure gebruikt vervolgens de StorSimple Device Manager-service de gegevens te verzenden naar het apparaat. StorSimple maakt gebruik van een systeem van paren van asymmetrische sleutels om ervoor te zorgen dat een inbreuk op de Azure-service niet tot een inbreuk op de opgeslagen gegevens leidt.

![Versleuteling van gegevens tijdens de overdracht](./media/storsimple-security/DataEncryption.png)

Het systeem voor asymmetrische sleutel beveiligt de gegevens die via de service als volgt:

1. Een gegevens-versleutelingscertificaat die gebruikmaakt van een asymmetrische openbare en persoonlijke sleutel paar op het apparaat wordt gegenereerd en wordt gebruikt om de gegevens te beveiligen. De sleutels worden gegenereerd wanneer het eerste apparaat is geregistreerd.
2. De gegevens van het certificaat versleutelingssleutels worden geëxporteerd naar een Personal Information Exchange (PFX)-bestand dat wordt beveiligd door de gegevensversleutelingssleutel, namelijk een sterke 128-bits sleutel die wordt willekeurig gegenereerd door het eerste apparaat tijdens de registratie.
3. De openbare sleutel van het certificaat veilig beschikbaar wordt gesteld aan de StorSimple Device Manager-service en de persoonlijke sleutel blijft met het apparaat.
4. Gegevens invoeren van de service is versleuteld met behulp van de openbare sleutel en ontsleuteld met de persoonlijke sleutel die is opgeslagen op het apparaat, is ervoor te zorgen dat de Azure-service kan niet worden gebruikt voor het ontsleutelen van de gegevens die op het apparaat.

De versleutelingssleutel voor servicegegevens gegenereerd op alleen het eerste apparaat geregistreerd bij de service. Alle latere apparaten die zijn geregistreerd bij de service moeten de dezelfde versleutelingssleutel voor servicegegevens gebruiken.

> [!IMPORTANT]
> Het is heel belangrijk dat u een kopie van de versleutelingssleutel voor servicegegevens en sla deze op een veilige locatie. Een kopie van de versleutelingssleutel voor servicegegevens moet zodanig dat deze kan worden geopend door niet-gemachtigde personen en eenvoudig kan worden doorgegeven aan de apparaatbeheerder worden opgeslagen.
> 
> Als de versleutelingssleutel voor servicegegevens gegaan is, kunt een ondersteuningsmedewerker van Microsoft u om op te halen deze voorwaarde dat u ten minste één apparaat in een online status hebt. U wordt aangeraden dat u de versleutelingssleutel voor servicegegevens wijzigen nadat deze is opgehaald.

Als u wilt wijzigen van de versleutelingssleutel voor servicegegevens en het bijbehorende certificaat voor versleuteling, volg de stappen in [wijzigen van de versleutelingssleutel voor servicegegevens voor uw StorSimple Device Manager-service](storsimple-8000-manage-service.md#change-the-service-data-encryption-key). Wijzigen van de versleutelingssleutels vereist dat alle apparaten worden bijgewerkt met de nieuwe sleutel. Daarom raden wij aan dat u de sleutel wijzigen wanneer alle apparaten online zijn. Als er apparaten zijn offline, kunnen de sleutels worden gewijzigd op een ander tijdstip. De apparaten met verouderde sleutels worden nog steeds back-ups uitvoeren, maar ze niet mogelijk om gegevens te herstellen totdat de sleutel wordt bijgewerkt.

De versleutelingssleutel voor servicegegevens en het certificaat voor versleuteling, niet verlopen. We raden echter aan dat u de versleutelingssleutel voor servicegegevens jaarlijks om te voorkomen dat inbreuk op sleutel wijzigen.

## <a name="protect-data-at-rest"></a>Gegevens in rust beveiligen

Gegevens van het StorSimple-apparaat worden beheerd door op te slaan in lagen, lokaal en in de cloud, afhankelijk van de frequentie van gebruik. Alle hostmachines die zijn verbonden met het apparaat verzenden gegevens naar het apparaat en deze gegevens vervolgens naar de cloud, waar nodig verplaatst. Gegevens worden overgebracht van het apparaat naar de cloud veilig via Internet. Elk apparaat heeft een iSCSI-doel waarmee alle gedeelde volumes op het apparaat. Alle gegevens worden versleuteld voordat deze wordt verzonden naar de cloudopslag. 

![coderingssleutel voor cloudopslag](./media/storsimple-security/CloudStorageEncryption.png)

Om te helpen de beveiliging en integriteit van gegevens naar de cloud verplaatst, kunt StorSimple u voor het definiëren van versleutelingssleutels voor cloud-opslag als volgt:

* U geeft de coderingssleutel voor cloudopslag wanneer u een volumecontainer maken. De sleutel kan niet worden gewijzigd of later toegevoegd.
* Alle volumes in een volumecontainer delen de dezelfde versleutelingssleutel. Als u een andere vorm van versleuteling voor een bepaald volume wilt, wordt u aangeraden dat u een nieuwe volumecontainer voor het hosten van dat volume maken.
* Wanneer u de versleutelingssleutel voor cloudopslag in de StorSimple Device Manager-service invoert, wordt de sleutel is gecodeerd met behulp van het openbare gedeelte van de versleutelingssleutel voor servicegegevens en vervolgens naar het apparaat verzonden.
* De coderingssleutel voor cloudopslag worden niet overal opgeslagen in de service en is alleen bekend bij het apparaat.
* Een coderingssleutel voor cloudopslag op te geven is optioneel. U kunt gegevens die zijn versleuteld op de host naar het apparaat verzenden.

### <a name="additional-security-best-practices"></a>Aanbevolen procedures voor extra beveiliging

* Verkeer splitsen: uw iSCSI-SAN uit het gebruikersverkeer op een LAN-netwerk isoleren door het implementeren van een volledig gescheiden netwerk en het gebruik van VLAN's waarbij fysieke isolatie kan niet worden gebruikt. Een speciaal netwerk voor iSCSI-opslag zorgt ervoor dat de veiligheid en prestaties van uw bedrijfskritieke gegevens. Met een combinatie van opslag en verkeer via een LAN-netwerk kunt wordt niet aanbevolen hogere latentie en netwerk ontstaan.
* Gebruik voor netwerkbeveiliging van host-zijde, netwerkinterfaces die ondersteuning bieden voor TCP/IP-Offload Engine (TOE). CPU-belasting beperkt TOE door het verwerken van TCP voor de netwerkadapter.

## <a name="protect-data-via-storage-accounts"></a>Bescherm gegevens via de storage-accounts

Elke Microsoft Azure-abonnement kan een of meer opslagaccounts maken. (Een opslagaccount biedt een unieke naamruimte voor het werken met gegevens die zijn opgeslagen in de Azure-cloud.) Toegang tot een opslagaccount wordt bepaald door het abonnement en de toegang tot sleutels die zijn gekoppeld aan dit account.

Wanneer u een opslagaccount maakt, genereert Microsoft Azure twee 512-bits opslagtoegangssleutels, een van die wordt gebruikt voor verificatie bij de StorSimple-apparaat toegang heeft tot het opslagaccount. Houd er rekening mee dat er slechts één van deze sleutels gebruikt wordt. De andere sleutel is ondergebracht in reserveren, zodat u kunt de sleutels periodiek draaien. De secundaire sleutel activeren om te roteren sleutels, en verwijder vervolgens de primaire sleutel. Vervolgens kunt u een nieuwe sleutel voor gebruik tijdens de volgende draaiing maken. (Uit veiligheidsoverwegingen vereisen veel datacenters sleutelroulatie.)

U wordt aangeraden dat u deze aanbevolen procedures voor sleutelroulatie volgt:

* U moet draaien opslagaccountsleutels regelmatig om ervoor te zorgen dat uw storage-account niet is geopend door onbevoegde gebruikers.
* Uw Azure-beheerder moet periodiek, wijzigen of de primaire of secundaire sleutel opnieuw genereren met behulp van de sectie voor opslag van de Azure-portal voor rechtstreekse toegang tot het opslagaccount.

## <a name="protect-data-via-encryption"></a>Bescherm gegevens via versleuteling

StorSimple maakt gebruik van de volgende versleutelingsalgoritmen gegevens die zijn opgeslagen- of onderweg tussen de onderdelen van uw StorSimple-oplossing.

| Algoritme | Sleutellengte | Protocollen/applications/opmerkingen |
| --- | --- | --- |
| RSA |2048 |1 van RSA PKCS-v1.5 wordt gebruikt door de Azure-portal voor het versleutelen van configuratiegegevens die wordt verzonden naar het apparaat: bijvoorbeeld storage accountreferenties, configuratie van de StorSimple-apparaat, en versleutelingssleutels voor opslag in de cloud. |
| AES |256 |AES met CBC wordt gebruikt voor het versleutelen van het openbare gedeelte van de versleutelingssleutel voor servicegegevens voordat deze wordt verzonden naar de Azure-portal van de StorSimple-apparaat. Het is ook door de StorSimple-apparaat gebruikt om gegevens te versleutelen voordat de gegevens worden verzonden naar de cloud storage-account. |

## <a name="storsimple-cloud-appliance-security"></a>StorSimple-Cloudapparaat-beveiliging

[!INCLUDE [storsimple Cloud Appliance security](../../includes/storsimple-virtual-device-security.md)]

## <a name="managing-personal-information"></a>Persoonlijke gegevens beheren

De StorSimple Device Manager voor zowel fysieke als virtuele serie verzamelt persoonlijke gegevens in de volgende belangrijke gevallen:

- Ontvang een waarschuwing gebruikersinstellingen waar e-mailadres van gebruikers zijn geconfigureerd. Deze informatie kan worden weergegeven en worden uitgeschakeld door de beheerder. Dit geldt voor de apparaten uit de StorSimple 8000-serie en de virtuele StorSimple-matrices.
  * Als u wilt weergeven en schakelt u de instellingen voor de StorSimple 8000-serie, volg de stappen in [weergeven en beheren van waarschuwingen voor StorSimple](storsimple-8000-manage-alerts.md#configure-alert-settings)
  * Als u wilt weergeven en schakelt u de instellingen voor StorSimple Virtual Array, volg de stappen in [weergeven en beheren van waarschuwingen voor StorSimple](storsimple-virtual-array-manage-alerts.md#configure-alert-settings)
- Gebruikers die toegang krijgen tot de gegevens die zich bevinden op de shares. Een lijst met gebruikers die toegang de sharegegevens tot krijgen wordt weergegeven en kan worden weergegeven. Deze lijst wordt ook verwijderd wanneer de shares wordt verwijderd. Dit geldt alleen voor virtuele StorSimple-matrices.
  * Om weer te geven van de lijst met gebruikers die toegang tot of naar een share verwijdert, voert u de stappen in [shares op de StorSimple Virtual Array beheren](storsimple-virtual-array-manage-shares.md)

Bekijk het Microsoft-privacybeleid in het [Vertrouwenscentrum](https://www.microsoft.com/trustcenter) voor meer informatie.

## <a name="frequently-asked-questions-faq"></a>Veelgestelde vragen

Hier volgen enkele vragen en antwoorden over de beveiliging en Microsoft Azure StorSimple.

**VRAAG:** Mijn service wordt aangetast. Wat moet de volgende stappen?

**A:** U moet onmiddellijk de versleutelingssleutel voor servicegegevens en de opslagaccountsleutels voor het opslagaccount dat wordt gebruikt voor cloudlagen gegevens te wijzigen. Voor instructies gaat u naar:

* [Wijzigen van de versleutelingssleutel voor servicegegevens](storsimple-8000-manage-service.md#change-the-service-data-encryption-key)
* [Sleutelroulatie van storage-accounts](storsimple-8000-manage-storage-accounts.md#key-rotation-of-storage-accounts)

**VRAAG:** Ik heb een nieuwe StorSimple-apparaat dat is de serviceregistratiesleutel wordt gevraagd. Hoe ik ophalen?

**A:** Deze sleutel is gemaakt tijdens het maken van de StorSimple Device Manager-service. Wanneer u de StorSimple Device Manager-service verbinding maken met het apparaat gebruikt, kunt u de pagina snel starten met service kunt gebruiken om te bekijken of de serviceregistratiesleutel opnieuw genereren. Een nieuwe serviceregistratiesleutel genereren, heeft dit geen invloed op bestaande geregistreerde apparaten. Voor instructies gaat u naar:

* [Weergeven of de serviceregistratiesleutel opnieuw genereren](storsimple-8000-manage-service.md##regenerate-the-service-registration-key)

**VRAAG:** Kan ik mijn versleutelingssleutel voor servicegegevens is verbroken. Wat moet ik doen?

**A:** Neem contact op met Microsoft Ondersteuning. Ze kunnen zich aanmelden bij een ondersteuningssessie op uw apparaat en hulp bij het ophalen van de sleutel (op voorwaarde dat ten minste één apparaat online is). Onmiddellijk nadat u de versleutelingssleutel voor servicegegevens hebt verkregen, moet u wijzigen om ervoor te zorgen dat de nieuwe sleutel alleen bij u bekend is. Voor instructies gaat u naar:

* [Wijzigen van de versleutelingssleutel voor servicegegevens](storsimple-8000-manage-service.md#change-the-service-data-encryption-key)

**VRAAG:**  Ik een apparaat voor een service-versleuteling sleutel gegevenswijziging geautoriseerd, maar is het proces voor sleutel niet gestart. Wat moet ik doen?

**A:** Als de time-outperiode is verlopen, moet u opnieuw autoriseren van het apparaat voor de service gegevens codering belangrijke wijziging en het proces opnieuw starten.

**VRAAG:**  Ik heb de versleutelingssleutel voor servicegegevens gewijzigd, maar ik kan niet voor het bijwerken van de andere apparaten binnen vier uur. Heb ik opnieuw te starten?

**A:** De periode van 4 uur is alleen voor het starten van de wijziging. Nadat u het updateproces op de geautoriseerde StorSimple-apparaat gestart, is de autorisatie geldig totdat alle apparaten worden bijgewerkt.

**VRAAG:** Onze StorSimple-beheerder heeft het bedrijf verlaten. Wat moet ik doen?

**A:** Wijzigen en de wachtwoorden die toegang tot het StorSimple-apparaat geven opnieuw instellen en wijzigen van de versleutelingssleutel voor servicegegevens om ervoor te zorgen dat de nieuwe informatie niet bekend is niet-geautoriseerd personeel. Voor instructies gaat u naar:

* [De StorSimple Device Manager-service gebruiken om uw storsimple-wachtwoorden wijzigen](storsimple-8000-change-passwords.md)
* [Wijzigen van de versleutelingssleutel voor servicegegevens](storsimple-8000-manage-service.md#change-the-service-data-encryption-key)
* [CHAP configureren voor uw StorSimple-apparaat](storsimple-8000-configure-chap.md)

**VRAAG:** Ik wil het wachtwoord voor StorSimple Snapshot Manager naar een host die verbinding met het StorSimple-apparaat maakt geven, maar het wachtwoord is niet beschikbaar. Wat kan ik doen?

**A:** Als u het wachtwoord vergeten bent, moet u een nieuwe maken. Vervolgens moet u alle bestaande gebruikers te informeren dat het wachtwoord is gewijzigd en dat ze hun clients voor het gebruik van het nieuwe wachtwoord moeten bijwerken. Voor instructies gaat u naar:

* [Wijzig het wachtwoord voor StorSimple Snapshot Manager](storsimple-8000-change-passwords.md#set-the-storsimple-snapshot-manager-password)
* [Een apparaat verifiëren](storsimple-snapshot-manager-manage-devices.md#authenticate-a-device)

**VRAAG:** Het certificaat voor externe toegang tot de Windows PowerShell voor StorSimple is gewijzigd op het apparaat. Hoe kan ik mijn RAS-clients bijwerken?

**A:** U kunt het nieuwe certificaat downloaden van de StorSimple Device Manager-service, en deze vervolgens opgeven in het certificaatarchief van de RAS-clients kunnen worden geïnstalleerd. Voor instructies gaat u naar:

* [De cmdlet Import-Certificate](https://docs.microsoft.com/powershell/module/pkiclient/import-certificate)

**VRAAG:** Wordt mijn gegevens beveiligd als de StorSimple Device Manager-service is geknoeid?

**A:** Configuratiegegevens van de service wordt altijd versleuteld door de openbare sleutel in een webbrowser bekijkt. Omdat de service heeft geen toegang tot de persoonlijke sleutel, is de service niet mogelijk geen gegevens zien. Als de service StorSimple Device Manager is geknoeid, zijn er geen gevolgen als er geen sleutels die zijn opgeslagen in de StorSimple Device Manager-service zijn.

**VRAAG:** Als iemand zich toegang verschaft tot het certificaat voor versleuteling, worden mijn gegevens worden aangetast?

**A:** Microsoft Azure worden de gegevensversleutelingssleutel van de klant (PFX-bestand) in een versleutelde indeling opgeslagen. Omdat het .pfx-bestand is versleuteld en de StorSimple-service beschikt niet over de versleutelingssleutel voor servicegegevens voor het ontsleutelen van het pfx-bestand, wordt er bij het gewoon de toegang tot het pfx-bestand niet alle geheimen beschikbaar.

**VRAAG:** Wat gebeurt er als een overheidsinstantie Microsoft voor mijn gegevens vraagt?

**A:** Omdat alle gegevens worden versleuteld op de service en de persoonlijke sleutel wordt gehouden met het apparaat, moet de overheidsinstantie vraag van de klant voor de gegevens.



## <a name="next-steps"></a>Volgende stappen

[StorSimple-apparaat implementeren](storsimple-8000-deployment-walkthrough-u2.md).

