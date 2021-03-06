---
title: Verwalten von Streamingendpunkten mit .NET SDK | Microsoft Docs
description: Diese Thema zeigt, wie Sie Streamingendpunkte mithilfe des Azure-Portals verwalten.
services: media-services
documentationcenter: 
author: Juliako
writer: juliako
manager: erikre
editor: 
ms.assetid: 0da34a97-f36c-48d0-8ea2-ec12584a2215
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.translationtype: HT
ms.sourcegitcommit: 26c07d30f9166e0e52cb396cdd0576530939e442
ms.openlocfilehash: 2aecdd9337a74a27c8116ee14efac8103b3eb548
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---

# <a name="manage-streaming-endpoints-with-net-sdk"></a>Verwalten von Streamingendpunkten mit .NET SDK

>[!NOTE]
>Sehen Sie sich zunächst das Thema [Übersicht](media-services-streaming-endpoints-overview.md) an. Lesen Sie außerdem [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).

Der Code in diesem Thema zeigt, wie die folgenden Aufgaben mithilfe des Azure Media Services .NET SDK ausgeführt werden:

- Untersuchen des Standard-Streamingendpunkts.
- Erstellen/Hinzufügen eines neuen Streamingendpunkts.

    Es empfehlen sich ggf. mehrere Streamingendpunkte, wenn Sie planen, mit verschiedenen CDNs oder mit einem CDN und Direktzugriff zu arbeiten.

    > [!NOTE]
    > Abgerechnet werden nur ausgeführte Streamingendpunkte.
    
- Aktualisieren des Streamingendpunkts.
    
    Rufen Sie unbedingt die Update()-Funktion auf.

- Löschen des Streamingendpunkts.

    >[!NOTE]
    >Der standardmäßige Streamingendpunkt kann nicht gelöscht werden.

Weitere Informationen zum Skalieren des Streamingendpunkts finden Sie in [diesem](media-services-portal-scale-streaming-endpoints.md) Thema.

## <a name="create-and-configure-a-visual-studio-project"></a>Erstellen und Konfigurieren eines Visual Studio-Projekts

Richten Sie Ihre Entwicklungsumgebung ein, und füllen Sie die Datei „app.config“ mit Verbindungsinformationen, wie unter [Media Services-Entwicklung mit .NET](media-services-dotnet-how-to-use.md) beschrieben. 

## <a name="add-code-that-manages-streaming-endpoints"></a>Hinzufügen von Code, der Streamingendpunkte verwaltet
    
Ersetzen Sie den Code in „Program.cs“ durch den folgenden Code:

    using System;
    using System.Configuration;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.Live;

    namespace AMSStreamingEndpoint
    {
        class Program
        {
        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        private static CloudMediaContext _context = null;

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            var defaultStreamingEndpoint = _context.StreamingEndpoints.Where(s => s.Name.Contains("default")).FirstOrDefault();
            ExamineStreamingEndpoint(defaultStreamingEndpoint);

            IStreamingEndpoint newStreamingEndpoint = AddStreamingEndpoint();
            UpdateStreamingEndpoint(newStreamingEndpoint);
            DeleteStreamingEndpoint(newStreamingEndpoint);
        }

        static public void ExamineStreamingEndpoint(IStreamingEndpoint streamingEndpoint)
        {
            Console.WriteLine(streamingEndpoint.Name);
            Console.WriteLine(streamingEndpoint.StreamingEndpointVersion);
            Console.WriteLine(streamingEndpoint.FreeTrialEndTime);
            Console.WriteLine(streamingEndpoint.ScaleUnits);
            Console.WriteLine(streamingEndpoint.CdnProvider);
            Console.WriteLine(streamingEndpoint.CdnProfile);
            Console.WriteLine(streamingEndpoint.CdnEnabled);
        }

        static public IStreamingEndpoint AddStreamingEndpoint()
        {
            var name = "StreamingEndpoint" + DateTime.UtcNow.ToString("hhmmss");
            var option = new StreamingEndpointCreationOptions(name, 1)
            {
            StreamingEndpointVersion = new Version("2.0"),
            CdnEnabled = true,
            CdnProfile = "CdnProfile",
            CdnProvider = CdnProviderType.PremiumVerizon
            };

            var streamingEndpoint = _context.StreamingEndpoints.Create(option);

            return streamingEndpoint;
        }

        static public void UpdateStreamingEndpoint(IStreamingEndpoint streamingEndpoint)
        {
            if (streamingEndpoint.StreamingEndpointVersion == "1.0")
            streamingEndpoint.StreamingEndpointVersion = "2.0";

            streamingEndpoint.CdnEnabled = false;
            streamingEndpoint.Update();
        }

        static public void DeleteStreamingEndpoint(IStreamingEndpoint streamingEndpoint)
        {
            streamingEndpoint.Delete();
        }
        }
    }


## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie die Media Services-Lernpfade.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


