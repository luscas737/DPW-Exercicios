[lucas@arch DPW-exercicios]$ rm -rf node_modules
[lucas@arch DPW-exercicios]$ 

[lucas@arch DPW-exercicios]$ pnpm install --frozen-lockfile
✓ Lockfile passes supply-chain policies (verified 19m ago)
Lockfile is up to date, resolution step is skipped
Packages: +1
+
Packages are hard linked from the content-addressable store to the virtual store.
  Content-addressable store is at: /home/lucas/.local/share/pnpm/store/v11
  Virtual store is at:             node_modules/.pnpm
Progress: resolved 1, reused 1, downloaded 0, added 1, done

devDependencies:
+ prettier 3.9.6

Done in 621ms using pnpm v11.23.0

[lucas@arch DPW-exercicios]$ rm -rf node_modules
[lucas@arch DPW-exercicios]$ pnpm install --frozen-lockfile
✓ Lockfile passes supply-chain policies (verified 19m ago)
Lockfile is up to date, resolution step is skipped
Packages: +1
+
Packages are hard linked from the content-addressable store to the virtual store.
  Content-addressable store is at: /home/lucas/.local/share/pnpm/store/v11
  Virtual store is at:             node_modules/.pnpm
Progress: resolved 1, reused 1, downloaded 0, added 1, done

devDependencies:
+ prettier 3.9.6

Done in 621ms using pnpm v11.23.0
[lucas@arch DPW-exercicios]$ git status --short
[lucas@arch DPW-exercicios]$ 

###

https://github.com/luscas737/DPW-Exercicios/blob/ca7de34aa5284996b31acf0300ef1272e16b5386/.gitignore

###

uma frase: por que o pnpm-lock.yaml é versionado e o node_modules/ não?

O pnpm-lock.yaml é versionado para garantir versões consistentes das dependências, enquanto o node_modules/ não é porque pode ser recriado automaticamente a partir do package.json e do arquivo de lock.
