# 🏅 Sistema de Gestão das Olimpíadas (SGO)

---

## 📘 **Descrição do Sistema**

O **Sistema de Gestão das Olimpíadas (SGO)** tem como objetivo **gerenciar competições, inscrições de atletas, alocação de locais, controle de resultados e geração de relatórios de medalhas**.

### ⚙️ **Funcionalidades Principais**
- 📆 **Cadastrar competições** e seus detalhes;
- 🏃‍♂️ **Inscrever atletas** representando países;
- 🏟️ **Alocar locais** de forma a evitar conflitos de horário;
- 🏁 **Registrar resultados** e vencedores;
- 🥇 **Gerar relatórios de medalhas** por país.

---

## 👥 **Histórias de Usuário**

| Código | História de Usuário | Papel |
|--------|---------------------|--------|
| **US01** | Como **organizador**, quero cadastrar novas competições com nome, modalidade, data, horário e local, para que sejam incluídas no cronograma oficial. | 🧑‍💼 Organizador |
| **US02** | Como **atleta**, quero me inscrever em uma competição específica representando meu país, para participar oficialmente dos jogos. | 🏃‍♂️ Atleta |
| **US03** | Como **administrador**, quero alocar locais para cada competição sem sobreposição de horários, garantindo o uso adequado das instalações. | 🧑‍💻 Administrador |
| **US04** | Como **juiz**, quero registrar os vencedores e classificados de cada competição, para atualizar o quadro de medalhas. | ⚖️ Juiz |
| **US05** | Como **comitê organizador**, quero visualizar o total de medalhas de cada país (ouro, prata e bronze), para divulgar os resultados oficiais. | 🎖️ Comitê |

---

## 🎯 **Diagramas UML**

A seguir estão todos os diagramas UML exigidos no trabalho, modelando o **SGO** conforme as regras de negócio descritas.

---

### 🧩 **Diagrama de Caso de Uso**

```mermaid
graph TD
  A[👤 Organizador] --> B(Gerenciar Competição)
  A --> C(Alocar Local)
  A --> D(Gerenciar Atletas)
  E[🏃‍♂️ Atleta] --> F(Inscrever-se em Competição)
  G[⚖️ Juiz] --> H(Salvar Resultados)
  I[🎖️ Comitê] --> J(Gerar Relatório de Medalhas)


##  Diagrama de Caso de Uso 

```mermaid
graph TD
  A[👤 Organizador] --> B(Gerenciar Competição)
  A --> C(Alocar Local)
  A --> D(Gerenciar Atletas)
  E[🏃‍♂️ Atleta] --> F(Inscrever-se em Competição)
  G[⚖️ Juiz] --> H(Salvar Resultados)
  I[🎖️ Comitê] --> J(Gerar Relatório de Medalhas)

```
---

##  Diagrama de Classes e de Pacotes 

```mermaid
classDiagram
    class Atleta {
      +id: int
      +nome: string
      +idade: int
      +pais: Pais
      +competicoes: List<Competicao>
      +inscreverCompeticao(c: Competicao)
      +consultarCompeticoes()
    }
    
  class Pais {
    +codigo: string
    +nome: string
    +medalhasOuro: int
    +medalhasPrata: int
    +medalhasBronze: int
    +atualizarMedalhas(tipo: string)
  }
    
class Competicao {
  +id: int
  +modalidade: string
  +data: date
  +horario: time
  +local: Local
  +atletas: List<Atleta>
  +resultado: Resultado
  +adicionarAtleta(a: Atleta)
  +registrarResultado(r: Resultado)
}
    

class Local {
  +id: int
  +nome: string
  +endereco: string
  +capacidade: int
  +verificarDisponibilidade(data: date, horario: time)
}

class Resultado {
  +id: int
  +competicao: Competicao
  +ouro: Atleta
  +prata: Atleta
  +bronze: Atleta
  +registrarMedalhistas(ouro: Atleta, prata: Atleta, bronze: Atleta)
}
    Competicao "1" -- "*" Atleta
    Competicao "1" -- "1" Local
    Competicao "1" -- "1" Resultado
    Atleta "1" -- "1" Pais

```

---

```mermaid
graph LR
  Participantes[📦 Participantes] --> Gerenciamento[📦 Gerenciamento]
  Gerenciamento --> Resultados[📦 Resultados]
  Resultados --> Medalhas[📦 Relatórios]
  Participantes --> Medalhas

```

---

## Diagrama de Componentes

```mermaid
%%{init: {'theme': 'neutral'}}%%
graph TD

%% --- Camada de apresentação ---
UI[Interface do Usuario Web]

%% --- Camada de negócio ---
INS[Modulo de Inscricoes]
COMP[Modulo de Competicoes]
RES[Modulo de Resultados]
REL[Modulo de Relatorios]

%% --- Camada de persistência ---
DB[(Banco de Dados SGO)]

%% --- Estrutura e conexões ---
UI --> INS
UI --> COMP
UI --> REL
INS --> COMP
COMP --> RES
RES --> REL
INS --> DB
COMP --> DB
RES --> DB
REL --> DB

%% --- Classes visuais simulando camadas ---
classDef frontend fill:#f4f4f4,stroke:#333,stroke-width:1px;
classDef backend fill:#e0f7fa,stroke:#006064,stroke-width:1px;
classDef database fill:#fff3e0,stroke:#ef6c00,stroke-width:1px;

class UI frontend;
class INS,COMP,RES,REL backend;
class DB database;

```
---

## Diagrama de Implantação

```mermaid

flowchart TB
    %% --- Camada do Usuário ---
    subgraph USERS [Dispositivos dos Usuários]
        ADM[Laptop Administrador]
        ORG[Tablet Organizador]
        ATL[Smartphone Atleta]
    end

    %% --- Zona Desmilitarizada (Acesso Web) ---
    subgraph DMZ [Zona Desmilitarizada]
        FW1[Firewall Frontal]
        LB[Balanceador de Carga]
        WEB["Servidor Web - Interface SGO"]
    end

    %% --- Camada de Aplicação (Lógica do Sistema) ---
    subgraph APP [Camada de Aplicação]
        FW2[Firewall Interno]
        MOD1["Módulo de Inscrições"]
        MOD2["Módulo de Alocação de Locais"]
        MOD3["Módulo de Resultados e Relatórios"]
    end

    %% --- Camada de Dados ---
    subgraph DATA [Camada de Dados]
        DB1[("Banco de Dados SGO")]
        BKP[("Servidor de Backup")]
    end

    %% --- Sistemas Externos (Integrações futuras) ---
    subgraph EXT [Sistemas Externos]
        API1["API Comitê Olímpico Internacional"]
        API2["Sistema de Notificações por E-mail"]
    end

    %% --- Conexões ---
    USERS --> FW1
    FW1 --> LB
    LB --> WEB
    WEB --> FW2
    FW2 --> MOD1
    FW2 --> MOD2
    FW2 --> MOD3
    MOD1 --> DB1
    MOD2 --> DB1
    MOD3 --> DB1
    DB1 --> BKP
    MOD3 --> API2
    MOD1 --> API1

    %% --- Definição de cores (com texto preto) ---
    classDef user fill:#cce5ff,stroke:#004085,stroke-width:1px,color:#000;
    classDef dmz fill:#fff3cd,stroke:#856404,stroke-width:1px,color:#000;
    classDef app fill:#d4edda,stroke:#155724,stroke-width:1px,color:#000;
    classDef data fill:#f8d7da,stroke:#721c24,stroke-width:1px,color:#000;
    classDef ext fill:#e2e3e5,stroke:#6c757d,stroke-width:1px,color:#000;

    %% --- Aplicando cores ---
    class ADM,ORG,ATL user;
    class FW1,LB,WEB dmz;
    class FW2,MOD1,MOD2,MOD3 app;
    class DB1,BKP data;
    class API1,API2 ext;

   ```




```

