---
title: Maken van WSFC, listener en ILB configureren voor de groep van een Always On-beschikbaarheid op een SQL Server-VM met de Azure Quickstart-sjabloon
description: Gebruik Azure Quickstart-sjablonen voor het vereenvoudigen van het proces voor het maken van beschikbaarheidsgroepen voor SQL Server-VM's in Azure met behulp van een sjabloon te maken van het cluster, SQL-VM's aan het cluster worden toegevoegd, de listener maken en configureren van de ILB.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/04/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 9be8717bc9b1d15a59486edf206dd0657a711c06
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54360280"
---
# <a name="create-wsfc-listener-and-configure-ilb-for-an-always-on-availability-group-on-a-sql-server-vm-with-azure-quickstart-template"></a>Maken van WSFC, listener en ILB configureren voor de groep van een Always On-beschikbaarheid op een SQL Server-VM met de Azure Quickstart-sjabloon
Dit artikel wordt beschreven hoe u met de Azure-Snelstartsjablonen gedeeltelijk de implementatie van een Always On configuratie beschikbaarheidsgroep voor SQL Server Virtual Machines in Azure te automatiseren. Er zijn twee Azure Quickstart-sjablonen die worden gebruikt in dit proces. 

   | Template | Description |
   | --- | --- |
   | [101-sql-vm-ag-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-ag-setup) | Het Windows-failovercluster maakt en lid wordt van de SQL Server-VM's toe. |
   | [101-sql-vm-aglistener-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-aglistener-setup) | De beschikbaarheidsgroep-listener maakt en configureert u de interne Load Balancer. |
   | &nbsp; | &nbsp; |

Andere onderdelen van de configuratie van de beschikbaarheidsgroep moeten handmatig worden uitgevoerd, zoals het maken van de beschikbaarheidsgroep en de interne Load Balancer is gemaakt. Dit artikel bevat de volgorde van de automatische en handmatige stappen.
 

## <a name="prerequisites"></a>Vereisten 
Voor het automatiseren van de installatie van een Always On-beschikbaarheidsgroep met behulp van de Quick Start-sjablonen, hebt u al de volgende vereisten: 
- Een [Azure-abonnement](https://azure.microsoft.com/free/).
- Een resourcegroep met een [domeincontroller](https://docs.microsoft.com/azure/architecture/reference-architectures/identity/adds-forest). 
- Een of meer domein [virtuele machines in Azure uitgevoerd SQL Server 2016 (of hoger) Enterprise edition](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) in dezelfde set of beschikbaarheid beschikbaarheidszone die zijn [geregistreerd bij de SQL-VM-resourceprovider](#register-existing-sql-vm-with-new-resource-provider).  

## <a name="register-existing-sql-vm-with-new-resource-provider"></a>Bestaande SQL-VM met nieuwe resourceprovider registreren
Sinds deze beschikbaarheidsgroep Azure Quickstart-sjablonen zijn afhankelijk van de SQL-VM resourceprovider (Microsoft.SqlVirtualMachine), moet de bestaande SQL Server-VM's worden geregistreerd bij de resourceprovider van SQL-VM. Deze stap overslaan als u uw SQL Server-VM na December 2018, als alle SQL Server-VM's die zijn gemaakt gemaakt na deze datum worden automatisch geregistreerd. Deze sectie bevat stappen om u te registreren bij de provider met behulp van de Azure-portal, maar u kunt ook [PowerShell](virtual-machines-windows-sql-ahb.md#powershell). 

  >[!IMPORTANT]
  > Als u uw SQL Server-VM-resource, gaat u terug naar de vastgelegde licentie-instelling van de installatiekopie. 

1. Open de Azure-portal en Ga naar **alle Services**. 
1. Navigeer naar **abonnementen** en selecteer het abonnement van belang zijn.  
1. In de **abonnementen** blade, gaat u naar **Resourceprovider**. 
1. Type `sql` in het filter om de SQL-gerelateerde resourceproviders. 
1. Selecteer een *registreren*, *opnieuw registreren*, of *registratie* voor de **Microsoft.SqlVirtualMachine** provider afhankelijk van uw gewenste actie. 

  ![De provider wijzigen](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

## <a name="step-1---create-the-wsfc-and-join-sql-server-vms-to-the-cluster-using-quickstart-template"></a>Stap 1: de WSFC maken en SQL Server-VM's toevoegen aan het cluster met behulp van quickstart-sjabloon 
Zodra uw SQL Server-VM's zijn geregistreerd met de nieuwe resourceprovider voor SQL-VM, kunt u deelnemen aan uw SQL Server-VM's in *SqlVirtualMachineGroup*. Deze bron wordt gedefinieerd voor de metagegevens van het Windows-failovercluster, met inbegrip van de versie, versie, Fully Qualified Domain Name, AD-accounts voor het beheren van het cluster en de Storage-Account als de Cloud-Witness. Toevoegen van de SQL Server-VM naar de *SqlVirtualMachineGroup* bootstrapt van de Windows Failover Cluster-Service en de SQL Server-VM's aan wordt toegevoegd aan het cluster. Deze stap is geautomatiseerd met de **101-sql-vm-ag-setup** quickstart-sjabloon en kunnen worden geïmplementeerd met de volgende stappen uit:

1. Navigeer naar de [ **101-sql-vm-ag-setup** ](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-ag-setup) quickstart-sjabloon en selecteer **implementeren in Azure** om te starten van de quickstart-sjabloon in Azure portal.
1. Vul de vereiste velden in de metagegevens van de Windows-failovercluster configureren. De optionele velden kunnen leeg worden gelaten.

    De volgende tabel ziet u de waarden die nodig zijn voor de sjabloon: 

   | **Veld** | Waarde |
   | --- | --- |
   | **Abonnement** |  Het abonnement waarin uw SQL Server-VM's bestaan. |
   |**Resourcegroep** | De resourcegroep waar uw SQL Server-VM's zich bevinden. | 
   |**De naam van Failover-Cluster** | De gewenste naam voor uw nieuwe Windows-failovercluster. |
   | **Bestaande Vm-lijst** | De SQL Server-VM's die u wilt deelnemen aan in de beschikbaarheidsgroep, en als zodanig deel uitmaken van dit nieuwe cluster. Deze waarden scheiden met een komma en een spatie (ex: SQLVM1, SQLVM2). |
   | **SQL Server-versie** | Selecteer de SQL Server-versie van uw SQL Server-VM's in de vervolgkeuzelijst. Momenteel wordt alleen SQL 2016 en 2017 van de SQL-installatiekopieën worden ondersteund. |
   | **Bestaande Fully Qualified Domain Name** | De bestaande FQDN-naam voor het domein waarin uw SQL Server-VM's zich bevinden. |
   | **Bestaande domeinaccount** | Een bestaand domeinaccount dat sysadmin toegang tot de SQL-Server heeft. | 
   | **Wachtwoord voor een domeinaccount** | Het wachtwoord voor de eerder genoemde domeinaccount. | 
   | **Bestaande Sql-serviceaccount** | De domeingebruikersaccount die wordt gebruikt voor het beheren van de SQL Server-service. Deze informatie kan worden gevonden met behulp van de [ **SQL Server Configuration Manager**](https://docs.microsoft.com/sql/relational-databases/sql-server-configuration-manager?view=sql-server-2017). |
   | **Het wachtwoord van SQL-Service** | Het wachtwoord dat wordt gebruikt door de domeingebruikersaccount die Hiermee bepaalt u de SQL Server-service. |
   | &nbsp; | &nbsp; |

1. Als u akkoord met de voorwaarden en bepalingen gaat, selecteert u het selectievakje in naast **ik ga akkoord met de voorwaarden en bepalingen bovenstaande** en selecteer **aankoop** voor het voltooien van de Quick Start-sjabloonimplementatie. 
1. Voor het controleren van uw implementatie, selecteert u de implementatie van de **meldingen** belpictogram in de banner van de bovenste navigatiebalk of Ga naar uw **resourcegroep** in Azure portal, selecteert u  **Implementaties** in de **instellingen** veld en kiest u de implementatie 'Microsoft.Template'. 

## <a name="step-2---manually-create-the-availability-group"></a>Stap 2: de beschikbaarheidsgroep handmatig maken 
De beschikbaarheidsgroep handmatig maken zoals u gewend bent, met behulp van [PowerShell](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell?view=sql-server-2017), [SQL Server Management Studio](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio?view=sql-server-2017) of [Transact-SQL](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/create-an-availability-group-transact-sql?view=sql-server-2017). 

  >[!IMPORTANT]
  > Voer **niet** een listener op dit moment niet maken omdat dit wordt automatisch door de **101-sql-vm-aglistener-setup** quickstart-sjabloon in stap 4. 

## <a name="step-3---manually-create-the-internal-load-balancer-ilb"></a>Stap 3: de interne Load Balancer (ILB) handmatig maken
De Always On (AG) beschikbaarheidsgroeplistener is een interne Azure Load Balancer (ILB) vereist. De ILB biedt een 'zwevende' IP-adres voor de AG-listener die voor een snellere failover en opnieuw verbinden zorgt. Als de SQL Server-VM's in een beschikbaarheidsgroep deel van dezelfde beschikbaarheidsset bevinden, uitmaken kunt u een Basic Load Balancer. anders moet u een Standard Load Balancer gebruiken.  **De ILB moet zich in hetzelfde vNet als de SQL Server-VM-exemplaren.** De ILB moet alleen worden gemaakt, de rest van de configuratie (zoals de back-endpool, statustest en load balancing regels) wordt verwerkt door de **101-sql-vm-aglistener-setup** quickstart-sjabloon in stap 4. 

1. Open de resourcegroep met de SQL Server-machines in Azure portal. 
2. Klik in de resourcegroep op **toevoegen**.
3. Zoeken naar **netwerktaakverdeler** en selecteer vervolgens in de lijst met zoekresultaten **Load Balancer**, die is gepubliceerd via **Microsoft**.
4. Op de **Load Balancer** blade, klikt u op **maken**.
5. In de **load balancer maken** dialoogvenster vak, configureer de load balancer als volgt:

   | Instelling | Waarde |
   | --- | --- |
   | **Naam** |Een naam voor de load balancer. Bijvoorbeeld, **sqlLB**. |
   | **Type** |**Interne**: De meeste implementaties gebruikt een interne load balancer, waardoor toepassingen binnen hetzelfde virtuele netwerk verbinding maken met de beschikbaarheidsgroep.  </br> **Externe**: Zorgt ervoor dat toepassingen verbinding maken met de beschikbaarheidsgroep via een openbare internetverbinding. |
   | **Virtueel netwerk** |Selecteer het virtuele netwerk die de SQL Server-exemplaren in. |
   | **Subnet** |Selecteer het subnet die de SQL Server-exemplaren in. |
   | **IP-adrestoewijzing** |**Statische** |
   | **Privé IP-adres** |Geef een beschikbaar IP-adres van het subnet. Gebruik dit IP-adres wanneer u een listener op het cluster maakt.|
   | **Abonnement** |Als u meerdere abonnementen hebt, kan dit veld weergegeven. Selecteer het abonnement dat u wilt koppelen aan deze resource. Het is normaal hetzelfde abonnement als alle resources voor de beschikbaarheidsgroep. |
   | **Resourcegroep** |Selecteer de resourcegroep die de SQL Server-exemplaren in. |
   | **Locatie** |Selecteer de Azure-locatie hebben als de SQL Server-exemplaren. |
   | &nbsp; | &nbsp; |

6. Selecteer **Maken**. 


  >[!NOTE]
  > De openbare IP-adresresource voor elke virtuele machine van SQL Server moet een standaard-SKU voor compatibiliteit met de Standard Load Balancer hebben. Om te bepalen van de SKU van de openbare IP-resource van de virtuele machine, gaat u naar uw **resourcegroep**, selecteer uw **openbaar IP-adres** resource voor de gewenste SQL Server-VM, en zoek de waarde onder **SKU**  van de **overzicht** deelvenster. 

## <a name="step-4---create-the-ag-listener-and-configure-the-ilb-with-the-quickstart-template"></a>Stap 4: de AG-listener maken en configureren van de ILB met de quickstart-sjabloon

De beschikbaarheidsgroep-listener maken en configureren van de interne Load Balancer (ILB) automatisch met de **101-sql-vm-aglistener-setup** quickstart-sjabloon zoals deze bepalingen Microsoft.SqlVirtualMachine/Sql virtuele Bron van de machine-groepen/beschikbaarheidsgroep-Listener. De **101-sql-vm-aglistener-setup** quickstart-sjabloon, via de SQL-VM-resourceprovider biedt de volgende acties:

 - Hiermee configureert u de instellingen voor het cluster en de ILB. 
 - Hiermee configureert u de ILB-back-endpool, statustest en load balancing regels.
 - De AG-listener maakt met het opgegeven IP-adres en de naam.

De ILB configureren en de AG-listener maakt, doet u het volgende:
1. Navigeer naar de [ **101-sql-vm-aglistener-setup** ](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-aglistener-setup) quickstart-sjabloon en selecteer **implementeren in Azure** om te starten van de quickstart-sjabloon in Azure portal.
1. Vul de vereiste velden voor het configureren van de ILB en de beschikbaarheidsgroep-listener maken. De optionele velden kunnen leeg worden gelaten. 

    De volgende tabel ziet u de waarden die nodig zijn voor de sjabloon: 

   | **Veld** | Waarde |
   | --- | --- |
   |**Resourcegroep** | De resourcegroep waar uw SQL Server-VM's en de beschikbaarheidsgroep bestaat. | 
   |**De naam van een bestaande failovercluster** | De naam van het cluster dat u uw SQL Server-VM's zijn gekoppeld aan. |
   | **Bestaande Sql-beschikbaarheidsgroep**| De naam van de beschikbaarheidsgroep die uw SQL Server-VM's deel uit van maken. |
   | **Bestaande Vm-lijst** | De namen van de SQL Server-VM's die deel van de eerder genoemde beschikbaarheidsgroep uitmaken. De namen moeten worden gescheiden door een komma en een spatie (ex: SQLVM1, SQLVM2). |
   | **Bestaande Fully Qualified Domain Name** | De bestaande FQDN-naam voor het domein waarin uw SQL Server-VM's zich bevinden. |
   | **Listener** | De DNS-naam die u wilt toewijzen aan de listener. Deze sjabloon bevat de naam aglistener standaard, maar kan worden gewijzigd. |
   | **-Listener-poort** | De poort die u wilt dat de listener te gebruiken. Normaal gesproken deze poort moet de standaardpoort 1433 en als zodanig, dit is het poortnummer dat is opgegeven door deze sjabloon. Echter moet, als de standaardpoort is gewijzigd, klikt u vervolgens de listener-poort gebruiken die waarde in plaats daarvan. | 
   | **Bestaand Vnet** | De naam van de vNet waar uw SQL Server-VM's en de ILB zich bevinden. |
   | **Bestaande Subnet** | De *naam* van het interne subnet van uw SQL Server-VM's (ex: standaard). Deze waarde kan worden bepaald door te navigeren naar uw **resourcegroep**, selecteer uw **vNet**, Klik daarvoor **subnetten** onder de **instellingen**deelvenster en kopieer de waarde onder **naam**. |
   | **Bestaande interne Load Balancer** | De naam van de ILB die u hebt gemaakt in stap 3. |
   | **Testpoort** | De testpoort die u wilt dat de ILB om te gebruiken. De sjabloon maakt gebruik van 59999 standaard, maar deze waarde kan worden gewijzigd. |
   | &nbsp; | &nbsp; |

1. Als u akkoord met de voorwaarden en bepalingen gaat, selecteert u het selectievakje in naast **ik ga akkoord met de voorwaarden en bepalingen bovenstaande** en selecteer **aankoop** voor het voltooien van de Quick Start-sjabloonimplementatie. 
1. Voor het controleren van uw implementatie, selecteert u de implementatie van de **meldingen** belpictogram in de banner van de bovenste navigatiebalk of Ga naar uw **resourcegroep** in Azure portal, selecteert u  **Implementaties** in de **instellingen** veld en kiest u de implementatie 'Microsoft.Template'. 

  >[!NOTE]
  >Als uw implementatie halverwege mislukt, u moet handmatig [verwijderen van de zojuist gemaakte listener](#remove-availability-group-listener) voordat het opnieuw te implementeren met behulp van PowerShell het **101-sql-vm-aglistener-setup** quickstart-sjabloon. 

## <a name="remove-availability-group-listener"></a>-Listener voor beschikbaarheidsgroep verwijderen
Als u later verwijderen van de beschikbaarheidsgroep-listener geconfigureerd door de sjabloon wilt, moet u de SQL-VM-resourceprovider doorlopen. Omdat de listener is geregistreerd via de SQL-VM-resourceprovider, is deze alleen worden verwijderd via SQL Server Management Studio onvoldoende. Daadwerkelijk moet worden verwijderd via de SQL-VM-resourceprovider met behulp van PowerShell. In dat geval wordt de metagegevens van de Beschikbaarheidsgroep-listener verwijderd uit de SQL-VM-resourceprovider en verwijdert de listener fysiek uit de beschikbaarheidsgroep. 

Het volgende codefragment wordt de SQL-listener voor beschikbaarheidsgroep verwijderd van zowel de SQL-resourceprovider en van de beschikbaarheidsgroep: 

```PowerShell
# Remove the AG listener
# example: Remove-AzureRmResource -ResourceId '/subscriptions/a1a11a11-1a1a-aa11-aa11-1aa1a11aa11a/resourceGroups/SQLAG-RG/providers/Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/Cluster/availabilitygrouplisteners/aglistener' -Force
Remove-AzureRmResource -ResourceId '/subscriptions/<SubscriptionID>/resourceGroups/<resource-group-name>/providers/Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/<cluster-name>/availabilitygrouplisteners/<listener-name>' -Force
```
 
## <a name="known-issues-and-errors"></a>Bekende problemen en fouten
In deze sectie wordt beschreven enkele bekende problemen en hun mogelijk worden opgelost. 

### <a name="availability-group-listener-for-availability-group-ag-name-already-exists"></a>Beschikbaarheidsgroep-listener voor de beschikbaarheidsgroep '\<AG-naam >' bestaat al
De geselecteerde beschikbaarheidsgroep die al wordt gebruikt in de AG-listener Azure Quickstart-sjabloon bevat een listener, fysiek in de beschikbaarheidsgroep, of als metagegevens in de SQL-VM-resourceprovider.  Verwijderen van de listener met behulp van [PowerShell](#remove-availability-group-listener) voordat u opnieuw wilt implementeren de **101-sql-vm-aglistener-setup** quickstart-sjabloon. 

### <a name="connection-only-works-from-primary-replica"></a>Verbinding werkt alleen met primaire replica
Dit gedrag is het waarschijnlijk in een mislukte **101-sql-vm-aglistener-setup** sjabloonimplementatie verlaten van de ILB-configuratie in een inconsistente status. Controleer of dat de back-endpool wordt de lijst met beschikbaarheid, en dat regels bestaan voor de statustest en voor de taakverdelingsregels. Als er iets ontbreekt, is de configuratie van de ILB een inconsistente toestand terechtkomen. 

U lost dit probleem, verwijdert u de listener met behulp van [PowerShell](#remove-availability-group-listener), de interne Load Balancer via Azure portal verwijderen en opnieuw beginnen [stap 3](#step-3---manually-create-the-internal-load-balanced-ilb). 

### <a name="badrequest---only-sql-virtual-machine-list-can-be-updated"></a>BadRequest - lijst van de virtuele machine alleen SQL kan worden bijgewerkt
Deze fout kan optreden bij het implementeren van de **101-sql-vm-aglistener-setup** sjabloon als de listener via SQL Server Management Studio (SSMS) is verwijderd, maar is niet verwijderd van de SQL-VM-resourceprovider. Verwijderen van de listener via SSMS verwijdert niet de metagegevens van de listener van de resourceprovider van SQL-VM; de listener moet worden verwijderd uit de resourceprovider met behulp van [PowerShell](#remove-availability-group-listener). 


## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor meer informatie de volgende artikelen: 

* [Overzicht van SQL Server-machine](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server-VM-Veelgestelde vragen](virtual-machines-windows-sql-server-iaas-faq.md)
* [SQL Server-VM prijsinformatie](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Opmerkingen bij de release van SQL Server-VM](virtual-machines-windows-sql-server-iaas-release-notes.md)
* [Switch licentiemodellen voor een SQL Server-VM](virtual-machines-windows-sql-ahb.md)



