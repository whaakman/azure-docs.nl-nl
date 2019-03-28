---
title: Veelgestelde vragen - Azure toegewezen HSM | Microsoft Docs
description: Veelgestelde vragen over die betrekking hebben op andere onderwerpen over Azure toegewezen HSM
services: dedicated-hsm
author: johncdawson
manager: barbkess
tags: azure-resource-manager
ms.custom: mvc
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 3/27/2019
ms.author: barclayn
ms.openlocfilehash: 19e2fb7736457884d29a142e997338e3c7ef72e7
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58540819"
---
# <a name="frequently-asked-questions-faq"></a>Veelgestelde vragen

Vind antwoorden op veelgestelde vragen over Microsoft Azure toegewezen HSM.

## <a name="the-basics"></a>De basisbeginselen

### <a name="q-what-is-a-hardware-security-module-hsm"></a>V: Wat is er een hardware security module (HSM)?

Een Hardware Security Module (HSM) is een fysieke computer of fysiek apparaat gebruikt om te beveiligen en beheren van cryptografische sleutels. Sleutels die zijn opgeslagen in HSM's kunnen worden gebruikt voor cryptografische bewerkingen. Het sleutelmateriaal blijft veilig in vervalsen, verzegelde hardware-modules. De HSM kan alleen geverifieerde en gemachtigde toepassingen om de sleutels te gebruiken. De sleutels blijven altijd binnen het bereik van de HSM-beveiliging.

### <a name="q-what-is-the-azure-dedicated-hsm-offering"></a>V: Wat is de Azure toegewezen HSM aanbieding?

Azure toegewezen HSM is een cloud-gebaseerde service waarmee HSM's die worden gehost in Azure-datacenters die rechtstreeks zijn verbonden met het virtuele netwerk van een klant. Deze HSM's zijn toegewezen netwerkapparaten (de Gemalto SafeNet netwerk HSM 7 Model A790). Ze zijn geïmplementeerd rechtstreeks naar een klanten privé IP-adresruimte en Microsoft heeft geen toegang heeft tot de cryptografische functionaliteit van de HSM's. Alleen de klant heeft volledige administratieve en cryptografische controle over deze apparaten. Klanten zijn verantwoordelijk voor het beheer van het apparaat en ze volledig activiteitenlogboeken rechtstreeks vanaf hun apparaten kunnen krijgen. Toegewezen HSM's helpen klanten te voldoen aan naleving/wettelijke vereisten zoals FIPS 140-2 Level 3, HIPAA, PCI-DSS en eIDAS en nog veel meer.

### <a name="q-what-hardware-is-used-for-dedicated-hsm"></a>V: Welke hardware wordt gebruikt voor het toegewezen HSM?

Microsoft is een partnerschap aangegaan met Gemalto om de service Azure toegewezen HSM te leveren. Het apparaat dat wordt gebruikt is de [SafeNet Luna netwerk HSM 7 Model A790](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/). Dit apparaat wordt niet alleen biedt FIPS 140-2 Level 3 gevalideerd firmware, maar er wordt een biedt ook met lage latentie, hoge prestaties en hoge capaciteit via 10 partities. 

### <a name="q-what-is-an-hsm-used-for"></a>V: Waarvoor wordt een HSM gebruikt?

HSM's worden gebruikt voor het opslaan van versleutelingssleutels die worden gebruikt voor versleutelingsfunctionaliteit, zoals SSL (Secure Socket Layer), het versleutelen van gegevens, PKI (Public Key Infrastructure), DRM (Digital Rights Management) en het ondertekenen van documenten.

### <a name="q-how-does-dedicated-hsm-work"></a>V: Hoe werkt toegewezen HSM?

Klanten kunnen inrichten HSM's in specifieke regio's met PowerShell of opdrachtregelinterface. De klant geeft aan welke virtuele netwerk de HSM's worden verbonden met en één keer ingericht de HSM's zijn beschikbaar in het opgegeven subnet op toegewezen IP-adressen in de persoonlijke IP-adresruimte van de klant. Vervolgens klanten verbinding kunnen maken met de HSM's voor het beheer van het apparaat met behulp van SSH en beheer, instellen van de HSM-clientverbindingen, initialiseren van HSM's, partities maken, definiëren en rollen, zoals partitie officer, crypto officer en crypto-gebruiker toe te wijzen. Vervolgens wordt de klant Gemalto opgegeven HSM-hulpprogramma's / SDK/clientsoftware gebruiken om uit te voeren cryptografische bewerkingen via hun toepassingen.

### <a name="q-what-software-is-provided-with-the-dedicated-hsm-service"></a>V: Welke software wordt geleverd met de toegewezen HSM-service?

Gemalto geeft alle software voor de HSM-apparaat eenmaal is ingericht door Microsoft. De software is beschikbaar op de [Gemalto customer support portal](https://supportportal.gemalto.com/csm/). Klanten die gebruikmaken van de toegewezen HSM-service zijn vereist om te worden geregistreerd voor Gemalto ondersteunen en een klant-ID waarmee de toegang en downloaden van de relevante software hebben. De ondersteunde client-software is versie 7.2 die compatibel met de FIPS 140-2 Level 3 gevalideerd firmwareversie 7.0.3 is. 

### <a name="q-does-azure-dedicated-hsm-offer-password-based-and-ped-based-authentication"></a>V: Biedt Azure toegewezen HSM verificatie op basis van wachtwoorden en op basis van een PED?

Op dit moment biedt Azure toegewezen HSM alleen HSM's met verificatie op basis van wachtwoorden.

### <a name="q-will-azure-dedicated-hsm-host-my-hsms-for-me"></a>V: Azure toegewezen HSM host Mijn HSM's voor mij?

Microsoft biedt de Gemalto SafeNet Luna netwerk-HSM via de toegewezen HSM-service alleen en kan geen host zijn geen apparaten klant worden geleverd.

### <a name="q-does-azure-dedicated-hsm-support-payment-pinetf-features"></a>V: Betaling van Azure toegewezen HSM-ondersteuning (PINCODE/ETF)-functies gebruikt?

De service Azure toegewezen HSM gebruikt SafeNet Luna netwerk HSM 7 (model A790)-apparaten. Deze apparaten bieden geen ondersteuning voor betaling HSM specifieke functionaliteit (zoals een PINCODE of ETF) of certificaten. Als u Azure toegewezen HSM-service voor de ondersteuning van betaling HSM's in de toekomst, Geef op de feedback aan uw Microsoft-accountvertegenwoordiger.

### <a name="q-which-azure-regions-is-dedicated-hsm-available-in"></a>V: Welke Azure-regio's is toegewezen HSM beschikbaar in?

Vanaf eind maart 2019, toegewezen HSM is beschikbaar in de 14 regio's die hieronder worden vermeld. Meer regio's zijn gepland en via uw Microsoft-accountvertegenwoordiger kunnen worden besproken.

* US - oost
* US - oost 2
* US - west
* US - zuid-centraal
* Azië - zuidoost
* Azië - oost
* Europa - noord
* Europa -west
* Verenigd Koninkrijk Zuid
* Verenigd Koninkrijk West
* Canada - midden
* Canada - oost
* Australië - oost
* Australië - zuidoost

## <a name="interoperability"></a>Interoperabiliteit

### <a name="q-how-does-my-application-connect-to-a-dedicated-hsm"></a>V: Hoe mijn toepassing verbinding met een HSM toegewezen?

U Gemalto opgegeven client-hulpprogramma's / SDK/software van de HSM om uit te voeren cryptografische bewerkingen van uw toepassingen. De software is beschikbaar op de [Gemalto customer support portal](https://supportportal.gemalto.com/csm/). Klanten die gebruikmaken van de toegewezen HSM-service zijn vereist om te worden geregistreerd voor Gemalto ondersteunen en een klant-ID waarmee de toegang en downloaden van de relevante software hebben.

### <a name="q-can-an-application-connect-to-dedicated-hsm-from-a-different-vnet-in-or-across-regions"></a>V: Kan een toepassing verbinden aan toegewezen HSM vanuit een ander VNET in of meerdere regio's?

Ja, u moet gebruiken [VNET-peering](../virtual-network/virtual-network-peering-overview.md) binnen een regio om verbinding te maken tussen virtuele netwerken. Voor regio-overschrijdende verbindingen, moet u [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).

### <a name="q-can-i-synchronize-dedicated-hsm-with-on-premises-hsms"></a>V: Kan ik toegewezen HSM synchroniseren met on-premises HSM's?

Ja, kunt u on-premises HSM's met toegewezen HSM synchroniseren. [Point-to-Point VPN of punt-naar-site](../vpn-gateway/vpn-gateway-about-vpngateways.md) verbinding kan worden gebruikt om verbinding met uw on-premises netwerk te maken.

### <a name="q-can-i-encrypt-data-used-by-other-azure-services-using-keys-stored-in-dedicated-hsm"></a>V: Kan ik gegevens die worden gebruikt door andere Azure-services met behulp van sleutels die zijn opgeslagen in HSM toegewezen coderen?

Nee. Azure toegewezen HSM's zijn alleen toegankelijk binnen het virtuele netwerk.

### <a name="q-can-i-import-keys-from-an-existing-on-premises-hsm-to-dedicated-hsm"></a>V: Kan ik sleutels importeren uit een bestaande On-premises HSM naar toegewezen HSM?

Ja, als u on-premises Gemalto SafeNet HSM's. Er zijn meerdere methoden. Raadpleeg de documentatie Gemalto HSM.

### <a name="q-what-operating-systems-are-supported-by-dedicated-hsm-client-software"></a>V: Welke besturingssystemen worden ondersteund door de clientsoftware van toegewezen HSM?

* Windows, Linux, Solaris, AIX, HP-UX, FreeBSD
* Virtual: VMware, hyperv, Xen, KVM

### <a name="q-how-do-i-configure-my-client-application-to-create-a-high-availability-configuration-with-multiple-partitions-from-multiple-hsms"></a>V: Hoe configureer ik mijn-clienttoepassing voor het maken van een maximaal beschikbaarheidsconfiguratie met meerdere partities uit meerdere HSM's?

Als u wilt een hoge beschikbaarheid hebben, moet u uw HSM-clientconfiguratie toepassing ingesteld voor het gebruik van de partities van de HSM. Raadpleeg de documentatie van Gemalto HSM client software.

### <a name="q-what-authentication-mechanisms-are-supported-by-dedicated-hsm"></a>V: Welke verificatiemechanismen worden ondersteund door toegewezen HSM?

Azure toegewezen HSM gebruikt SafeNet netwerk-HSM-7-apparaten (model A790) en ondersteuning van verificatie op basis van wachtwoorden.

### <a name="q-what-sdks-apis-client-software-is-available-to-use-with-dedicated-hsm"></a>V: Welke software van de client SDK's, API's, is beschikbaar voor gebruik met toegewezen HSM?

PKCS #11, Java (JCA/JCE), Microsoft CAPI en CNG, OpenSSL

### <a name="q-can-i-importmigrate-keys-from-luna-56-hsms-to-azure-dedicated-hsms"></a>V: Kan ik importeren/migreren sleutels uit Luna 5/6 HSM's naar Azure toegewezen HSM's?

Ja. Raadpleeg de Migratiehandleiding Gemalto. 

## <a name="using-your-hsm"></a>Met behulp van uw HSM

### <a name="q-how-do-i-decide-whether-to-use-azure-key-vault-or-azure-dedicated-hsm"></a>V: Hoe bepaal ik of Azure Key Vault of Azure toegewezen HSM wilt gebruiken?

Azure toegewezen HSM is de juiste keuze voor ondernemingen migreert naar Azure on-premises toepassingen die gebruikmaken van HSM's. Toegewezen HSM's bieden de mogelijkheid voor het migreren van een toepassing met minimale wijzigingen. Als in de toepassingscode die wordt uitgevoerd in een virtuele machine van Azure of een Web-App worden cryptografische bewerkingen uitgevoerd, kunnen ze toegewezen HSM gebruiken. In het algemeen krimp software die wordt uitgevoerd in de modellen van IaaS (infrastructuur als een service), die ondersteuning bieden voor HSM's zoals een sleutelarchief kunt reserveren HSM, zoals Application gateway of traffic manager voor een SSL, AD CS (Active Directory Certificate Services), of vergelijkbare PKI-hulpprogramma's, hulpprogramma's / toepassingen die worden gebruikt voor het ondertekenen van documenten, ondertekening van programmacode of een SQL Server (IaaS) is geconfigureerd met TDE (transparent databaseversleuteling) met de hoofdsleutel in een HSM met behulp van een EKM-provider (extensible key management). Azure Key Vault is geschikt voor 'geboren-in-cloud'-toepassingen of voor versleuteling-at-rest-scenario's waar klantgegevens worden verwerkt door PaaS (platform als een service) of SaaS (Software als een service)-scenario's zoals de sleutel in de Office 365-klant, Azure Information Protection , Azure Disk Encryption, Azure Data Lake Store-versleuteling met door de klant beheerde sleutel, Azure Storage met de klant beheerde sleutel, en met SQL Azure met de klant beheerde sleutel.

### <a name="q-what-usage-scenarios-best-suit-azure-dedicated-hsm"></a>V: Welke gebruiksscenario's voor aanbevolen Azure toegewezen HSM aanpassen?

Azure toegewezen HSM is het meest geschikt voor scenario's voor migratie. Dit betekent dat als u on-premises toepassingen naar Azure die al van HSM's gebruikmaken migreert. Dit biedt een lage wrijving optie om te migreren naar Azure met minimale wijzigingen in de toepassing. Als in de toepassingscode die wordt uitgevoerd in Azure VM of Web-App worden cryptografische bewerkingen uitgevoerd, kunnen toegewezen HSM kan worden gebruikt. In het algemeen krimp software die wordt uitgevoerd in de modellen van IaaS (infrastructuur als een service), die ondersteuning bieden voor HSM's zoals een sleutelarchief een speciale HSM, zoals gebruiken kan:

* Application gateway of traffic manager voor een SSL
* AD CS (Active Directory certificaatservices)
* Vergelijkbare PKI-hulpprogramma 's
* Hulpprogramma's / toepassingen die worden gebruikt voor het ondertekenen van documenten
* Ondertekening van programmacode
* SQL Server (IaaS) is geconfigureerd met TDE (transparent databaseversleuteling) met de hoofdsleutel in een HSM met behulp van een EKM-provider (extensible key management)

### <a name="q-can-dedicated-hsm-be-used-with-office-365-customer-key-azure-information-protection-azure-data-lake-store-disk-encryption-azure-storage-encryption-azure-sql-tde"></a>V: Kan toegewezen HSM worden gebruikt met Office 365 Customer Key, Azure Information Protection, Azure Data Lake Store, Azure Disk Encryption, Azure Storage Service Encryption, Azure SQL TDE?

Nee. Toegewezen HSM wordt ingericht rechtstreeks in privé-IP-adresruimte van een klant, zodat deze niet toegankelijk is voor andere Azure- of Microsoft-services.

## <a name="administration-access-and-control"></a>Beheer, toegang tot en beheer

### <a name="q-does-the-customer-get-full-exclusive-control-over-the-hsms-with-dedicated-hsms"></a>V: Volledige exclusieve controle over de HSM's met toegewezen HSM's krijgen door de klant?

Ja. Elke HSM-apparaat is volledig toegewezen voor een afzonderlijke klant en niemand anders heeft beheer eenmaal ingericht en de administrator-wachtwoord gewijzigd.

### <a name="q-what-level-of-access-does-microsoft-have-to-my-hsm"></a>V: Welk niveau van toegang beschikt Microsoft over naar Mijn HSM?

Microsoft heeft geen eventuele administratieve of cryptografische controle over de HSM. Microsoft heeft toegang via een seriële poortverbinding om op te halen van basic telemetriegegevens zoals temperatuur en onderdeel health monitor. Hierdoor kan Microsoft voor proactieve meldingen van statusproblemen. Indien nodig, kan de klant dit account uitschakelen.

### <a name="q-can-microsoft-or-anyone-at-microsoft-access-keys-in-my-dedicated-hsm"></a>V: Kan Microsoft of iemand bij Microsoft access-toetsen in Mijn toegewezen HSM?

Nee. Microsoft heeft geen toegang heeft tot de sleutels die zijn opgeslagen in klant toegewezen HSM toegewezen.

### <a name="q-can-i-upgrade-softwarefirmware-on-hsms-allocated-to-me"></a>V: Kan ik software/firmware op HSM's die aan mij toegewezen upgraden?

Als u de beste ondersteuning, wordt aangeraden niet bij te werken software/firmware op de HSM. De klant heeft echter volledig beheren, inclusief het upgraden van software/firmware als specifieke functies nodig zijn uit verschillende firmware-versies zijn. Voordat u wijzigingen aanbrengt, de implicaties moeten worden geïnterpreteerd als dit kan, bijvoorbeeld effect FIPS status gevalideerd. 

### <a name="q-how-do-i-manage-dedicated-hsm"></a>V: Hoe beheer ik toegewezen HSM?

U kunt de toegewezen HSM's beheren door deze met behulp van SSH.

### <a name="q-how-do-i-manage-partitions-on-the-dedicated-hsm"></a>V: Hoe beheer ik partities op de HSM toegewezen?

De clientsoftware Gemalto HSM wordt gebruikt voor het beheren van de HSM's en partities.

### <a name="q-how-do-i-monitor-my-hsm"></a>V: Hoe bewaak ik mijn HSM?

Een klant heeft volledige toegang tot HSM-activiteitenlogboeken via syslog- en SNMP. Een klant moet voor het instellen van een syslog-server of een SNMP-server de logboeken of gebeurtenissen ontvangen van de HSM's.

### <a name="q-can-i-get-full-access-log-of-all-hsm-operations-from-dedicated-hsm"></a>V: Kan ik volledige toegang tot logboek van alle HSM-bewerkingen van toegewezen HSM krijgen?

Ja. U kunt Logboeken van de HSM-apparaat verzenden naar een syslog-server

## <a name="high-availability"></a>Hoge beschikbaarheid

### <a name="q-is-it-possible-to-configure-high-availability-in-the-same-region-or-across-multiple-regions"></a>V: Is het mogelijk voor het configureren van hoge beschikbaarheid in dezelfde regio of in meerdere regio's?

Ja. Hoge beschikbaarheid, configuratie en installatie uitgevoerd binnen de HSM-clientsoftware die is geleverd door Gemalto. HSM's in hetzelfde VNET of andere VNETs in dezelfde regio of tussen regio's, of on-premises HSM's die zijn verbonden met een VNET maken via site-naar-site of point-to-point VPN kan worden toegevoegd aan dezelfde configuratie voor hoge beschikbaarheid. Er moet worden opgemerkt dat dit alleen sleutelmateriaal en geen specifieke configuratie-items, zoals rollen synchroniseert.

### <a name="q-can-i-add-hsms-from-my-on-premises-network-to-a-high-availability-group-with-azure-dedicated-hsm"></a>V: Kan ik HSM's van mijn on-premises netwerk aan een groep met hoge beschikbaarheid met Azure toegewezen HSM toevoegen?

Ja. Ze moeten voldoen aan de vereisten voor hoge beschikbaarheid voor 7 SafeNet Luna netwerk-HSM.

### <a name="q-can-i-add-luna-56-hsms-from-on-premises-networks-to-a-high-availability-group-with-azure-dedicated-hsm"></a>V: Kan ik Luna 5/6 HSM's van on-premises netwerken toevoegen aan een groep met hoge beschikbaarheid met Azure toegewezen HSM?

Nee.

### <a name="q-how-many-hsms-can-i-add-to-the-same-high-availability-configuration-from-one-single-application"></a>V: Hoeveel HSM's kan ik toevoegen aan de dezelfde configuratie voor hoge beschikbaarheid van één enkele toepassing?

16 leden van een HA-groep heeft onder geworden, revolutionaire testen met uitstekende resultaten.

## <a name="support"></a>Ondersteuning

### <a name="q-what-is-the-sla-for-dedicated-hsm-service"></a>V: Wat is de SLA voor toegewezen HSM-service?

Er is geen specifieke uptime guarentee opgegeven voor de toegewezen HSM-service. Microsoft zorgt ervoor dat niveau netwerktoegang tot het apparaat en kan daarom standard Azure networking Sla's van toepassing.

### <a name="q-how-are-the-hsms-used-in-azure-dedicated-hsm-protected"></a>V: Hoe worden de HSM's in Azure toegewezen HSM-beveiligde gebruikt?

Azure-datacenters hebben besturingselementen uitgebreide beveiliging voor fysieke en procedures. Daarnaast worden toegewezen HSM's gehost in een gebied verder met beperkte toegang in het datacenter. Deze gebieden zijn extra fysieke toegangscontrole en videocamera bewaking voor extra beveiliging.

### <a name="q-what-happens-if-there-is-a-security-breach-or-hardware-tampering-event"></a>V: Wat gebeurt er als er een schending van de beveiliging of manipulatie van gebeurtenis hardware?

Toegewezen HSM-service maakt gebruik van SafeNet netwerk-HSM-7-apparaten. Deze apparaten ondersteuning voor detectie van fysieke en logische onrechtmatige wijzigingen. Als er een gebeurtenis onrechtmatige wijzigingen is worden automatisch de HSM's zeroized.

### <a name="q-how-do-i-ensure-that-keys-in-my-dedicated-hsms-are-not-lost-due-to-error-or-a-malicious-insider-attack"></a>V: Hoe kan ik ervoor zorgen dat er geen sleutels in Mijn toegewezen HSM verloren vanwege een fout of een kwaadwillende insider-aanval?

Het is raadzaam een back-up on-premises HSM-apparaat gebruiken om uit te voeren van reguliere periodieke back-up van de HSM's voor herstel na noodgevallen. U moet een peer-to-peer of site-naar-site VPN-verbinding naar een on-premises werkstation dat is verbonden met een back-HSM-apparaat gebruiken.

### <a name="q-how-do-i-get-support-for-dedicated-hsm"></a>V: Hoe krijg ik ondersteuning voor toegewezen HSM?

Er wordt ondersteuning geboden door zowel Microsoft als Gemalto.  Als u er een probleem is met de hardware of netwerktoegang, verhogen met Microsoft ondersteuning aan te vragen als er een probleem is met de ontwikkeling van de configuratie, software en toepassingen van de HSM kunt rasie wordt een ondersteuningsaanvraag met Gemalto. Als u een onbepaald probleem, het verhogen van een aanvraag withg voor ondersteuning van Microsoft hebt en vervolgens Gemalto kunnen worden ingezet als vereist. 

### <a name="q-how-do-i-get-the-client-software-documentation-and-access-to-integration-guidance-for-the-safenet-luna-7-hsm"></a>V: Hoe ontvang ik de client software, documentatie en toegang tot integratie richtlijnen voor de HSM SafeNet Luna 7?

Nadat u hebt geregistreerd voor de service, een Gemalto klant-ID worden opgegeven waarmee voor registratie in de portal voor Gemalto klanten ondersteuning. Hiermee schakelt u de toegang tot alle software en documentatie, evenals inschakelen ondersteuningsaanvragen rechtstreeks met Gemalto.

### <a name="q-if-there-is-a-security-vulnerability-found-and-a-patch-is-released-by-gemalto-who-is-responsible-for-upgradingpatching-osfirmware"></a>V: Als er een beveiligingsprobleem gevonden en een patch is uitgegeven door Gemalto, die verantwoordelijk is voor upgraden/patchen OS/Firmware?

Microsoft heeft niet de mogelijkheid om te verbinden met HSM's die zijn toegewezen aan klanten. Klanten moeten upgraden en vullen van de HSM's.

### <a name="q-what-if-i-need-to-reboot-my-hsm"></a>V: Wat gebeurt er als ik wil mijn HSM opnieuw opstarten?

 De HSM is een optie voor het opnieuw opstarten van vanaf de opdrachtregel en moet worden opgemerkt dat de optie 'hard opstarten' moet worden gebruikt. Als dit niet om een bepaalde reden lukt, kunnen verhogen met Microsoft ondersteuning aan te vragen en we het apparaat fysiek opnieuw opgestart. 

## <a name="cryptography-and-standards"></a>Cryptografie en standaarden

### <a name="q-is-it-safe-to-store-encryption-keys-for-my-most-important-data-in-dedicated-hsm"></a>V: Is het veilig voor het opslaan van versleutelingssleutels voor de belangrijkste gegevens in toegewezen HSM?

Ja, bepalingen toegewezen HSM SafeNet netwerk-HSM-7-apparaten die gebruik FIPS 140-2 Level 3 HSM's gevalideerde. 

### <a name="q-what-cryptographic-keys-and-algorithms-are-supported-by-dedicated-hsm"></a>V: Welke cryptografische sleutels en de algoritmen worden ondersteund door toegewezen HSM?

Toegewezen HSM-service bepalingen SafeNet netwerk-HSM-7-apparaten. Ze bieden ondersteuning voor een breed scala aan cryptografische sleuteltypen en algoritmen, met inbegrip van: Volledige Suite B-ondersteuning

* Asymmetrische:
  * RSA
  * DSA
  * Diffie-Hellman-
  * Elliptic Curve
  * Cryptografie (ECDSA, ECDH, Ed25519, ECIES) met naam, de gebruiker gedefinieerde en Brainpool curven, KCDSA
* Symmetrische:
  * AES-GCM
  * Drievoudig DES
  * DES
  * ARIA, SEED
  * RC2
  * RC4
  * RC5
  * CAST
  * Digest/HMAC hash/bericht: SHA-1, SHA-2, SM3
  * Afgeleide sleutel: SP800 108 prestatiemeteritem modus
  * Key Wrapping: SP800-38F
  * Genereren van willekeurige getallen: FIPS 140-2 goedgekeurd DRBG (SP 800-90 CTR modus), die voldoet aan Institution DRG.4

### <a name="q-is-dedicated-hsm-fips-140-2-level-3-validated"></a>V: Is toegewezen HSM FIPS 140-2 Level 3 is gevalideerd?

Ja. Toegewezen HSM-service bepalingen SafeNet netwerk-HSM-7-apparaten die gebruikmaken van FIPS 140-2 Level 3 gevalideerde HSM's.

### <a name="q-what-do-i-need-to-do-to-make-sure-i-operate-dedicated-hsm-in-fips-140-2-level-3-validated-mode"></a>V: Wat moet ik doen om ervoor te zorgen dat ik werken toegewezen HSM in FIPS 140-2 Level 3 modus gevalideerd?

De toegewezen HSM-service inricht SafeNet Luna netwerk-HSM 7-apparaten. Deze apparaten gebruik FIPS 140-2 Level 3 gevalideerde HSM's. De standaardconfiguratie geïmplementeerd, het besturingssysteem en de firmware zijn ook FIPS-gevalideerde modules. U hoeft niet te doen voor FIPS 140-2 Level 3-compatibiliteit.

### <a name="q-how-does-a-customer-ensure-that-when-an-hsm-is-deprovisioned-all-the-key-material-is-wiped-out"></a>V: Hoe garandeert een klant dat het sleutelmateriaal uit gewist wanneer de inrichting van een HSM is beëindigd?

Voordat u aanvraagt ongedaan maken van inrichting, moet de HSM Gemalto opgegeven HSM clienthulpprogramma's gebruiken op een klant hebt zeroized.

## <a name="performance-and-scale"></a>Prestaties en schaal

### <a name="q-how-many-cryptographic-operations-are-supported-per-second-with-dedicated-hsm"></a>V: Hoeveel cryptografische bewerkingen per seconde met toegewezen HSM worden ondersteund?

Toegewezen HSM bepalingen SafeNet netwerk-HSM-7-apparaten (model A790). Hier volgt een samenvatting van de maximale prestaties voor bepaalde bewerkingen: 

* RSA-2048: 10.000 transacties per seconde
* ECC P256: 20.000 transacties per seconde
* AES-GCM: 17.000 transacties per seconde

### <a name="q-how-many-partitions-can-be-created-in-dedicated-hsm"></a>V: Het aantal partities kunnen worden gemaakt in de toegewezen HSM?

Het SafeNet Luna HSM-7-model dat a790 gebruikt bevat een licentie voor 10 partities in de kosten van de service. Het apparaat heeft een limiet van 100 partities en toe te voegen partities tot deze limiet zou kosten voor extra licenties en installatie van een nieuwe licentiebestand op het apparaat vereist.

### <a name="q-how-many-keys-can-be-supported-in-dedicated-hsm"></a>V: Het aantal sleutels kunnen worden ondersteund in toegewezen HSM?

Het maximum aantal sleutels is een functie van het beschikbare geheugen. Het model SafeNet Luna 7 A790 in gebruik is 32MB aan geheugen. De volgende getallen zijn ook van toepassing op paren van sleutels als asymmetrische sleutels.

* RSA-2048 - 19.000
* ECC-P256 - 91,000

Capaciteit varieert, afhankelijk van bepaalde belangrijke kenmerken instellen in de sjabloon voor genereren van sleutels en het aantal partities.
