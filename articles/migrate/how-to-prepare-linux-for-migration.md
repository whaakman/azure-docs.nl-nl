---
title: Virtuele VMware-machines waarop Linux wordt uitgevoerd voor de migratie naar Azure met Azure Migrate-servermigratie voorbereiden | Microsoft Docs
description: Hierin wordt beschreven hoe u een Linux-VM voorbereiden voor migratie naar Azure met Azure Migrate-servermigratie
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 11/14/2018
ms.author: raynew
ms.openlocfilehash: efc410699ef6f4722857c812b38473c8f54b911b
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811789"
---
# <a name="prepare-linux-vmware-vms-for-migration-to-azure"></a>Linux virtuele VMware-machines voorbereiden voor migratie naar Azure 

Dit artikel wordt beschreven hoe virtuele VMware-machines waarop Linux wordt uitgevoerd wanneer u migreren wilt naar Azure met voorbereiden [Azure Migrate](migrate-overview.md). 

> [!NOTE]
> Azure Migrate servermigratie is momenteel in openbare preview. U kunt de bestaande GA-versie van Azure Migrate en VM's beoordelen voor migratie, maar de daadwerkelijk migratie wordt niet ondersteund in de bestaande GA-versie.

Linux-machines als volgt voorbereiden:

1. Installeer de Hyper-V Linux integratieservices. Nieuwere versies van Linux-distributies mogelijk hebt u dit onderdeel wordt standaard geïnstalleerd).
2. De installatiekopie van het Linux-init opnieuw zodat deze de vereiste Hyper-V-stuurprogramma's bevat, en dat de virtuele machine wordt opgestart in Azure (vereist voor sommige distributies).
3. Logboekregistratie voor het oplossen van de seriële console inschakelen. [Meer informatie](https://docs.microsoft.com/azure/virtual-machines/linux/serial-console).
4. Het toewijzingsbestand apparaat bijwerken met de naam van het apparaat naar volume koppelingen, permanente apparaat-id's gebruiken.
5. Fstab-vermeldingen voor het gebruik van permanent volume-id's worden bijgewerkt.
6. Verwijder de udev-regels die namen van de gebruikersinterface op basis van MAC-adres enzovoort reserveren.
7. Update-netwerkinterfaces voor het ontvangen van DHCP IP-adressen.
8. Zorg ervoor dat ssh is ingeschakeld. Controleer of de sshd-service is ingesteld op automatisch starten bij opnieuw opstarten.
9. Zorg ervoor dat binnenkomende ssh verbindingsaanvragen worden niet geblokkeerd door de firewall van besturingssysteem, of het IP-regels.

[Meer informatie](https://docs.microsoft.com/azure/virtual-machines/linux/serial-console) over het maken van deze wijzigingen in de meest populaire Linux-distributies.

## <a name="sample-script"></a>Voorbeeldscript

Met dit script (voorbereiden-voor-azure.sh) bevat een voorbeeld ter voorbereiding op Linux-machines. Het script kan niet worden gebruikt voor alle distributies en omgevingen, maar het is een handig startpunt.

Het script wordt getoond hoe u: 

- De Linux-init-installatiekopie met de benodigde stuurprogramma's indien nodig opnieuw te genereren.
- Fstab-vermeldingen voor het gebruik van permanent volume-id's worden bijgewerkt.
- Consolelogboeken omleiden naar de seriële poort.
- Seriële toegang tot Azure-console inschakelen
- Udev net regels verwijderen
- Injecteer een uitvoering op opstarten-script dat wordt uitgevoerd wanneer de virtuele machine wordt opgestart. Er wordt gecontroleerd of de machine wordt uitgevoerd in Azure. Als dit het geval is, werkt de netwerkconfiguratie op de virtuele machine en Hiermee stelt u de eerste Ethernet-interface met DHCP een IP-adres hebt aanschaft.

### <a name="before-you-start"></a>Voordat u begint

- Het voorbeeldscript bevat een voorbeeld van de stappen. Deze mag niet worden uitgevoerd op productiesystemen. Het kan schade of beschadigd raken de virtuele machine waarop deze wordt uitgevoerd.
- U wordt aangeraden dat u uitvoeren op een test-VM. Voordat u begint, neemt u een back-up van virtuele machine of momentopname, zodat dat u de virtuele machine herstellen kunt indien nodig. 
- Het script werkt wanneer de virtuele machine wordt uitgevoerd op een van deze Linux-distributies:
    - Red Hat Enterprise Linux 6.5+, 7.1+
    - Cent OS 6.5 +, 7.1 +
    - SUSE Linux Enterprise Server 12 SP1,SP2, SP3
    - Ubuntu 14.04, 16.04, 18.04
    - Debian 7, 8

### <a name="run-the-script"></a>Het script uitvoeren

1. Kopieer het script naar de Linux virtuele machine met behulp van de sftp- of een scp-client, zoals FileZilla of WinScp testen.
2. Voeg SSH toe aan de Linux-machine met een beheerdersaccount.
3. Ga naar de map script.
4. Uitvoeren als u het script in een uitvoerbaar bestand, **sudo chmod 777 voorbereiden-voor-azure.sh**.
5. Voer het script met **./prepare-for-azure.sh**.

Hier ziet u hoe het script wordt uitgevoerd:

![Linux-script](./media/how-to-prepare-linux-for-migration/script1.png)
![Linux script](./media/how-to-prepare-linux-for-migration/script2.png)
![Linux script](./media/how-to-prepare-linux-for-migration/script3.png)
![Linux script](./media/how-to-prepare-linux-for-migration/script4.png)
![Linux script](./media/how-to-prepare-linux-for-migration/script5.png)
![Linux-script ](./media/how-to-prepare-linux-for-migration/script6.png)
 ![Linux script](./media/how-to-prepare-linux-for-migration/script7.png)
![Linux script](./media/how-to-prepare-linux-for-migration/script8.png)
![Linux script](./media/how-to-prepare-linux-for-migration/script9.png)
![Linux script](./media/how-to-prepare-linux-for-migration/script10.png)
![Linux-script ](./media/how-to-prepare-linux-for-migration/script11.png)
 ![Linux script](./media/how-to-prepare-linux-for-migration/script12.png)
![Linux script](./media/how-to-prepare-linux-for-migration/script13.png)
![Linux script](./media/how-to-prepare-linux-for-migration/script14.png)
![Linux script](./media/how-to-prepare-linux-for-migration/script15.png)
![Linux-script ](./media/how-to-prepare-linux-for-migration/script16.png)
 ![Linux script](./media/how-to-prepare-linux-for-migration/script17.png)
![Linux script](./media/how-to-prepare-linux-for-migration/script18.png)
![Linux script](./media/how-to-prepare-linux-for-migration/script19.png)
![Linux script](./media/how-to-prepare-linux-for-migration/script20.png)
![Linux-script ](./media/how-to-prepare-linux-for-migration/script21.png)
 ![Linux script](./media/how-to-prepare-linux-for-migration/script22.png)
![Linux script](./media/how-to-prepare-linux-for-migration/script23.png)
![Linux script](./media/how-to-prepare-linux-for-migration/script24.png)
![Linux-script](./media/how-to-prepare-linux-for-migration/script25.png)



## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het gebruik van [machine afhankelijkheidstoewijzing](how-to-create-group-machine-dependencies.md) te maken van groepen met hoge betrouwbaarheid.
- [Meer informatie](concepts-assessment-calculation.md) over hoe evaluaties worden berekend.
