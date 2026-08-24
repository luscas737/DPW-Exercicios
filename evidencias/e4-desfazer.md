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
