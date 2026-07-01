
Dicas de dia a dia e controles.

### Configure & Clone
#### Clone
`git clone [repolink] [ProjName]`
`cd [ProjName]`
#### Config User
`git config --local user.name "Seu Nome"`
`git config --local user.email "seu.email@example.com"`
#### Analisys Branch
`git switch -c analisys/SeuNome origin/main`
### Git RULES

#### git sync
`git config --global alias.sync "!git checkout main && git pull origin main"`
#### git home
`git config --global alias.home "!git checkout analisys/SeuNome && git pull origin main"`

### Alterações & Correções

#### Criar Branch
`git switch -c feat/XXX-NNNN origin/main`
OR
`git switch -c fix/BUG-NNNN origin/main`

#### Add
`git add .

#### Commit & Push
`git commit -m "Mensagem do Commit"`
`git push -u origin {[feat/XXX-NNNN] OR [fix/BUG-NNNN]}`
