---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 10/23/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: ee5faedd4f59aa791424a1f178f0462922f21d28
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58094688"
---
Elk eindpunt heeft een *openbare poort* en een *particuliere poort*:

* De openbare poort wordt gebruikt door de Azure load balancer om te luisteren naar binnenkomend verkeer op de virtuele machine via internet.
* De particuliere poort wordt gebruikt door de virtuele machine om te luisteren naar binnenkomend verkeer, meestal dat is bestemd voor een toepassing of service die wordt uitgevoerd op de virtuele machine.

Standaardwaarden voor de IP-protocol en de TCP of UDP-poorten voor bekende protocollen worden geleverd wanneer u eindpunten met de Azure portal maken-netwerk. Geef het juiste IP-protocol (TCP of UDP) en de openbare en particuliere poorten voor aangepaste eindpunten. Als u wilt willekeurig inkomend verkeer verdelen over meerdere virtuele machines, een load balancer-set die bestaan uit meerdere eindpunten te maken.

Nadat u een eindpunt hebt gemaakt, kunt u een toegangsbeheerlijst (ACL) kunt gebruiken voor het definiëren van regels voor toestaan of weigeren van het binnenkomende verkeer naar de openbare poort van het eindpunt op basis van de bron-IP-adres. Echter, als de virtuele machine zich in een Azure-netwerk, gebruik netwerkbeveiligingsgroepen in plaats daarvan. Zie voor meer informatie, [over netwerkbeveiligingsgroepen](../articles/virtual-network/security-overview.md).

> [!NOTE]
> Firewall-configuratie voor virtuele machines van Azure wordt automatisch gedaan voor poorten die zijn gekoppeld aan externe connectiviteit-eindpunten die Azure automatisch ingesteld. Er is geen configuratie wordt voor poorten die zijn opgegeven voor alle andere eindpunten, automatisch uitgevoerd aan de firewall van de virtuele machine. Wanneer u een eindpunt voor de virtuele machine maakt, zorg ervoor dat de firewall van de virtuele machine kunt ook het verkeer voor het protocol en de particuliere poort die overeenkomt met de configuratie van eindpunten. Als u wilt de firewall configureren, Zie de documentatie of de online help voor het besturingssysteem wordt uitgevoerd op de virtuele machine.
>
>

## <a name="create-an-endpoint"></a>Een eindpunt maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer **virtuele machines**, en selecteer vervolgens de virtuele machine die u wilt configureren.

3. Selecteer **eindpunten** in de **instellingen** groep. De **eindpunten** pagina wordt weergegeven, waarin de huidige eindpunten voor de virtuele machine. (In dit voorbeeld is voor een Windows-VM. Een Linux VM ziet standaard u een eindpunt voor SSH.)

   <!-- ![Endpoints](./media/virtual-machines-common-classic-setup-endpoints/endpointswindows.png) -->
   ![Eindpunten](./media/virtual-machines-common-classic-setup-endpoints/endpointsblade.png)


4. Selecteer in de opdrachtbalk boven de eindpunt-vermeldingen **toevoegen**. De **eindpunt toevoegen** pagina wordt weergegeven.

5. Voor **naam**, voer een naam voor het eindpunt.

6. Voor **Protocol**, kiest u **TCP** of **UDP**.

7. Voor **openbare poort**, voer het poortnummer voor het binnenkomende verkeer vanaf internet. 

8. Voor **particuliere poort**, voer het poortnummer waar de virtuele machine luistert. De openbare en persoonlijke poortnummers kan afwijken. Zorg ervoor dat de firewall op de virtuele machine is geconfigureerd voor het verkeer overeenkomt met het protocol en de particuliere poort.

9. Selecteer **OK**.

Het nieuwe eindpunt wordt weergegeven op de **eindpunten** pagina.

![Eindpunt maken voltooid](./media/virtual-machines-common-classic-setup-endpoints/endpointcreated.png)

## <a name="manage-the-acl-on-an-endpoint"></a>De ACL voor een eindpunt beheren
Voor het definiëren van de set computers die verkeer kunnen verzenden, kan de ACL voor een eindpunt beperken het verkeer op basis van de bron-IP-adres. Volg deze stappen als u wilt toevoegen, wijzigen of verwijderen van een ACL op een eindpunt.

> [!NOTE]
> Als het eindpunt deel uit van een load balancer-set maakt, worden eventuele wijzigingen die u in de ACL voor een eindpunt aanbrengt worden toegepast op alle eindpunten in de set.
>
>

Als de virtuele machine in een Azure-netwerk, gebruikt u netwerkbeveiligingsgroepen in plaats van ACL's. Zie voor meer informatie, [over netwerkbeveiligingsgroepen](../articles/virtual-network/security-overview.md).

1. Meld u aan bij Azure Portal.

2. Selecteer **virtuele machines**, en selecteer vervolgens de naam van de virtuele machine die u wilt configureren.

3. Selecteer **eindpunten**. Selecteer het juiste eindpunt in de lijst met eindpunten. De lijst met ACL is aan de onderkant van de pagina.

   ![Geef details op ACL](./media/virtual-machines-common-classic-setup-endpoints/aclpreentry.png)

4. Gebruik de rijen in de lijst wilt toevoegen, verwijderen, of de regels voor een ACL bewerken en de volgorde wijzigen. De **extern SUBNET** waarde is een IP-adresbereik voor het binnenkomende verkeer vanaf internet die gebruikmaakt van de Azure load balancer toestaan of weigeren van het verkeer op basis van de bron-IP-adres. Zorg ervoor dat het IP-adresbereik in (classless Inter-Domain routing CIDR)-indeling, ook wel bekend als adres-voorvoegselindeling opgeven. Bijvoorbeeld `10.1.0.0/8`.

   ![Nieuwe ACL-vermelding](./media/virtual-machines-common-classic-setup-endpoints/newaclentry.png)


U kunt regels gebruiken om toe te staan alleen verkeer van specifieke computers die overeenkomt met uw computers op het internet of om verkeer te weigeren van specifieke, bekende-adresbereiken.

De regels worden geëvalueerd in volgorde beginnen met de eerste regel en eindigend met de laatste regel. Regels moeten daarom worden gerangschikt van minst beperkende naar meest beperkende. Zie voor meer informatie, [wat is er een Network Access Control List](../articles/virtual-network/virtual-networks-acl.md).
