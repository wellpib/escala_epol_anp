# Portal ePol — Gestão de Alocações

Portal web para visualização e controle de disponibilidade de servidores e grade de horários do curso ePol, com **CRUD completo** e **persistência de dados** via Firebase e LocalStorage.

---

## ✨ Novidades da Versão 2.0

🎉 **CRUD de Servidores**: Adicione, edite e exclua servidores diretamente pelo portal  
🗄️ **Banco de Dados**: Integração com Firebase Firestore para persistência na nuvem  
💾 **Backup Local**: LocalStorage como fallback (funciona offline)  
🔄 **Sincronização**: Dados salvos automaticamente e disponíveis em qualquer dispositivo

---

## 🚀 Como usar

### Opção 1 — Abrir diretamente no navegador
1. Abra o arquivo `index.html` em qualquer navegador moderno (Chrome, Edge, Firefox)
2. (Opcional) Configure o Firebase — veja [FIREBASE_CONFIG.md](FIREBASE_CONFIG.md)

### Opção 2 — GitHub Pages (Acesso Web)
1. Faça o fork / upload deste repositório para o GitHub
2. Acesse **Settings → Pages → Source: main / (root)**
3. O portal estará disponível em `https://seu-usuario.github.io/nome-do-repo/`

📖 **Guia completo de hospedagem:** [GITHUB_PAGES.md](GITHUB_PAGES.md)

---

## 📋 Funcionalidades

### 👥 Gestão de Servidores (NOVO!)
- ➕ **Adicionar**: Cadastrar novos servidores manualmente
- ✏️ **Editar**: Atualizar informações de servidores existentes
- 🗑️ **Excluir**: Remover servidores do sistema
- 🔍 **Filtrar**: Por função, cargo, data de disponibilidade e nome
- 📊 **Visualizar**: Tabela completa com todas as informações

### 📊 Dashboard
- Cards estatísticos com resumo de servidores e vagas
- Gráficos de vagas por data e distribuição de funções
- Lista de servidores disponíveis para a semana

### 📅 Grade da Semana
- Visualização completa com filtros multi-seleção
- Contagem automática de vagas Prof. EPOL e Monitor EPOL
- Identificação de vagas preenchidas e em aberto
- Importação de grades CSV/Excel

### ✏️ Montar Grade
- Interface interativa para alocar servidores às vagas
- Validação de disponibilidade
- Detecção de conflitos
- Salvamento automático

### 📈 Relatórios
- **Servidores por Disponibilidade**: Lista filtrada por período
- **Vagas por Data**: Análise de vagas ePol por data e função
- **Cruzamento**: Servidores disponíveis × Vagas em aberto
- **Exportação CSV**: Todos os relatórios exportáveis

---

## 🗄️ Persistência de Dados

O portal suporta **dois métodos de armazenamento**:

### 1. Firebase Firestore (Recomendado)
- ☁️ Dados salvos na nuvem
- 🔄 Sincronização entre dispositivos
- 🔒 Seguro com autenticação
- 📊 Plano gratuito generoso

**Configuração:** Veja o guia completo em [FIREBASE_CONFIG.md](FIREBASE_CONFIG.md)

### 2. LocalStorage (Fallback)
- 💾 Armazenamento local no navegador
- 📴 Funciona offline
- ⚠️ Dados específicos por dispositivo

---

## 📁 Arquivos esperados

| Arquivo | Descrição |
|---|---|
| `Alocações ePol.xlsx` | Planilha de servidores — abas **FORA DE BRASÍLIA** e **DE BRASÍLIA** |
| `semana XX.csv` ou `.xlsx` | Grade de horários da semana a ser preenchida |

**Observação:** Com o CRUD implementado, você pode adicionar servidores manualmente sem precisar do Excel!

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
