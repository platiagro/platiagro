author:            Fabio Beranizo Lopes
summary:           Kubeflow AIops
id:                kubeflow-aiops-aifest2019
categories:        cloud
status:            draft
feedback link:     github.com/fberanizo/kubeflow-aiops-workshop

# Tutorial de utilização da plataforma

## Overview

[TODO]

## Listar tarefas

1. Clique na aba **Tarefas** do menu.
![A screenshot of welcome page. Button "Tarefas" is highlighted.](assets/tarefas/home.png)

2. O usuário será redirecionado para a página das tarefas, onde será listado todas as tarefas existentes.
![A screenshot of tasks page.](assets/tarefas/lista.png)

## Criar nova tarefa

1. Clique na aba **Tarefas** do menu.
![A screenshot of welcome page. Button "Tarefas" is highlighted.](assets/tarefas/home.png)

2. Clique no botão **Nova Tarefa**.
![A screenshot of tasks page. Button "Nova Tarefa" is highlighted.](assets/tarefas/nova-tarefa.png)

3. Será aberto um modal.
![A screenshot of new task modal.](assets/tarefas/nova-tarefa-modal.png)

4. Após escolher como deseja criar a tarefa, clique no botão **Criar Notebooks**.
![A screenshot of new task modal. Button "Criar Notebooks" is highlighted.](assets/tarefas/nova-tarefa-modal-botao-criar.png)

5. Será aberto uma nova aba no navegador contento os Notebooks criados na tarefa.
![A screenshot of notebooks page.](assets/tarefas/nova-tarefa-notebooks.png)

## Editar tarefa

1. Clique na aba **Tarefas** do menu.
![A screenshot of welcome page. Button "Tarefas" is highlighted.](assets/tarefas/home.png)

2. O usuário será redirecionado para a página das tarefas, onde será listado todas as tarefas existentes.
![A screenshot of tasks page.](assets/tarefas/editar-tarefa-lista.png)

3. Selecione qual tarefa deseja editar, clicando no nome da tarefa.
![A screenshot of tasks page. One task is highlighted.](assets/tarefas/editar-tarefa-lista-selecao.png)

4. Será aberto uma nova aba no navegador contento os Notebooks da tarefa selecionada para edição.
![A screenshot of notebooks page.](assets/tarefas/editar-tarefa-notebook.png)

## Listar projetos

1. Clique na aba **Projetos** do menu.
![A screenshot of welcome page. Button "Projetos" is highlighted.](assets/projetos/home.png)

2. O usuário será redirecionado para a página dos projetos, onde será listado todas os projetos existentes.
![A screenshot of projects page.](assets/projetos/projetos.png)

## Criar novo projeto

1. Clique na aba **Projetos** do menu
![A screenshot of welcome page. Button "Projetos" is highlighted.](assets/projetos/home.png)

2. Clique no botão **Novo Projeto**
![A screenshot of projects page. Button "Novo Projeto" is highlighted.](assets/projetos/projetos-novo.png)

3. Será aberto um modal
![A screenshot of new project modal.](assets/projetos/projetos-novo-modal.png)

4. Escolha um nome para o projeto e clique no botão **Criar**
![A screenshot of new project modal. Button "Criar" is highlighted.](assets/projetos/projetos-novo-modal-criar.png)

5. O usuário será redirecionado para a página de experimentos do projeto
![A screenshot of experiments page.](assets/projetos/experimentos.png)

## Listar experimentos

1. Clique na aba **Projetos** do menu
![A screenshot of welcome page. Button "Projetos" is highlighted.](assets/projetos/home.png)

2. O usuário será redirecionado para a página dos projetos, onde será listado todas os projetos existentes
![A screenshot of projects page.](assets/projetos/projetos.png)

3. Selecione qual projeto deseja listar os experimentos, clicando no nome do projeto
![A screenshot of projects page. One project is highlighted.](assets/projetos/projetos-lista.png)

4. O usuário será redirecionado para a página de experimentos do projeto selecionado
![A screenshot of experiments page.](assets/projetos/experimentos-lista.png)

## Criar novo experimento

1. Clique na aba **Projetos** do menu
![A screenshot of welcome page. Button "Projetos" is highlighted.](assets/projetos/home.png)

2. O usuário será redirecionado para a página dos projetos, onde será listado todas os projetos existentes
![A screenshot of projects page.](assets/projetos/projetos.png)

3. Selecione qual projeto deseja criar um novo experimento, clicando no nome do projeto
![A screenshot of projects page. One project is highlighted.](assets/projetos/projetos-lista.png)

4. O usuário será redirecionado para a página de experimentos do projeto selecionado
![A screenshot of experiments page.](assets/projetos/experimentos.png)

5. Clique no botão **+**
![A screenshot of experiments page. Button "+" is highlighted.](assets/projetos/experimentos-criar.png)

6. Será aberto um modal
![A screenshot of new experiment modal.](assets/projetos/experimentos-criar-modal.png)

7. Escolha um nome para o experimento e clique no botão **Criar**
![A screenshot of new experiment modal. Button "Criar" is highlighted.](assets/projetos/experimentos-criar-modal-botao.png)

## Montar fluxo de experimento

1. Clique na aba **Projetos** do menu.
![A screenshot of welcome page. Button "Projetos" is highlighted.](assets/projetos/home.png)

2. O usuário será redirecionado para a página dos projetos, onde será listado todas os projetos existentes.
![A screenshot of projects page.](assets/projetos/projetos.png)

3. Selecione o projeto desejado, clicando no nome do projeto.
![A screenshot of projects page. One project is highlighted.](assets/projetos/projetos-lista.png)

4. O usuário será redirecionado para a página de experimentos do projeto selecionado.
![A screenshot of experiments page.](assets/projetos/experimentos.png)

5. Selecione um experimento, clicando no nome do experimento.
![A screenshot of experiments page. One experiment is highlighted.](assets/projetos/experimentos-lista.png)

6. Após selecionado o experimento, o usuário estará habilitado para a montagem do fluxo.
![A screenshot of experiment flow.](assets/projetos/experimento-fluxo.png)

7. Para monta o fluxo, basta clicar nas tarefas desejadas.
![A screenshot of experiment flow. Tasks is highlighted.](assets/projetos/experimento-fluxo-tarefas.png)

8. Após clicar em uma tarefa, ela é adicionada automaticamente no fluxo.
![A screenshot of experiment flow with two tasks.](assets/projetos/experimento-fluxo-nova-tarefa.png)

### Remover tarefa do fluxo

1. Para remover uma tarefa do fluxo, basta clicar com o botão direito do mouse na tarefa desejada.
![A screenshot of tasks options in flow.](assets/projetos/experimento-fluxo-remover.png)

2. Clique em **Remover**.
![A screenshot of tasks options in flow. Button "Remover" is highlighted.](assets/projetos/experimento-fluxo-remover-botao.png)

### Dicas para montagem do fluxo
* Se o seu "alvo" é "Categorical", tem que terminar com um "Classifier" ou Logistic Regression.
* Se o seu "alvo" é "Numerical", tem que terminar com um "Regression",menos o Logistic Regression.

## Executar experimento


## Implantar um experimento

[TODO]

## Listar experimentos implantados

[TODO]

## Monitoramento de experimentos implantados

[TODO]

## Testar inferência de experimentos implantados

[TODO]
