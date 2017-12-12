---
title: Een aangepast domein toevoegen aan Azure AD | Microsoft Docs
description: Legt uit hoe u een aangepast domein toevoegen in Azure Active Directory.
services: active-directory
author: curtand
manager: mtillman
ms.assetid: 0a90c3c5-4e0e-43bd-a606-6ee00f163038
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: e7b85d5f4cd19c94fe904f16090e174d87ea120b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="quickstart-add-a-custom-domain-name-to-azure-active-directory"></a>Snelstartgids: Een aangepaste domeinnaam toevoegen aan Azure Active Directory

Elke Azure AD-directory wordt geleverd met een initiële domeinnaam in de vorm van *domainname*. onmicrosoft.com. De initiële domeinnaam kan niet worden gewijzigd of verwijderd, maar u kunt uw zakelijke domeinnaam toevoegen aan Azure AD ook. Uw organisatie heeft bijvoorbeeld waarschijnlijk andere domeinnamen gebruikt voor bedrijven en gebruikers die zich aanmeldt met uw zakelijke domeinnaam. Het toevoegen van aangepaste domeinnamen naar Azure AD, kunt u gebruikersnamen toewijzen in de directory die bekend aan uw gebruikers, zoals zijn 'alice@contoso.com.' in plaats van ' Els @*domeinnaam*. onmicrosoft.com'. Het proces is eenvoudig:

1. Voeg de aangepaste domeinnaam toe aan uw directory.
2. Voeg een DNS-vermelding voor de domeinnaam toe aan de domeinnaamregistrar.
3. Verifieer de aangepaste domeinnaam in Azure AD.

## <a name="add-the-custom-domain-name-to-your-directory"></a>Voeg de aangepaste domeinnaam toe aan uw directory.
1. Aanmelden bij de [Azure-portal](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) met een account met globale beheerdersrechten voor de map.
2. Selecteer aan de linkerkant **aangepaste domeinnamen**.
3. Selecteer **aangepaste domein toevoegen**.
   
   ![Selecteer de Add-opdracht](./media/add-custom-domain/add-custom-domain.png)
5. Op **aangepaste domeinnamen**, voer de naam van uw aangepaste domein in het vak, bijvoorbeeld 'contoso.com' en selecteer vervolgens **domein toevoegen**. Vergeet niet de extensie .com, .net of een andere extensie van het hoogste niveau toe te voegen.
6. Op ***domainname*** (dat wil zeggen, de naam van het nieuwe domein is voor de titel), de DNS-vermelding verzamelen om later te gebruiken om te controleren of de aangepaste domeinnaam in Azure AD.
   
   ![DNS-vermelding informatie ophalen](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

> [!TIP]
> Als u van plan bent om te federeren van uw lokale Windows Server AD met Azure AD, moet u selecteert de **ik van plan bent dit domein te configureren voor eenmalige aanmelding met mijn lokale Active Directory** selectievakje wanneer u het Azure AD Connect-hulpprogramma om te worden uitgevoerd Synchroniseer uw mappen. U moet ook registreren van de dezelfde domeinnaam die u selecteert voor het federeren met uw on-premises directory in de **Azure AD Domain** stap in de wizard. [In deze instructies](./connect/active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation) ziet u hoe die stap van de wizard eruitziet. Als u het hulpprogramma Azure AD Connect niet hebt, kunt u het [hier](http://go.microsoft.com/fwlink/?LinkId=615771) downloaden.

## <a name="add-a-dns-entry-for-the-domain-name-at-the-domain-name-registrar"></a>Voeg een DNS-vermelding voor de domeinnaam toe aan de domeinnaamregistrar.
De volgende stap voor het gebruik van de aangepaste domeinnaam met Azure AD bestaat uit het bijwerken van het DNS-zonebestand voor het domein. Azure AD kunt vervolgens controleren of uw organisatie eigenaar is van de aangepaste domeinnaam. U kunt [Azure DNS](https://docs.microsoft.com/azure/dns/dns-getstarted-portal) voor uw Azure/Office 365/externe DNS-records in Azure, of Voeg de DNS-vermelding op [een andere DNS-registratieservice](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/).

1. Meld u aan bij de domeinnaamregistrar voor het domein. Als u geen toegang hebt om de DNS-vermelding bij te werken, vraagt u de persoon die of het team dat wel over deze toegang beschikt om stap 2 uit te voeren en u te laten weten wanneer deze is voltooid.
2. Werk het DNS-zonebestand voor het domein bij, door de DNS-vermelding toe te voegen die u van Azure AD hebt ontvangen. De DNS-vermelding leidt niet tot veranderingen in het gedrag van bijvoorbeeld mailroutering of webhosting.

## <a name="verify-the-custom-domain-name-in-azure-ad"></a>Verifieer de aangepaste domeinnaam in Azure AD.
Nadat u de DNS-vermelding hebt toegevoegd, kunt u de domeinnaam bij Azure AD verifiëren. De naam van een domein kan worden gecontroleerd nadat de DNS-records zijn doorgegeven. Deze doorgifte duurt vaak slechts enkele seconden, maar het kan ook wel eens een uur of langer duren. Als de verificatie de eerste keer niet werkt, probeer het dan later nog eens.

1. Aanmelden bij [Azure AD](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) met een account met globale beheerdersrechten voor de tenant.
2. Selecteer **aangepaste domeinnamen**.
3. Selecteer de niet-geverifieerde domeinnaam die u wilt controleren.
4. Controleer de gegevens en selecteer **controleren** om de verificatie te voltooien.

Nu kunt u [gebruikersnamen toewijzen die uw aangepaste domeinnaam omvatten](active-directory-users-create-azure-portal.md). U kunt de cloud-gebaseerde gebruikersaccounts maken of update gesynchroniseerd eerder lokale gebruikersaccountgegevens met behulp van uw aangepaste domeinnaam. U kunt ook wijzigen gesynchroniseerde gebruikersaccount domein achtervoegsel informatie met [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) of de [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

> [!TIP]
> U kunt tot maximaal 900 beheerde domeinnamen toevoegen. Als u alle domeinen voor Federatie on-premises met Active Directory configureert, kunt u met een maximum van 450 domeinnamen in elke directory up toevoegen. Zie voor meer informatie [federatieve en beheerde domeinnamen](https://docs.microsoft.com/azure/active-directory/active-directory-add-domain-concepts#federated-and-managed-domain-names).

## <a name="troubleshooting"></a>Problemen oplossen
Als u een aangepaste domeinnaam niet kan controleren, voert u de volgende probleemoplossingsstappen uit:

1. **Wacht een uur**. DNS-records moeten doorgeven voordat u Azure AD kunt controleren of het domein. Dit proces kan een uur of langer duren.
2. **Controleer of de DNS-record is opgegeven en of deze juist is**. Voer deze stap uit op de website van de domeinnaamregistrar voor het domein. Azure AD kan de domeinnaam niet controleren als 
  * De DNS-vermelding is niet aanwezig in het DNS-zonebestand
  * Het is niet exact overeen met de DNS-vermelding die Azure AD u opgegeven. 
  
  Als u geen toegang hebt tot de site van de domeinnaamregistrar om de DNS-records voor het domein bij te werken, deel de DNS-vermelding dan met de persoon die of het team dat in uw organisatie deze toegang heeft en vraag om de DNS-vermelding toe te voegen.
3. **Verwijder de domeinnaam uit andere mappen in Azure AD**. Een domeinnaam kan maar in één map worden geverifieerd. Als u een domeinnaam is momenteel geverifieerd in een andere map, kan niet het worden geverifieerd in uw directory nieuwe totdat deze is verwijderd op de andere. Zie [Aangepaste domeinnamen beheren](active-directory-domains-manage-azure-portal.md) voor meer informatie over het verwijderen van domeinnamen.    

Herhaal de stappen in dit artikel voor elk van uw domeinnamen toevoegen.

## <a name="learn-more"></a>Meer informatie
[Conceptueel overzicht van aangepaste domeinnamen in Azure AD](active-directory-domains-manage-azure-portal.md)

[Aangepaste domeinnamen beheren](active-directory-domains-manage-azure-portal.md)

## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids hebt u geleerd hoe u een aangepast domein toevoegen aan Azure AD. 

De volgende koppeling kunt u een nieuw aangepast domein toevoegen in Azure AD via de Azure-portal.

> [!div class="nextstepaction"]
> [Een aangepast domein toevoegen](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/QuickStart) 