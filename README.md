# Haven Furniture Store

Modern furniture e-commerce independent site with PayPal checkout and visual product admin panel.

## Quick Start

1. Open `index.html` in your browser to view the store
2. Open `admin.html` to manage products
3. Deploy to Cloudflare Pages / Netlify / Vercel for free

See [DEPLOY-GUIDE.md](./DEPLOY-GUIDE.md) for full deployment instructions.

## Features

- Product catalog with categories and filters
- Shopping cart with coupon support
- 4-step checkout process
- PayPal Smart Buttons integration
- Order email notifications
- Blog / journal section
- Wishlist
- Legal pages (Privacy, Terms, Returns, Shipping, Warranty)
- **Visual admin panel** — add/edit/delete products without touching code
- Fully responsive design
- Zero server cost (static site)

## Managing Products

Open `admin.html` in your browser:

1. Click **Settings** and enter your GitHub Personal Access Token (repo scope)
2. Products load automatically from `products.json` in this repository
3. Add, edit, or delete products using the visual interface
4. Click **Save to GitHub** — changes are committed automatically
5. Your deployed site updates within 1 minute (Cloudflare Pages auto-deploy)

## File Structure

```
├── index.html          # Main storefront
├── admin.html          # Product management panel
├── products.json       # Product data (managed via admin)
├── nook-simple/        # Plan B: minimal one-page store
│   └── index.html
├── DEPLOY-GUIDE.md     # Deployment guide
└── README.md
```

## Tech

- Vanilla HTML / CSS / JavaScript
- PayPal SDK for payments
- FormSubmit.co for order notifications
- GitHub API for product management (no backend needed)
- Google Fonts (self-hosted mirror)

## License

MIT
