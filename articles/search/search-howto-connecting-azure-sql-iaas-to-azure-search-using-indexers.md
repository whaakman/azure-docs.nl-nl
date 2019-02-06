---
title: Zoeken naar Azure SQL VM VM-verbinding voor zoekindexen - Azure
description: Versleutelde verbindingen inschakelen en configureren van de firewall om toe te staan van verbindingen met SQL Server op een Azure-machine (VM) van een indexeerfunctie voor Azure Search.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 02/04/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 90e5a133bac519cbc5ab2d7b112d51a019e8f698
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55751375"
---
# <a name="configure-a-connection-from-an-azure-search-indexer-to-sql-server-on-an-azure-vm"></a>Configureren van een verbinding tussen een Azure Search-indexeerfunctie en SQL Server op een Azure VM
Zoals aangegeven [Azure SQL-Database verbinding te maken in Azure Search met indexeerfuncties](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#faq), het maken van indexeerfuncties tegen **SQL Server op Azure Virtual machines** (of **SQL Azure-VM's** voor korte) wordt ondersteund Azure Search, maar er zijn enkele beveiliging gerelateerde vereisten voor het eerste regelt. 

Verbindingen van Azure Search met SQL Server op een virtuele machine is een openbare internetverbinding. Alle van de beveiligingsmaatregelen die u normaal gesproken voor deze verbindingen volgen ook van toepassing zijn hier:

+ Verkrijgen van een certificaat van een [certificeringsinstantie provider](https://en.wikipedia.org/wiki/Certificate_authority#Providers) voor de volledig gekwalificeerde domeinnaam van de SQL Server-exemplaar op de virtuele machine van Azure.
+ Het certificaat installeren op de virtuele machine, inschakelen en configureren van versleutelde verbindingen op de virtuele machine met behulp van de instructies in dit artikel.

## <a name="enable-encrypted-connections"></a>Versleutelde verbindingen inschakelen
Azure Search is een versleuteld kanaal vereist voor alle indexeerfunctie aanvragen via een openbare internetverbinding. Deze sectie vindt u de stappen uit om dit werk.

1. Controleer de eigenschappen van het certificaat om te controleren of dat de naam van het onderwerp is de volledig gekwalificeerde domeinnaam (FQDN) van de Azure-VM. U kunt een hulpprogramma zoals CertUtils of de module Certificaten gebruiken om de eigenschappen weer te geven. U kunt de FQDN-naam ophalen uit de sectie Essentials van de serviceblade van de virtuele machine, in de **openbaar IP-adres/DNS-naamlabel** veld in de [Azure-portal](https://portal.azure.com/).
   
   * Voor virtuele machines die zijn gemaakt met behulp van de nieuwere **Resource Manager** sjabloon, de FQDN-naam is opgemaakt als `<your-VM-name>.<region>.cloudapp.azure.com`
   * Voor oudere VM's die zijn gemaakt als een **klassieke** VM, de FQDN-naam is opgemaakt als `<your-cloud-service-name.cloudapp.net>`.

2. SQL Server voor het gebruik van het certificaat met behulp van de Register-Editor (regedit) configureren. 
   
    Hoewel SQL Server Configuration Manager vaak voor deze taak gebruikt wordt, kunt u deze niet gebruiken voor dit scenario. Dit niet het geïmporteerde certificaat wordt niet vinden omdat de FQDN-naam van de virtuele machine op Azure komt niet overeen met de FQDN-naam zoals wordt bepaald door de virtuele machine (het identificeert het domein als de lokale computer of de netwerk-domein waaraan deze is gekoppeld). Wanneer de namen komen niet overeen, gebruikt u regedit om op te geven van het certificaat.
   
   * Blader in regedit, naar deze registersleutel: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`.
     
     De `[MSSQL13.MSSQLSERVER]` onderdeel varieert op basis van de versie en exemplaarnaam. 
   * Stel de waarde van de **certificaat** sleutel op de **vingerafdruk** van het SSL-certificaat dat u hebt geïmporteerd naar de virtuele machine.
     
     Er zijn verschillende manieren waarop u de vingerafdruk sommige beter dan andere. Als u vanuit kopieert de **certificaten** -module in MMC-module u waarschijnlijk haalt een onzichtbare toonaangevende teken [zoals beschreven in dit ondersteuningsartikel](https://support.microsoft.com/kb/2023869/), wat resulteert in een fout wanneer u een verbinding probeert . Er bestaan verschillende oplossingen voor het corrigeren van dit probleem. De eenvoudigste manier is het backspace via en typ nogmaals het eerste teken van de vingerafdruk van het verwijderen van het eerste teken in het veld sleutelwaarde in regedit. U kunt ook een ander hulpprogramma gebruiken om te kopiëren van de vingerafdruk van het.

3. Machtigingen verlenen voor het serviceaccount. 
   
    Zorg ervoor dat de dat juiste machtigingen voor de persoonlijke sleutel van het SSL-certificaat is verleend door de SQL Server-serviceaccount. Als u deze stap over het hoofd zien, kan SQL Server niet worden gestart. U kunt de **certificaten** -module of **CertUtils** voor deze taak.
    
4. Start de SQL Server-service opnieuw.

## <a name="configure-sql-server-connectivity-in-the-vm"></a>SQL Server-connectiviteit in de virtuele machine configureren
Na het instellen van de versleutelde verbinding vereist voor Azure Search, zijn er aanvullende configuratiestappen verbetert de intrinsieke SQL Server op Azure Virtual machines. Als u dit nog niet hebt gedaan, wordt de volgende stap is het voltooien van de configuratie met behulp van een van de volgende artikelen:

* Voor een **Resource Manager** VM, Zie [verbinding maken met een SQL Server-VM op Azure met behulp van Resource Manager](../virtual-machines/windows/sql/virtual-machines-windows-sql-connect.md). 
* Voor een **klassieke** VM, Zie [verbinden met een SQL Server-VM in de klassieke Azure-](../virtual-machines/windows/classic/sql-connect.md).

Lees met name de sectie in elk artikel voor 'verbinding maken via internet'.

## <a name="configure-the-network-security-group-nsg"></a>De Netwerkbeveiligingsgroep (NSG) configureren
Het is niet ongebruikelijk dat de NSG en bijbehorende Azure-eindpunt of lijst met ACL (Access Control) zodat uw Azure-VM toegankelijk is voor andere partijen configureren. Waarschijnlijk dat hebt u dit nog gedaan om toe te staan van uw eigen toepassingslogica verbinding maakt met uw SQL Azure-VM. Het is niet anders voor een Azure Search-verbinding met uw SQL Azure-VM. 

De onderstaande koppelingen bevatten instructies voor de NSG-configuratie voor VM-implementaties. Volg deze instructies voor de Toegangsbeheerlijst van een Azure Search-eindpunt op basis van het IP-adres.

> [!NOTE]
> Zie voor achtergrondinformatie [wat is er een Netwerkbeveiligingsgroep?](../virtual-network/security-overview.md)
> 
> 

* Voor een **Resource Manager** VM, Zie [nsg's maken voor ARM-implementaties](../virtual-network/tutorial-filter-network-traffic.md). 
* Voor een **klassieke** VM, Zie [nsg's maken voor klassieke implementaties](../virtual-network/virtual-networks-create-nsg-classic-ps.md).

IP-adressen kan opleveren voor enkele uitdagingen die eenvoudig oplossen zijn als u zich bewust bent van het probleem en mogelijke oplossingen. De resterende secties vindt u aanbevelingen voor het afhandelen van problemen met betrekking tot IP-adressen in de ACL.

#### <a name="restrict-access-to-the-search-service-ip-address"></a>Toegang beperken tot het IP-adres van de search-service
Het is raadzaam dat u de toegang beperken tot het IP-adres van uw zoekservice in de ACL in plaats van uw SQL Azure-VM's breed mogelijk toegankelijk maken voor de verbindingsaanvragen die. U kunt eenvoudig uit het IP-adres vinden door te pingen van de FQDN-naam (bijvoorbeeld `<your-search-service-name>.search.windows.net`) van uw search-service.

#### <a name="managing-ip-address-fluctuations"></a>IP-adres fluctuaties beheren
Als uw search-service heeft slechts één zoekeenheid (dat wil zeggen, een van de replica en één partitie), wordt het IP-adres wijzigen tijdens de routine-service opnieuw is opgestart, ongeldig maken van een bestaande ACL met IP-adres van de search-service.

Een manier om te voorkomen dat de volgende verbindingsfout is meer dan één replica en één partitie gebruiken in Azure Search. In dat geval de kosten verhoogt, maar het ook het IP-adres probleem oplost. In Azure Search geen IP-adressen wanneer u meer dan één zoekeenheid hebt gewijzigd.

Een tweede methode is om de verbinding mislukt, en vervolgens opnieuw de ACL's in de NSG te configureren. Gemiddeld, kunt u IP-adressen te wijzigen om de paar weken verwachten. Voor klanten die gecontroleerde indexeren op basis van incidentele, deze aanpak is wellicht mogelijk.

Een derde levensvatbare (maar niet erg veilig)-aanpak is om op te geven van het IP-adresbereik van de Azure-regio waar uw search-service is ingericht. De lijst met IP-adresbereiken waaruit openbare IP-adressen worden toegewezen aan Azure-resources wordt gepubliceerd op [Azure Datacenter IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653). 

#### <a name="include-the-azure-search-portal-ip-addresses"></a>Neem de portal IP-adressen van Azure Search
Als u een indexeerfunctie maken via de Azure-portal, moet Azure Search portal logische ook toegang tot uw SQL Azure-virtuele machine tijdens het aanmaken. Zoeken in Azure portal IP-adressen kunnen u vinden door te pingen `stamp2.search.ext.azure.com`.

## <a name="next-steps"></a>Volgende stappen
Met zoals configuratie, kunt u nu een SQL-Server op virtuele Azure-machine opgeven als de gegevensbron voor een Azure Search-indexeerfunctie. Zie [Azure SQL-Database verbinding te maken in Azure Search met indexeerfuncties](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) voor meer informatie.

