---
title: Instellen van een algemene identiteit voor de gegevens wetenschappelijke virtuele Machine, Azure | Microsoft Docs
description: Een algemene identiteit in omgevingen met een enterprise-team DSVM instellen.
keywords: deep leren, AI en hulpmiddelen voor wetenschappelijke gegevens, gegevens wetenschappelijke virtuele machine, georuimtelijke analytics, team gegevens wetenschappelijke processen
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: d6235f3a425481a13e627d683bb4c3943b473b40
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309825"
---
# <a name="set-up-a-common-identity-on-the-data-science-virtual-machine"></a>Een algemene identiteit op de gegevens wetenschappelijke virtuele Machine instellen

In Azure een virtuele machine (VM), met inbegrip van gegevens wetenschappelijke virtuele Machine (DSVM) kunt u lokale gebruikersaccounts maken tijdens het inrichten van de virtuele machine. Gebruikers verifiëren vervolgens naar de virtuele machine met behulp van deze referenties. Als u meerdere virtuele machines die u nodig hebt voor toegang tot hebt, kunt deze benadering snel opvragen omslachtige zoals het beheren van referenties. Algemene gebruikersaccounts en -beheer via een op standaarden gebaseerde identiteitsprovider kunt u op een enkele set referenties voor toegang tot meerdere resources in Azure, met inbegrip van meerdere DSVMs. 

Active Directory is een populair identiteitsprovider en wordt ondersteund op Azure als een service en on-premises. U kunt gebruikmaken van Azure Active Directory (Azure AD) of het on-premises Active Directory om gebruikers in een zelfstandige DSVM of een cluster van DSVMs in de schaalset van een virtuele machine van Azure te verifiëren. U doen dit door de exemplaren DSVM toevoegen aan een Active Directory-domein. 

Als u al Active Directory om de identiteiten te beheren, kunt u deze als algemene id-provider. Als u geen Active Directory hebt, kunt u een beheerde Active Directory-exemplaar op Azure uitvoeren via een service met de naam [Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/) (Azure AD DS). 

De documentatie voor [Azure AD](https://docs.microsoft.com/azure/active-directory/) biedt gedetailleerde [management instructies](https://docs.microsoft.com/azure/active-directory/choose-hybrid-identity-solution#synchronized-identity), met inbegrip van Azure AD verbinden met uw on-premises directory als er een. 

In dit artikel beschrijft de stappen voor het instellen van een volledig beheerde Active Directory domain Services in Azure met Azure AD DS. U kunt uw DSVMs vervolgens toevoegen aan het beheerde Active Directory-domein zodat gebruikers toegang tot een groep DSVMs (en andere Azure-resources) met een gemeenschappelijk gebruikersaccount en referenties. 

## <a name="set-up-a-fully-managed-active-directory-domain-on-azure"></a>Stel een volledig beheerde Active Directory-domein in Azure

Azure AD DS kunt u eenvoudig uw om identiteiten te beheren door een volledig beheerde service in Azure. Op deze Active Directory-domein u gebruikers en groepen beheren. De stappen voor het instellen van een Azure gehoste Active Directory-domein- en gebruikersaccounts in uw directory zijn:

1. In de Azure portal, moet u de gebruiker toevoegen aan Active Directory: 

   a. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met een account met globale beheerdersrechten voor de map.
    
   b. Selecteer **Azure Active Directory** en vervolgens **gebruikers en groepen**.
    
   c. Op **gebruikers en groepen**, selecteer **alle gebruikers**, en selecteer vervolgens **nieuwe gebruiker**.
   
      De **gebruiker** deelvenster wordt geopend.
      
      ![Het deelvenster 'Gebruiker'](./media/add-user.png)
    
   d. Voer details voor de gebruiker, zoals **naam** en **gebruikersnaam**. Het domeingedeelte van de naam van de gebruikersnaam moet de eerste standaard domain name '[domeinnaam].onmicrosoft.com' of een geverifieerde niet-gefedereerde [aangepaste domeinnaam](../../active-directory/add-custom-domain.md) zoals 'contoso.com'.
    
   e. Kopieer of het wachtwoord van de gebruiker gegenereerde anders opmerking zodat u deze informatie aan de gebruiker verstrekt kunt nadat dit proces voltooid is.
    
   f. U kunt desgewenst openen en vul de informatie in **profiel**, **groepen**, of **functie Directory** voor de gebruiker. 
    
   g. Op **gebruiker**, selecteer **maken**.
    
   h. Het gegenereerde wachtwoord naar de nieuwe gebruiker veilig distribueren zodat de gebruiker zich kan aanmelden.

2. Maak een Azure AD DS-exemplaar. Volg de instructies in het artikel [inschakelen Azure Active Directory Domain Services met Azure portal](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started) (taken 1 tot 5). Het is belangrijk om bij te werken van de bestaande gebruikerswachtwoorden in Active Directory zodat het wachtwoord in Azure AD DS is gesynchroniseerd. Het is ook belangrijk om toe te voegen DNS naar Azure AD DS, zoals beschreven in de taak 4 van het artikel. 

3. Maak een apart DSVM subnet in het virtuele netwerk in taak 2 van de vorige stap hebt gemaakt.
4. Een of meer exemplaren van gegevens wetenschappelijke VM in het subnet DSVM maken. 
5. Ga als volgt de [instructies](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-join-ubuntu-linux-vm ) DSVM toevoegen aan Active Directory. 
6. Koppel een share Azure-bestanden voor het hosten van uw directory thuis- of -notebook om in te schakelen voor het koppelen van uw werkruimte op elke computer. (Als u nauwe bestandsniveau machtigingen nodig hebt, moet u NFS uitgevoerd op een of meer virtuele machines.)

   a. [Maak een share Azure Files](../../storage/files/storage-how-to-create-file-share.md).
    
   b. Koppel deze aan de DSVM Linux. Wanneer u selecteert de **Connect** knop voor de Azure-bestanden delen in uw opslagaccount in de Azure portal, de opdracht om uit te voeren in de Bash shell op de Linux DSVM wordt weergegeven. De opdracht ziet er als volgt:
   
   ```
   sudo mount -t cifs //[STORAGEACCT].file.core.windows.net/workspace [Your mount point] -o vers=3.0,username=[STORAGEACCT],password=[Access Key or SAS],dir_mode=0777,file_mode=0777,sec=ntlmssp
   ```
7. Wordt ervan uitgegaan dat uw Azure-bestanden delen in /data/workspace, bijvoorbeeld is gekoppeld. Nu mappen maken voor elk van uw gebruikers in de share: /data/workspace/user1, /data/workspace/user2, enzovoort. Maak een `notebooks` map in de werkruimte van de gebruiker. 
8. Symbolische koppelingen maken voor `notebooks` in `$HOME/userx/notebooks/remote`.   

U hebt nu de gebruikers in uw gehost in Azure Active Directory-exemplaar. Gebruikers kunnen met behulp van de Active Directory-referenties aanmelden bij een DSVM (SSH of JupyterHub) die is gekoppeld aan Azure AD DS. Omdat de gebruiker werkruimte op een share Azure-bestanden, hebben gebruikers toegang tot hun laptops en andere taken van een DSVM wanneer ze JupyterHub gebruiken. 

Voor automatisch schalen, kunt u een virtuele-machineschaalset ingesteld voor het maken van een pool van virtuele machines die zijn gekoppeld aan het domein op deze manier en met de gedeelde schijf die is gekoppeld. Gebruikers kunnen zich aanmelden bij alle beschikbare machine in de virtuele-machineschaalset en toegang hebben tot de gedeelde schijf waar hun laptops worden opgeslagen. 

## <a name="next-steps"></a>Volgende stappen

* [Veilig opslaan van referenties voor toegang tot cloud-bronnen](dsvm-secure-access-keys.md)



