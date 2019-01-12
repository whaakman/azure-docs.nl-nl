---
title: Systeemvereisten voor Microsoft Azure StorSimple Virtual Array | Microsoft Docs
description: Meer informatie over de software en netwerkvereisten voor uw StorSimple Virtual Array
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ea1d3bca-e71b-453d-aa82-440d2638f5e3
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/11/2019
ms.author: alkohli
ms.openlocfilehash: 7e5cf79613bdbd62427e99a0d1f2aa29ed8f85be
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54245188"
---
# <a name="storsimple-virtual-array-system-requirements"></a>Systeemvereisten voor StorSimple virtuele array
## <a name="overview"></a>Overzicht
Dit artikel beschrijft de belangrijke systeemvereisten voor uw Microsoft Azure StorSimple Virtual Array en voor de opslagclients toegang tot de matrix. Het is raadzaam dat u de informatie voordat u uw StorSimple-systeem implementeert, en vervolgens terug naar deze zo nodig tijdens de implementatie en latere bewerking verwijzen zorgvuldig te controleren.

De systeemvereisten zijn onder andere:

* **Softwarevereisten voor opslagclients** -beschrijving van de ondersteunde virtualisatieplatforms, webbrowsers, iSCSI-initiators, SMB-clients, minimale virtueel apparaatvereisten en eventuele bijkomende vereisten voor deze besturingssystemen.
* **Netwerkvereisten voor de StorSimple-apparaat** -vindt u informatie over de poorten die moeten worden geopend in uw firewall om toe te staan voor iSCSI-, cloud of beheer van verkeer.

StorSimple-systeem informatie over de vereisten in dit artikel is gepubliceerd geldt alleen voor virtuele StorSimple-matrices.

* Voor apparaten van de 8000-serie, gaat u naar [systeemvereisten voor de StorSimple 8000-apparaat](storsimple-system-requirements.md).
* Voor apparaten van de 7000-serie, gaat u naar [systeemvereisten voor StorSimple 5000-7000-serie-apparaat](http://onlinehelp.storsimple.com/1_StorSimple_System_Requirements).

## <a name="software-requirements"></a>Softwarevereisten
De softwarevereisten omvatten de informatie over de ondersteunde webbrowsers, SMB-versies, virtualisatieplatforms en de vereisten voor minimale virtuele apparaten.

### <a name="supported-virtualization-platforms"></a>Ondersteunde virtualisatieplatforms
| **Hypervisor** | **Versie** |
| --- | --- |
| Hyper-V |Windows Server 2008 R2 SP1 en hoger |
| VMware ESXi |5.0, 5.5, 6.0 en 6.5. |

> [!IMPORTANT]
> VMware-hulpprogramma's niet te installeren op uw StorSimple Virtual Array; Dit zal resulteren in een niet-ondersteunde configuratie.

### <a name="virtual-device-requirements"></a>Vereisten voor virtuele apparaten
| **Onderdeel** | **Vereiste** |
| --- | --- |
| Minimum aantal virtuele processors (kernen) |4 |
| Minimaal geheugen (RAM) |8 GB <br> Voor een bestandsserver, 8 GB voor minder dan 2 miljoen bestanden en 16 GB 2-4 miljoen bestanden|
| Schijfruimte<sup>1</sup> |De schijf met besturingssysteem - 80 GB <br></br>Gegevensschijf - 500 GB tot 8 TB |
| Minimum aantal netwerkinterfaces |1 |
| Internetbandbreedte<sup>2</sup> |Minimale bandbreedte vereist: 5 Mbps <br> Aanbevolen bandbreedte: 100 Mbps <br> De snelheid van data transfer worden geschaald met de Internet-bandbreedte. 100 GB aan gegevens heeft bijvoorbeeld twee dagen om over te dragen met 5 Mbps die tot mislukte back-ups leiden kunnen omdat de dagelijkse back-ups kunnen niet worden voltooid in een dag. Met een bandbreedte van 100 Mbps, kunt u 100 GB aan gegevens overbrengen in 2,5 uur.   |

<sup>1</sup> - thin ingericht

<sup>2</sup> -netwerkvereisten kunnen variëren afhankelijk van de dagelijkse veranderingssnelheid van gegevens. Bijvoorbeeld, als een apparaat een back-up 10 GB of meer wijzigingen gedurende een dag moet, klikt u vervolgens de dagelijkse back-up via een verbinding met 5 Mbps kan 4,25 uur duren (als de gegevens kan niet worden gecomprimeerd of ontdubbeld).

### <a name="supported-web-browsers"></a>Ondersteunde webbrowsers
| **Onderdeel** | **Versie** | **Aanvullende vereisten/opmerkingen** |
| --- | --- | --- |
| Microsoft Edge |Meest recente versie | |
| Internet Explorer |Meest recente versie |Getest met Internet Explorer 11 |
| Google Chrome |Meest recente versie |Getest met Chrome 46 |

### <a name="supported-storage-clients"></a>Ondersteunde opslagclients
De volgende vereisten zijn voor de iSCSI-initiators die toegang hebben tot uw StorSimple Virtual Array (geconfigureerd als een iSCSI-server).

| **Ondersteunde besturingssystemen** | **Vereiste versie** | **Aanvullende vereisten/opmerkingen** |
| --- | --- | --- |
| Windows Server |2008R2 SP1, 2012, 2012R2 |StorSimple kunt maken dun ingerichte en volledig ingerichte volumes. Er kan geen gedeeltelijk ingerichte volumes maken. StorSimple iSCSI-volumes worden alleen ondersteund voor: <ul><li>Eenvoudige volumes op standaardschijven van Windows.</li><li>Windows NTFS voor het opmaken van een volume.</li> |

De volgende vereisten zijn voor de SMB-clients die toegang hebben tot uw StorSimple Virtual Array (geconfigureerd als een bestandsserver).

| **SMB-versie** |
| --- |
| SMB 2.x |
| SMB 3.0 |
| SMB 3.02 |

> [!IMPORTANT]
> Niet kopiëren of opslaan van bestanden die zijn beveiligd door Windows Encrypting File System (EFS) op de bestandsserver van de StorSimple Virtual Array; Dit zal resulteren in een niet-ondersteunde configuratie.


### <a name="supported-storage-format"></a>Opslagindeling ondersteund
Alleen de Azure block blob-opslag wordt ondersteund. Pagina-blobs worden niet ondersteund. Meer informatie [over blok-blobs en pagina-blobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs).

## <a name="networking-requirements"></a>Netwerkvereisten
De volgende tabel staan de poorten die moeten worden geopend in uw firewall om toe te staan voor iSCSI, SMB, cloud of beheer van verkeer. In deze tabel *in* of *inkomende* verwijst naar de richting waarin binnenkomende aanvragen van clients toegang uw apparaat tot. *Uit* of *uitgaande* verwijst naar de richting waarin uw StorSimple-apparaat gegevens extern, na de implementatie verzendt: bijvoorbeeld uitgaand naar het Internet.

| **Nee. de poort<sup>1</sup>** | **In- of uitschalen** | **Poort-bereik** | **Vereist** | **Opmerkingen** |
| --- | --- | --- | --- | --- |
| TCP 80 (HTTP) |Uit |WAN |Nee |Uitgaande poort wordt gebruikt voor toegang tot Internet om op te halen van updates. <br></br>De webproxy uitgaande is gebruiker worden geconfigureerd. |
| TCP 443 (HTTPS) |Uit |WAN |Ja |Uitgaande poort wordt gebruikt voor toegang tot gegevens in de cloud. <br></br>De webproxy uitgaande is gebruiker worden geconfigureerd. |
| UDP 53 (DNS) |Uit |WAN |In sommige gevallen; Zie de opmerkingen. |Deze poort is alleen vereist als u van een op basis van een Internet-DNS-server gebruikmaakt. <br></br> Houd er rekening mee dat als een bestandsserver implementeert, wordt u aangeraden lokale DNS-server. |
| UDP 123 (NTP) |Uit |WAN |In sommige gevallen; Zie de opmerkingen. |Deze poort is alleen vereist als u een Internet-gebaseerde NTP-server gebruikt.<br></br> Houd er rekening mee dat als u een bestandsserver implementeert, het beste tijd synchroniseren met uw Active Directory-domeincontrollers. |
| TCP 80 (HTTP) |In |LAN |Ja |Dit is de binnenkomende poort voor lokale gebruikersinterface op het StorSimple-apparaat voor lokaal beheer. <br></br> Houd er rekening mee dat toegang tot de gebruikersinterface voor het lokale via HTTP worden automatisch omgeleid naar HTTPS. |
| TCP 443 (HTTPS) |In |LAN |Ja |Dit is de binnenkomende poort voor lokale gebruikersinterface op het StorSimple-apparaat voor lokaal beheer. |
| TCP-3260 (iSCSI) |In |LAN |Nee |Deze poort wordt gebruikt voor toegang tot gegevens via iSCSI. |

<sup>1</sup> geen inkomende poorten moeten worden geopend op het openbare Internet.

> [!IMPORTANT]
> Zorg ervoor dat de firewall niet wijzigen of ontsleutelen van een SSL-verkeer tussen de StorSimple-apparaat en Azure.
> 
> 

### <a name="url-patterns-for-firewall-rules"></a>URL-patronen voor firewall-regels
Netwerkbeheerders kunnen vaak geavanceerde firewall-regels op basis van de URL-patronen voor het filteren van de inkomende en uitgaande verkeer configureren. Uw virtuele matrix en de StorSimple Device Manager-service, is afhankelijk van andere Microsoft-toepassingen, zoals Azure Service Bus, toegangsbeheer van Azure Active Directory, opslagaccounts en Microsoft Update-servers. De URL-patronen die zijn gekoppeld aan deze toepassingen kunnen worden gebruikt om de firewall-regels configureren. Het is belangrijk om te begrijpen dat de URL-patronen die zijn gekoppeld aan deze toepassingen kunnen wijzigen. Op zijn beurt hiervoor moet de netwerkbeheerder om te controleren en firewallregels bijwerken voor uw StorSimple als en wanneer dat nodig is. 

U wordt aangeraden dat u uw firewall-regels voor uitgaand verkeer, op basis van vaste IP-adressen, opneemt in de meeste gevallen StorSimple instellen. Echter, kunt u de onderstaande informatie om in te stellen van geavanceerde firewallregels die nodig zijn om beveiligde omgevingen te maken.

> [!NOTE]
> 
> * Het apparaat (bron) IP-adressen moet altijd worden ingesteld op alle netwerkinterfaces ingeschakeld voor de cloud. 
> * De IP-adressen moet worden ingesteld op bestemming [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653).
> 
> 

| URL-patroon | Onderdeel/functionaliteit |
| --- | --- |
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*` <br>`https://login.windows.net`|StorSimple-apparaatbeheerservice<br>Access Control Service<br>Azure Service Bus<br>Verificatieservice|
| `http://*.backup.windowsazure.com` |Apparaatregistratie |
| `http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*` |Het intrekken van certificaten |
| `https://*.core.windows.net/*`<br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Azure storage-accounts en bewaking |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com` |Microsoft Update-servers<br> |
| `http://*.deploy.akamaitechnologies.com` |Akamai CDN |
| `https://*.partners.extranet.microsoft.com/*` |Ondersteuningspakket |
| `http://*.data.microsoft.com ` |Telemetrieservice in Windows, Zie de [update voor de gebruikerservaring en diagnostische telemetrie](https://support.microsoft.com/en-us/kb/3068708) |

## <a name="next-steps"></a>Volgende stappen
* [Voorbereiden van de portal voor het implementeren van uw StorSimple Virtual Array](storsimple-virtual-array-deploy1-portal-prep.md)
