# Repositório de Gerenciamento e Versionamento de Datasets (MLOps)
## Descrição
Este repositório funciona como um **gerenciador e centralizador central de conjuntos de dados anotados (datasets)** para projetos de Machine Learning. Ele simula o ponto de ingestão onde dados processados por uma equipe de anotação são recebidos, organizados e versionados, servindo como uma fonte única e confiável de dados limpos e rastreáveis para todo o fluxo de trabalho de ML.

### Principais Funcionalidades

O sistema utiliza o **Data Version Control (DVC)** para oferecer as seguintes operações específicas:
*   **Versionamento e Rastreamento**: Mantém um histórico completo e imutável de todas as versões dos datasets anotados.
*   **Organização e Manutenção**: Estrutura e armazena os dados de forma lógica e consistente para fácil acesso.
*   **Busca e Recuperação**: Permite encontrar, acessar e restaurar versões anteriores específicas dos dados de maneira precisa e eficiente.

### Público-Alvo Primário

Este programa foi concebido para atender, primordialmente:
*   **Estudantes de graduação e pós-graduação** em disciplinas como Ciência da Computação, Engenharia de Software, Engenharia de Machine Learning e áreas afins que envolvam projetos práticos de programação e pipeline de dados.
*   **Pesquisadores e profissionais de Machine Learning/MLOps** que necessitam de um ambiente controlado e didático para compreender, simular e praticar os fluxos de ingestão e controle de versão de dados em pipelines de ML.

### Natureza do Projeto

Este repositório é um **Módulo de Sistema em Desenvolvimento** (parte de uma suíte maior de projetos acadêmicos - PFP). Ele funciona como uma **prova de conceito parcial e simplificada**, demonstrando de maneira prática um componente essencial de um pipeline de MLOps: o repositório central e o controle de versão de dados.

### Ressalvas Importantes

Este projeto, assim como os demais da suíte PFP, é um **Minimum Viable Product (MVP)**. Ele representa uma **versão inicial e demonstrativa** do pipeline, desenvolvida com fins educacionais, e será **atualizada e incrementada gradualmente**. Portanto, os interessados devem estar cientes de que:
*   Trata-se de uma ferramenta de caráter **primariamente didático e demonstrativo**.
*   Pode não conter todos os recursos, otimizações e a robustez exigidos por um ambiente de produção.
*   A estrutura e o código estão sujeitos a evoluções e mudanças significativas ao longo do tempo.

## Visão de Projeto
Esta seção descreve a experiência dos usuários através de cenários. Seu objetivo é guiar o projeto, o uso e a evolução futura do software, servindo como uma bússola para manter o foco nas necessidades principais.

### Cenário Positivo 1: Rastreamento de Dados em um Fluxo de Trabalho Dinâmico
**Persona:** Alice, Engenheira de Machine Learning em uma empresa de tecnologia.
**Contexto:** Alice é responsável pela qualidade e rastreabilidade dos dados usados para treinar modelos de visão computacional. Diariamente, uma equipe de anotadores processa novos lotes de imagens brutas, gerando um volume crescente de dados anotados. O desafio de Alice é implementar um sistema que organize e preserve o histórico desses dados de forma eficiente, permitindo que qualquer versão específica seja recuperada para auditoria, *debug* ou retreinamento de modelos.

**Narrativa:** Diariamente, após receber o novo lote de anotações de sua equipe, Alice registra essas alterações no sistema. A ferramenta de versionamento identifica automaticamente os arquivos novos e modificados, criando uma espécie de "etiqueta digital" única para esse conjunto. Ela então sincroniza essas etiquetas com o histórico do projeto e envia os arquivos de dados em si para um armazenamento em nuvem dedicado. Tudo é registrado com uma mensagem clara sobre o conteúdo daquela entrega. Semanas depois, quando um modelo em produção começa a apresentar um comportamento inesperado, Alice suspeita de um lote de dados específico usado em seu treinamento. Ela revisa o histórico de entregas, identifica a versão exata dos dados daquele período e, com alguns cliques, solicita ao sistema que restaure localmente aquele conjunto específico. Em minutos, ela tem os dados exatos da época em suas mãos, conseguindo isolar e investigar a raiz do problema.

### Cenário Positivo 2: Infraestrutura Interna de Versionamento de Dados
**Persona**: Bob, Engenheiro de Machine Learning.
**Contexto**: A empresa de Bob opera sob rigorosas políticas de segurança que exigem que todos os dados, especialmente os anotados para treinamento de modelos, sejam armazenados exclusivamente dentro da infraestrutura interna. Não é permitido o uso de serviços de nuvem pública. Bob é responsável por criar uma solução segura e acessível que permita versionar, armazenar e compartilhar esses dados com as diversas equipes de pesquisa e desenvolvimento através da rede local.

**Narrativa**: Todo dia, Bob recebe novos conjuntos de dados que precisam ser integrados ao repositório principal. Sua missão é garantir que esse processo seja tão eficiente e confiável quanto seria em um serviço de nuvem, mas mantendo tudo dentro do data center da empresa. Para isso, ele implanta uma solução de armazenamento de código aberto que é totalmente compatível com a API do Amazon S3. Ao executar essa solução em um contêiner dentro da rede corporativa, ele cria um "bucket privado" interno. Essa abordagem oferece uma grande vantagem: todas as ferramentas e *scripts* que sua equipe já conhece e utiliza para interagir com a nuvem continuam funcionando perfeitamente, sem necessidade de alterações. Agora, com a solução em funcionamento, qualquer engenheiro ou cientista de dados na rede pode, de forma segura e controlada, enviar novas versões de dados ou recuperar versões específicas para seus experimentos, mantendo o fluxo de trabalho ágil e totalmente dentro dos limites da empresa.

### Cenário Negativo: A Armadilha da Rastreabilidade Garantida

**Persona**: Carlos, Cientista de Dados.
**Contexto**: Carlos integra uma equipe que mantém um pipeline complexo de dados. Um conjunto inicial de imagens brutas (`dataset_v1`) passa por várias etapas: primeiro é anotado, depois filtrado para remover exemplos ruins, e finalmente é balanceado para corrigir distribuições de classe. Cada etapa gera um novo conjunto de dados derivado (ex: `annotated_v1`, `filtered_v1`, `balanced_v1`), todos versionados com Git e DVC. A equipe confia que, ao versionar cada arquivo de dados, a reprodutibilidade está garantida.

**Narrativa**: Após alguns meses, um modelo treinado com o `balanced_v5` apresenta uma queda inexplicável de performance em uma classe específica. Para investigar, Carlos precisa recriar exatamente como o `balanced_v5` foi construído. Ele usa o DVC para baixar os arquivos corretos. No entanto, ele se depara com uma teia de dependências difícil de desvendar: o arquivo `balanced_v5.dvc` aponta apenas para o conjunto de dados final. Para entender quais regras de filtro foram aplicadas e com base em qual versão dos dados anotados, Carlos precisa manualmente rastrear o histórico de commits do Git, buscar mensagens que possam fazer sentido e cruzar os hashes dos `.dvc` files de cada etapa intermediária. Uma única mensagem de commit ambígua como "ajustes no pipeline" o faz perder horas. Ele percebe que, embora cada *snapshot* dos dados esteja perfeitamente preservado, o *processo* e a lógica que conectam esses snapshots – a linhagem ou proveniência dos dados – não está codificada de forma clara e automática pelo sistema básico de versionamento. A garantia de reprodutibilidade, portanto, depende fortemente da disciplina e da clareza manual da equipe em documentar cada passo fora do sistema, uma exigência que se mostra frágil com o tempo e a rotatividade de pessoas.

## Documentação Técnica do Projeto: Sistema de Versionamento de Dados para Machine Learning

### 1. Especificação de Requisitos
#### 1.1. Requisitos Funcionais
**RF01 - Versionamento de Conjuntos de Dados:** O sistema deve permitir o controle de versão de conjuntos de dados anotados, rastreando mudanças incrementais em arquivos de imagem e anotações correspondentes.

**RF02 - Armazenamento Remoto Otimizado:** Deve armazenar os dados versionados em um repositório remoto compatível com S3 (nuvem pública ou serviço interno), enviando apenas arquivos novos ou modificados.

**RF03 - Recuperação de Versões Específicas:** Deve possibilitar a recuperação precisa de qualquer versão histórica dos dados a partir de um ponto específico no histórico do código.

**RF04 - Integração com Git:** Deve funcionar em conjunto com Git, utilizando-o para versionar metadados (arquivos `.dvc`) enquanto armazena dados reais separadamente.

**RF05 - Pipeline Reprodutível:** Deve permitir a definição de pipelines de processamento de dados (`dvc.yaml` se aplicável) que garantam a reprodutibilidade das transformações aplicadas aos dados.

#### 1.2. Requisitos Não-Funcionais
**RNF01 - Desempenho com Grandes Volumes:** Deve lidar eficientemente com grandes volumes de dados (imagens, vídeos) sem sobrecarregar o repositório Git.

**RNF02 - Segurança e Controle de Acesso:** O sistema deve permitir a configuração e utilização de um serviço de armazenamento remoto compatível com S3, com flexibilidade para ser tanto um serviço de nuvem pública (e.g., AWS S3) quanto uma solução local para desenvolvimento e testes (e.g., S3-Ninja, conforme `docker-compose.yml`). Quando configurado para uso interno, deve operar dentro da infraestrutura da empresa, sem dependência de serviços de nuvem externa.

**RNF03 - Compatibilidade:** Deve ser compatível com ferramentas padrão do ecossistema MLOps e formatos comuns de anotação (COCO JSON, YOLO, Pascal VOC).

**RNF04 - Usabilidade para Equipes de ML:** A interface de linha de comando e fluxo de trabalho devem ser acessíveis para engenheiros e cientistas de dados com conhecimento básico de Git.

**RNF05 - Manutenibilidade:** A configuração do projeto deve ser clara e documentada para facilitar a colaboração e evolução contínua.

### 2. Arquitetura e Modelo de Dados

#### 2.1. Arquitetura do Sistema
O sistema adota uma arquitetura híbrida que integra:

*   **Git**: Responsável pelo controle de versão do código-fonte do projeto, scripts, arquivos de configuração e, crucialmente, os metadados do DVC (arquivos `.dvc` e `dvc.lock`). O `.gitignore` garante que arquivos grandes de dados não sejam versionados diretamente pelo Git.
*   **DVC (Data Version Control)**: Atua como uma camada de versionamento para grandes conjuntos de dados. Ele gerencia o rastreamento das pastas `data/` e seus conteúdos, criando ponteiros (`data.dvc`) no repositório Git que referenciam os dados reais armazenados separadamente.
*   **Repositório de Armazenamento Remoto Compatível com S3**: Este é o local onde os dados versionados pelo DVC são de fato armazenados. Para desenvolvimento e testes locais, o `docker-compose.yml` configura o `s3-ninja`, um serviço que emula a API S3, permitindo o trabalho offline. Em ambientes de produção, pode ser facilmente configurado para utilizar serviços de nuvem como AWS S3, Google Cloud Storage, Azure Blob Storage, ou soluções S3 internas.

#### 2.2. Fluxo de Dados

O fluxo de dados no sistema opera da seguinte forma:

```
[Dados Originais] → [Processamento/Anotação] → [Conteúdo da pasta data/] → [DVC Add da pasta data] → [DVC Push] → [Armazenamento Remoto (S3-Ninja/Cloud S3)]
       ↓                        ↓                             ↓                             ↓                  ↓                              ↓
   (Input)                (Transformação)              (Organização no projeto)       (Versionamento DVC)  (Upload para remoto)         (Persistência dos dados)
       ↓                        ↓                             ↓                             ↓                  ↓                              ↓
[raw/origens] ← [Ferramentas Externas] → [data/images/ & data/labels/] ← [data.dvc no Git] ← [Git Commit] ← [Metadados no Git]
                                                              ↑                                 │
                                                              └─────────────────────────────────┘
```

#### 2.3. Modelo de Dados e Estrutura de Pastas

O projeto está estruturado de forma a separar os dados versionados (na pasta `data/`) do código e das configurações.

*   `.dvc/`: Contém a configuração interna do DVC e seu cache de dados, permitindo que o DVC rastreie eficientemente os grandes arquivos sem comprometê-los ao Git.
*   `.dvcignore`: Define quais arquivos e diretórios dentro de um conjunto de dados DVC devem ser ignorados pelo próprio DVC.
*   `.git/`: É o repositório Git padrão, contendo todo o histórico de código e os metadados do DVC.
*   `.gitignore`: Lista arquivos e diretórios que o Git deve ignorar, como ambientes virtuais (`.venv/`), dados (`/data`, `labeled`, `raw`), e arquivos de cache, garantindo que apenas o código-fonte e os ponteiros DVC sejam versionados.
*   `.python-version`: Garante a utilização consistente da versão do Python 3.14 entre os desenvolvedores.
*   `.venv/`: O ambiente virtual Python, criado e gerenciado pelo `uv`, isolando as dependências do projeto.
*   `data/`: Esta pasta contém os dados (imagens em `images/` e anotações em `labels/`) que estão sob o controle de versão do DVC. O arquivo `data.yaml` dentro desta pasta pode conter configurações ou metadados adicionais para o dataset, que também é versionado junto com o restante da pasta `data/` via DVC.
*   `data.dvc`: Este arquivo, versionado pelo Git, atua como um ponteiro para a versão atual da pasta `data/` no armazenamento remoto.
*   `docker-compose.yml`: Essencial para configurar e iniciar o serviço `s3-ninja`, que oferece um ambiente S3 local para testes e desenvolvimento sem a necessidade de um serviço de nuvem real.
*   `pyproject.toml`: Define os metadados do projeto e as dependências Python, incluindo a especificação de que o DVC deve ser instalado com suporte para S3 (`dvc[s3]`).
*   `uv.lock`: Garante que todas as dependências do Python sejam instaladas com as versões exatas e hashs específicos, garantindo a reprodutibilidade do ambiente.

### 3. Modelo Funcional e Fluxos de Trabalho

#### 3.1. Fluxo Principal: Adição e Versionamento de Novos Dados
1.  **Preparação**: Novos dados de imagens e anotações são colocados nos diretórios `data/images` e `data/labels`, respectivamente.
2.  **Versionamento DVC**: O comando `dvc add data/` é executado para que o DVC rastreie as mudanças na pasta `data/` e atualize o arquivo `data.dvc`.
3.  **Upload para Remoto**: `dvc push` envia os dados (e seu histórico) para o repositório remoto configurado (S3-Ninja local ou S3 na nuvem).
4.  **Registro de Metadados Git**: As alterações no arquivo `data.dvc` (que agora aponta para a nova versão dos dados no remoto) são commitadas no Git.
5.  **Commit Git**: `git commit -m "1ª Versão dos dados"` registra o estado do código e dos ponteiros DVC.

#### 3.2. Fluxo de Recuperação de Versão Anterior
1.  **Identificação da Versão**: O usuário utiliza o histórico do Git (`git log`) para identificar o commit correspondente à versão desejada do conjunto de dados.
2.  **Checkout Git**: `git checkout <hash_do_commit>` restaura o código e o arquivo `data.dvc` para o estado daquele commit.
3.  **Recuperação de Dados DVC**: `dvc pull` baixa os dados correspondentes à versão indicada pelo `data.dvc` (restaurado no passo anterior) do armazenamento remoto para o cache local do DVC.
4.  **Restauração dos Dados**: O DVC restaura os dados baixados para a pasta `data/` no diretório de trabalho.

#### 3.3. Fluxo Colaborativo (Múltiplos Anotadores/Desenvolvedores)
1.  **Branching**: Cada membro da equipe trabalha em um branch Git separado para suas modificações de código e dados.
2.  **Versionamento Independente**: Em cada branch, os novos dados são adicionados e versionados com DVC, e os arquivos `.dvc` são commitados no Git local.
3.  **Merge/Rebase**: Quando as alterações são concluídas, os branches são integrados ao branch principal (e.g., `main`).
4.  **Resolução de Conflitos**: Conflitos nos arquivos `.dvc` (se diferentes versões dos dados foram rastreadas em branches distintos) devem ser resolvidos manualmente, escolhendo qual versão de dados prevalecerá.
5.  **Sincronização**: Após o merge, `dvc push` é executado para garantir que todos os dados versionados no branch principal estejam sincronizados com o armazenamento remoto.

### 4. Sobre o Código e Implementação

#### 4.1. Linguagens e Tecnologias
*   **Linguagem Principal**: Python 3.14
*   **Ferramentas Principais**:
    *   **DVC (Data Version Control)**: Versão `3.64.1`, para versionamento de dados.
    *   **Git**: Para versionamento de código e metadados DVC.
    *   **uv**: Gerenciador de pacotes e ambiente virtual
    *   **Docker/Docker Compose**: Para orquestração de serviços locais, notavelmente o `s3-ninja`.
*   **Armazenamento**:
    *   **S3-Ninja**: Para emular um serviço S3 localmente
    *   **Serviços de Nuvem S3**: Compatibilidade nativa com AWS S3, Google Cloud Storage, etc.

#### 4.2. Estrutura de Código e Arquivos Chave

O projeto está estruturado de forma a separar os dados versionados (na pasta `data/`) do código e das configurações.

*   `.dvc/`: Contém a configuração interna do DVC e seu cache de dados, permitindo que o DVC rastreie eficientemente os grandes arquivos sem comprometê-los ao Git.
*   `.dvcignore`: Define quais arquivos e diretórios dentro de um conjunto de dados DVC devem ser ignorados pelo próprio DVC.
*   `.git/`: É o repositório Git padrão, contendo todo o histórico de código e os metadados do DVC.
*   `.gitignore`: Lista arquivos e diretórios que o Git deve ignorar, como ambientes virtuais (`.venv/`), dados (`/data`, `labeled`, `raw`), e arquivos de cache, garantindo que apenas o código-fonte e os ponteiros DVC sejam versionados.
*   `.python-version`: Garante a utilização consistente da versão do Python 3.14 entre os desenvolvedores.
*   `.venv/`: O ambiente virtual Python, criado e gerenciado pelo `uv`, isolando as dependências do projeto.
*   `data/`: Esta pasta contém os dados (imagens em `images/` e anotações em `labels/`) que estão sob o controle de versão do DVC. O arquivo `data.yaml` dentro desta pasta pode conter configurações ou metadados adicionais para o dataset, que também é versionado junto com o restante da pasta `data/` via DVC.
*   `data.dvc`: Este arquivo, versionado pelo Git, atua como um ponteiro para a versão atual da pasta `data/` no armazenamento remoto.
*   `docker-compose.yml`: Essencial para configurar e iniciar o serviço `s3-ninja`, que oferece um ambiente S3 local para testes e desenvolvimento sem a necessidade de um serviço de nuvem real.
*   `pyproject.toml`: Define os metadados do projeto e as dependências Python, incluindo a especificação de que o DVC deve ser instalado com suporte para S3 (`dvc[s3]`).
*   `uv.lock`: Garante que todas as dependências do Python sejam instaladas com as versões exatas e hashs específicos, garantindo a reprodutibilidade do ambiente.

#### 4.3. Estratégia de Comentários e Documentação
*   **Comentários em Código Python**: Onde houver scripts Python, será utilizada a convenção de docstrings no formato Google Style para funções e classes públicas, e comentários inline para lógica complexa.
*   **Comentários em Arquivos de Configuração**: Comentários serão utilizados em arquivos como `docker-compose.yml` e `pyproject.toml` para explicar a finalidade de seções específicas.
*   **Documentação Inline**: O `README.md` é o principal documento de alto nível, e esta documentação técnica (wiki) complementa com detalhes de implementação.

#### 4.4. Diretivas de Execução e Dependências
*   **Gerenciamento de Dependências**: As dependências do projeto são especificadas em `pyproject.toml` e gerenciadas com `uv`, utilizando `uv.lock` para garantir versões exatas. A instalação é feita via `uv sync`.
*   **Configuração do Ambiente Virtual**: O ambiente virtual `.venv` é criado e ativado automaticamente pelo `uv` ao instalar as dependências.
*   **Serviço S3 Local**: Para inicializar o `s3-ninja` para uso local, o comando `docker-compose up -d s3-ninja` deve ser executado no diretório raiz do projeto. Isso criará um bucket S3 local para o DVC.
*   **Configuração do Remoto DVC**: O remoto S3 para o DVC é configurado via comandos DVC, apontando para o endpoint do `s3-ninja` (ou para um bucket S3 real). Exemplo: `dvc remote add -d myremote s3://<bucket-name>` e `dvc config remote.myremote.endpointurl <url-endpoint-s3>`.

#### 4.5. Boas Práticas Implementadas
1.  **Separação de Preocupações**: Código e dados são claramente separados, com o Git focado no código e o DVC nos dados.
2.  **Versionamento Completo**: Tanto o código quanto os dados são versionados, garantindo a reprodutibilidade de experimentos de ML.
3.  **Ambiente Controlado**: O uso de `.python-version`, `pyproject.toml` e `uv.lock` garante um ambiente de desenvolvimento consistente.
4.  **Flexibilidade de Armazenamento**: O `docker-compose.yml` permite um ambiente de desenvolvimento S3 local que pode ser facilmente migrado para a nuvem.
5.  **Nomeação Descritiva**: Arquivos e diretórios são nomeados de forma clara e intuitiva.

#### 4.6. Limitações Conhecidas e Considerações
*   **Tamanho do Histórico DVC**: O histórico completo dos dados é mantido no repositório remoto, o que pode consumir espaço considerável ao longo do tempo.
*   **Resolução de Conflitos DVC**: Embora DVC e Git facilitem o trabalho colaborativo, a resolução de conflitos em arquivos `.dvc` em cenários de múltiplos usuários modificando o mesmo conjunto de dados requer atenção manual.
*   **Curva de Aprendizado**: Usuários sem familiaridade com Git e DVC podem precisar de um tempo para se adaptar ao fluxo de trabalho de versionamento de dados.

# Manual de Utilização do Sistema de Versionamento de Dados para Machine Learning

Este manual foi desenvolvido para guiar cientistas e engenheiros de dados na utilização eficaz do sistema de versionamento de dados, que combina Git para código e metadados DVC, e DVC para o gerenciamento de grandes volumes de dados. Ele é consistente com a documentação técnica e arquitetural do projeto.

## 1. Configurando um Projeto Existente Localmente

Esta tarefa descreve como clonar um projeto existente e preparar seu ambiente local para começar a trabalhar com os dados versionados.

### Guia de Instruções:

Para **configurar e sincronizar um projeto existente em sua máquina local** faça:

1.  **Clone o repositório Git do projeto:**
    ```bash
    git clone <URL_DO_REPOSITORIO_GIT>
    cd <nome_do_diretorio_do_projeto>
    ```
    *   *Alternativa:* Se o repositório já foi clonado, certifique-se de estar no diretório raiz do projeto.
2.  **Verifique a versão do Python e ative o ambiente virtual:**
    *   O projeto utiliza Python 3.14 (conforme `.python-version`). Recomenda-se usar o `uv` para gerenciar o ambiente.
    *   Instale as dependências e ative o ambiente virtual:
        ```bash
        uv sync
        uv shell # Para ativar o ambiente na sessão atual
        ```
    *   *Alternativa (se uv não for usado):* Crie e ative um ambiente virtual Python com `python3.14 -m venv .venv` e `source .venv/bin/activate`. Em seguida, instale as dependências de `pyproject.toml` com `pip install -e ".[s3]"`.
3.  **Inicie o serviço S3-Ninja localmente (para desenvolvimento/testes):**
    *   Certifique-se de ter o Docker e Docker Compose instalados.
    ```bash
    docker-compose up -d s3-ninja
    ```
    Este comando iniciará o serviço S3-Ninja em segundo plano, tornando-o acessível na porta `9444` do seu localhost. Ele emula um bucket S3 para o DVC.
4.  **Recupere os dados versionados pelo DVC:**
    ```bash
    dvc pull
    ```
    Este comando fará o download dos dados reais do armazenamento remoto (neste caso, o S3-Ninja local) e os colocará na pasta `data/` do seu projeto, de acordo com o `data.dvc` atual.


### Exceções ou potenciais problemas:

*   **Se `git clone` falhar:**
    *   É porque: A URL do repositório está incorreta ou você não tem permissão de acesso.
    *   Então faça: Verifique a URL e suas credenciais de acesso ao Git.
*   **Se `uv sync` ou `uv shell` falharem:**
    *   É porque: O `uv` não está instalado, ou há problemas nas dependências em `pyproject.toml`/`uv.lock`.
    *   Então faça: Instale o `uv` se necessário (`pip install uv`) ou verifique a integridade dos arquivos de dependência.
*   **Se `docker-compose up -d s3-ninja` falhar:**
    *   É porque: O Docker não está em execução, o Docker Compose não está instalado, ou a porta `9444` já está em uso.
    *   Então faça: Inicie o Docker, instale o Docker Compose, ou edite o `docker-compose.yml` para usar uma porta diferente (ex: `9445:9000`) e ajuste o `endpointurl` do DVC de acordo.
*   **Se `dvc pull` falhar:**
    *   É porque: O serviço S3-Ninja não está em execução ou não foi configurado corretamente (o DVC não consegue se conectar ao remoto).
    *   Então faça: Certifique-se de que `docker-compose up -d s3-ninja` foi executado com sucesso. Verifique se o DVC está configurado para usar o S3-Ninja como endpoint remoto (`dvc remote list -v` e `dvc config remote.myremote.endpointurl`).

## 2. Adicionando Novos Dados ao Projeto

Esta tarefa descreve o processo de incorporar novos conjuntos de imagens e suas anotações ao sistema de versionamento.

### Guia de Instruções:

Para **adicionar novos dados (imagens e anotações) ao projeto e versioná-los** faça:

1.  **Organize os novos arquivos de dados:**
    *   Coloque as novas imagens na pasta `data/images/`.
    *   Coloque as anotações correspondentes na pasta `data/labels/`.
    *   *Importante:* Mantenha os nomes dos arquivos de imagem e anotação consistentes para fácil correspondência.
2.  **Instrua o DVC a rastrear as mudanças na pasta `data/`:**
    ```bash
    dvc add data/
    ```
    Este comando irá:
    *   Analisar a pasta `data/` e identificar quaisquer novos arquivos ou modificações.
    *   Mover os dados reais (se novos) para o cache interno do DVC (`.dvc/cache/`).
    *   Atualizar o arquivo `data.dvc` (na raiz do projeto) com um novo hash que reflete o estado atual da pasta `data/`.
3.  **Adicione as mudanças do DVC ao Git:**
    ```bash
    git add data.dvc
    ```
    Você está adicionando o *ponteiro* (`data.dvc`) ao Git, não os arquivos de imagem/anotação reais.
4.  **Commite as mudanças no Git:**
    ```bash
    git commit -m "Adiciona novo lote de dados para treinamento/avaliação"
    ```
    A mensagem do commit deve ser descritiva sobre o que foi adicionado.
5.  **Envie os dados reais para o armazenamento remoto do DVC:**
    ```bash
    dvc push
    ```
    Este comando enviará os arquivos de dados que o DVC moveu para seu cache local para o seu remoto S3 configurado. O DVC é otimizado para enviar apenas os dados que ainda não estão no remoto.
6.  **Envie as mudanças do Git para o repositório remoto Git:**
    ```bash
    git push origin main # ou o nome do seu branch de trabalho
    ```


### Exceções ou potenciais problemas:

*   **Se `dvc add data/` não reconhecer as mudanças esperadas:**
    *   É porque: Os arquivos podem não estar nos subdiretórios `data/images/` ou `data/labels/`, ou a estrutura está diferente do que o DVC espera. Verifique também se algum arquivo não está sendo ignorado pelo `.dvcignore`.
    *   Então faça: Confirme os caminhos dos arquivos. Se houver um novo subdiretório dentro de `data/` que precise ser versionado, certifique-se de que `data.dvc` abranja todo o conteúdo desejado.
*   **Se você esquecer de `git add data.dvc` após `dvc add data/`:**
    *   É porque: O Git não terá o registro da nova versão dos seus dados. Isso pode levar a inconsistências onde o seu código aponta para uma versão de dados, mas o repositório Git remoto aponta para outra.
    *   Então faça: Execute `git add data.dvc` e `git commit` antes de prosseguir com `dvc push` e `git push`.
*   **Se `dvc push` falhar ou for muito lento:**
    *   É porque: O serviço S3-Ninja pode estar com problemas de conexão ou seu link de internet está lento (se estiver usando um remoto S3 na nuvem).
    *   Então faça: Verifique a conectividade com o S3-Ninja (ou o serviço S3 na nuvem). Monitore o progresso com a barra do DVC ou use `dvc push -v` para detalhes.

## 3. Recuperando uma Versão Específica dos Dados

Esta tarefa é fundamental para reproduzir experimentos ou analisar dados de um ponto específico no tempo.

### Guia de Instruções:

Para **restaurar o conjunto de dados para uma versão anterior do projeto** faça:

1.  **Identifique a versão do projeto (commit Git) que você deseja restaurar:**
    *   Use `git log` para visualizar o histórico de commits. Observe as mensagens de commit que indicam a inclusão ou alteração de dados.
    *   Para filtrar commits que afetaram o DVC, use: `git log --oneline data.dvc`
    *   Anote o `hash` (identificador único) do commit desejado.
2.  **Faça o checkout do commit Git correspondente à versão desejada:**
    ```bash
    git checkout <hash_do_commit>
    ```
    *   Isso reverterá seu código e o arquivo `data.dvc` para o estado exato daquele commit. O conteúdo da sua pasta `data/` *não será alterado imediatamente* por este comando.
    *   *Se houver arquivos não commitados:* O Git pode pedir para você fazer commit ou descartar suas mudanças antes do checkout. Salve seu trabalho se necessário (`git stash` ou `git commit -am "WIP"`).
3.  **Restaurar os dados reais com o DVC:**
    ```bash
    dvc pull
    ```
    *   O DVC lerá o arquivo `data.dvc` que foi restaurado pelo `git checkout`.
    *   Ele então procurará os dados correspondentes no cache local do DVC e, se não estiverem lá, fará o download do armazenamento remoto (S3-Ninja ou S3 na nuvem).
    *   Finalmente, o DVC restaurará esses dados para a pasta `data/` do seu projeto.
4.  **Voltar para o branch principal (opcional):**
    *   Após inspecionar a versão anterior, você pode retornar ao seu branch de trabalho:
    ```bash
    git checkout main # ou o nome do seu branch
    ```
    *   Após o `git checkout main`, a pasta `data/` ainda refletirá a versão anterior. Execute `dvc pull` novamente para sincronizá-la com a versão mais recente do `main`:
    ```bash
    dvc pull
    ```


### Exceções ou potenciais problemas:

*   **Se `git checkout` falhar devido a conflitos ou modificações locais:**
    *   É porque: Você tem arquivos não commitados ou que entrariam em conflito com a versão do commit de destino.
    *   Então faça: Faça commit ou descarte suas alterações locais (`git stash`, `git reset --hard`) antes de tentar o `git checkout` novamente.
*   **Se `dvc pull` não baixar os dados corretos ou reportar erros:**
    *   É porque: Os dados para aquele `hash_do_commit` específico podem não ter sido enviados para o remoto DVC originalmente (`dvc push` foi esquecido), ou há problemas de conectividade/permissão com o armazenamento remoto.
    *   Então faça: Verifique a configuração do seu remoto DVC e a acessibilidade do serviço S3-Ninja (ou S3 na nuvem). Se os dados nunca foram enviados, eles não poderão ser recuperados.
*   **Se a pasta `data/` não mudar após `dvc pull`:**
    *   É porque: Os dados já estão presentes no cache local do DVC e correspondem à versão solicitada, ou há um problema com a vinculação do DVC.
    *   Então faça: Verifique os hashes no `data.dvc` e no cache DVC. Tente `dvc repro data.dvc` para forçar a reprodução ou `dvc pull --force` em casos extremos.