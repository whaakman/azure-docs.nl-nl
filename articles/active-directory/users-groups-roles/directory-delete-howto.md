---
title: Verwijderen van een directory van Azure Active Directory-tenant | Microsoft Docs
description: Wordt uitgelegd hoe u een Azure AD-tenant-directory voor verwijdering
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 06/13/2018
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: 44781c976afa94ebe3c8df8c7fd70c3e16956916
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37871951"
---
# <a name="delete-an-azure-active-directory-tenant"></a>Een Azure Active Directory-tenant niet verwijderen
Wanneer een tenant is verwijderd, worden ook alle resources die zijn opgenomen in de tenant verwijderd. U moet de tenant voorbereiden door het minimaliseren van alle bijbehorende resources, voordat u verwijderen. Alleen een globale beheerder van Azure Active Directory (Azure AD) u een Azure AD-tenant uit de portal verwijderen kunt.

## <a name="prepare-the-tenant-for-deletion"></a>Bereid de tenant voor verwijdering

U kunt een tenant niet verwijderen in Azure AD pas verschillende controles wordt doorgegeven. Deze controles Verminder risico dat het verwijderen van een tenant negatieve gevolgen heeft voor gebruikerstoegang, zoals de mogelijkheid om aan te melden bij Office 365 of toegang tot resources in Azure. Bijvoorbeeld, als de tenant die is gekoppeld aan een abonnement per ongeluk is verwijderd, klikt u vervolgens gebruikers geen toegang tot de Azure-resources voor dat abonnement. Hieronder worden de voorwaarden die worden gecontroleerd:

* Er mag geen gebruikers in de tenant met uitzondering van een hoofdbeheerder die de tenant verwijderen. Andere gebruikers moeten worden verwijderd voordat de tenant kan worden verwijderd. Als gebruikers van on-premises worden gesynchroniseerd, klikt u vervolgens synchronisatie moet worden uitgeschakeld en de gebruikers in de cloudtenant met behulp van de Azure portal of Azure PowerShell-cmdlets moeten worden verwijderd. 
* Er zijn geen toepassingen in de tenant. Alle toepassingen moeten worden verwijderd voordat de tenant kan worden verwijderd.
* Er mag geen multi-factor authenticatieproviders die aan de tenant zijn gekoppeld.
* Kunnen er geen abonnementen voor alle Microsoft Online Services zoals Microsoft Azure, Office 365 of Azure AD Premium is gekoppeld aan de tenant. Bijvoorbeeld, als een standaardtenant in Azure voor u is gemaakt, verwijderen u niet deze tenant als uw Azure-abonnement is nog steeds afhankelijk van deze tenant voor verificatie is. U kunt op dezelfde manier een tenant niet verwijderen als een andere gebruiker heeft een abonnement gekoppeld. 

## <a name="delete-an-azure-ad-tenant"></a>Een Azure AD-tenant niet verwijderen

1. Aanmelden bij de [Azure AD-beheercentrum](https://aad.portal.azure.com) met een account dat de globale beheerder voor de tenant.

2. Selecteer **Azure Active Directory**.

3. Switch voor de tenant die u wilt verwijderen.
  
  ![knop directory verwijderen](./media/directory-delete-howto/delete-directory-command.png)

4. Selecteer **verwijderen directory**.
  
  ![knop directory verwijderen](./media/directory-delete-howto/delete-directory-list.png)

5. Als uw tenant voldoet niet aan een of meer controles, bent u hebt opgegeven met een koppeling naar meer informatie over hoe u om door te geven. Nadat u alle controles kunt toevoegen, selecteert u **verwijderen** om het proces te voltooien.

## <a name="i-have-an-expired-subscription-but-i-cant-delete-the-tenant"></a>Ik heb een verlopen abonnement, maar ik kan de tenant niet verwijderen

Wanneer u uw Azure Active Directory-tenant hebt geconfigureerd, hebt u mogelijk ook abonnementen op basis van een licentie geactiveerd voor uw organisatie, zoals Azure Active Directory Premium P2, Office 365 Business Premium of Enterprise Mobility + Security E5. Deze abonnementen blokkeren map verwijderen totdat ze volledig worden verwijderd, om te onbedoeld gegevensverlies voorkomen. De abonnementen moeten zich een **inrichting ongedaan gemaakt** staat om toe te staan van het verwijderen van tenants. Een **verlopen** of **geannuleerd** abonnement wordt verplaatst naar de **uitgeschakelde** status en de laatste fase is de **Deprovisoned** staat. 

Zie de volgende tabel voor wat u kunt verwachten wanneer u een proefversie Office 365-abonnement is verlopen (dit is exclusief betaalde Partner/CSP, Enterprise Agreement of Volume Licensing). Zie voor meer informatie over Office 365 gegevens bewaren en abonnement lifecycle [wat gebeurt er met mijn gegevens en toegang wanneer mijn Office 365 voor bedrijven-abonnement is beëindigd?](https://support.office.com/article/what-happens-to-my-data-and-access-when-my-office-365-for-business-subscription-ends-4436582f-211a-45ec-b72e-33647f97d8a3). 

Abonnementsstatus | Gegevens | Toegang tot gegevens
----- | ----- | -----
Actieve (voor de proefversie van 30 dagen)  | Gegevens die toegankelijk is voor alle    | <li>Gebruikers hebben normale toegang tot Office 365-bestanden of toepassingen<li>Beheerders hebben de normale wijze toegang tot Office 365-beheercentrum en bronnen 
Verlopen (30 dagen)   | Gegevens die toegankelijk is voor alle    | <li>Gebruikers hebben normale toegang tot Office 365-bestanden of toepassingen<li>Beheerders hebben de normale wijze toegang tot Office 365-beheercentrum en bronnen
Uitgeschakeld (30 dagen) | Gegevens die toegankelijk is voor alleen administratie  | <li>Gebruikers geen toegang tot Office 365-bestanden of toepassingen<li>Beheerders kunnen toegang krijgen tot de Office 365-beheercentrum, maar kan geen licenties toewijzen aan of gebruikers bijwerken
Ongedaan gemaakt (30 dagen na uitgeschakeld) | Gegevens zijn verwijderd (automatisch verwijderd als er geen andere services gebruikt worden) | <li>Gebruikers geen toegang tot Office 365-bestanden of toepassingen<li>Beheerders hebben toegang tot de Office 365-beheercentrum aanschaffen en andere abonnementen beheren 

U kunt een abonnement in plaatst een **Deprovisoned** status moet worden verwijderd in 3 dagen met behulp van de Microsoft Store voor bedrijven-beheercentrum. Deze functie is binnenkort beschikbaar voor Office 365-beheercentrum.

1. Aanmelden bij de [Microsoft Store voor bedrijven-beheercentrum](https://businessstore.microsoft.com/en-us/manage/) met een account dat een globale beheerder in de tenant. Als u probeert te verwijderen van de tenant 'Contoso' waarvoor de initiële standaard domein contoso.onmicrosoft.com, aanmelden met een UPN zoals admin@contoso.onmicrosoft.com.

2. Ga naar de **beheren** tabblad en selecteer **producten en Services**, kies vervolgens het abonnement dat u wilt annuleren. Nadat u op **annuleren**, vernieuw de pagina.
  
  ![Koppeling voor het verwijderen van abonnement verwijderen](./media/directory-delete-howto/delete-command.png)
  
3. Selecteer **verwijderen** verwijderen van het abonnement en de voorwaarden accepteren. Alle gegevens worden definitief verwijderd binnen drie dagen. Als u van gedachten verandert, kunt u het abonnement tijdens de periode van drie dagen opnieuw activeren.
  
  ![algemene voorwaarden](./media/directory-delete-howto/delete-terms.png)

4. Nu de abonnementsstatus is gewijzigd, wordt het abonnement is gemarkeerd voor verwijdering. De abonnement-eneters de **inrichting ongedaan gemaakt** status 72 uur later opnieuw.

5. Als u een abonnement op uw tenant hebt verwijderd en 72 uur zijn verstreken, kunt u zich aanmelden terug in de Azure AD-beheercentrum opnieuw en er moeten er is geen actie vereist en er zijn geen abonnementen blokkeren van het verwijderen van uw tenants. U moet mogelijk uw Azure AD-tenant is verwijderd.
  
  ![scherm verwijderen abonnement selectievakje doorgeven](./media/directory-delete-howto/delete-checks-passed.png)

## <a name="next-steps"></a>Volgende stappen
[Documentatie voor Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)
