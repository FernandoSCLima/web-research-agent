# Agente de Pesquisa Web

Um subagente do Claude Code para operadores hoteleiros. Pesquisa web com ótimo custo-benefício — encontrando empresas locais, fornecedores, produtos, vendedores e informações sobre a concorrência — e salvando os resultados para que você não precise pesquisar a mesma coisa duas vezes.

Construído sobre a **estrutura WAT** (Workflows, Agents, Tools): IA probabilística cuida do raciocínio, scripts determinísticos cuidam da execução.

---

## O que faz

- **Encontra empresas locais** (encanadores, eletricistas, fornecedores, empreiteiros) — retorna nome, telefone, e-mail, endereço e avaliação
- **Pesquisa produtos** — especificações, preços, disponibilidade e onde comprar localmente
- **Extrai dados de concorrentes/mercado** — preços, ofertas e avaliações de propriedades semelhantes
- **Retorna resultados estruturados** — tabelas ou listas com URLs de origem, avaliações e uma recomendação clara
- **Armazena em cache os resultados** — cada pesquisa é salva na memória do agente, para que a próxima execução utilize o cache em vez de pesquisar novamente

---

## O que você precisa

- [Claude Code](https://claude.ai/code) instalado
- Acesso à web (o agente utiliza as ferramentas integradas `WebSearch` e `WebFetch` do Claude Code)
- Opcional: uma chave de API da Anthropic, caso esteja executando o agente fora de uma sessão paga do Claude Code

---

## Rápido Início

1. **Clone o repositório** em um projeto Claude Code:

``bash

git clone https://github.com/th1-ai/web-research-agent.git

cd web-research-agent

```

2. **Copie o modelo de ambiente** e preencha-o (ou ignore — o agente usa as configurações padrão do Claude Code se você não tiver chaves de API para configurar):

``bash

cp .env.example .env

```

3. **Edite [`.claude/agents/web-research.md`](.claude/agents/web-research.md)** — substitua os marcadores `{{HOTEL_*}}` descritos em [Customising](#customising) com os detalhes da sua propriedade.

4. **Abra no Claude Code** a partir da raiz do projeto:

``bash

claude

```

5. **Invoque o agente**:

> "Encontre três encanadores qualificados a até 30 minutos da propriedade que trabalhem com serviços comerciais."

O Claude Code detectará automaticamente o subagente `web-research` devido à linha `description` no frontmatter.

---

## Estrutura de arquivos

```
web-research-agent/
├── README.md # Este arquivo
├── .env.example # Chaves de API opcionais
├── .gitignore
├── LICENSE
└── .claude/

├── agents/

│ └── web-research.md # A definição do subagente

└── agent-memory/
└── web-research/

├── MEMORY.md # Índice de projetos de pesquisa salvos

└── example-feedback-pattern.md # Exemplo de "nota de memória" — substitua pela sua próprio
```

---

## Personalização

O arquivo do agente usa marcadores de posição para que possa ser adaptado a qualquer propriedade em qualquer lugar do mundo. Substitua o seguinte antes da primeira execução:

| Marcador de posição | O que inserir | Exemplo |

|---|---|---|

| `{{HOTEL_NAME}}` | Nome da sua propriedade | `The Olive Tree Inn` |

| `{{HOTEL_LOCATION}}` | Cidade/vila | `<sua cidade>` |

| `{{HOTEL_REGION}}` | Região ou estado | `<sua região>` |

| `{{HOTEL_NEARBY_TOWNS}}` | Lista de cidades próximas separadas por vírgulas | `<cidade A>, <cidade B>, <cidade C>` |

| `{{HOTEL_LANGUAGE}}` | Idioma de busca local | `Francês`, `Espanhol`, `Inglês` |

Você pode fazer isso com qualquer editor de texto ou com uma única passagem do `sed`:

```bash
sed -i '' \
-e 's|{{HOTEL_NAME}}|The Olive Tree Inn|g' \
-e 's|{{HOTEL_LOCATION}}|<sua cidade>|g' \
-e 's|{{HOTEL_REGION}}|<sua região>|g' \
-e 's|{{HOTEL_NEARBY_TOWNS}}|<cidades próximas>|g' \
-e 's|{{HOTEL_LANGUAGE}}|<idioma local>|g' \
.claude/agents/web-research.md
```

---

## Como a memória funciona

O agente usa a memória por agente do Claude Code em `.claude/agent-memory/web-research/`. Após cada sessão de pesquisa, o sistema salva:

- **Arquivos de projeto** (`project_<slug>.md`) — um arquivo por tópico de pesquisa, contendo a pergunta, a resposta, os detalhes de contato e os URLs das fontes. A próxima execução lê esses arquivos primeiro para evitar pesquisar novamente informações já conhecidas.

- **Notas de feedback** — padrões que tornam pesquisas futuras mais rápidas ou econômicas (por exemplo, "para fornecedores locais, uma consulta focada no idioma local é melhor do que três consultas genéricas em inglês").

[`MEMORY.md`](.claude/agent-memory/web-research/MEMORY.md) é o índice — mantenha uma linha por arquivo de projeto, com a data, para identificar pesquisas desatualizadas. [`example-feedback-pattern.md`](.claude/agent-memory/web-research/example-feedback-pattern.md) mostra o formato recomendado para uma nota de feedback. Exclua-o assim que tiver o seu próprio formato.

**Atenção ao commitar arquivos de memória:** pesquisas de projetos frequentemente contêm números de telefone, preços e endereços de e-mail reais de empresas locais. O arquivo `.gitignore` incluído impede que o arquivo `.env` seja incluído no Git, mas não exclui o arquivo `.claude/agent-memory/`. Se você deseja manter a privacidade dos arquivos de memória, adicione `.claude/agent-memory/` ao seu arquivo `.gitignore` local ou mantenha um fork privado.

---

## Agentes relacionados

Este agente funciona bem em conjunto com:

- [`concierge-agent`](https://github.com/th1-ai/concierge-agent) — importa resultados de pesquisas ao adicionar novos restaurantes, clubes de praia ou fornecedores ao vett
