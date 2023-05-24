# Design Patterns - CBL

## O que é Design Pattern?

Um Design Pattern, ou padrão de projeto, é uma solução reutilizável para um problema comum que ocorre durante o processo de design e desenvolvimento de software. Ele descreve uma abordagem testada e comprovada para resolver um problema específico, fornecendo diretrizes e estruturas que podem ser aplicadas em diferentes contextos.

Os Design Patterns são criados com base em boas práticas e experiências anteriores, e têm como objetivo ajudar os desenvolvedores a resolver problemas de design de forma eficiente, promovendo a reutilização de soluções já estabelecidas. Eles fornecem uma linguagem comum para comunicar e compartilhar conhecimento sobre design de software entre os membros de uma equipe de desenvolvimento.

Os Design Patterns podem ser classificados em três categorias principais: padrões de criação, padrões estruturais e padrões comportamentais. Cada categoria aborda um conjunto específico de problemas de design e oferece soluções adequadas a esses problemas.

Ao aplicar um Design Pattern, é importante adaptá-lo ao contexto e aos requisitos do projeto, levando em consideração as características e restrições específicas. Os Design Patterns não são soluções universais, mas sim ferramentas que podem ser utilizadas de forma flexível e adaptada às necessidades de cada projeto.

Em resumo, um Design Pattern é uma solução comprovada e reutilizável para um problema comum de design de software, que proporciona eficiência, clareza e qualidade ao desenvolvimento de software.

## Quais são os tipo de Design Pattern?

1. Padrões de Criação (Creational Patterns):
    - Factory Method
    - Abstract Factory
    - Singleton
    - Builder
    - Prototype

2. Padrões Estruturais (Structural Patterns):
    - Adapter
    - Bridge
    - Composite
    - Decorator
    - Facade
    - Flyweight
    - Proxy

3. Padrões Comportamentais (Behavioral Patterns):
    - Chain of Responsibility
    - Command
    - Interpreter
    - Iterator
    - Mediator
    - Memento
    - Observer
    - State
    - Strategy
    - Template Method
    - Visitor

4. Padrões de Arquitetura (Architectural Patterns):
    - Model-View-Controller (MVC)
    - Model-View-Presenter (MVP)
    - Model-View-ViewModel (MVVM)
    - Layered Architecture
    - Microservices

5. Padrões de Concorrência (Concurrency Patterns):
    - Active Object
    - Barrier
    - Read-Write Lock
    - Monitor Object
    - Thread Pool

6. Padrões de Segurança (Security Patterns):
    - Access Control
    - Authentication
    - Authorization
    - Encryption
    - Secure Proxy

7. Padrões de Integração (Integration Patterns):
    - Message Queue
    - Publish-Subscribe
    - Service Bus
    - Data Mapper
    - Data Transfer Object (DTO)

## Architectural Patterns

Padrões de Arquitetura (Architectural Patterns) são padrões que fornecem diretrizes de alto nível para a estruturação e organização de sistemas de software. Eles abordam a arquitetura geral do sistema, definindo a forma como os componentes se relacionam e interagem entre si para atender aos requisitos funcionais e não funcionais do sistema. Esses padrões ajudam a criar sistemas escaláveis, flexíveis e de fácil manutenção.

1. Model-View-Controller (MVC):

    - O padrão MVC separa a lógica de apresentação (View) da lógica de negócios (Model) e da lógica de controle (Controller). Isso permite a separação de preocupações e facilita a modificação e reutilização de componentes.

2. Model-View-Presenter (MVP):

    - O padrão MVP é semelhante ao MVC, mas com ênfase na separação da lógica de apresentação (View) e da lógica de negócios (Model), usando um Presenter como intermediário entre eles.

3. Model-View-ViewModel (MVVM):

    - O padrão MVVM é usado em aplicativos de interface de usuário, onde a lógica de apresentação (View) é separada da lógica de negócios (Model) usando um ViewModel como intermediário. Ele suporta a ligação de dados bidirecional entre a View e o ViewModel.

4. Layered Architecture:

    - A arquitetura em camadas divide o sistema em camadas distintas, como a camada de apresentação, camada de negócios e camada de acesso a dados. Cada camada tem responsabilidades específicas e se comunica com as camadas adjacentes de forma bem definida.

5. Microservices:

    - O padrão de arquitetura de microservices divide o sistema em componentes independentes e autônomos chamados microservices. Cada microservice é responsável por uma funcionalidade específica e se comunica com outros microservices por meio de APIs. Isso permite a escalabilidade e a manutenção de sistemas complexos.

Vamos considerar um cenário em que temos um aplicativo de lista de tarefas (to-do list). Faremos uma implementação básica de cada padrão para mostrar como eles se diferenciam na organização e interação dos componentes.

### MVC: 

```javascript

// Model
class Task {
  constructor(description, completed = false) {
    this.description = description;
    this.completed = completed;
  }
}

// View
class TaskView {
  render(task) {
    console.log(`[${task.completed ? 'x' : ' '}] ${task.description}`);
  }
}

// Controller
class TaskController {
  constructor() {
    this.taskView = new TaskView();
  }
  
  createTask(description) {
    const task = new Task(description);
    this.taskView.render(task);
  }
}

// Utilização do MVC
const taskController = new TaskController();
taskController.createTask("Fazer compras");
```

Neste exemplo, o Model representa a estrutura da tarefa. A View é responsável por renderizar a tarefa na interface do usuário. O Controller atua como intermediário, controlando as interações entre a View e o Model. O Controller cria uma nova tarefa e solicita à View que a renderize.

### MVP:

```javascript
// Model
class Task {
  constructor(description, completed = false) {
    this.description = description;
    this.completed = completed;
  }
}

// View
class TaskView {
  render(task) {
    console.log(`[${task.completed ? 'x' : ' '}] ${task.description}`);
  }
}

// Presenter
class TaskPresenter {
  constructor() {
    this.taskView = new TaskView();
  }
  
  createTask(description) {
    const task = new Task(description);
    this.taskView.render(task);
  }
}

// Utilização do MVP
const taskPresenter = new TaskPresenter();
taskPresenter.createTask("Fazer compras");
```

Neste exemplo de MVP, a diferença é que o Presenter é responsável por orquestrar a lógica de apresentação e manipulação de eventos. Ele recebe as ações da View e atualiza o Model e a View conforme necessário.

### MVVM:

```javascript
// Model
class Task {
  constructor(description, completed = false) {
    this.description = description;
    this.completed = completed;
  }
}

// ViewModel
class TaskViewModel {
  constructor() {
    this.tasks = [];
  }
  
  createTask(description) {
    const task = new Task(description);
    this.tasks.push(task);
  }
}

// Utilização do MVVM
const taskViewModel = new TaskViewModel();
taskViewModel.createTask("Fazer compras");
console.log(taskViewModel.tasks);
```

No exemplo de MVVM, o ViewModel é responsável por encapsular os dados e a lógica de apresentação. Ele possui uma propriedade tasks que armazena as tarefas e fornece métodos para manipulá-las. Nesse caso, o ViewModel é utilizado diretamente para criar uma nova tarefa.

Esses exemplos mostram uma visão geral das diferenças entre os subtipos de padrões de arquitetura. Cada um deles tem abordagens distintas para separar as responsabilidades e organizar os componentes em um sistema. É importante observar que esses exemplos são simplificados e podem variar dependendo do tamanho e complexidade do projeto real.

## Links

- https://refactoring.guru/design-patterns
- https://www.alura.com.br/artigos/design-patterns-introducao-padroes-projeto
- https://sourcemaking.com/design_patterns
- https://www.freecodecamp.org/news/the-basic-design-patterns-all-developers-need-to-know/

## Apresentação 

https://www.canva.com/design/DAFj1a7aEyg/A0kLkKiFIiJ2CMSPHpChYQ/edit?utm_content=DAFj1a7aEyg&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton
