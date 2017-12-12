---
title: Een WindowsServer-machine toevoegen aan Azure Active Directory Domain Services | Microsoft Docs
description: Windows Server een virtuele machine toevoegen aan een beheerd domein met behulp van Azure Resource Manager-sjablonen.
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 4eabfd8e-5509-4acd-86b5-1318147fddb5
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: maheshu
ms.openlocfilehash: b90198696b7bdb9277fd2f2b8e8e727af42c5cfa
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-a-resource-manager-template"></a>Windows Server een virtuele machine toevoegen aan een beheerd domein met een Resource Manager-sjabloon
In dit artikel laat zien hoe een virtuele machine met Windows Server toevoegen aan een beheerd domein van Azure AD Domain Services met behulp van Resource Manager-sjablonen.

## <a name="before-you-begin"></a>Voordat u begint
Als u wilt uitvoeren van de taken worden in dit artikel worden vermeld, hebt u het volgende nodig:
1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -ofwel gesynchroniseerd met een on-premises adreslijst of een map alleen in de cloud.
3. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [handleiding](active-directory-ds-getting-started.md).
4. Zorg ervoor dat u de IP-adressen van het beheerde domein als de DNS-servers voor het virtuele netwerk hebt geconfigureerd. Zie voor meer informatie [het bijwerken van DNS-instellingen voor de virtuele Azure-netwerk](active-directory-ds-getting-started-dns.md)
5. Voltooi de stappen die zijn vereist voor het [wachtwoorden aan uw Azure AD Domain Services beheerd domein synchroniseren](active-directory-ds-getting-started-password-sync.md).


## <a name="install-and-configure-required-tools"></a>Installeren en configureren van vereiste hulpprogramma 's
U kunt een van de volgende opties gebruiken om uit te voeren van de stappen in dit document:
* **Azure PowerShell**: [installeren en configureren](https://azure.microsoft.com/documentation/articles/powershell-install-configure/)
* **Azure platformoverschrijdende opdrachtregelinterface**: [installeren en configureren](https://azure.microsoft.com/documentation/articles/xplat-cli-install/)


## <a name="option-1-provision-a-new-windows-server-vm-and-join-it-to-a-managed-domain"></a>Optie 1: Een nieuwe virtuele machine voor Windows-Server in te richten en deze toevoegen aan een beheerd domein
**Snel starten-sjabloonnaam**: [201-vm--domein](https://azure.microsoft.com/resources/templates/201-vm-domain-join/)

Implementeren van een virtuele machine van Windows Server en deze toevoegen aan een beheerd domein, voert u de volgende stappen uit:
1. Navigeer naar de [snel starten-sjabloon](https://azure.microsoft.com/resources/templates/201-vm-domain-join/).
2. Klik op **Implementeren in Azure**.
3. In de **aangepaste implementatie** pagina, typt u de vereiste informatie voor het inrichten van de virtuele machine.
4. Selecteer de **Azure-abonnement** waarin de virtuele machine inrichten. Kies de dezelfde Azure-abonnement waarin u Azure AD Domain Services hebt ingeschakeld.
5. Kies een bestaande **resourcegroep** of maak een nieuwe.
6. Kies een **locatie** waarin u de nieuwe virtuele machine te implementeren.
7. In **bestaande VNET-naam**, geef het virtuele netwerk waarin u uw Azure AD Domain Services beheerd domein hebt geïmplementeerd.
8. In **bestaande subnetnaam**, geef het subnet in het virtuele netwerk waarin u wilt implementeren van deze virtuele machine. Selecteer het gatewaysubnet niet in het virtuele netwerk. Selecteer ook niet het toegewezen subnet waarin uw beheerde domein wordt geïmplementeerd.
9. In **DNS-Label-voorvoegsel**, geef de hostnaam voor de virtuele machine wordt ingericht. Bijvoorbeeld 'contoso-win'.
10. Selecteer de relevante **VM-grootte** voor de virtuele machine.
11. In **voor lid worden van domein**, geef de DNS-domeinnaam van uw beheerde domein.
12. In **domeingebruikersnaam**, geef de naam van het gebruikersaccount op uw beheerde domein dat moet worden gebruikt voor de virtuele machine toevoegen aan het beheerde domein.
13. In **domeinwachtwoord**, het wachtwoord van de domeingebruikersaccount waarnaar wordt verwezen door de parameter 'domainUsername' opgeven.
14. Optioneel: Kunt u een **pad van de organisatie-eenheid** naar een aangepaste organisatie-eenheid, waarin u de virtuele machine toe te voegen. Als u een waarde voor deze parameter niet opgeeft, wordt de virtuele machine toegevoegd aan de standaardgroep **AAD DC Computers** organisatie-eenheid op het beheerde domein.
15. In de **VM-Beheerdersgebruikersnaam** veld, geeft u de naam van een lokale administrator-account voor de virtuele machine.
16. In de **VM beheerderswachtwoord** veld, geeft u een lokaal beheerderswachtwoord voor de virtuele machine. Een sterk lokale administrator-wachtwoord voor de virtuele machine als bescherming tegen beveiligingsaanvallen wachtwoord opgeven.
17. Klik op **ik ga akkoord met de voorwaarden en bepalingen bovengenoemde**.
18. Klik op **aankoop** voor het inrichten van de virtuele machine.

> [!WARNING]
> **Wachtwoorden behoedzaam worden verwerkt.**
> Het sjabloonbestand parameter bevat wachtwoorden voor domeinaccounts evenals lokale administrator-wachtwoord voor de virtuele machine. Zorg ervoor dat u niet laat dit bestand zich rond op bestandsshares of andere gedeelde locaties. Het is raadzaam de buitengebruikstelling van dit bestand als u klaar bent voor het implementeren van uw virtuele machines.
>

Nadat de implementatie voltooid is, wordt uw nieuw ingerichte virtuele Windows-computer toegevoegd aan het beheerde domein.


## <a name="option-2-join-an-existing-windows-server-vm-to-a-managed-domain"></a>Optie 2: Een bestaande virtuele machine van Windows Server toevoegen aan een beheerd domein
**Snel starten-sjabloon**: [201-vm-domain-join-bestaande](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/)

Als u wilt toevoegen aan een bestaande virtuele machine voor Windows Server een beheerd domein, moet u de volgende stappen uitvoeren:
1. Navigeer naar de [snel starten-sjabloon](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/).
2. Klik op **Implementeren in Azure**.
3. In de **aangepaste implementatie** pagina, typt u de vereiste informatie voor het inrichten van de virtuele machine.
4. Selecteer de **Azure-abonnement** waarin de virtuele machine inrichten. Kies de dezelfde Azure-abonnement waarin u Azure AD Domain Services hebt ingeschakeld.
5. Kies een bestaande **resourcegroep** of maak een nieuwe.
6. Kies een **locatie** waarin u de nieuwe virtuele machine te implementeren.
7. In de **VM lijst** veld, geeft u de namen van de bestaande virtuele machines worden toegevoegd aan het beheerde domein. Gebruik een komma te scheiden van afzonderlijke VM-namen. Bijvoorbeeld: **web van contoso, contoso-api**.
8. In **domeingebruikersnaam Join**, geef de naam van het gebruikersaccount op uw beheerde domein dat moet worden gebruikt voor de virtuele machine toevoegen aan het beheerde domein.
9. In **Domeingebruikerswachtwoord Join**, het wachtwoord van de domeingebruikersaccount waarnaar wordt verwezen door de parameter 'domainUsername' opgeven.
10. In **FQDN van**, geef de DNS-domeinnaam van uw beheerde domein.
11. Optioneel: Kunt u een **pad van de organisatie-eenheid** naar een aangepaste organisatie-eenheid, waarin u de virtuele machine toe te voegen. Als u een waarde voor deze parameter niet opgeeft, wordt de virtuele machine toegevoegd aan de standaardgroep **AAD DC Computers** organisatie-eenheid op het beheerde domein.
12. Klik op **ik ga akkoord met de voorwaarden en bepalingen bovengenoemde**.
13. Klik op **aankoop** voor het inrichten van de virtuele machine.

> [!WARNING]
> **Wachtwoorden behoedzaam worden verwerkt.**
> Het sjabloonbestand parameter bevat wachtwoorden voor domeinaccounts evenals lokale administrator-wachtwoord voor de virtuele machine. Zorg ervoor dat u niet laat dit bestand zich rond op bestandsshares of andere gedeelde locaties. Het is raadzaam de buitengebruikstelling van dit bestand als u klaar bent voor het implementeren van uw virtuele machines.
>

Nadat de implementatie voltooid is, worden de opgegeven Windows virtuele machines toegevoegd aan het beheerde domein.


## <a name="related-content"></a>Gerelateerde inhoud
* [Overzicht van Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.4.0)
* [Azure-sjabloon snel starten - een nieuwe virtuele machine lid worden van domein](https://azure.microsoft.com/resources/templates/201-vm-domain-join/)
* [Azure-sjabloon snel starten - bestaande virtuele machines lid worden van domein](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/)
* [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
