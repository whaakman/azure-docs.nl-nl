---
title: Over het installeren van HANA op SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Over het installeren van HANA op SAP HANA op Azure (grote instantie).
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 76a7ce99799b85d81aa6e127ebe1e57e2df3e59a
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44164723"
---
# <a name="example-of-an-sap-hana-installation-on-hana-large-instances"></a>Voorbeeld van een installatie van de SAP HANA op grote HANA-instanties

In deze sectie wordt beschreven hoe SAP HANA installeren op een eenheid HANA grote instantie. De status gestart die we hebben er als volgt uitzien:

- U hebt opgegeven Microsoft de gegevens voor het implementeren van een SAP HANA grote instantie.
- U hebt de SAP HANA grote instantie van Microsoft hebt ontvangen.
- U hebt een Azure-VNet die is verbonden met uw on-premises netwerk hebt gemaakt.
- U hebt verbinding het circuit ExpressRotue voor HANA grote instanties met de dezelfde Azure-VNet.
- U hebt een Azure-VM die u als een jump-box voor HANA grote instanties gebruikt geïnstalleerd.
- U hebt ervoor gezorgd dat u verbinding vanuit de jump-box op uw machine HANA grote instantie en vice versa maken kunt.
- U hebt gecontroleerd of alle vereiste pakketten en patches worden geïnstalleerd.
- U leest de SAP-opmerkingen en documentatie met betrekking tot HANA-installatie op het besturingssysteem u gebruikt en u hebt ervoor gezorgd dat de HANA-release van keuze wordt ondersteund voor het besturingssysteem release.

Wat wordt weergegeven in de volgende reeksen is het downloaden van de HANA-installatiepakketten voor de VM, in dit geval wordt uitgevoerd op een Windows-besturingssysteem, het exemplaar van de pakketten aan de eenheid HANA grote instantie en de volgorde van de installatie van de jump-box.

## <a name="download-of-the-sap-hana-installation-bits"></a>Het downloaden van de SAP HANA-installatie-bits
Aangezien de eenheden HANA grote instantie geen directe verbinding met internet hebben, niet kan u de installatiepakketten rechtstreeks downloaden vanaf SAP voor HANA grote instantie VM. Om te strijden tegen de ontbrekende directe verbinding met internet, moet u de jump-box. U downloaden de pakketten naar de jump-box VM.

De installatiepakketten HANA downloaden, moet u een SAP-S-gebruiker of een andere gebruiker, waardoor u toegang krijgt tot de SAP-Marketplace. Met deze reeks schermen volgen na het aanmelden:

Ga naar [SAP Service Marketplace](https://support.sap.com/en/index.html) > Klik op downloaden van Software >-installaties en -Upgrade > door alfabetische > onder H: SAP HANA-Platform Edition > SAP HANA-Platform Edition 2.0 > installatie > downloaden de volgende bestanden

![HANA-installatie downloaden](./media/hana-installation/image16_download_hana.PNG)

In het geval demonstratie hebben we SAP HANA 2.0 installatiepakketten gedownload. In de Azure jump-box VM uitbreiden u de zelfuitpakkend archieven naar de map zoals hieronder wordt weergegeven.

![Uitpakken van HANA-installatie](./media/hana-installation/image17_extract_hana.PNG)

Als het archief zijn uitgepakt, Kopieer de map die zijn gemaakt door de extractie, in het geval hierboven 51052030, aan de HANA grote instantie-eenheid in het volume /hana/shared in een map die u hebt gemaakt.

> [!Important]
> Kopieer de installatiepakketten niet in de hoofdmap of opstartinstallatiekopieën LUN, omdat er ruimte is beperkt en moet worden gebruikt door andere processen ook.


## <a name="install-sap-hana-on-the-hana-large-instance-unit"></a>SAP HANA installeren op de eenheid HANA grote instantie
Als u wilt installeren SAP HANA, moet u aanmelden als gebruiker hoofdmap. Alleen basis heeft onvoldoende machtigingen voor het installeren van SAP HANA.
Het eerste wat dat u moet doen is het instellen van machtigingen op de map die u in/hana/gedeelde hebt gekopieerd. De machtigingen nodig om in te stellen, zoals

```
chmod –R 744 <Installation bits folder>
```

Als u wilt voor het installeren van SAP HANA met behulp van de grafische installatie, moet het pakket gtk2 worden geïnstalleerd op de HANA grote instanties. Controleer of deze is geïnstalleerd met de opdracht

```
rpm –qa | grep gtk2
```

In de verdere stappen, zijn we aan te tonen de SAP HANA-installatie met behulp van de grafische gebruikersinterface. Als volgende stap gaat u naar de installatiemap en navigeer naar de map sub HDB_LCM_LINUX_X86_64. Starten

```
./hdblcmgui 
```
uit die map. Nu ophalen u geleid via een reeks schermen waar u nodig hebt om te zorgen dat de gegevens voor de installatie. In het geval is aangetoond dat installeert we de SAP HANA-database-server en de SAP HANA client-onderdelen. Onze selectie is daarom 'SAP HANA-Database, zoals hieronder wordt weergegeven

![HANA in installatie selecteren](./media/hana-installation/image18_hana_selection.PNG)

In het volgende scherm kiest u de optie Nieuw systeem installeren

![Selecteer de nieuwe installatie HANA](./media/hana-installation/image19_select_new.PNG)

Na deze stap moet u kiezen tussen verschillende extra onderdelen die verder kunnen worden geïnstalleerd met de SAP HANA-database-server.

![Aanvullende HANA onderdelen selecteren](./media/hana-installation/image20_select_components.PNG)

We hebben gekozen ten behoeve van deze documentatie wordt de SAP HANA-Client en de SAP HANA Studio. We ook een exemplaar omhoog geïnstalleerd. daarom in het volgende scherm moet u kiezen één Host-systeem 

![Selecteer de installatie van de scale-up](./media/hana-installation/image21_single_host.PNG)

In het volgende scherm moet u enkele gegevens opgeven

![Bieden van SAP HANA-SID](./media/hana-installation/image22_provide_sid.PNG)

> [!Important]
> Als HANA systeem-ID (SID), moet u voor de dezelfde SID, als u Microsoft verstrekt wanneer u de implementatie van HANA grote instantie besteld. Een andere SID is de installatie mislukken vanwege problemen met toegang tot de machtiging op de verschillende volumes

Als de installatie van gedeelde map die u gebruikt de/hana/directory. In de volgende stap moet u de locaties opgeven voor de HANA-gegevensbestanden en de HANA-logboekbestanden


![Geef HANA Logboeklocatie](./media/hana-installation/image23_provide_log.PNG)

> [!Note]
> U moet definiëren als gegevens en logboekbestanden de volumes die al bij de koppelpunten met de SID die u hebt gekozen in de selectie van het scherm voordat u dit scherm is geleverd. Als de beveiligings-id niet met de naam die u hebt getypt overeen in, in het scherm voordat, gaat u terug en aanpassen van de SID op de waarde die u op de koppelpunten hebt.

De hostnaam van de beoordeling en uiteindelijk pas dit in de volgende stap. 

![Naam van de beoordeling-host](./media/hana-installation/image24_review_host_name.PNG)

In de volgende stap moet u ook gegevens die u naar Microsoft is opgegeven toen u de implementatie van HANA grote instantie besteld op te halen. 

![Geef systeemgebruiker UID en groeps-id](./media/hana-installation/image25_provide_guid.PNG)

> [!Important]
> U moet dezelfde gebruikers-ID van systeem en ID van de groep van de gebruiker opgeven als u Microsoft verstrekt als u de implementatie van de eenheid order. Als u niet de dezelfde id's geven, mislukt de installatie van SAP HANA op de eenheid HANA grote instantie.

In de volgende twee schermen die we niet zichtbaar zijn in deze documentatie, moet u het wachtwoord opgeven voor de gebruiker van de SAP HANA-database en het wachtwoord voor de gebruiker sapadm, die wordt gebruikt voor de SAP-Hostagent die wordt geïnstalleerd als onderdeel van de SAP HANA-datab exemplaar van de as-omgeving.

Na het definiëren van het wachtwoord is een bevestigingsvenster weergegeven. Controleer alle gegevens die worden vermeld en doorgaan met de installatie. U bereiken een scherm met de installatievoortgang waarin de voortgang van de installatie, zoals de onderstaande

![Voortgang van de installatie controleren](./media/hana-installation/image27_show_progress.PNG)

Als de installatie is voltooid, moet u een afbeelding, zoals de volgende uitvoer

![Installatie is voltooid](./media/hana-installation/image28_install_finished.PNG)

Op dit moment moet de SAP HANA-instantie actief en werkend en klaar voor gebruik. U moet geen verbinding maken met het vanuit SAP HANA Studio. Controleer ook of u controleren of de meest recente patches van SAP HANA en toepassen van patches.


**Volgende stappen**

- Raadpleeg [SAP HANA grote instanties hoge beschikbaarheid en herstel na noodgeval op Azure](hana-overview-high-availability-disaster-recovery.md).

