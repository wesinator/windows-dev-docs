---
title: Share certificates between Windows apps
description: Windows apps that require secure authentication beyond a user Id and password combination can use certificates for authentication.
ms.date: 09/05/2024
ms.topic: how-to
keywords: windows, winui, winrt, dotnet, security
---

# Share certificates between Windows apps

Windows apps that require secure authentication beyond a user Id and password combination can use certificates for authentication. Certificate authentication provides a high level of trust when authenticating a user. In some cases, a group of services will want to authenticate a user for multiple apps. This article shows how you can authenticate multiple apps using the same certificate, and how you can provide convenient code for a user to import a certificate that was provided to access secured web services.

Apps can authenticate to a web service using a certificate, and multiple apps can use a single certificate from the certificate store to authenticate the same user. If a certificate does not exist in the store, you can add code to your app to import a certificate from a PFX file.

## Create and publish a secured web service

1. Open Microsoft Visual Studio and select **Create a new project** from the start screen.
1. In the Create a new project dialog, select **API** in the **Select a project type** dropdown list to filter the available project templates.
1. Select the **ASP.NET Core Web API** template and select **Next**.
1. Name the application "FirstContosoBank" and select **Next**.
1. Choose **.NET 8.0** as the **Framework**, set the **Authentication type** to **None**, ensure **Configure for HTTPS** is checked, uncheck **Enable OpenAPI support**, check **Do not use top-level statements** and **Use controllers**, and select **Create**.
1. Right-click the **WeatherForecastController.cs** file in the **Controllers** folder and select **Rename**. Change the name to **BankController.cs** and let Visual Studio rename the class and all references to the class.
1. In the **launchSettings.json** file, change the value of "launchUrl" from "weatherforecast" to "bank" for all three configuration what use the value.
1. In the **BankController.cs** file, add following "Login" method.

   ```cs
   [HttpGet]
   [Route("login")]
   public string Login()
   {
       // Verify certificate with CA
       var cert = new System.Security.Cryptography.X509Certificates.X509Certificate2(
           this.Request.HttpContext.Connection.ClientCertificate);
       bool test = cert.Verify();
       return test.ToString();
   }
   ```

1. Start debugging the project to launch the web service. The web service will be available at `https://localhost:7072/bank`. You can test the web service by opening a web browser and entering the web address. You will see the generated weather forecast data formatted as JSON. Keep the web service running while you create the client app.

## Create a WinUI app that uses certificate authentication

Now that you have one or more secured web services, your apps can use certificates to authenticate to those web services. When you make a request to an authenticated web service using the [HttpClient](/uwp/api/Windows.Web.Http.HttpClient) object, the initial request will not contain a client certificate. The authenticated web service will respond with a request for client authentication. When this occurs, the Windows client will automatically query the certificate store for available client certificates. Your user can select from these certificates to authenticate to the web service. Some certificates are password protected, so you will need to provide the user with a way to input the password for a certificate.

If there are no client certificates available, then the user will need to add a certificate to the certificate store. You can include code in your app that enables a user to select a PFX file that contains a client certificate and then import that certificate into the client certificate store.

> [!TIP]
> You can use the PowerShell cmdlets **New-SelfSignedCertificate** and **Export-PfxCertificate** to create a self-signed certificate and export it to a PFX file to use with this quickstart. For information, see [New-SelfSignedCertificate](/powershell/module/pki/new-selfsignedcertificate) and [Export-PfxCertificate](/powershell/module/pki/export-pfxcertificate).

1. Open Visual Studio and create a new WinUI project from the start page. Name the new project "FirstContosoBankApp". Click **Create** to create the new project.
1. In the MainWindow.xaml file, add the following XAML to a **Grid** element, replacing the existing **StackPanel** element and its contents. This XAML includes a button to browse for a PFX file to import, a text box to enter a password for a password-protected PFX file, a button to import a selected PFX file, a button to log in to the secured web service, and a text block to display the status of the current action.

   ```xml
   <Button x:Name="Import" Content="Import Certificate (PFX file)" HorizontalAlignment="Left" Margin="352,305,0,0" VerticalAlignment="Top" Height="77" Width="260" Click="Import_Click" FontSize="16"/>
   <Button x:Name="Login" Content="Login" HorizontalAlignment="Left" Margin="611,305,0,0" VerticalAlignment="Top" Height="75" Width="240" Click="Login_Click" FontSize="16"/>
   <TextBlock x:Name="Result" HorizontalAlignment="Left" Margin="355,398,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Height="153" Width="560"/>
   <PasswordBox x:Name="PfxPassword" HorizontalAlignment="Left" Margin="483,271,0,0" VerticalAlignment="Top" Width="229"/>
   <TextBlock HorizontalAlignment="Left" Margin="355,271,0,0" TextWrapping="Wrap" Text="PFX password" VerticalAlignment="Top" FontSize="18" Height="32" Width="123"/>
   <Button x:Name="Browse" Content="Browse for PFX file" HorizontalAlignment="Left" Margin="352,189,0,0" VerticalAlignment="Top" Click="Browse_Click" Width="499" Height="68" FontSize="16"/>
   <TextBlock HorizontalAlignment="Left" Margin="717,271,0,0" TextWrapping="Wrap" Text="(Optional)" VerticalAlignment="Top" Height="32" Width="83" FontSize="16"/>
   ```

1. Save the MainWindow.xaml file.
1. In the MainWindow.xaml.cs file, add the following using statements.

   ```cs
   using Windows.Web.Http;
   using System.Text;
   using Windows.Security.Cryptography.Certificates;
   using Windows.Storage.Pickers;
   using Windows.Storage;
   using Windows.Storage.Streams;
   ```

1. In the MainWindow.xaml.cs file, add the following variables to the **MainWindow** class. They specify the address for the secured "Login" method of your "FirstContosoBank" web service, and a global variable that holds a PFX certificate to import into the certificate store. Update the `<server-name>` to `localhost:7072` or whichever port is specified in the "https" configuration in your API project's launchSettings.json file.

   ```cs
   private Uri requestUri = new Uri("https://<server-name>/bank/login");
   private string pfxCert = null;
   ```

1. In the MainWindow.xaml.cs file, add the following click handler for the login button and method to access the secured web service.

   ```cs
   private void Login_Click(object sender, RoutedEventArgs e)
   {
       MakeHttpsCall();
   }

   private async void MakeHttpsCall()
   {

       StringBuilder result = new StringBuilder("Login ");
       HttpResponseMessage response;
       try
       {
           Windows.Web.Http.HttpClient httpClient = new Windows.Web.Http.HttpClient();
           response = await httpClient.GetAsync(requestUri);
           if (response.StatusCode == HttpStatusCode.Ok)
           {
               result.Append("successful");
           }
           else
           {
               result = result.Append("failed with ");
               result = result.Append(response.StatusCode);
           }
       }
       catch (Exception ex)
       {
           result = result.Append("failed with ");
           result = result.Append(ex.Message);
       }

       Result.Text = result.ToString();
   }
   ```

1. In the MainPage.xaml.cs file, add the following click handlers for the button to browse for a PFX file and the button to import a selected PFX file into the certificate store.

   ```cs
   private async void Import_Click(object sender, RoutedEventArgs e)
   {
       try
       {
           Result.Text = "Importing selected certificate into user certificate store....";
           await CertificateEnrollmentManager.UserCertificateEnrollmentManager.ImportPfxDataAsync(
                 pfxCert,
                 PfxPassword.Password,
                 ExportOption.Exportable,
                 KeyProtectionLevel.NoConsent,
                 InstallOptions.DeleteExpired,
                 "Import Pfx");

           Result.Text = "Certificate import succeeded";
       }
       catch (Exception ex)
       {
           Result.Text = "Certificate import failed with " + ex.Message;
       }
   }

   private async void Browse_Click(object sender, RoutedEventArgs e)
   {
       var result = new StringBuilder("Pfx file selection ");
       var pfxFilePicker = new FileOpenPicker();
       IntPtr hwnd = WinRT.Interop.WindowNative.GetWindowHandle(this);
       WinRT.Interop.InitializeWithWindow.Initialize(pfxFilePicker, hwnd);
       pfxFilePicker.FileTypeFilter.Add(".pfx");
       pfxFilePicker.CommitButtonText = "Open";
       try
       {
           StorageFile pfxFile = await pfxFilePicker.PickSingleFileAsync();
           if (pfxFile != null)
           {
               IBuffer buffer = await FileIO.ReadBufferAsync(pfxFile);
               using (DataReader dataReader = DataReader.FromBuffer(buffer))
               {
                   byte[] bytes = new byte[buffer.Length];
                   dataReader.ReadBytes(bytes);
                   pfxCert = System.Convert.ToBase64String(bytes);
                   PfxPassword.Password = string.Empty;
                   result.Append("succeeded");
               }
           }
           else
           {
               result.Append("failed");
           }
       }
       catch (Exception ex)
       {
           result.Append("failed with ");
           result.Append(ex.Message); ;
       }

       Result.Text = result.ToString();
   }
   ```

1. Open the Package.appxmanifest file and add the following capabilities to the **Capabilities** tab.

   - **EnterpriseAuthentication**
   - **SharedUserCertificates**

1. Run your app and log in to your secured web service as well as import a PFX file into the local certificate store.

You can use these steps to create multiple apps that use the same user certificate to access the same or different secured web services.

## Related topics

[Windows Hello](windows-hello.md)

[Security and identity](index.md)
