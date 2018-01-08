---
title: Overzicht van Azure Active Direcory-tenant | Microsoft Docs
description: In dit artikel wordt uitgelegd wat een Azure AD-tenant is en hoe u Azure beheert via Azure Active Directory
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro;oldportal
ms.openlocfilehash: 0218365a21f241b5672774005cd6e0417fba358b
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="manage-your-azure-ad-directory"></a>Uw Azure AD-directory beheren

## <a name="what-is-an-azure-ad-tenant"></a>Wat is een Azure AD-tenant?
In Azure Active Directory (Azure AD) is een tenant het exclusieve exemplaar van een Azure Active-directory dat uw organisatie na aanmelding voor een Microsoft-cloudservice (zoals Azure of Office 365) krijgt toegewezen. Elke Azure AD-directory is uniek en werkt afzonderlijk van andere Azure AD-directory’s. Net zoals een kantoorgebouw een beveiligd bedrijfsmiddel is van uw organisatie, is ook een Azure AD-directory ontworpen als een beveiligd bedrijfsmiddel dat alleen door uw organisatie kan worden gebruikt. De architectuur van Azure AD zorgt ervoor dat klantgegevens en identiteitsgegevens van elkaar zijn gescheiden, zodat gebruikers en beheerders van de ene Azure AD-directory niet per ongeluk of opzettelijk gegevens in een andere directory kunnen weergeven.

![Azure Active Directory beheren](./media/active-directory-administer/aad_portals.png)

## <a name="how-can-i-get-an-azure-ad-directory"></a>Hoe krijg ik een Azure AD-directory?
Azure AD levert de kerndirectory en identiteitsbeheermogelijkheden voor de meeste cloudservices van Microsoft, waaronder:

* Azure
* Microsoft Office 365
* Microsoft Dynamics CRM Online
* Microsoft Intune

U krijgt een Azure AD-directory wanneer u zich aanmeldt voor een van deze Microsoft-cloudservices. U kunt naar behoefte aanvullende directory’s maken. Zo kunt u bijvoorbeeld uw eerste directory gebruiken als productiedirectory en nog een tweede directory maken voor testen en fasering.

### <a name="using-the-azure-ad-directory-that-comes-with-a-new-azure-subscription"></a>De Azure AD-directory gebruiken die hoort bij een nieuw Azure-abonnement

Het is raadzaam om voor registratie bij andere Microsoft-services, het beheerdersaccount te gebruiken dat u hebt gebruikt voor registratie bij uw eerste service. De gegevens die u bij de eerste registratie voor een Microsoft-service opgeeft, worden gebruikt voor het maken van een nieuw exemplaar van de Azure AD-directory voor uw organisatie. Als u deze directory gebruikt om aanmeldingspogingen te verifiëren wanneer u zich registreert voor andere Microsoft-services, kunnen voor de aanmelding de bestaande gebruikersaccounts, beleidsregels, instellingen of on-premises directoryintegratie worden gebruikt die in de standaarddirectory zijn geconfigureerd.

Als u zich bijvoorbeeld registreert voor een abonnement op Microsoft Intune en u daarna de on-premises Active Directory synchroniseert met uw Azure AD-directory, kunt u zich registreren voor een andere Microsoft-service, zoals Office 365, en eenvoudig beschikken over dezelfde voordelen van directoryintegratie als bij Microsoft Intune.

Zie [Uw on-premises directory's integreren met Azure Active Directory](active-directory-aadconnect.md) voor meer informatie over de integratie van uw on-premises directory in Azure AD.

### <a name="associate-an-existing-azure-ad-directory-with-a-new-azure-subscription"></a>Een bestaande Azure AD-directory koppelen aan een nieuw Azure-abonnement
U kunt een nieuw Azure-abonnement koppelen aan dezelfde directory waarmee aanmeldingsbewerkingen voor een bestaand Office 365- of Microsoft Intune-abonnement worden geverifieerd. Zie [Transfer ownership of an Azure subscription to another account](../billing/billing-subscription-transfer.md) (Eigendom van een Azure-abonnement naar een ander account overdragen) voor meer informatie over dit scenario.

### <a name="create-an-azure-ad-directory-by-signing-up-for-a-microsoft-cloud-service-as-an-organization"></a>Een Azure AD-directory maken door u als organisatie te registreren voor een Microsoft-cloudservice
Als u nog geen abonnement hebt op een Microsoft-cloudservice, kunt u een van de volgende koppelingen gebruiken om u te registreren. Als u zich voor uw eerste service registreert, wordt er automatisch een Azure AD-directory gemaakt.

* [Microsoft Azure](https://account.azure.com/organization)
* [Office 365](http://products.office.com/business/compare-office-365-for-business-plans/)
* [Microsoft Intune](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0%20)

### <a name="how-to-change-the-default-directory-for-a-subscription"></a>De standaarddirectory voor een abonnement wijzigen

1. Meld u bij het [Azure-accountcentrum](https://account.azure.com/Subscriptions) aan met een account van de accountbeheerder voor het abonnement waarvan u het eigendom wilt overdragen.
2. Zorg ervoor dat de gebruiker die u wilt instellen als de eigenaar van het abonnement, aanwezig is in de doeldirectory.
3. Klik op **Abonnement overdragen**.
4. Geef de ontvanger op. De ontvanger ontvangt automatisch een e-mail met een acceptatielink.
5. De ontvanger klikt op de link en volgt de instructies, waaronder het invoeren van zijn of haar betaalgegevens. Als de ontvanger klaar is, wordt het abonnement overgedragen. 
6. Als de overdracht van abonnementseigendom geslaagd is, is de standaardmap van het abonnement gewijzigd naar de map waarin de gebruiker zich bevindt.

Zie [Transfer Azure subscription ownership to another account](../billing/billing-subscription-transfer.md) (Het eigendom van een Azure-abonnement overdragen aan een ander account) voor meer informatie

### <a name="manage-the-default-directory-in-azure"></a>De standaarddirectory in Azure beheren
Wanneer u zich registreert voor Azure, wordt er een standaarddirectory van Azure AD gekoppeld aan uw abonnement. Er zijn geen kosten verbonden aan het gebruik van Azure AD en uw directory's zijn eveneens gratis. Er zijn betaalde Azure AD-services waarvoor een afzonderlijke licentie nodig is en die aanvullende functionaliteit bieden, zoals de huisstijl van het bedrijf in het aanmeldingsscherm en selfservice voor wachtwoordherstel. U kunt ook een aangepast domein maken met een DNS-naam waarvan u de eigenaar bent, in plaats van het standaarddomein *. onmicrosoft.com te gebruiken.

## <a name="how-can-i-manage-directory-data"></a>Hoe kan ik directorygegevens beheren?
Als u een of meer abonnementen op Microsoft-cloudservices wilt beheren, gebruikt u het [beheercentrum van Azure AD](https://aad.portal.azure.com), de Microsoft Intune-accountportal of het [Office 365-beheercentrum](https://portal.office.com/) om de directorygegevens van uw organisatie te beheren. U kunt ook [PowerShell-cmdlets van Azure Active Directory](https://docs.microsoft.com/powershell/azure/active-directory) gebruiken om gegevens te beheren die in Azure AD zijn opgeslagen.

Via deze portals (en cmdlets) kunt u het volgende doen:

* Accounts voor gebruikers en groepen maken en beheren
* Gerelateerde cloudservices beheren voor de abonnementen van uw organisatie
* On-premises integratie met services van Azure AD voor identiteitscontrole en verificatie instellen

Het beheercentrum van Azure AD, het Office 365-beheercentrum, de Microsoft Intune-accountportal en de Azure AD-cmdlets lezen uit en schrijven naar één gedeeld exemplaar van Azure AD dat is gekoppeld aan de directory van uw organisatie. Elk van deze hulpprogramma's fungeert als een front-interface die gegevens in de directory ophaalt of wijzigt.
Wanneer u gegevens van uw organisatie wijzigt via een van de portals of cmdlets terwijl u bent aangemeld in de context van een van deze services, worden de wijzigingen ook weergegeven in de andere portals als u zich de volgende keer aanmeldt. Deze gegevens worden gedeeld tussen de Microsoft-cloudservices waarop u bent geabonneerd.

Als u bijvoorbeeld het Office 365-beheercentrum gebruikt om de aanmelding van een gebruiker te blokkeren, kan die gebruiker zich ook niet meer aanmelden bij andere services waarop uw organisatie momenteel is geabonneerd. Als u hetzelfde gebruikersaccount bekijkt in de Microsoft Intune-accountportal, ziet u dat de gebruiker daar ook is geblokkeerd.

## <a name="how-can-i-add-and-manage-multiple-directories"></a>Hoe kan ik meerdere directory’s toevoegen en beheren?
U kunt een [Azure AD-directory toevoegen via Azure Portal](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory). Vul de gegevens in en selecteer **Maken**.

U kunt elke directory als volledig onafhankelijke resource beheren: elke directory is een peer, volledig uitgerust en logisch onafhankelijk van andere directory's die u beheert. Er is geen sprake van een structuur met boven- en onderliggende directory's. Onder deze onafhankelijkheid van uw directory’s vallen ook resourceonafhankelijkheid, beheeronafhankelijkheid en synchronisatieonafhankelijkheid.

* **Resourceonafhankelijkheid**. Wanneer u een resource maakt in of verwijdert uit de ene directory, heeft dat geen gevolgen voor de resources in andere directory's. Externe gebruikers vormen hier een gedeeltelijke uitzondering op. Als u voor een bepaalde directory het aangepaste domein contoso.com gebruikt, kan dit domein niet meer voor andere directory’s worden gebruikt.
* **Beheeronafhankelijkheid**.  Als een gebruiker die geen beheerder is van de directory Contoso een testdirectory Test maakt:
  
  * De beheerders van de directory Contoso hebben geen directe beheerdersrechten voor de directory Test, tenzij de beheerder van Test hun deze rechten verleent. Beheerders van Contoso kunnen de toegang tot de directory Test beheren doordat ze het beheer hebben over het gebruikersaccount waarmee Test is gemaakt.
    
  * Als u in de ene directory een beheerdersrol voor een gebruiker toewijst of verwijdert, heeft deze wijziging geen gevolgen voor beheerdersrollen die de betreffende gebruiker mogelijk heeft in een andere directory.
* **Synchronisatieonafhankelijkheid**. U kunt elke Azure AD-tenant onafhankelijk configureren, zodat gegevens worden gesynchroniseerd vanuit één exemplaar. Hiervoor gebruikt u het hulpprogramma voor directorysynchronisatie van Azure AD Connect.

In tegenstelling tot andere Azure-resources, zijn uw directory's geen onderliggende resources van een Azure-abonnement. Wanneer u uw Azure-abonnement annuleert of als het abonnement verloopt, hebt u nog gewoon toegang tot uw directorygegevens via Azure AD PowerShell, de Azure Graph API of andere interfaces, zoals het Office 365-beheercentrum. U kunt ook een ander abonnement koppelen aan de directory.

## <a name="how-to-prepare-to-delete-an-azure-ad-directory"></a>Wat moet ik doen om een Azure AD-directory te verwijderen?
Een hoofdbeheerder kan een Azure AD-directory verwijderen via de portal. Wanneer een directory wordt verwijderd, worden alle resources in de lijst ook verwijderd. Controleer of u de directory echt niet meer nodig hebt voordat u deze verwijdert.

> [!NOTE]
> Als de gebruiker is aangemeld met een werk- of schoolaccount, moet de gebruiker niet proberen om zijn of haar basisdirectory te verwijderen. Als een gebruiker bijvoorbeeld is aangemeld als joe@contoso.onmicrosoft.com, kan deze niet de directory verwijderen die contoso.onmicrosoft.com als standaarddomein gebruikt.

Als u een Azure AD-directory wilt verwijderen, moet aan bepaalde voorwaarden worden voldaan. Dit vermindert het risico dat het verwijderen van een directory een negatieve invloed heeft op gebruikers of toepassingen, zoals op de mogelijkheid van gebruikers om zich aan te melden bij Office 365 of om resources te openen in Azure. Het per ongeluk verwijderen van een directory voor een abonnement heeft bijvoorbeeld tot gevolg dat gebruikers geen toegang meer hebben tot de Azure-resources voor dat abonnement.

Er wordt gecontroleerd of aan de volgende voorwaarden is voldaan:

* De enige gebruiker in de directory is de hoofdbeheerder die de directory gaat verwijderen. Andere gebruikers moeten worden verwijderd voordat de directory kan worden verwijderd. Als gebruikers on-premises worden gesynchroniseerd, moet de synchronisatie worden uitgeschakeld. Gebruikers moeten vervolgens worden verwijderd uit de clouddirectory via Azure Portal of cmdlets van Azure PowerShell. Verwijdering van groepen of contactpersonen, zoals contactpersonen die vanuit het Office 365-beheercentrum zijn toegevoegd, is geen vereiste.
* Er mogen in de directory geen toepassingen aanwezig zijn. Alle toepassingen moeten worden verwijderd om de directory te kunnen verwijderen.
* Aan de directory kunnen geen Multi-Factor Authentication-providers worden gekoppeld.
* Aan de directory mogen geen abonnementen op Microsoft Online Services zijn gekoppeld, zoals een abonnement op Microsoft Azure, Office 365 of Azure AD Premium. Als er bijvoorbeeld een standaarddirectory voor u is gemaakt in Azure, kunt u deze directory niet verwijderen als uw Azure-abonnement deze directory gebruikt voor verificatie. Het is evenmin mogelijk om een directory te verwijderen als een andere gebruiker er een abonnement aan heeft gekoppeld. 


## <a name="next-steps"></a>Volgende stappen
* [Azure AD Forum](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=windowsazuread)
* [Azure Multi-Factor Authentication-forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=windowsazureactiveauthentication)
* [StackOverflow voor Azure-vragen](https://stackoverflow.com/questions/tagged/azure-active-directory)
* [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory)
* [Beheerdersrollen toewijzen in Azure AD](active-directory-assign-admin-roles-azure-portal.md)
