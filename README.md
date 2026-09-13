# VB6: Limitação do Ctrl+Z (Undo) a 20 alterações

## Introdução

O **Microsoft Visual Basic 6 (VB6)** possui uma limitação histórica em seu editor de código: o mecanismo de **Undo (Ctrl+Z)** mantém um histórico limitado das últimas **20 ações de edição**.

Para quem ainda trabalha com projetos VB6, principalmente projetos antigos e de grande porte, essa limitação pode causar bastante dificuldade durante o desenvolvimento e a manutenção do código.

> **Importante:** o limite de 20 refere-se a ações/edições registradas pelo editor, e não necessariamente a 20 teclas pressionadas.

---

## Como funciona

Quando uma alteração é realizada no editor de código, o VB6 adiciona essa ação à pilha de Undo.

De forma simplificada:

```text
Alteração 1
Alteração 2
Alteração 3
...
Alteração 20
```

Ao pressionar:

```text
Ctrl + Z
```

o VB6 desfaz a alteração mais recente.

Se continuarmos utilizando `Ctrl + Z`, as alterações anteriores podem ser restauradas até o limite disponível na pilha.

O problema aparece quando já foram realizadas mais de 20 ações.

Por exemplo:

```text
Alteração 1
Alteração 2
Alteração 3
...
Alteração 20
Alteração 21
Alteração 22
Alteração 23
Alteração 24
Alteração 25
```

Quando chegamos a esse ponto, as alterações mais antigas deixam de estar disponíveis no histórico de Undo.

Assim, não podemos simplesmente pressionar `Ctrl + Z` várias vezes e retornar ao estado anterior a todas as 25 alterações.

---

## "20 teclas" ou "20 interações"?

É importante fazer uma distinção.

O VB6 trabalha com **ações de edição**, e não simplesmente com cada caractere digitado.

Por exemplo, determinadas operações podem ser tratadas como uma ação:

```vb
Dim Cliente As String
```

Da mesma forma, uma operação de copiar e colar um bloco de código pode representar uma alteração significativa no histórico.

Portanto, a maneira mais correta de descrever a limitação é:

> **O editor de código do VB6 possui uma pilha de Undo limitada a aproximadamente 20 ações de edição.**

---

## Por que isso é um problema?

Imagine um cenário de manutenção:

1. Abrimos um módulo antigo.
2. Alteramos uma função.
3. Criamos uma variável.
4. Movemos um trecho de código.
5. Colamos uma nova rotina.
6. Fazemos algumas correções.
7. Executamos o projeto.
8. Encontramos outro problema.
9. Fazemos novas alterações.
10. Continuamos trabalhando.

Depois de várias operações, percebemos que uma alteração realizada anteriormente estava errada.

Então pensamos:

> "Vou voltar usando Ctrl+Z."

O problema é que, se essa alteração estiver além das últimas 20 ações registradas, o VB6 não conseguirá mais desfazê-la através do Undo.

---

## Undo não é controle de versão

Esse é um ponto fundamental.

O `Ctrl+Z` do VB6 não deve ser considerado um sistema de versionamento do projeto.

O Undo serve para desfazer ações recentes durante uma sessão de edição.

Ele não substitui ferramentas como:

- Git
- GitHub
- GitLab
- SVN
- backups
- cópias versionadas do projeto

Um sistema de versionamento permite trabalhar com uma linha histórica muito maior:

```text
v1.0 ── Projeto funcionando
  │
  ├── v1.1 ── Nova rotina
  │
  ├── v1.2 ── Alteração no banco
  │
  ├── v1.3 ── Nova funcionalidade
  │
  └── v1.4 ── Alteração problemática
```

Se a versão `v1.4` apresentar um problema, podemos comparar ou recuperar uma versão anterior.

---

# Uma solução interessante: ModernVB

Existe um projeto chamado **ModernVB**, desenvolvido para modernizar algumas características da experiência de desenvolvimento com VB6.

Entre suas melhorias está justamente a possibilidade de ampliar o histórico de **Undo/Redo**, eliminando a limitação prática dos 20 níveis do editor original.

Projeto:

https://github.com/VykosX/ModernVB

O projeto é particularmente interessante para desenvolvedores que continuam utilizando VB6 atualmente.

---

## Mas é preciso ter cuidado

Modificar o ambiente de desenvolvimento do VB6 pode ser útil, mas deve ser feito com cautela.

O VB6 é uma ferramenta antiga e muitos projetos dependem de:

- controles ActiveX;
- DLLs;
- OCXs;
- APIs do Windows;
- componentes de terceiros;
- referências específicas;
- versões antigas do runtime;
- ferramentas auxiliares.

Por isso, antes de modificar uma instalação de produção do VB6, é recomendável manter uma cópia de segurança e, de preferência, testar a alteração em um ambiente separado.

---

# Uma abordagem mais segura: Git + VB6

Mesmo utilizando uma solução para aumentar o Undo, eu recomendaria utilizar **Git** para controlar o histórico do projeto.

Uma estrutura simples poderia ser:

```text
ProjetoVB6/
│
├── Projeto.vbp
├── Formulario.frm
├── Modulo.bas
├── Classe.cls
├── Banco.bas
│
└── .git/
```

E os commits poderiam representar pontos importantes:

```text
Inicialização do projeto
    ↓
Correção do cadastro de clientes
    ↓
Implementação de consulta
    ↓
Alteração da conexão com banco
    ↓
Nova funcionalidade
```

Dessa forma, temos duas camadas de segurança:

```text
VB6
 │
 └── Ctrl+Z
       └── Desfaz alterações recentes

Git
 │
 └── Histórico do projeto
       ├── versões
       ├── commits
       ├── comparação de alterações
       └── recuperação de versões
```

---

# Conclusão

O limite de aproximadamente **20 ações de Undo** é uma característica histórica do editor do VB6.

Para pequenos ajustes, normalmente não representa um grande problema.

Porém, em projetos grandes ou durante refatorações, esse limite pode ser bastante incômodo.

A melhor estratégia é não depender exclusivamente do `Ctrl+Z`.

Uma combinação recomendada é:

```text
VB6
 +
Ctrl+Z
 +
Git
 +
Backups
```

O `Ctrl+Z` resolve erros imediatos.

O **Git** mantém o histórico do projeto.

Os **backups** protegem contra problemas maiores, como perda ou corrupção dos arquivos.

---

## Referências

### Microsoft

Documentação relacionada ao menu Edit e às operações Undo/Redo:

https://learn.microsoft.com/en-us/office/vba/language/reference/user-interface-help/edit-menu

### ModernVB

Projeto que busca modernizar a experiência de desenvolvimento do VB6, incluindo melhorias relacionadas ao Undo/Redo:

https://github.com/VykosX/ModernVB

---

## Observação

O VB6 continua sendo utilizado em muitos sistemas legados. Apesar de suas limitações, ele ainda possui uma grande quantidade de aplicações em produção.

Conhecer essas limitações e adotar ferramentas complementares, como Git e sistemas de backup, é uma maneira prática de tornar a manutenção desses projetos mais segura.
