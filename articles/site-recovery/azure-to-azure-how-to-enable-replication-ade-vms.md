---
title: Replicatie is ingeschakeld voor Azure disk encryption (ADE) virtuele machines configureren in Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe het configureren van replicatie voor virtuele machines ADE, van een Azure-regio naar de andere met behulp van Site Recovery.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: sutalasi
ms.openlocfilehash: f9abc6d79bd821ef612e9e7648b1b5af98bb5cf6
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56456228"
---
# <a name="replicate-azure-disk-encryption-ade-enabled-virtual-machines-to-another-azure-region"></a>Azure disk encryption (ADE) ingeschakeld virtuele machines repliceren naar een andere Azure-regio

In dit artikel wordt beschreven hoe u virtuele machines de replicatie van Azure disk encryption (ADE) is ingeschakeld, van een Azure-regio naar een andere inschakelen.

>[!NOTE]
>Alleen Azure-VM's met Windows die zijn [ingeschakeld voor versleuteling met Azure AD-app](https://aka.ms/ade-aad-app) worden momenteel ondersteund door Azure Site Recovery.
>

## <a name="required-user-permissions"></a>Vereiste machtigingen
Azure Site Recovery is vereist voor de gebruiker een machtiging voor het maken van de key vault in de doel-regio en kopieert u de sleutels naar de regio.

Als u wilt inschakelen replicatie van virtuele machines ADE uit portal, de gebruiker moet beschikken over de onderstaande machtigingen.
- Key vault-machtigingen
    - list
    - Maken
    - Ophalen

-   Sleutelkluis geheime machtigingen
    - Lijst
    - Maken
    - Ophalen

- Sleutelkluis sleutelmachtigingen (alleen vereist als de virtuele machines Key-versleutelingssleutel gebruikt voor het versleutelen van sleutels voor versleuteling van schijf)
    - Lijst
    - Ophalen
    - Maken
    - Versleutelen
    - Ontsleutelen

U kunt de machtigingen beheren door te navigeren naar de sleutelkluis-resource in de portal en de vereiste machtigingen toe te voegen aan de gebruiker. Bijvoorbeeld: hieronder stapsgewijze handleiding laat zien hoe u om in te schakelen voor de Key vault 'ContosoWeb2Keyvault', dat zich in de bronregio.


-  Ga naar ' Start > Keyvaults > ContosoWeb2KeyVault > beleid voor gegevenstoegang '

![Key Vault-machtigingen](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-1.png)



- U kunt zien er is geen gebruiker persimmon daarom de hierboven genoemde machtiging toevoegen door te klikken op 'Nieuwe toevoegen' en de gebruiker en de machtigingen

![Key Vault-machtigingen](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-2.png)

Als de gebruiker inschakelen van herstel na noodgeval (DR) beschikt niet over de vereiste machtigingen om te kopiëren van de sleutels, het onderstaande script kan worden opgegeven voor de beveiligingsbeheerder met de juiste machtigingen om te kopiëren van de versleuteling geheimen en sleutels naar de doelregio.

Raadpleeg [in dit artikel](#trusted-root-certificates-error-code-151066) voor het oplossen van problemen met de machtiging.
>[!NOTE]
>Als u wilt inschakelen replicatie van virtuele machine ADE uit portal, moet u ten minste de machtigingen 'List' in de sleutelkluizen, geheimen en sleutels
>

## <a name="copy-ade-keys-to-dr-region-using-powershell-script"></a>ADE sleutels naar de DR-regio met behulp van PowerShell-Script kopiëren

1. De onbewerkte 'CopyKeys'-scriptcode in een browservenster geopend door te klikken op [deze koppeling](https://aka.ms/ade-asr-copy-keys-code).
2. Kopieer het script naar een bestand en geef deze de naam ' kopiëren-keys.ps1'.
2. Open de Windows PowerShell-toepassing en Ga naar de map waar het bestand bestaat.
3. Start ' kopiëren-keys.ps1'
4. Geef de referenties van de Azure-aanmelding.
5. Selecteer de **Azure-abonnement** van uw VM's.
6. Wacht totdat de resourcegroepen te laden en selecteer vervolgens de **resourcegroep** van uw VM's.
7. Selecteer de virtuele machines in de lijst met virtuele machines die worden weergegeven. Alleen virtuele machines met Azure disk encryption is ingeschakeld, worden weergegeven in de lijst.
8. Selecteer de **doellocatie**.
9. **Sleutelkluizen voor schijfversleuteling**: In Azure Site Recovery wordt standaard een nieuwe sleutelkluis in de doelregio gemaakt met het achtervoegsel 'asr' in de naam, op basis van de versleutelingssleutels voor de bron-VM-schijf. In het geval dat de sleutelkluis die wordt gemaakt door Azure Site Recovery al bestaat, wordt deze opnieuw gebruikt. In de lijst zo nodig kunt u een andere key vault.
10. **Sleutelkluizen voor versleutelingssleutel**: In Azure Site Recovery wordt standaard een nieuwe sleutelkluis in de doelregio gemaakt met het achtervoegsel 'asr' in de naam, op basis van de versleutelingssleutels voor de bron-VM-sleutel. In het geval dat de sleutelkluis die wordt gemaakt door Azure Site Recovery al bestaat, wordt deze opnieuw gebruikt. In de lijst zo nodig kunt u een andere key vault.

## <a name="enable-replication"></a>Replicatie inschakelen

Deze procedure wordt ervan uitgegaan dat de primaire Azure-regio Oost-Azië is en de secundaire regio Zuidoost-Azië.

1. Klik in de kluis op **+ repliceren**.
2. Houd rekening met de volgende velden:
    - **Bron**: Het punt van herkomst van de virtuele machines, die in dit geval **Azure**.
    - **Bronlocatie**: De Azure-regio waar u wilt beveiligen van uw virtuele machines. In dit voorbeeld is de locatie van de Oost-Azië
    - **Implementatiemodel**: Azure-implementatiemodel van de bronmachines.
    - **Abonnement van bron**: Het abonnement waartoe de virtuele bronmachines behoren. Dit kan elk abonnement zijn binnen dezelfde Azure Active Directory-tenant waar uw Recovery Services-kluis zich bevindt.
    - **Resourcegroep**: De resourcegroep waartoe de virtuele bronmachines behoren. Alle virtuele machines onder de geselecteerde resourcegroep worden weergegeven voor beveiliging in de volgende stap.

3. In **virtuele Machines > virtuele machines selecteren**, klikt u op en selecteer elke VM die u wilt repliceren. U kunt alleen machines selecteren waarvoor replicatie kan worden ingeschakeld. Klik vervolgens op **OK**.

4. In **instellingen**, kunt u eventueel target-site-instellingen configureren:

    - **Doellocatie**: De locatie waar de brongegevens van de virtuele machine worden gerepliceerd. Afhankelijk van de locatie van uw geselecteerde machines, biedt Site Recovery u de lijst met geschikte doelregio's. U wordt aangeraden dat u de doellocatie hetzelfde als de locatie voor de Recovery Services-kluis.
    - **Doelabonnement**: Het doelabonnement dat wordt gebruikt voor herstel na noodgevallen. Standaard is het doelabonnement niet hetzelfde als het bronabonnement.
    - **Doelresourcegroep**: De resourcegroep waarin alle uw gerepliceerde virtuele machines behoren. Azure Site Recovery maakt standaard een nieuwe resourcegroep in de doelregio met de naam met achtervoegsel 'asr'. In het geval resourcegroep hebt gemaakt met Azure Site Recovery al bestaat, wordt dit opnieuw gebruikt. U kunt ook aanpassen zoals wordt weergegeven in de onderstaande sectie. De locatie van de doelresourcegroep kan zijn van andere Azure-regio's met uitzondering van de regio waarin de virtuele bronmachines worden gehost.
    - **Target Virtual Network**: Site Recovery maakt standaard een nieuw virtueel netwerk in de doelregio met de naam met achtervoegsel 'asr'. Dit is toegewezen aan de bron-netwerk en voor eventuele toekomstige beveiliging gebruikt. [Meer informatie](site-recovery-network-mapping-azure-to-azure.md) over netwerktoewijzing.
    - **Doelopslagaccounts (als de bron-VM maakt geen gebruik van schijven beheerde)**: Site Recovery maakt standaard een nieuw doel-opslagaccount mimicking de opslagconfiguratie van de bron-VM. In het geval er bestaat al een storage-account, wordt dit opnieuw gebruikt.
    - **Beheerde replicaschijven (als de bron-VM gebruikmaakt van beheerde schijven)**: Site Recovery maakt nieuwe beheerde replicaschijven in de doelregio voor het spiegelen van beheerde schijven van de bron-VM met hetzelfde opslagtype (Standard of premium) als de bron-VM van schijf beheerde.
    - **Storage-accounts in de cache**: Site Recovery moet extra opslagaccount met de naam van cacheopslag in de bronregio. Alle wijzigingen die plaatsvinden op de bron-VM's worden bijgehouden en verzonden naar de cache-opslagaccount voor het repliceren van die naar de doellocatie.
    - **Beschikbaarheidsset**: Azure Site Recovery maakt standaard een nieuwe beschikbaarheidsset in de doelregio met de naam met achtervoegsel 'asr'. Als de beschikbaarheidsset die wordt gemaakt met Azure Site Recovery, al bestaat, wordt deze opnieuw gebruikt.
    - **Sleutelkluizen voor schijfversleuteling**: In Azure Site Recovery wordt standaard een nieuwe sleutelkluis in de doelregio gemaakt met het achtervoegsel 'asr' in de naam, op basis van de versleutelingssleutels voor de bron-VM-schijf. In het geval dat de sleutelkluis die wordt gemaakt door Azure Site Recovery al bestaat, wordt deze opnieuw gebruikt.
    - **Sleutelkluizen voor versleutelingssleutel**: In Azure Site Recovery wordt standaard een nieuwe sleutelkluis in de doelregio gemaakt met het achtervoegsel 'asr' in de naam, op basis van de versleutelingssleutels voor de bron-VM-sleutel. In het geval dat de sleutelkluis die wordt gemaakt door Azure Site Recovery al bestaat, wordt deze opnieuw gebruikt.
    - **Beleid voor wachtwoordreplicatie**: Hiermee worden de instellingen voor recovery point bewaren geschiedenis en de app de momentopnamefrequentie gedefinieerd. Azure Site Recovery maakt standaard een nieuw replicatiebeleid met de standaardinstellingen van 24 uur voor de bewaarperiode voor herstelpunten en ' 60 minuten voor de frequentie voor app-consistente momentopname te maken.



## <a name="customize-target-resources"></a>Doelresources aanpassen

U kunt de standaardinstellingen van de doel-gebruikt door Site Recovery kunt wijzigen.


1. Klik op **aanpassen:** naast 'Doelabonnement' om te wijzigen van het doelabonnement standaard. Selecteer het abonnement uit de lijst met alle abonnementen die beschikbaar zijn in dezelfde Azure Active Directory (AAD)-tenant.

2. Klik op **aanpassen:** naast ' Resource group, netwerk, opslag en beschikbaarheidssets wijzigen de onderstaande standaardinstellingen:
    - In **doelresourcegroep**, selecteer de resourcegroep in de lijst met alle resourcegroepen in de doellocatie van het abonnement.
    - In **virtueel doelnetwerk**, het netwerk selecteren uit een lijst van het virtuele netwerk op de doellocatie.
    - In **beschikbaarheidsset**, u beschikbaarheid instellen op de virtuele machine, kunt toevoegen als ze deel uitmaken van een beschikbaarheidsset in de bronregio.
    - In **Doelopslag accounts**, selecteert u het account dat u wilt gebruiken.


2. Klik op **aanpassen:** naast Encryption-instellingen wijzigen de onderstaande standaardinstellingen:
    - In **doel schijf versleuteling sleutelkluis**, selecteert u de sleutelkluis voor doel schijf versleuteling uit de lijst met alle key vaults in de doellocatie van het abonnement.
  - In **doel key-versleuteling sleutelkluis**, selecteert u de key vault voor doel-key-versleuteling in de lijst met alle key vaults in de doellocatie van het abonnement.

3. Klik op **maken doelresource** > **inschakelen replicatie**.
4. Nadat de virtuele machines zijn ingeschakeld voor replicatie, kunt u de status van de status van de virtuele machine onder controleren **gerepliceerde items**

>[!NOTE]
>Tijdens de eerste replicatie kan de status even om te vernieuwen, zonder dat wordt uitgevoerd. Klik op de **vernieuwen** knop, de meest recente status ophalen.
>

## <a name="update-target-vm-encryption-settings"></a>Versleutelingsinstellingen voor doel-VM bijwerken
In de onderstaande scenario's, moet u de versleutelingsinstellingen van de doel-VM bijwerken.
  - U Site Recovery-replicatie op de virtuele machine ingeschakeld en Azure Disk Encryption (ADE) op de bron-VM op een later tijdstip ingeschakeld
  - U Site Recovery-replicatie op de virtuele machine ingeschakeld en de versleutelingssleutel en/of de sleutel van versleutelingssleutel op de bron-VM op een later tijdstip worden gewijzigd

U kunt [het script](#copy-ade-keys-to-dr-region-using-powershell-script) te kopiëren van de versleutelingssleutels naar doelregio en werk vervolgens de instellingen van de doel-codering in **Recovery services-kluis -> gerepliceerd item -> Eigenschappen -> Compute en netwerk.**

![update-ade-settings](./media/azure-to-azure-how-to-enable-replication-ade-vms/update-ade-settings.png)

## <a name="trusted-root-certificates-error-code-151066"></a>Oplossen van problemen met de machtigingen van de Key vault tijdens de replicatie van virtuele machines van Azure naar Azure

**1 oorzaak:** Mogelijk hebt u een gemaakte Keyvault geselecteerd in de doelregio dat niet beschikt over de vereiste machtigingen.
Als u een gemaakte Keyvault selecteert in de doelregio plaats kunt maken van Azure Site Recovery. Zorg ervoor dat de Key vault is vereist machtigingen als hierboven.</br>
*Bijvoorbeeld*: Een gebruiker probeert te repliceren van een virtuele machine met een key vault op bronregio bijvoorbeeld 'ContososourceKeyvault'.
Gebruiker heeft de machtiging all op de bron regio key vault, maar tijdens beveiliging hij selecteert een gemaakte sleutelkluis 'ContosotargetKeyvault', dat niet is gemachtigd en -beveiliging wordt een fout genereert.</br>
**Over het oplossen van:** Heb ' Start > Keyvaults > ContososourceKeyvault > beleid voor gegevenstoegang ' en voeg machtigingen toe zoals hierboven wordt weergegeven. 

**2 oorzaak:** U hebt geselecteerd een gemaakte Keyvault van de doelregio die niet zijn decry pt-versleutelen machtigingen.
Als u een gemaakte Keyvault selecteert in de doelregio plaats kunt maken van Azure Site Recovery. Zorg ervoor dat de gebruiker heeft decoderen-versleutelen machtigingen in het geval u bij het versleutelen van de sleutel te in de bronregio.</br>
*Bijvoorbeeld*: Een gebruiker probeert te repliceren van een virtuele machine met een key vault op bronregio bijvoorbeeld 'ContososourceKeyvault'.
Gebruiker heeft de machtiging all op de bron regio key vault, maar tijdens beveiliging selecteert hij een gemaakte sleutelkluis 'ContosotargetKeyvault', niet is gemachtigd om te ontsleutelen en te versleutelen.</br>
**Over het oplossen van:** Hebt u op ' Start > Keyvaults > ContososourceKeyvault > beleid voor gegevenstoegang ' en machtigingen onder de sleutel machtigingen toevoegen > cryptografische bewerkingen.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie](site-recovery-test-failover-to-azure.md) over het uitvoeren van een testfailover.
