---
title: Algemene identiteit voor gegevens wetenschappelijke virtuele Machine - Azure instellen | Microsoft Docs
description: Het instellen van algemene identiteit in enterprise-team DSVM omgevingen.
keywords: deep leren, AI en hulpmiddelen voor wetenschappelijke gegevens, gegevens wetenschappelijke virtuele machine, georuimtelijke analytics, team gegevens wetenschappelijke processen
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: 431d3079c0d942c15ccbc8352261ccfe1f5f6e47
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="setup-common-identity-on-the-data-science-vm"></a>Algemene identiteit van de installatie op de wetenschappelijke gegevens VM

Standaard op Azure VM, met inbegrip van de lokale gebruiker gegevens wetenschappelijke VM (DSVM) accounts worden gemaakt tijdens het inrichten van de virtuele machine en de gebruikers naar de virtuele machine met deze referenties worden geverifieerd. Als u meerdere virtuele machines die u nodig hebt voor toegang tot hebt, kan deze benadering downloaden omslachtige voor het beheren van referenties. Algemene gebruikersaccounts en -beheer met die een op standaarden gebaseerde identiteitsprovider u één set referenties kunt voor toegang tot meerdere resources in Azure, met inbegrip van meerdere DSVMs gebruiken. 

Active Directory (AD) is een populair identiteitsprovider en wordt ondersteund op Azure als een service als On-premises. U kunt gebruikmaken van AD of Azure AD om te verifiëren van gebruikers op een zelfstandige de gegevens wetenschappelijke VM (DSVM) of in een cluster van DSVM op een virtuele machine van Azure-schaalset. Dit wordt gedaan door de exemplaren DSVM toevoegen aan een AD-domein. Als u al een Active Directory om de identiteiten te beheren, kunt u deze als algemene id-provider. Als u een advertentie geen hebt, kunt u een beheerde AD in Azure uitvoeren via een service met de naam [Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/)(Azure AD DS). 

De documentatie voor [Azure Active directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/) biedt gedetailleerde [instructies](https://docs.microsoft.com/azure/active-directory/choose-hybrid-identity-solution#synchronized-identity) naar beheerde active directory, inclusief de Azure AD verbinden met uw On-premises directory als er een. 

De rest van dit artikel beschrijft de stappen voor het instellen van een volledig beheerde service van AD-domein in Azure met Azure AD DS en uw DSVMs toevoegen aan het beheerde AD-domein om gebruikers toegang krijgen tot een groep DSVMs (en andere Azure-resources) met behulp van een algemene gebruikersaccount en referenties. 

##  <a name="set-up-a-fully-managed-active-directory-domain-on-azure"></a>Stel een volledig beheerde Active Directory-domein in Azure

Azure AD DS kunt u eenvoudig uw om identiteiten te beheren door een volledig beheerde service in Azure. Op deze Active directory-domein, worden gebruikers en groepen beheerd.  De stappen voor het instellen van een Azure AD-domein gehost en gebruikersaccounts in uw directory zijn:

1. Gebruiker (s) toevoegen aan Active directory op de portal 

    a. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met een account met globale beheerdersrechten voor de map.
    
    b. Selecteer **Azure Active Directory** en vervolgens **gebruikers en groepen**.
    
    c. Op **gebruikers en groepen**, selecteer **alle gebruikers**, en selecteer vervolgens **nieuwe gebruiker**.
   
   ![De opdracht Add selecteren](./media/add-user.png)
    
    d. Voer details voor de gebruiker, zoals **naam** en **gebruikersnaam**. Het domeingedeelte van de naam van de gebruikersnaam moet worden de oorspronkelijke standaard domain name '[domeinnaam].onmicrosoft.com' of een geverifieerde niet-gefedereerde [aangepaste domeinnaam](../../active-directory/add-custom-domain.md) zoals 'contoso.com'.
    
    e. Kopieer of het wachtwoord van de gebruiker gegenereerde anders opmerking zodat u deze informatie aan de gebruiker verstrekt kunt nadat dit proces voltooid is.
    
    f. U kunt desgewenst openen en vul de informatie in **profiel**, **groepen**, of **functie Directory** voor de gebruiker. 
    
    g. Op **gebruiker**, selecteer **maken**.
    
    h. Het gegenereerde wachtwoord naar de nieuwe gebruiker veilig distribueren zodat de gebruiker zich kan aanmelden.

2.  Azure AD Domain Services maken

    Voor het maken van een Azure wordt toegevoegd, volg de instructies in het artikel '[inschakelen Azure Active Directory Domain Services met Azure portal](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started)' (taak 1 tot taak 5). Het is belangrijk dat de bestaande gebruikerswachtwoorden in Active directory worden bijgewerkt zodat het wachtwoord in Azure AD DS is gesynchroniseerd. Het is ook belangrijk dat u de DNS-server toevoegen aan Azure AD DS, zoals vermeld in de taak #4 van het bovenstaande artikel. 

3.  Maken van een afzonderlijk DSVM Subnet in het virtuele netwerk in taak 2 van de voorgaande stap hebt gemaakt
4.  Een of meer exemplaren van gegevens wetenschappelijke VM in het subnet DSVM maken 
5.  Ga als volgt [instructies](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-join-ubuntu-linux-vm ) DSVM toevoegen aan AD. 
6.  Koppel vervolgens een gedeelde Azure-bestanden voor het hosten van uw directory thuis- of -notebook om in te schakelen voor het koppelen van uw werkruimte op elke computer. (Als u verregaande niveau bestandsmachtigingen nodig moet u een NFS uitgevoerd op een of meer virtuele machines)

    a. [Een Azure-bestandsshare maken](../../storage/files/storage-how-to-create-file-share.md)
    
    b. Koppel deze aan de DSVM Linux. Wanneer u op de knop 'Verbinden' voor de Azure-bestanden in uw opslagaccount in de Azure portal klikt, kunt u de opdracht uit te voeren in bash-shell op de Linux DSVM wordt weergegeven. De opdracht ziet er als volgt:
```
sudo mount -t cifs //[STORAGEACCT].file.core.windows.net/workspace [Your mount point] -o vers=3.0,username=[STORAGEACCT],password=[Access Key or SAS],dir_mode=0777,file_mode=0777,sec=ntlmssp
```
7.  Stel, u uw Azure-bestanden in /data/workspace gekoppeld. Nu u mappen maken voor elk van uw gebruikers in de share. /Data/Workspace/User1, /data/workspace/user2 enzovoort. Maak een ```notebooks``` map in de werkruimte van de gebruiker. 
8. Symbolische koppelingen maken voor de ```notebooks``` in ```$HOME/userx/notebooks/remote```.   

U hebt nu de gebruikers in uw active directory die worden gehost in Azure en kunnen zich aanmelden bij een DSVM (SSH, Jupyterhub) die is gekoppeld aan de Azure AD DS met behulp van de AD-referenties. Omdat de gebruiker werkruimte op gedeelde bestanden met Azure, wordt de gebruiker toegang tot hun laptops en andere taken van eventuele DSVM hebben wanneer u Jupyterhub. 

Voor automatisch schalen, kunt u de virtuele-machineschaalset ingesteld voor het maken van een pool van virtuele machines die zijn gekoppeld aan het domein op deze manier en met de gedeelde schijf die is gekoppeld. Gebruikers kunnen zich aanmelden bij alle beschikbare machine in de virtuele-machineschaalset en toegang hebben tot de gedeelde schijf waar hun laptops worden opgeslagen. 

## <a name="next-steps"></a>Volgende stappen

* [Veilig opslaan van referenties voor toegang tot cloud-bronnen](dsvm-secure-access-keys.md)



