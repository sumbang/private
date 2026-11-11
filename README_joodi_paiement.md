# Joodi Paiement

Web payment gateway for **Joodi**, the African podcast platform. This small PHP site
serves the payment pages to which the Joodi apps redirect users to pay for a purchase
(subscription, content…), processes the payment with the selected provider, then
notifies the Joodi API of the transaction result.

Audience: end users of the Joodi apps (payment) and the Joodi technical team
(deployment and maintenance of the gateway).

## Main features

- **Card payment via Stripe** (`stripe.php` → `stripe-processing.php` →
  `stripe-process.php`): card form, payment creation/confirmation, then
  sending the final status to the Joodi API.
- **Mobile Money payment** (`momo.php` → `momo-processing.php`): the provider is selected
  based on the user's country:
  - My-CoolPay for Cameroon (XAF);
  - Monetbil for Benin, Congo, DR Congo and Senegal;
  - MyPVit for the country associated with the `AM` code.
  Only payments in FCFA are accepted on this flow.
- **PayPal return** (`succes-paypal.php`): captures the PayPal order after the buyer
  approves it and updates the payment on the Joodi side.
- **Mobile Money confirmation** (`success_momo.php`): verifies the transaction with the
  provider (Lygos) before confirmation.
- Result pages: `success.php`, `error.php`, `error_momo.php`, `process.php`
  (payment in progress) and `index.php` (invalid address).
- Target environment selected via the `serveur` parameter (`DEMO` or production) to
  query the corresponding Joodi API.
- Utility HTTP client (`HttpClient.php`): GET/POST requests, Basic or Bearer
  authentication, UUID v4 generation.

## Tech stack

- PHP 8.1 (Docker image `php:8.1-cli`, built-in PHP server)
- Composer 2; dependency `stripe/stripe-php` ^15.10
- Front end: Bootstrap, jQuery, Font Awesome, Ionicons (local static files)
- Tests: PHPUnit ^9
- CI/CD: GitHub Actions (tests, analysis, building an archive and a Docker image,
  publishing to an artifact repository, snapshot → release promotion)

## Project structure

```
.
├── index.php, stripe*.php, momo*.php, succes*.php, error*.php, process.php   # pages and processing
├── HttpClient.php        # HTTP client (namespace App\Utils)
├── Footer.php            # shared footer
├── css/, js/, img/       # static assets
├── tests/                # PHPUnit tests for the processing scripts
├── phpunit.xml           # PHPUnit configuration
├── Dockerfile            # runtime image
└── .github/workflows/    # CI/CD pipelines (snapshot and release)
```

## Installation and running

Prerequisites: PHP 8.1+ with the cURL extension, Composer.

```bash
composer install
php -S 0.0.0.0:8000 -t .
```

The site is then available at `http://localhost:8000`. The payment pages expect
URL parameters (e.g. `stripe.php?item=<id_paiement>&serveur=DEMO`).

With Docker:

```bash
docker build -t joodi-payment .
docker run -p 8000:8000 --env-file .env joodi-payment
```

Tests:

```bash
vendor/bin/phpunit
```

## Configuration

Provider credentials are read from environment variables (no
`.env.example` file is provided):

| Variable | Purpose |
| --- | --- |
| `STRIPE_SK_PROD` | Stripe secret key |
| `PAYPAL_USERNAME` / `PAYPAL_PASSWORD` | PayPal API client credentials |
| `MONETBILL_SERVICE` | Monetbil service ID |
| `MYPVIT_MARCHAND_CODE` / `MYPVIT_TOKEN` | MyPVit merchant code and token |
| `LYGOS_API_KEY` | Lygos API key |
