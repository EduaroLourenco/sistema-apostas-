# Sistema de Apostas — Campeonato de Futebol

Aplicação desktop Java com interface Swing para gerenciamento de um bolão de futebol.

## Funcionalidades

- **Clubes** — Cadastro com nome, cidade e estádio
- **Campeonatos** — Criação e vinculação de clubes
- **Partidas** — Agendamento com data/hora e clubes participantes
- **Grupos e Participantes** — Organização dos apostadores em grupos
- **Apostas** — Registro de palpites (bloqueado 20 min antes da partida)
- **Resultados** — Inserção protegida por senha do administrador com cálculo automático de pontos
- **Histórico** — Log completo de todas as ações realizadas no sistema

## Pontuação

| Acerto | Pontos |
|--------|--------|
| Placar exato | 10 pts |
| Apenas resultado (V/E/D) | 5 pts |
| Errou | 0 pts |

## Banco de Dados

Utiliza **H2 Database** (embedded, pure Java). O arquivo `apostas.mv.db` é criado automaticamente na pasta onde o sistema é executado.

### Padrão Repository

Cada entidade possui seu próprio repositório com CRUD completo:
- `ClubeRepository`
- `CampeonatoRepository`
- `PartidaRepository`
- `GrupoRepository`
- `ParticipanteRepository`
- `ApostaRepository`
- `EventoRepository`

## Como executar

### Pré-requisito
- Java 17+ instalado

### Via JAR (mais simples)
```bash
java -jar sistema-apostas.jar
```

### Via Maven
```bash
mvn package
java -jar target/sistema-apostas.jar
```

## Credenciais padrão

- **Senha do Administrador:** `admin123`

## Estrutura do projeto

```
src/main/java/br/edu/sistema/
├── Main.java
├── db/
│   └── ConexaoDB.java          # Singleton — conexão H2
├── interfaces/
│   ├── Cadastravel.java
│   ├── Pontuavel.java
│   └── Visualizavel.java
├── model/
│   ├── Pessoa.java             # Abstrata — base de Participante e Administrador
│   ├── EntidadeBase.java       # Abstrata — base de Clube e Campeonato
│   ├── Administrador.java
│   ├── Participante.java
│   ├── Clube.java
│   ├── Campeonato.java
│   ├── Partida.java
│   ├── Aposta.java
│   └── Grupo.java
├── repository/
│   ├── Repository.java         # Interface genérica — padrão Repository
│   ├── ClubeRepository.java
│   ├── CampeonatoRepository.java
│   ├── PartidaRepository.java
│   ├── GrupoRepository.java
│   ├── ParticipanteRepository.java
│   ├── ApostaRepository.java
│   └── EventoRepository.java
├── service/
│   └── SistemaApostas.java     # Singleton — fachada de toda lógica de negócio
├── ui/
│   ├── MainFrame.java
│   ├── PainelClubes.java
│   ├── PainelCampeonatos.java
│   ├── PainelPartidas.java
│   ├── PainelGrupos.java
│   ├── PainelApostas.java
│   ├── PainelResultados.java
│   └── PainelHistorico.java
└── util/
    └── Tema.java               # Centraliza cores, fontes e factory de componentes
```

## Padrões de Projeto aplicados

- **Singleton** — `SistemaApostas` e `ConexaoDB`
- **Repository** — interface genérica `Repository<T, ID>` implementada por cada entidade
- **Facade** — `SistemaApostas` expõe a lógica de negócio sem expor os repositórios diretamente à UI
