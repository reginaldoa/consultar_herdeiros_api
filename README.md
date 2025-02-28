# consultar_herdeiros_api
Buscando pessoas relacionadas através do CPF

# Consulta de Herdeiros com API Datastone

Este projeto consulta a API da **Datastone** para enriquecer dados de CPFs e CNPJs, retornando informações sobre herdeiros (familiares) associados a esses CPFs/CNPJs e gerando um novo arquivo Excel com essas informações.

## Funcionalidade

O código realiza as seguintes tarefas:

1. **Consulta à API Datastone**: Realiza uma consulta para CPFs ou CNPJs usando a API da Datastone.
2. **Enriquecimento dos dados**: Para cada CPF presente no arquivo Excel, o código consulta a API e coleta informações sobre familiares (herdeiros) associados.
3. **Geração de um novo arquivo Excel**: O arquivo original é enriquecido e um novo arquivo Excel é gerado com os dados sobre os relacionados(Nome e CPF).
4. Os relacionados podem ser pessoas da familia ou até mesmo vizinhos.
## Como usar

### Requisitos

Antes de executar o código, instale as dependências necessárias. Utilize o comando abaixo para instalar as bibliotecas `requests` e `pandas`:

```bash
pip install requests pandas




Passos
Prepare o arquivo Excel:

O arquivo Excel a ser processado deve estar no mesmo diretório do script, com o nome HERDEIROS.xlsx
O arquivo deve conter uma coluna chamada CPF com os CPFs a serem consultados.
Configuração da API:

O código requer uma chave de API da Datastone. Substitua o valor de "SEU_TOKEN" pela sua chave de API.



Estrutura do Arquivo Gerado

CPF TITULAR	                Nome TITULAR	                Nome RELACIONADO	                                  CPF RELACIONADO
XXX.XXX.XXX-XX	            João Silva	                   Maria Silva	                                      XXX.XXX.XXX-XX
XXX.XXX.XXX-XX              Carlos Souza	                 Ana Souza	                                        XXX.XXX.XXX-XX
