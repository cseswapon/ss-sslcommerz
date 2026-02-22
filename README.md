
````md
# ss-sslcommerz

A TypeScript-compatible, framework-agnostic wrapper for the [SSLCommerz](https://www.sslcommerz.com/) payment gateway. Developed by **Swapon Saha**. This module works with Node.js, NestJS, Express, or any JavaScript/TypeScript environment.

---

## 🚀 Features

- ✅ TypeScript & JavaScript support
- ✅ NestJS/Express/Node.js compatible
- ✅ Sandbox & Live modes
- ✅ Full SSLCommerz API coverage:
  - Payment initialization
  - Payment validation
  - Refund initiation
  - Refund status query
  - Transaction query (by session or transaction ID)

---

## 📦 Installation

Install the package using npm:

```bash
npm install ss-sslcommerz
````

---

## 🚀 Usage Example (NestJS, Express, Node.js Compatible)

### Step 1: Import the Package

```ts
import SSLCommerzPayment from 'ss-sslcommerz';
```

---

### Step 2: Initialize the Payment Client

```ts
import SSLCommerzPayment from "ss-sslcommerz";

const sslcz = new SSLCommerzPayment(
  "your_store_id",
  "your_store_password",
  false,
);

export default sslcz;
```

---

### Step 3: Create a Payment Session

```ts
const session = await sslcz.init({
  store_passwd:"example",
  store_id:"example",
  tran_id: 'TRANSACTION_12345',
  total_amount: 1000,
  currency: 'BDT',
  success_url: 'https://yourdomain.com/success',
  fail_url: 'https://yourdomain.com/fail',
  cancel_url: 'https://yourdomain.com/cancel',
  cus_name: 'Swapon Saha',
  cus_email: 'swapon@example.com',
  cus_add1: 'Mirpur, Dhaka',
  cus_city: 'Dhaka',
  cus_state: 'Dhaka',
  cus_postcode: '1216',
  cus_country: 'Bangladesh',
  cus_phone: '017xxxxxxxx',
  shipping_method: 'Courier',
  num_of_item: 1,
  product_name: 'Flight Ticket',
  product_category: 'Travel',
  product_profile: 'general'
});

console.log(session);
```

---

## ✅ Other API Methods

### 🔹 Validate Payment

```ts
const validation = await sslcz.validate({ val_id: 'SSL_VALIDATION_ID' });
```

### 🔹 Refund Request

```ts
const refund = await sslcz.initiateRefund({
  refund_amount: '500',
  refund_remarks: 'Customer canceled',
  bank_tran_id: 'BANK123',
  refe_id: 'REF123'
});
```

### 🔹 Refund Query

```ts
const result = await sslcz.refundQuery({ refund_ref_id: 'REFUND_456' });
```

### 🔹 Query Transaction by Session ID or Transaction ID

```ts
await sslcz.transactionQueryBySessionId({ sessionkey: 'session_xyz' });
await sslcz.transactionQueryByTransactionId({ tran_id: 'tran_abc' });
```

---

## 🧠 Tips

* Always use the correct `store_id` and `store_password`.
* Set `live = true` in constructor for production.
* Handle API responses properly and validate fields from SSLCommerz response.

---

## 👨‍💻 Maintained By

**Swapon Saha**
🔗 GitHub: [cseswapon](https://github.com/cseswapon)

---

## 📄 License

Licensed under the [ISC License](LICENSE).

---

## 🤝 Contribute

Pull requests and stars are welcome! 🙌
If you found this useful, feel free to ⭐ the repo.

