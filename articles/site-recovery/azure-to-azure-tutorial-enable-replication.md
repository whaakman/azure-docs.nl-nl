---
title: Herstel na noodgevallen instellen van Azure-VM’s naar een secundaire Azure-regio met Azure Site Recovery
description: Leer hoe u herstel na noodgevallen van Azure-VM’s naar een andere Azure-regio kunt instellen met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 0e73c0f94e0aa240349aec45b4a146ba5eb37dab
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55700771"
---
# <a name="set-up-disaster-recovery-for-azure-vms-to-a-secondary-azure-region"></a>Herstel na noodgevallen instellen van Azure-VM’s naar een secundaire Azure-regio

De [Azure Site Recovery](site-recovery-overview.md)-service draagt bij aan uw strategie voor herstel na noodgeval, door de replicatie, failover en failback van on-premises machines en Azure-VM’s (virtuele machines) te beheren en in te delen.

Deze zelfstudie laat zien hoe u herstel na noodgevallen in een secundaire Azure-regio kunt instellen voor Azure VM’s. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Recovery Services-kluis maken
> * Doelresource-instellingen controleren
> * Uitgaande toegang instellen voor VM’s
> * Replicatie inschakelen voor een VM

> [!NOTE]
> Dit artikel biedt instructies voor het implementeren van herstel na noodgeval met de eenvoudigste instellingen. Als u informatie wilt over aangepaste instellingen, beoordeelt u de artikelen in de [Instructiesectie](azure-to-azure-how-to-enable-replication.md). o

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- Zorg ervoor dat u inzicht hebt in de [architectuur en onderdelen voor dit scenario](concepts-azure-to-azure-architecture.md).
- Raadpleeg de [ondersteuningsvereisten](site-recovery-support-matrix-azure-to-azure.md) voor alle onderdelen.

## <a name="create-a-vault"></a>Een kluis maken

Maak de kluis in elke gewenste regio, met uitzondering van de bronregio.

1. Meld u aan bij [Azure Portal](https://portal.azure.com) > **Recovery Services**.
2. Klik op **Een resource maken** > **Beheerhulpprogramma's** > **Backup en Site Recovery**.
3. Geef in **Naam** een beschrijvende naam op om de kluis mee aan te duiden. Als u meer dan één abonnement hebt, selecteert u het gewenste abonnement.
4. Maak een resourcegroep of selecteer een bestaande resourcegroep. Geef een Azure-regio op. Zie Geografische beschikbaarheid in [Prijsinformatie voor Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) om na te gaan welke regio's er worden ondersteund.
5. Als u de kluis snel wilt kunnen openen via het dashboard, klikt u op **Vastmaken aan dashboard** en vervolgens op **Maken**.

   ![Nieuwe kluis](./media/azure-to-azure-tutorial-enable-replication/new-vault-settings.png)

   De nieuwe kluis wordt toegevoegd op het **Dashboard** onder **Alle resources** en op de hoofdpagina van **Recovery Services-kluizen**.

## <a name="verify-target-resources"></a>Doelbronnen controleren

1. Controleer of u een Azure-abonnement hebt waarmee u VM´s kunt maken in de doelregio. Neem contact op met ondersteuning voor het inschakelen van het vereiste quotum.
2. Zorg ervoor dat uw abonnement voldoende resources heeft die ondersteuning bieden voor VM’s met grootten die overeenkomen met de bron-VM’s. Site Recovery kiest voor de doel-VM dezelfde of de dichtstbijzijnde grootte.

## <a name="configure-outbound-network-connectivity"></a>Uitgaande netwerkconnectiviteit configureren

Om ervoor te zorgen dat Site Recovery werkt zoals verwacht, moet u de uitgaande netwerkconnectiviteit wijzigen van de VM's die u wilt repliceren.

> [!NOTE]
> Site Recovery biedt geen ondersteuning voor het gebruiken van een verificatieproxy om netwerkconnectiviteit te beheren.



### <a name="outbound-connectivity-for-urls"></a>Uitgaande connectiviteit voor URL's

Als u een URL-firewallproxy gebruikt om de uitgaande connectiviteit te beheren, staat u toegang tot deze URL’s toe.

| **URL** | **Details** |
| ------- | ----------- |
| *.blob.core.windows.net | Hiermee kunnen gegevens van de VM naar het cache-opslagaccount in de bronregio worden geschreven. |
| login.microsoftonline.com | Verzorgt autorisatie en authenticatie voor de URL’s van Site Recovery. |
| *.hypervrecoverymanager.windowsazure.com | Maakt het de VM mogelijk te communiceren met de Site Recovery-service. |
| *.servicebus.windows.net | Maakt het de VM mogelijk bewakings- en diagnosegegevens van Site Recovery te schrijven. |

### <a name="outbound-connectivity-for-ip-address-ranges"></a>Uitgaande connectiviteit voor IP-adresbereiken

Als u de uitgaande connectiviteit wilt beheren met behulp van IP-adressen in plaats van URL’s, staat u deze adressen toe voor firewall-, proxy- of NSG-regels op basis van een IP-adres.

  - [Microsoft Azure Datacenter IP-bereiken](https://www.microsoft.com/en-us/download/details.aspx?id=41653)
  - [Windows Azure Datacenter IP-bereiken in Duitsland](https://www.microsoft.com/en-us/download/details.aspx?id=54770)
  - [Windows Azure Datacenter IP-bereiken in China](https://www.microsoft.com/en-us/download/details.aspx?id=42064)
  - [Office 365 URLs and IP address ranges (URL's en IP-adresbereiken voor Office 365)](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity)
  - [Site Recovery-service-eindpunt-IP-adressen](https://aka.ms/site-recovery-public-ips)

U kunt dit [script](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702) gebruiken om de vereiste NSG-regels maken.

## <a name="verify-azure-vm-certificates"></a>Azure VM-certificaten controleren

Controleer of de VM’s die u wilt repliceren, beschikken over de meest recente basiscertificaten. Als dit niet het geval is, kunnen de VM’s niet worden geregistreerd bij Site Recovery, vanwege beveiligingsbeperkingen.

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
4. Selecteer het **Bronabonnement** waar de virtuele machines worden uitgevoerd. Dit kan elk abonnement zijn binnen dezelfde Azure Active Directory-tenant waar uw Recovery Services-kluis zich bevindt.
5. Selecteer de **Bronresourcegroep** voor Resource Manager-VM's of **cloudservice** voor klassieke VM’s.
6. Klik op **OK** om de instellingen op te slaan.

### <a name="select-the-vms"></a>De VM’s selecteren

Site Recovery haalt een lijst op van de VM’s die zijn gekoppeld aan het abonnement en de resourcegroep/cloudservice.

1. Selecteer in **Virtuele machines** de VM’s die u wilt repliceren.
2. Klik op **OK**.

### <a name="configure-replication-settings"></a>Replicatie-instellingen configureren

Site Recovery maakt standaardinstellingen en replicatiebeleid voor de doelregio. U kunt de instellingen wijzigen zoals vereist.

1. Klik op **Instellingen** om de doel- en replicatie-instellingen te bekijken.
2. Klik op **Aanpassen** naast **Resourcegroep, netwerk, opslag en beschikbaarheid** als u de standaarddoelinstellingen wilt overschrijven.

  ![Instellingen configureren](./media/azure-to-azure-tutorial-enable-replication/settings.png)


3. Pas doelinstellingen als volgt aan:

    - **Doelabonnement**: Het doelabonnement dat wordt gebruikt voor herstel na noodgevallen. Standaard is het doelabonnement niet hetzelfde als het bronabonnement. Klik op 'Aanpassen' om een ander doelabonnement binnen dezelfde Azure Active Directory-tenant te selecteren.
    - **Doellocatie**: De doelregio die wordt gebruikt voor herstel na noodgevallen. Wij raden aan dezelfde doellocatie te gebruiken als de locatie van de Site Recovery-kluis.
    - **Doelresourcegroep**: De resourcegroep in de doelregio waarin Azure-VM’s zich na een failover bevinden. Site Recovery maakt standaard een nieuwe resourcegroep in de doelregio met het achtervoegsel 'asr'. resourcegroep-locatie van de doelresourcegroep mag elke regio zijn, behalve de regio waar uw virtuele bronmachines worden gehost.
    - **Virtueel doelnetwerk**: Het netwerk in de doelregio waar VM’s zich na een failover bevinden.
      Site Recovery maakt standaard een nieuw virtueel netwerken (met subnets) in de doelregio met het achtervoegsel 'asr'.
    - **Cacheopslagaccounts**: Site Recovery maakt gebruik van een opslagaccount in de bronregio. Wijzigingen in de bron-VM's worden naar dit account verzonden vóór replicatie naar de doellocatie.
      >[!NOTE]
      >Als u gebruikmaakt van een cacheopslagaccount met een firewall, zorgt u ervoor dat u vertrouwde Microsoft-services toestaat. [Meer informatie.](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions)
      >

    - **Doelopslagaccounts (als de bron-VM niet gebruikmaakt van beheerde schijven)**: Met Site Recovery wordt standaard een nieuw opslagaccount in de doelregio gemaakt om het bron-VM-opslagaccount te spiegelen.
      >[!NOTE]
      >Als u gebruikmaakt van een bron- of doelopslagaccount met een firewall, zorgt u ervoor dat u vertrouwde Microsoft-services toestaat. [Meer informatie.](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions)
      >

    - **Beheerde replicaschijven (als de bron-VM gebruikmaakt van beheerde schijven)**: Met Site Recovery worden standaard replicaschijven in de doelregio gemaakt om de beheerde schijven van de bron-VM's met hetzelfde opslagtype (Standard of Premium) te spiegelen als de beheerde schijf van de bron-VM's.
    - **Doelbeschikbaarheidssets**: In Azure Site Recovery wordt standaard een nieuwe beschikbaarheidsset gemaakt in de doelregio, met een naam die eindigt op 'asr' voor het VM-gedeelte van een beschikbaarheidsset in de bronregio. Als de beschikbaarheidsset die wordt gemaakt met Azure Site Recovery, al bestaat, wordt deze opnieuw gebruikt.
    - **Doelbeschikbaarheidszones**: In Site Recovery wordt standaard hetzelfde zonenummer toegewezen als de bronregio in de doelregio, als de doelregio ondersteuning biedt voor beschikbaarheidszones. 

    Als de doelregio geen ondersteuning biedt voor beschikbaarheidszones, worden de doel-VM's standaard geconfigureerd als enkele exemplaren. Indien vereist, kunt u dergelijke VM's configureren als onderdeel van beschikbaarheidssets in de doelregio door op Aanpassen te klikken.

    >[!NOTE]
    >Nadat u replicatie hebt ingeschakeld, kunt u het type beschikbaarheid - enkel exemplaar, beschikbaarheidsset of beschikbaarheidszone - niet meer wijzigen. Als u het type beschikbaarheid wilt wijzigen, moet u replicatie uitschakelen en opnieuw inschakelen.
    >

4. Klik naast **Replicatiebeleid** op **Aanpassen** en pas de volgende instellingen aan, zoals is vereist, om de instellingen voor het replicatiebeleid aan te passen:

    - **Naam voor replicatiebeleid**: Beleidsnaam.
    - **Bewaarperiode van herstelpunt**: In Site Recovery worden herstelpunten standaard 24 uur bewaard. U kunt een waarde van 1 tot 72 uur configureren.
    - **Frequentie van de app-consistente momentopname**: In Site Recovery wordt standaard elke 4 uur een app-consistente momentopname gemaakt. U kunt een waarde van 1 tot 12 uur configureren. Een app-consistente momentopname is een momentopname van de toepassingsgegevens in de VM op een bepaald tijdstip. Met Volume Shadow Copy Service (VSS) wordt ervoor gezorgd dat apps op de VM een consistente status hebben wanneer er een momentopname wordt gemaakt.
    - **Replicatiegroep**: Als uw toepassing consistentie tussen meerdere VM's nodig heeft, kunt u een replicatiegroep maken voor deze VM’s. Standaard maken de geselecteerde VM’s geen deel uit van een replicatiegroep.

5. Selecteer in **Aanpassen** de optie **Ja** voor consistentie van multi-VM, als u VM’s wilt toevoegen aan een nieuwe of bestaande replicatiegroep. Klik vervolgens op **OK**.

    - Alle machines in een replicatiegroep hebben gedeelde crash-consistente en app-consistente herstelpunten bij een failover. Het inschakelen van multi-VM-consistentie is CPU-intensief en kan de prestaties van de werklast beïnvloeden. Deze functie moet alleen worden gebruikt als de computers dezelfde werklast hebben en u consistentie tussen meerdere computers nodig hebt.
    - Een replicatiegroep kan maximaal 16 virtuele machines bevatten.
    - Als u multi-VM-consistentie inschakelt, communiceren machines in de replicatiegroep met elkaar via poort 20004. Zorg ervoor dat de interne communicatie tussen de VM's via poort 20004 niet door een firewall-apparaat wordt geblokkeerd. Als u wilt dat Linux VM’s deel uitmaken van een replicatiegroep, zorg er dan voor dat het uitgaande verkeer op poort 20004 handmatig wordt geopend volgens de richtlijnen van de specifieke Linux-versie.

### <a name="configure-encryption-settings"></a>Versleutelingsinstellingen configureren

Als ADE (Azure-schijfversleuteling) is ingeschakeld op de virtuele bronmachine, worden de versleutelingsinstellingen weergegeven:

1. Controleer de versleutelingsinstellingen.
    - **Sleutelkluizen voor schijfversleuteling**: In Azure Site Recovery wordt standaard een nieuwe sleutelkluis in de doelregio gemaakt met het achtervoegsel 'asr' in de naam, op basis van de versleutelingssleutels voor de bron-VM-schijf. In het geval dat de sleutelkluis die wordt gemaakt door Azure Site Recovery al bestaat, wordt deze opnieuw gebruikt.
    - **Sleutelkluizen voor versleutelingssleutel**: In Azure Site Recovery wordt standaard een nieuwe sleutelkluis in de doelregio gemaakt met het achtervoegsel 'asr' in de naam, op basis van de versleutelingssleutels voor de bron-VM-sleutel. In het geval dat de sleutelkluis die wordt gemaakt door Azure Site Recovery al bestaat, wordt deze opnieuw gebruikt.

2. Klik op **Aanpassen** om de aangepaste sleutelkluizen te selecteren.

>[!NOTE]
>Momenteel worden alleen Azure-VM's met Windows-besturingssystemen die zijn [ingeschakeld voor versleuteling met Azure AD-app](https://aka.ms/ade-aad-app), ondersteund voor Azure Site Recovery.
>

### <a name="track-replication-status"></a>Replicatiestatus volgen

1. Klik in **Instellingen** op **Vernieuwen** om de nieuwste status op te halen.
2. U kunt als volgt de voortgang en status bijhouden:
    - Houd de voortgang bij van de taak **Beveiliging inschakelen** via **Instellingen** > **Taken** > **Site Recovery-taken**.
    - In **Instellingen** > **Gerepliceerde Items** kunt u de status van VM’s en de voortgang van de initiële replicatie bekijken. Klik op de VM om in te zoomen op de instellingen ervan.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u herstel na noodgevallen geconfigureerd voor een Azure-VM. U kunt nu een analyse van herstel na noodgeval initiëren om te controleren of de failover werkt zoals verwacht.

> [!div class="nextstepaction"]
> [Noodherstelanalyse uitvoeren](azure-to-azure-tutorial-dr-drill.md)
