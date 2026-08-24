# E00.5 — Roteiro de Diagnóstico (Adaptado)

Baseado nos arquivos de setup e nas ferramentas especificadas.

## Tabela de Diagnóstico (5 passos)

| Passo | Comando | Se a saída for X | Então |
|-------|---------|------------------|-------|
| **1** | `node --version` | `node: command not found` | Node não está instalado. Instale com `fnm install 20` (Linux/macOS) ou `winget install OpenJS.NodeJS.LTS` (Windows). |
| | | `v20.x.x` ou superior | Node está instalado. **Próximo passo.** |
| **2** | `pnpm --version` | `pnpm: command not found` | pnpm não está ativo. Rode `corepack enable` e `corepack prepare pnpm@latest --activate`. Windows: pode precisar `npm install -g pnpm`. |
| | | `9.x.x` ou superior | pnpm está instalado. **Próximo passo.** |
| **3** | `pnpm list --depth=0` (na raiz do projeto) | Pacote **não** aparece na lista | O pacote não está instalado. Instale com `pnpm add <pacote>` (ou `pnpm --filter <workspace> add <pacote>` se for específico). |
| | | Pacote aparece na lista | Está instalado. **Próximo passo.** |
| **4** | `ls -la node_modules/<pacote>` (Linux/macOS) <br> `dir node_modules\<pacote>` (Windows) | `No such file or directory` | `node_modules` está inconsistente. Execute `pnpm install` na raiz para recriar. |
| | | Diretório existe | Arquivos estão no lugar. **Próximo passo.** |
| **5** | `node -e "require.resolve('<pacote>')"` <br> (ou `import.meta.resolve` para ES modules) | Erro `Cannot find module` | Caminho de resolução quebrado. Verifique: <br> • Está no workspace correto? <br> • `package.json` tem `"main"` ou `"exports"`? <br> • Para monorepo: o `pnpm-workspace.yaml` inclui a pasta? |
| | | Caminho resolvido (ex: `/project/node_modules/pacote/index.js`) | O Node encontra o pacote. Verifique o **código** que faz o import — pode ser erro de sintaxe ou caminho relativo incorreto. |

---

## Demonstração — Provocando e diagnosticando a falha

### 1. Quebrar o ambiente (Windows/PowerShell)

```powershell
# Entrar no projeto
Set-Location C:\dev\bibliocom

# Instalar o pacote axios
pnpm add axios

# Simular a falha: apagar o diretório do pacote manualmente
Remove-Item -Recurse -Force node_modules\axios
```

### 2. Aplicar o roteiro

**Passo 1 — Verificar Node**
```powershell
$ node --version
v20.11.0
✅ Node instalado
```

**Passo 2 — Verificar pnpm**
```powershell
$ pnpm --version
9.1.0
✅ pnpm instalado
```

**Passo 3 — Listar pacotes instalados**
```powershell
$ pnpm list --depth=0
dependencies:
axios 1.6.0
express 4.18.0
✅ axios aparece na lista (diz que está instalado)
```

**Passo 4 — Verificar diretório do pacote**
```powershell
$ dir node_modules\axios
dir : Cannot find path 'C:\dev\bibliocom\node_modules\axios' because it does not exist.
❌ PACOTE NÃO ESTÁ NO DISCO!
```

**Correção:**
```powershell
$ pnpm install axios
Packages: +1
Progress: resolved 1, reused 0, downloaded 1, done
```

**Passo 5 — Testar import**
```powershell
$ node -e "console.log(require('axios').VERSION)"
1.6.0
✅ Funcionou!
```

### Diagnóstico final
A causa era `node_modules` inconsistente — o pacote estava registrado no `package.json` e no `pnpm-lock.yaml`, mas **não existia no disco**. Reinstalar resolveu.

---

## Demonstração — Cenário alternativo (monorepo)

### 1. Quebrar de outra forma: workspace errado

```bash
# No backend, mas tentando instalar no frontend
cd ~/dev/bibliocom/backend
pnpm add @bibliocom/tipos  # ← isso vai falhar porque o pacote é local
```

### 2. Aplicar o roteiro

**Passo 3 — Listar pacotes**
```bash
$ pnpm list --depth=0
dependencies:
@nestjs/core 10.0.0
@nestjs/common 10.0.0
❌ @bibliocom/tipos não aparece!
```

**Passo 4 — Verificar node_modules**
```bash
$ ls node_modules/@bibliocom
ls: cannot access 'node_modules/@bibliocom': No such file or directory
❌ Não existe
```

**Correção:**
```bash
# Voltar para a raiz e instalar com o filtro correto
cd ~/dev/bibliocom
pnpm --filter backend add @bibliocom/tipos@workspace:*
✅ Instalado corretamente
```

---

## Registro completo da demonstração

```bash
# 1. Simular o erro no Windows
PS C:\dev\bibliocom> pnpm add axios
Packages: +1
...
PS C:\dev\bibliocom> Remove-Item -Recurse -Force node_modules\axios

# 2. Diagnóstico
PS C:\dev\bibliocom> node --version
v20.11.0
✅

PS C:\dev\bibliocom> pnpm --version
9.1.0
✅

PS C:\dev\bibliocom> pnpm list --depth=0 | findstr axios
├── axios 1.6.0
✅

PS C:\dev\bibliocom> dir node_modules\axios
dir : Cannot find path 'C:\dev\bibliocom\node_modules\axios' because it does not exist.
❌ PROBLEMA ENCONTRADO!

# 3. Correção
PS C:\dev\bibliocom> pnpm install
Packages: +1
...
PS C:\dev\bibliocom> dir node_modules\axios
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        24/08/2026     10:30                axios
✅ RESOLVIDO!

# 4. Verificação final
PS C:\dev\bibliocom> node -e "console.log(require('axios').VERSION)"
1.6.0
✅ SUCESSO!
```

---
