# Deployment Guide - 3D Portfolio

This guide will help you deploy your 3D portfolio to popular hosting platforms.

## Prerequisites

Before deploying, make sure:
- All personal information is updated (name, bio, projects, social links)
- The production build works: `npm run build`
- All links and images are correct

## Option 1: Vercel (Recommended) ⚡

Vercel is the easiest and fastest way to deploy your portfolio.

### Method A: Vercel CLI

1. Install Vercel CLI globally:
```bash
npm install -g vercel
```

2. Run deployment:
```bash
vercel
```

3. Follow the prompts:
   - Set up and deploy? **Y**
   - Which scope? Choose your account
   - Link to existing project? **N**
   - Project name? **portfolio-3d** (or your choice)
   - Directory? **./portfolio-3d**
   - Override settings? **N**

4. Your site will be live! Vercel will give you a URL like `https://portfolio-3d.vercel.app`

### Method B: Vercel Git Integration

1. Push your code to GitHub
2. Go to [vercel.com](https://vercel.com)
3. Click "Add New Project"
4. Import your GitHub repository
5. Vercel auto-detects Vite - just click "Deploy"
6. Done! Your site is live with automatic deployments on every git push

**Custom Domain:**
- Go to Project Settings → Domains
- Add your custom domain (e.g., `yourname.com`)
- Follow DNS configuration instructions

---

## Option 2: Netlify 🌐

### Method A: Netlify Drop (Easiest)

1. Build your project:
```bash
npm run build
```

2. Go to [app.netlify.com/drop](https://app.netlify.com/drop)

3. Drag and drop the `dist` folder

4. Your site is live! Example: `https://your-site-name.netlify.app`

### Method B: Netlify CLI

1. Install Netlify CLI:
```bash
npm install -g netlify-cli
```

2. Build the project:
```bash
npm run build
```

3. Deploy:
```bash
netlify deploy
```

4. For production:
```bash
netlify deploy --prod
```

### Method C: Netlify Git Integration

1. Push code to GitHub
2. Go to [app.netlify.com](https://app.netlify.com)
3. Click "Add new site" → "Import an existing project"
4. Connect your GitHub repository
5. Build settings:
   - Build command: `npm run build`
   - Publish directory: `dist`
6. Click "Deploy site"

---

## Option 3: GitHub Pages 📘

1. Install gh-pages package:
```bash
npm install --save-dev gh-pages
```

2. Update `package.json`:
```json
{
  "homepage": "https://yourusername.github.io/portfolio-3d",
  "scripts": {
    "predeploy": "npm run build",
    "deploy": "gh-pages -d dist"
  }
}
```

3. Update `vite.config.js`:
```javascript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  base: '/portfolio-3d/'  // Add this line with your repo name
})
```

4. Deploy:
```bash
npm run deploy
```

5. Enable GitHub Pages:
   - Go to repository Settings → Pages
   - Source: Deploy from branch
   - Branch: `gh-pages` → `/ (root)`
   - Click Save

6. Your site will be live at `https://yourusername.github.io/portfolio-3d`

---

## Option 4: Custom VPS (Advanced) 🖥️

If you have your own server (AWS, DigitalOcean, etc.):

1. Build the project:
```bash
npm run build
```

2. Upload the `dist` folder contents to your web server

3. Configure nginx (example):
```nginx
server {
    listen 80;
    server_name yourdomainname yourdomain.com;

    root /var/www/portfolio/dist;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

4. Restart nginx:
```bash
sudo systemctl restart nginx
```

---

## Post-Deployment Checklist ✅

After deploying, verify:

- [ ] Site loads correctly
- [ ] 3D elements render properly
- [ ] All navigation links work
- [ ] Forms submit correctly
- [ ] Responsive design works on mobile
- [ ] No console errors
- [ ] SEO meta tags are correct
- [ ] Social media preview looks good (test with [OpenGraph.xyz](https://www.opengraph.xyz/))

---

## Custom Domain Setup 🌍

### For Vercel:
1. Project Settings → Domains
2. Add your domain
3. Update DNS with provided records

### For Netlify:
1. Site Settings → Domain Management
2. Add custom domain
3. Follow DNS configuration steps

### For GitHub Pages:
1. Add `CNAME` file to `/public` with your domain
2. Update DNS:
   - Type: A Record
   - Name: @
   - Value: 185.199.108.153, 185.199.109.153, 185.199.110.153, 185.199.111.153

---

## Continuous Deployment 🔄

For automatic deployments on code changes:

**Vercel & Netlify (Git Integration):**
- Every push to `main` branch = automatic deployment
- No additional setup needed

**GitHub Pages:**
- Create `.github/workflows/deploy.yml`:
```yaml
name: Deploy

on:
  push:
    branches: [ main ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
      - run: npm ci
      - run: npm run build
      - uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

---

## Troubleshooting 🔧

**3D elements not loading:**
- Check browser console for errors
- Ensure WebGL is supported
- Verify Three.js assets loaded correctly

**404 on page refresh:**
- Configure your hosting for SPA routing
- Netlify: Add `_redirects` file with `/* /index.html 200`
- Vercel: Add `vercel.json` with rewrites configuration

**Slow loading:**
- Run `npm run build` to create optimized production build
- Enable CDN on your hosting platform
- Check bundle size analyzer

---

## Need Help? 💬

- Vercel Docs: https://vercel.com/docs
- Netlify Docs: https://docs.netlify.com
- GitHub Pages: https://docs.github.com/pages

Happy deploying! 🚀
