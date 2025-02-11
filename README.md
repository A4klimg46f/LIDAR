## Explicação do Código
1. Input e Output: O código lê um arquivo LAS (input.las) e salva o arquivo processado como output_cleaned.las.
2. Pipeline PDAL:
   - filters.outlier: Remove pontos fora de faixa utilizando o método estatístico.
   - filters.range: Filtra pontos com elevação fora do intervalo especificado ou com classificação indesejada.
   - filters.hag_delaunay: Calcula a altura acima do terreno usando triangulação de Delaunay.
3. Execução do Pipeline: O código cria um arquivo JSON com a configuração do pipeline e executa o comando via linha de comando utilizando pdal pipeline.

## Requisitos do Ambiente
  - O PDAL deve estar instalado e configurado no ambiente (com PATH ajustado para executá-lo via terminal).
  - Você pode adaptar os filtros para outras tarefas, como extração de feições específicas, utilizando outros filtros do PDAL, como filters.smrf para segmentação de terreno.

# Processamento de Dados LiDAR em C# com PDAL
Este projeto fornece um script em C# para realizar a limpeza e extração de feições de dados LiDAR (formato LAS/LAZ) utilizando a biblioteca PDAL (Point Data Abstraction Library).
O código permite realizar o pré-processamento básico, incluindo remoção de outliers, filtragem por elevação e cálculo de altura acima do terreno.
## Requisitos
1. Dependências:
   - PDAL instalado no sistema (https://pdal.io).
   - Ambiente de desenvolvimento configurado para executar C# (.NET Core ou Framework).
   - Arquivo de entrada no formato .las ou .laz.
2. Bibliotecas e Configurações
   - O PDAL deve estar configurado no PATH do sistema operacional para execução via terminal.
   - Arquivos de entrada e saída devem ser acessíveis no diretório onde o código é executado.
## Funcionalidades do Código
Este script realiza as seguintes etapas no processamento de dados LiDAR:
1. Leitura do Arquivo LiDAR: Lê o arquivo no formato .las (ou .laz se o PDAL tiver suporte).
2. Limpeza de Dados
   - Remove pontos fora de faixa (outliers) utilizando o filtro estatístico.
   - Filtra os dados com base na elevação (Z) e classificação, mantendo apenas pontos desejados.
3. Cálculo de Feições:
   - Calcula a altura acima do solo utilizando triangulação de Delaunay.
4. Exportação de Dados:
   - Salva o arquivo processado em um novo arquivo .las.
## Como Usar
1. Prepare o Ambiente:
   - Certifique-se de que o PDAL está instalado e acessível via linha de comando.
   - Insira o arquivo LiDAR de entrada (ex.: input.las) no mesmo diretório do script ou forneça o caminho correto.
2. Execute o Script:
   - Compile o código utilizando o .NET CLI ou Visual Studio.
   - Execute o programa. O script criará um arquivo JSON temporário com o pipeline PDAL e o executará automaticamente.
3. Saída:
   - O arquivo processado será salvo como output_cleaned.las.
## Exemplo de Pipeline JSON
O pipeline JSON utilizado no script é gerado dinamicamente, mas segue o seguinte formato:

{
    "pipeline": [
        "input.las",
        {
            "type": "filters.outlier",
            "method": "statistical",
            "mean_k": 8,
            "multiplier": 2.5
        },
        {
            "type": "filters.range",
            "limits": "Classification![7:7],Z[0:1000]"
        },
        {
            "type": "filters.hag_delaunay"
        },
        "output_cleaned.las"
    ]
}


Este pipeline realiza:
- Remoção de outliers (método estatístico).
- Filtragem de pontos fora dos limites de elevação (Z[0:1000]) e classificação (removendo objetos indesejados).
- Cálculo da altura acima do terreno.
## Personalizações
- Para ajustar os limites de elevação, altere a linha "Z[0:1000]" no pipeline JSON.
- Adicione ou modifique filtros do PDAL de acordo com suas necessidades (ex.: filters.smrf para segmentação de terreno).
- Substitua os nomes dos arquivos de entrada e saída diretamente no código ou utilize argumentos de linha de comando para maior flexibilidade.

## Erros Comuns
1. PDAL não encontrado:
   - Certifique-se de que o PDAL está instalado e configurado no PATH do sistema operacional.
2. Arquivo LAS/LAZ inválido:
   - Verifique se o arquivo de entrada está no formato correto e acessível.
3. Permissões de Escrita
   - Garanta que o programa tenha permissão para salvar arquivos no diretório de saída.


  
     
