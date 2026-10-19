# SECCOM UFSC 2026 — Minicurso: Modelos de IA "Sistema 1"

Material do minicurso sobre **modelos System One (Sistema 1)** aplicados a um caso de suporte ao cliente: em vez de pedir a um LLM generalista que entenda, classifique, decida o tom e responda, dividimos o trabalho.

```
 ticket (texto)
      │
      ▼
┌──────────────┐   saída tipada    ┌──────────────────┐
│     Jev      │ ────────────────► │  LLM (OpenAI)    │ ──► resposta ao cliente
│ classificador│  departamento     │  gera a resposta │
└──────────────┘  frustração       │  guiado pelos    │
                 urgência          │  sinais tipados  │
                                   └──────────────────┘
```

O **Jev** (TypeSafe) devolve valores tipados e calibrados (`choice`, `score`, `noul`), e o LLM só se preocupa em escrever bem.

## Conteúdo do repositório

| Arquivo | Descrição |
|---|---|
| `apresenta.html` | Slides do minicurso ("Modelos de IA do tipo Sistema 1"). Abra diretamente no navegador. |
| `Seccom_2026_Minicurso_System_1.ipynb` | Notebook prático (Python) para executar no Google Colab. |

### O que o notebook cobre

1. **Setup** — instalação das dependências e carregamento das chaves de API.
2. **Perguntas para o Jev** — os três tipos de *primitive*: `Choice`, `Score` e `Noul`.
3. **Classificador com saída tipada** — função `classify` que devolve um `dataclass` (`Triage`).
4. **Sinais tipados → instruções para o LLM** — regras determinísticas (tom, urgência, fallback por baixa confiança) montando o prompt.
5. **Pipeline completo Jev → OpenAI** — com uma célula interativa para você testar seus próprios tickets.
6. **Comparação Jev vs. LLM com Structured Outputs** — acerto contra um gabarito de 16 tickets, tokens, latência e custo estimado.
7. **Por que separar assim?** — previsibilidade, controle explícito, calibração e responsabilidades claras.

## Como executar o notebook no Google Colab

### 1. Importe o notebook

1. Acesse [colab.research.google.com](https://colab.research.google.com) e entre com sua conta Google.
2. Em **Arquivo → Fazer upload do notebook**, selecione `Seccom_2026_Minicurso_System_1.ipynb` (baixe-o antes deste repositório).

   > Alternativa: em **Arquivo → Abrir notebook → GitHub**, cole a URL do repositório e escolha o notebook.

### 2. Obtenha as chaves de API

Você precisará de duas chaves:

| Variável | Onde gerar |
|---|---|
| `TYPESAFE_API_KEY` | <https://console.typesafe.ai/keys> |
| `OPENAI_API_KEY` | <https://platform.openai.com/api-keys> |

> 📌 **Observação:** duas **chaves temporárias** serão enviadas aos inscritos no minicurso, para uso **exclusivo da parte prática**. Ao final do minicurso, quem quiser continuar experimentando deverá usar **chaves pessoais**, geradas nos links acima.

### 3. Configure as variáveis de ambiente (Secrets do Colab)

Ao abrir o notebook, **antes de executar qualquer célula**:

1. No menu lateral esquerdo do Colab, clique no ícone de chave **🔑 Secrets**.
2. Clique em **Adicionar novo secret** e crie os dois secrets, com exatamente estes nomes:
   - `TYPESAFE_API_KEY`
   - `OPENAI_API_KEY`
3. Cole o valor de cada chave no campo correspondente.
4. Ative o botão **"Acesso ao notebook"** de cada secret, para que este notebook possa lê-los.

A célula de setup do notebook lê esses secrets (via `google.colab.userdata`) e os exporta como variáveis de ambiente (`os.environ`). Se os Secrets não estiverem configurados, o notebook pedirá as chaves por `getpass` no momento da execução.

> ⚠️ **Nunca** cole suas chaves diretamente no código, nem compartilhe o notebook com elas preenchidas.

### 4. Execute

Rode as células em ordem (**Ambiente de execução → Executar tudo**, ou `Shift+Enter` célula a célula). A primeira célula instala `typesafe-sdk` e `openai`.

## Referências

- [Quickstart do Jev (TypeSafe)](https://docs.typesafe.ai/introduction/quickstart)
