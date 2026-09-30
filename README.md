# Function Hall Booking (with real payments)

Node.js + Express backend, plain HTML frontend. Supports **Stripe Checkout** or **Razorpay**.

## Run locally
1. Install Node 18+.
2. `npm install`
3. Copy `.env.example` to `.env`, set `PAYMENT_PROVIDER` and your **test** keys.
4. `npm start` then open http://localhost:3000

## Stripe
- Dashboard > Developers > API keys: copy the secret key into `.env`.
- Webhook (required to confirm payments):
  `stripe listen --forward-to localhost:3000/api/stripe-webhook`
  and put the printed `whsec_...` in `STRIPE_WEBHOOK_SECRET`.
- Production: add endpoint `https://YOUR-SITE/api/stripe-webhook` with event `checkout.session.completed`.
- Test card: 4242 4242 4242 4242, any future expiry, any CVC.

## Razorpay
- Set `PAYMENT_PROVIDER=razorpay`, `CURRENCY=INR`, and your key id and secret.
- Payments are verified on the server with the signature check.

## Notes
- Prices are defined in `server.js` (HALLS, CATER). The server recalculates every total.
- Bookings are stored in `bookings.json`. For production use a real database (Postgres, MongoDB).
- Online bookings hold the date for 30 minutes until paid; bank transfer holds for 48 hours.
- Deploy behind HTTPS and set `BASE_URL` to your public URL.
- Still to add before launch: admin login to view, cancel and refund bookings, confirmation emails, terms and refund policy.
