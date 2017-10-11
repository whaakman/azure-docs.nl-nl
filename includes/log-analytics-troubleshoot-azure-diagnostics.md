### <a name="troubleshoot-azure-diagnostics"></a>Problemen oplossen met Azure Diagnostics

Als u het volgende foutbericht ontvangt, is de resourceprovider Microsoft.insights niet geregistreerd:

`Failed to update diagnostics for 'resource'. {"code":"Forbidden","message":"Please register the subscription 'subscription id' with Microsoft.Insights."}`

Voer de volgende stappen in Azure Portal uit om de resourceprovider te registreren:

1.  Klik in het navigatiedeelvenster aan de linkerkant op *Abonnementen*
2.  Selecteer het abonnement dat wordt vermeld in het foutbericht
3.  Klik op *Resourceproviders*
4.  Ga naar de *Microsoft.insights*-provider
5.  Klik op de koppeling *Registreren*

![Registreer de resourceprovider Microsoft.insights](./media/log-analytics-troubleshoot-azure-diagnostics/log-analytics-register-microsoft-diagnostics-resource-provider.png)

Nadat de *Microsoft.insights* resourceprovider is geregistreerd, configureert u Diagnostics opnieuw.


Als u het volgende foutbericht ontvangt in PowerShell moet u werk uw versie van PowerShell:

`Set-AzureRmDiagnosticSetting : A parameter cannot be found that matches parameter name 'WorkspaceId'.`

Werk uw versie van PowerShell naar de November 2016 (v2.3.0) of hoger, vrijgeven van de instructies in de [aan de slag met Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) artikel.
