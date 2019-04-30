---
title: Configureren van replicatie voor Azure Disk Encryption ingeschakeld virtuele machines in Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe u replicatie voor Azure Disk Encryption ingeschakeld virtuele machines van de ene Azure-regio naar een andere configureren met behulp van Site Recovery.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/08/2019
ms.author: sutalasi
ms.openlocfilehash: 4943b730bb46ee00200d84faf95a7ccb069d3aa8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60790992"
---
# <a name="replicate-azure-disk-encryption-enabled-virtual-machines-to-another-azure-region"></a>Azure Disk Encryption ingeschakeld virtuele machines naar een andere Azure-regio repliceren

In dit artikel wordt beschreven hoe u virtuele machines van Azure Disk Encryption-functionaliteit van een Azure-regio repliceren naar een andere.

>[!NOTE]
>Azure Site Recovery ondersteunt momenteel alleen Azure-VM's waarop een Windows-besturingssysteem wordt uitgevoerd en die zijn [ingeschakeld voor de versleuteling met Azure Active Directory (Azure AD)](https://aka.ms/ade-aad-app).

## <a name="required-user-permissions"></a>Vereiste machtigingen
Site Recovery moet de gebruiker machtigingen hebben voor het maken van de key vault in de doel-regio en kopieert u de sleutels naar de regio.

Als u wilt inschakelen replicatie van virtuele machines van Azure portal schijfversleuteling is ingeschakeld, moet de gebruiker de volgende machtigingen:

- Key vault-machtigingen
    - Lijst
    - Maken
    - Ophalen

-   Sleutelkluis geheime machtigingen
    - Lijst
    - Maken
    - Ophalen

- Sleutelkluis sleutelmachtigingen (alleen als de virtuele machines sleutel van versleutelingssleutel gebruikt voor het versleutelen van sleutels vereist)
    - Lijst
    - Ophalen
    - Maken
    - Versleutelen
    - Ontsleutelen

Voor het beheren van machtigingen, gaat u naar de sleutelkluis-resource in de portal. Voeg de vereiste machtigingen voor de gebruiker. Het volgende voorbeeld laat zien hoe machtigingen tot de key vault inschakelen *ContosoWeb2Keyvault*, deze bevindt zich in de bronregio.

1. Ga naar **Start** > **Keyvaults** > **ContosoWeb2KeyVault > beleid voor gegevenstoegang**.

   ![Machtigingenvenster sleutelkluis](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-1.png)

2. U kunt zien dat er geen gebruikersmachtigingen. Selecteer **nieuwe toevoegen**. Geef de machtigingen voor gebruikers en informatie.

   ![Key Vault-machtigingen](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-2.png)

Als de gebruiker die het inschakelen van herstel na noodgeval (DR) heeft geen machtigingen voor het kopiëren van de sleutels zijn, kan een security-beheerder die de juiste machtigingen heeft het volgende script gebruiken om te kopiëren van de versleuteling geheimen en sleutels naar de doelregio.

Raadpleeg voor het oplossen van machtigingen, [belangrijke problemen met de machtigingen van de kluis](#trusted-root-certificates-error-code-151066) verderop in dit artikel.

>[!NOTE]
>Als u wilt inschakelen replicatie van virtuele machines uit de portal voor schijfversleuteling is ingeschakeld, moet u ten minste "weergeven' machtigingen voor de key vaults, geheimen en sleutels.

## <a name="copy-disk-encryption-keys-to-the-dr-region-by-using-the-powershell-script"></a>Versleutelingssleutels schijf kopiëren naar de DR-regio met behulp van het PowerShell-script

1. [Open de onbewerkte scriptcode "CopyKeys"](https://aka.ms/ade-asr-copy-keys-code).
2. Kopieer het script naar een bestand en geef deze de naam **kopie keys.ps1**.
3. Open de Windows PowerShell-toepassing en Ga naar de map waar u het bestand hebt opgeslagen.
4. Kopiëren-keys.ps1 worden uitgevoerd.
5. Geef Azure-referenties aan te melden.
6. Selecteer de **Azure-abonnement** van uw VM's.
7. Wacht tot de resourcegroepen te laden en selecteer vervolgens de **resourcegroep** van uw VM's.
8. Selecteer de virtuele machines in de lijst die wordt weergegeven. Alleen virtuele machines die zijn ingeschakeld voor schijfversleuteling zijn in de lijst.
9. Selecteer de **doellocatie**.

    - **Schijf versleuteling sleutelkluizen**
    - **Sleutelkluizen Key-versleuteling**

   Site Recovery maakt standaard een nieuwe sleutelkluis in de doelregio. De naam van de kluis is een achtervoegsel 'asr' die gebaseerd op de versleutelingssleutels voor bron VM-schijf. Als er al een key vault die is gemaakt door Site Recovery bestaat, wordt dit opnieuw gebruikt. Selecteer een andere key vault in de lijst indien nodig.

## <a name="enable-replication"></a>Replicatie inschakelen

In dit voorbeeld de primaire Azure-regio is Oost-Azië en de secundaire regio is Zuidoost-Azië.

1. Selecteer in de kluis **+ repliceren**.
2. Houd er rekening mee de volgende velden.
    - **Bron**: Het punt van herkomst van de virtuele machines, die in dit geval **Azure**.
    - **Bronlocatie**: De Azure-regio waar u wilt beveiligen van uw virtuele machines. In dit voorbeeld is de locatie van de "Oost-Azië."
    - **Implementatiemodel**: De Azure-implementatiemodel van de bronmachines.
    - **Abonnement van bron**: Het abonnement waartoe de virtuele bronmachines behoren. Elk abonnement dat zich in dezelfde Azure Active Directory-tenant als de recovery services-kluis kan zijn.
    - **Resourcegroep**: De resourcegroep waartoe de virtuele bronmachines behoren. Alle virtuele machines in de geselecteerde resourcegroep worden weergegeven voor beveiliging in de volgende stap.

3. In **virtuele Machines** > **virtuele machines selecteren**, selecteert u elke virtuele machine die u wilt repliceren. U kunt alleen machines selecteren waarvoor replicatie kan worden ingeschakeld. Selecteer vervolgens **OK**.

4. In **instellingen**, kunt u de volgende target-site-instellingen configureren.

    - **Doellocatie**: De locatie waar de brongegevens van de virtuele machine worden gerepliceerd. Site Recovery biedt een lijst met geschikte doelregio's op basis van de locatie van de geselecteerde computer. U wordt aangeraden dat u dezelfde locatie als locatie voor de Recovery Services-kluis gebruiken.
    - **Doelabonnement**: Het doelabonnement dat wordt gebruikt voor herstel na noodgevallen. Het doelabonnement is standaard hetzelfde als de bronabonnement.
    - **Doelresourcegroep**: De resourcegroep waarin alle uw gerepliceerde virtuele machines behoren. Site Recovery maakt standaard een nieuwe resourcegroep in de doelregio. De naam van de haalt het achtervoegsel 'asr'. Als er al een resourcegroep die is gemaakt door Azure Site Recovery bestaat, wordt dit opnieuw gebruikt. U kunt ook om aan te passen, zoals wordt weergegeven in de volgende sectie. De locatie van de doelresourcegroep kan zijn van andere Azure-regio's met uitzondering van de regio waar de virtuele bronmachines worden gehost.
    - **Virtueel doelnetwerk**: Site Recovery maakt standaard een nieuw virtueel netwerk in de doelregio. De naam van de haalt het achtervoegsel 'asr'. Deze is toegewezen aan de bron-netwerk en gebruikt voor eventuele toekomstige beveiliging. [Meer informatie](site-recovery-network-mapping-azure-to-azure.md) over netwerktoewijzing.
    - **Doelopslagaccounts (als de bron-VM geen gebruik maakt van schijven beheerde)**: Site Recovery maakt standaard een nieuw doel-opslagaccount door mimicking de opslagconfiguratie van de bron-VM. Als een storage-account al bestaat, wordt dit opnieuw gebruikt.
    - **Beheerde replicaschijven (als de bron-VM gebruikmaakt van beheerde schijven)**: Site Recovery maakt nieuwe beheerde replicaschijven in de doelregio voor het spiegelen van beheerde schijven van de hetzelfde opslagtype (standard of premium) als de bron-VM-beheerde schijven van de bron-VM.
    - **Cacheopslagaccounts**: Site Recovery moet een extra opslagaccount met de naam *opslag in de cache* in de bronregio. De wijzigingen op de bron-VM's worden bijgehouden en verzonden naar het cacheopslagaccount. Ze worden vervolgens gerepliceerd naar de doellocatie.
    - **Beschikbaarheidsset**: Site Recovery maakt standaard een nieuwe beschikbaarheidsset in de doelregio. De naam van de heeft het achtervoegsel 'asr'. Als een beschikbaarheidsset die is gemaakt door Site Recovery al bestaat, wordt dit opnieuw gebruikt.
    - **Sleutelkluizen voor schijfversleuteling**: Site Recovery maakt standaard een nieuwe sleutelkluis in de doelregio. Heeft het achtervoegsel 'asr' die gebaseerd op de versleutelingssleutels voor bron VM-schijf. Als een key vault die is gemaakt door Azure Site Recovery al bestaat, wordt dit opnieuw gebruikt.
    - **Sleutelkluizen voor versleutelingssleutel**: Site Recovery maakt standaard een nieuwe sleutelkluis in de doelregio. De naam is een achtervoegsel 'asr' die gebaseerd op de bron VM key-versleutelingssleutels. Als een sleutelkluis gemaakt door Azure Site Recovery al bestaat, wordt dit opnieuw gebruikt.
    - **Beleid voor wachtwoordreplicatie**: Definieert de instellingen voor bewaren van herstelpuntgeschiedenis en frequentie van de app-consistente momentopname. Site Recovery maakt standaard een nieuw replicatiebeleid met de standaardinstellingen van *24 uur* wijst bewaren voor herstel en *60 minuten* voor de frequentie van app-consistente momentopname.

## <a name="customize-target-resources"></a>Doelresources aanpassen

Volg deze stappen voor het hulpprogramma voor het doel van de Site Recovery-standaardinstellingen wijzigen.

1. Selecteer **aanpassen** naast 'Doelabonnement' om te wijzigen van het doelabonnement standaard. Selecteer het abonnement uit de lijst met abonnementen die beschikbaar in de Azure AD-tenant zijn.

2. Selecteer **aanpassen** naast 'resourcegroep, netwerk, opslag en beschikbaarheidssets' om te wijzigen van de volgende standaardinstellingen:
    - Voor **doelresourcegroep**, selecteer de resourcegroep in de lijst met resourcegroepen in de doellocatie van het abonnement.
    - Voor **virtueel doelnetwerk**, selecteert u het netwerk in een lijst met virtuele netwerken op de doellocatie.
    - Voor **beschikbaarheidsset**, u beschikbaarheid instellen op de virtuele machine, kunt toevoegen als ze deel uitmaken van een beschikbaarheidsset in de bronregio.
    - Voor **Doelopslag accounts**, selecteert u het account moet worden gebruikt.

2. Selecteer **aanpassen** naast 'Versleutelingsinstellingen' om te wijzigen van de volgende standaardinstellingen:
   - Voor **doel schijf versleuteling sleutelkluis**, selecteert u de sleutelkluis voor doel schijf versleuteling uit de lijst met sleutelkluizen in de doellocatie van het abonnement.
   - Voor **doel key-versleuteling sleutelkluis**, selecteert u de key vault voor doel-key-versleuteling in de lijst met sleutelkluizen in de doellocatie van het abonnement.

3. Selecteer **maken doelresource** > **inschakelen replicatie**.
4. Nadat de virtuele machines zijn ingeschakeld voor replicatie, kunt u de integriteitsstatus van de virtuele machines onder controleren **gerepliceerde items**.

>[!NOTE]
>Tijdens de initiële replicatie, kan de status even om te vernieuwen, zonder duidelijk wordt uitgevoerd. Klik op **vernieuwen** om op te halen van de meest recente status.

## <a name="update-target-vm-encryption-settings"></a>Versleutelingsinstellingen voor doel-VM bijwerken
In de volgende scenario's en zult u moeten bijwerken de versleutelingsinstellingen van de doel-VM:
  - U Site Recovery-replicatie op de virtuele machine hebt ingeschakeld. Later, u schijfversleuteling op de bron-VM hebt ingeschakeld.
  - U Site Recovery-replicatie op de virtuele machine hebt ingeschakeld. Later kunt u de versleutelingssleutel van de schijf of de sleutel van versleutelingssleutel op de bron-VM gewijzigd.

U kunt [een script](#copy-disk-encryption-keys-to-the-dr-region-by-using-the-powershell-script) te kopiëren van de versleutelingssleutels naar de doelregio en werk vervolgens de instellingen van de doel-codering in **Recovery services-kluis** > *gerepliceerd item*  >  **Eigenschappen** > **berekening en netwerk**.

![Dialoogvenster ADE-instellingen bijwerken](./media/azure-to-azure-how-to-enable-replication-ade-vms/update-ade-settings.png)

## <a id="trusted-root-certificates-error-code-151066"></a>Oplossen van problemen met de machtigingen van de sleutelkluis tijdens de replicatie van virtuele machines van Azure naar Azure

**1 oorzaak:** U hebt geselecteerd in de doelregio een vooraf gemaakte sleutelkluis die niet beschikt over de vereiste machtigingen in plaats van Site Recovery maakt u er een laten. Zorg ervoor dat de key vault heeft de machtigingen die zijn vereist, zoals eerder beschreven.

*Bijvoorbeeld*: U probeert te repliceren van een virtuele machine waarvoor sleutelkluis *ContososourceKeyvault* op een regio van de gegevensbron.
U hebt alle machtigingen op de bron regio key vault. Maar tijdens de beveiliging, selecteert u de al gemaakte sleutelkluis ContosotargetKeyvault, dat niet beschikt over machtigingen. Er treedt een fout op.

**Over het oplossen van:** Ga naar **Start** > **Keyvaults** > **ContososourceKeyvault** > **toegangsbeleid** en voeg de juiste machtigingen.

**2 oorzaak:** U hebt geselecteerd in de doelregio een vooraf gemaakte sleutelkluis die geen machtigingen in plaats van Site Recovery maakt u er een laten decoderen versleutelen. Zorg ervoor dat u hebt decoderen-versleutelen machtigingen als u ook van de sleutel in de bronregio versleutelen bent.</br>

*Bijvoorbeeld*: U probeert te repliceren van een virtuele machine waarvoor een key vault *ContososourceKeyvault* in de bronregio. U hebt de benodigde machtiging op de bron regio key vault. Maar tijdens de beveiliging, selecteert u de al gemaakte sleutelkluis ContosotargetKeyvault, dat niet beschikt over machtigingen om te ontsleutelen en te versleutelen. Er treedt een fout op.</br>

**Over het oplossen van:** Ga naar **Start** > **Keyvaults** > **ContososourceKeyvault** > **toegangsbeleid**. Voeg machtigingen onder **sleutelmachtigingen** > **cryptografische bewerkingen**.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie](site-recovery-test-failover-to-azure.md) over het uitvoeren van een testfailover.
