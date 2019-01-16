---
title: Gebruikers beheren in Azure Blockchain Workbench
description: Over het beheren van gebruikers in Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 01/14/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: a481f5b073b7e8db9135d7b10cf602168290738a
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54331364"
---
# <a name="manage-users-in-azure-blockchain-workbench"></a>Gebruikers beheren in Azure Blockchain Workbench

Azure Blockchain Workbench bevat Gebruikersbeheer voor mensen en organisaties die deel van uw consortium uitmaken.

## <a name="prerequisites"></a>Vereisten

Een Blockchain Workbench-implementatie is vereist. Zie [Azure Blockchain Workbench-implementatie](deploy.md) voor meer informatie over de implementatie.

## <a name="add-azure-ad-users"></a>Azure AD-gebruikers toevoegen

De Azure Blockchain Workbench maakt gebruik van Azure Active Directory (Azure AD) voor verificatie, toegangsbeheer en rollen. Gebruikers in de Blockchain Workbench, Azure AD-tenant kunnen verifiëren en gebruiken van Blockchain Workbench. Gebruikers toevoegen aan de beheerdersrol van de toepassing om te communiceren en acties uitvoeren.

Blockchain Workbench-gebruikers moeten aanwezig zijn in de Azure AD-tenant voordat u ze aan toepassingen en functies toewijzen kunt. Gebruikers toevoegen aan Azure AD, gebruikt u de volgende stappen uit:

1.  Meld u aan bij [Azure Portal](https://portal.azure.com).
2.  Selecteer uw account in de rechterbovenhoek en schakel over naar de Azure AD-tenant die is gekoppeld aan Blockchain Workbench.
3.  Selecteer **Azure Active Directory > gebruikers**. U ziet een lijst van gebruikers in uw directory.
4.  Selecteer om gebruikers toe aan de directory **nieuwe gebruiker**. Voor externe gebruikers, selecteert u **nieuwe gastgebruiker**.

    ![Nieuwe gebruiker](./media/manage-users/add-ad-user.png)

5.  Vul de vereiste velden voor de nieuwe gebruiker. Selecteer **Maken**.

Ga naar [Azure AD](../../active-directory/fundamentals/add-users-azure-active-directory.md) documentatie voor meer informatie over het beheren van gebruikers in Azure AD.

## <a name="manage-blockchain-workbench-administrators"></a>Blockchain Workbench beheerders beheren

Nadat gebruikers zijn toegevoegd aan de directory, wordt de volgende stap is om te kiezen welke gebruikers zijn beheerders Blockchain Workbench. Gebruikers in de **beheerder** groep zijn gekoppeld aan de **toepassing beheerdersrol** in Blockchain Workbench. Beheerders kunnen toevoegen of verwijderen van gebruikers, gebruikers toewijzen aan specifieke scenario's en nieuwe toepassingen maken.

Gebruikers toevoegen aan de **beheerder** groep in de Azure AD-map:

1.  Meld u aan bij [Azure Portal](https://portal.azure.com).
2.  Controleer of dat u zich in de Azure AD-tenant die is gekoppeld aan Blockchain Workbench door uw account in de rechterbovenhoek te selecteren.
3.  Selecteer **Azure Active Directory > bedrijfstoepassingen**.
4.  Selecteer de Azure AD-clienttoepassing voor Blockchain Workbench
    
    ![Alle registraties voor enterprise-toepassing](./media/manage-users/select-blockchain-client-app.png)

5.  Selecteer **gebruikers en groepen > gebruiker toevoegen**.
6.  In **toevoegen toewijzing**, selecteer **gebruikers**. Kies of zoek naar de gebruiker die u wilt toevoegen als beheerder. Klik op **Selecteer** wanneer u klaar bent te kiezen.

    ![Toewijzing toevoegen](./media/manage-users/add-user-assignment.png)

9.  Controleer of **rol** is ingesteld op **beheerder**
10. Selecteer **Toewijzen**. De toegevoegde gebruikers worden weergegeven in de lijst met de beheerdersrol is toegewezen.

    ![Blockchain-client-app-gebruikers](./media/manage-users/blockchain-admin-list.png)

## <a name="managing-blockchain-workbench-members"></a>Blockchain Workbench leden beheren

Gebruik de Blockchain Workbench-toepassing voor het beheren van gebruikers en organisaties die deel van uw consortium uitmaken. U kunt toevoegen of verwijderen van gebruikers tot toepassingen en functies.

1. [Open de Blockchain Workbench](deploy.md#blockchain-workbench-web-url) in uw browser en meld u aan als beheerder.

    ![Blockchain Workbench](./media/manage-users/blockchain-workbench-applications.png)

    Leden worden toegevoegd aan elke toepassing. Leden kunnen een of meer toepassingsrollen te initiëren van opdrachten of acties ondernemen hebben.

2. Voor het beheren van leden voor een toepassing, selecteer de tegel van een toepassing in de **toepassingen** deelvenster.

    Het aantal leden dat is gekoppeld aan de geselecteerde toepassing wordt weergegeven in de tegel leden.

    ![Toepassing selecteren](./media/manage-users/blockchain-workbench-select-application.png)


#### <a name="add-member-to-application"></a>Lid toevoegen aan toepassing

1. Selecteer de tegel lid om een lijst van de huidige leden weer te geven.
2. Selecteer **leden toevoegen**.

    ![Leden toevoegen](./media/manage-users/application-add-members.png)

3. Zoeken naar de naam van de gebruiker.  Alleen de Azure AD-gebruikers die zijn opgenomen in de tenant Blockchain Workbench worden weergegeven. Als de gebruiker niet wordt gevonden, moet u [Azure AD-gebruikers toevoegen](#add-azure-ad-users).

    ![Leden toevoegen](./media/manage-users/find-user.png)

4. Selecteer een **rol** in de vervolgkeuzelijst.

    ![Leden van een rol selecteren](./media/manage-users/application-select-role.png)

5. Selecteer **toevoegen** om het lid met de bijbehorende functieservices toe te voegen aan de toepassing.

#### <a name="remove-member-from-application"></a>Lid uit toepassing verwijderen

1. Selecteer de tegel lid om een lijst van de huidige leden weer te geven.
2. Voor de gebruiker die u wilt verwijderen, kiest u **verwijderen** uit de vervolgkeuzelijst van de rol.

    ![Lid verwijderen](./media/manage-users/application-remove-member.png)

#### <a name="change-or-add-role"></a>Wijzig of rol toevoegen

1. Selecteer de tegel lid om een lijst van de huidige leden weer te geven.
2. Voor de gebruiker die u wilt wijzigen, klikt u op de vervolgkeuzelijst en selecteer de nieuwe rol.

    ![Rol wijzigen](./media/manage-users/application-change-role.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel met instructies, hebt u geleerd over het beheren van gebruikers voor Azure Blockchain Workbench. Ga door naar het volgende artikel voor meer informatie over het maken van een blockchain-toepassing.

> [!div class="nextstepaction"]
> [Een blockchain-toepassing maken in Azure Blockchain Workbench](create-app.md)
