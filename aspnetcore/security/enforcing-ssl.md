---
title: Impor HTTPS no ASP.NET Core
author: rick-anderson
description: Mostra como exigir HTTPS/TLS em um ASP.NET Core em aplicativo web.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 6e16191b1a4627e683fd2281e5556b2a6e84c082
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523136"
---
# <a name="enforce-https-in-aspnet-core"></a>Impor HTTPS no ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este documento demonstra como:

* Exigir HTTPS para todas as solicitações.
* Redirecione todas as solicitações HTTP para HTTPS.

Nenhuma API pode impedir que um cliente envie dados confidenciais na primeira solicitação.

> [!WARNING]
> **Não usar** [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) em APIs da Web que recebe informações confidenciais. `RequireHttpsAttribute` usa códigos de status HTTP para redirecionar navegadores de HTTP para HTTPS. Os clientes da API não podem compreender ou obedecem redirecionamentos de HTTP para HTTPS. Esses clientes podem enviar informações por meio de HTTP. As APIs da Web deverá:
>
> * Não realizar a escuta em HTTP.
> * Feche a conexão com o código de status 400 (solicitação incorreta) e não atender à solicitação.

<a name="require"></a>
## <a name="require-https"></a>Exigir HTTPS

::: moniker range=">= aspnetcore-2.1"

É recomendável toda a produção de ASP.NET Core, chamada de aplicativos web:

* O Middleware de redirecionamento de HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) para redirecionar todas as solicitações HTTP para HTTPS.
* [UseHsts](#hsts), protocolo de segurança de transporte estrito HTTP (HSTS).

### <a name="usehttpsredirection"></a>UseHttpsRedirection

O código a seguir chama `UseHttpsRedirection` no `Startup` classe:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

O código realçado anterior:

* Usa o padrão [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).
* Usa o padrão [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), a menos que substituído pela `ASPNETCORE_HTTPS_PORT` variável de ambiente ou [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).

> [!WARNING] 
>Uma porta deve estar disponível para o middleware redirecionar para HTTPS. Se nenhuma porta estiver disponível, o redirecionamento de HTTPS não ocorrerá. A porta HTTPS pode ser especificada por qualquer um dos seguinte configuração:
> 
>* `HttpsRedirectionOptions.HttpsPort` 
>* O `ASPNETCORE_HTTPS_PORT` variável de ambiente. 
>* No desenvolvimento, uma url HTTPS no *launchsettings. JSON*. 
>* Uma url HTTPS configurada diretamente no Kestrel ou HttpSys. 

O seguinte realçado código chama [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) para configurar as opções de middleware:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Chamando `AddHttpsRedirection` só é necessário alterar os valores de ` HttpsPort` ou ` RedirectStatusCode`;

O código realçado anterior:

* Conjuntos [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) para `Status307TemporaryRedirect`, que é o valor padrão.
* Define a porta HTTPS para 5001. O valor padrão é 443.

Os seguintes mecanismos de definir a porta automaticamente:

* O middleware pode descobrir as portas por meio [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) quando as condições a seguir se aplicam:

   * O kestrel ou HTTP. sys é usada diretamente com pontos de extremidade HTTPS (também se aplica a execução do aplicativo com o depurador do Visual Studio Code).
   * Somente **uma porta HTTPS** é usado pelo aplicativo.

* O Visual Studio é usado:
   * O IIS Express tem habilitadas para HTTPS.
   * *launchsettings. JSON* define o `sslPort` para o IIS Express.

> [!NOTE]
> Quando um aplicativo é executado atrás de um proxy reverso (por exemplo, IIS, IIS Express), `IServerAddressesFeature` não está disponível. A porta deve ser configurada manualmente. Quando a porta não for definida, as solicitações não são redirecionadas.

A porta pode ser configurada definindo a [definição de configuração do Host da Web https_port](xref:fundamentals/host/web-host#https-port):

**Chave**: https_port  
**Tipo**: *string*  
**Padrão**: um valor padrão não está definido.  
**Definido usando**: `UseSetting`  
**Variável de ambiente**: `<PREFIX_>HTTPS_PORT` (o prefixo é `ASPNETCORE_` ao usar o Host da Web.)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> A porta pode ser configurada indiretamente, definindo a URL com o `ASPNETCORE_URLS` variável de ambiente. A variável de ambiente configura o servidor e, em seguida, o middleware indiretamente descobre a porta HTTPS por meio de `IServerAddressesFeature`.

Se nenhuma porta for definida:

* As solicitações não são redirecionadas.
* O middleware registra o aviso "Falha ao determinar a porta https para redirecionamento."

> [!NOTE]
> Uma alternativa ao uso de Middleware de redirecionamento de HTTPS (`UseHttpsRedirection`) é usar o Middleware de reconfiguração de URL (`AddRedirectToHttps`). `AddRedirectToHttps` também pode definir o código de status e a porta quando o redirecionamento é executado. Para obter mais informações, consulte [Middleware de reconfiguração de URL](xref:fundamentals/url-rewriting).
>
> Ao redirecionar para HTTPS sem a necessidade de regras de redirecionamento adicional, é recomendável usar o Middleware de redirecionamento de HTTPS (`UseHttpsRedirection`) descrito neste tópico.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

O [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) é usada para exigir HTTPS. `[RequireHttpsAttribute]` pode decorar controladores ou métodos, ou podem ser aplicadas globalmente. Para aplicar o atributo globalmente, adicione o seguinte código ao `ConfigureServices` em `Startup`:

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

O código realçado anterior requer que todas as solicitações usem `HTTPS`; portanto, as solicitações HTTP são ignoradas. O seguinte código realçado redireciona todas as solicitações HTTP para HTTPS:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Para obter mais informações, consulte [Middleware de reconfiguração de URL](xref:fundamentals/url-rewriting). O middleware também permite que o aplicativo para definir o código de status ou o código de status e a porta quando o redirecionamento é executado.

Exigir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) é uma prática recomendada de segurança. Aplicando o `[RequireHttps]` atributo a todas as páginas do Razor/controladores não é considerado tão seguro quanto exigir HTTPS globalmente. Você não pode garantir o `[RequireHttps]` atributo é aplicado quando novos controladores e páginas Razor são adicionadas.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>Protocolo de segurança de transporte estrito HTTP (HSTS)

Por [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [segurança de transporte estrito HTTP (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) é um aprimoramento de segurança opcional é especificado por um aplicativo web com o uso de um cabeçalho de resposta. Quando um [navegador que ofereça suporte a HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) recebe esse cabeçalho:

* O navegador armazena a configuração para o domínio que evita o envio de qualquer comunicação por HTTP. O navegador força todas as comunicações via HTTPS.
* O navegador impede que o usuário usando os certificados não confiáveis ou é inválidos. O navegador desativa prompts que permitem que um usuário para confiar temporariamente esses certificados.

Como HSTS é imposta pelo cliente, ele tem algumas limitações:

* O cliente deve oferecer suporte a HSTS.
* HSTS requer pelo menos uma solicitação HTTPS bem-sucedida para estabelecer a política HSTS.
* O aplicativo deve verificar todas as solicitações HTTP e redirecionar ou rejeitar a solicitação HTTP.

ASP.NET Core 2.1 ou posterior implementa HSTS com o `UseHsts` método de extensão. O código a seguir chama `UseHsts` quando o aplicativo não está no [modo de desenvolvimento](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` não é recomendado em desenvolvimento porque as configurações de HSTS são altamente armazenáveis em cache por navegadores. Por padrão, `UseHsts` exclui o endereço de loopback local.

Para ambientes de produção implementando HTTPS pela primeira vez, defina o valor inicial de HSTS para um valor pequeno. Defina o valor de horas como não mais do que um único dia caso você precise reverter a infraestrutura HTTPS para HTTP. Depois que você estiver confiante em sustentabilidade da configuração do HTTPS, aumente o valor de idade máxima HSTS; um valor comumente usado é um ano. 

O código a seguir:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Define o parâmetro de pré-carregamento do cabeçalho de segurança de transporte estrito. Pré-carregamento não faz parte dos [especificação RFC HSTS](https://tools.ietf.org/html/rfc6797), mas é compatível com navegadores da web para pré-carregar sites HSTS na nova instalação. Veja [https://hstspreload.org/](https://hstspreload.org/) para obter mais informações.
* Habilita [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), que se aplica a política HSTS para hospedar subdomínios. 
* Define explicitamente o parâmetro de idade máxima do cabeçalho de segurança de transporte estrito para 60 dias. Se não for definido, o padrão é 30 dias. Consulte a [max-age diretiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) para obter mais informações.
* Adiciona `example.com` à lista de hosts a serem excluídos.

`UseHsts` Exclui os seguintes hosts de loopback:

* `localhost` : O endereço de loopback do IPv4.
* `127.0.0.1` : O endereço de loopback do IPv4.
* `[::1]` : O endereço de loopback do IPv6.

O exemplo anterior mostra como adicionar outros hosts.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>Recusa de HTTPS na criação do projeto

Os modelos de aplicativos do ASP.NET Core web 2.1 ou posterior (do Visual Studio ou a linha de comando do dotnet) habilitam [redirecionamento a HTTPS](#require) e [HSTS](#hsts). Para implantações que não exijam HTTPS, você pode recusar de HTTPS. Por exemplo, alguns serviços de back-end em que HTTPS está sendo manipulado externamente na borda, usando HTTPS em cada nó não é necessária.

A recusa de HTTPS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Desmarque a **configurar para HTTPS** caixa de seleção.

![Diagrama de entidade](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli) 

Use a opção `--no-https`. Por exemplo

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a>Como configurar um certificado de desenvolvedor para o Docker

Ver [esse problema de GitHub](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end

## <a name="additional-information"></a>Informações adicionais

* [Suporte a navegador HSTS OWASP](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
