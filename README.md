# Jornada Dynamics 365 F&O

Este repositório documenta minha evolução no desenvolvimento para Microsoft Dynamics 365 Finance & Operations.

## 📅 Semana 1: Fundamentos e Validações
**Foco:** Criação de tabelas, formulários e lógica de validação via X++.

### ✨ O que foi feito
1.  **Criação de Tabela e Campos:**
    * Adicionado campo `ABCAtivarValidacaoVIP` nos Parâmetros de Clientes.
    * Adicionado campo `CreditMax` (Limite de Crédito) na tabela de Clientes.
2.  **Interface (Forms):**
    * Criação de extensão de formulário para exibir os novos parâmetros.
3.  **Lógica de Negócios (X++):**
    * Implementação de validação na classe `CustTable`.
    * Regra: Impede salvar se o limite for > 18.000 E o parâmetro VIP estiver ativado.
4.  **Debug:**
    * Configuração de breakpoints e inspeção de variáveis no Visual Studio.

### 📸 Prints das Funcionalidades

<details>
  <summary><strong>1. Tela de Parâmetros (Configuração VIP) - Clique para expandir</strong> 🔽</summary>
  <br>
  <img width="1918" height="642" alt="creditCard" src="https://github.com/user-attachments/assets/71644ccb-7c9c-4d7f-8680-cbb1cf8f5487" />
</details>
<br>
<details>
  <summary><strong>2. Erro de Validação no Cliente - Clique para expandir</strong> 🔽</summary>
  <br>
  <img width="1916" height="646" alt="customers" src="https://github.com/user-attachments/assets/9f9ddd58-e740-4069-8267-3bcc6eb2f222" />
</details>

## 📅 Semana 2: Manipulação de Dados e "Batch Jobs"
**Foco:** Criação de rotinas de atualização em massa via X++ (Runnable Class).

### ✨ O que foi feito
1.  **Job de Atualização em Massa:**
    * Desenvolvimento de `Runnable Class` para varrer a tabela de Clientes.
    * Uso de `while select forUpdate` para filtrar apenas o grupo 'VIP'.
    * Uso de Transações (`ttsbegin` / `ttscommit`) para segurança dos dados.
2.  **Integração com Validações:**
    * Ajuste no Job para respeitar a lógica de negócios criada na Semana 1.
    * Uso explícito de `.validateWrite()` para garantir que regras de limite de crédito sejam checadas antes do `.update()`.

### 📸 Prints das Funcionalidades

<details>
  <summary><strong>Log de Execução (Bloqueio de Regra de Negócio) - Clique para expandir</strong> 🔽</summary>
  <br>
  <img width="929" height="275" alt="blockingMessage" src="https://github.com/user-attachments/assets/e454c2f6-9f33-47a8-83c0-60f604b54b4e" />
</details>

## 📅 Semana 3: Framework SysOperation (Padrão MVC)

**Foco:** Evolução de Jobs manuais para o framework robusto de operações em lote (SysOperation Framework) e integração ao menu do sistema.

### ✨ O que foi feito

* **Arquitetura MVC (Model-View-Controller):**
    * Refatoração da lógica para separar responsabilidades, abandonando o uso de *Runnable Classes* (Jobs) simples para processos complexos.
* **Implementação dos Componentes:**
    * **Contract (O Bagageiro):** Criação de classe `DataContract` para definir parâmetros de entrada (ex: datas, valores) e gerar automaticamente os campos da janela de diálogo.
    * **Service (O Motor):** Centralização da lógica de negócios (atualização dos clientes VIP) em uma classe de serviço pura, desacoplada da interface.
    * **Controller (O Piloto):** Configuração da classe controladora que orquestra a execução e define o modo de operação (Síncrono/Assíncrono).
* **Integração e Acesso:**
    * **Menu Item:** Criação de item de menu do tipo *Action* apontando para o Controller.
    * **Módulo Contas a Receber:** Inserção da funcionalidade no menu principal do módulo através de uma *Menu Extension*.
* **Interface de Usuário (Dialog):**
    * Geração automática da interface de parâmetros para o usuário final baseada no *Contract*.
    * Validação de dados direto na entrada do usuário.

### 📸 Prints das Funcionalidades

<details>
  <summary><strong>1. Seleção da ação de menu (Atualiza Cliente VIP) - Clique para expandir</strong> 🔽</summary>
  <br>
  <img width="953" height="732" alt="menu-item" src="https://github.com/user-attachments/assets/c23c0081-e02d-4ad8-adef-f4bb3eefe825" />
</details>
<br>
<details>
  <summary><strong>2. Janela de Diálogo (SysOperation) - Clique para expandir</strong> 🔽</summary>
  <br>
  <img width="960" height="1038" alt="screen-job" src="https://github.com/user-attachments/assets/78653367-6d81-4dbb-98c5-b0d9f13957bd" />
</details>

---

### 🛠 Tecnologias Utilizadas
* **Linguagem:** X++
* **IDE:** Visual Studio (Finance & Operations Tools)
* **Banco de Dados:** SQL Server (AxDB)
