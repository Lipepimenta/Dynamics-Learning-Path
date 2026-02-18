# Dynamics 365 F&O - VIP Credit Control  
### Learning Path aplicado em cenário empresarial

---

## 📌 Propósito do Projeto

Este repositório documenta minha jornada prática de aprendizado no Microsoft Dynamics 365 Finance & Operations.

A ideia não foi apenas criar uma funcionalidade, mas sim evoluí-la como aconteceria dentro de uma empresa real.  
Cada semana representa um passo de maturidade: sair de algo simples e funcional até chegar a uma solução mais estruturada, auditável e organizada.

Escolhi o cenário de controle de limite de crédito para clientes VIP como base para aplicar conceitos centrais do Dynamics em situações próximas do dia a dia empresarial.

---

# 🔹 Semana 1 — Fazendo Funcionar

## O que eu queria resolver

Comecei com um problema direto: impedir que o limite de crédito ultrapassasse um valor máximo definido.

Nada sofisticado.  
Eu queria entender como o Dynamics valida dados antes de gravar no banco.

## O que eu fiz

Implementei a regra diretamente no `validateWrite`, comparando o valor informado com um limite fixo (hardcoded).

As mensagens estavam escritas diretamente no código e exibidas via InfoLog.  
Se o valor ultrapassasse o limite, o registro simplesmente não era salvo.

Funcionava. Simples assim.

## O que eu aprendi

- Como o `validateWrite` participa do ciclo de gravação
- Como usar `checkFailed` para bloquear a operação
- Que fazer funcionar é só o primeiro passo

Na prática, percebi que a solução resolvia o problema, mas não estava preparada para crescer.

<details>
  <summary>🖼️ Ver validação bloqueando limite acima do permitido</summary>
  <br>
  <img src="https://github.com/user-attachments/assets/2d07b8ff-aad6-4880-aa6b-66900bc6d034" alt="Validação limite hardcoded" width="1916" height="646" />
</details>

# 🔹 Semana 2 — Internacionalização e Boas Práticas

Com a ideia validada, o foco passou a ser a padronização técnica.

Uma solução empresarial não pode depender de strings fixas nem ignorar padrões de internacionalização.  
Nesta etapa, o objetivo foi alinhar o código às boas práticas recomendadas para ambientes corporativos e multilíngues.

## Evolução Técnica

As mensagens exibidas ao usuário foram migradas para um arquivo de Labels (AxLabel), eliminando strings fixas no código.

Além disso, a validação foi refinada para impedir valores negativos, garantindo consistência dos dados antes da gravação.

A estrutura do código foi ajustada para seguir o padrão do framework, utilizando retornos booleanos e `checkFailed` apenas quando necessário.

## Aprendizado

- Criação e uso de Labels (AxLabel)
- Eliminação de hardcode textual
- Validação de consistência de dados
- Aderência às boas práticas do framework

# 🔹 Semana 3 — Parametrização, Segmentação e Automação

Nesta fase, a solução deixou de ser apenas técnica e passou a ser administrável e operacional.

O limite não deveria estar no código.  
Deveria estar configurável na interface.

Além disso, surgiu a necessidade de processar alterações em massa, introduzindo o uso de Batch.

## Evolução Técnica

Foram criados campos de configuração em tabela de parâmetros, incluindo:

- Toggle de ativação da funcionalidade
- Campo numérico para limite máximo configurável

A regra passou a ser aplicada apenas a clientes pertencentes ao grupo VIP (`CustGroup`).

A validação passou a considerar três condições:

- A funcionalidade precisa estar ativa
- O cliente deve pertencer ao grupo VIP
- O valor informado não pode ultrapassar o limite configurado

Com isso, ajustes de negócio passaram a ser feitos diretamente pela interface, sem necessidade de deploy.

### 🔄 Implementação do Batch (Evolução Arquitetural)

O processamento em massa foi inicialmente implementado de forma direta, com uma classe executando a lógica de atualização.

Embora funcional, essa abordagem não permitia integração adequada com Menu Items nem seguia o padrão do Batch Framework.

Para resolver isso, a solução foi refatorada para o padrão MVC, separando responsabilidades em:

- Data Contract
- Classe de execução
- Estrutura compatível com Batch Job

Após a refatoração, o processo pôde ser exposto via Menu Item, permitindo execução manual ou agendada, alinhada ao padrão do Dynamics.

Este foi um ponto-chave do aprendizado: entender quando refatorar para aderir à arquitetura do framework.

## Aprendizado

- Parametrização via tabela
- Segmentação por grupo de clientes (`CustGroup`)
- Introdução ao Batch Framework
- Refatoração para padrão MVC
- Disponibilização do Batch via Menu Item

<details>
  <summary>🖼️ Ver parametro de bloqueio de limite</summary>
  <br>
  <img width="1918" height="642" alt="creditCard" src="https://github.com/user-attachments/assets/fdbd5a7f-fdab-4feb-b7c6-c191da30beca" />
</details>

# 🔹 Semana 4 — Auditoria, Segurança e Governança

Com a parametrização e o processamento em massa estruturados, o foco passou a ser controle e governança.

Uma solução empresarial não deve apenas executar regras.  
Ela precisa registrar, proteger e organizar.

## Evolução Técnica

Foi criada a tabela `CustVIPLog`, responsável por registrar cada alteração de limite realizada pelo sistema.

Os registros armazenam:

- Conta do cliente
- Usuário responsável
- Valor anterior
- Novo valor
- Data e hora
- Tipo de execução (Manual ou Batch)

Um formulário dedicado foi desenvolvido para consulta desses registros, permitindo auditoria completa das movimentações.

Para garantir segurança adequada, foram implementados Security Privileges, restringindo o acesso ao log apenas a usuários autorizados.

Além disso, o repositório foi reorganizado seguindo o padrão `PackagesLocalDirectory`, mantendo versionamento apenas de metadados (XML) e estabilizando o branch principal.

A solução passou a oferecer:

- Rastreabilidade completa
- Controle de acesso estruturado
- Organização profissional de código

## Aprendizado

- Criação de tabela e formulário customizados
- Registro automatizado de eventos
- Segurança baseada em privilégios
- Governança de repositório no padrão oficial

<details>
<summary>🖼️ Evidência Visual - Clique para expandir</summary>
<br>
<summary>Ações no modulo</summary>
<img width="930" height="1010" alt="image" src="https://github.com/user-attachments/assets/ed164ebb-5a91-4cd9-8964-044fe1258e41" />
<br>
<summary>Tela do Batch</summary>
<img width="928" height="1009" alt="image" src="https://github.com/user-attachments/assets/d1de757a-b8d0-42a1-9493-3a54d7b2d723" />
<br>
<summary>Tela dos Logs</summary>
<img width="929" height="370" alt="image" src="https://github.com/user-attachments/assets/f7eb6cd2-8be4-42c6-ba19-d34a4db3c827" />
<br>
<summary>Tela do Privilégio</summary>
<img width="930" height="662" alt="image" src="https://github.com/user-attachments/assets/543be8cd-cd8b-4169-8f64-bb19df4982a9" />

</details>

---

# 📅 Linha do Tempo do Aprendizado

| Semana | Foco | Conceitos Dynamics Aplicados | Resultado Prático |
|--------|------|-----------------------------|-------------------|
| 1 | Prova de Conceito | `validateWrite`, lógica de tabela, InfoLog | Validação de limite máximo hardcoded |
| 2 | Internacionalização | Labels (AxLabel), validação negativa, padronização de mensagens | Código multilíngue e aderente às boas práticas |
| 3 | Parametrização e Automação | Tabela de parâmetros, segmentação por `CustGroup`, Batch Framework, refatoração para MVC, Menu Item | Regra configurável via interface e processamento em massa estruturado |
| 4 | Auditoria e Governança | Tabela de log customizada, formulário de consulta, Security Privileges, organização em `PackagesLocalDirectory` | Rastreabilidade completa e repositório profissional |

---

# Conclusão

Este projeto representa um estudo estruturado de maturidade arquitetural dentro do Dynamics 365 F&O.

A evolução seguiu um fluxo natural:

Ideia → Padronização → Parametrização → Automação → Auditoria → Governança

Mais do que implementar uma funcionalidade, o objetivo foi compreender como uma solução enterprise nasce simples e evolui para se tornar sustentável, auditável e administrável.

Este repositório documenta essa jornada prática de aprendizado aplicada a um cenário realista do dia a dia empresarial.

