---
name: refactor
description: 'Refatoracao cirurgica de codigo para melhorar manutenibilidade sem mudar comportamento. Cobre extracao de funcoes, renomeacao de variaveis, quebra de funcoes enormes, melhoria de type safety, eliminacao de code smells e aplicacao de design patterns. Menos drastico que repo-rebuilder; use para melhorias graduais.'
license: MIT
---

# Refatoracao

## Visao Geral

Melhore estrutura e legibilidade do codigo sem alterar comportamento externo. Refatoracao e evolucao gradual, nao revolucao. Use para melhorar codigo existente, nao para reescrever do zero.

## Quando Usar

Use esta skill quando:

- Codigo e dificil de entender ou manter
- Funcoes/classes sao grandes demais
- Code smells precisam ser enderecados
- Adicionar features e dificil por causa da estrutura
- Usuario pede "clean up this code", "refactor this", "improve this"

---

## Principios de Refatoracao

### Regras de Ouro

1. **Comportamento e preservado** - Refatoracao nao muda o que o codigo faz, apenas como
2. **Passos pequenos** - Faça mudancas pequenas, teste apos cada uma
3. **Controle de versao e seu amigo** - Commite antes e depois de cada estado seguro
4. **Testes sao essenciais** - Sem testes, nao e refatoracao, e edicao
5. **Uma coisa por vez** - Nao misture refatoracao com mudancas de feature

### Quando NAO Refatorar

```
- Codigo que funciona e nao vai mudar (if it ain't broke...)
- Codigo critico em producao sem testes (adicione testes primeiro)
- Quando estiver sob deadline apertado
- "So porque sim" - precisa de um proposito claro
```

---

## Code Smells (Maus Cheiros) Comuns e Correcoes

### 1. Metodo/Funcao Longa

```diff
# RUIM: funcao de 200 linhas que faz tudo
- async function processOrder(orderId) {
-   // 50 lines: fetch order
-   // 30 lines: validate order
-   // 40 lines: calculate pricing
-   // 30 lines: update inventory
-   // 20 lines: create shipment
-   // 30 lines: send notifications
- }

# BOM: quebrada em funcoes focadas
+ async function processOrder(orderId) {
+   const order = await fetchOrder(orderId);
+   validateOrder(order);
+   const pricing = calculatePricing(order);
+   await updateInventory(order);
+   const shipment = await createShipment(order);
+   await sendNotifications(order, pricing, shipment);
+   return { order, pricing, shipment };
+ }
```

### 2. Codigo Duplicado

```diff
# RUIM: mesma logica em varios lugares
- function calculateUserDiscount(user) {
-   if (user.membership === 'gold') return user.total * 0.2;
-   if (user.membership === 'silver') return user.total * 0.1;
-   return 0;
- }
-
- function calculateOrderDiscount(order) {
-   if (order.user.membership === 'gold') return order.total * 0.2;
-   if (order.user.membership === 'silver') return order.total * 0.1;
-   return 0;
- }

# BOM: extraia logica comum
+ function getMembershipDiscountRate(membership) {
+   const rates = { gold: 0.2, silver: 0.1 };
+   return rates[membership] || 0;
+ }
+
+ function calculateUserDiscount(user) {
+   return user.total * getMembershipDiscountRate(user.membership);
+ }
+
+ function calculateOrderDiscount(order) {
+   return order.total * getMembershipDiscountRate(order.user.membership);
+ }
```

### 3. Classe/Modulo Grande

```diff
# RUIM: god object que sabe demais
- class UserManager {
-   createUser() { /* ... */ }
-   updateUser() { /* ... */ }
-   deleteUser() { /* ... */ }
-   sendEmail() { /* ... */ }
-   generateReport() { /* ... */ }
-   handlePayment() { /* ... */ }
-   validateAddress() { /* ... */ }
-   // 50 more methods...
- }

# BOM: responsabilidade unica por classe
+ class UserService {
+   create(data) { /* ... */ }
+   update(id, data) { /* ... */ }
+   delete(id) { /* ... */ }
+ }
+
+ class EmailService {
+   send(to, subject, body) { /* ... */ }
+ }
+
+ class ReportService {
+   generate(type, params) { /* ... */ }
+ }
+
+ class PaymentService {
+   process(amount, method) { /* ... */ }
+ }
```

### 4. Lista Longa de Parametros

```diff
# RUIM: parametros demais
- function createUser(email, password, name, age, address, city, country, phone) {
-   /* ... */
- }

# BOM: agrupe parametros relacionados
+ interface UserData {
+   email: string;
+   password: string;
+   name: string;
+   age?: number;
+   address?: Address;
+   phone?: string;
+ }
+
+ function createUser(data: UserData) {
+   /* ... */
+ }

# AINDA MELHOR: use builder pattern para construcao complexa
+ const user = UserBuilder
+   .email('test@example.com')
+   .password('secure123')
+   .name('Test User')
+   .address(address)
+   .build();
```

### 5. Inveja de Funcionalidade (Feature Envy)

```diff
# RUIM: metodo que usa mais dados de outro objeto do que os proprios
- class Order {
-   calculateDiscount(user) {
-     if (user.membershipLevel === 'gold') {
+       return this.total * 0.2;
+     }
+     if (user.accountAge > 365) {
+       return this.total * 0.1;
+     }
+     return 0;
+   }
+ }

# BOM: mova a logica para o objeto que possui os dados
+ class User {
+   getDiscountRate(orderTotal) {
+     if (this.membershipLevel === 'gold') return 0.2;
+     if (this.accountAge > 365) return 0.1;
+     return 0;
+   }
+ }
+
+ class Order {
+   calculateDiscount(user) {
+     return this.total * user.getDiscountRate(this.total);
+   }
+ }
```
