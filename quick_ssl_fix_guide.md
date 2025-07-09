# 🚨 URGENT: SSL Certificate Quick Fix Guide

## **CRITICAL PROBLEM IDENTIFIED**
Your GitHub Pages site is **completely broken over HTTPS** due to an invalid SSL certificate for the custom domain `svstoyanov.com`.

### Current Status
- ❌ `https://svstoyanov.github.io` → redirects to `http://svstoyanov.com` (insecure)
- ❌ `https://svstoyanov.com` → SSL certificate error
- ❌ **Users see security warnings and can't access your site securely**

---

## **IMMEDIATE ACTION REQUIRED** 🔴

### **STEP 1: Quick Fix (Do This Now)**
Remove the CNAME file to stop the broken redirect:

```bash
# Navigate to your repository
cd /workspace

# Remove the CNAME file
git rm CNAME

# Commit the change
git commit -m "Emergency: Remove CNAME to fix SSL certificate issue"

# Push to GitHub
git push origin master
```

### **STEP 2: Verify Fix (Wait 5-10 minutes)**
After pushing, test the site:
- Visit: `https://svstoyanov.github.io`
- Should load without SSL errors
- Should show green padlock in browser

---

## **CHOOSE YOUR LONG-TERM SOLUTION**

### **Option A: Keep Custom Domain** (Recommended)
1. **Set up proper DNS records** for `svstoyanov.com`:
   ```
   Type: A, Name: @, Value: 185.199.108.153
   Type: A, Name: @, Value: 185.199.109.153
   Type: A, Name: @, Value: 185.199.110.153
   Type: A, Name: @, Value: 185.199.111.153
   ```

2. **Re-add custom domain via GitHub UI**:
   - Go to: GitHub repo → Settings → Pages
   - Add custom domain: `svstoyanov.com`
   - Enable "Enforce HTTPS"
   - Wait 24-48 hours for SSL certificate

### **Option B: Use GitHub Pages Domain** (Quick)
1. **Update all references** from `svstoyanov.com` to `svstoyanov.github.io`
2. **Site works immediately** with GitHub's SSL certificate

---

## **CRITICAL CODE FIXES NEEDED**

### **Fix 1: Update Content Security Policy**
In `index.html`, change:
```html
<!-- FROM -->
<meta http-equiv="Content-Security-Policy" content="script-src 'self' 'unsafe-eval' http://www.youtube.com/iframe_api https://s.ytimg.com; ...">

<!-- TO -->
<meta http-equiv="Content-Security-Policy" content="script-src 'self' 'unsafe-eval' https://www.youtube.com/iframe_api https://s.ytimg.com; ...">
```

### **Fix 2: Update OpenGraph Metadata**
In `index.html`, change:
```html
<!-- FROM -->
<meta property="og:url" content="http://svstoyanov.com">

<!-- TO -->
<meta property="og:url" content="https://svstoyanov.github.io">
<!-- OR if keeping custom domain -->
<meta property="og:url" content="https://svstoyanov.com">
```

### **Fix 3: Update JavaScript API Calls**
Search for any HTTP URLs in your JavaScript files and update them to HTTPS.

---

## **TESTING CHECKLIST**

After making changes, verify:
- [ ] Site loads over HTTPS without warnings
- [ ] Green padlock icon appears in browser
- [ ] No mixed content warnings in browser console
- [ ] All external resources (YouTube, Google Fonts) load correctly
- [ ] Social media sharing works properly

---

## **MONITORING**

Set up monitoring to prevent this issue in the future:
- Use SSL monitoring tools (SSL Labs, etc.)
- Set up alerts for certificate expiration
- Regularly test your site over HTTPS

---

## **NEED HELP?**

If you encounter issues:
1. Check GitHub Pages status: https://www.githubstatus.com/
2. Verify DNS propagation: https://dnschecker.org/
3. Test SSL certificate: https://www.ssllabs.com/ssltest/

**Remember**: SSL certificate provisioning can take 24-48 hours after DNS changes!