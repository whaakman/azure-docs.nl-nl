---
title: Instellen van herstel na noodgevallen voor virtuele Azure-machines naar een secundair Azure-regio met Azure Site Recovery (Preview)
description: Informatie over het instellen van herstel na noodgevallen voor virtuele Azure-machines naar een andere Azure-regio, via de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/08/2017
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 3db1ead1f1a8b83cc47f53b915ed54bb78db7ab3
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="set-up-disaster-recovery-for-azure-vms-to-a-secondary-azure-region-preview"></a>Instellen van herstel na noodgevallen voor virtuele Azure-machines naar een secundair Azure-regio (Preview)

De [Azure Site Recovery](site-recovery-overview.md) service draagt bij aan uw strategie voor noodherstel door te beheren en te organiseren replicatie, failover en failback van on-premises machines en virtuele Azure-machines (VM's).

Deze zelfstudie laat zien hoe u voor het instellen van herstel na noodgevallen secundaire Azure-regio voor de virtuele Azure-machines. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Recovery Services-kluis maken
> * Controleer de instellingen van de doel-resource
> * Uitgaande toegang instellen voor virtuele machines
> * Replicatie inschakelen voor een virtuele machine

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- Zorg ervoor dat u begrijpt de [scenario-architectuur en onderdelen](concepts-azure-to-azure-architecture.md).
- Controleer de [ondersteuningsvereisten](site-recovery-support-matrix-azure-to-azure.md) voor alle onderdelen.

## <a name="create-a-vault"></a>Een kluis maken

Maak de kluis in elke regio, met uitzondering van de bron-regio.

1. Meld u aan bij [Azure Portal](https://portal.azure.com) > **Recovery Services**.
2. Klik op **Nieuw** > **Bewaking en beheer** > **Backup en Site Recovery**.
3. Geef in **Naam** een beschrijvende naam op om de kluis mee aan te duiden. Als u meer dan één abonnement hebt, selecteert u het juiste bestand.
4. Een resourcegroep maken of een bestaande set selecteren. Geef een Azure-regio op. Zie Geografische beschikbaarheid in [Prijsinformatie voor Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) om na te gaan welke regio's er worden ondersteund.
5. Voor snelle toegang tot de kluis vanuit het dashboard, klikt u op **vastmaken aan dashboard** en klik vervolgens op **maken**.

   ![Nieuwe kluis](./media/azure-to-azure-tutorial-enable-replication/new-vault-settings.png)

   De nieuwe kluis wordt toegevoegd aan de **Dashboard** onder **alle resources**, en klik op de hoofdblade **Recovery Services-kluizen** pagina.

## <a name="verify-target-resources"></a>Controleer of de doelresources

1. Controleer of uw Azure-abonnement kunt u virtuele machines in de doelregio die wordt gebruikt voor herstel na noodgevallen maken. Neem contact op met ondersteuning voor het inschakelen van de quota voor vereist.

2. Zorg ervoor dat uw abonnement heeft onvoldoende bronnen ter ondersteuning van virtuele machines met grootten die overeenkomen met uw virtuele bronmachines. Site Recovery kiest even groot of de dichtstbijzijnde grootte voor het VM-doel.

## <a name="configure-outbound-network-connectivity"></a>Uitgaande netwerkconnectiviteit configureren

Voor de Site is hersteld naar verwachting werkt, moet u enkele wijzigingen aanbrengen in uitgaande netwerkverbinding van de VM's die u wilt repliceren.

- Site Recovery biedt geen ondersteuning voor gebruik van een verificatieproxy voor verbinding met het netwerk beheren.
- Als er een verificatieproxy, kan niet replicatie worden ingeschakeld.

### <a name="outbound-connectivity-for-urls"></a>Uitgaande verbinding voor URL 's

Als u een URL gebaseerde, firewall-proxy om te bepalen van de uitgaande verbinding gebruikt, kunt u toegang tot de volgende URL's die worden gebruikt door Site Recovery.

| **URL** | **Details** |
| ------- | ----------- |
| *.blob.core.windows.net | Hiermee kunt gegevens naar het storage-account van de cache in het brongebied van de virtuele machine worden geschreven. |
| Login.microsoftonline.com | Biedt autorisatie en verificatie met Site Recovery-service-URL's. |
| *.hypervrecoverymanager.windowsazure.com | Kan de virtuele machine om te communiceren met de Site Recovery-service. |
| *. servicebus.windows.net | Kan de virtuele machine schrijven bewaking van Site Recovery en de diagnostics-gegevens. |

### <a name="outbound-connectivity-for-ip-address-ranges"></a>Uitgaande verbinding voor IP-adresbereiken

Als u een firewall op basis van IP-, proxy- of NSG-regels voor uitgaande verbinding beheren, moeten de volgende IP-adresbereiken wilt plaatsen. Een lijst met adresbereiken downloaden van de volgende koppelingen:

  - [Microsoft Azure Datacenter IP-adresbereiken](http://www.microsoft.com/en-us/download/details.aspx?id=41653)
  - [Windows Azure Datacenter IP-in Duitsland adresbereiken](http://www.microsoft.com/en-us/download/details.aspx?id=54770)
  - [Windows Azure Datacenter IP-in China adresbereiken](http://www.microsoft.com/en-us/download/details.aspx?id=42064)
  - [Office 365 URLs and IP address ranges (URL's en IP-adresbereiken voor Office 365)](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity)
  - [Site Recovery service-eindpunt IP-adressen](https://aka.ms/site-recovery-public-ips)

Gebruik deze lijsten voor het configureren van de netwerk-toegangsbeheer in uw netwerk. U kunt deze [script](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702) vereist NSG-regels maken.

## <a name="verify-azure-vm-certificates"></a>Controleer of de certificaten van de virtuele machine in Azure

Controleer of de meest recente basiscertificaten aanwezig zijn op de Windows- of Linux-VM's die u wilt repliceren. Als de meest recente basiscertificaten niet, kan niet de virtuele machine geregistreerd bij Site Recovery vervaldatum in beveiligingsbeperkingen.

- Installeren de meest recente Windows updates voor Windows-VM's op de virtuele machine, zodat de vertrouwde basiscertificaten op de machine zijn. Volg de standaard Windows Update en de processen die certificaat updates voor uw organisatie in een omgeving zonder verbinding.

- Volg de richtlijnen bij uw Linux-leverancier om op te halen van de meest recente vertrouwde basiscertificaten en de certificaatintrekkingslijst op de virtuele machine voor Linux VM's.

## <a name="set-permissions-on-the-account"></a>Machtigingen voor het account instellen

Azure Site Recovery biedt drie ingebouwde functies voor het beheren van Site Recovery-beheerbewerkingen.

- **Site Recovery Inzender** -deze rol heeft alle machtigingen nodig voor het beheren van Azure Site Recovery-bewerkingen in een Recovery Services-kluis. Een gebruiker met deze rol echter niet maken of een Recovery Services-kluis verwijderen of toewijzen van rechten aan andere gebruikers. Deze rol is het meest geschikt voor disaster recovery beheerders die kunnen inschakelen en beheren van herstel na noodgevallen voor toepassingen of volledige organisaties.

- **Site Recovery-Operator** -deze rol heeft de machtigingen om uit te voeren en de Failover en Failback operations manager. Een gebruiker met deze rol kan niet inschakelen of uitschakelen van replicatie, maken of verwijderen kluizen, registreren van nieuwe infrastructuur of toegangsrechten toewijzen aan andere gebruikers. Deze rol is het meest geschikt is voor een disaster recovery operator die kan worden uitgevoerd voor virtuele machines of toepassingen wanneer u hierom wordt gevraagd door toepassingseigenaars en IT-beheerders. Post resolutie van de noodherstelservice, de operator DR kunt beveiligt en failback van de virtuele machines.

- **Site Recovery-lezer** -deze rol machtigingen heeft voor het weergeven van alle beheerbewerkingen van Site Recovery. Deze rol is het meest geschikt is voor een IT-bewaking leidinggevende die kunt bewaken van de huidige status van de beveiliging en ondersteuningstickets verhogen.

Meer informatie over [ingebouwde Azure RBAC-rollen](../active-directory/role-based-access-built-in-roles.md)

## <a name="enable-replication"></a>Replicatie inschakelen

### <a name="select-the-source"></a>Selecteer de gegevensbron

1. Klik op de kluisnaam in Recovery Services-kluizen > **+ repliceren**.
2. In **bron**, selecteer **Azure - PREVIEW**.
3. In **bronlocatie**, selecteer de bron van Azure-regio waarin uw virtuele machines worden uitgevoerd.
4. Selecteer de **virtuele machine van Azure-implementatiemodel** voor virtuele machines: **Resource Manager** of **klassieke**.
5. Selecteer de **resourcegroep** voor Resource Manager-VM's of **cloudservice** voor klassieke virtuele machines.
6. Klik op **OK** om de instellingen op te slaan.

### <a name="select-the-vms"></a>Selecteer de virtuele machines

Site Recovery haalt een lijst van de virtuele machines die zijn gekoppeld aan het abonnement en de bronservice groep/cloud.

1. In **virtuele Machines**, selecteert u de virtuele machines die u wilt repliceren.
2. Klik op **OK**.

### <a name="configure-replication-settings"></a>Replicatie-instellingen configureren

Site Recovery maakt standaardinstellingen en beleid voor wachtwoordreplicatie voor de doelregio. U kunt de instellingen op basis van uw vereisten wijzigen.

1. Klik op **instellingen** om de doel- en replicatie-instellingen te bekijken.
2. Te overschrijven de standaardinstellingen van het doel op **aanpassen** naast **resourcegroep, netwerk-, opslag- en Beschikbaarheidssets**.

  ![Instellingen configureren](./media/azure-to-azure-tutorial-enable-replication/settings.png)


- **Doellocatie**: de doelregio gebruikt voor herstel na noodgevallen. We raden aan dat de doellocatie overeenkomt met de locatie van de Site Recovery-kluis.

- **Doelresourcegroep**: de resourcegroep in de doelregio waarin virtuele Azure-machines na een failover. Site Recovery maakt standaard een nieuwe resourcegroep in de doelregio met een achtervoegsel 'asr'.

- **Virtuele doelnetwerk**: het netwerk in de doelregio virtuele machines worden opgeslagen na de failover.
  Site Recovery maakt standaard een nieuw virtueel netwerk (en subnetten) in de doelregio met een achtervoegsel 'asr'.

- **Storage-accounts in de cache**: Site Recovery maakt gebruik van een opslagaccount in de bron-regio. Wijzigingen in de bron-VM's worden verzonden naar dit account voordat de replicatie naar de doellocatie.

- **Storage-accounts zijn gericht**: Site Recovery wordt standaard een nieuw opslagaccount gemaakt in de doelregio voor het spiegelen van de bron-VM storage-account.

- **Beschikbaarheidssets doel**: Site Recovery maakt standaard een nieuwe beschikbaarheidsset voor de doelregio met het achtervoegsel 'asr'. U kunt alleen beschikbaarheidssets toevoegen als virtuele machines deel van een set in de bron-regio uitmaken.

Als u wilt overschrijven de standaardinstellingen van de replicatie-beleid, klikt u op **aanpassen** naast **replicatiebeleid**.  

- **Replicatie-beleidsnaam**: de naam van beleid.

- **Herstel bewaarperiode**: standaard Site Recovery herstelpunten blijft gedurende 24 uur. U kunt een waarde tussen 1 en 72 uur configureren.

- **App-consistente momentopname frequentie**: standaard Site Recovery maakt een app-consistente momentopname elke 4 uur. U kunt een waarde tussen 1 en 12 uur. Een app-consistente momentopname is een momentopname van een punt in tijd van de toepassingsgegevens in de virtuele machine. Volume Shadow Copy Service (VSS) zorgt ervoor dat app op de virtuele machine bevinden zich in een consistente status wanneer een momentopname wordt gemaakt.

- **Replicatiegroep**: als uw toepassing consistentie tussen meerdere VM's over VM's moet, kunt u een replicatiegroep voor de virtuele machines. Standaard zijn de geselecteerde virtuele machines niet deel uit van een replicatiegroep.

  Klik op **aanpassen** naast **replicatiebeleid** en selecteer vervolgens **Ja** op consistentie van multi-VM te maken van virtuele machines deel uitmaken van een replicatiegroep. U kunt een nieuwe replicatiegroep maken of een bestaande replicatiegroep gebruiken. Selecteer de virtuele machines deel uitmaken van de replicatiegroep en klik op **OK**.

> [!IMPORTANT]
  Alle machines in een replicatiegroep wordt vastlopen consistent en toepassingsconsistente herstelpunten wanneer failover hebt gedeeld. Inschakelen multi-VM consistentie kan invloed hebben op prestaties workload en mag alleen worden gebruikt als machines dezelfde werkbelasting worden uitgevoerd en moet u de consistentie tussen meerdere machines.

> [!IMPORTANT]
  Als u de consistentie tussen meerdere VM's inschakelt, wordt de machines in de replicatiegroep met elkaar communiceren via poort 20004. Zorg ervoor dat er geen firewall-apparaat de interne communicatie tussen de VM's via poort 20004 te blokkeren. Als u virtuele Linux-machines deel uitmaken van een replicatiegroep wilt, zorg ervoor dat het uitgaande verkeer op poort 20004 handmatig aan de hand van de richtlijnen van de specifieke versie van Linux wordt geopend.

### <a name="track-replication-status"></a>Replicatiestatus volgen

1. In **instellingen**, klikt u op **vernieuwen** ophalen van de meest recente status.

2. U kunt de voortgang van volgen de **beveiliging inschakelen** taak **instellingen** > **taken** > **Site Recovery-taken**.

3. In **instellingen** > **gerepliceerde Items**, vindt u de status van virtuele machines en de voortgang van de initiële replicatie. Klik op de virtuele machine Inzoomen op de instellingen ervan.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u herstel na noodgevallen geconfigureerd voor een virtuele machine in Azure. Volgende stap is het testen van uw configuratie.

> [!div class="nextstepaction"]
> [Noodherstelanalyse uitvoeren](azure-to-azure-tutorial-dr-drill.md)
