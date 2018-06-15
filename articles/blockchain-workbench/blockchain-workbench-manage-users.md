---
title: Gebruikers in Azure Blockchain Workbench beheren
description: Het beheren van gebruikers in Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/26/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 80f20e438b1d923e688aeef9e6b353642bd74c27
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
ms.locfileid: "34158932"
---
# <a name="manage-users-in-azure-blockchain-workbench"></a>Gebruikers in Azure Blockchain Workbench beheren

Azure Blockchain Workbench bevat Gebruikersbeheer voor personen en organisaties die deel van uw consortium uitmaken.

## <a name="prerequisites"></a>Vereisten

Een implementatie-Blockchain Workbench is vereist. Zie [Azure Blockchain Workbench-implementatie](blockchain-workbench-deploy.md) voor meer informatie over de implementatie.

## <a name="add-azure-ad-users"></a>Azure AD-gebruikers toevoegen

De Azure Blockchain Workbench gebruikt Azure Active Directory (Azure AD) voor verificatie, toegangsbeheer en rollen. Gebruikers in de Workbench Blockchain Azure AD-tenant kunnen verifiëren en Blockchain Workbench gebruiken. Gebruikers toevoegen aan de rol beheerder toepassing om te communiceren en acties uitvoeren.

Blockchain Workbench gebruikers moeten bestaan in de Azure AD-tenant voordat u deze aan toepassingen en functies toewijzen kunt. Als gebruikers wilt toevoegen aan Azure AD, gebruikt u de volgende stappen uit:

1.  Meld u aan bij [Azure Portal](https://portal.azure.com).
2.  Selecteer uw account in de rechterbovenhoek en Ga naar de Azure AD-tenant die is gekoppeld aan Blockchain Workbench.
3.  Selecteer **Azure Active Directory > gebruikers**. U ziet een lijst met gebruikers in uw directory.
4.  Selecteer om gebruikers toe aan de directory **nieuwe gebruiker**. Selecteer voor externe gebruikers **nieuwe gastgebruiker**.

    ![Nieuwe gebruiker](media/blockchain-workbench-manage-users/add-ad-user.png)

5.  Vul de vereiste velden voor de nieuwe gebruiker. Selecteer **Maken**.

Ga naar [Azure AD](../active-directory/add-users-azure-active-directory.md) documentatie voor meer informatie over het beheren van gebruikers in Azure AD.

## <a name="manage-blockchain-workbench-administrators"></a>Blockchain Workbench beheerders beheren

Nadat gebruikers zijn toegevoegd aan de directory, wordt de volgende stap is om te kiezen Blockchain Workbench beheerders zijn. Gebruikers in de **beheerder** groep zijn gekoppeld aan de **toepassing beheerdersrol** in Blockchain Workbench. Beheerders kunnen toevoegen of verwijderen van gebruikers, gebruikers toewijzen aan specifieke scenario's en maak nieuwe toepassingen.

Gebruikers toevoegen aan de **beheerder** groep in de Azure AD-map:

1.  Meld u aan bij [Azure Portal](https://portal.azure.com).
2.  Controleer of dat u zich in de Azure AD-tenant die is gekoppeld aan Blockchain Workbench door uw account selecteren in de rechterbovenhoek.
3.  Selecteer **Azure Active Directory > bedrijfstoepassingen**.
4.  Selecteer de Azure AD-clienttoepassing voor Blockchain Workbench
    
    ![Alle registraties voor enterprise-toepassing](media/blockchain-workbench-manage-users/select-blockchain-client-app.png)

5.  Selecteer **gebruikers en groepen > gebruiker toevoegen**.
6.  In **toevoegen toewijzing**, selecteer **gebruikers**. Kies of zoek naar de gebruiker die u wilt toevoegen als beheerder. Klik op **Selecteer** na voltooiing kiezen.

    ![Toewijzing toevoegen](media/blockchain-workbench-manage-users/add-user-assignment.png)

9.  Controleer of **rol** is ingesteld op **beheerder**
10. Selecteer **Toewijzen**. De toegevoegde gebruikers worden weergegeven in de lijst met de beheerdersrol is toegewezen.

    ![Blockchain client app-gebruikers](media/blockchain-workbench-manage-users/blockchain-admin-list.png)

## <a name="managing-blockchain-workbench-members"></a>Het beheren van Blockchain Workbench leden

De toepassing Blockchain Workbench gebruiken voor het beheren van gebruikers en organisaties die deel van uw consortium uitmaken. U kunt toevoegen of verwijderen van gebruikers tot toepassingen en functies.

1. [Open de Blockchain Workbench](blockchain-workbench-deploy.md#blockchain-workbench-web-url) in uw browser en meld u aan als beheerder.

    ![Blockchain Workbench](media/blockchain-workbench-manage-users/blockchain-workbench-applications.png)

    Leden worden toegevoegd aan elke toepassing. Leden kunnen een of meer toepassingsrollen te initiëren opdrachten of acties ondernemen hebben.

2. Voor het beheren van leden voor een toepassing, selecteer de tegel van een toepassing in de **toepassingen** deelvenster.

    Het aantal leden die zijn gekoppeld aan de geselecteerde toepassing wordt weergegeven in de tegel leden.

    ![Toepassing selecteren](media/blockchain-workbench-manage-users/blockchain-workbench-select-application.png)


#### <a name="add-member-to-application"></a>Lid toevoegen aan de toepassing

1. Selecteer de tegel lid een lijst van de huidige leden weergeven.
2. Selecteer **leden toevoegen**.

    ![Leden toevoegen](media/blockchain-workbench-manage-users/application-add-members.png)

3. Zoek de naam van de gebruiker.  Alleen worden Azure AD-gebruikers die zijn opgenomen in de tenant Blockchain Workbench weergegeven. Als de gebruiker niet wordt gevonden, moet u [Azure AD-gebruikers toevoegen](#add-azure-ad-users).

    ![Leden toevoegen](media/blockchain-workbench-manage-users/find-user.png)

4. Selecteer een **rol** in de vervolgkeuzelijst.

    ![Leden van een rol selecteren](media/blockchain-workbench-manage-users/application-select-role.png)

5. Selecteer **toevoegen** het lid met de bijbehorende functieservices toevoegen aan de toepassing.

#### <a name="remove-member-from-application"></a>Lid van toepassing verwijderen

1. Selecteer de tegel lid een lijst van de huidige leden weergeven.
2. Voor de gebruiker die u wilt verwijderen, kiest u **verwijderen** uit de vervolgkeuzelijst van de rol.

    ![Lid verwijderen](media/blockchain-workbench-manage-users/application-remove-member.png)

#### <a name="change-or-add-role"></a>Wijzig of functie toevoegen

1. Selecteer de tegel lid een lijst van de huidige leden weergeven.
2. Voor de gebruiker die u wilt wijzigen, klikt u op de vervolgkeuzelijst en selecteer de nieuwe rol.

    ![Rol wijzigen](media/blockchain-workbench-manage-users/application-change-role.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel instructies hebt u geleerd hoe gebruikers beheren voor Azure Blockchain Workbench. Meer informatie over het maken van een toepassing blockchain, blijven de volgende procedures voor artikel.

> [!div class="nextstepaction"]
> [Een toepassing blockchain maken in Azure Blockchain Workbench](blockchain-workbench-create-app.md)
