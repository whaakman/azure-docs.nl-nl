### <a name="azure-powershell"></a>Azure PowerShell

#### <a name="install-powershell"></a>PowerShell installeren
Installeer de nieuwste versie van PowerShell als deze nog niet op uw computer staat. 

1. Navigeer in uw webbrowser naar de pagina [Azure downloads](https://azure.microsoft.com/downloads/). 
2. Klik op **Windows installeren** in de sectie **Opdrachtregelprogramma's** -> **PowerShell**. 
3. Voer het **MSI**-bestand uit om PowerShell te installeren. 

Zie [Azure PowerShell installeren en configureren](/powershell/azure/install-azurerm-ps) voor gedetailleerde instructies. 

#### <a name="log-in-to-powershell"></a>Aanmelden bij PowerShell

1. Start **PowerShell** op uw computer. Houd PowerShell geopend tot het einde van deze QuickStart. Als u het programma sluit en opnieuw opent, moet u deze opdrachten opnieuw uitvoeren.

    ![PowerShell starten](media/data-factory-quickstart-prerequisites-2/search-powershell.png)
1. Voer de volgende opdracht uit en geef de gebruikersnaam en het wachtwoord op waarmee u zich aanmeldt bij Azure Portal:
       
    ```powershell
    Login-AzureRmAccount
    ```        
2. Voer de volgende opdracht uit om alle abonnementen voor dit account weer te geven:

    ```powershell
    Get-AzureRmSubscription
    ```
3. Als u meerdere abonnementen hebt, voert u de volgende opdracht uit om het abonnement te selecteren waarmee u wilt werken. Vervang **SubscriptionId** door de id van uw Azure-abonnement:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```
