# Flutter Git Hooks

- [Git Hooks Explained Using Flutter](https://medium.com/@rogood/githooks-explained-using-flutter-babcdeb4048d#:~:text=Git%20hooks%20allow%20scripts%20to,identical%20across%20all%20git%20projects)
- [Git Hooks Example](https://support.gitkraken.com/working-with-repositories/githooksexample/)

## Steps

- Create `.githooks/` folder in the source root.

- Change the hooks folder to be .githooks, run:

  ```echo
  git config core.hooksPath .githooks/
  ```

- Create the `pre-commit` and `pre-push` file without format inside `.githooks` folder.

  <img  alt="Trucko - Logo" src="https://i.imgur.com/vDEzMlA.png"/><br/>

- Inside `pre-commit` file, put this:

  ```bash
  #!/usr/bin/env bash

  printf "\e[36;1m%s\e[0m\n" 'Pre-commit hook started'

  #Flutter format
  printf "\e[33;1m%s\e[0m\n" 'Running the Flutter formatter'
  flutter format .
  printf "\e[32;1m%s\e[0m\n" 'Finished running the Flutter formatter'

  printf "\e[36;1m%s\e[0m\n" 'Pre-commit hook finished'
  ```

- Inside `pre-push` file, put this:

  ```bash
  #!/usr/bin/env bash

  #Check if exist some file(s) modified that have not been committed
  if [[ `git status --porcelain` ]]; then
    printf "\e[31;1m%s\e[0m\n" 'This script needs to run against committed code only. Please commit or stash you changes.'
    exit 1
  fi

  printf "\e[36;1m%s\e[0m\n" 'Pre-push hook started'

  #Flutter analyzer
  printf "\e[33;1m%s\e[0m\n" 'Running the Flutter analyzer'
  flutter analyze
  if [ $? -ne 0 ]; then
    printf "\e[31;1m%s\e[0m\n" 'Flutter analyzer error'
    exit 1
  fi
  printf "\e[32;1m%s\e[0m\n" 'Finished running the Flutter analyzer'

  #Flutter test
  printf "\e[33;1m%s\e[0m\n" 'Running unit tests'
  flutter test
  if [ $? -ne 0 ]; then
    printf "\e[31;1m%s\e[0m\n" 'Unit tests error'
    exit 1
  fi
  printf "\e[32;1m%s\e[0m\n" 'Finished running unit tests'

  printf "\e[36;1m%s\e[0m\n" 'Pre-push hook finished'
  ```

- After this, now we need make the `pre-commit` and `pre-push` files executable:

  ```bash
  cd .githooks && chmod +x pre-commit && chmod +x pre-push && cd ..
  ```

- `Now` if you `commit` or `push` some code, will `run/execute` this `hooks`:

- `Commit`:

  <img  alt="pre-commit" src="https://i.imgur.com/MAJmZgP.png"/><br/>

- `Push`:

  <img  alt="pre-push" src="https://i.imgur.com/2UvWl3Z.png"/><br/>
