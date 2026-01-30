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


---
*Em desenvolvimento.*
