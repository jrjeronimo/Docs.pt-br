---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: "Autenticação de dois fatores usando SMS e email com o ASP.NET Identity | Microsoft Docs"
author: HaoK
description: "Este tutorial mostrará como configurar a autenticação de dois fatores (2FA) usando o SMS e email. Este artigo foi escrito de Rick Anderson ( @RickAndMSFT ), por..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: ecb1fc693063995a3a05a7af5db64554c9f595e2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a><span data-ttu-id="63df6-104">Autenticação de dois fatores usando SMS e email com a identidade do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="63df6-104">Two-factor authentication using SMS and email with ASP.NET Identity</span></span>
====================
<span data-ttu-id="63df6-105">por [Kung Hao](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span><span class="sxs-lookup"><span data-stu-id="63df6-105">by [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span></span>

> <span data-ttu-id="63df6-106">Este tutorial mostrará como configurar a autenticação de dois fatores (2FA) usando o SMS e email.</span><span class="sxs-lookup"><span data-stu-id="63df6-106">This tutorial will show you how to set up Two-factor authentication (2FA) using SMS and email.</span></span>
> 
> <span data-ttu-id="63df6-107">Este artigo foi escrito de Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung e Suhas Joshi.</span><span class="sxs-lookup"><span data-stu-id="63df6-107">This article was written by Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung, and Suhas Joshi.</span></span> <span data-ttu-id="63df6-108">O exemplo do NuGet foi escrito principalmente por Hao Kung.</span><span class="sxs-lookup"><span data-stu-id="63df6-108">The NuGet sample was written primarily by Hao Kung.</span></span>


<span data-ttu-id="63df6-109">Este tópico aborda os seguintes tópicos:</span><span class="sxs-lookup"><span data-stu-id="63df6-109">This topic covers the following:</span></span>

- [<span data-ttu-id="63df6-110">Compilando o exemplo de identidade</span><span class="sxs-lookup"><span data-stu-id="63df6-110">Building the Identity sample</span></span>](#build)
- [<span data-ttu-id="63df6-111">Configurar o SMS para autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="63df6-111">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="63df6-112">Habilitar a autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="63df6-112">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="63df6-113">Como registrar um provedor de autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="63df6-113">How to register a Two-factor authentication provider</span></span>](#reg)
- [<span data-ttu-id="63df6-114">Combinar as contas de logon local e social</span><span class="sxs-lookup"><span data-stu-id="63df6-114">Combine social and local login accounts</span></span>](#combine)
- [<span data-ttu-id="63df6-115">Bloqueio de conta contra ataques de força bruta</span><span class="sxs-lookup"><span data-stu-id="63df6-115">Account lockout from brute force attacks</span></span>](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a><span data-ttu-id="63df6-116">Compilando o exemplo de identidade</span><span class="sxs-lookup"><span data-stu-id="63df6-116">Building the Identity sample</span></span>

<span data-ttu-id="63df6-117">Nesta seção, você usará o NuGet para baixar um exemplo com que trabalharemos.</span><span class="sxs-lookup"><span data-stu-id="63df6-117">In this section, you'll use NuGet to download a sample we will work with.</span></span> <span data-ttu-id="63df6-118">Comece instalando e executando [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="63df6-118">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="63df6-119">Instalar o Visual Studio [2013 atualização 2](https://go.microsoft.com/fwlink/?LinkId=390521) ou superior.</span><span class="sxs-lookup"><span data-stu-id="63df6-119">Install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="63df6-120">Aviso: Você deve instalar o Visual Studio [2013 atualização 2](https://go.microsoft.com/fwlink/?LinkId=390521) para concluir este tutorial.</span><span class="sxs-lookup"><span data-stu-id="63df6-120">Warning: You must install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) to complete this tutorial.</span></span>


1. <span data-ttu-id="63df6-121">Criar um novo ***vazio*** projeto da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="63df6-121">Create a new ***empty*** ASP.NET Web project.</span></span>
2. <span data-ttu-id="63df6-122">No Console do Gerenciador de pacotes, digite os seguintes comandos a seguir:</span><span class="sxs-lookup"><span data-stu-id="63df6-122">In the Package Manager Console, enter the following the following commands:</span></span>  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
 <span data-ttu-id="63df6-123">Neste tutorial, vamos usar [SendGrid](http://sendgrid.com/) para enviar email e [Twilio](https://www.twilio.com/) ou [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) para textos do sms.</span><span class="sxs-lookup"><span data-stu-id="63df6-123">In this tutorial, we'll use [SendGrid](http://sendgrid.com/) to send email and [Twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) for sms texting.</span></span> <span data-ttu-id="63df6-124">O `Identity.Samples` pacote instala o código que trabalhará.</span><span class="sxs-lookup"><span data-stu-id="63df6-124">The `Identity.Samples` package installs the code we will be working with.</span></span>
3. <span data-ttu-id="63df6-125">Definir o [projeto para usar SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="63df6-125">Set the [project to use SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="63df6-126">*Opcional*: siga as instruções no meu [tutorial de confirmação de Email](account-confirmation-and-password-recovery-with-aspnet-identity.md) ligar SendGrid e, em seguida, executar o aplicativo e registrar uma conta de email.</span><span class="sxs-lookup"><span data-stu-id="63df6-126">*Optional*: Follow the instructions in my [Email confirmation tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md) to hook up SendGrid and then run the app and register an email account.</span></span>
5. <span data-ttu-id="63df6-127">* Opcional: * Remova o código de confirmação de link de email de demonstração do exemplo (o `ViewBag.Link` código no controlador de conta.</span><span class="sxs-lookup"><span data-stu-id="63df6-127">*Optional: *Remove the demo email link confirmation code from the sample (The `ViewBag.Link` code in the account controller.</span></span> <span data-ttu-id="63df6-128">Consulte o `DisplayEmail` e `ForgotPasswordConfirmation` métodos de ação e modos de exibição do razor).</span><span class="sxs-lookup"><span data-stu-id="63df6-128">See the `DisplayEmail` and `ForgotPasswordConfirmation` action methods and razor views ).</span></span>
6. <span data-ttu-id="63df6-129">* Opcional: * Remova o `ViewBag.Status` código de controladores de gerenciamento e a conta no *Views\Account\VerifyCode.cshtml* e *Views\Manage\VerifyPhoneNumber.cshtml* modos de exibição do razor.</span><span class="sxs-lookup"><span data-stu-id="63df6-129">*Optional: *Remove the `ViewBag.Status` code from the Manage and Account controllers and from the *Views\Account\VerifyCode.cshtml* and *Views\Manage\VerifyPhoneNumber.cshtml* razor views.</span></span> <span data-ttu-id="63df6-130">Como alternativa, você pode manter o `ViewBag.Status` exibição para testar como este aplicativo funciona localmente sem a necessidade de conectar e enviar email e mensagens SMS.</span><span class="sxs-lookup"><span data-stu-id="63df6-130">Alternatively, you can keep the `ViewBag.Status` display to test how this app works locally without having to hook up and send email and SMS messages.</span></span>

> [!NOTE]
> <span data-ttu-id="63df6-131">Aviso: Se você alterar as configurações de segurança neste exemplo, aplicativos de produção serão necessário passar por uma auditoria de segurança que explicitamente chama as alterações feitas.</span><span class="sxs-lookup"><span data-stu-id="63df6-131">Warning: If you change any of the security settings in this sample, productions apps will need to undergo a security audit that explicitly calls out the changes made.</span></span>


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="63df6-132">Configurar o SMS para autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="63df6-132">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="63df6-133">Este tutorial fornece instruções sobre como usar o Twilio ou ASPSMS, mas você pode usar qualquer outro provedor SMS.</span><span class="sxs-lookup"><span data-stu-id="63df6-133">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="63df6-134">**Criando uma conta de usuário com um provedor de SMS**</span><span class="sxs-lookup"><span data-stu-id="63df6-134">**Creating a User Account with an SMS provider**</span></span>  
  
 <span data-ttu-id="63df6-135">Criar um [Twilio](https://www.twilio.com/try-twilio) ou um [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) conta.</span><span class="sxs-lookup"><span data-stu-id="63df6-135">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="63df6-136">**Instalando pacotes adicionais ou adicionando referências de serviço**</span><span class="sxs-lookup"><span data-stu-id="63df6-136">**Installing additional packages or adding service references**</span></span>  
  
 <span data-ttu-id="63df6-137">Twilio:</span><span class="sxs-lookup"><span data-stu-id="63df6-137">Twilio:</span></span>  
 <span data-ttu-id="63df6-138">No Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="63df6-138">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
 <span data-ttu-id="63df6-139">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="63df6-139">ASPSMS:</span></span>  
 <span data-ttu-id="63df6-140">A referência de serviço a seguir precisa ser adicionado:</span><span class="sxs-lookup"><span data-stu-id="63df6-140">The following service reference needs to be added:</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
 <span data-ttu-id="63df6-141">endereço:</span><span class="sxs-lookup"><span data-stu-id="63df6-141">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 <span data-ttu-id="63df6-142">Namespace:</span><span class="sxs-lookup"><span data-stu-id="63df6-142">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="63df6-143">**Descobrir as credenciais do usuário do provedor de SMS**</span><span class="sxs-lookup"><span data-stu-id="63df6-143">**Figuring out SMS Provider User credentials**</span></span>  
  
 <span data-ttu-id="63df6-144">Twilio:</span><span class="sxs-lookup"><span data-stu-id="63df6-144">Twilio:</span></span>  
 <span data-ttu-id="63df6-145">Do **painel** guia da sua conta do Twilio, copie o **SID de conta** e **token de autenticação**.</span><span class="sxs-lookup"><span data-stu-id="63df6-145">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
 <span data-ttu-id="63df6-146">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="63df6-146">ASPSMS:</span></span>  
 <span data-ttu-id="63df6-147">De suas configurações de conta, navegue até **chave de usuário confidenciais** e copiá-lo junto com seus Self definido **senha**.</span><span class="sxs-lookup"><span data-stu-id="63df6-147">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
 <span data-ttu-id="63df6-148">Mais tarde armazenaremos esses valores nas variáveis de `SMSAccountIdentification` e `SMSAccountPassword` .</span><span class="sxs-lookup"><span data-stu-id="63df6-148">We will later store these values in the variables `SMSAccountIdentification` and `SMSAccountPassword` .</span></span>
4. <span data-ttu-id="63df6-149">**Especificando SenderID / originador**</span><span class="sxs-lookup"><span data-stu-id="63df6-149">**Specifying SenderID / Originator**</span></span>  
  
 <span data-ttu-id="63df6-150">Twilio:</span><span class="sxs-lookup"><span data-stu-id="63df6-150">Twilio:</span></span>  
 <span data-ttu-id="63df6-151">Do **números** guia, copie o seu número de telefone do Twilio.</span><span class="sxs-lookup"><span data-stu-id="63df6-151">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
 <span data-ttu-id="63df6-152">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="63df6-152">ASPSMS:</span></span>  
 <span data-ttu-id="63df6-153">Dentro de **desbloquear originadores** Menu desbloquear originadores de uma ou mais ou escolha um originador alfanumérico (não é suportado por todas as redes).</span><span class="sxs-lookup"><span data-stu-id="63df6-153">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
 <span data-ttu-id="63df6-154">Posteriormente, armazenará esse valor na variável `SMSAccountFrom` .</span><span class="sxs-lookup"><span data-stu-id="63df6-154">We will later store this value in the variable `SMSAccountFrom` .</span></span>
5. <span data-ttu-id="63df6-155">**Transferindo as credenciais do provedor SMS em aplicativo**</span><span class="sxs-lookup"><span data-stu-id="63df6-155">**Transferring SMS provider credentials into app**</span></span>  
  
 <span data-ttu-id="63df6-156">Verifique as credenciais e o número de telefone do remetente disponíveis para o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="63df6-156">Make the credentials and sender phone number available to the app:</span></span>

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > <span data-ttu-id="63df6-157">Segurança - nunca armazenar os dados confidenciais em seu código-fonte.</span><span class="sxs-lookup"><span data-stu-id="63df6-157">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="63df6-158">A conta e as credenciais são adicionadas ao código acima para manter o exemplo simples.</span><span class="sxs-lookup"><span data-stu-id="63df6-158">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="63df6-159">Consulte de Jon Atten [ASP.NET MVC: manter particulares configurações fora do controle do código-fonte](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span><span class="sxs-lookup"><span data-stu-id="63df6-159">See Jon Atten's [ASP.NET MVC: Keep Private Settings Out of Source Control](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span></span>
6. <span data-ttu-id="63df6-160">**Implementação de transferência de dados para o provedor de SMS**</span><span class="sxs-lookup"><span data-stu-id="63df6-160">**Implementation of data transfer to SMS provider**</span></span>  
  
 <span data-ttu-id="63df6-161">Configurar o `SmsService` classe no *aplicativo\_Start\IdentityConfig.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="63df6-161">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
 <span data-ttu-id="63df6-162">Dependendo do provedor SMS usado ativar o **Twilio** ou **ASPSMS** seção:</span><span class="sxs-lookup"><span data-stu-id="63df6-162">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. <span data-ttu-id="63df6-163">Executar o aplicativo e faça logon usando a conta que você registrou anteriormente.</span><span class="sxs-lookup"><span data-stu-id="63df6-163">Run the app and log in with the account you previously registered.</span></span>
8. <span data-ttu-id="63df6-164">Clique em sua ID de usuário, que ativa o `Index` método de ação `Manage` controlador.</span><span class="sxs-lookup"><span data-stu-id="63df6-164">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. <span data-ttu-id="63df6-165">Clique em Adicionar.</span><span class="sxs-lookup"><span data-stu-id="63df6-165">Click Add.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. <span data-ttu-id="63df6-166">Em alguns segundos, você receberá uma mensagem de texto com o código de verificação.</span><span class="sxs-lookup"><span data-stu-id="63df6-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="63df6-167">Insira-o e pressione **enviar**.</span><span class="sxs-lookup"><span data-stu-id="63df6-167">Enter it and press **Submit**.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. <span data-ttu-id="63df6-168">O modo de gerenciar mostra que o número de telefone foi adicionado.</span><span class="sxs-lookup"><span data-stu-id="63df6-168">The Manage view shows your phone number was added.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a><span data-ttu-id="63df6-169">Examine o código</span><span class="sxs-lookup"><span data-stu-id="63df6-169">Examine the code</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

<span data-ttu-id="63df6-170">O `Index` método de ação `Manage` controlador define a mensagem de status com base em sua ação anterior e fornece links para alterar sua senha local ou adicionar uma conta local.</span><span class="sxs-lookup"><span data-stu-id="63df6-170">The `Index` action method in `Manage` controller sets the status message based on your previous action and provides links to change your local password or add a local account.</span></span> <span data-ttu-id="63df6-171">O `Index` método também exibe o estado ou a 2FA 2FA habilitado, logons externos, número de telefone e lembre-se o método 2FA para este navegador (explicado posteriormente).</span><span class="sxs-lookup"><span data-stu-id="63df6-171">The `Index` method also displays the state or your 2FA phone number, external logins, 2FA enabled, and remember 2FA method for this browser(explained later).</span></span> <span data-ttu-id="63df6-172">Clicando em sua ID de usuário (email) na barra de título não passa uma mensagem.</span><span class="sxs-lookup"><span data-stu-id="63df6-172">Clicking on your user ID (email) in the title bar doesn't pass a message.</span></span> <span data-ttu-id="63df6-173">Clicar no **telefone: remover** link passa `Message=RemovePhoneSuccess` como uma cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="63df6-173">Clicking on the **Phone Number : remove** link passes `Message=RemovePhoneSuccess` as a query string.</span></span>  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

<span data-ttu-id="63df6-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span><span class="sxs-lookup"><span data-stu-id="63df6-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span></span>

<span data-ttu-id="63df6-175">O `AddPhoneNumber` método de ação exibe uma caixa de diálogo para inserir um número de telefone que pode receber mensagens SMS.</span><span class="sxs-lookup"><span data-stu-id="63df6-175">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

<span data-ttu-id="63df6-176">Clicar no **enviar o código de verificação** botão envia o número de telefone para o HTTP POST `AddPhoneNumber` método de ação.</span><span class="sxs-lookup"><span data-stu-id="63df6-176">Clicking on the **Send verification code** button posts the phone number to the HTTP POST `AddPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

<span data-ttu-id="63df6-177">O `GenerateChangePhoneNumberTokenAsync` método gera o token de segurança que será definido na mensagem SMS.</span><span class="sxs-lookup"><span data-stu-id="63df6-177">The `GenerateChangePhoneNumberTokenAsync` method generates the security token which will be set in the SMS message.</span></span> <span data-ttu-id="63df6-178">Se o serviço SMS tiver sido configurado, o token é enviado como a cadeia de caracteres &quot;seu código de segurança é &lt;token&gt;&quot;.</span><span class="sxs-lookup"><span data-stu-id="63df6-178">If the SMS service has been configured, the token is sent as the string &quot;Your security code is &lt;token&gt;&quot;.</span></span> <span data-ttu-id="63df6-179">O `SmsService.SendAsync` método é chamado de forma assíncrona, em seguida, o aplicativo é redirecionado para o `VerifyPhoneNumber` método de ação (que exibe a caixa de diálogo a seguir), onde você pode inserir o código de verificação.</span><span class="sxs-lookup"><span data-stu-id="63df6-179">The `SmsService.SendAsync` method to is called asynchronously, then the app is redirected to the `VerifyPhoneNumber` action method (which displays the following dialog), where you can enter the verification code.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

<span data-ttu-id="63df6-180">Depois que você insira o código e clique em enviar, o código é postado para o HTTP POST `VerifyPhoneNumber` método de ação.</span><span class="sxs-lookup"><span data-stu-id="63df6-180">Once you enter the code and click submit, the code is posted to the HTTP POST `VerifyPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

<span data-ttu-id="63df6-181">O `ChangePhoneNumberAsync` método verifica o código de segurança lançadas.</span><span class="sxs-lookup"><span data-stu-id="63df6-181">The `ChangePhoneNumberAsync` method checks the posted security code.</span></span> <span data-ttu-id="63df6-182">Se o código está correto, o número de telefone é adicionado para o `PhoneNumber` campo o `AspNetUsers` tabela.</span><span class="sxs-lookup"><span data-stu-id="63df6-182">If the code is correct, the phone number is added to the `PhoneNumber` field of the `AspNetUsers` table.</span></span> <span data-ttu-id="63df6-183">Se essa chamada for bem-sucedida, o `SignInAsync` método é chamado:</span><span class="sxs-lookup"><span data-stu-id="63df6-183">If that call is successful, the `SignInAsync` method is called:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

<span data-ttu-id="63df6-184">O `isPersistent` parâmetro define se a sessão de autenticação é persistente entre várias solicitações.</span><span class="sxs-lookup"><span data-stu-id="63df6-184">The `isPersistent` parameter sets whether the authentication session is persisted across multiple requests.</span></span>

<span data-ttu-id="63df6-185">Quando você altera seu perfil de segurança, um novo carimbo de segurança é gerado e armazenado na `SecurityStamp` campo o *AspNetUsers* tabela.</span><span class="sxs-lookup"><span data-stu-id="63df6-185">When you change your security profile, a new security stamp is generated and stored in the `SecurityStamp` field of the *AspNetUsers* table.</span></span> <span data-ttu-id="63df6-186">Observe que o `SecurityStamp` campo é diferente do cookie de segurança.</span><span class="sxs-lookup"><span data-stu-id="63df6-186">Note, the `SecurityStamp` field is different from the security cookie.</span></span> <span data-ttu-id="63df6-187">O cookie de segurança não é armazenado no `AspNetUsers` tabela (ou em qualquer lugar no banco de dados de identidade).</span><span class="sxs-lookup"><span data-stu-id="63df6-187">The security cookie is not stored in the `AspNetUsers` table (or anywhere else in the Identity DB).</span></span> <span data-ttu-id="63df6-188">O token de cookie de segurança é autoassinado usando [DPAPI](https://msdn.microsoft.com/en-us/library/system.security.cryptography.protecteddata.aspx) e é criado com o `UserId, SecurityStamp` e informações de tempo de expiração.</span><span class="sxs-lookup"><span data-stu-id="63df6-188">The security cookie token is self-signed using [DPAPI](https://msdn.microsoft.com/en-us/library/system.security.cryptography.protecteddata.aspx) and is created with the `UserId, SecurityStamp` and expiration time information.</span></span>

<span data-ttu-id="63df6-189">O middleware de cookie verifica o cookie em cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="63df6-189">The cookie middleware checks the cookie on each request.</span></span> <span data-ttu-id="63df6-190">O `SecurityStampValidator` método o `Startup` classe atinge o banco de dados e verifica periodicamente, o carimbo de segurança conforme especificado com o `validateInterval`.</span><span class="sxs-lookup"><span data-stu-id="63df6-190">The `SecurityStampValidator` method in the `Startup` class hits the DB and checks security stamp periodically, as specified with the `validateInterval`.</span></span> <span data-ttu-id="63df6-191">Isso acontece apenas a cada 30 minutos (em nosso exemplo), a menos que você altere seu perfil de segurança.</span><span class="sxs-lookup"><span data-stu-id="63df6-191">This only happens every 30 minutes (in our sample) unless you change your security profile.</span></span> <span data-ttu-id="63df6-192">O intervalo de 30 minutos foi escolhido para minimizar as viagens ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="63df6-192">The 30 minute interval was chosen to minimize trips to the database.</span></span>

<span data-ttu-id="63df6-193">O `SignInAsync` método precisa ser chamado quando nenhuma alteração é feita para o perfil de segurança.</span><span class="sxs-lookup"><span data-stu-id="63df6-193">The `SignInAsync` method needs to be called when any change is made to the security profile.</span></span> <span data-ttu-id="63df6-194">Quando o perfil de segurança for alterada, o banco de dados é as atualizações de `SecurityStamp` campo e sem chamar o `SignInAsync` método seria permanecer conectado *somente* até a próxima vez que o pipeline OWIN atinge o banco de dados (o `validateInterval`).</span><span class="sxs-lookup"><span data-stu-id="63df6-194">When the security profile changes, the database is updates the `SecurityStamp` field, and without calling the `SignInAsync` method you would stay logged in *only* until the next time the OWIN pipeline hits the database (the `validateInterval`).</span></span> <span data-ttu-id="63df6-195">Você pode testar isso alterando o `SignInAsync` método para retornar imediatamente e definindo o cookie `validateInterval` propriedade de 30 minutos para 5 segundos:</span><span class="sxs-lookup"><span data-stu-id="63df6-195">You can test this by changing the `SignInAsync` method to return immediately, and setting the cookie `validateInterval` property from 30 minutes to 5 seconds:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

<span data-ttu-id="63df6-196">Com as alterações de código acima, você pode alterar seu perfil de segurança (por exemplo, alterando o estado do **dois fatores ativados**) e você será desconectado em 5 segundos quando o `SecurityStampValidator.OnValidateIdentity` método falhar.</span><span class="sxs-lookup"><span data-stu-id="63df6-196">With the above code changes, you can change your security profile (for example, by changing the state of **Two Factor Enabled**) and you will be logged out in 5 seconds when the `SecurityStampValidator.OnValidateIdentity` method fails.</span></span> <span data-ttu-id="63df6-197">Remova a linha de retorno no `SignInAsync` método, alterar a outro perfil de segurança e você não será registrado-out. O `SignInAsync` método gera um novo cookie de segurança.</span><span class="sxs-lookup"><span data-stu-id="63df6-197">Remove the return line in the `SignInAsync` method, make another security profile change and you will not be logged out. The `SignInAsync` method generates a new security cookie.</span></span>

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a><span data-ttu-id="63df6-198">Habilitar a autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="63df6-198">Enable two-factor authentication</span></span>

<span data-ttu-id="63df6-199">No aplicativo de exemplo, você precisa usar a interface do usuário para habilitar a autenticação de dois fatores (2FA).</span><span class="sxs-lookup"><span data-stu-id="63df6-199">In the sample app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="63df6-200">Para habilitar a 2FA, clique em sua ID de usuário (alias de email) na barra de navegação.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="63df6-200">To enable 2FA, click on your user ID (email alias) in the navigation bar.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span></span>  
<span data-ttu-id="63df6-201">Clique em Habilitar 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="63df6-201">Click on enable 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span></span> <span data-ttu-id="63df6-202">Faça logoff e logon novamente.</span><span class="sxs-lookup"><span data-stu-id="63df6-202">Log out, then log back in.</span></span> <span data-ttu-id="63df6-203">Se você tiver habilitado o email (consulte meu [tutorial anterior](account-confirmation-and-password-recovery-with-aspnet-identity.md)), você pode selecionar o email para 2FA ou SMS.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="63df6-203">If you've enabled email (see my [previous tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span></span> <span data-ttu-id="63df6-204">A página de código verificar é exibida, onde você pode inserir o código (de SMS ou email).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="63df6-204">The Verify Code page is displayed where you can enter the code (from SMS or email).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span></span> <span data-ttu-id="63df6-205">Clicar no **lembrar este navegador** caixa de seleção isentará a necessidade de usar 2FA para fazer logon com esse computador e o navegador.</span><span class="sxs-lookup"><span data-stu-id="63df6-205">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on with that computer and browser.</span></span> <span data-ttu-id="63df6-206">Habilitando 2FA e clicando no **lembrar este navegador** fornecerá 2FA forte proteção contra usuários mal-intencionados que tentar acessar sua conta, desde que eles não têm acesso a seu computador.</span><span class="sxs-lookup"><span data-stu-id="63df6-206">Enabling 2FA and clicking on the **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your computer.</span></span> <span data-ttu-id="63df6-207">Você pode fazer isso em qualquer computador privada que você usa regularmente.</span><span class="sxs-lookup"><span data-stu-id="63df6-207">You can do this on any private machine you regularly use.</span></span> <span data-ttu-id="63df6-208">Definindo **lembrar este navegador**, obter a segurança adicional de 2FA de computadores que você não usa regularmente e obter a conveniência de não ter percorrer 2FA em seus próprios computadores.</span><span class="sxs-lookup"><span data-stu-id="63df6-208">By setting **Remember this browser**, you get the added security of 2FA from computers you don't regularly use, and you get the convenience on not having to go through 2FA on your own computers.</span></span> 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a><span data-ttu-id="63df6-209">Como registrar um provedor de autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="63df6-209">How to register a Two-factor authentication provider</span></span>

<span data-ttu-id="63df6-210">Quando você cria um novo projeto MVC, a *IdentityConfig.cs* arquivo contém o código a seguir para registrar um provedor de autenticação de dois fatores:</span><span class="sxs-lookup"><span data-stu-id="63df6-210">When you create a new MVC project, the *IdentityConfig.cs* file contains the following code to register a Two-factor authentication provider:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a><span data-ttu-id="63df6-211">Adicionar um número de telefone para 2FA</span><span class="sxs-lookup"><span data-stu-id="63df6-211">Add a phone number for 2FA</span></span>

<span data-ttu-id="63df6-212">O `AddPhoneNumber` método de ação de `Manage` controlador gera um token de segurança e envia-o para o telefone número que você forneceu.</span><span class="sxs-lookup"><span data-stu-id="63df6-212">The `AddPhoneNumber` action method in the `Manage` controller generates a security token and sends it to the phone number you have provided.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

<span data-ttu-id="63df6-213">Depois de enviar o token, ele redireciona para o `VerifyPhoneNumber` método de ação, onde você pode inserir o código para registrar o SMS para 2FA.</span><span class="sxs-lookup"><span data-stu-id="63df6-213">After sending the token, it redirects to the `VerifyPhoneNumber` action method, where you can enter the code to register SMS for 2FA.</span></span> <span data-ttu-id="63df6-214">2FA SMS não é usado até que você verificou se o número de telefone.</span><span class="sxs-lookup"><span data-stu-id="63df6-214">SMS 2FA is not used until you have verified the phone number.</span></span>

## <a name="enabling-2fa"></a><span data-ttu-id="63df6-215">Habilitando 2FA</span><span class="sxs-lookup"><span data-stu-id="63df6-215">Enabling 2FA</span></span>

<span data-ttu-id="63df6-216">O `EnableTFA` método de ação permite 2FA:</span><span class="sxs-lookup"><span data-stu-id="63df6-216">The `EnableTFA` action method enables 2FA:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

<span data-ttu-id="63df6-217">Observe o `SignInAsync` deve ser chamado porque habilitar 2FA é uma alteração para o perfil de segurança.</span><span class="sxs-lookup"><span data-stu-id="63df6-217">Note the `SignInAsync` must be called because enable 2FA is a change to the security profile.</span></span> <span data-ttu-id="63df6-218">Quando 2FA estiver habilitado, o usuário precisará usar 2FA para logon, usando as abordagens 2FA eles registraram (SMS e email no exemplo).</span><span class="sxs-lookup"><span data-stu-id="63df6-218">When 2FA is enabled, the user will need to use 2FA to log in, using the 2FA approaches they have registered (SMS and email in the sample).</span></span>

<span data-ttu-id="63df6-219">Você pode adicionar mais provedores de 2FA como geradores de código QR ou você pode escrever sua propriedade (consulte [usando o Google Authenticator com a identidade do ASP.NET](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span><span class="sxs-lookup"><span data-stu-id="63df6-219">You can add more 2FA providers such as QR code generators or you can write you own (See [Using Google Authenticator with ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span></span>

> [!NOTE]
> <span data-ttu-id="63df6-220">Os códigos de 2FA são gerados usando [baseada em tempo algoritmo de senha de uso único](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) e são válidos por seis minutos.</span><span class="sxs-lookup"><span data-stu-id="63df6-220">The 2FA codes are generated using [Time-based One-time Password Algorithm](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) and codes are valid for six minutes.</span></span> <span data-ttu-id="63df6-221">Se você levar mais de seis minutos para inserir o código, você obterá uma mensagem de erro de código inválido.</span><span class="sxs-lookup"><span data-stu-id="63df6-221">If you take more than six minutes to enter the code, you'll get an Invalid code error message.</span></span>


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="63df6-222">Combinar as contas de logon local e social</span><span class="sxs-lookup"><span data-stu-id="63df6-222">Combine social and local login accounts</span></span>

<span data-ttu-id="63df6-223">Você pode combinar as contas locais e sociais clicando no link seu email.</span><span class="sxs-lookup"><span data-stu-id="63df6-223">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="63df6-224">Na sequência a seguir &quot; RickAndMSFT@gmail.com &quot; é criado pela primeira vez como um logon local, mas você pode criar a conta como um log social primeiro e adicionar um logon local.</span><span class="sxs-lookup"><span data-stu-id="63df6-224">In the following sequence &quot;RickAndMSFT@gmail.com&quot; is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

<span data-ttu-id="63df6-225">Clique no **gerenciar** link.</span><span class="sxs-lookup"><span data-stu-id="63df6-225">Click on the **Manage** link.</span></span> <span data-ttu-id="63df6-226">Observe externo 0 (logons sociais) associada à conta.</span><span class="sxs-lookup"><span data-stu-id="63df6-226">Note the 0 external (social logins) associated with this account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

<span data-ttu-id="63df6-227">Clique no link para outro log no serviço e aceitar as solicitações do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="63df6-227">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="63df6-228">As duas contas foram combinadas, você poderá fazer logon com a conta.</span><span class="sxs-lookup"><span data-stu-id="63df6-228">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="63df6-229">Convém que os usuários adicionem contas locais no caso de seu log social no serviço de autenticação está inoperante ou mais provável que perdeu o acesso à sua conta social.</span><span class="sxs-lookup"><span data-stu-id="63df6-229">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="63df6-230">Na imagem a seguir, Tom é um logon social (que você pode ver o **logons externos: 1** mostradas na página).</span><span class="sxs-lookup"><span data-stu-id="63df6-230">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

<span data-ttu-id="63df6-231">Clicando em **escolher uma senha** permite que você adicione um log local em associados com a mesma conta.</span><span class="sxs-lookup"><span data-stu-id="63df6-231">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a><span data-ttu-id="63df6-232">Bloqueio de conta contra ataques de força bruta</span><span class="sxs-lookup"><span data-stu-id="63df6-232">Account lockout from brute force attacks</span></span>

<span data-ttu-id="63df6-233">Você pode proteger as contas em seu aplicativo contra ataques de dicionário, permitindo o bloqueio do usuário.</span><span class="sxs-lookup"><span data-stu-id="63df6-233">You can protect the accounts on your app from dictionary attacks by enabling user lockout.</span></span> <span data-ttu-id="63df6-234">O código a seguir no `ApplicationUserManager Create` método configura o bloqueio:</span><span class="sxs-lookup"><span data-stu-id="63df6-234">The following code in the `ApplicationUserManager Create` method configures lock out:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

<span data-ttu-id="63df6-235">O código acima permite que o bloqueio para somente a autenticação de dois fatores.</span><span class="sxs-lookup"><span data-stu-id="63df6-235">The code above enables lockout for two factor authentication only.</span></span> <span data-ttu-id="63df6-236">Enquanto você pode habilitar o bloqueio para logons alterando `shouldLockout` como verdadeiro no `Login` método do controlador de conta, é recomendável não habilitar bloqueio para logons porque ele faz com que a conta suscetíveis a [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) logon ataques.</span><span class="sxs-lookup"><span data-stu-id="63df6-236">While you can enable lock out for logins by changing `shouldLockout` to true in the `Login` method of the account controller, we recommend you not enable lock out for logins because it makes the account susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) login attacks.</span></span> <span data-ttu-id="63df6-237">No código de exemplo, o bloqueio está desabilitado para a conta de administrador criada no `ApplicationDbInitializer Seed` método:</span><span class="sxs-lookup"><span data-stu-id="63df6-237">In the sample code, lockout is disabled for the admin account created in the `ApplicationDbInitializer Seed` method:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a><span data-ttu-id="63df6-238">Exigir que o usuário tenha uma conta de email validado</span><span class="sxs-lookup"><span data-stu-id="63df6-238">Requiring a user to have a validated email account</span></span>

<span data-ttu-id="63df6-239">O código a seguir requer que um usuário tenha uma conta de email validadas antes de fazer logon:</span><span class="sxs-lookup"><span data-stu-id="63df6-239">The following code requires a user to have a validated email account before they can log in:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a><span data-ttu-id="63df6-240">Como o SignInManager procura 2FA requisito</span><span class="sxs-lookup"><span data-stu-id="63df6-240">How SignInManager checks for 2FA requirement</span></span>

<span data-ttu-id="63df6-241">Logon local e para log social na verificação para ver se 2FA está habilitada.</span><span class="sxs-lookup"><span data-stu-id="63df6-241">Both the local log in and social log in check to see if 2FA is enabled.</span></span> <span data-ttu-id="63df6-242">Se 2FA estiver habilitado, o `SignInManager` retorna do método de logon `SignInStatus.RequiresVerification`, e o usuário será redirecionado para o `SendCode` método de ação, onde eles terão de digitar o código para concluir o logon na sequência.</span><span class="sxs-lookup"><span data-stu-id="63df6-242">If 2FA is enabled, the `SignInManager` logon method returns `SignInStatus.RequiresVerification`, and the user will be redirected to the `SendCode` action method, where they will have to enter the code to complete the log in sequence.</span></span> <span data-ttu-id="63df6-243">Se o usuário tiver RememberMe é definido no cookie de local de usuários, o `SignInManager` retornará `SignInStatus.Success` e não será necessário passar por 2FA.</span><span class="sxs-lookup"><span data-stu-id="63df6-243">If the user has RememberMe is set on the users local cookie, the `SignInManager` will return `SignInStatus.Success` and they will not have to go through 2FA.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

<span data-ttu-id="63df6-244">O código a seguir mostra o `SendCode` método de ação.</span><span class="sxs-lookup"><span data-stu-id="63df6-244">The following code shows the `SendCode` action method.</span></span> <span data-ttu-id="63df6-245">Um [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) é criado com todos os métodos de 2FA habilitados para o usuário.</span><span class="sxs-lookup"><span data-stu-id="63df6-245">A [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) is created with all the 2FA methods enabled for the user.</span></span> <span data-ttu-id="63df6-246">O [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) é passado para o [DropDownListFor](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.dropdownlist.aspx) auxiliar, que permite ao usuário selecionar a abordagem 2FA (normalmente, email e SMS).</span><span class="sxs-lookup"><span data-stu-id="63df6-246">The [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) is passed to the [DropDownListFor](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, which allows the user to select the 2FA approach (typically email and SMS).</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

<span data-ttu-id="63df6-247">Depois que o usuário envia a abordagem 2FA, o `HTTP POST SendCode` é chamado de método de ação, o `SignInManager` envia o código de 2FA e o usuário é redirecionado para o `VerifyCode` método de ação podem digitar o código para concluir o logon.</span><span class="sxs-lookup"><span data-stu-id="63df6-247">Once the user posts the 2FA approach, the `HTTP POST SendCode` action method is called, the `SignInManager` sends the 2FA code, and the user is redirected to the `VerifyCode` action method where they can enter the code to complete the log in.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a><span data-ttu-id="63df6-248">Bloqueio de 2FA</span><span class="sxs-lookup"><span data-stu-id="63df6-248">2FA Lockout</span></span>

<span data-ttu-id="63df6-249">Embora você possa definir o bloqueio de conta em falhas de tentativa de senha de logon, essa abordagem torna seu logon suscetíveis a [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) bloqueios.</span><span class="sxs-lookup"><span data-stu-id="63df6-249">Although you can set account lockout on login password attempt failures, that approach makes your login susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) lockouts.</span></span> <span data-ttu-id="63df6-250">Recomendamos que você use o bloqueio de conta apenas com 2FA.</span><span class="sxs-lookup"><span data-stu-id="63df6-250">We recommend you use account lockout only with 2FA.</span></span> <span data-ttu-id="63df6-251">Quando o `ApplicationUserManager` é criado, o código de exemplo define 2FA bloqueio e `MaxFailedAccessAttemptsBeforeLockout` a cinco.</span><span class="sxs-lookup"><span data-stu-id="63df6-251">When the `ApplicationUserManager` is created, the sample code sets 2FA lockout and `MaxFailedAccessAttemptsBeforeLockout` to five.</span></span> <span data-ttu-id="63df6-252">Depois que um usuário fizer logon (por meio de conta local ou social), cada tentativa falha de 2FA é armazenada e se o número máximo de tentativas for atingido, o usuário é bloqueado por cinco minutos (você pode definir o bloqueio de tempo com `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="63df6-252">Once a user logs in (through local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span>

<a id="addRes"></a>

## <a name="additional-resources"></a><span data-ttu-id="63df6-253">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="63df6-253">Additional Resources</span></span>

- <span data-ttu-id="63df6-254">[Identidade do ASP.NET recomendado recursos](../getting-started/aspnet-identity-recommended-resources.md) assim vincula uma lista completa de blogs de identidade, vídeos, tutoriais e ótimo.</span><span class="sxs-lookup"><span data-stu-id="63df6-254">[ASP.NET Identity Recommended Resources](../getting-started/aspnet-identity-recommended-resources.md) Complete list of Identity blogs, videos, tutorials and great SO links.</span></span>
- <span data-ttu-id="63df6-255">[Aplicativo do MVC 5 com o Facebook, Twitter, LinkedIn e logon do Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) também mostra como adicionar informações de perfil para a tabela de usuários.</span><span class="sxs-lookup"><span data-stu-id="63df6-255">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) also shows how to add profile information to the users table.</span></span>
- <span data-ttu-id="63df6-256">[ASP.NET MVC e identidade 2.0: Noções básicas](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) por John Atten.</span><span class="sxs-lookup"><span data-stu-id="63df6-256">[ASP.NET MVC and Identity 2.0: Understanding the Basics](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) by John Atten.</span></span>
- [<span data-ttu-id="63df6-257">Confirmação de conta e senha de recuperação com a identidade do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="63df6-257">Account Confirmation and Password Recovery with ASP.NET Identity</span></span>](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [<span data-ttu-id="63df6-258">Introdução a identidade do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="63df6-258">Introduction to ASP.NET Identity</span></span>](../getting-started/introduction-to-aspnet-identity.md)
- <span data-ttu-id="63df6-259">[Anunciando RTM do ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) por Pranav Rastogi.</span><span class="sxs-lookup"><span data-stu-id="63df6-259">[Announcing RTM of ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) by Pranav Rastogi.</span></span>
- <span data-ttu-id="63df6-260">[Identidade do ASP.NET 2.0: Configurando a validação da conta e a autorização de dois fatores](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) por John Atten.</span><span class="sxs-lookup"><span data-stu-id="63df6-260">[ASP.NET Identity 2.0: Setting Up Account Validation and Two-Factor Authorization](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) by John Atten.</span></span>