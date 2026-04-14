# Portal ePol — Gestão de Alocações

Portal web estático para visualização e controle de disponibilidade de servidores e grade de horários do curso ePol.

## Como usar

### Opção 1 — Abrir diretamente no navegador
Abra o arquivo `index.html` em qualquer navegador moderno (Chrome, Edge, Firefox).

### Opção 2 — GitHub Pages
1. Faça o fork / upload deste repositório para o GitHub
2. Acesse **Settings → Pages → Source: main / (root)**
3. O portal estará disponível em `https://seu-usuario.github.io/nome-do-repo/`

## Arquivos esperados

| Arquivo | Descrição |
|---|---|
| `Alocações ePol.xlsx` | Planilha de servidores — abas **FORA DE BRASÍLIA** e **DE BRASÍLIA** |
| `semana XX.csv` ou `.xlsx` | Grade de horários da semana a ser preenchida |

## Funcionalidades

- **Dashboard** — cards de resumo com total de servidores disponíveis, vagas Prof./Monitor EPOL, distribuição de funções e gráfico de vagas por data
- **Servidores** — tabela completa com filtros por função, origem (Brasília / fora), data disponível e nome
- **Grade da Semana** — grade com filtros por data, horário, módulo, função e turma; vagas EPOL destacadas com contador de posições em aberto
- **Relatórios**:
  - Disponibilidade por período (exportável em CSV)
  - Vagas por data e função (exportável em CSV)
  - Cruzamento Servidor × Vaga (quais servidores disponíveis cobrem quais dias com vagas EPOL)

## Estrutura das planilhas

### Alocações ePol.xlsx

**Aba FORA DE BRASÍLIA**
| Coluna | Conteúdo |
|---|---|
| NOME | Nome do servidor |
| CARGO | EPF / APF |
| LOTAÇÃO | Unidade de lotação |
| FUNÇÃO | PROFESSOR ou MONITOR |
| Data início (col 9) | Início da disponibilidade |
| DATAS ALOCAÇÃO (col 10) | Fim da disponibilidade |

**Aba DE BRASÍLIA** — mesma estrutura, sem datas de viagem (servidores locais)

### semana XX.csv

| Coluna | Conteúdo |
|---|---|
| DATA | Data da aula (dd/mm/yyyy) |
| HORARIO | Horário (HH:MM-HH:MM) |
| MODULO | M2 ou M3 |
| COD_AULA | Código da aula |
| FUNCAO | Função (Prof. EPOL, Monitor EPOL, etc.) |
| SERVIDOR | Nome do servidor (vazio = em aberto) |
| TURMA | Identificação da turma |

## Tecnologias

- HTML5 / CSS3 / JavaScript (vanilla)
- [Bootstrap 5.3](https://getbootstrap.com/)
- [SheetJS (xlsx)](https://sheetjs.com/) — leitura de Excel/CSV no navegador
- 100% client-side — nenhum servidor necessário

## Privacidade

Todos os dados são processados **localmente no navegador**. Nenhum arquivo é enviado para servidores externos.
