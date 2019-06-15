---
title: Verwijderen van een Azure AD-directory - Azure Active Directory | Microsoft Docs
description: Wordt uitgelegd hoe u een Azure AD-directory voor verwijdering, met inbegrip van self-service mappen
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 04/15/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 91ac6b4530414850c52605bac8cb701aa2b877d4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60473130"
---
# <a name="delete-a-directory-in-azure-active-directory"></a>Een map in Azure Active Directory verwijderen

Wanneer een Azure AD-directory wordt verwijderd, worden ook alle resources die zijn opgenomen in de map verwijderd. Bereid uw organisatie door het minimaliseren van alle bijbehorende resources, voordat u verwijderen. Alleen een globale beheerder van Azure Active Directory (Azure AD) u een Azure AD-directory via de portal verwijderen kunt.

## <a name="prepare-the-directory"></a>De directory voorbereiden

U kunt een map niet verwijderen in Azure AD pas verschillende controles wordt doorgegeven. Deze controles Verminder risico dat het verwijderen van een Azure AD-directory een negatieve gevolgen heeft voor gebruikerstoegang, zoals de mogelijkheid om aan te melden bij Office 365 of toegang tot resources in Azure. Bijvoorbeeld, als de map die is gekoppeld aan een abonnement per ongeluk is verwijderd, klikt u vervolgens gebruikers geen toegang tot de Azure-resources voor dat abonnement. Er wordt gecontroleerd of aan de volgende voorwaarden is voldaan:

* Er mag geen gebruikers in de map met uitzondering van een hoofdbeheerder die de directory verwijderen. Andere gebruikers moeten worden verwijderd voordat de directory kan worden verwijderd. Als gebruikers van on-premises worden gesynchroniseerd, klikt u vervolgens synchronisatie moet eerst worden uitgeschakeld en de gebruikers moeten worden verwijderd uit de clouddirectory via de Azure portal of Azure PowerShell-cmdlets.
* Er mogen in de directory geen toepassingen aanwezig zijn. Alle toepassingen moeten worden verwijderd voordat de map kan worden verwijderd.
* Er mag geen multi-factor authenticatieproviders die aan de map is gekoppeld.
* Aan de directory mogen geen abonnementen op Microsoft Online Services zijn gekoppeld, zoals een abonnement op Microsoft Azure, Office 365 of Azure AD Premium. Als er bijvoorbeeld een standaarddirectory voor u is gemaakt in Azure, kunt u deze directory niet verwijderen als uw Azure-abonnement deze directory gebruikt voor verificatie. Het is evenmin mogelijk om een directory te verwijderen als een andere gebruiker er een abonnement aan heeft gekoppeld.

## <a name="delete-the-directory"></a>De directory verwijderen

1. Aanmelden bij de [Azure AD-beheercentrum](https://aad.portal.azure.com) met een account dat de globale beheerder voor uw organisatie.

2. Selecteer **Azure Active Directory**.

3. Ga naar de map die u wilt verwijderen.
  
   ![Controleer of de organisatie voordat u verwijdert](./media/directory-delete-howto/delete-directory-command.png)

4. Selecteer **verwijderen directory**.
  
   ![Selecteer de opdracht voor het verwijderen van de organisatie](./media/directory-delete-howto/delete-directory-list.png)

5. Als uw directory voldoet niet aan een of meer controles, bent u hebt opgegeven met een koppeling naar meer informatie over hoe u om door te geven. Nadat u alle controles kunt toevoegen, selecteert u **verwijderen** om het proces te voltooien.

## <a name="if-you-cant-delete-the-directory"></a>Als u de map niet verwijderen

Wanneer u uw Azure AD-directory hebt geconfigureerd, hebt u mogelijk ook abonnementen op basis van een licentie geactiveerd voor uw organisatie, zoals Azure AD Premium P2, Office 365 Business Premium of Enterprise Mobility + Security E5. Als u wilt voorkomen dat onbedoeld gegevensverlies, verwijderen u een map niet tot de abonnementen zijn volledig verwijderd. De abonnementen moeten zich een **inrichting ongedaan gemaakt** staat om toe te staan van de directory verwijderen. Een **verlopen** of **geannuleerd** abonnement wordt verplaatst naar de **uitgeschakelde** status en de laatste fase is de **inrichting ongedaan gemaakt** staat.

Zie de volgende tabel voor wat u kunt verwachten wanneer u een proefversie Office 365-abonnement is verlopen (dit is exclusief betaalde Partner/CSP, Enterprise Agreement of Volume Licensing). Zie voor meer informatie over Office 365 gegevens bewaren en abonnement lifecycle [wat gebeurt er met mijn gegevens en toegang wanneer mijn Office 365 voor bedrijven-abonnement is beëindigd?](https://support.office.com/article/what-happens-to-my-data-and-access-when-my-office-365-for-business-subscription-ends-4436582f-211a-45ec-b72e-33647f97d8a3). 

Abonnementsstatus | Gegevens | Toegang tot gegevens
----- | ----- | -----
Actieve (voor de proefversie van 30 dagen) | Gegevens die toegankelijk is voor alle | Gebruikers hebben normale toegang tot Office 365-bestanden of toepassingen<br>Beheerders hebben de normale wijze toegang tot Microsoft 365-beheercentrum en bronnen 
Verlopen (30 dagen) | Gegevens die toegankelijk is voor alle| Gebruikers hebben normale toegang tot Office 365-bestanden of toepassingen<br>Beheerders hebben de normale wijze toegang tot Microsoft 365-beheercentrum en bronnen
Uitgeschakeld (30 dagen) | Gegevens die toegankelijk is voor alleen administratie | Gebruikers geen toegang tot Office 365-bestanden of toepassingen<br>Beheerders kunnen krijgen tot het beheercentrum van Microsoft 365, maar kan geen licenties toewijzen aan of gebruikers bijwerken
Ongedaan gemaakt (30 dagen na uitgeschakeld) | Gegevens zijn verwijderd (automatisch verwijderd als er geen andere services gebruikt worden) | Gebruikers geen toegang tot Office 365-bestanden of toepassingen<br>Beheerders hebben toegang tot het Microsoft 365-beheercentrum aanschaffen en andere abonnementen beheren

## <a name="delete-a-subscription"></a>Een abonnement verwijderen

U kunt een abonnement in de status van de inrichting ongedaan gemaakt om u te worden verwijderd in drie dagen via het Microsoft 365-beheercentrum plaatsen.

1. Aanmelden bij de [Microsoft 365-beheercentrum](https://admin.microsoft.com) met een account dat een globale beheerder in uw organisatie. Als u probeert te verwijderen van de map 'Contoso' waarvoor de initiële standaard domein contoso.onmicrosoft.com, meld u aan met een UPN zoals admin@contoso.onmicrosoft.com.

2. Selecteer **facturering** en selecteer **abonnementen**, kies vervolgens het abonnement dat u wilt annuleren. Nadat u op **annuleren**, vernieuw de pagina.
  
   ![Koppeling voor het verwijderen van abonnement verwijderen](./media/directory-delete-howto/delete-command.png)
  
3. Selecteer **verwijderen** verwijderen van het abonnement en de voorwaarden accepteren. Alle gegevens binnen drie dagen definitief verwijderd. Als u van gedachten verandert, kunt u het abonnement tijdens de periode van drie dagen opnieuw activeren.
  
   ![Lees voorwaarden en bepalingen](./media/directory-delete-howto/delete-terms.png)

4. Nu de abonnementsstatus is gewijzigd, wordt het abonnement is gemarkeerd voor verwijdering. Het abonnement voert de **inrichting ongedaan gemaakt** status 72 uur later opnieuw.

5. Als u een abonnement hebt verwijderd in uw directory en 72 uur zijn verstreken, kunt u zich aanmelden moet terug in de Azure AD-beheercentrum opnieuw en er geen actie vereist en er zijn geen abonnementen blokkeert de verwijdering van uw directory. U zou het mogelijk is uw Azure AD-directory te verwijderen.
  
   ![scherm verwijderen abonnement selectievakje doorgeven](./media/directory-delete-howto/delete-checks-passed.png)

## <a name="i-have-a-trial-subscription-that-blocks-deletion"></a>Ik heb een proefabonnement waarmee verwijdering wordt geblokkeerd

Er zijn [Self-service-aanmelding producten](https://docs.microsoft.com/office365/admin/misc/self-service-sign-up?view=o365-worldwide) , zoals Microsoft Power BI, Rights Management Services, Microsoft Power Apps of Dynamics 365, individuele gebruikers kunnen zich aanmelden via Office 365, waardoor ook een gastgebruiker voor de verificatie in wordt gemaakt uw Azure AD-directory. Deze producten selfservice blokkeren directory verwijderingen totdat ze volledig worden verwijderd uit de map, om gegevensverlies te voorkomen. Ze kunnen worden verwijderd door de Azure AD-beheerder of de gebruiker zich heeft geregistreerd afzonderlijk of aan het product is toegewezen.

Er zijn twee soorten selfservice aanmelding producten in de manier waarop ze worden toegewezen: 

* Toewijzing van de organisatie op serverniveau: Een Azure AD-beheerder wijst het product voor de hele organisatie en een gebruiker kan worden actief gebruik van de service met deze toewijzing org-niveau, zelfs als ze zijn niet afzonderlijk een licentie.
* Toewijzing van de niveau van de gebruiker: Het product naar zichzelf sturen zonder dat een beheerder. tijdens de self-service-aanmelding op een afzonderlijke gebruiker in feite toegewezen Als de organisatie wordt beheerd door een beheerder (Zie [beheerder overnemen van een niet-beheerde adreslijst](domains-admin-takeover.md), en vervolgens de beheerder het product rechtstreeks aan gebruikers zonder self-service-aanmelding toewijzen kan.  

Wanneer u begint met het verwijderen van de registratie selfservice-product, wordt de actie permanent verwijdert de gegevens en verwijdert u alle gebruikerstoegang tot de service. Elke gebruiker die de aanbieding is toegewezen, afzonderlijk of op het organisatieniveau van de wordt vervolgens geblokkeerd aanmelding bij of toegang tot alle bestaande gegevens. Als u wilt voorkomen dat gegevens verloren gaan met het selfservice aanmelding product [Microsoft Power BI-dashboards](https://docs.microsoft.com/power-bi/service-export-to-pbix) of [Rights Management Services-beleidsconfiguratie](https://docs.microsoft.com/azure/information-protection/configure-policy#how-to-configure-the-azure-information-protection-policy), zorg ervoor dat de gegevens een back-up en ergens anders opgeslagen.

Zie voor meer informatie over de momenteel beschikbare selfservice Meld u aan producten en services, [beschikbare programma's selfservice](https://docs.microsoft.com/office365/admin/misc/self-service-sign-up?view=o365-worldwide#available-self-service-programs).

Zie de volgende tabel voor wat u kunt verwachten wanneer u een proefversie Office 365-abonnement is verlopen (dit is exclusief betaalde Partner/CSP, Enterprise Agreement of Volume Licensing). Zie voor meer informatie over Office 365 gegevens bewaren en abonnement lifecycle [wat gebeurt er met mijn gegevens en toegang wanneer mijn Office 365 voor bedrijven-abonnement is beëindigd?](https://docs.microsoft.com/office365/admin/subscriptions-and-billing/what-if-my-subscription-expires?view=o365-worldwide).

Productstatus | Gegevens | Toegang tot gegevens
------------- | ---- | --------------
Actieve (voor de proefversie van 30 dagen) | Gegevens die toegankelijk is voor alle | Gebruikers hebben normale toegang tot Self-servicegebruikers Meld u aan product, bestanden of toepassingen<br>Beheerders hebben de normale wijze toegang tot Microsoft 365-beheercentrum en bronnen
Verwijderen | Gegevens zijn verwijderd | Gebruikers geen toegang tot Self-servicegebruikers Meld u aan product, bestanden of toepassingen<br>Beheerders hebben toegang tot het Microsoft 365-beheercentrum aanschaffen en andere abonnementen beheren

## <a name="how-can-i-delete-a-self-service-sign-up-product-in-the-azure-portal"></a>Hoe verwijder ik een selfservice aanmelding product in de Azure-portal?

U kunt een selfservice aanmelding product zoals Microsoft Power BI of Azure Rights Management Services in plaatst een **verwijderen** status moet onmiddellijk worden verwijderd in de Azure AD-portal.

1. Aanmelden bij de [Azure AD-beheercentrum](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) met een account dat een globale beheerder in de organisatie. Als u probeert te verwijderen van de map 'Contoso' waarvoor de initiële standaard domein contoso.onmicrosoft.com, aanmelden met een UPN zoals admin@contoso.onmicrosoft.com.

2. Selecteer **licenties**, en selecteer vervolgens **Self-service-aanmelding producten**. Hier ziet u alle selfservice aanmelding producten los van de abonnementen op basis van een seat. Kies het product die u permanent wilt verwijderen. Hier volgt een voorbeeld in Microsoft Power BI:

    ![de gebruikersnaam is een typefout gemaakt of is niet gevonden](./media/directory-delete-howto/licenses-page.png)

3. Selecteer **verwijderen** verwijderen van het product en accepteer de voorwaarden die gegevens onmiddellijk en onherroepelijk is verwijderd. Deze delete-actie verwijdert u alle gebruikers en verwijderen van de organisatie toegang tot het product. Klik op Ja om door te gaan met het verwijderen.  

    ![de gebruikersnaam is een typefout gemaakt of is niet gevonden](./media/directory-delete-howto/delete-product.png)

4. Wanneer u selecteert **Ja**, het verwijderen van het product zelf wordt gestart. Er is een melding dat u van de verwijdering wordt uitgevoerd vertelt.  

    ![de gebruikersnaam is een typefout gemaakt of is niet gevonden](./media/directory-delete-howto/progress-message.png)

5. Nu de Productstatus van de self-service-aanmelding is gewijzigd in **verwijderde**. Wanneer u de pagina vernieuwt, het product moet worden verwijderd uit de **Self-service-aanmelding producten** pagina.  

    ![de gebruikersnaam is een typefout gemaakt of is niet gevonden](./media/directory-delete-howto/product-deleted.png)

6. Nadat u alle producten hebt verwijderd, kunt u zich opnieuw bij de Azure AD-beheercentrum opnieuw en er mogen zich geen actie vereist en er zijn geen producten blokkeert de verwijdering van uw directory. U zou het mogelijk is uw Azure AD-directory te verwijderen.

    ![de gebruikersnaam is een typefout gemaakt of is niet gevonden](./media/directory-delete-howto/delete-organization.png)

## <a name="next-steps"></a>Volgende stappen

[Documentatie voor Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)
