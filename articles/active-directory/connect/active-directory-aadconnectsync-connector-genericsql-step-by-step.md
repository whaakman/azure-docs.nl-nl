---
title: Algemene SQL-Connector stap voor stap | Microsoft Docs
description: In dit artikel wordt u stap voor stap een eenvoudig HR-systeem stapsgewijze met behulp van de algemene SQL-Connector.
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 28c1cc60-24fd-4d0d-a36d-b4aba6de86e7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 3fdc1b405b95180d031aa4ad45b406f7fc149d8f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="generic-sql-connector-step-by-step"></a>Algemene SQL Connector, stap voor stap
In dit onderwerp is een stapsgewijze handleiding. Deze maakt een eenvoudig voorbeeld HR-database en deze gebruiken voor het importeren van sommige gebruikers en hun groepslidmaatschap.

## <a name="prepare-the-sample-database"></a>De voorbeelddatabase voorbereiden
Op een server met SQL Server, voert de SQL-script gevonden in [bijlage A](#appendix-a). Dit script maakt een voorbeelddatabase met de naam GSQLDEMO. Het objectmodel voor de database is gemaakt, ziet deze afbeelding:  
![Objectmodel](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/objectmodel.png)

Ook een gebruiker die u wilt gebruiken om verbinding met de database te maken. In dit scenario wordt de gebruiker FABRIKAM\SQLUser aangeroepen en zich in het domein.

## <a name="create-the-odbc-connection-file"></a>Maken van het ODBC-verbindingsbestand
De algemene SQL-Connector maakt gebruik van ODBC verbinding maken met de externe server. Eerst moet een bestand met de gegevens voor de ODBC-verbinding maken.

1. Start het ODBC-management-hulpprogramma op uw server:  
   ![ODBC](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc.png)
2. Selecteer het tabblad **DSN-bestand**. Klik op **toevoegen...** .  
   ![ODBC1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc1.png)
3. De out-of-box stuurprogramma werkt fijn, dus te selecteren en op **volgende >**.  
   ![ODBC2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc2.png)
4. Geef het bestand een naam, zoals **GenericSQL**.  
   ![ODBC3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc3.png)
5. Klik op **Voltooien**.  
   ![ODBC4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc4.png)
6. Tijd om de verbinding te configureren. Geef de gegevensbron een goede beschrijving en geef de naam van de server met SQL Server.  
   ![ODBC5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc5.png)
7. Selecteer hoe u verifieert met behulp van SQL. In dit geval gebruiken we Windows-verificatie.  
   ![ODBC6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc6.png)
8. Geef de naam van de voorbeelddatabase **GSQLDEMO**.  
   ![ODBC7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc7.png)
9. Houd alles op dit scherm standaard. Klik op **Voltooien**.  
   ![ODBC8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc8.png)
10. Om te controleren of alles werkt zoals verwacht, klikt u op **gegevensbron testen**.  
    ![ODBC9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc9.png)
11. Zorg ervoor dat de test is geslaagd.  
    ![ODBC10](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc10.png)
12. Het ODBC-configuratiebestand worden nu zichtbaar in de DSN-bestand.  
    ![ODBC11](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc11.png)

We hebben nu het bestand we nodig en u kunt beginnen met het maken van de Connector.

## <a name="create-the-generic-sql-connector"></a>De algemene SQL-Connector maken
1. Selecteer in de UI Synchronization Service Manager **Connectors** en **maken**. Selecteer **algemene SQL (Microsoft)** en wijs hieraan een beschrijvende naam.  
   ![Connector1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector1.png)
2. Zoek de DSN-bestand dat u in de vorige sectie hebt gemaakt en dit uploaden naar de server. Geef de referenties voor verbinding met de database.  
   ![Connector2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector2.png)
3. In dit overzicht we zijn zodat u gemakkelijk voor ons en er zijn twee objecttypen spreken **gebruiker** en **groep**.
   ![Connector3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector3.png)
4. Ga voor de kenmerken, willen we de Connector voor het detecteren van deze kenmerken door te kijken naar de tabel zelf. Aangezien **gebruikers** is een gereserveerd woord in SQL, moeten we opgeven in vierkante haken [].  
   ![Connector4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector4.png)
5. Tijd om het ankerkenmerk en het kenmerk DN te definiëren. Voor **gebruikers**, gebruiken we de combinatie van de twee kenmerken gebruikersnaam en het werknemer-id. Voor **groep**, gebruiken we GroupName (geen realistisch in de praktijk, maar voor dit scenario werkt).
   ![Connector5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector5.png)
6. Niet alle kenmerktypen kunnen worden gedetecteerd in een SQL-database. Kan met name het kenmerk referentietype niet. Voor het objecttype van de groep moeten we de OwnerID en MemberID om te verwijzen naar wijzigen.  
   ![Connector6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector6.png)
7. De kenmerken die we geselecteerd als verwijzingskenmerken in de vorige stap is vereist voor het objecttype deze waarden zijn een verwijzing naar. In ons geval typt u het gebruikersobject.  
   ![Connector7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector7.png)
8. Selecteer op de pagina Algemene Parameters **watermerk** als de delta-strategie. Ook in de datum-/ tijdnotatie typen **jjjj-MM-dd: mm: SS**.
   ![Connector8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector8.png)
9. Op de **configureren partities en hiërarchieën** pagina, selecteert u beide objecttypen.
   ![Connector9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector9.png)
10. Op de **objecttypen selecteren** en **kenmerken selecteren**, objecttypen en alle kenmerken selecteren. Op de **ankers configureren** pagina, klikt u op **voltooien**.

## <a name="create-run-profiles"></a>Uitvoeringsprofielen maken
1. Selecteer in de UI Synchronization Service Manager **Connectors**, en **Uitvoeringsprofielen configureren**. Klik op **nieuw profiel**. We beginnen met **volledige Import**.  
   ![Runprofile1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile1.png)
2. Selecteer het type **volledige Import (alleen Faseren)**.  
   ![Runprofile2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile2.png)
3. Selecteer de partitie **OBJECT = gebruiker**.  
   ![Runprofile3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile3.png)
4. Selecteer **tabel** en het type **[gebruikers]**. Schuif helemaal naar het gedeelte van het type object met meerdere waarden en voer de gegevens in de volgende afbeelding. Selecteer **voltooien** om op te slaan van de stap.  
   ![Runprofile4a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4a.png)  
   ![Runprofile4b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4b.png)  
5. Selecteer **nieuwe stap**. Selecteer deze keer **OBJECT groep =**. Gebruik de configuratie zoals in de volgende afbeelding op de laatste pagina. Klik op **Voltooien**.  
   ![Runprofile5a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5a.png)  
   ![Runprofile5b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5b.png)  
6. Optioneel: Als u wilt, kunt u extra profielen uitvoeren. Voor dit scenario wordt alleen de volledige Import gebruikt.
7. Klik op **OK** voltooid uitvoeringsprofielen wijzigen.

## <a name="add-some-test-data-and-test-the-import"></a>Sommige gegevens testen en testen van de invoer toevoegen
Voer enkele testgegevens in de voorbeelddatabase. Wanneer u klaar bent, selecteert u **uitvoeren** en **volledige import**.

Hier volgt een gebruiker met twee telefoonnummers en een groep met een aantal leden.  
![cs1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs1.png)  
![CS2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs2.png)  

## <a name="appendix-a"></a>Bijlage A
**SQL-script voor het maken van de voorbeelddatabase**

```SQL
---Creating the Database---------
Create Database GSQLDEMO
Go
-------Using the Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```
