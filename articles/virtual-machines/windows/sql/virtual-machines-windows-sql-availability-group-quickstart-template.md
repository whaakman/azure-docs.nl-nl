---
title: Gebruik Azure-Snelstartsjablonen voor AlwaysOn-beschikbaarheidsgroep configureren voor SQL Server op een Azure VM
description: Gebruik Azure Quickstart-sjablonen te maken van het Windows-failovercluster, SQL Server-VM's aan het cluster worden toegevoegd, de listener te maken en configureren van de interne Load Balancer in Azure.
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
ms.date: 01/04/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 5390885ccb4bbc3e1552d3f5e80c1b451b7bee38
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57570161"
---
# <a name="use-azure-quickstart-templates-to-configure-always-on-availability-group-for-sql-server-on-an-azure-vm"></a>Gebruik Azure-Snelstartsjablonen voor AlwaysOn-beschikbaarheidsgroep configureren voor SQL Server op een Azure VM
Dit artikel wordt beschreven hoe u met de Azure-Snelstartsjablonen gedeeltelijk de implementatie van een Always On configuratie beschikbaarheidsgroep voor SQL Server Virtual Machines in Azure te automatiseren. Er zijn twee Azure Quickstart-sjablonen die worden gebruikt in dit proces. 

   | Template | Description |
   | --- | --- |
   | [101-sql-vm-ag-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-ag-setup) | Het Windows-failovercluster maakt en lid wordt van de SQL Server-VM's toe. |
   | [101-sql-vm-aglistener-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-aglistener-setup) | De beschikbaarheidsgroep-listener maakt en configureert u de interne Load Balancer. Deze sjabloon kan alleen worden gebruikt als het Windows-failovercluster is gemaakt met de **101-sql-vm-ag-setup** sjabloon. |
   | &nbsp; | &nbsp; |

Andere onderdelen van de configuratie van de beschikbaarheidsgroep moeten handmatig worden uitgevoerd, zoals het maken van de beschikbaarheidsgroep en de interne Load Balancer is gemaakt. Dit artikel bevat de volgorde van de automatische en handmatige stappen.
 

## <a name="prerequisites"></a>Vereisten 
Voor het automatiseren van de installatie van een Always On-beschikbaarheidsgroep met behulp van de Quick Start-sjablonen, hebt u al de volgende vereisten: 
- Een [Azure-abonnement](https://azure.microsoft.com/free/).
- Een resourcegroep met een domeincontroller. 
- Een of meer domein [virtuele machines in Azure uitgevoerd SQL Server 2016 (of hoger) Enterprise edition](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) in dezelfde set of beschikbaarheid beschikbaarheidszone die zijn [geregistreerd bij de SQL-VM-resourceprovider](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider).  
- Twee beschikbare (niet gebruikt door een entiteit) IP-adressen, één voor de interne Load Balancer en één voor de beschikbaarheidsgroep-listener binnen hetzelfde subnet als de beschikbaarheidsgroep. Als een bestaande load balancer wordt gebruikt, is alleen een beschikbaar IP-adres nodig.  

## <a name="permissions"></a>Machtigingen
De volgende machtigingen zijn die nodig zijn voor de AlwaysOn-beschikbaarheidsgroep configureren met behulp van Azure-Snelstartsjablonen: 

- Een bestaand domeingebruikersaccount dat gemachtigd om 'Computerobject maken' in het domein is.  Bijvoorbeeld, een domeinaccount voor de admin meestal voldoende machtigingen heeft (ex: account@domain.com). _Dit account moet ook deel uit van de lokale beheerdersgroep op elke virtuele machine om het cluster te maken._
- De domeingebruikersaccount die Hiermee bepaalt u de SQL Server-service. 


## <a name="step-1---create-the-wsfc-and-join-sql-server-vms-to-the-cluster-using-quickstart-template"></a>Stap 1: de WSFC maken en SQL Server-VM's toevoegen aan het cluster met behulp van quickstart-sjabloon 
Zodra uw SQL Server-VM's zijn geregistreerd met de nieuwe resourceprovider voor SQL-VM, kunt u deelnemen aan uw SQL Server-VM's in *SqlVirtualMachineGroups*. Deze bron wordt gedefinieerd voor de metagegevens van het Windows-failovercluster, met inbegrip van de versie, versie, Fully Qualified Domain Name, AD-accounts voor het beheren van het cluster en de SQL-Service en de Storage-Account als de Cloud Witness. Toevoegen van SQL Server-VM's naar de *SqlVirtualMachineGroups* resourcegroep bootstrapt van de Windows Failover Cluster-Service voor het maken van het cluster en voegt deze SQL Server-VM's naar dat cluster. Deze stap is geautomatiseerd met de **101-sql-vm-ag-setup** quickstart-sjabloon en kunnen worden geïmplementeerd met de volgende stappen uit:

1. Navigeer naar de [ **101-sql-vm-ag-setup** ](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-ag-setup) quickstart-sjabloon en selecteer **implementeren in Azure** om te starten van de quickstart-sjabloon in Azure portal.
1. Vul de vereiste velden in de metagegevens van de Windows-failovercluster configureren. De optionele velden kunnen leeg worden gelaten.

    De volgende tabel ziet u de waarden die nodig zijn voor de sjabloon: 

   | **Veld** | Value |
   | --- | --- |
   | **Abonnement** |  Het abonnement waarin uw SQL Server-VM's bestaan. |
   |**Resourcegroep** | De resourcegroep waar uw SQL Server-VM's zich bevinden. | 
   |**De naam van Failover-Cluster** | De gewenste naam voor uw nieuwe Windows-failovercluster. |
   | **Bestaande Vm-lijst** | De SQL Server-VM's die u wilt deelnemen aan in de beschikbaarheidsgroep, en als zodanig deel uitmaken van dit nieuwe cluster. Deze waarden scheiden met een komma en een spatie (ex: SQLVM1, SQLVM2). |
   | **SQL Server-versie** | Selecteer de SQL Server-versie van uw SQL Server-VM's in de vervolgkeuzelijst. Momenteel wordt alleen SQL 2016 en 2017 van de SQL-installatiekopieën worden ondersteund. |
   | **Bestaande Fully Qualified Domain Name** | De bestaande FQDN-naam voor het domein waarin uw SQL Server-VM's zich bevinden. |
   | **Bestaande domeinaccount** | Een bestaand domein-gebruikersaccount dat gemachtigd om 'Computerobject maken' in het domein als is de [CNO](/windows-server/failover-clustering/prestage-cluster-adds) tijdens de sjabloonimplementatie van de wordt gemaakt. Bijvoorbeeld, een domeinaccount voor de admin meestal voldoende machtigingen heeft (ex: account@domain.com). *Dit account moet ook deel uit van de lokale beheerdersgroep op elke virtuele machine om het cluster te maken.*| 
   | **Wachtwoord voor een domeinaccount** | Het wachtwoord voor de eerder genoemde domeingebruikersaccount. | 
   | **Bestaande Sql-serviceaccount** | De domeingebruikersaccount die bepaalt de [SQL Server-service](/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions) tijdens de implementatie van de groep beschikbaarheid (ex: account@domain.com). |
   | **Het wachtwoord van SQL-Service** | Het wachtwoord dat wordt gebruikt door de domeingebruikersaccount die Hiermee bepaalt u de SQL Server-service. |
   | **De naam van de cloud-Witness** | Dit is een nieuwe Azure storage-account dat wordt gemaakt en gebruikt voor de cloud-witness. Deze naam kan worden gewijzigd. |
   | **\_artefacten locatie** | Dit veld is standaard ingesteld en mogen niet worden gewijzigd. |
   | **\_artefacten locatie Sas-Token** | Dit veld is leeg opzettelijk. |
   | &nbsp; | &nbsp; |

1. Als u akkoord met de voorwaarden en bepalingen gaat, selecteert u het selectievakje in naast **ik ga akkoord met de voorwaarden en bepalingen bovenstaande** en selecteer **aankoop** voor het voltooien van de Quick Start-sjabloonimplementatie. 
1. Voor het controleren van uw implementatie, selecteert u de implementatie van de **meldingen** belpictogram in de banner van de bovenste navigatiebalk of Ga naar uw **resourcegroep** in Azure portal, selecteert u  **Implementaties** in de **instellingen** veld en kiest u de implementatie 'Microsoft.Template'. 

  >[!NOTE]
  > Referenties die zijn opgegeven tijdens de sjabloonimplementatie van de worden alleen opgeslagen voor de lengte van de implementatie. Nadat de implementatie is voltooid, deze wachtwoorden worden verwijderd en u wordt gevraagd om dit te regelen opnieuw moet u meer SQL Server-VM's aan het cluster toevoegen. 


## <a name="step-2---manually-create-the-availability-group"></a>Stap 2: de beschikbaarheidsgroep handmatig maken 
De beschikbaarheidsgroep handmatig maken zoals u gewend bent, met behulp van [SQL Server Management Studio](/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio), [PowerShell](/sql/database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell), of [Transact-SQL](/sql/database-engine/availability-groups/windows/create-an-availability-group-transact-sql). 

  >[!IMPORTANT]
  > Voer **niet** een listener op dit moment niet maken omdat dit wordt automatisch door de **101-sql-vm-aglistener-setup** quickstart-sjabloon in stap 4. 

## <a name="step-3---manually-create-the-internal-load-balancer-ilb"></a>Stap 3: de interne Load Balancer (ILB) handmatig maken
De Always On (AG) beschikbaarheidsgroeplistener is een interne Azure Load Balancer (ILB) vereist. De ILB biedt een 'zwevende' IP-adres voor de AG-listener die voor een snellere failover en opnieuw verbinden zorgt. Als de SQL Server-VM's in een beschikbaarheidsgroep deel van dezelfde beschikbaarheidsset bevinden, uitmaken kunt u een Basic Load Balancer. anders moet u een Standard Load Balancer gebruiken.  **De ILB moet zich in hetzelfde vNet als de SQL Server-VM-exemplaren.** De ILB moet alleen worden gemaakt, de rest van de configuratie (zoals de back-endpool, statustest en load balancing regels) wordt verwerkt door de **101-sql-vm-aglistener-setup** quickstart-sjabloon in stap 4. 

1. Open de resourcegroep met de SQL Server-machines in Azure portal. 
2. Klik in de resourcegroep op **toevoegen**.
3. Zoeken naar **netwerktaakverdeler** en selecteer vervolgens in de lijst met zoekresultaten **Load Balancer**, die is gepubliceerd via **Microsoft**.
4. Op de **Load Balancer** blade, klikt u op **maken**.
5. In de **load balancer maken** dialoogvenster vak, configureer de load balancer als volgt:

   | Instelling | Value |
   | --- | --- |
   | **Naam** |Een naam voor de load balancer. Bijvoorbeeld, **sqlLB**. |
   | **Type** |**Interne**: De meeste implementaties gebruikt een interne load balancer, waardoor toepassingen binnen hetzelfde virtuele netwerk verbinding maken met de beschikbaarheidsgroep.  </br> **Externe**: Zorgt ervoor dat toepassingen verbinding maken met de beschikbaarheidsgroep via een openbare internetverbinding. |
   | **Virtueel netwerk** | Selecteer het virtuele netwerk die de SQL Server-exemplaren in. |
   | **Subnet** | Selecteer het subnet die de SQL Server-exemplaren in. |
   | **IP-adrestoewijzing** |**Statische** |
   | **Privé IP-adres** | Geef een beschikbaar IP-adres van het subnet. |
   | **Abonnement** |Als u meerdere abonnementen hebt, kan dit veld weergegeven. Selecteer het abonnement dat u wilt koppelen aan deze resource. Het is normaal hetzelfde abonnement als alle resources voor de beschikbaarheidsgroep. |
   | **Resourcegroep** |Selecteer de resourcegroep die de SQL Server-exemplaren in. |
   | **Locatie** |Selecteer de Azure-locatie hebben als de SQL Server-exemplaren. |
   | &nbsp; | &nbsp; |

6. Selecteer **Maken**. 


  >[!IMPORTANT]
  > De openbare IP-adresresource voor elke virtuele machine van SQL Server moet een standaard-SKU voor compatibiliteit met de Standard Load Balancer hebben. Om te bepalen van de SKU van de openbare IP-resource van de virtuele machine, gaat u naar uw **resourcegroep**, selecteer uw **openbaar IP-adres** resource voor de gewenste SQL Server-VM, en zoek de waarde onder **SKU**  van de **overzicht** deelvenster. 

## <a name="step-4---create-the-ag-listener-and-configure-the-ilb-with-the-quickstart-template"></a>Stap 4: de AG-listener maken en configureren van de ILB met de quickstart-sjabloon

De beschikbaarheidsgroep-listener maken en configureren van de interne Load Balancer (ILB) automatisch met de **101-sql-vm-aglistener-setup** quickstart-sjabloon zoals deze bepalingen de Microsoft.SqlVirtualMachine/ SqlVirtualMachineGroups/AvailabilityGroupListener resource. De **101-sql-vm-aglistener-setup** quickstart-sjabloon, via de SQL-VM-resourceprovider biedt de volgende acties:

 - Hiermee maakt u een nieuwe frontend-IP-resource (op basis van de IP-adreswaarde die is opgegeven tijdens de implementatie) voor de listener. 
 - Hiermee configureert u de instellingen voor het cluster en de ILB. 
 - Hiermee configureert u de ILB-back-endpool, statustest en load balancing regels.
 - De AG-listener maakt met het opgegeven IP-adres en de naam.
 
   >[!NOTE]
   > De **101-sql-vm-aglistener-setup** kan alleen worden gebruikt als het Windows-failovercluster is gemaakt met de **101-sql-vm-ag-setup** sjabloon.
   
   
De ILB configureren en de AG-listener maakt, doet u het volgende:
1. Navigeer naar de [ **101-sql-vm-aglistener-setup** ](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-aglistener-setup) quickstart-sjabloon en selecteer **implementeren in Azure** om te starten van de quickstart-sjabloon in Azure portal.
1. Vul de vereiste velden voor het configureren van de ILB en de beschikbaarheidsgroep-listener maken. De optionele velden kunnen leeg worden gelaten. 

    De volgende tabel ziet u de waarden die nodig zijn voor de sjabloon: 

   | **Veld** | Value |
   | --- | --- |
   |**Resourcegroep** | De resourcegroep waar uw SQL Server-VM's en de beschikbaarheidsgroep bestaat. | 
   |**De naam van een bestaande failovercluster** | De naam van het cluster dat u uw SQL Server-VM's zijn gekoppeld aan. |
   | **Bestaande Sql-beschikbaarheidsgroep**| De naam van de beschikbaarheidsgroep die uw SQL Server-VM's deel uit van maken. |
   | **Bestaande Vm-lijst** | De namen van de SQL Server-VM's die deel van de eerder genoemde beschikbaarheidsgroep uitmaken. De namen moeten worden gescheiden door een komma en een spatie (ex: SQLVM1, SQLVM2). |
   | **Listener** | De DNS-naam die u wilt toewijzen aan de listener. Deze sjabloon bevat de naam aglistener standaard, maar kan worden gewijzigd. De naam mag niet groter zijn dan 15 tekens. |
   | **-Listener-poort** | De poort die u wilt dat de listener te gebruiken. Normaal gesproken deze poort moet de standaardpoort 1433 en als zodanig, dit is het poortnummer dat is opgegeven door deze sjabloon. Echter moet, als de standaardpoort is gewijzigd, klikt u vervolgens de listener-poort gebruiken die waarde in plaats daarvan. | 
   | **Listener IP** | Het IP-adres wilt u de listener te gebruiken.  Dit IP-adres tijdens de sjabloonimplementatie van de wordt gemaakt, dus bieden een IP-adres dat is niet al in gebruik.  |
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
# example: Remove-AzResource -ResourceId '/subscriptions/a1a11a11-1a1a-aa11-aa11-1aa1a11aa11a/resourceGroups/SQLAG-RG/providers/Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/Cluster/availabilitygrouplisteners/aglistener' -Force
Remove-AzResource -ResourceId '/subscriptions/<SubscriptionID>/resourceGroups/<resource-group-name>/providers/Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/<cluster-name>/availabilitygrouplisteners/<listener-name>' -Force
```
 
## <a name="common-errors"></a>Algemene fouten
In deze sectie wordt beschreven enkele bekende problemen en hun mogelijk worden opgelost. 

### <a name="availability-group-listener-for-availability-group-ag-name-already-exists"></a>Beschikbaarheidsgroep-listener voor de beschikbaarheidsgroep '\<AG-naam >' bestaat al
De geselecteerde beschikbaarheidsgroep die al wordt gebruikt in de AG-listener Azure Quickstart-sjabloon bevat een listener, fysiek in de beschikbaarheidsgroep, of als metagegevens in de SQL-VM-resourceprovider.  Verwijderen van de listener met behulp van [PowerShell](#remove-availability-group-listener) voordat u opnieuw wilt implementeren de **101-sql-vm-aglistener-setup** quickstart-sjabloon. 

### <a name="connection-only-works-from-primary-replica"></a>Verbinding werkt alleen met primaire replica
Dit gedrag is het waarschijnlijk in een mislukte **101-sql-vm-aglistener-setup** sjabloonimplementatie verlaten van de ILB-configuratie in een inconsistente status. Controleer of dat de back-endpool wordt de lijst met beschikbaarheid, en dat regels bestaan voor de statustest en voor de taakverdelingsregels. Als er iets ontbreekt, is de configuratie van de ILB een inconsistente toestand terechtkomen. 

U lost dit probleem, verwijdert u de listener met behulp van [PowerShell](#remove-availability-group-listener), de interne Load Balancer via Azure portal verwijderen en opnieuw starten bij stap 3. 

### <a name="badrequest---only-sql-virtual-machine-list-can-be-updated"></a>BadRequest - lijst van de virtuele machine alleen SQL kan worden bijgewerkt
Deze fout kan optreden bij het implementeren van de **101-sql-vm-aglistener-setup** sjabloon als de listener via SQL Server Management Studio (SSMS) is verwijderd, maar is niet verwijderd van de SQL-VM-resourceprovider. Verwijderen van de listener via SSMS verwijdert niet de metagegevens van de listener van de resourceprovider van SQL-VM; de listener moet worden verwijderd uit de resourceprovider met behulp van [PowerShell](#remove-availability-group-listener). 

### <a name="domain-account-does-not-exist"></a>Domeinaccount bestaat niet
Deze fout kan worden veroorzaakt door een van twee redenen. De opgegeven domeinaccount echt bestaat niet of het ontbreekt de [UPN (User Principal Name)](/windows/desktop/ad/naming-properties#userprincipalname) gegevens. De **101-sql-vm-ag-setup** sjabloon wordt verwacht dat een domeinaccount in de vorm van de UPN (dat wil zeggen user@domain.com), maar sommige domeinaccounts mogelijk ontbreekt het. Dit kan doorgaans gebeuren als een lokale gebruiker is gemigreerd naar het eerste domein administrator-account zijn wanneer de server werd gepromoveerd naar een domeincontroller, of wanneer een gebruiker is gemaakt via PowerShell. 

 Controleer of het account bestaat. Als dit het geval is, kunt u worden uitgevoerd in de tweede situatie. U kunt dit oplossen door het volgende te doen:

 1. Open op de domeincontroller, de **Active Directory: gebruikers en Computers** -venster de **extra** optie **Serverbeheer**. 
 2. Navigeer naar de account door te selecteren **gebruikers** in het linkerdeelvenster.
 3. Met de rechtermuisknop op het gewenste account en selecteer **eigenschappen**.
 4. Selecteer de **Account** tabblad en controleer of als de **aanmeldingsnaam van gebruiker** is leeg. Als dit het geval is, is dit de oorzaak van de fout. 

     ![Lege gebruikersaccount wordt aangegeven dat ontbrekende UPN](media/virtual-machines-windows-sql-availability-group-quickstart-template/account-missing-upn.png)

 5. Vul de **aanmeldingsnaam van gebruiker** moet overeenkomen met de naam van de gebruiker en selecteer het juiste domein in de vervolgkeuzelijst omlaag. 
 6. Selecteer **toepassen** Sla uw wijzigingen op en sluit het dialoogvenster selecteert **OK**. 

 Nadat deze wijzigingen zijn aangebracht, probeert u zodra er meer implementeren van de Azure Quickstart-sjabloon. 



## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor meer informatie de volgende artikelen: 

* [Overzicht van SQL Server-machine](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server-VM-Veelgestelde vragen](virtual-machines-windows-sql-server-iaas-faq.md)
* [SQL Server-VM prijsinformatie](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Opmerkingen bij de release van SQL Server-VM](virtual-machines-windows-sql-server-iaas-release-notes.md)
* [Switch licentiemodellen voor een SQL Server-VM](virtual-machines-windows-sql-ahb.md)



