<properties 
	pageTitle="Gerenciando ativos de Serviços de Mídia através de várias contas de armazenamento" 
	description="Este artigo orienta sobre como gerenciar ativos de Serviços de Mídia através de várias contas de armazenamento" 
	services="media-services" 
	documentationCenter="" 
	authors="Juliako" 
	manager="dwrede" 
	editor=""/>

<tags 
	ms.service="media-services" 
	ms.workload="media" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="01/28/2016" 
	ms.author="juliako"/>


#Gerenciando ativos de Serviços de Mídia através de várias contas de armazenamento

Começando com o Microsoft Azure Media Services 2.2, você pode anexar várias contas de armazenamento para uma única conta de Serviços de Mídia. A capacidade de anexar diversas contas de armazenamento a uma conta dos Serviços de Mídia oferece os seguintes benefícios:

- Balanceamento de carga seus ativos entre diversas contas de armazenamento.
- Dimensionamento dos Serviços de Mídia para grandes quantidades de processamento de conteúdo (já que, no momento, uma única conta de armazenamento tem um limite máximo de 500 TB). 

Este tópico demonstra como anexar várias contas de armazenamento a uma conta de Serviços de Mídia usando API REST do Gerenciamento de Serviços do Azure. Ele também mostra como especificar diferentes contas de armazenamento ao criar ativos usando o SDK dos Serviços de Mídia.

##Considerações

Ao anexar diversas contas de armazenamento para sua conta de Serviços de Mídia, aplicam-se as seguintes considerações:

- Todas as contas de armazenamento anexadas a uma conta dos Serviços de Mídia devem estar no mesmo data center que a conta de serviços de mídia.
- No momento, depois que uma conta de armazenamento é anexada à conta de Serviços de Mídia especificada, ele não pode ser desanexado.
- A conta de armazenamento principal é a indicado durante o tempo de criação de conta do Serviços de Mídia. No momento, não é possível alterar a conta de armazenamento padrão. 

Outras considerações:

Os Serviços de Mídia usam o valor da propriedade **IAssetFile.Name** ao compilar URLs para o conteúdo de streaming (por exemplo, http://{WAMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.). Por esse motivo, não é permitida a codificação por porcentagem. O valor da propriedade Name não pode ter nenhum dos seguintes [caracteres reservados para codificação de percentual](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#". Além disso, pode haver somente um '.' para a extensão de nome de arquivo.

##Para anexar uma conta de armazenamento com a API REST do Gerenciamento de Serviços do Azure

Atualmente, a única maneira de anexar diversas contas de armazenamento é usando [API REST do Gerenciamento de Serviços do Azure](http://msdn.microsoft.com/library/azure/dn167014.aspx). O exemplo de código no tópico [Como: Usar a API REST de gerenciamento dos Serviços de Mídia](https://msdn.microsoft.com/library/azure/dn167656.aspx) define o método **AttachStorageAccountToMediaServiceAccount** que anexa uma conta de armazenamento a conta de Serviços de Mídia especificada. O código no mesmo tópico define o método **ListStorageAccountDetails**, que lista todas as contas de armazenamento anexadas à conta dos Serviços de Mídia especificada.


##Gerenciar ativos de Serviços de Mídia através de várias contas de armazenamento

O código a seguir usa o SDK mais recente dos Serviços de Mídia para executar as seguintes tarefas:

1. Exibir todas as contas de armazenamento associadas à conta de Serviços de Mídia especificada.
1. Recuperar o nome da conta de armazenamento padrão
1. Criar um novo ativo na conta de armazenamento padrão
1. Criar um ativo de saída do trabalho de codificação na conta de armazenamento especificada.
	
		using Microsoft.WindowsAzure.MediaServices.Client;
		using System;
		using System.Collections.Generic;
		using System.Configuration;
		using System.IO;
		using System.Linq;
		using System.Text;
		using System.Threading;
		using System.Threading.Tasks;
		
		namespace MultipleStorageAccounts
		{
		    class Program
		    {
		        // Location of the media file that you want to encode. 
		        private static readonly string _singleInputFilePath =
		            Path.GetFullPath(@"../..\supportFiles\multifile\interview2.wmv");
		
		        private static readonly string MediaServicesAccountName = 
		            ConfigurationManager.AppSettings["MediaServicesAccountName"];
		        private static readonly string MediaServicesAccountKey = 
		            ConfigurationManager.AppSettings["MediaServicesAccountKey"];
		
		        private static CloudMediaContext _context;
		        private static MediaServicesCredentials _cachedCredentials = null;
	
		        static void Main(string[] args)
		        {
	
		            // Create and cache the Media Services credentials in a static class variable.
		            _cachedCredentials = new MediaServicesCredentials(
		                            MediaServicesAccountName,
		                            MediaServicesAccountKey);
		            // Used the cached credentials to create CloudMediaContext.
		            _context = new CloudMediaContext(_cachedCredentials);
	
		
		            // Display the storage accounts associated with 
		            // the specified Media Services account:
		            foreach (var sa in _context.StorageAccounts)
		                Console.WriteLine(sa.Name);
		
		            // Retrieve the name of the default storage account.
		            var defaultStorageName = _context.StorageAccounts.Where(s => s.IsDefault == true).FirstOrDefault();
		            Console.WriteLine("Name: {0}", defaultStorageName.Name);
		            Console.WriteLine("IsDefault: {0}", defaultStorageName.IsDefault);
		
		            // Retrieve the name of a storage account that is not the default one.
		            var notDefaultStroageName = _context.StorageAccounts.Where(s => s.IsDefault == false).FirstOrDefault();
		            Console.WriteLine("Name: {0}", notDefaultStroageName.Name);
		            Console.WriteLine("IsDefault: {0}", notDefaultStroageName.IsDefault);
		            
		            // Create the original asset in the default storage account.
		            IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.None, 
		                defaultStorageName.Name, _singleInputFilePath);
		            Console.WriteLine("Created the asset in the {0} storage account", asset.StorageAccountName);
		            
		            // Create an output asset of the encoding job in the other storage account.
		            IAsset outputAsset = CreateEncodingJob(asset, notDefaultStroageName.Name, _singleInputFilePath);
		            if(outputAsset!=null)
		                Console.WriteLine("Created the output asset in the {0} storage account", outputAsset.StorageAccountName);
		
		        }
		
		        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string storageName, string singleFilePath)
		        {
		            var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
		            
		            // If you are creating an asset in the default storage account, you can omit the StorageName parameter.
		            var asset = _context.Assets.Create(assetName, storageName, assetCreationOptions);
		
		            var fileName = Path.GetFileName(singleFilePath);
		
		            var assetFile = asset.AssetFiles.Create(fileName);
		
		            Console.WriteLine("Created assetFile {0}", assetFile.Name);
		
		            assetFile.Upload(singleFilePath);
		            
		            Console.WriteLine("Done uploading {0}", assetFile.Name);
		
		            return asset;
		        }
		
		        static IAsset CreateEncodingJob(IAsset asset, string storageName, string inputMediaFilePath)
		        {
		            // Declare a new job.
		            IJob job = _context.Jobs.Create("My encoding job");
		            // Get a media processor reference, and pass to it the name of the 
		            // processor to use for the specific task.
		            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
		
		            // Create a task with the encoding details, using a string preset.
		            ITask task = job.Tasks.AddNew("My encoding task",
		                processor,
		                "H264 Multiple Bitrate 720p",
		                Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.ProtectedConfiguration);
		
		            // Specify the input asset to be encoded.
		            task.InputAssets.Add(asset);
		            // Add an output asset to contain the results of the job. 
		            // This output is specified as AssetCreationOptions.None, which 
		            // means the output asset is not encrypted. 
		            task.OutputAssets.AddNew("Output asset", storageName,
		                AssetCreationOptions.None);
		
		            // Use the following event handler to check job progress.  
		            job.StateChanged += new
		                    EventHandler<JobStateChangedEventArgs>(StateChanged);
		
		            // Launch the job.
		            job.Submit();
		
		            // Check job execution and wait for job to finish. 
		            Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
		            progressJobTask.Wait();
		
		            // Get an updated job reference.
		            job = GetJob(job.Id);
		
		            // If job state is Error the event handling 
		            // method for job progress should log errors.  Here we check 
		            // for error state and exit if needed.
		            if (job.State == JobState.Error)
		            {
		                Console.WriteLine("\nExiting method due to job error.");
		                return null;
		            }
		
		            // Get a reference to the output asset from the job.
		            IAsset outputAsset = job.OutputMediaAssets[0];
		
		            return outputAsset;
		        }
		
		
		        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
		        {
		            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
		                ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
		
		            if (processor == null)
		                throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
		
		            return processor;
		        }
		
		        private static void StateChanged(object sender, JobStateChangedEventArgs e)
		        {
		            Console.WriteLine("Job state changed event:");
		            Console.WriteLine("  Previous state: " + e.PreviousState);
		            Console.WriteLine("  Current state: " + e.CurrentState);
		
		            switch (e.CurrentState)
		            {
		                case JobState.Finished:
		                    Console.WriteLine();
		                    Console.WriteLine("********************");
		                    Console.WriteLine("Job is finished.");
		                    Console.WriteLine("Please wait while local tasks or downloads complete...");
		                    Console.WriteLine("********************");
		                    Console.WriteLine();
		                    Console.WriteLine();
		                    break;
		                case JobState.Canceling:
		                case JobState.Queued:
		                case JobState.Scheduled:
		                case JobState.Processing:
		                    Console.WriteLine("Please wait...\n");
		                    break;
		                case JobState.Canceled:
		                case JobState.Error:
		                    // Cast sender as a job.
		                    IJob job = (IJob)sender;
		                    // Display or log error details as needed.
		                    Console.WriteLine("An error occurred in {0}", job.Id);
		                    break;
		                default:
		                    break;
		            }
		        }
		
		        static IJob GetJob(string jobId)
		        {
		            // Use a Linq select query to get an updated 
		            // reference by Id. 
		            var jobInstance =
		                from j in _context.Jobs
		                where j.Id == jobId
		                select j;
		            // Return the job reference as an Ijob. 
		            IJob job = jobInstance.FirstOrDefault();
		
		            return job;
		        }
		    }
		}
 

##Roteiros de aprendizagem dos Serviços de Mídia

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Fornecer comentários

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!---HONumber=AcomDC_0204_2016-->