---
title: Použití centra v ASP.NET Core SignalR
author: rachelappel
description: Další informace o použití centra v ASP.NET Core SignalR.
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 73397ba6951f2752653575920bdf739439874366
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/03/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Použití centra v systému SignalR pro ASP.NET Core

Podle [Rachel Appel](https://twitter.com/rachelappel) a [kevina Griffin](https://twitter.com/1kevgriff)

## <a name="what-is-a-signalr-hub"></a>Co je rozbočovače SignalR

Rozhraní API rozbočovače SignalR umožňuje volat metody pro připojené klienty ze serveru. V serverovém kódu definujete metody, které se nazývají klientem. V klientském kódu definujete metody, které se nazývají ze serveru. SignalR postará vše na pozadí, které umožňuje v reálném čase komunikaci klienta se serverem a server klient.

## <a name="configure-signalr-hubs"></a>Konfigurace rozbočovače SignalR

SignalR middleware vyžaduje některé služby, které jsou nakonfigurované voláním `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=35)]

Při přidávání funkce SignalR do aplikace ASP.NET Core, instalační program SignalR trasy voláním `app.UseSignalR` v `Startup.Configure` metoda.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=55-58)]

## <a name="create-and-use-hubs"></a>Vytváření a používání centra

Vytvoření rozbočovač deklarováním třídy, která dědí z `Hub`a přidejte do ní veřejné metody. Klienti mohou volat metody, které jsou definovány jako `public`.

[!code-csharp[Create and use hubs](hubs/sample/chathub.cs?range=10-13)]

Můžete zadat návratový typ a parametry, včetně komplexní typy a pole, jako byste v libovolné metody C#. SignalR zpracovává serializace a deserializace komplexní objekty a pole v parametry a návratové hodnoty.

## <a name="the-clients-object"></a>Objekt klientů

Každá instance `Hub` třída má vlastnost s názvem `Clients` obsahující následující členy pro komunikaci mezi serverem a klientem:

| Vlastnost | Popis |
| ------ | ----------- |
| `All` | Volání metody na všech připojených klientů |
| `Caller` | Volání metody na straně klienta, který volá metodu rozbočovače |
| `Others` | Volání metody na všechny připojené klienty kromě klienta, která volá metodu |

Kromě toho `Hub` třída obsahuje následující metody:

| Metoda | Popis |
| ------ | ----------- |
| `AllExcept` | Volání metody na všechny připojené klienty s výjimkou zadaných připojení |
| `Client` | Volání metody na konkrétní připojený klient |
| `Clients` | Volání metody na konkrétní připojené klienty |
| `Group` | Odešle zprávu do všech připojení v určené skupině  |
| `GroupExcept` | Odešle zprávu do všech připojení v určené skupině, s výjimkou zadaných připojení |
| `Groups` | Odešle zprávu do více skupin pro připojení  |
| `OthersInGroup` | Odešle zprávu do připojení s výjimkou klienta, která volá metodu rozbočovače na skupinu  |
| `User` | Odešle zprávu do všech připojení spojené s konkrétním uživatelem |
| `Users` | Odešle zprávu do všech připojení přidružené k určení uživatelé |

Každé vlastnosti nebo metody v předchozích tabulkách vrátí objekt s `SendAsync` metoda. `SendAsync` Metoda umožňuje zadat název a parametry metody klienta k volání.

## <a name="send-messages-to-clients"></a>Odesílat zprávy pro klienty

Volání na konkrétní klienty, použijte vlastnosti `Clients` objektu. V následujícím v následujícím příkladu `SendMessageToCaller` metoda ukazuje odesílání zprávy připojení, která volá metodu rozbočovače. `SendMessageToGroups` Metoda odešle zprávu do skupin, které jsou uložené v `List` s názvem `groups`.

[!code-csharp[Send messages](hubs/sample/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>Zpracování událostí pro připojení

Poskytuje rozhraní API rozbočovače SignalR `OnConnectedAsync` a `OnDisconnectedAsync` virtuální metody ke správě a sledování připojení. Přepsání `OnConnectedAsync` virtuální metoda k provádění akcí, když se klient připojí k rozbočovači, jako je například přidávání do skupiny.

[!code-csharp[Handle events](hubs/sample/chathub.cs?range=26-30)]

## <a name="handle-errors"></a>Zpracování chyb

Výjimky vzniklé v metodách vašeho centra se posílají klientovi, který volá metodu. Na straně klienta JavaScript `invoke` metoda vrátí [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). Když klient obdrží chybu s obslužnou rutinou připojené pomocí promise `catch`, má vyvolat a předá jako JavaScriptu `Error` objektu.

[!code-javascript[Error](hubs/sample/chat.js?range=20)]
[!code-javascript[Error](hubs/sample/chat.js?range=16-18)]

## <a name="related-resources"></a>Související informační zdroje

[Úvod do základní ASP.NET SignalR](xref:signalr/introduction)