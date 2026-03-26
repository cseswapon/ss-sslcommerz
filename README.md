````md
# ss-sslcommerz Integration for Node.js / Express / NestJS

A TypeScript-compatible, framework-agnostic integration for the [SSLCommerz](https://www.sslcommerz.com/) payment gateway. Developed by **Swapon Saha**. This module supports handling **Founder Donations** with automatic status update and frontend redirection.

---

## 🚀 Features

- ✅ TypeScript & JavaScript support
- ✅ Node.js / Express / NestJS compatible
- ✅ Sandbox & Live modes
- ✅ Full SSLCommerz API coverage:
  - Payment initialization
  - Payment validation
  - Refund initiation & query
  - Transaction query (by session or transaction ID)
- ✅ Automatic handling of **success**, **fail**, **cancel** callbacks
- ✅ Backend updates payment status in DB and redirects to frontend URL

---

## 📦 Installation

```bash
npm install ss-sslcommerz
````

---

## 🔧 Setup

### 1. Import and Initialize SSLCommerz

```ts
import * as SSLCommerz from "ss-sslcommerz";
const SSLCommerzPayment = (SSLCommerz as any).SSLCommerzPayment;

export const sslcz = new SSLCommerzPayment(
  process.env.SSL_STORE_ID as string,
  process.env.SSL_STORE_PASSWORD as string,
  false // set true for live
);
```

---

### 2. Create a Payment Session

```ts
import { sslcz } from './config/ssl.config';
import { env } from './config/env';

const session = await sslcz.init({
  store_passwd: env.SSL.STORE_PASSWORD,
  store_id: env.SSL.STORE_ID,
  tran_id: payment.transactionId, // unique transaction id from backend
  total_amount: payment.amount,
  currency: 'BDT',
  success_url: `${env.BACKEND_URL}/payment/success`,
  fail_url: `${env.BACKEND_URL}/payment/fail`,
  cancel_url: `${env.BACKEND_URL}/payment/cancel`,
  cus_name: founder.name,
  cus_email: founder.email,
  cus_add1: "N/A",
  cus_city: "N/A",
  cus_state: "N/A",
  cus_postcode: "0000",
  cus_country: "Bangladesh",
  cus_phone: founder.phone,
  shipping_method: "N/A",
  num_of_item: 1,
  product_name: "Founder Donation",
  product_category: "Found",
  product_profile: "general",
  ship_name: "N/A",
  ship_add1: "N/A",
  ship_city: "N/A",
  ship_state: "N/A",
  ship_postcode: "N/A",
  ship_country: "N/A"
});

console.log(session); // session.gw_url will be used to redirect user to payment page
```

---

### 3. Backend Routes for Callbacks

#### Success Route

```ts
app.post('/payment/success', async (req, res) => {
  const transactionId = req.body?.tran_id;
  await founderService.updatePaymentStatus(transactionId, PaymentStatus.SUCCESS);

  // Redirect to frontend success page
  res.redirect(`${env.FRONTEND_URL}/payment/success?transactionId=${transactionId}`);
});
```

#### Fail Route

```ts
app.post('/payment/fail', async (req, res) => {
  const transactionId = req.body?.tran_id;
  await founderService.updatePaymentStatus(transactionId, PaymentStatus.FAIL);

  res.redirect(`${env.FRONTEND_URL}/payment/fail?transactionId=${transactionId}`);
});
```

#### Cancel Route

```ts
app.post('/payment/cancel', async (req, res) => {
  const transactionId = req.body?.tran_id;
  await founderService.updatePaymentStatus(transactionId, PaymentStatus.CANCEL);

  res.redirect(`${env.FRONTEND_URL}/payment/cancel?transactionId=${transactionId}`);
});
```

> All routes **receive POST requests** from SSLCommerz after payment completion and then redirect to the frontend with the transactionId in query params.

---

### 4. Optional: Clean Up Pending Payments

```ts
await founderService.cleanUpPaymentPending(); 
// Deletes all payments with PENDING status older than 25 minutes
```

---

### 5. Tips

* Always use correct `store_id` and `store_password`.
* Use `live = true` in constructor for production.
* Validate SSLCommerz responses before updating DB.
* Keep `transactionId` unique for each payment.
* Use `catchAsync` in Express to handle async errors in routes.

---

### Maintained By

**Swapon Saha**
🔗 GitHub: [cseswapon](https://github.com/cseswapon)

---

### License

Licensed under [ISC License](LICENSE)
