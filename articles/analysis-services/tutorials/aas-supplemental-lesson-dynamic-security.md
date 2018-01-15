---
title: 'Azure Analysis Services-zelfstudie - Aanvullende les: Dynamische beveiliging | Microsoft Docs'
description: In deze les wordt beschreven hoe u dynamische beveiliging kunt toepassen door rijfilters te gebruiken in de zelfstudie over Azure Analysis Services.
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 01/08/2018
ms.author: owend
ms.openlocfilehash: 7512af39041090db22d4ef45ec46925c14c4adb9
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="supplemental-lesson---dynamic-security"></a>Aanvullende les: Dynamische beveiliging

In deze aanvullende les gaat u een extra rol maken om dynamische beveiliging te implementeren. Dynamische beveiliging maakt beveiliging op rijniveau mogelijk op basis van de gebruikersnaam of aanmeldings-id van de momenteel aangemelde gebruiker. 
  
Als u dynamische beveiliging wilt implementeren, voegt u een tabel toe aan uw model met daarin de gebruikersnamen van gebruikers die verbinding mogen maken met het model en die objecten en gegevens in het model mogen raadplegen. Het model dat u maakt met behulp van deze zelfstudie is in de context van Adventure Works. Om deze les te voltooien, moet u echter een tabel toevoegen met gebruikers uit uw eigen domein. U hoeft niet de wachtwoorden te weten voor de gebruikersnamen die worden toegevoegd. U gaat een tabel EmployeeSecurity maken, met daarin een klein aantal gebruikers uit uw eigen domein, door werknemersgegevens te kopiëren uit een Excel-spreadsheet. In een praktijkscenario is de tabel met gebruikersnamen meestal een tabel uit een gegevensbron zoals een live database, bijvoorbeeld een tabel DimEmployee.  
  
Voor het implementeren van dynamische beveiliging gaat u twee DAX-functies gebruiken: [USERNAME](http://msdn.microsoft.com/22dddc4b-1648-4c89-8c93-f1151162b93f) en [LOOKUPVALUE](http://msdn.microsoft.com/73a51c4d-131c-4c33-a139-b1342d10caab). Deze functies, toegepast in een formule voor een rijfilter, worden gedefinieerd in een nieuwe rol. Met behulp van de functie LOOKUPVALUE wordt in de formule een waarde opgehaald uit de tabel EmployeeSecurity. De formule geeft deze waarde vervolgens door aan de functie USERNAME. Deze functie geeft de gebruikersnaam aan van de aangemelde gebruiker die deel uitmaakt van deze rol. De gebruiker kan vervolgens alleen gegevens raadplegen die voldoen aan de rijfilters van de rol. In dit scenario gaat u instellen dat verkoopmedewerkers alleen gegevens van internetverkopen kunnen inzien voor de verkoopregio waarvan ze lid zijn.  
  
De taken die uniek zijn voor dit scenario van het tabellaire model Adventure Works, maar die niet noodzakelijkerwijs van toepassing zijn op een praktijkscenario, worden als zodanig aangegeven. Elke taak bevat aanvullende informatie over het doel van de taak.  
  
Geschatte tijd voor het voltooien van deze les: **30 minuten**  
  
## <a name="prerequisites"></a>Vereisten  
Deze aanvullende les maakt deel uit van een zelfstudie over het ontwerpen van een tabellair model. De lessen van de zelfstudie moeten op volgorde worden uitgevoerd. Voordat u de taken in deze les gaat uitvoeren, moet u alle vorige lessen hebben voltooid.  
  
## <a name="add-the-dimsalesterritory-table-to-the-aw-internet-sales-tabular-model-project"></a>De tabel DimSalesTerritory toevoegen aan het tabellaire project AW Internet Sales  
Als u dynamische beveiliging wilt implementeren voor dit Adventure Works-scenario, moet u twee aanvullende tabellen toevoegen aan uw model. De eerste tabel die u toevoegt, is DimSalesTerritory (als Sales Territorium) uit dezelfde database met AdventureWorksDW. U gaat later een rijfilter toepassen op de tabel SalesTerritory om de specifieke gegevens te definiëren die de aangemelde gebruiker kan raadplegen.  
  
#### <a name="to-add-the-dimsalesterritory-table"></a>De tabel DimSalesTerritory toevoegen:  
  
1.  Klik in Tabular Model Explorer > **Data Sources** met de rechtermuisknop op uw verbinding en klik vervolgens op **Import New Tables**.  

    Als het dialoogvenster Impersonation Credentials wordt weergegeven, typt u de referenties die u hebt gebruikt in Les 2: Gegevens toevoegen.
  
2.  Selecteer in Navigator de tabel **DimSalesTerritory** en klik vervolgens op **OK**.    
  
3.  Klik in Query-editor op de query **DimSalesTerritory** en verwijder vervolgens de kolom **SalesTerritoryAlternateKey**.  
  
7.  Klik op **Import**.  
  
    De nieuwe tabel wordt toegevoegd aan de modelwerkruimte. Objecten en gegevens uit de brontabel DimSalesTerritory worden vervolgens geïmporteerd in het tabellaire model AW Internet Sales.  
  
9. Als de tabel is geïmporteerd, klikt u op **Close**.  

## <a name="add-a-table-with-user-name-data"></a>Een tabel toevoegen met gegevens van gebruikersnamen  
De tabel DimEmployee in de voorbeelddatabase AdventureWorksDW bevat gebruikers uit het domein AdventureWorks. Deze gebruikersnamen bestaan niet in uw eigen omgeving. U moet daarom een tabel maken in het model met een klein aantal (ten minste drie) werkelijke gebruikers uit uw organisatie. Deze gebruikers voegt u vervolgens als leden toe aan de nieuwe rol. U hoeft niet de wachtwoorden voor deze gebruikersnamen niet te weten, maar u moet wel bestaande Windows-gebruikersnamen uit uw eigen domein opgeven.  
  
#### <a name="to-add-an-employeesecurity-table"></a>Een tabel EmployeeSecurity toevoegen:  
  
1.  Open Microsoft Excel en maak een werkblad.  
  
2.  Kopieer de volgende tabel, met inbegrip van de rij met kolomkoppen, en plak deze in het werkblad.  

    ```
      |EmployeeId|SalesTerritoryId|FirstName|LastName|LoginId|  
      |---------------|----------------------|--------------|-------------|------------|  
      |1|2|<user first name>|<user last name>|\<domain\username>|  
      |1|3|<user first name>|<user last name>|\<domain\username>|  
      |2|4|<user first name>|<user last name>|\<domain\username>|  
      |3|5|<user first name>|<user last name>|\<domain\username>|  
    ```

3.  Vervang de velden met de voornaam, de achternaam en domein\gebruikersnaam door de namen en aanmeldings-id's van drie gebruikers in uw organisatie. Plaats dezelfde gebruiker in de eerste twee rijen, bijvoorbeeld EmployeeId 1, om aan te geven dat deze gebruiker meer dan één verkoopregio heeft. Laat de velden EmployeeId en SalesTerritoryId ongewijzigd.  
  
4.  Sla het werkblad op als **SampleEmployee**.  
  
5.  Selecteer in het werkblad alle cellen met gegevens van werknemers, met inbegrip van de koppen, klik vervolgens met de rechtermuisknop op de geselecteerde gegevens en klik vervolgens op **Copy**.  
  
6.  Klik in SSDT op het menu **Edit** en klik vervolgens op **Paste**.  
  
    Als Paste lichter gekleurd wordt weergegeven, klikt u op een kolom in een tabel in het venster van de ontwerpfunctie en probeert u het opnieuw.  
  
7.  Typ **EmployeeSecurity** voor **Table Name** in het dialoogvenster **Paste Preview**.  
  
8.  Controleer of bij **Data to be pasted** alle gebruikersgegevens en koppen uit het werkblad SampleEmployee worden weergegeven.  
  
9. Controleer of **Use first row as column headers** is ingeschakeld en klik vervolgens op **OK**.  
  
    Er wordt een nieuwe tabel gemaakt met de naam EmployeeSecurity, met daarin werknemersgegevens die zijn gekopieerd uit het werkblad SampleEmployee.  
  
## <a name="create-relationships-between-factinternetsales-dimgeography-and-dimsalesterritory-table"></a>Relaties maken tussen de tabellen FactInternetSales, DimGeography en DimSalesTerritory  
De tabellen FactInternetSales, DimGeography en DimSalesTerritory hebben een gemeenschappelijke kolom, namelijk SalesTerritoryId. De kolom SalesTerritoryId in de tabel DimSalesTerritory bevat waarden met een andere id voor elke verkoopregio.  
  
#### <a name="to-create-relationships-between-the-factinternetsales-dimgeography-and-the-dimsalesterritory-table"></a>Relaties maken tussen de tabellen FactInternetSales, DimGeography en DimSalesTerritory:  
  
1.  Zorg dat de diagramweergave actief is en sleep vervolgens in de tabel **DimGeography** de kolom **SalesTerritoryId** naar de kolom **SalesTerritoryId** in de tabel **DimSalesTerritory**.  
  
2.  Sleep in de tabel **FactInternetSales** de kolom **SalesTerritoryId** naar de kolom **SalesTerritoryId** in de tabel **DimSalesTerritory**.  
  
    De eigenschap Active voor deze relatie staat op False, wat betekent dat de relatie niet actief is. De tabel FactInternetSales heeft al een andere actieve relatie.  
  
## <a name="hide-the-employeesecurity-table-from-client-applications"></a>De tabel EmployeeSecurity verbergen in clienttoepassingen  
In deze taak gaat u de tabel EmployeeSecurity verbergen, zodat deze niet wordt weergegeven in de lijst met velden in een clienttoepassing. Als u een tabel verbergt, betekent dit niet automatisch dat de tabel is beveiligd. Gebruikers kunnen nog steeds gegevens uit de tabel EmployeeSecurity opvragen als ze weten hoe dat moet. Als u de gegevens in de tabel EmployeeSecurity wilt beveiligen, om te voorkomen dat gebruikers via een query gegevens kunnen opvragen, moet u in een latere taak een filter toepassen.  
  
#### <a name="to-hide-the-employeesecurity-table-from-client-applications"></a>De tabel EmployeeSecurity verbergen in clienttoepassingen:  
  
-   Klik in de diagramweergave van de ontwerpfunctie voor modellen met de rechtermuisknop op de tabelkop **Employee** en klik vervolgens op **Hide from Client Tools**.  
  
## <a name="create-a-sales-employees-by-territory-user-role"></a>Een gebruikersrol Sales Employees by Territory maken  
In deze taak gaat u een gebruikersrol maken. Deze taak omvat het definiëren van een rijfilter waarmee wordt bepaald welke rijen van de tabel DimSalesTerritory zichtbaar zijn voor gebruikers. Het filter wordt vervolgens in de richting een-op-veel toegepast op alle andere tabellen die zijn gerelateerd aan DimSalesTerritory. U gaat ook een filter toepassen waarmee u de hele tabel EmployeeSecurity beveiligt tegen query's door gebruikers die geen lid zijn van de rol.  
  
> [!NOTE]  
> De rol Sales Employees by Territory die u in deze les maakt, zorgt ervoor dat leden van de rol alleen verkoopgegevens voor hun verkoopregio kunnen raadplegen (of opvragen). Als u een gebruiker als lid toevoegt aan de rol Sales Employees by Territory die ook beschikt over een rol die is gemaakt in [Les 11: Rollen maken](../tutorials/aas-lesson-11-create-roles.md), ontstaat er een combinatie van machtigingen. Wanneer een gebruiker meerdere rollen heeft, worden de machtigingen, en de rijfilters die voor elke rol zijn gedefinieerd, gecombineerd. De gebruiker beschikt dan over de hoogste machtigingen die het resultaat zijn van de combinatie van rollen.  
  
#### <a name="to-create-a-sales-employees-by-territory-user-role"></a>Een gebruikersrol Sales Employees by Territory maken:  
  
1.  Klik in SSDT op het menu **Model** en klik vervolgens op **Roles**.  
  
2.  Klik in **Role Manager** op **New**.  
  
    Er wordt een nieuwe rol met de machtiging None toegevoegd aan de lijst.  
  
3.  Klik op de nieuwe rol en wijzig de naam van de rol vervolgens in de kolom **Name** in **Sales Employees by Territory**.  
  
4.  Klik op de vervolgkeuzelijst in de kolom **Machtigingen** en selecteer vervolgens de machtiging **Lezen**.  
  
5.  Klik op het tabblad **Members** en klik vervolgens op **Add**.  
  
6.  Ga in het dialoogvenster **Select User or Group** naar **Enter the object named to select** en typ het eerste voorbeeld van een gebruikersnaam dat u hebt gebruikt bij het maken van de tabel EmployeeSecurity. Klik op **Check Names** om te controleren of de gebruikersnaam geldig is en klik vervolgens op **OK**.  
  
    Herhaal deze stap om de andere voorbeelden van gebruikersnamen toe te voegen die u hebt gebruikt bij het maken van de tabel EmployeeSecurity.  
  
7.  Klik op het tabblad **Row Filters**.  
  
8.  Typ voor de tabel **EmployeeSecurity** de volgende formule in de kolom **DAX Filter**:  
  
    ```
      =FALSE()  
    ```
  
    Deze formule geeft aan dat alle kolommen voldoen aan de Boole-voorwaarde False. Dit betekent dat geen enkele kolom van de tabel EmployeeSecurity kan worden geraadpleegd door een lid van de gebruikersrol Sales Employees by Territory.  
  
9. Typ de volgende formule voor de tabel **DimSalesTerritory**:  

    ```  
    ='Sales Territory'[Sales Territory Id]=LOOKUPVALUE('Employee Security'[Sales Territory Id], 
      'Employee Security'[Login Id], USERNAME(), 
      'Employee Security'[Sales Territory Id], 
      'Sales Territory'[Sales Territory Id]) 
    ```
  
    In deze formule retourneert de functie LOOKUPVALUE alle waarden voor de kolom DimEmployeeSecurity [SalesTerritoryId] waarvoor de EmployeeSecurity [LoginId] gelijk is aan de momenteel aangemelde Windows-gebruikersnaam en EmployeeSecurity [SalesTerritoryId] overeenkomt met DimSalesTerritory [SalesTerritoryId].  
  
    De reeks met verkoopregio-id's die wordt geretourneerd door LOOKUPVALUE, wordt vervolgens gebruikt om de rijen te beperken die worden weergegeven in de tabel DimSalesTerritory. Alleen rijen waarvan de SalesTerritoryID voor de rij aanwezig is in de reeks met id's die is geretourneerd door de functie LOOKUPVALUE worden weergegeven.  
  
10. Klik in Role Manager op **OK**.  
  
## <a name="test-the-sales-employees-by-territory-user-role"></a>De gebruikersrol Sales Employees by Territory testen  
In deze taak gebruikt u de functie Analyse in Excel van SSDT om de effectiviteit van de gebruikersrol Sales Employees by Territory te testen. U gaat een van de gebruikersnamen opgeven die u hebt toegevoegd aan de tabel EmployeeSecurity en als een lid van de rol. Deze gebruikersnaam wordt vervolgens daadwerkelijk gebruikt om verbinding te maken tussen Excel en het model.  
  
#### <a name="to-test-the-sales-employees-by-territory-user-role"></a>De gebruikersrol Sales Employees by Territory testen:  
  
1.  Klik in SSDT op het menu **Model** en klik vervolgens op **Analyze in Excel**.  
  
2.  Ga in het dialoogvenster **Analyze in Excel** naar **Specify the user name or role to use to connect to the model**, selecteer **Other Windows User** en klik op **Browse**.  
  
3.  Ga in het dialoogvenster **Select User or Group** naar **Enter the object named to select**, typ een gebruikersnaam die u hebt opgenomen in de tabel EmployeeSecurity en klik vervolgens op **Check Names**.  
  
4.  Klik op **OK** om het dialoogvenster **Select User or Group** te sluiten en klik vervolgens op **OK** om het dialoogvenster **Analyze in Excel** te sluiten.  
  
    Excel wordt geopend met een nieuwe werkmap. Er wordt automatisch een draaitabel gemaakt. De lijst met draaitabelvelden bevat vrijwel alle gegevensvelden die beschikbaar zijn in het nieuwe model.  
  
    De tabel EmployeeSecurity wordt niet vermeld in de lijst met draaitabellen. U hebt deze tabel namelijk in een eerdere taak verborgen voor weergave in clienthulpprogramma's.  
  
5.  Selecteer in de lijst **Fields**, in **∑ Internet Sales** (measures) de meting **InternetTotalSales**. De meting wordt ingevoerd in de velden **Values**.  
  
6.  Selecteer de kolom **SalesTerritoryId** uit de tabel **DimSalesTerritory**. De kolom wordt ingevoerd in de velden **Row Labels**.  
  
    U ziet dat er alleen cijfers van internetverkopen worden weergegeven voor de regio van de actieve gebruikersnaam. Als u een andere kolom selecteert als Row Label-veld, zoals City uit de tabel DimGeography, worden alleen plaatsen weergegeven uit de verkoopregio van de actieve gebruiker.  
  
    Deze gebruiker kan dan geen gegevens van internetverkopen inzien of opvragen voor andere verkoopregio's. Deze beperking is van toepassing omdat het rijfilter dat is gedefinieerd voor de tabel DimSalesTerritory, voor de gebruikersrol Sales Employees by Territory, gegevens beveiligt voor alle gegevens die betrekking hebben op andere verkoopregio's.  
  
## <a name="see-also"></a>Zie ook  
[USERNAME, functie (DAX)](https://msdn.microsoft.com/library/hh230954.aspx)  
[LOOKUPVALUE, functie (DAX)](https://msdn.microsoft.com/library/gg492170.aspx)  
[CUSTOMDATA, functie (DAX)](https://msdn.microsoft.com/library/hh213140.aspx)  