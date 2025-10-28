# 📍 Monitor de Licitações do PNCP

**Automatize o Monitoramento de Licitações de Obras em Todo o Brasil**

Um sistema robusto e escalável para monitorar licitações de obras públicas através da API do Portal Nacional de Contratações Públicas (PNCP) em tempo real, com filtros inteligentes, geolocalização e integração com ferramentas de gestão de tarefas.

---

## 🎯 Funcionalidades em Destaque

- **🔍 Busca Automatizada** - Consulta contínua da API PNCP para identificar novas licitações
- **🎯 Filtragem Inteligente** - Sistema de 3 níveis (serviço, contexto, exclusões) para refinar resultados
- **🗺️ Filtro Geográfico** - Busca por distância em relação a cidades de interesse (ex: 200km, 300km)
- **📊 Geração de Relatórios** - Exportação em JSON, CSV e XLSX para análise
- **📈 Informações Detalhadas** - Acesso aos dados completos de cada licitação
- **🔗 Integração Extensível** - Suporte para ClickUp, webhooks e outras ferramentas
- **⏰ Monitoramento em Tempo Real** - Alertas para novas oportunidades conforme surgem

---

## 💼 Casos de Uso Reais

- **Monitoramento Regional** - Acompanhe 50+ cidades em raio de 300km
- **Alertas por Setor** - Receba notificações apenas de licitações relacionadas ao seu negócio
- **Integração com Task Managers** - Crie automaticamente tarefas no ClickUp
- **Análise Histórica** - Acesse dados históricos para identificar padrões e tendências
- **Relatórios Gerenciais** - Gere planilhas consolidadas para apresentações

---

## 📋 Pré-requisitos

Antes de começar, certifique-se de ter instalado:

- **Python 3.7+** - [Download Python](https://www.python.org/downloads/)
- **Node.js 14+** (opcional, apenas para backend) - [Download Node.js](https://nodejs.org/)
- **Git** - [Download Git](https://git-scm.com/)
- **pip** - Gerenciador de pacotes Python (incluído com Python)

---

## 🚀 Instalação Rápida (5 Passos)

### 1. Clonar o Repositório

```bash
git clone https://github.com/rodrigochavesoa/monitor_licitacoes.git
cd monitor_licitacoes
```

### 2. Criar Ambiente Virtual

**Windows (PowerShell):**
```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

**macOS/Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Instalar Dependências Python

```bash
pip install -r requirements.txt
```

> **Nota:** Este comando instalará todas as bibliotecas necessárias, incluindo `python-dotenv` para carregar configurações do arquivo `.env`.

### 4. Instalar Dependências Node.js (Opcional)

```bash
npm install
```

### 5. Configurar Variáveis de Ambiente

```bash
# Copiar template de ambiente
cp .env.example .env

# Editar o arquivo .env com seus valores
```

---

## ⚙️ Guia de Configuração Detalhado

### 1. Preparar o Arquivo `.env`

Abra o arquivo `.env` e configure as variáveis principais:

```env
# Dias a buscar no passado
DIAS_DE_BUSCA=1

# Estado alvo (sigla)
UF=BA

# Estado alvo (nome completo)
ESTADO=Bahia

# ClickUp (opcional)
CLICKUP_TOKEN=seu_token_aqui
CLICKUP_LIST_ID=seu_list_id_aqui

# Servidor backend
PORT=3001
```

### 2. Preparar Dados Geográficos

O projeto já inclui dados de exemplo de **Bahia** e **Sergipe**. Para usar dados de outro estado:

#### Opção A: Usar Dados Existentes (Bahia)
Nenhuma configuração necessária - os dados já estão prontos em `malhas_municipais/Bahia/`

#### Opção B: Gerar Dados para Novo Estado

```bash
# Executar script para identificar cidades próximas
python identificar_cidades.py
```

Este script:
- Lê os arquivos shapefile da pasta `malhas_municipais/<ESTADO>/`
- Gera `cidades_alvo.csv` com as cidades prioritárias
- Pode incluir filtros de distância (200km, 300km, etc.)

### 3. Personalizar Filtros de Busca

Os filtros de licitação são totalmente personalizáveis através do arquivo `.env`, **sem necessidade de editar código Python**. Isso permite adaptar o sistema para diferentes segmentos de negócio.

#### Como Funciona o Sistema de Filtros

O sistema utiliza **3 níveis de filtragem** para refinar os resultados:

**📋 Abra o arquivo `.env` e localize a seção de filtros:**

```env
# GRUPO_SERVICO: Palavras que descrevem o tipo de serviço/produto
# A licitação PRECISA conter pelo menos UMA destas palavras
GRUPO_SERVICO=pavimentação,reforma,construção,obra,infraestrutura

# GRUPO_CONTEXTO: Palavras que confirmam o contexto de contratação
# A licitação PRECISA conter pelo menos UMA destas palavras
GRUPO_CONTEXTO=engenharia,execução de,mão de obra

# GRUPO_EXCLUSAO: Palavras que descartam a licitação
# Se a licitação contiver QUALQUER uma destas palavras, será DESCARTADA
GRUPO_EXCLUSAO=consultoria,software,treinamento,veículo
```

#### Exemplos de Filtros para Diferentes Segmentos

**🏗️ Obras de Engenharia Civil (Padrão):**
```env
GRUPO_SERVICO=pavimentação,paralelepípedo,reforma,ampliação,construção,edificação,obra
GRUPO_CONTEXTO=engenharia,serviços de engenharia,execução de,mão de obra
GRUPO_EXCLUSAO=fotográficos,consultoria,software,material de consumo
```

**🛠️ Produtos/Equipamentos de Engenharia:**
```env
GRUPO_SERVICO=aquisição,fornecimento,compra,equipamento,material,ferramenta
GRUPO_CONTEXTO=material,produto,equipamento,fornecimento de,aquisição de
GRUPO_EXCLUSAO=serviço de,mão de obra,consultoria,obra,construção
```

**🏥 Saúde e Hospitalar:**
```env
GRUPO_SERVICO=medicamento,equipamento médico,material hospitalar,insumo,vacina
GRUPO_CONTEXTO=hospital,clínica,saúde,posto de saúde,unidade básica
GRUPO_EXCLUSAO=obra,construção,reforma,pavimentação
```

**💻 TI e Tecnologia:**
```env
GRUPO_SERVICO=software,hardware,sistema,aplicativo,equipamento de informática
GRUPO_CONTEXTO=tecnologia da informação,TI,sistema de,software de
GRUPO_EXCLUSAO=obra,construção,material de consumo
```

> **💡 Dica:** Sempre use palavras em **minúsculas** e separe-as por **vírgula** sem espaços extras.

#### Modalidades de Licitação (Filtros Avançados)

> **Nota:** O sistema busca apenas as modalidades mais comuns e relevantes para obras públicas:
> - Pregão Eletrônico
> - Concorrência Eletrônica
> - Concorrência Presencial
> - Dispensa de Licitação
> - Inexigibilidade

Essas modalidades são definidas na lista `MODALIDADES_INTERESSE` no início do arquivo `monitor_licitacoesv2.py`:

```python
MODALIDADES_INTERESSE = [
    6,  # Pregão - Eletrônico
    4,  # Concorrência - Eletrônica
    5,  # Concorrência - Presencial
    8,  # Dispensa de Licitação
    9   # Inexigibilidade
]
```

Existem outras modalidades disponíveis no PNCP, conforme o manual oficial. Se desejar incluir ou remover modalidades, basta editar essa lista conforme sua necessidade.

### Sistema de 3 Níveis - Fluxo Visual

```
Busca na API PNCP
        ↓
┌───────────────────────────────────┐
│ Nível 1: GRUPO_SERVICO            │
│ ✓ Contém "Reforma" OU             │
│ ✓ Contém "Construção"             │
└───────────────────────────────────┘
        ↓ (passa filtro)
┌───────────────────────────────────┐
│ Nível 2: GRUPO_CONTEXTO           │
│ ✓ É "Municipal" OU "Estadual"     │
└───────────────────────────────────┘
        ↓ (passa filtro)
┌───────────────────────────────────┐
│ Nível 3: GRUPO_EXCLUSAO           │
│ ✗ Não contém "Consultoria"        │
│ ✗ Não contém "Projeto"            │
└───────────────────────────────────┘
        ↓
   ✅ RESULTADO FINAL
```

---

## 📖 Como Usar

### Execução Básica

```bash
# Ativar ambiente virtual
.\venv\Scripts\Activate.ps1  # Windows
source venv/bin/activate     # macOS/Linux

# Executar monitor
python monitor_licitacoesv2.py
```

### Scripts Utilitários

#### 1. Converter Dados Geográficos
```bash
python shpforcsv.py
```
Converte arquivos shapefile para CSV

#### 2. Identificar Cidades por Proximidade
```bash
python identificar_cidades.py
```
Gera arquivo `cidades_alvo.csv` com cidades próximas

#### 3. Converter CSV para JSON
```bash
python csvforjson.py
```
Converte dados em CSV para formato JSON

#### 4. Converter Licitações
```bash
python conversao_conlicitacoes.py
```
Converte licitações exportadas do Portal Conlicitação para o formato aceito pelo sistema.

**Como funciona:**
1. Acesse o [Portal Conlicitação](https://conlicitacao.com.br) e faça login na sua conta (é necessário ter assinatura ativa).
2. Gere o boletim de licitações desejado e exporte o arquivo em formato `.xlsx` pelo próprio portal.
3. Coloque o arquivo `.xlsx` gerado na pasta `licitacoes_xlsx/` do projeto.
4. Execute o comando acima para converter o arquivo para `.json` no formato compatível com o sistema.
5. O arquivo `.json` gerado pode ser utilizado, por exemplo, para alimentar o painel web `wp7licitacoes.html`.

> ⚠️ **Importante:** O Portal Conlicitação exige assinatura para acessar e exportar os boletins. O sistema não faz download automático, apenas converte arquivos já exportados.

#### 5. Iniciar Servidor Backend
```bash
node server.js
```
Inicia servidor Express na porta configurada (padrão: 3001)

### Exemplo de Saída

Após executar o monitor, você verá:

```json
{
  "id": "123456",
  "orgao": "Prefeitura de Salvador",
  "objeto": "Reforma de escola",
  "valor": 150000.00,
  "data_publicacao": "2025-10-27",
  "data_limite": "2025-11-10",
  "status": "Aberta",
  "cidade": "Salvador",
  "distancia_km": 0.0,
  "url": "https://pncp.gov.br/..."
}
```

---

## 🔌 Integração com ClickUp (Opcional)

### Pré-requisitos

1. **Conta ClickUp** - [Criar conta grátis](https://clickup.com)
2. **API Token** - Gerar em https://app.clickup.com/api_token
3. **ID da Lista** - Encontrar em URL: `app.clickup.com/list/<LIST_ID>`

### Configuração

1. Abra o arquivo `.env` e configure:
```env
CLICKUP_TOKEN=pk_xxxxxxxxxxxxx
CLICKUP_LIST_ID=123456789
```

2. Execute o servidor:
```bash
node server.js
```

3. As novas licitações serão criadas automaticamente como tarefas no ClickUp

### Como Funciona

```
monitor_licitacoesv2.py (detecta nova licitação)
            ↓
    server.js (backend)
            ↓
    API ClickUp (cria tarefa)
            ↓
  ✅ Tarefa aparece no ClickUp
```

---

## 📁 Estrutura de Diretórios

```
monitor_licitacoes/
│
├── 📄 monitor_licitacoesv2.py          ⭐ Script principal
├── 📄 server.js                        ⭐ Backend opcional
├── 📄 identificar_cidades.py           Identificar cidades por distância
├── 📄 shpforcsv.py                     Converter shapefile para CSV
├── 📄 csvforjson.py                    Converter CSV para JSON
├── 📄 conversao_conlicitacoes.py       Converter licitações
│
├── .gitignore                          Configurações do Git
├── .env.example                        Template de variáveis
├── requirements.txt                    Dependências Python
├── package.json                        Dependências Node.js
├── readme.md                           📖 Este arquivo
│
├── data/                               📁 Dados gerados (JSON)
│   ├── .gitkeep
│   └── (boletins_*.json, etc)
│
├── licitacoes_xlsx/                    📁 Planilhas geradas
│   ├── .gitkeep
│   └── xlsx_processados/
│       └── .gitkeep
│
└── malhas_municipais/                  📁 Dados geográficos
    ├── Bahia/
    │   ├── BA_Municipios_2024.shp      Arquivo shapefile
    │   ├── BA_Municipios_2024.dbf
    │   ├── BA_Municipios_2024.shx
    │   ├── municipios_Bahia.csv        CSV de referência
    │   └── (mais arquivos shapefile)
    │
    └── Sergipe/
        ├── SE_Municipios_2024.shp
        └── municipios_Sergipe.csv
```

**Explicação de Diretórios:**

| Diretório | Propósito |
|-----------|----------|
| `data/` | Armazena JSONs com licitações capturadas |
| `licitacoes_xlsx/` | Armazena exportações em XLSX |
| `malhas_municipais/` | Dados geográficos (shapefiles, municipios) |

---

## �️ Identificar Cidades-Alvo por Proximidade

### 🎯 O que é o Script `identificar_cidades.py`?

Este script identifica automaticamente todas as cidades dentro de um raio específico a partir de uma cidade sede. **Permite buscar em múltiplos estados simultaneamente**, perfeito para monitorar oportunidades de licitação em uma região geográfica.

### ⚙️ Configuração

Abra o arquivo `identificar_cidades.py` e configure:

#### 1. Cidade Sede (sua empresa)
```python
ESTADO_SEDE = 'Bahia'
NOME_CIDADE_SEDE = 'Serrinha'
```

#### 2. Estados onde buscar cidades-alvo

**Um único estado:**
```python
ESTADOS_ALVO = ['Sergipe']
```

**Múltiplos estados:**
```python
ESTADOS_ALVO = ['Sergipe', 'Bahia', 'SaoPaulo']
```

#### 3. Raios de Busca
```python
RAIO_MIN_KM = 5          # Distância mínima
RAIO_MAX_KM = 400        # Distância máxima
LIMITE_CIDADES = None    # Sem limite (todas as cidades)
# ou
LIMITE_CIDADES = 50      # Apenas 50 cidades mais próximas
```

### 📂 Estrutura de Arquivos Necessária

Para cada estado que você quer buscar, **DEVE existir** esta estrutura:

```
malhas_municipais/
├── Bahia/
│   └── municipios_Bahia.csv
├── Sergipe/
│   └── municipios_Sergipe.csv
└── SaoPaulo/
    └── municipios_SaoPaulo.csv
```

### 📋 Formato do CSV (obrigatório)

Cada arquivo CSV **DEVE** conter estas colunas:
- `CD_MUN` - Código IBGE do município
- `NM_MUN` - Nome do município
- `latitude` - Latitude
- `longitude` - Longitude

**Exemplo:**
```csv
CD_MUN,NM_MUN,latitude,longitude
2930501,Serrinha,-11.665667104222422,-38.99379057060335
2930105,Salvador,-12.971388888888889,-38.501666666666665
```

### 🚀 Como Executar

```bash
# Ativar ambiente
.\venv\Scripts\Activate.ps1

# Executar
python identificar_cidades.py
```

### 📊 Arquivos Gerados

O script cria:

1. **Arquivo Consolidado:**
   ```
   malhas_municipais/cidades_alvo_consolidado.csv
   ```
   Contém: `codigo_ibge`, `nome_municipio`, `estado`, `distancia`

2. **Arquivos por Estado:**
   ```
   malhas_municipais/Sergipe/cidades_alvo.csv
   malhas_municipais/Bahia/cidades_alvo.csv
   ```
   Contém: `codigo_ibge`, `nome_municipio`

### ❓ FAQ - Script de Cidades

**P: E se colocar um estado que não tem arquivo CSV?**

R: O script detecta, mostra um aviso e continua com os outros estados:
```
⚠️  AVISO: Arquivo não encontrado. Pulando estado SaoPaulo.
```

**P: Posso adicionar novos estados depois?**

R: Sim! Basta:
1. Criar pasta em `malhas_municipais/<ESTADO>/`
2. Adicionar arquivo `municipios_<ESTADO>.csv` com as colunas corretas
3. Adicionar estado na lista `ESTADOS_ALVO`

**P: Posso usar a mesma cidade sede em estado diferente do alvo?**

R: **SIM!** Esse é exatamente o propósito:
- Sede em **Serrinha (Bahia)**
- Buscar cidades em **Sergipe, Pernambuco, Alagoas**, etc.

### 📝 Exemplos de Uso

**Exemplo 1: Apenas Sergipe**
```python
ESTADO_SEDE = 'Bahia'
NOME_CIDADE_SEDE = 'Serrinha'
ESTADOS_ALVO = ['Sergipe']
RAIO_MIN_KM = 5
RAIO_MAX_KM = 400
```

**Exemplo 2: Múltiplos Estados**
```python
ESTADOS_ALVO = ['Sergipe', 'Bahia', 'Pernambuco']
```

**Exemplo 3: Apenas 50 Cidades Mais Próximas**
```python
ESTADOS_ALVO = ['Sergipe', 'Bahia', 'Pernambuco']
RAIO_MIN_KM = 5
RAIO_MAX_KM = 500
LIMITE_CIDADES = 50
```

### 🛠️ Validações Automáticas

O script valida:
- ✅ Existência dos arquivos CSV
- ✅ Presença das colunas obrigatórias
- ✅ Coordenadas válidas
- ✅ Cidade sede existe no arquivo

---

## 🔧 Troubleshooting

### Problema: `ModuleNotFoundError: No module named 'requests'`

**Solução:**
```bash
pip install -r requirements.txt
```

### Problema: `ModuleNotFoundError: No module named 'dotenv'`

**Solução:**
```bash
pip install python-dotenv
```

### Problema: Filtros não estão funcionando / Nenhuma licitação encontrada

**Possíveis causas:**

1. **Filtros muito restritivos**
   - Verifique se as palavras no `.env` estão corretas e em minúsculas
   - Tente ampliar o `GRUPO_SERVICO` e `GRUPO_CONTEXTO`
   - Reduza ou remova palavras do `GRUPO_EXCLUSAO`

2. **Arquivo `.env` não existe**
   ```bash
   cp .env.example .env
   # Edite o .env com seus valores
   ```

3. **Sistema usando filtros padrão**
   - Se aparecer a mensagem "⚠️  AVISO: Filtros não encontrados no .env"
   - Verifique se o arquivo `.env` está na raiz do projeto
   - Confirme que as variáveis `GRUPO_SERVICO`, `GRUPO_CONTEXTO` e `GRUPO_EXCLUSAO` estão definidas

**Como testar seus filtros:**
```bash
# Ative o ambiente
.\venv\Scripts\Activate.ps1

# Execute o monitor e observe as mensagens
python monitor_licitacoesv2.py
```

### Problema: `FileNotFoundError: .env`

**Solução:**
```bash
pip install -r requirements.txt
```

### Problema: `FileNotFoundError: .env`

**Solução:**
```bash
cp .env.example .env
# Editar .env com seus valores
```

### Problema: API PNCP retorna erro 403

**Solução:**
- Aguarde alguns minutos (limite de taxa)
- Verifique conexão com internet
- Confirme que a API está disponível em https://pncp.gov.br

### Problema: ClickUp não cria tarefas

**Solução:**
1. Verifique se `CLICKUP_TOKEN` está correto
2. Verifique se `CLICKUP_LIST_ID` é válido
3. Teste o token: https://clickup.com/api
4. Verifique logs do servidor: `node server.js`

### Problema: Dados geográficos não funcionam

**Solução:**
1. Confirme que shapefile existe em `malhas_municipais/<ESTADO>/`
2. Execute `python shpforcsv.py` para converter
3. Verifique se `identificar_cidades.py` gera `cidades_alvo.csv`

---

## 📚 Referências Úteis

| Recurso | Link |
|---------|------|
| Portal PNCP | https://pncp.gov.br |
| Documentação API PNCP | https://www.gov.br/pncp/pt-br/central-de-conteudo/manuais |
| ClickUp API | https://clickup.com/api |
| Biblioteca Geopy | https://geopy.readthedocs.io |
| IBGE Dados Municipais | https://www.ibge.gov.br |
| Python Documentation | https://docs.python.org |
| Express.js Guide | https://expressjs.com |

---

## 📊 Exemplo de Uso Completo

### 1. Primeiras Execuções

```bash
# Ativar ambiente
.\venv\Scripts\Activate.ps1

# Executar primeira busca
python monitor_licitacoesv2.py

# Verificar resultados em data/
```

### 2. Configurar Integração com ClickUp

```bash
# Editar .env com seus tokens
code .env

# Iniciar servidor
node server.js

# Em outro terminal, rodar monitor
python monitor_licitacoesv2.py
```

### 3. Gerar Relatórios

```bash
# Converter dados para Excel
python csvforjson.py

# Arquivo estará em licitacoes_xlsx/
```

---

## 📄 Licença

Este projeto está licenciado sob a **MIT License** - veja o arquivo [LICENSE](LICENSE) para detalhes.

---

## 👨‍💼 Autor

Desenvolvido por **rodrigochavesoa**

- 🐙 GitHub: [@rodrigochavesoa](https://github.com/rodrigochavesoa)
- 📧 Email: [pontochavedesign@gmail.com](mailto:pontochavedesign@gmail.com)

---

## 🤝 Contribuições

Contribuições são bem-vindas! Para reportar bugs ou sugerir melhorias:

1. Abra uma [Issue](https://github.com/rodrigochavesoa/monitor_licitacoes/issues)
2. Faça um Fork do projeto
3. Crie uma branch para sua feature (`git checkout -b feature/MinhaFeature`)
4. Commit suas mudanças (`git commit -m 'Adiciona MinhaFeature'`)
5. Push para a branch (`git push origin feature/MinhaFeature`)
6. Abra um Pull Request

---

## ⭐ Curtiu o Projeto?

Se este projeto foi útil para você, deixe uma ⭐ no GitHub!

**[⭐ Deixe uma estrela no GitHub](https://github.com/rodrigochavesoa/monitor_licitacoes)**

---

## 📞 Suporte

Tem dúvidas? Abra uma [Discussion](https://github.com/rodrigochavesoa/monitor_licitacoes/discussions) ou [Issue](https://github.com/rodrigochavesoa/monitor_licitacoes/issues)

---

**Last Updated:** Outubro de 2025  
**Status:** ✅ Ativo e em desenvolvimento

Estrutura dos Arquivos
identificar_cidades.py: Script para encontrar as cidades-alvo.

monitor_licitacoesv2.py: Script principal que busca as licitações.

municipios_bahia.csv: Arquivo de dados com o nome e as coordenadas de todos os municípios da Bahia.

requirements.txt: Lista de bibliotecas Python necessárias.

cidades_alvo.txt: (Gerado) Arquivo com a lista das cidades a serem monitoradas.

licitacoes_encontradas_*.csv: (Gerado) Relatório com as licitações encontradas.
