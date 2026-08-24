# Evidências - Arqueologia do Repositório

Este documento apresenta as respostas para as cinco perguntas de análise do histórico do repositório utilizando comandos Git.

---

### 1. Quantos commits o repositório tem?

**Comando:**
```bash
git rev-list --all --count
```

**Saída:**
```text
21785
```

---

### 2. Qual foi o primeiro commit, e em que data?

**Comando:**
```bash
git log --reverse | head -n 6
```

**Saída:**
```text
commit f7c8d10fb20943bc7102c73d5ecbe49e6c0b5ea1
Author: kamil.mysliwiec <kamil.mysliwiec@frogriot.com>
Date:   Sun Jan 8 15:09:41 2017 +0100

    Initial commit
```

---

### 3. Quem mais modificou packages/core/injector/injector.ts?

**Comando:**
```bash
git shortlog -sn -- packages/core/injector/injector.ts
```

**Saída:**
```text
    88  Kamil Myśliwiec
    12  Jay McDoniel
     6  Kamil Mysliwiec
     4  Jean-Baptiste Pionnier
     4  Livio Brunner
     3  Micael Levi (lab)
     2  Jiri Hajek
     2  Micael Levi L. Cavalcante
     2  mag123c
     1  Elies Lou
     1  Lee Donghyun
     1  Livio
     1  Lutz
     1  Nathan Knight
     1  Sergei Yudin
     1  Tony133
     1  codytseng
     1  cojack
     1  coti-z
     1  jacob87o2
     1  malekelkssas
     1  tooleks
     1  youmoo
```

---

### 4. O que mudou no último commit que tocou esse arquivo?

**Comando:**
```bash
git log -p -n 1 -- packages/core/injector/injector.ts
```

**Saída (Linhas relevantes):**
```diff
commit 5d1b19bca65c7b25dd5dc27c0a6384b8015ee43d
Merge: b6bdd79c4 a112f3fbd
Author: Kamil Myśliwiec <mail@kamilmysliwiec.com>
Date:   Fri Aug 14 16:05:35 2026 +0200

    Merge branch 'master' into v12.0.0
    
    Resolves conflicts between the v12 ESM/vitest migration and master:
    - Ported the SseSignal/AbortController SSE feature into the ESM codebase
      (sse-signal.decorator, router-response-controller, router-execution-context,
      interceptors-consumer transformDeferred rewrite)
    - Converted master's chai/sinon/mocha test additions to vitest style
    - Kept v12 sample structure (oxlint/vitest) while adopting master's
      renovate dependency bumps
    - Regenerated package-lock.json
    
    Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>
```

---

### 5. Quantos commits foram feitos nos últimos 90 dias?

**Comando:**
```bash
git rev-list --count --since="90 days ago" --all
```

**Saída:**
```text
703
```
