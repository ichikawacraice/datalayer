
# 📊 Documentação de Implementação GA4 Ecommerce  
**Modelo: Standard Page Load**

## 🎯 Diretriz de Identificação de Produtos

| Campo | Regra |
|------|------|
| `item_id` | **Sempre o ProductID** (ID do produto pai) |
| `item_variant` | **Sempre o VariantID** (SKU / variação) |

---

## 1. Page View (Visita à Página)

**Momento**  
No topo de **todas as páginas**, antes de qualquer outro evento.

```javascript
window.dataLayer = window.dataLayer || [];
dataLayer.push({
  "event": "page_view",
  "page_title": document.title,
  "page_location": window.location.href,
  "user_id": "USER_123" // Opcional: enviar apenas se o usuário estiver logado
});
```

---

## 2. View Item List (Visualização de Lista / Vitrine)

**Momento**  
Páginas de **Categoria**, **Busca** ou **Carrosséis** na Home.

```javascript
dataLayer.push({
  "event": "view_item_list",
  "ecommerce": {
    "item_list_name": "Categoria: Verão 2026",
    "items": [
      {
        "item_id": "PROD_001",
        "item_name": "Camiseta Algodão",
        "item_brand": "Marca Própria",
        "item_category": "Vestuário",
        "item_category2": "Masculino",
        "price": 89.90,
        "index": 1
      }
    ]
  }
});
```

---

## 3. View Item (Página de Detalhes do Produto – PDP)

**Momento**  
Quando a página do produto carrega.

```javascript
dataLayer.push({
  "event": "view_item",
  "ecommerce": {
    "currency": "BRL",
    "value": 89.90,
    "items": [
      {
        "item_id": "PROD_001",
        "item_name": "Camiseta Algodão",
        "item_brand": "Marca Própria",
        "item_category": "Vestuário",
        "item_category2": "Masculino",
        "item_variant": "SKU_AZUL_G", // ou em branco
        "price": 89.90
      }
    ]
  }
});
```

---

## 4. Add to Cart (Adicionar ao Carrinho)

**Momento**  
No clique do botão **Comprar / Adicionar ao Carrinho**.

```javascript
dataLayer.push({
  "event": "add_to_cart",
  "ecommerce": {
    "currency": "BRL",
    "value": 89.90,
    "items": [
      {
        "item_id": "PROD_001",
        "item_name": "Camiseta Algodão",
        "item_variant": "SKU_AZUL_G",
        "price": 89.90,
        "quantity": 1
      }
    ]
  }
});
```

---

## 5. Begin Checkout (Início do Checkout)

**Momento**  
No carregamento da **primeira etapa do checkout**.

```javascript
dataLayer.push({
  "event": "begin_checkout",
  "ecommerce": {
    "currency": "BRL",
    "value": 179.80,
    "items": [
      {
        "item_id": "PROD_001",
        "item_name": "Camiseta Algodão",
        "item_category": "Vestuário",
        "item_variant": "SKU_AZUL_G",
        "price": 89.90,
        "quantity": 2
      }
    ]
  }
});
```

---

## 6. Add Shipping Info & Add Payment Info

### 6.1 Add Shipping Info

**Momento**  
Quando o usuário seleciona o **tipo de frete**.

```javascript
dataLayer.push({
  "event": "add_shipping_info",
  "ecommerce": {
    "currency": "BRL",
    "value": 179.80,
    "shipping_tier": "Sedex",
    "items": [
      {
        "item_id": "PROD_001",
        "item_variant": "SKU_AZUL_G",
        "price": 89.90,
        "quantity": 2
      }
    ]
  }
});
```

---

### 6.2 Add Payment Info

**Momento**  
Quando o usuário escolhe a **forma de pagamento**.

```javascript
dataLayer.push({
  "event": "add_payment_info",
  "ecommerce": {
    "currency": "BRL",
    "value": 179.80,
    "payment_type": "Cartão de Crédito",
    "items": [
      {
        "item_id": "PROD_001",
        "item_variant": "SKU_AZUL_G",
        "price": 89.90,
        "quantity": 2
      }
    ]
  }
});
```

---

## 7. Purchase (Compra Finalizada)

**Momento**  
Página de sucesso / Obrigado.

### ⚠️ Regra de Ouro
- **`value` = Itens − Descontos**
- **Frete não entra no `value`**
- Frete deve ser enviado **apenas no campo `shipping`**

```javascript
dataLayer.push({
  "event": "purchase",
  "ecommerce": {
    "transaction_id": "PEDIDO_995544",
    "value": 169.80,
    "shipping": 25.00,
    "tax": 0.00,
    "currency": "BRL",
    "coupon": "BEMVINDO10",
    "items": [
      {
        "item_id": "PROD_001",
        "item_name": "Camiseta Algodão",
        "item_brand": "Marca Própria",
        "item_category": "Vestuário",
        "item_category2": "Masculino",
        "item_variant": "SKU_AZUL_G",
        "price": 84.90,
        "discount": 5.00,
        "quantity": 2
      }
    ]
  }
});
```
