# Fato ou Farsa — Backend

**Plataforma de Auditoria e Verificação de Conteúdos do TikTok**

> Dossiê técnico do MVP Backend desenvolvido em Python e Django 5 para o Trabalho de Conclusão de Curso (TCC).

| Informação | Descrição |
|---|---|
| **Protocolo** | `FOF-MVP-DJANGO-2026-AL82G0` |
| **Versão** | MVP Backend |
| **Linguagem** | Python 3.12 |
| **Framework** | Django 5.x |
| **API REST** | Django REST Framework |
| **Banco de dados** | SQLite |
| **Arquitetura** | Backend REST integrado a uma aplicação React SPA |
| **Emissão da especificação** | 09/11/2026 |

---

## 1. Sobre o Projeto

O **Fato ou Farsa** é uma plataforma acadêmica de auditoria digital desenvolvida com o objetivo de avaliar a credibilidade e a integridade de conteúdos publicados no TikTok.

O sistema combina análise estatística, auditoria de métricas públicas e verificação de alegações factuais com inteligência artificial, buscando identificar possíveis inconsistências em perfis e publicações.

A camada de backend foi desenvolvida em **Python utilizando Django 5 e Django REST Framework**, priorizando organização, persistência de dados, integridade relacional e capacidade de validação por meio de testes automatizados.

### Objetivos técnicos

- Analisar métricas públicas de perfis e vídeos.
- Identificar padrões estatísticos atípicos por meio do método IQR.
- Calcular indicadores de engajamento e desempenho.
- Verificar alegações factuais com apoio de inteligência artificial.
- Registrar investigações e evidências em banco de dados.
- Disponibilizar endpoints REST para integração com o frontend.

## 2. Arquitetura do Backend

A aplicação utiliza uma arquitetura baseada em Django, separando as responsabilidades de modelagem, serialização, processamento e disponibilização dos dados.

**Tecnologias principais:**

| Tecnologia | Responsabilidade |
|---|---|
| Python 3.12 | Linguagem principal |
| Django 5.x | Estrutura do backend |
| Django REST Framework | Construção da API REST |
| SQLite | Persistência relacional |
| Django ORM | Manipulação e relacionamento dos dados |
| React SPA | Interface de comunicação com o usuário |

### Estrutura de diretórios

```text
fato-ou-farsa/
│
├── fato_backend/
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
│
├── verification/
│   ├── models.py
│   ├── serializers.py
│   ├── views.py
│   └── tests.py
│
├── manage.py
├── requirements.txt
└── README.md
```

Cada módulo possui uma responsabilidade específica:

- **models.py:** definição dos modelos e relacionamentos do banco de dados.
- **serializers.py:** serialização e validação dos dados utilizando DRF.
- **views.py:** implementação dos controladores e endpoints REST.
- **tests.py:** testes automatizados dos modelos, cálculos e funcionalidades.

## 3. Modelagem do Banco de Dados

A persistência dos dados utiliza o Django ORM, permitindo estruturar as informações das investigações em entidades relacionais.

### Principais entidades

**TikTokProfile**

Armazena os dados de identificação e as métricas públicas dos criadores de conteúdo.

- Username e nome de exibição.
- Biografia.
- Número de seguidores e seguindo.
- Curtidas acumuladas.
- Indicador de verificação oficial.
- Categoria temática.

**TikTokVideo**

Responsável pelo armazenamento das informações de cada publicação analisada.

- Visualizações (plays/views).
- Curtidas.
- Comentários.
- Compartilhamentos.
- Transcrição do áudio.
- Identificação de outliers.
- Pontuação de anomalia estatística.

**InvestigationReport**

Centraliza os resultados das análises realizadas pela plataforma.

- Média aritmética.
- Mediana.
- Desvio padrão.
- Relação entre visualizações e seguidores.
- Percentual de engajamento.
- Enquadramento da investigação.

**ClaimVerification e EvidenceSource**

Estruturam o processo de verificação de alegações factuais, relacionando afirmações analisadas às respectivas evidências e fontes de referência.

Entre as instituições e bases de consulta previstas estão:

- OMS — Organização Mundial da Saúde.
- ANVISA — Agência Nacional de Vigilância Sanitária.
- CFM — Conselho Federal de Medicina.
- CVM — Comissão de Valores Mobiliários.
- BACEN — Banco Central do Brasil.

## 4. Endpoints da API REST

A comunicação entre frontend e backend ocorre por meio de endpoints disponibilizados pelo Django REST Framework.

| Método | Endpoint | Funcionalidade |
|---|---|---|
| GET | `/api/status` | Verifica a disponibilidade e o estado da aplicação. |
| POST | `/api/analyze` | Executa uma auditoria de perfil ou vídeo. |
| POST | `/api/verify-claims` | Realiza a verificação de alegações factuais. |
| POST | `/api/calculate-stats` | Calcula indicadores e métricas estatísticas. |
| GET | `/api/history` | Recupera o histórico de investigações. |
| GET | `/admin/` | Acessa o painel administrativo do Django. |

### Descrição das operações

**Análise de conteúdo**

O endpoint `/api/analyze` centraliza o processo de auditoria, reunindo informações disponíveis, executando os cálculos estatísticos e registrando os resultados da investigação.

**Verificação de alegações**

O endpoint `/api/verify-claims` realiza a análise de afirmações factuais, relacionando-as a evidências e fontes de referência.

**Processamento estatístico**

O endpoint `/api/calculate-stats` disponibiliza os cálculos estatísticos de forma independente, permitindo avaliar indicadores sem necessariamente executar uma investigação completa.

**Histórico de investigações**

O endpoint `/api/history` permite consultar os registros armazenados no banco de dados.

## 5. Metodologia de Análise Estatística

Um dos pilares do projeto é a utilização de métodos estatísticos para identificar comportamentos atípicos nas métricas de publicações.

### Método IQR (Interquartile Range)

O método IQR é utilizado para identificar possíveis valores discrepantes (*outliers*) em conjuntos de dados.

O cálculo considera:

- Q1: primeiro quartil.
- Q3: terceiro quartil.
- IQR: diferença entre o terceiro e o primeiro quartil.

A identificação dos limites é realizada por:

```python
IQR = Q3 - Q1

limite_inferior = Q1 - (1.5 * IQR)
limite_superior = Q3 + (1.5 * IQR)
```

Valores que ultrapassam esses limites são classificados como possíveis outliers.

**Importante:** a identificação de um outlier não representa, isoladamente, evidência de fraude ou manipulação. O resultado indica apenas um comportamento estatisticamente atípico que pode exigir investigação complementar.

## 6. Integração e Conformidade com a API do TikTok

O projeto adota como princípio a utilização de meios autorizados para obtenção de informações, respeitando as diretrizes de privacidade e os termos de serviço da plataforma.

A arquitetura prevê integração com:

**TikTok Display API v2**

Integração oficial baseada em autenticação OAuth, destinada à obtenção de informações disponibilizadas conforme as permissões concedidas pelo usuário.

**TikTok Research API**

Possibilidade de integração futura voltada à pesquisa acadêmica, condicionada aos critérios de elegibilidade e aprovação estabelecidos pelo TikTok.

### Limitações de acesso

A plataforma não utiliza raspagem não autorizada (*scraping*) nem simula informações indisponíveis.

Quando não houver autorização ou acesso aos dados de terceiros, o sistema poderá oferecer alternativas de análise baseadas em informações fornecidas pelo usuário, como legendas, transcrições e alegações factuais.

## 7. Testes Automatizados

A validação do backend contempla uma suíte de testes unitários desenvolvida com o sistema nativo de testes do Django.

**Resultado documentado na especificação: 14/14 testes aprovados.**

### Cobertura dos testes

| Categoria | Validação |
|---|---|
| Modelos ORM | Criação de registros e relacionamentos em cascata. |
| Cálculos estatísticos | Detecção de outliers pelo método IQR. |
| Medianas | Cálculos para conjuntos pares e ímpares. |
| Pontuação de anomalia | Validação dos resultados estatísticos. |
| Normalização | Sanitização de URLs de perfis e vídeos. |
| Integridade numérica | Preservação de valores numéricos iguais a zero. |

### Executar os testes

```bash
python3 manage.py test verification
```

## 8. Instalação e Execução Local

### Pré-requisitos

- Python 3.12 ou superior compatível com as dependências.
- pip.
- Git.

### 1. Clonar o repositório

```bash
git clone <URL_DO_REPOSITORIO>
cd fato-ou-farsa
```

### 2. Criar o ambiente virtual

```bash
python -m venv .venv
```

Ativação no Windows:

```powershell
.venv\Scripts\activate
```

Ativação no Linux:

```bash
source .venv/bin/activate
```

### 3. Instalar as dependências

```bash
pip install -r requirements.txt
```

### 4. Aplicar as migrações

```bash
python manage.py migrate
```

### 5. Executar os testes

```bash
python manage.py test verification
```

### 6. Iniciar o servidor

```bash
python manage.py runserver 5001
```

Após iniciar, o backend estará disponível localmente em:

```text
http://127.0.0.1:5001/
```

O painel administrativo poderá ser acessado em:

```text
http://127.0.0.1:5001/admin/
```

## 9. Segurança e Variáveis de Ambiente

Credenciais, tokens OAuth e chaves de serviços externos não devem ser armazenados diretamente no código-fonte ou enviados ao GitHub.

Recomenda-se utilizar variáveis de ambiente para gerenciar informações sensíveis, mantendo arquivos locais de configuração fora do controle de versão.

Exemplo de arquivo `.env`:

```env
DJANGO_SECRET_KEY=sua_chave_secreta
DJANGO_DEBUG=True
TIKTOK_CLIENT_KEY=sua_client_key
TIKTOK_CLIENT_SECRET=sua_client_secret
```

O arquivo `.env` deve estar incluído no `.gitignore`.

## 10. Considerações Finais

O backend do Fato ou Farsa representa a estrutura responsável pelo processamento, armazenamento e disponibilização dos dados utilizados pela plataforma.

A utilização de Django e Django REST Framework permite organizar os processos de auditoria em uma arquitetura modular, com persistência relacional e recursos de validação automatizada.

A combinação de métodos estatísticos e verificação de alegações com inteligência artificial busca oferecer uma abordagem multidimensional para a análise de conteúdos digitais.

Por se tratar de um projeto acadêmico, os resultados devem ser interpretados considerando as limitações de acesso aos dados, a disponibilidade das fontes e os critérios metodológicos adotados.

---

## Identificação Técnica

**Projeto:** Fato ou Farsa — Plataforma de Auditoria e Verificação de Conteúdos do TikTok.

**Finalidade:** Trabalho de Conclusão de Curso (TCC).

**Responsável:** Pesquisador Acadêmico / TCC.

**Protocolo:** `FOF-MVP-DJANGO-2026-AL82G0`

**Hash SHA-256 da especificação original:**

```text
05fa2d7d1542cbd9aa071dafb505257ea37db00d84d8f9c77e1baa1b10618702
```

*Documento de especificação acadêmica e técnica — 2026.*

**Fato ou Farsa © 2026**
