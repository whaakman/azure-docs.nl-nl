---
title: Over het installeren van HANA op SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Over het installeren van HANA op SAP HANA op Azure (grote instanties).
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
ms.openlocfilehash: f4629894933507bda7359fb034c4079d38100029
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50231403"
---
# <a name="install-hana-on-sap-hana-on-azure-large-instances"></a>HANA installeren op SAP HANA op Azure (grote instanties)

Als u wilt installeren HANA op SAP HANA op Azure (grote instanties), moet u eerst het volgende doen:
- U voorzien Microsoft van de gegevens op te implementeren voor u een SAP HANA grote instantie.
- U ontvangt de SAP HANA grote instantie van Microsoft.
- U maakt een Azure-netwerk dat is verbonden met uw on-premises netwerk.
- U kunt het ExpressRoute-circuit voor HANA grote instanties verbinding met hetzelfde Azure virtual network.
- U installeert een Azure-machine die u als een jump-box voor HANA grote instanties gebruikt.
- U ervoor zorgen dat u vanuit de jump-box met uw eenheid HANA grote instantie verbinden kunt, en vice versa.
- U controleren of de vereiste pakketten en patches worden geïnstalleerd.
- U de SAP-opmerkingen en documentatie over de HANA-installatie op het besturingssysteem die u hebt gelezen. Zorg ervoor dat de HANA-release van keuze in de release van het besturingssysteem wordt ondersteund.

De volgende sectie ziet u een voorbeeld van het downloaden van de HANA-installatiepakketten aan de sprong in het virtuele machine. In dit geval is het besturingssysteem Windows.

## <a name="download-the-sap-hana-installation-bits"></a>Downloaden van de SAP HANA-installatie-bits
De eenheden HANA grote instantie worden niet rechtstreeks verbonden met internet. U downloaden niet de installatiepakketten rechtstreeks van SAP HANA grote instantie virtual machine. U kunt in plaats daarvan de pakketten downloaden aan de sprong in het virtuele machine.

U moet een SAP-S-gebruiker of een andere gebruiker, waardoor u toegang krijgt tot de SAP-Marketplace.

1. Meld u aan en gaat u naar [SAP Service Marketplace](https://support.sap.com/en/index.html). Selecteer **Software downloaden** > **-installaties en -Upgrade** > **door alfabetische**. Selecteer vervolgens **onder H: SAP HANA-Platform Edition** > **SAP HANA-Platform Edition 2.0** > **installatie**. Download de bestanden in de volgende schermafbeelding wordt weergegeven.

   ![Schermafbeelding van de bestanden te downloaden](./media/hana-installation/image16_download_hana.PNG)

2. In dit voorbeeld hebben we SAP HANA 2.0 installatiepakketten gedownload. In de Azure gaat u in het virtuele machine, de zelfuitpakkend archieven uitbreiden naar de map, zoals hieronder weergegeven.

   ![Schermafbeelding van zichzelf uitpakkend archief](./media/hana-installation/image17_extract_hana.PNG)

3. Als het archief zijn uitgepakt, Kopieer de map die zijn gemaakt door de extractie (in dit geval 51052030). Kopieer de map van het volume /hana/shared HANA grote instantie in een map die u hebt gemaakt.

   > [!Important]
   > Kopieer de installatiepakketten niet in de hoofdmap of opstartinstallatiekopieën LUN, omdat er ruimte is beperkt en moet worden gebruikt door andere processen ook.


## <a name="install-sap-hana-on-the-hana-large-instance-unit"></a>SAP HANA installeren op de eenheid HANA grote instantie
Wilt u SAP HANA installeren, moet u zich aanmelden als gebruiker root. Alleen basis heeft onvoldoende machtigingen voor het installeren van SAP HANA. Machtigingen instellen voor de map die u in /hana/shared hebt gekopieerd.

```
chmod –R 744 <Installation bits folder>
```

Als u installeren van SAP HANA wilt met behulp van de grafische interface-installatie, moet het pakket gtk2 op HANA grote instanties worden geïnstalleerd. Als u wilt controleren of deze is geïnstalleerd, moet u de volgende opdracht uitvoeren:

```
rpm –qa | grep gtk2
```

(In de volgende stappen laten we zien de SAP HANA-installatie met behulp van de grafische gebruikersinterface.)

Ga naar de installatiemap en gaat u naar de map sub HDB_LCM_LINUX_X86_64. 

Uit die map starten:

```
./hdblcmgui 
```
Op dit punt, uitgevoerd via een reeks schermen waarin u de gegevens voor de installatie opgeven. In dit voorbeeld wordt de SAP HANA-database-server en de SAP HANA client-onderdelen installeert. Onze selectie is daarom **SAP HANA-Database**.

![Schermafbeelding van SAP HANA Lifecycle Management scherm, met SAP HANA-Database geselecteerd](./media/hana-installation/image18_hana_selection.PNG)

Selecteer op het volgende scherm **nieuw systeem installeren**.

![Schermafbeelding van SAP HANA Lifecycle Management scherm, installeren nieuwe systeem geselecteerd](./media/hana-installation/image19_select_new.PNG)

Selecteer vervolgens onder verschillende aanvullende onderdelen die u kunt installeren.

![Schermafbeelding van SAP HANA Lifecycle Management scherm, met de lijst van extra onderdelen](./media/hana-installation/image20_select_components.PNG)

Hier kiezen we de SAP HANA-Client en de SAP HANA Studio. We ook installeren een exemplaar omhoog. Kies vervolgens **één hostsysteem**. 

![Schermafbeelding van SAP HANA Lifecycle Management scherm, met één hostsysteem geselecteerd](./media/hana-installation/image21_single_host.PNG)

Geef vervolgens de gegevens.

![Schermafbeelding van SAP HANA Lifecycle Management scherm, met eigenschappen systeemvelden definiëren](./media/hana-installation/image22_provide_sid.PNG)

> [!Important]
> Als HANA systeem-ID (SID), moet u de dezelfde SID als u Microsoft verstrekt wanneer u de implementatie van HANA grote instantie besteld opgeven. Kies een andere SID zorgt ervoor dat de installatie is mislukt vanwege problemen met toegang tot de machtiging op de verschillende volumes.

Gebruik de map /hana/shared voor het installatiepad. In de volgende stap biedt u de locaties voor de HANA-gegevensbestanden en de logboekbestanden van HANA.


![Schermafbeelding van SAP HANA Lifecycle Management scherm, gegevens en logboekbestanden gebied velden](./media/hana-installation/image23_provide_log.PNG)

> [!Note]
> De SID die u hebt opgegeven toen u Systeemeigenschappen (twee schermen geleden) hebt gedefinieerd, moet overeenkomen met de SID van de koppelpunten. Als er een niet-overeenkomend, gaat u terug en aanpassen van de SID op de waarde die u op de koppelpunten hebt.

De hostnaam van de beoordeling en uiteindelijk pas dit in de volgende stap. 

![Schermafbeelding van SAP HANA Lifecycle Management scherm, met de naam van host](./media/hana-installation/image24_review_host_name.PNG)

In de volgende stap moet u ook gegevens die u naar Microsoft is opgegeven toen u de implementatie van HANA grote instantie besteld op te halen. 

![Schermafbeelding van SAP HANA Lifecycle Management, met de beheerder systeemvelden definiëren](./media/hana-installation/image25_provide_guid.PNG)

> [!Important]
> Geef dezelfde **systeembeheerder gebruikers-ID** en **-ID van de groep van de gebruiker** zoals u hebt opgegeven voor Microsoft, als u de implementatie van de eenheid bestellen. Anders mislukt de installatie van SAP HANA op de eenheid HANA grote instantie.

De volgende twee schermen worden niet hier weergegeven. Hiermee kunt u voor het wachtwoord voor de gebruiker van de SAP HANA-database en het wachtwoord voor de gebruiker sapadm. De laatste wordt gebruikt voor de SAP-Hostagent die wordt geïnstalleerd als onderdeel van het exemplaar van de SAP HANA-database.

Na het definiëren van het wachtwoord, ziet u een bevestigingsvenster. Controleer alle gegevens die worden vermeld en doorgaan met de installatie. U bereikt een scherm met de installatievoortgang waarin de voortgang van de installatie, zoals deze:

![Schermafbeelding van SAP HANA Lifecycle Management scherm, met de installatie uitgevoerd indicatoren](./media/hana-installation/image27_show_progress.PNG)

Als de installatie is voltooid, ziet u een scherm zoals dit:

![Schermafbeelding van SAP HANA Lifecycle Management scherm, waarmee wordt aangegeven installatie is voltooid](./media/hana-installation/image28_install_finished.PNG)

De SAP HANA-instantie moet nu actief en werkend en klaar voor gebruik. U moet geen verbinding maken met het vanuit SAP HANA Studio. Controleer ook of u controleren en de meest recente updates toepassen.


## <a name="next-steps"></a>Volgende stappen

- [SAP HANA grote instanties hoge beschikbaarheid en herstel na noodgeval op Azure](hana-overview-high-availability-disaster-recovery.md)

