# Evidências - Desfazer sem Pânico

Este documento apresenta a tabela resumo de comandos para desfazer alterações no Git, acompanhada dos testes práticos com a validação do status do repositório antes e depois de cada ação.

---

### Tabela Resumo de Cenários

| # | Cenário | Comando |
| :--- | :--- | :--- |
| **1** | Editei um arquivo e quero descartar a alteração (ainda não fiz `add`) | `git restore README.md` |
| **2** | Fiz `git add` do arquivo errado e quero tirá-lo do stage | `git restore --staged arquivo_errado.txt` |
| **3** | A mensagem do último commit está errada (ainda não fiz push) | `git commit --amend -m "Mensagem corrigida"` |
| **4** | Quero desfazer o último commit, mas manter as alterações no working directory | `git reset --soft HEAD~1` |
| **5** | Quero reverter um commit **já enviado** para o remoto | `git revert <hash-do-commit>` |

---

### Execução e Validação dos Cenários

#### Cenário 1: Descartar alteração em arquivo local (sem add)

*   **Antes do comando:**
    ```text
    On branch main
    Your branch is up to date with 'origin/main'.

    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git restore <file>..." to discard changes in working directory)
            modified:   README.md
    ```

*   **Comando executado:**
    ```bash
    git restore README.md
    ```

*   **Depois do comando:**
    ```text
    On branch main
    Your branch is up to date with 'origin/main'.
    nothing to commit, working tree clean
    ```

---

#### Cenário 2: Remover arquivo do Stage (Unstage)

*   **Antes do comando:**
    ```text
    On branch main
    Your branch is up to date with 'origin/main'.

    Changes to be committed:
      (use "git restore --staged <file>..." to unstage)
            new file:   arquivo_errado.txt
    ```

*   **Comando executado:**
    ```bash
    git restore --staged arquivo_errado.txt
    ```

*   **Depois do comando:**
    ```text
    On branch main
    Your branch is up to date with 'origin/main'.
    ```

---

#### Cenário 3: Corrigir mensagem do último commit (Amend)

*   **Comando executado:**
    ```bash
    git commit --amend -m "Mensagem corrigida"
    ```

*   **Depois do comando (Confirmação da alteração):**
    ```text
    [main 9d25adc] Mensagem corrigida
     Date: Mon Aug 24 16:57:40 2026 -0300
     2 files changed, 50 insertions(+)
     create mode 100644 evidencias/arquivo_errado.txt
     create mode 100644 evidencias/e4-desfazer.md
    ```

---

#### Cenário 4: Desfazer o último commit mantendo as alterações locais

*   **Antes do comando:**
    ```text
    On branch main
    Your branch is ahead of 'origin/main' by 1 commit.
      (use "git push" to publish your local commits)
    ```

*   **Comando executado:**
    ```bash
    git reset --soft HEAD~1
    ```

*   **Depois do comando:**
    ```text
    On branch main
    Your branch is up to date with 'origin/main'.

    Changes to be committed:
      (use "git restore --staged <file>..." to unstage)
            new file:   arquivo_errado.txt
            new file:   e4-desfazer.md
    ```

---

### 4. Histórico Local de Referências (Reflog)

**Comando:**
```bash
git reflog -10
```

**Saída:**
```text
c0e4ad6 (HEAD -> main, origin/main) HEAD@{0}: commit: Questão 4 - Exercício 00
2c04f7d HEAD@{1}: reset: moving to HEAD~1
9d25adc HEAD@{2}: commit (amend): Mensagem corrigida
be61ca9 HEAD@{3}: commit: teste
2c04f7d HEAD@{4}: commit: correções
b73386c HEAD@{5}: commit: Questão 3 - Exercício 00
fafc4d3 HEAD@{6}: commit (merge): Merge branch 'feat/titulo-b'
d595ead (feat/titulo-a) HEAD@{7}: checkout: moving from feat/titulo-b to main
4987ce3 (feat/titulo-b) HEAD@{8}: commit: teste de branch - parte 2
82a4ba5 HEAD@{9}: checkout: moving from main to feat/titulo-b
```

---

### 5. Análise de Comandos de Reversão

**Pergunta:** Por que o caso 5 é diferente do 4?

**Resposta:**
O caso 4 (`reset --soft`) **reescreve o histórico** localmente, removendo o commit — mas se já tiver sido enviado ao remoto, causa problemas para outros desenvolvedores.
O caso 5 (`revert`) **preserva o histórico** criando um novo commit que desfaz as alterações do commit anterior — é seguro para usar em commits já compartilhados no remoto.
Ou seja: `reset` muda o passado, `revert` adiciona um novo capítulo que nega o passado sem apagá-lo.
