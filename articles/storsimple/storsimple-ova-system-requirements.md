---
title: Systeemvereisten voor Microsoft Azure StorSimple virtuele matrix | Microsoft Docs
description: Meer informatie over de software- en netwerkvereisten voor uw virtuele StorSimple-matrix
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: ea1d3bca-e71b-453d-aa82-440d2638f5e3
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/14/2017
ms.author: alkohli
ms.openlocfilehash: 5d01523f326bd7e2518bff06e62ae62db8f318d3
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="storsimple-virtual-array-system-requirements"></a>Systeemvereisten voor StorSimple virtuele array
## <a name="overview"></a>Overzicht
Dit artikel worden de belangrijke systeemvereisten voor uw Microsoft Azure StorSimple virtuele matrix en voor de opslag-clients toegang tot de matrix. Het is raadzaam dat u de informatie voordat u uw StorSimple-systeem te implementeren en vervolgens naar deze zo nodig tijdens de implementatie en de volgende bewerking terugverwijzen zorgvuldig te controleren.

De systeemvereisten zijn onder andere:

* **Softwarevereisten voor opslag clients** -beschrijft de ondersteunde virtualisatieplatforms, webbrowsers iSCSI-initiators, SMB-clients, minimale virtueel apparaatvereisten en eventuele bijkomende vereisten voor deze besturingssystemen.
* **Netwerkvereisten voor de StorSimple-apparaat** -bevat informatie over de poorten die moeten worden geopend in uw firewall om toe te staan voor iSCSI-, cloud of management-verkeer.

StorSimple-systeem informatie over de vereisten gepubliceerd in dit artikel is alleen van toepassing op virtuele StorSimple-matrices.

* Voor apparaten 8000 serie, gaat u naar [systeemvereisten voor uw StorSimple 8000 series apparaat](storsimple-system-requirements.md).
* Voor apparaten van de 7000-serie, gaat u naar [systeemvereisten voor uw StorSimple 5000 7000-serie apparaat](http://onlinehelp.storsimple.com/1_StorSimple_System_Requirements).

## <a name="software-requirements"></a>Softwarevereisten
De softwarevereisten omvatten de informatie over de ondersteunde webbrowsers, SMB-versies, virtualisatieplatforms en de vereisten voor de minimale virtueel apparaat.

### <a name="supported-virtualization-platforms"></a>Ondersteunde virtualisatieplatforms
| **Hypervisor** | **Versie** |
| --- | --- |
| Hyper-V |Windows Server 2008 R2 SP1 en hoger |
| VMware ESXi |5.0, 5.5 en 6.0 <br> (6.5 wordt niet ondersteund.) |

> [!IMPORTANT]
> Installeer VMware tools niet op uw virtuele StorSimple-matrix; Dit leidt tot een niet-ondersteunde configuratie.

### <a name="virtual-device-requirements"></a>Vereisten voor virtuele apparaten
| **Onderdeel** | **Vereiste** |
| --- | --- |
| Minimum aantal virtuele processors (kernen) |4 |
| Minimaal geheugen (RAM) |8 GB <br> Voor een bestandsserver, 8 GB voor minder dan 2 miljoen bestanden en 16 GB voor 2-4 miljoen bestanden|
| Schijfruimte<sup>1</sup> |Besturingssysteemschijf - 80 GB <br></br>Gegevensschijf - 500 GB 8 TB |
| Minimum aantal netwerkinterfaces |1 |
| Internetbandbreedte<sup>2</sup> |Minimale bandbreedte die is vereist: 5 Mbps <br> Aanbevolen bandbreedte: 100 Mbps <br> De snelheid van de gegevensoverdracht kan worden geschaald met de Internet-bandbreedte. Bijvoorbeeld duurt 100 GB aan gegevens 2 dagen om over te dragen met 5 Mbps die tot mislukte back-ups leiden kunnen omdat de dagelijkse back-ups zou niet worden voltooid in een dag. Met een bandbreedte van 100 Mbps kan 100 GB aan gegevens worden overgebracht in 2,5 uur.   |

<sup>1</sup> - thin ingericht

<sup>2</sup> -netwerkvereisten kunnen variëren afhankelijk van de dagelijkse wijzigingssnelheid van gegevens. Bijvoorbeeld, als een apparaat een back-up van 10 GB of meer wijzigingen aanbrengen tijdens een dag moet, klikt u vervolgens de dagelijkse back-up via een verbinding met 5 Mbps kan 4,25 uur duren (als de gegevens kan niet worden gecomprimeerd of ontdubbeld).

### <a name="supported-web-browsers"></a>Ondersteunde webbrowsers
| **Onderdeel** | **Versie** | **Aanvullende vereisten/opmerkingen** |
| --- | --- | --- |
| Microsoft Edge |meest recente versie | |
| Internet Explorer |meest recente versie |Getest met Internet Explorer 11 |
| Google Chrome |meest recente versie |Getest met Chrome 46 |

### <a name="supported-storage-clients"></a>Opslag van ondersteunde clients
De volgende softwarevereisten zijn voor de iSCSI-initiators die toegang hebben tot uw virtuele StorSimple-matrix (geconfigureerd als een iSCSI-server).

| **Ondersteunde besturingssystemen** | **Vereiste versie** | **Aanvullende vereisten/opmerkingen** |
| --- | --- | --- |
| Windows Server |2008 R2 SP1, 2012 2012R2 |StorSimple kunt maken voor thin provisioning en volledig ingerichte volumes. Er kan geen gedeeltelijk ingerichte volumes maken. ISCSI-StorSimple-volumes worden alleen ondersteund voor: <ul><li>Eenvoudige volumes op standaardschijven van Windows.</li><li>Windows NTFS voor het formatteren van een volume.</li> |

De volgende softwarevereisten zijn voor de SMB-clients die toegang hebben tot uw virtuele StorSimple-matrix (geconfigureerd als een bestandsserver).

| **SMB-versie** |
| --- |
| SMB 2.x |
| SMB 3.0 |
| SMB 3.02 |

> [!IMPORTANT]
> Niet kopiëren of opslaan van bestanden die zijn beveiligd door Windows Encrypting File System (EFS) op de bestandsserver virtuele StorSimple-matrix; Dit leidt tot een niet-ondersteunde configuratie.


### <a name="supported-storage-format"></a>Opslagindeling ondersteund
Alleen de Azure blok-blob-opslag wordt ondersteund. Pagina-blobs worden niet ondersteund. Meer informatie [over blok-blobs en pagina-blobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs).

## <a name="networking-requirements"></a>Netwerkvereisten
De volgende tabel bevat de poorten die moeten worden geopend in uw firewall om toe te staan voor iSCSI, SMB, cloud of beheer van verkeer. In deze tabel *in* of *inkomende* verwijst naar de richting waarin binnenkomende aanvragen van clients toegang uw apparaat tot. *Uitgaand* of *uitgaande* verwijst naar de richting waarin uw StorSimple-apparaat gegevens extern, afgezien van de implementatie verzendt: bijvoorbeeld uitgaand naar het Internet.

| **Poort nr.<sup>1</sup>** | **In- of uitzoomen** | **Poort-bereik** | **Vereist** | **Opmerkingen bij de** |
| --- | --- | --- | --- | --- |
| TCP 80 (HTTP) |out |WAN |Nee |Uitgaande poort wordt gebruikt voor toegang tot Internet voor het ophalen van updates. <br></br>De webproxy uitgaande is gebruiker worden geconfigureerd. |
| TCP 443 (HTTPS) |out |WAN |Ja |Uitgaande poort wordt gebruikt voor toegang tot gegevens in de cloud. <br></br>De webproxy uitgaande is gebruiker worden geconfigureerd. |
| UDP 53 (DNS) |out |WAN |In sommige gevallen; Zie de opmerkingen. |Deze poort is alleen vereist als u een Internet-gebaseerd DNS-server gebruikt. <br></br> Houd er rekening mee dat als een bestandsserver implementeert, wordt u aangeraden lokale DNS-server. |
| UDP 123 (NTP) |out |WAN |In sommige gevallen; Zie de opmerkingen. |Deze poort is alleen vereist als u een Internet-gebaseerd NTP-server gebruikt.<br></br> Houd er rekening mee dat als u een bestandsserver implementeert, raden wij aan tijd synchroniseren met uw Active Directory-domeincontrollers. |
| TCP 80 (HTTP) |in |LAN |Ja |Dit is de binnenkomende poort voor de lokale gebruikersinterface op de StorSimple-apparaat voor lokaal beheer. <br></br> Houd er rekening mee dat de lokale gebruikersinterface die via HTTP worden automatisch omgeleid naar HTTPS. |
| TCP 443 (HTTPS) |in |LAN |Ja |Dit is de binnenkomende poort voor de lokale gebruikersinterface op de StorSimple-apparaat voor lokaal beheer. |
| TCP-3260 (iSCSI) |in |LAN |Nee |Deze poort wordt gebruikt voor toegang tot gegevens via iSCSI. |

<sup>1</sup> geen poorten voor inkomend verkeer moeten worden geopend op het openbare Internet.

> [!IMPORTANT]
> Zorg ervoor dat de firewall niet wijzigen of ontsleutelen van een SSL-verkeer tussen de StorSimple-apparaat en Azure.
> 
> 

### <a name="url-patterns-for-firewall-rules"></a>URL-patronen voor firewallregels
Netwerkbeheerders kunnen vaak geavanceerde firewall-regels op basis van de URL-patronen voor het filteren van het inkomende en uitgaande verkeer configureren. Uw virtuele matrix en de service Manager voor StorSimple-apparaat, is afhankelijk van andere Microsoft-toepassingen zoals Azure Service Bus, Azure Active Directory-toegangsbeheer, storage-accounts en Microsoft Update-servers. De URL-patronen die zijn gekoppeld aan deze toepassingen kunnen worden gebruikt voor het configureren van firewallregels. Het is belangrijk te weten dat de URL-patronen die zijn gekoppeld aan deze toepassingen kunnen wijzigen. Op zijn beurt hiervoor moet de netwerkbeheerder om te controleren en firewallregels bijwerken voor uw StorSimple als en indien nodig. 

Het is raadzaam dat u de firewallregels voor uitgaand verkeer, op basis van vaste IP-adressen, opneemt in de meeste gevallen StorSimple ingesteld. U kunt echter de onderstaande informatie geavanceerde firewall-regels die nodig zijn voor het maken van beveiligde omgevingen in te stellen.

> [!NOTE]
> 
> * Het apparaat (bron) IP-adressen moet altijd worden ingesteld op alle interfaces voor cloud-netwerk. 
> * De bestemming IP-adressen moet worden ingesteld op [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653).
> 
> 

| URL-patroon | Onderdeel/functionaliteit |
| --- | --- |
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*` <br>`https://login.windows.net`|StorSimple-apparaat Manager-service<br>Access Control Service<br>Azure Service Bus<br>Authentication-Service|
| `http://*.backup.windowsazure.com` |Apparaatregistratie |
| `http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*` |Intrekken van certificaten |
| `https://*.core.windows.net/*`<br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Azure storage-accounts en controle |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com` |Microsoft Update-servers<br> |
| `http://*.deploy.akamaitechnologies.com` |CDN van Akamai |
| `https://*.partners.extranet.microsoft.com/*` |Ondersteuningspakket |
| `http://*.data.microsoft.com ` |Telemetrie-service in Windows, Zie de [update voor klantervaring en diagnostische telemetrie](https://support.microsoft.com/en-us/kb/3068708) |

## <a name="next-steps"></a>Volgende stappen
* [Voorbereiden van de portal voor het implementeren van uw virtuele StorSimple-matrix](storsimple-virtual-array-deploy1-portal-prep.md)
