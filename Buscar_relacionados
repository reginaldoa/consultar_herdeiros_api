import requests
import pandas as pd
import os

# Função para consultar a API Datastone
def get_datastone_data(cpf):
    # Verifica se é um CPF ou CNPJ (número de 11 ou 14 dígitos)
    if len(str(cpf)) > 11:  # CNPJ
        url = f'https://api.datastone.com.br/v1/companies/?cnpj={cpf}&fields=all'
    else:  # CPF
        url = f'https://api.datastone.com.br/v1/persons/?cpf={cpf}&fields=all'
    
    # Carrega a chave da API do ambiente
    api_token = os.getenv('DATASSTONE_API_TOKEN') 
    
    # Adiciona o cabeçalho com a chave da API
    headers = {"Authorization": f"Token {api_token}"}

    try:
        # Faz a requisição GET para a API
        response = requests.get(url, headers=headers)
        response.raise_for_status()  # Vai levantar uma exceção se a requisição falhar
        # Retorna os dados da resposta em formato JSON
        return response.json()
    except requests.exceptions.RequestException as e:
        # Em caso de erro, imprime a mensagem de erro
        print(f"Erro ao consultar Datastone para CPF/CNPJ {cpf}: {e}")
        return None

# Função para ler o arquivo Excel, consultar a API e gerar o novo Excel com dados relacionados
def consultar_herdeiros():
    # Carrega o arquivo Excel
    df = pd.read_excel("HERDEIROS.xlsx")
    
    # Verifica se a coluna de CPF existe
    if "CPF" not in df.columns:
        print("A coluna 'CPF' não foi encontrada no arquivo Excel.")
        return
    
    # Cria uma lista para armazenar os dados do novo DataFrame
    novos_dados = []

    # Itera sobre os CPFs na coluna 'CPF'
    for cpf in df["CPF"]:
        print(f"Consultando dados para o CPF: {cpf}")
        dados_enriquecidos = get_datastone_data(cpf)

        if dados_enriquecidos:
            print(f"Dados obtidos para o CPF {cpf}:")
            # Nome do titular
            nome_titular = dados_enriquecidos[0]['name']
            # Extrai os CPFs e nomes dos relacionados
            for pessoa in dados_enriquecidos[0].get('family_persons', []):
                nome_relacionado = pessoa.get('name', None)
                cpf_relacionado = pessoa.get('cpf', None)

                # Adiciona as informações ao novo DataFrame
                novos_dados.append({
                    'CPF TITULAR': cpf,
                    'Nome TITULAR': nome_titular,
                    'Nome RELACIONADO': nome_relacionado,
                    'CPF RELACIONADO': cpf_relacionado
                })
        else:
            print(f"Sem dados encontrados para o CPF {cpf}.")
            novos_dados.append({
                'CPF TITULAR': cpf,
                'Nome TITULAR': None,
                'Nome RELACIONADO': None,
                'CPF RELACIONADO': None
            })
    
    # Cria um novo DataFrame com os dados processados
    novo_df = pd.DataFrame(novos_dados)
    
    # Salva o DataFrame modificado em um novo arquivo Excel
    novo_df.to_excel("HERDEIROS_COM_RELACIONADOS_FORMATADO.xlsx", index=False)
    print("Novo arquivo Excel gerado: HERDEIROS_COM_RELACIONADOS_FORMATADO.xlsx")

# Chama a função para consultar os herdeiros e gerar o novo arquivo Excel
consultar_herdeiros()
