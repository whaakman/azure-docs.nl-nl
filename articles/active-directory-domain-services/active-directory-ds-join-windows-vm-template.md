---
title: Een WindowsServer-VM toevoegen aan Azure Active Directory Domain Services | Microsoft Docs
description: Neem deel aan een Windows Server-machine aan een beheerd domein met behulp van Azure Resource Manager-sjablonen.
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 4eabfd8e-5509-4acd-86b5-1318147fddb5
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: maheshu
ms.openlocfilehash: 2b94496895c0ab06127b6c556b1240e3279e572f
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39504410"
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-a-resource-manager-template"></a>Een Windows Server virtuele machine toevoegen aan een beheerd domein met behulp van Resource Manager-sjabloon
Dit artikel ziet u hoe u een Windows Server-machine koppelen aan een beheerd domein van Azure AD Domain Services met behulp van Resource Manager-sjablonen.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Voordat u begint
Als u de taken die in dit artikel worden vermeld, hebt u het volgende nodig:
1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -een gesynchroniseerd met een on-premises directory of een map alleen in de cloud.
3. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [introductiehandleiding](active-directory-ds-getting-started.md).
4. Zorg ervoor dat u de IP-adressen van het beheerde domein als de DNS-servers voor het virtuele netwerk hebt geconfigureerd. Zie voor meer informatie, [DNS-instellingen voor de Azure-netwerk bijwerken](active-directory-ds-getting-started-dns.md)
5. De stappen die nodig zijn om te voltooien [Synchroniseer de wachtwoorden voor uw Azure AD Domain Services beheerde domein](active-directory-ds-getting-started-password-sync.md).


## <a name="install-and-configure-required-tools"></a>Installeren en configureren van vereiste hulpprogramma 's
U kunt een van de volgende opties gebruiken om uit te voeren van de stappen in dit document:
* **Azure PowerShell**: [installeren en configureren](https://azure.microsoft.com/documentation/articles/powershell-install-configure/)
* **Azure platformoverschrijdende opdrachtregelinterface**: [installeren en configureren](https://azure.microsoft.com/documentation/articles/xplat-cli-install/)


## <a name="option-1-provision-a-new-windows-server-vm-and-join-it-to-a-managed-domain"></a>Optie 1: Inrichten van een nieuwe Windows Server-VM en koppel deze aan een beheerd domein
**Snel starten-sjabloonnaam**: [201-vm-domain-join](https://azure.microsoft.com/resources/templates/201-vm-domain-join/)

Voor het implementeren van een Windows Server-machine en voeg deze toe aan een beheerd domein, moet u de volgende stappen uitvoeren:
1. Navigeer naar de [snel starten-sjabloon](https://azure.microsoft.com/resources/templates/201-vm-domain-join/).
2. Klik op **Implementeren in Azure**.
3. In de **aangepaste implementatie** pagina, geef de vereiste gegevens voor het inrichten van de virtuele machine.
4. Selecteer de **Azure-abonnement** waarin de virtuele machine inrichten. Kies de dezelfde Azure-abonnement waarin u Azure AD Domain Services hebt ingeschakeld.
5. Kies een bestaande **resourcegroep** of maak een nieuwe.
6. Kies een **locatie** waarin u kunt de nieuwe virtuele machine implementeren.
7. In **bestaande VNET-naam**, geeft u het virtuele netwerk waarin u uw Azure AD Domain Services beheerde domein hebt geïmplementeerd.
8. In **bestaande subnetnaam**, geef het subnet binnen het virtuele netwerk waar u wilt implementeren van deze virtuele machine. Selecteer het gatewaysubnet niet in het virtuele netwerk. Schakel ook niet de toegewezen subnet waarin uw beheerde domein is geïmplementeerd.
9. In **DNS-Label-voorvoegsel**, geef de hostnaam voor de virtuele machine wordt ingericht. Bijvoorbeeld 'contoso-win'.
10. Selecteer de juiste **VM-grootte** voor de virtuele machine.
11. In **op lid worden van domein**, de DNS-domeinnaam van uw beheerde domein opgeven.
12. In **domeingebruikersnaam**, naam van het gebruikersaccount op uw beheerde domein dat moet worden gebruikt om de virtuele machine toevoegen aan het beheerde domein opgeven.
13. In **domeinwachtwoord**, geef het wachtwoord van het domeinaccount van de gebruiker waarnaar wordt verwezen door de parameter 'domainUsername'.
14. Optioneel: U kunt opgeven een **pad van de organisatie-eenheid** naar een aangepaste organisatie-eenheid, waarin de virtuele machine toe te voegen. Als u een waarde voor deze parameter niet opgeeft, de virtuele machine is toegevoegd aan de standaard **AAD DC Computers** organisatie-eenheid in het beheerde domein.
15. In de **gebruikersnaam van de VM-beheerder** veld, geeft u de naam van een lokale administrator-account voor de virtuele machine.
16. In de **VM beheerderswachtwoord** veld, geeft u een lokaal beheerderswachtwoord voor de virtuele machine. Een sterke lokale administrator-wachtwoord voor de virtuele machine om te beveiligen tegen beveiligingsaanvallen wachtwoord opgeven.
17. Klik op **ik ga akkoord met de voorwaarden en bepalingen bovenstaande**.
18. Klik op **aankoop** voor het inrichten van de virtuele machine.

> [!WARNING]
> **Wachtwoorden voorzichtig worden verwerkt.**
> Het sjabloonbestand parameter bevat de wachtwoorden voor domeinaccounts en wachtwoorden voor lokale beheerder voor de virtuele machine. Zorg ervoor dat u niet laat dit bestand zich rond op bestandsshares of andere gedeelde locatie. Het is raadzaam de buitengebruikstelling van dit bestand als u klaar bent voor het implementeren van uw virtuele machines.
>

Wanneer de implementatie voltooid is, wordt uw nieuw ingerichte virtuele machine voor Windows toegevoegd aan het beheerde domein.


## <a name="option-2-join-an-existing-windows-server-vm-to-a-managed-domain"></a>Optie 2: Een bestaande Windows Server-VM toevoegen aan een beheerd domein
**Snel starten-sjabloon**: [201-vm-domain-join-bestaande](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/)

Als u wilt deelnemen aan een bestaande Windows Server virtuele machine aan een beheerd domein, moet u de volgende stappen uitvoeren:
1. Navigeer naar de [snel starten-sjabloon](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/).
2. Klik op **Implementeren in Azure**.
3. In de **aangepaste implementatie** pagina, geef de vereiste gegevens voor het inrichten van de virtuele machine.
4. Selecteer de **Azure-abonnement** waarin de virtuele machine inrichten. Kies de dezelfde Azure-abonnement waarin u Azure AD Domain Services hebt ingeschakeld.
5. Kies een bestaande **resourcegroep** of maak een nieuwe.
6. Kies een **locatie** waarin u kunt de nieuwe virtuele machine implementeren.
7. In de **VM List** veld, geeft u de namen van de bestaande virtuele machines worden toegevoegd aan het beheerde domein. Gebruik een komma te scheiden van afzonderlijke VM-namen. Bijvoorbeeld, **web van contoso, contoso-api**.
8. In **domeingebruikersnaam Join**, naam van het gebruikersaccount op uw beheerde domein dat moet worden gebruikt om de virtuele machine toevoegen aan het beheerde domein opgeven.
9. In **Domeingebruikerswachtwoord Join**, geef het wachtwoord van het domeinaccount van de gebruiker waarnaar wordt verwezen door de parameter 'domainUsername'.
10. In **FQDN van**, de DNS-domeinnaam van uw beheerde domein opgeven.
11. Optioneel: U kunt opgeven een **pad van de organisatie-eenheid** naar een aangepaste organisatie-eenheid, waarin de virtuele machine toe te voegen. Als u een waarde voor deze parameter niet opgeeft, de virtuele machine is toegevoegd aan de standaard **AAD DC Computers** organisatie-eenheid in het beheerde domein.
12. Klik op **ik ga akkoord met de voorwaarden en bepalingen bovenstaande**.
13. Klik op **aankoop** voor het inrichten van de virtuele machine.

> [!WARNING]
> **Wachtwoorden voorzichtig worden verwerkt.**
> Het sjabloonbestand parameter bevat de wachtwoorden voor domeinaccounts en wachtwoorden voor lokale beheerder voor de virtuele machine. Zorg ervoor dat u niet laat dit bestand zich rond op bestandsshares of andere gedeelde locatie. Het is raadzaam de buitengebruikstelling van dit bestand als u klaar bent voor het implementeren van uw virtuele machines.
>

Wanneer de implementatie voltooid is, wordt de opgegeven Windows-virtuele machines zijn gekoppeld aan het beheerde domein.


## <a name="related-content"></a>Gerelateerde inhoud
* [Overzicht van Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.4.0)
* [Azure quick start-sjabloon: een nieuwe virtuele machine lid worden van domein](https://azure.microsoft.com/resources/templates/201-vm-domain-join/)
* [Azure quick start-sjabloon: bestaande virtuele machines lid worden van domein](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/)
* [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
