# Bitácora del laboratorio de rescate

## 1. Ver el historial

- **Comandos usados:** `git log`, `git log --oneline`, `git log --oneline --graph --all`
- **¿Qué muestra cada comando?**
  - `git log`: muestra el historial detallado de cada commit (el código completo del commit, el autor, la fecha y el mensaje).
  - `git log --oneline`: muestra el mismo historial pero resumido en una sola línea por commit (código corto y mensaje).
  - `git log --oneline --graph --all`: muestra el resumen en una línea de todas las ramas junto con un gráfico en texto que representa cómo se conectan o ramifican los commits.
- **¿Qué es el código que aparece al inicio de cada commit?**
  - Es el *hash* (identificador único) que Git genera para distinguir cada commit de forma exacta dentro del historial.

## 2. Ver qué cambió antes de guardar

- **Comandos usados:** `git diff`, `git add notas.txt`, `git diff`, `git diff --staged`
- **¿Por qué el segundo `git diff` no mostró nada?**
  - Porque `git diff` sin opciones solo muestra los cambios que están en el directorio de trabajo y aún no se han agregado al área de preparación (*staging*). Al ejecutar `git add notas.txt`, el cambio pasó al área de preparación y ya no había diferencias pendientes por agregar.
- **¿Qué muestra `--staged`?**
  - `git diff --staged` muestra los cambios que ya fueron agregados con `git add` y están listos en el área de preparación para incluirse en el próximo commit.

## 3. "Borré un archivo por error"

- **Comandos usados:** `del notas.txt` (o `rm notas.txt`), `git status`, `git restore notas.txt`
- **¿Qué hace `git restore`?**
  - Descarta los cambios locales no confirmados (incluyendo el borrado accidental) y restaura el archivo exactamente como estaba en el último commit guardado.
- **¿Funcionaría si nunca hubieras hecho commit de ese archivo?**
  - No, porque si el archivo nunca fue guardado en un commit (ni está en el área de preparación), Git no tiene ninguna copia previa registrada de dónde recuperarlo.

## 4. "Agregué un archivo que no debía subir"

- **Comandos usados:** `git add .`, `git status`, `git restore --staged secreto.env`, `.gitignore`
- **¿Qué diferencia hay entre `git restore` y `git restore --staged`?**
  - `git restore` revierte el archivo en nuestra carpeta de trabajo y borra los cambios locales que hayamos hecho sin guardar.
  - `git restore --staged` solo saca el archivo del área de preparación para que no entre en el próximo commit, pero mantiene el archivo intacto en nuestra carpeta.
  - Además, al crear `.gitignore` con `*.env`, evitamos que archivos sensibles como `secreto.env` se vuelvan a rastrear o subir por accidente.

## 5. "Escribí mal el mensaje del commit"

- **Comandos usados:** `git commit -m "cambios"`, `git commit --amend -m "docs: agrega línea 4 a notas"`
- **¿Qué hace este comando?**
  - `git commit --amend -m "..."` reemplaza el mensaje (o contenido) del último commit local para corregirlo sin crear un commit extra. Solo debe usarse cuando todavía no se ha hecho `git push`, porque modifica el historial reciente.

## 6. "Subí un commit que dañó el proyecto"

- **Comandos usados:** `git revert HEAD`, `git log --oneline`, `git push`
- **¿`git revert` borró el commit equivocado o creó uno nuevo?**
  - Creó un commit nuevo (`Revert "fix: limpia notas"`) que aplica los cambios opuestos para deshacer lo que hizo el commit anterior, dejando intacto el commit equivocado en el historial.
- **¿Por qué es la opción segura cuando ya se hizo push?**
  - Porque no reescribe ni elimina commits que ya están en GitHub y que otros compañeros pudieron haber descargado; simplemente suma una nueva corrección al historial compartido.

## 7. "Hice un commit antes de tiempo"

- **Comandos usados:** `git reset --soft HEAD~1`, `git status`
- **¿Qué diferencia hay entre `git reset --soft` y `git revert`?**
  - `git reset --soft HEAD~1` retrocede el historial local borrando el último commit, pero conserva todos los cambios de los archivos listos en el área de preparación (*staged*).
  - `git revert` no borra ningún commit del historial, sino que crea un commit nuevo que revierte los cambios de un commit anterior.
- **¿Cuándo se usa cada uno?**
  - `git reset --soft` se usa cuando el commit es únicamente local y todavía no se ha subido con `git push`.
  - `git revert` se usa cuando el commit ya fue subido al repositorio remoto (GitHub) y no se debe alterar el historial publicado.

## 8. "Tengo trabajo a medias y debo cambiar de tarea"

- **Comandos usados:** `git stash`, `git switch -c fix/urgente`, `git switch main`, `git stash pop`
- **¿Para qué sirve `git stash`?**
  - Sirve para guardar temporalmente los cambios que están a medias sin necesidad de hacer un commit incompleto, dejando el área de trabajo limpia para poder cambiar de rama sin problemas.
- **¿Qué hace `git stash pop`?**
  - Recupera los últimos cambios guardados en el *stash*, los vuelve a aplicar sobre los archivos actuales y los elimina de la lista temporal del *stash*.

## 9. Cierre: historial final (`git log --oneline --graph`)

```text
* 28bd6e7 docs: agrega línea 5 a notas
* 7c68366 feat: agrega borrador completo
* 297b8a9 Revert "fix: limpia notas"
* 5a62c59 fix: limpia notas
* 2cb3cad docs: agrega línea 4 a notas
* 70634aa chore: agrega .gitignore para archivos .env
* 1370268 docs: modifica línea 2 de notas
* f31c43e feat: agrega notas y bitácora
* 46c8c18 Initial commit
```
