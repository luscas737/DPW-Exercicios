# Evidências - Conflito de Merge

Este documento apresenta o registro do processo de simulação de conflito entre ramificações, a estrutura dos marcadores de conflito, o grafo do histórico e a análise técnica do motivo da falha de mesclagem automática.

---

### 1. Saída do Git Merge com Conflito

**Comando:**
```bash
git merge feat/titulo-b
```

**Saída:**
```text
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

---

### 2. Conteúdo do Arquivo Durante o Conflito

**Arquivo:** `README.md` (Visualização com marcadores antes da resolução)

```text
<<<<<<< HEAD
abroba
# DPW — Exercícios do M00
=======
# DPW — Exercícios do M00
>>>>>>> feat/titulo-b

**Nome:** Seu Nome Completo
```

---

### 3. Grafo do Histórico do Repositório

**Comando:**
```bash
git log --graph --oneline --all
```

**Saída:**
```text
*   fafc4d3 (HEAD -> main, origin/main) Merge branch 'feat/titulo-b'
|\  
| * 4987ce3 (feat/titulo-b) teste  de branch - parte 2
* | d595ead (feat/titulo-a) teste de branch - Questão 3
|/  
*   82a4ba5 Merge branch 'main' of https://github.com/luscas737/DPW-Exercicios
|\  
| * c999e3f Formatando corretamente o arquivo da atividade para o formato markdown
* | 2977a75 Questão 2 do Exercício 00
|/  
* aa78758 Corrigindo o arquivo README para comportar o link para o 1° commit
* 2a71744 Questão 1 do Exercício 00
* ca7de34 Adiciona arquivos iniciais do projeto
```

---

### 4. Links Permanentes (Linhas de Tempo)

* **Commit de Merge:** [fafc4d3](https://github.com/luscas737/DPW-Exercicios/commit/fafc4d31b87836e64039e5576fc760b035debcbc)
* **Gráfico de Rede (Network):** [luscas737/DPW-Exercicios/network](https://github.com)

---

### 5. Análise Técnica do Conflito

**Pergunta:** Por que o Git não conseguiu resolver sozinho?

**Resposta:**
O Git não conseguiu resolver sozinho porque **as duas branches modificaram exatamente a mesma linha** do mesmo arquivo, e **não há como saber qual versão é a correta** — isso exige decisão humana.
Como os commits têm históricos divergentes (um veio depois do merge do outro), o Git não tem base para escolher automaticamente entre as duas versões conflitantes.
Por isso ele interrompe o merge e pede que você resolva manualmente, escolhendo ou combinando as alterações.
