# Evidências - Instalação de Dependências e Status do Git

Este documento apresenta o registro da limpeza, instalação limpa das dependências do projeto com `pnpm` e a verificação subsequente do status do repositório.

---

### 1. Limpeza e Instalação de Dependências

**Comandos:**
```bash
rm -rf node_modules
pnpm install --frozen-lockfile
```

**Saída:**
```text
✓ Lockfile passes supply-chain policies (verified 19m ago)
Lockfile is up to date, resolution step is skipped
Packages: +1 +
Packages are hard linked from the content-addressable store to the virtual store.
Content-addressable store is at: /home/lucas/.local/share/pnpm/store/v11
Virtual store is at: node_modules/.pnpm
Progress: resolved 1, reused 1, downloaded 0, added 1, done

devDependencies:
+ prettier 3.9.6

Done in 621ms using pnpm v11.23.0
```

---

### 2. Verificação do Status do Repositório

**Comando:**
```bash
git status --short
```

**Saída:**
```text
*(Nenhuma alteração pendente detectada)*
```

---

### 3. Análise de Versionamento (.gitignore)

**Pergunta:** Por que o `pnpm-lock.yaml` é versionado e o `node_modules/` não?

**Resposta:**
O `pnpm-lock.yaml` é versionado para garantir que toda a equipe e os ambientes de deploy instalem exatamente as mesmas versões e subversões das dependências, enquanto a pasta `node_modules/` não é guardada no Git por conter arquivos pesados e específicos de cada sistema operacional que podem ser recriados automaticamente a qualquer momento.
