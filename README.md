# Tutorial: Processando Dados do Censo do MEC

Este repositório contém scripts Python para processar dados do censo do MEC, incluindo a leitura de um arquivo TXT, a geração de um arquivo Excel e a conversão desse arquivo Excel de volta para um formato de texto.
<br>

## Ferramentas Recomendadas

Você pode usar diversas ferramentas para executar os scripts Python deste repositório. Aqui estão algumas recomendações:

- **Google Colab**: Uma ferramenta gratuita baseada na nuvem que permite executar e compartilhar códigos em Python. Não requer instalação e vem com várias bibliotecas pré-instaladas.
  
  Para usar o Google Colab:
  1. Acesse [Google Colab](https://colab.research.google.com/).
  2. Faça o upload dos arquivos `Censo_2023_Aluno.txt` e `censo_alunos.xlsx` (este será gerado no processo).
  3. Copie e cole o código dos scripts em uma nova célula e execute.
<br>

- **Jupyter Notebook**: Um ambiente interativo que permite criar e compartilhar documentos que contêm código executável, equações, visualizações e texto narrativo.
  
  Para usar o Jupyter Notebook:
  1. Instale o Jupyter Notebook com `pip install notebook`.
  2. Inicie o Jupyter Notebook com `jupyter notebook` no terminal.
  3. Crie um novo notebook e copie e cole o código dos scripts nas células e execute.
<br>

- **Visual Studio Code (VSCode)**: Um editor de código-fonte poderoso e gratuito com suporte para Python através da extensão Python.

  *Pré-requisitos*

  Certifique-se de ter os seguintes itens instalados na sua máquina:
  - Python 3.x
  - Biblioteca Pandas (`pip install pandas`)
  - Biblioteca NumPy (`pip install numpy`)
  
  Para usar o VSCode:
  1. Baixe e instale o [VSCode](https://code.visualstudio.com/).
  2. Instale a extensão Python.
  3. Abra o VSCode no diretório do repositório clonado e crie arquivos `.py` para cada script.
  4. Execute os scripts diretamente no VSCode.
<br>

## Passos

### 1. Clonar o Repositório

Clone este repositório para sua máquina local usando o comando:

```bash
git clone https://github.com/thaisgarcia/convert-censo-file.git
cd convert-censo-file
```

### 2. Preparar os Arquivos
Certifique-se de que você tem os arquivos `Censo_2023_Aluno.txt` e `censo_alunos.xlsx` (este será gerado no processo) no diretório raiz do repositório clonado.

### 3. Gerar o Arquivo Excel a partir do Arquivo TXT
Este script lê o arquivo TXT, processa os dados, separando-os em duas tabelas com base no tipo de registro (41 e 42), e depois combina essas tabelas em um único DataFrame. Por fim, ele salva os dados combinados em um arquivo Excel.

```bash
import pandas as pd
import numpy as np

# Lendo arquivo TXT inicial e separando em duas tabelas de acordo com o número de registro
with open('Censo_2023_Aluno.txt', 'r') as txtfile:
    # Ignorando a primeira linha do arquivo TXT
    reader = txtfile.readlines()[1:]

    # Inicializando listas para armazenar linhas de 41 e 42
    linhas_41 = []
    linhas_42 = []

    # Iterando sobre as linhas do arquivo TXT
    for line in reader:
        line = line.strip().split('|')

        # Verificando se a linha começa com '41' ou '42' e adicionando à lista correspondente
        if line[0] == '41':
            linhas_41.append(line)
        elif line[0] == '42':
            linhas_42.append(line)

# Convertendo listas em DataFrames
df_censo41 = pd.DataFrame(linhas_41)
df_censo42 = pd.DataFrame(linhas_42)

# Encontrar linhas duplicadas com base na coluna: identificação única do aluno na IES
linhas_duplicadas = df_censo42[df_censo42.duplicated(1, keep=False)]

# Concatenando os DataFrames ao longo do eixo das colunas
df_combinado = pd.concat([df_censo41, df_censo42], axis=1)

nomes_colunas = ['Tipo de registro', 'ID do aluno no Inep', 'Nome', 'CPF', 'Documento de estrangeiro ou passaporte',
                   'Data de nascimento', 'Cor/raça', 'Nacionalidade', 'UF de nascimento', 'Município de nascimento',
                   'País de origem', 'Aluno com deficiência, transtorno do espectro autista (TEA), altas habilidades ou superdotação',
                   'Tipo de deficiência – cegueira',  'Tipo de deficiência – baixa visão e visão monocular', 'Tipo de deficiência – surdez',
                   'Tipo de deficiência – deficiência auditiva', 'Tipo de deficiência – deficiência física',
                   'Tipo de deficiência – surdocegueira', 'Tipo de deficiência – intelectual', 'Tipo de deficiência - Transtorno do espectro autista (TEA)',
                   'Tipo de deficiência – altas habilidades ou superdotação', 'Tipo de escola que concluiu o Ensino Médio',
                   'Tipo de registro', 'ID na IES', 'Período de referência', 'Código do curso', 'Código do polo do curso a distância',
                   'Turno do aluno', 'Situação de vínculo do aluno ao curso', 'Curso origem', 'Semestre de conclusão do curso',
                   'Aluno Parfor', 'Segunda Licenciatura / Formação pedagógica', 'Tipo - Segunda Licenciatura / Formação pedagógica',
                   'Semestre de ingresso no curso', 'Forma de ingresso/seleção – vestibular', 'Forma de ingresso/seleção – Enem',
                   'Forma de ingresso/seleção – avaliação seriada', 'Forma de ingresso/seleção – seleção simplificada', 'Forma de ingresso/seleção – Egresso BI/LI',
                   'Forma de ingresso/seleção – PEC-G', 'Forma de ingresso/seleção – transferência ex officio', 'Forma de ingresso/seleção – decisão judicial',
                   'Forma de ingresso – seleção para vagas remanescentes', 'Forma de ingresso – seleção para vagas de programas especiais',
                   'Mobilidade acadêmica', 'Tipo de mobilidade acadêmica', 'IES destino', 'País destino', 'Programa de reserva de vagas',
                   'Programa de reserva de vagas/ações afirmativas – étnico', 'Programa de reserva de vagas/ações afirmativas – pessoa com deficiência',
                   'Programa de reserva de vagas – estudante procedente de escola pública', 'Programa de reserva de vagas/ações afirmativas – social/renda familiar',
                   'Programa de reserva de vagas/ações afirmativas – outros', 'Financiamento estudantil', 'Financiamento estudantil reembolsável – Fies',
                   'Financiamento estudantil reembolsável – governo estadual', 'Financiamento estudantil reembolsável – governo municipal', 'Financiamento estudantil reembolsável – IES',
                   'Financiamento estudantil reembolsável – entidades externas', 'Tipo de financiamento não reembolsável – ProUni integral',
                   'Tipo de financiamento não reembolsável – ProUni parcial', 'Tipo de financiamento não reembolsável – entidades externas',
                   'Tipo de financiamento não reembolsável – governo estadual', 'Tipo de financiamento não reembolsável – IES',
                   'Tipo de financiamento não reembolsável – governo municipal', 'Apoio social', 'Tipo de apoio social – alimentação',
                   'Tipo de apoio social – moradia', 'Tipo de apoio social – transporte', 'Tipo de apoio social – material didático',
                   'Tipo de apoio social – bolsa trabalho', 'Tipo de apoio social – bolsa permanência', 'Atividade extracurricular', 'Atividade extracurricular – pesquisa',
                   'Bolsa/remuneração referente à atividade extracurricular – pesquisa', 'Atividade extracurricular – extensão', 'Bolsa/remuneração referente à atividade extracurricular – extensão',
                   'Atividade extracurricular – monitoria', 'Bolsa/remuneração referente à atividade extracurricular – monitoria',
                   'Atividade extracurricular – estágio não obrigatório', 'Bolsa/remuneração referente à atividade extracurricular – estágio não obrigatório',
                   'Carga horária total do curso por aluno', 'Carga horária integralizada pelo aluno', 'Justificativa']

# Escrevendo em um arquivo Excel
with pd.ExcelWriter('censo_alunos.xlsx') as writer:
    df_combinado.to_excel(writer, index=False, header=nomes_colunas)
```

### 4. Converter o Arquivo Excel de Volta para o Formato de Texto
Após fazer todas alterações necessárias no arquivo Excel execute esse script. O script lê o arquivo Excel, processa os dados e escreve um novo arquivo de texto (censo_alunos_atualizado.txt). Ele mantém a primeira linha do arquivo TXT original e organiza os registros de acordo com a presença do valor 42.

```bash
import pandas as pd

# Lendo o arquivo Excel
df = pd.read_excel('censo_alunos.xlsx')

# Escrevendo em um novo arquivo de texto
with open('censo_alunos_atualizado.txt', 'w') as txtfile:
    # Lendo a primeira linha do arquivo de texto original
    with open('Censo_2023_Aluno.txt', 'r') as oldfile:
        first_line = oldfile.readline().strip()
        txtfile.write(first_line)  

    # Iterando sobre as linhas do DataFrame
    for _, row in df.iterrows():
        # Verificando se o valor 42 está presente em alguma coluna da linha
        if 42 in row.values:
            txtfile.write('\n')  # Pula uma linha antes de inserir o registro 42
        
        # Escrevendo a parte inicial da linha no arquivo de texto
        row_initial = row.iloc[:22].fillna('').astype(str)
        row_initial.replace('   ', '', inplace=True)
        
        #Converter dados float para int
        row_initial = row_initial.apply(lambda x: int(float(x)) if str(x).replace('.', '').isdigit() else x)
        row_initial = row_initial.astype(str)
        row_initial_str = '|'.join(row_initial)
        txtfile.write(row_initial_str + '\n')

        # Escrevendo as colunas da posição 22 em diante na linha seguinte
        row_after_22 = row.iloc[22:].fillna('').astype(str)
        row_after_22.replace('   ', '', inplace=True)
        
        #Converter dados float para int
        row_after_22 = row_after_22.apply(lambda x: int(float(x)) if str(x).replace('.', '').isdigit() else x)
        row_after_22 = row_after_22.astype(str)
        row_after_22_str = '|'.join(row_after_22)
        txtfile.write(row_after_22_str)
```
