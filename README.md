# Haven Furniture Store

Modern furniture e-commerce independent site with PayPal checkout, user accounts, and a visual product admin panel.

## Quick Start
1. Open `index.html` in your browser to view the store
2. Open `admin.html` to manage products
3. Deploy to GitHub Pages / Cloudflare Pages / Netlify / Vercel for free

See [DEPLOY-GUIDE.md](./DEPLOY-GUIDE.md) for deployment and [FIREBASE-GUIDE.md](./FIREBASE-GUIDE.md) for user-account setup.

## Features
- Product catalog with categories and filters
- Shopping cart with coupon support
- 4-step checkout process
- PayPal Smart Buttons integration
- Order email notifications
- **User accounts** — sign up / sign in, address book, order history, profile settings (Firebase cloud or local mode)
- **One-click checkout** — signed-in users get address auto-filled; orders saved to their account
- Blog / journal section
- Wishlist
- Legal pages (Privacy, Terms, Returns, Shipping, Warranty)
- **Visual admin panel** — add/edit/delete products without touching code
- Fully responsive design
- Zero server cost (static site)

## User Accounts
Works out of the box in **local demo mode** (data stays in the visitor's browser). For real cross-device accounts, connect a free Firebase project — see [FIREBASE-GUIDE.md](./FIREBASE-GUIDE.md) for a ~10-minute setup. The account center includes Overview, Orders, Address Book, and Settings.

## Managing Products
Open `admin.html`:
1. Click **Settings** and enter your GitHub Personal Access Token (repo scope)
2. Products load from `products.json` in this repository
3. Add / edit / delete products visually
4. Click **Save to GitHub** — committed automatically; the deployed site updates within a minute

## File Structure
```
├── index.html          # Main storefront
├── admin.html          # Product management panel
├── products.json       # Product data (managed via admin)
├── nook-simple/        # Plan B: minimal one-page store
│   └── index.html
├── DEPLOY-GUIDE.md     # Deployment guide
├── FIREBASE-GUIDE.md   # User account (Firebase) setup guide
└── README.md
```

## Tech
- Vanilla HTML / CSS / JavaScript
- PayPal SDK for payments
- Firebase Auth + Firestore for user accounts (optional, local fallback)
- FormSubmit.co for order notifications
- GitHub API for product management (no backend needed)
- Google Fonts (self-hosted mirror)

## License
MIT
