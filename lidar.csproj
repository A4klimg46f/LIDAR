using System;
using System.Diagnostics;
using System.IO;

class LidarProcessing
{
    static void Main(string[] args)
    {
        // Caminho para o arquivo LiDAR de entrada e saída
        string inputFile = "input.las";
        string outputFile = "output_cleaned.las";

        // Verificar se o arquivo de entrada existe
        if (!File.Exists(inputFile))
        {
            Console.WriteLine("Arquivo LAS de entrada não encontrado!");
            return;
        }

        // Criar o pipeline JSON para limpeza e extração de feições
        string pipelineJson = $@"
        {{
            ""pipeline"": [
                ""{inputFile}"",
                {{
                    ""type"": ""filters.outlier"",
                    ""method"": ""statistical"",
                    ""mean_k"": 8,
                    ""multiplier"": 2.5
                }},
                {{
                    ""type"": ""filters.range"",
                    ""limits"": ""Classification![7:7],Z[0:1000]""
                }},
                {{
                    ""type"": ""filters.hag_delaunay""
                }},
                ""{outputFile}""
            ]
        }}";

        // Salvar o pipeline JSON em um arquivo temporário
        string pipelineFile = "pipeline.json";
        File.WriteAllText(pipelineFile, pipelineJson);

        // Executar o PDAL pipeline usando o sistema operacional
        string pdalCommand = $"pdal pipeline {pipelineFile}";

        try
        {
            Console.WriteLine("Iniciando processamento...");
            ProcessStartInfo processInfo = new ProcessStartInfo("cmd.exe", "/C " + pdalCommand)
            {
                RedirectStandardOutput = true,
                UseShellExecute = false,
                CreateNoWindow = true
            };

            using (Process process = Process.Start(processInfo))
            {
                string output = process.StandardOutput.ReadToEnd();
                process.WaitForExit();

                Console.WriteLine("Processamento concluído.");
                Console.WriteLine(output);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Erro ao executar PDAL: {ex.Message}");
        }
        finally
        {
            // Limpar o arquivo temporário do pipeline
            if (File.Exists(pipelineFile))
            {
                File.Delete(pipelineFile);
            }
        }

        Console.WriteLine($"Arquivo LiDAR processado salvo em: {outputFile}");
    }
}
