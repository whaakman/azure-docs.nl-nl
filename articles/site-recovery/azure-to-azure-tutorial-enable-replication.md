---
title: Herstel na noodgevallen instellen van Azure-VM’s naar een secundaire Azure-regio met Azure Site Recovery
description: Leer hoe u herstel na noodgevallen van Azure-VM’s naar een andere Azure-regio kunt instellen met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 10/28/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 8dc6b7da77988a789de04578d6653b192f58afa8
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51261720"
---
# <a name="set-up-disaster-recovery-for-azure-vms-to-a-secondary-azure-region"></a>Herstel na noodgevallen instellen van Azure-VM’s naar een secundaire Azure-regio

De [Azure Site Recovery](site-recovery-overview.md)-service draagt bij aan uw strategie voor herstel na noodgevallen door de replicatie, failover en failback van on-premises machines en virtuele Azure-machines te beheren en in te delen.

Deze zelfstudie laat zien hoe u herstel na noodgevallen in een secundaire Azure-regio kunt instellen voor Azure VM’s. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Recovery Services-kluis maken
> * Doelresource-instellingen controleren
> * Uitgaande toegang instellen voor VM’s
> * Replicatie inschakelen voor een VM

> [!NOTE]
> Deze zelfstudie is bedoeld om de gebruiker stapsgewijs te begeleiden bij het uitvoeren van een replicatie met een minimale hoeveelheid aanpassing. Raadpleeg de documenten bij Instructies voor Azure-VM's als u meer informatie wilt over de verschillende aspecten van herstel na noodgevallen, inclusief netwerkoverwegingen, automatisering en het oplossen van problemen.


## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- Zorg ervoor dat u inzicht hebt in de [architectuur en onderdelen voor dit scenario](concepts-azure-to-azure-architecture.md).
- Raadpleeg de [ondersteuningsvereisten](site-recovery-support-matrix-azure-to-azure.md) voor alle onderdelen.

## <a name="create-a-vault"></a>Een kluis maken

Maak de kluis in elke gewenste regio, met uitzondering van de bronregio.

1. Meld u aan bij [Azure Portal](https://portal.azure.com) > **Recovery Services**.
2. Klik op **Een resource maken** > **Controle en beheer** > **Backup en Site Recovery**.
3. Geef in **Naam** een beschrijvende naam op om de kluis mee aan te duiden. Als u meer dan één abonnement hebt, selecteert u het gewenste abonnement.
4. Maak een resourcegroep of selecteer een bestaande resourcegroep. Geef een Azure-regio op. Zie Geografische beschikbaarheid in [Prijsinformatie voor Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) om na te gaan welke regio's er worden ondersteund.
5. Als u de kluis snel wilt kunnen openen via het dashboard, klikt u op **Vastmaken aan dashboard** en vervolgens op **Maken**.

   ![Nieuwe kluis](./media/azure-to-azure-tutorial-enable-replication/new-vault-settings.png)

   De nieuwe kluis wordt toegevoegd op het **Dashboard** onder **Alle resources** en op de hoofdpagina van **Recovery Services-kluizen**.

## <a name="verify-target-resources"></a>Doelbronnen controleren

1. Controleer of u een Azure-abonnement hebt waarmee u in staat bent om virtuele machines te maken in de doelregio die wordt gebruikt voor herstel na noodgevallen. Neem contact op met ondersteuning voor het inschakelen van het vereiste quotum.

2. Zorg ervoor dat uw abonnement voldoende resources heeft die ondersteuning kunnen bieden voor VM’s met grootten die overeenkomen met uw bron-VM’s. Site Recovery kiest voor de doel-VM dezelfde of de dichtstbijzijnde grootte.

## <a name="configure-outbound-network-connectivity"></a>Uitgaande netwerkconnectiviteit configureren

Om ervoor te zorgen dat Site Recovery werkt zoals verwacht, moet u enkele wijzigingen aanbrengen in de uitgaande netwerkconnectiviteit van VM's die u wilt repliceren.

- Site Recovery biedt geen ondersteuning voor het gebruiken van een verificatieproxy om netwerkconnectiviteit te beheren.
- Als u een verificatieproxy hebt, kan replicatie niet worden ingeschakeld.

### <a name="outbound-connectivity-for-urls"></a>Uitgaande connectiviteit voor URL's

Als u een firewall-proxy op basis van URL’s gebruikt om de uitgaande connectiviteit te beheren, moet u toegang verlenen aan de volgende URL’s die door Site Recovery worden gebruikt.

| **URL** | **Details** |
| ------- | ----------- |
| *.blob.core.windows.net | Hiermee kunnen gegevens van de VM naar het cache-opslagaccount in de bronregio worden geschreven. |
| login.microsoftonline.com | Verzorgt autorisatie en authenticatie voor de URL’s van Site Recovery. |
| *.hypervrecoverymanager.windowsazure.com | Maakt het de VM mogelijk te communiceren met de Site Recovery-service. |
| *.servicebus.windows.net | Maakt het de VM mogelijk bewakings- en diagnosegegevens van Site Recovery te schrijven. |

### <a name="outbound-connectivity-for-ip-address-ranges"></a>Uitgaande connectiviteit voor IP-adresbereiken

Als u uitgaande connectiviteit wilt beheren met behulp van IP-adressen in plaats van URL's, neem dan de juiste datacenterbereiken, Office 365-adressen en service-eindpuntadressen op in de lijst met toegestane adressen, voor op IP gebaseerde firewalls, proxy- of NSG-regels.

  - [Microsoft Azure Datacenter IP-bereiken](https://www.microsoft.com/en-us/download/details.aspx?id=41653)
  - [Windows Azure Datacenter IP-bereiken in Duitsland](https://www.microsoft.com/en-us/download/details.aspx?id=54770)
  - [Windows Azure Datacenter IP-bereiken in China](https://www.microsoft.com/en-us/download/details.aspx?id=42064)
  - [Office 365 URLs and IP address ranges (URL's en IP-adresbereiken voor Office 365)](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity)
  - [Site Recovery-service-eindpunt-IP-adressen](https://aka.ms/site-recovery-public-ips)

U kunt dit [script](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702) gebruiken om de vereiste NSG-regels maken.

## <a name="verify-azure-vm-certificates"></a>Azure VM-certificaten controleren

Controleer of de meest recente basiscertificaten aanwezig zijn op de Windows- of Linux-VM’s die u wilt repliceren. Als de meest recente basiscertificaten niet aanwezig zijn, kan de VM vanwege veiligheidsbeperkingen niet bij Site Recovery worden geregistreerd.

- Voor Windows-VM’s moet u de meest recente Windows-updates op de VM installeren, zodat alle vertrouwde basiscertificaten op de machine aanwezig zijn. In een niet-verbonden omgeving moet u de standaardprocedures van Windows Update en de standaardprocedures voor het bijwerken van de certificaten van uw organisatie volgen.

- Voor Linux-VM’s volgt u de richtlijnen van de Linux-distributeur voor het verkrijgen van de meest recente basiscertificaten en de certificaatintrekkingslijst op de VM.

## <a name="set-permissions-on-the-account"></a>Machtigingen instellen voor het account

Azure Site Recovery heeft drie ingebouwde rollen voor het beheren van Site Recovery-beheerbewerkingen.

- **Site Recovery-inzender** - deze rol bevat alle machtigingen die nodig zijn om Azure Site Recovery-bewerkingen in een Recovery Services-kluis te kunnen beheren. Een gebruiker met deze rol kan echter geen Recovery Services-kluis maken of verwijderen, noch toegangsrechten aan andere gebruikers toewijzen. Deze rol is het meest geschikt voor beheerders die herstel na noodgevallen voor toepassingen of hele organisaties kunnen inschakelen en beheren.

- **Site Recovery-operator**: deze rol heeft machtigingen om failover- en failback-bewerkingen uit te voeren en te beheren. Een gebruiker met deze rol kan replicatie niet in- of uitschakelen, geen kluizen maken of verwijderen, en geen nieuwe infrastructuur registreren of toegangsrechten toewijzen aan andere gebruikers. Deze rol is het meest geschikt voor een operator die failover-overschakeling kan uitvoeren voor virtuele machines of applicaties in opdracht van toepassingseigenaren en IT-beheerders. Na het noodherstel kan de DR-operator de virtuele machines opnieuw beschermen en een failback uitvoeren.

- **Site Recovery-lezer** - deze rol heeft machtigingen voor het weergeven van alle beheerbewerkingen van Site Recovery. Deze rol is het meest geschikt is voor een IT-leidinggevende die de huidige beschermingsstatus kan bewaken en ondersteuningstickets kan aanvragen.

Meer informatie over [ingebouwde Azure RBAC-rollen](../role-based-access-control/built-in-roles.md)

## <a name="enable-replication"></a>Replicatie inschakelen

### <a name="select-the-source"></a>De bron selecteren

1. Klik in Recovery Services-kluizen op de kluisnaam > **+Repliceren**.
2. Bij **Bron** selecteert u **Azure**.
3. Bij **Bronlocatie** selecteert u de Azure-bronregio waar uw VM’s momenteel worden uitgevoerd.
4. Selecteer het **Azure virtual machine-implementatiemodel** voor VM’s: **Resourcemanager** of **Klassiek**.
5. Selecteer het **Bronabonnement** waar de virtuele machines worden uitgevoerd. Dit kan elk abonnement zijn binnen dezelfde Azure Active Directory-tenant waar uw Recovery Services-kluis zich bevindt.
6. Selecteer de **Bronresourcegroep** voor Resource Manager-VM's of **cloudservice** voor klassieke VM’s.
7. Klik op **OK** om de instellingen op te slaan.

### <a name="select-the-vms"></a>De VM’s selecteren

Site Recovery haalt een lijst op van de VM’s die zijn gekoppeld aan het abonnement en de resourcegroep/cloudservice.

1. Selecteer in **Virtuele machines** de VM’s die u wilt repliceren.
2. Klik op **OK**.

### <a name="configure-replication-settings"></a>Replicatie-instellingen configureren

Site Recovery maakt standaardinstellingen en replicatiebeleid voor de doelregio. U kunt de instellingen aanpassen aan uw behoeften.

1. Klik op **Instellingen** om de doel- en replicatie-instellingen te bekijken.
2. Klik op **Aanpassen** naast **Resourcegroep, netwerk, opslag en beschikbaarheidssets** als u de standaarddoelinstellingen wilt overschrijven.

  ![Instellingen configureren](./media/azure-to-azure-tutorial-enable-replication/settings.png)


- **Doelabonnement**: het doelabonnement dat wordt gebruikt voor herstel na noodgevallen. Standaard is het doelabonnement niet hetzelfde als het bronabonnement. Klik op 'Aanpassen' om een ander doelabonnement binnen dezelfde Azure Active Directory-tenant te selecteren.

- **Doellocatie**: de doelregio die wordt gebruikt voor herstel na noodgevallen. Wij raden aan dezelfde doellocatie te gebruiken als de locatie van de Site Recovery-kluis.

- **Doelresourcegroep**: de resourcegroep in de doelregio waarin Azure-VM’s zich na een failover bevinden. Site Recovery maakt standaard een nieuwe resourcegroep in de doelregio met het achtervoegsel 'asr'. resourcegroep-locatie van de doelresourcegroep mag elke regio zijn, behalve de regio waar uw virtuele bronmachines worden gehost.

- **Virtueel doelnetwerk**: het netwerk in de doelregio waar VM’s zich na een failover bevinden.
  Site Recovery maakt standaard een nieuw virtueel netwerken (met subnets) in de doelregio met het achtervoegsel 'asr'.

- **Cacheopslagaccounts**: Site Recovery maakt gebruik van een opslagaccount in de bronregio. Wijzigingen in de bron-VM's worden naar dit account verzonden vóór replicatie naar de doellocatie.

- **Doelopslagaccounts (als bron-VM geen beheerde schijven gebruikt)**: Site Recovery maakt standaard een nieuw opslagaccount in de doelregio om het bron-VM-opslagaccount te spiegelen.

- **Beheerde replicaschijven (als de bron-VM gebruikmaakt van beheerde schijven)**: Site Recovery maakt standaard replicaschijven in de doelregio om de beheerde schijven van de bron-VM's te spiegelen met hetzelfde opslagtype (Standard of Premium) als de beheerde schijf van de bron-VM's.

- **Doelbeschikbaarheidssets**: Site Recovery maakt standaard een nieuwe beschikbaarheidsset in de doelregio met het achtervoegsel 'asr'. U kunt alleen beschikbaarheidssets toevoegen als VM’s deel uitmaken van een set in de bronregio.

Als u de standaardinstellingen voor het replicatiebeleid wilt overschrijven, klikt u op **Aanpassen** naast **Replicatiebeleid**.  

- **Naam replicatiebeleid**: de naam van het beleid.

- **Bewaarperiode van het herstelpunt**: Site Recovery bewaart herstelpunten standaard 24 uur. U kunt een waarde van 1 tot 72 uur configureren.

- **Frequentie van de app-consistente momentopname**: Site Recovery maakt standaard elke 4 uur een app-consistente momentopname. U kunt een waarde van 1 tot 12 uur configureren. Een app-consistente momentopname is een momentopname van de toepassingsgegevens in de VM op een bepaald tijdstip. Met Volume Shadow Copy Service (VSS) wordt ervoor gezorgd dat apps op de VM een consistente status hebben wanneer er een momentopname wordt gemaakt.

- **Replicatiegroep**: als uw toepassing consistentie tussen meerdere VM's nodig heeft, kunt u een replicatiegroep maken voor die VM’s. Standaard maken de geselecteerde VM’s geen deel uit van een replicatiegroep.

  Klik op **Aanpassen** naast **Replicatiebeleid** en selecteer vervolgens **Ja** bij multi-VM-consistentie om de VM’s deel te laten uitmaken van een replicatiegroep. U kunt een nieuwe replicatiegroep maken of een bestaande replicatiegroep gebruiken. Selecteer de VM’s die deel moeten uitmaken van de replicatiegroep en klik op **OK**.

> [!IMPORTANT]
  Alle machines in een replicatiegroep hebben gedeelde crash-consistente en app-consistente herstelpunten bij een failover. Het inschakelen van multi-VM-consistentie kan de prestaties van de werklast beïnvloeden en moet alleen worden gebruikt als de computers dezelfde werklast hebben en u consistentie tussen meerdere computers nodig hebt.

> [!IMPORTANT]
  Als u multi-VM-consistentie inschakelt, communiceren machines in de replicatiegroep met elkaar via poort 20004. Zorg ervoor dat de interne communicatie tussen de VM's via poort 20004 niet door een firewall-apparaat wordt geblokkeerd. Als u wilt dat Linux VM’s deel uitmaken van een replicatiegroep, zorg er dan voor dat het uitgaande verkeer op poort 20004 handmatig wordt geopend volgens de richtlijnen van de specifieke Linux-versie.

### <a name="configure-encryption-settings"></a>Versleutelingsinstellingen configureren

Als op de virtuele bronmachine ADE (Azure Disk Encryption) is ingeschakeld, worden de onderstaande instellingen voor versleuteling weergegeven.

- **Sleutelkluizen voor schijfversleuteling**: Azure Site Recovery maakt standaard een nieuwe sleutelkluis in de doelregio met het achtervoegsel 'asr' in de naam, op basis van de versleutelingssleutels voor de bron VM-schijf. In het geval dat de sleutelkluis die wordt gemaakt door Azure Site Recovery al bestaat, wordt deze opnieuw gebruikt.
- **Sleutelkluizen voor sleutelversleuteling**: Azure Site Recovery maakt standaard een nieuwe sleutelkluis in de doelregio met het achtervoegsel 'asr' in de naam, op basis van de versleutelingssleutels voor de bron VM-sleutel. In het geval dat de sleutelkluis die wordt gemaakt door Azure Site Recovery al bestaat, wordt deze opnieuw gebruikt.

Klik op Aanpassen naast Versleutelingsinstellingen om de standaardinstellingen te overschrijven en aangepaste sleutelkluizen te selecteren.

>[!NOTE]
>Alleen Azure-VM's met Windows die zijn [ingeschakeld voor versleuteling met Azure AD-app](https://aka.ms/ade-aad-app) worden momenteel ondersteund door Azure Site Recovery.
>

### <a name="track-replication-status"></a>Replicatiestatus volgen

1. Klik in **Instellingen** op **Vernieuwen** om de nieuwste status op te halen.

2. U kunt de voortgang van de taak **Beveiliging inschakelen** volgen via **Instellingen** > **Taken** > **Site Recovery-taken**.

3. In **Instellingen** > **Gerepliceerde Items** kunt u de status van VM’s en de voortgang van de initiële replicatie bekijken. Klik op de VM om in te zoomen op de instellingen ervan.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u herstel na noodgevallen geconfigureerd voor een Azure-VM. De volgende stap is het testen van uw configuratie door een noodherstelanalyse te starten.

> [!div class="nextstepaction"]
> [Noodherstelanalyse uitvoeren](azure-to-azure-tutorial-dr-drill.md)
