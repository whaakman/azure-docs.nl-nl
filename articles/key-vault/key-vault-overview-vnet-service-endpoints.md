---
ms.assetid: ''
title: VNET-Service-eindpunten voor Azure Key Vault | Microsoft Docs
description: Overzicht van Virtual Network-Service-eindpunten voor Key Vault
services: key-vault
author: amitbapat
ms.author: ambapat
manager: mbaldwin
ms.date: 08/31/2018
ms.service: key-vault
ms.workload: identity
ms.topic: conceptual
ms.openlocfilehash: 6e0029e051e418bc54471284547329a0b0a2e9cd
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50246651"
---
# <a name="virtual-network-service-endpoints-for-azure-key-vault"></a>Virtual Network-Service-eindpunten voor Azure Key Vault

De virtual Network-Service-eindpunten voor Key Vault kunt u de toegang beperken op de opgegeven Virtueelnetwerk en/of om een lijst met IPv4 (Internet Protocol versie 4)-adresbereiken. Een aanroeper verbinding te maken met uw key vault vanuit buiten deze bronnen wordt de toegang geweigerd. Als klant heeft gekozen in om toe te staan 'Vertrouwde Microsoft-services', zoals Office 365 Exchange Online, Office 365 SharePoint Online, Azure compute, Azure Resource Manager, enz. back-up van Azure, worden verbindingen van deze services kunt via de firewall. Natuurlijk dergelijke aanroepers nog steeds nodig om weer te geven van een geldige AAD-token en moet beschikken over machtigingen (geconfigureerd als toegangsbeleid) voor het uitvoeren van de aangevraagde bewerking. Lees meer technische informatie over [Virtual Network-Service-eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md).

## <a name="usage-scenarios"></a>Gebruiksscenario's

U kunt configureren [Key Vault-firewalls en virtuele netwerken](key-vault-network-security.md) voor het weigeren van toegang tot verkeer via alle netwerken (met inbegrip van het internetverkeer) standaard. Toegang kan worden verleend aan het verkeer van specifieke Azure-netwerken en/of het openbare internet-IP-adres bereiken, zodat u kunt een beveiligde netwerkgrens voor uw toepassingen te bouwen.

> [!NOTE]
> Key Vault-firewalls en virtuele netwerkregels alleen van toepassing op sleutelkluis [gegevenslaag](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control). Bewerkingen voor Key Vault de controlelaag (zoals een sleutelkluis maken, verwijderen, wijzigen van bewerkingen, instellen van toegangsbeleid, firewalls en virtuele netwerkregels in te stellen) worden niet beïnvloed door firewalls en virtual network-regels.

Bijvoorbeeld:
* Als u Key Vault gebruikt voor het opslaan van versleutelingssleutels, toepassingsgeheimen, certificaten, en u wilt blokkeren van toegang tot uw key vault via openbaar internet.
* U kunt toegang tot uw key vault vergrendelen zodat alleen uw toepassing of een korte lijst van bepaalde hosts verbinding met uw key vault maken kan
* U hebt een toepassing die wordt uitgevoerd in uw Azure-netwerk (VNET) en dit VNET is vergrendeld voor alle binnenkomend en uitgaand verkeer. Uw toepassing moet nog steeds verbinding maken met key vault wilt ophalen van geheimen of certificaten of cryptografische sleutels wilt gebruiken.

## <a name="configure-key-vault-firewalls-and-virtual-networks"></a>Key Vault-firewalls en virtuele netwerken configureren

Hier volgen de stappen die vereist voor het configureren van firewalls en virtuele netwerken. Deze stappen blijven hetzelfde, ongeacht welke interface (PowerShell, CLI, Azure portal) wordt gebruikt voor het instellen van de firewall en regels voor virtueel netwerk.
1. Optioneel maar ten zeerste aanbevolen: inschakelen [logboekregistratie van key vault](key-vault-logging.md) gedetailleerde toegang Logboeken. Dit helpt u bij het diagnostische gegevens over wanneer firewalls en virtuele netwerkregels te voorkomen toegang tot een key vault dat.
2. Service-eindpunten voor key vault voor de doel-virtuele netwerken en subnetten inschakelen
3. Firewalls en virtuele-netwerkregels voor een key vault toegang te beperken die key vault van specifieke virtuele netwerken, subnetten en IPv4-adresbereiken instellen.
4. Als deze sleutelkluis toegankelijk zijn via een vertrouwde Microsoft-services moet, moet u de optie 'vertrouwde Azure-Services toestaan ' verbinding maken met key vault.

Raadpleeg [configureren van Azure Key Vault Firewalls en virtuele netwerken](key-vault-network-security.md) voor gedetailleerde stapsgewijze instructies.

> [!IMPORTANT]
> Wanneer de firewall-regels zijn in feite alle Key Vault [gegevenslaag](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) bewerkingen kunnen alleen worden uitgevoerd wanneer de oproepende functie aanvragen afkomstig zijn uit de toegestane virtuele netwerken of een IPV4-adresbereik. (Voor meer informatie over management vlak vs gegevenslaag Lees [dit](../key-vault/key-vault-secure-your-key-vault.md#management-plane-access-control) dit geldt ook voor toegang tot key vault vanuit Azure portal. Wanneer een gebruiker kan de browser tot een key vault vanuit Azure portal, ze niet mogelijk aan de lijst met sleutels/geheimen/certificaten als hun clientmachine zich niet in de lijst met toegestane. Dit ook van invloed op de 'Key Vault kiezer' door andere Azure-services. Gebruikers mogelijk om te bekijken van de lijst met sleutelkluizen maar sleutels, niet wordt vermeld als de firewall-regels te voorkomen dat de client-computer.


> [!NOTE]
> * Een maximaal 127 VNET-regels en 127 IPv4-regels zijn toegestaan. 
> * Kleine adresbereiken met behulp van '/ 31' of '/ 32' voorvoegsel grootten worden niet ondersteund. Deze bereiken moeten worden geconfigureerd met behulp van afzonderlijke regels voor IP-adres.
> * IP-netwerkregels zijn alleen toegestaan voor openbare IP-adressen. IP-adresbereiken is gereserveerd voor particuliere netwerken (zoals gedefinieerd in RFC 1918) zijn niet toegestaan in IP-regels. Particuliere netwerken bevatten adressen die met beginnen * 10.* *, * 172.16. **, en * 192.168. **. 
> * Alleen IPv4-adressen worden ondersteund op dit moment.

## <a name="trusted-services"></a>Betrouwbare services
Hier volgt een lijst met vertrouwde-services die zijn toegestaan voor toegang tot een key vault als de optie 'Vertrouwde services toestaan' is ingeschakeld.

|Vertrouwde service|Gebruiksscenario's|
| --- | --- |
|Service voor implementatie van Azure Virtual Machines|[Certificaten implementeren op virtuele machines uit de klant beheerde Key Vault](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/)|
|Service voor implementatie Azure Resource Manager-sjablonen|[Beveiligde waarden doorgeven tijdens implementatie](../azure-resource-manager/resource-manager-keyvault-parameter.md)|
|Service voor versleuteling voor volumes van Azure Disk Encryption|Toegang tot BitLocker Key (Windows-VM) of DM wachtwoordzin (Linux-VM) en Key-versleutelingssleutel toestaan tijdens de implementatie van de virtuele machine om in te schakelen [Azure Disk Encryption](../security/azure-security-disk-encryption.md)|
|Azure Backup|Toestaan dat back-up en herstel van de relevante sleutels en geheimen tijdens back-up van virtuele machine van Azure, met behulp van [Azure Backup](../backup/backup-introduction-to-azure-backup.md)|
|Exchange Online en SharePoint Online|Toegang tot de klantsleutel toestaan voor Service-versleuteling met [Klantsleutel](https://support.office.com/article/Controlling-your-data-in-Office-365-using-Customer-Key-f2cd475a-e592-46cf-80a3-1bfb0fa17697).|
|Azure Information Protection|Toegang tot de tenantsleutel voor [Azure Information Protection.](https://docs.microsoft.com/azure/information-protection/what-is-information-protection)|
|App-services|[Azure Web App Certificate via Key Vault implementeren](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)|
|Azure SQL|[De Transparent Data Encryption met Bring Your Own Key-ondersteuning voor Azure SQL Database en Data Warehouse](../sql-database/transparent-data-encryption-byok-azure-sql.md?view=sql-server-2017&viewFallbackFrom=azuresqldb-current)|
|Azure Storage|[Versleuteling voor opslagservice met behulp van de klant beheerde sleutels in Azure Key Vault](../storage/common/storage-service-encryption-customer-managed-keys.md)|
|Azure Data Lake Store|[Versleuteling van gegevens in Azure Data Lake Store](../data-lake-store/data-lake-store-encryption.md) met de klant beheerde sleutel|



> [!NOTE]
> Het toegangsbeleid van de relevante sleutelkluis moeten worden ingesteld om de bijbehorende services om toegang tot key vault te krijgen.

## <a name="next-steps"></a>Volgende stappen

* [Uw Key Vault beveiligen](key-vault-secure-your-key-vault.md)
* [Azure Key Vault-Firewalls en virtuele netwerken configureren](key-vault-network-security.md)
