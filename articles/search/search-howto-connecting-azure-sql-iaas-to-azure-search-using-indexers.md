---
title: SQL VM verbinding met Azure Search | Microsoft Docs
description: Versleutelde verbindingen inschakelen en de firewall configureren voor verbindingen met SQL Server op Azure een virtuele machine (VM) van een indexeerfunctie op Azure Search.
services: search
documentationcenter: 
author: HeidiSteen
manager: pablocas
editor: 
ms.assetid: 46e42e0e-c8de-4fec-b11a-ed132db7e7bc
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/23/2017
ms.author: heidist
ms.openlocfilehash: bb61330ba5511955e0da16dcd5b8b19529d0e44b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-connection-from-an-azure-search-indexer-to-sql-server-on-an-azure-vm"></a>Een verbinding van een Azure Search-indexeerfunctie naar SQL Server configureren op een virtuele machine in Azure
Zoals vermeld in [verbinding maken met Azure SQL Database in Azure Search met indexeerfuncties](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#faq), maken indexeerfuncties tegen **SQL Server op Azure Virtual machines** (of **SQL Azure Virtual machines** voor korte) wordt ondersteund door Azure Search, maar er zijn enkele beveiliging gerelateerde vereisten voor het eerst behandelen. 

**De duur van de taak:** ongeveer 30 minuten, ervan uitgaande dat u al een certificaat geïnstalleerd op de virtuele machine.

## <a name="enable-encrypted-connections"></a>Versleutelde verbindingen inschakelen
Azure Search is een versleuteld kanaal voor alle aanvragen van de indexeerfunctie via een openbare internetverbinding vereist. Deze sectie vindt de stappen uit om dit werk.

1. Controleer de eigenschappen van het certificaat om te controleren of dat de naam van het onderwerp is de volledig gekwalificeerde domeinnaam (FQDN) van de Azure VM. U kunt een hulpprogramma zoals CertUtils of de module Certificaten gebruiken om de eigenschappen weer te geven. U kunt de FQDN-naam in van de VM-service-blade Essentials sectie, krijgen de **openbare IP-adres/DNS-naamlabel** veld in de [Azure-portal](https://portal.azure.com/).
   
   * Voor virtuele machines die zijn gemaakt met behulp van de nieuwere **Resource Manager** sjabloon, de FQDN-naam is geformatteerd als `<your-VM-name>.<region>.cloudapp.azure.com`. 
   * Voor oudere virtuele machines die zijn gemaakt als een **klassieke** VM, de FQDN-naam is geformatteerd als `<your-cloud-service-name.cloudapp.net>`. 
2. SQL Server configureren voor gebruik van het certificaat met de Register-Editor (regedit). 
   
    Hoewel SQL Server Configuration Manager vaak voor deze taak gebruikt wordt, kunt u deze niet gebruiken voor dit scenario. Dit vinden niet het geïmporteerde certificaat omdat de FQDN-naam van de virtuele machine in Azure, komt niet overeen met de FQDN-naam zoals wordt bepaald door de virtuele machine (verwijst naar het domein als de lokale computer of het netwerkdomein waaraan deze is gekoppeld). Wanneer namen niet overeenkomen, moet u regedit gebruiken om op te geven van het certificaat.
   
   * Blader in regedit naar deze registersleutel: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`.
     
     De `[MSSQL13.MSSQLSERVER]` onderdeel varieert op basis van de versie en exemplaarnaam. 
   * Stel de waarde van de **certificaat** sleutel op de **vingerafdruk** van het SSL-certificaat dat u hebt geïmporteerd naar de virtuele machine.
     
     Er zijn verschillende manieren waarop u de vingerafdruk sommige beter dan andere. Als u kopieert uit de **certificaten** -module in MMC, u waarschijnlijk opgehaald een onzichtbare toonaangevende teken [zoals beschreven in dit ondersteuningsartikel](https://support.microsoft.com/kb/2023869/), wat resulteert in een fout wanneer u een verbinding probeert . Er bestaan verschillende oplossingen voor het oplossen van dit probleem. De eenvoudigste manier is het backspace via en typ nogmaals het eerste teken van de vingerafdruk van het verwijderen van het eerste teken in het veld sleutelwaarde in regedit. U kunt ook kunt u een ander hulpprogramma voor het kopiëren van de vingerafdruk.
3. Machtigingen verlenen voor het serviceaccount. 
   
    Zorg ervoor dat de SQL Server-serviceaccount de benodigde machtiging van de persoonlijke sleutel van het SSL-certificaat is verleend. Als u deze stap laten corrigeren, worden SQL Server niet gestart. U kunt de **certificaten** -module of **CertUtils** voor deze taak.
4. Start de SQL Server-service opnieuw.

## <a name="configure-sql-server-connectivity-in-the-vm"></a>SQL Server-verbinding in de virtuele machine configureren
Nadat u de versleutelde verbinding vereist voor Azure Search hebt ingesteld, zijn er extra configuratiestappen intrinsieke met SQL Server op Azure Virtual machines. Als u dit nog niet hebt gedaan, wordt de volgende stap is om met behulp van een van deze artikelen configuratie te voltooien:

* Voor een **Resource Manager** VM, Zie [Connect naar een virtuele Machine in Azure met Resource Manager van SQL Server](../virtual-machines/windows/sql/virtual-machines-windows-sql-connect.md). 
* Voor een **klassieke** VM, Zie [verbinden met een SQL Server-VM op de klassieke Azure-](../virtual-machines/windows/classic/sql-connect.md).

In het bijzonder, Raadpleeg de sectie in elk artikel voor 'verbinding maken via internet'.

## <a name="configure-the-network-security-group-nsg"></a>De Netwerkbeveiligingsgroep (NSG) configureren
Het is niet ongebruikelijke voor het configureren van de NSG en bijbehorende Azure-eindpunt of lijst ACL (Access Control) om uw Azure VM toegankelijk maken voor andere partijen. Waarschijnlijk hebben dat u dit nog om toe te staan van uw eigen toepassingslogica verbinding maken met uw SQL Azure virtuele machine hebt gedaan. Het gaat niet anders voor een Azure Search-verbinding met uw SQL Azure virtuele machine. 

De onderstaande koppelingen bieden instructies over het NSG-configuratie voor VM-implementaties. Volg deze instructies voor de Toegangsbeheerlijst van een Azure SEarch-eindpunt op basis van het IP-adres.

> [!NOTE]
> Zie voor achtergrondinformatie, [wat is er een Netwerkbeveiligingsgroep?](../virtual-network/virtual-networks-nsg.md)
> 
> 

* Voor een **Resource Manager** VM, Zie [nsg's maken voor implementaties van ARM](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 
* Voor een **klassieke** VM, Zie [nsg's maken voor klassieke implementaties](../virtual-network/virtual-networks-create-nsg-classic-ps.md).

IP-adressering kan opleveren voor enkele uitdagingen die gemakkelijk te overwinnen zijn als u zich bewust bent van het probleem en mogelijke oplossingen. De resterende secties bevatten aanbevelingen voor het verwerken van problemen met het IP-adressen in de ACL.

#### <a name="restrict-access-to-the-search-service-ip-address"></a>Beperk de toegang tot het IP-adres van de search-service
Het is raadzaam dat u de toegang beperken tot het IP-adres van uw zoekservice in de ACL in plaats van uw SQL Azure VM's in verbindingsaanvragen breed open aangebracht. U kunt het IP-adres gemakkelijk ontdekken ping de FQDN-naam (bijvoorbeeld `<your-search-service-name>.search.windows.net`) van uw zoekservice.

#### <a name="managing-ip-address-fluctuations"></a>IP-adres schommelingen beheren
Als uw search-service heeft slechts één zoekeenheid (dat wil zeggen, één replica en één partitie), wordt het IP-adres wijzigen tijdens het opstarten wordt periodiek onderhoud, ongeldig te maken van een bestaande ACL met IP-adres van uw zoekservice.

Een manier om te voorkomen dat de volgende verbindingsfout is meer dan één replica en een partitie gebruiken in Azure Search. In dat geval de kosten verhoogt, maar deze ook is het IP-adres-probleem opgelost. In Azure Search worden IP-adressen niet gewijzigd wanneer u meer dan één zoekeenheid hebt.

Een tweede benadering is dat de verbinding met mislukken en configureer de ACL's in de NSG opnieuw. Gemiddeld, kunt u IP-adressen wijzigen om de paar weken verwachten. Voor klanten die gecontroleerde indexeren op basis van incidentele, kan deze benadering levensvatbaar zijn.

Een derde levensvatbaar (maar niet met name veilig)-methode is om op te geven van de IP-adresbereik van de Azure-regio waar uw search-service is ingericht. De lijst met IP-adresbereiken waarvan openbare IP-adressen zijn toegewezen aan Azure-resources wordt gepubliceerd op [Azure Datacenter IP ranges](https://www.microsoft.com/download/details.aspx?id=41653). 

#### <a name="include-the-azure-search-portal-ip-addresses"></a>De IP-adressen van Azure Search portal opnemen
Als u de Azure-portal voor het maken van een indexeerfunctie gebruikt, moet Azure Search portal logica ook toegang tot uw SQL Azure virtuele machine tijdens de aanmaakfase. Azure search portal IP-adressen kunnen u vinden door te pingen `stamp2.search.ext.azure.com`.

## <a name="next-steps"></a>Volgende stappen
Met de manier configuratie, kunt u nu een SQL Server op Azure VM opgeven als de gegevensbron voor een Azure Search-indexeerfunctie. Zie [verbinding maken met Azure SQL Database in Azure Search met indexeerfuncties](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) voor meer informatie.

