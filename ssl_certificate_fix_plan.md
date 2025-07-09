# SSL Certificate Fix Plan for GitHub Pages Site

## ⚠️ CRITICAL ISSUES DISCOVERED

### 🔴 **PRIMARY ISSUE**: Custom Domain SSL Certificate Failure
- **Problem**: The site redirects from `https://svstoyanov.github.io` → `http://svstoyanov.com`
- **Root Cause**: CNAME file pointing to `svstoyanov.com` but SSL certificate is invalid
- **Error**: `SSL: no alternative certificate subject name matches target hostname 'svstoyanov.com'`
- **Impact**: **Site is completely broken over HTTPS** - users get security warnings

### 🔴 **IMMEDIATE ACTIONS REQUIRED**
1. **Fix SSL certificate for custom domain** OR **Remove CNAME file** temporarily
2. **Test HTTPS redirect behavior**
3. **Update all HTTP references to HTTPS**

---

## Current Status
- **Repository**: svstoyanov/svstoyanov.github.io
- **Platform**: GitHub Pages
- **Custom Domain**: svstoyanov.com (configured via CNAME file)
- **SSL Status**: ❌ **BROKEN** - Certificate doesn't match domain
- **Current Behavior**: 
  - `https://svstoyanov.github.io` → `http://svstoyanov.com` (insecure redirect)
  - `https://svstoyanov.com` → SSL certificate error

## Identified Issues

### 1. **CRITICAL**: SSL Certificate Mismatch
- Custom domain `svstoyanov.com` has invalid SSL certificate
- Certificate subject name doesn't match the hostname
- GitHub Pages cannot generate proper certificate for custom domain

### 2. Mixed Content Problems
Found multiple HTTP references that need to be converted to HTTPS:
- YouTube API integration (`http://www.youtube.com/iframe_api`)
- OpenGraph metadata (`http://svstoyanov.com`)
- Content Security Policy contains both HTTP and HTTPS references

### 3. Content Security Policy (CSP) Issues
Current CSP in index.html:
```html
<meta http-equiv="Content-Security-Policy" content="script-src 'self' 'unsafe-eval' http://www.youtube.com/iframe_api https://s.ytimg.com; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; font-src 'self' data: https://fonts.gstatic.com;">
```

### 4. External Resource References
- Google Fonts: ✅ Already using HTTPS
- YouTube API: ❌ Using HTTP
- Social media links: Mixed HTTP/HTTPS

## 🚨 EMERGENCY FIX PLAN

### **Option A: Fix Custom Domain SSL (Recommended)**
1. **Remove CNAME file temporarily**
   ```bash
   git rm CNAME
   git commit -m "Temporarily remove CNAME to fix SSL"
   git push origin master
   ```

2. **Wait for deployment** (5-10 minutes)

3. **Verify site works on GitHub Pages domain**
   - Test: `https://svstoyanov.github.io`
   - Should load without SSL errors

4. **Re-add custom domain properly**
   - Go to GitHub repo Settings > Pages
   - Add custom domain: `svstoyanov.com`
   - Enable "Enforce HTTPS"
   - Wait for certificate provisioning (can take 24-48 hours)

### **Option B: Use GitHub Pages Domain (Quick Fix)**
1. **Remove CNAME file**
2. **Update all references from svstoyanov.com to svstoyanov.github.io**
3. **Site will work immediately on GitHub Pages domain**

## Step-by-Step Fix Plan

### Phase 1: Emergency SSL Fix
1. **Choose Option A or B above**
2. **Remove CNAME file** (temporary or permanent)
3. **Test basic HTTPS functionality**
4. **Update DNS records** (if keeping custom domain)

### Phase 2: Code Fixes
1. **Update Content Security Policy**
   - Change `http://www.youtube.com/iframe_api` to `https://www.youtube.com/iframe_api`
   - Review and update all HTTP references to HTTPS

2. **Fix OpenGraph Metadata**
   - Update `http://svstoyanov.com` to `https://svstoyanov.com` (or GitHub Pages domain)
   - Ensure all og:image URLs use HTTPS

3. **Update JavaScript API Calls**
   - Change YouTube API calls from HTTP to HTTPS
   - Update any hardcoded HTTP URLs in the source code

### Phase 3: Custom Domain SSL Setup (If Option A)
1. **Verify DNS Configuration**
   - A records should point to GitHub Pages IPs:
     - `185.199.108.153`
     - `185.199.109.153`
     - `185.199.110.153`
     - `185.199.111.153`
   - Remove any conflicting records

2. **GitHub Pages Configuration**
   - Re-add custom domain in Settings > Pages
   - Enable "Enforce HTTPS"
   - Wait for Let's Encrypt certificate provisioning

### Phase 4: Testing & Validation
1. **SSL Certificate Validation**
   - Use SSL testing tools (SSL Labs, etc.)
   - Verify certificate chain is valid
   - Check certificate expiration dates

2. **Browser Testing**
   - Test in multiple browsers
   - Check for security warnings
   - Verify all functionality works over HTTPS

## DNS Configuration for Custom Domain

If you want to keep the custom domain `svstoyanov.com`, ensure these DNS records:

```
Type: A
Name: @
Value: 185.199.108.153

Type: A
Name: @
Value: 185.199.109.153

Type: A
Name: @
Value: 185.199.110.153

Type: A
Name: @
Value: 185.199.111.153

Type: CNAME
Name: www
Value: svstoyanov.github.io
```

## Immediate Action Items

### 🔴 **DO THIS NOW** (Within 1 hour)
1. **Backup current CNAME file**
2. **Remove CNAME file** to stop broken redirect
3. **Test site on GitHub Pages domain**
4. **Decide on custom domain strategy**

### 🟡 **DO THIS TODAY** (Within 24 hours)
1. **Fix all HTTP references in code**
2. **Update Content Security Policy**
3. **Test all external integrations**
4. **Set up proper DNS** (if keeping custom domain)

### 🟢 **DO THIS WEEK** (Within 7 days)
1. **Re-add custom domain** (if desired)
2. **Monitor SSL certificate status**
3. **Set up SSL monitoring**
4. **Document the fix process**

## Common GitHub Pages SSL Issues & Solutions

### Issue 1: "Certificate not yet created" Error
**Solution**: 
- Remove and re-add custom domain in GitHub Pages settings
- Wait for DNS propagation (can take up to 24 hours)
- Verify DNS records are correct

### Issue 2: Mixed Content Warnings
**Solution**:
- Replace all `http://` with `https://` in HTML/CSS/JS
- Update CSP headers to use HTTPS only
- Verify all third-party resources support HTTPS

### Issue 3: Custom Domain SSL Issues
**Solution**:
- Ensure DNS records point to correct GitHub Pages IPs
- Remove conflicting DNS records
- Wait for Let's Encrypt certificate provisioning

### Issue 4: CSP Blocking Resources
**Solution**:
- Update CSP to allow necessary HTTPS resources
- Remove or update HTTP references in CSP
- Test thoroughly after CSP changes

## Tools for Testing

### SSL Certificate Testing
- SSL Labs SSL Test: https://www.ssllabs.com/ssltest/
- Mozilla Observatory: https://observatory.mozilla.org/
- Security Headers: https://securityheaders.com/

### Mixed Content Detection
- Browser Developer Tools (Console tab)
- Why No Padlock: https://www.whynopadlock.com/
- Mixed Content Scanner browser extensions

### DNS Testing
- DNS Checker: https://dnschecker.org/
- dig command: `dig svstoyanov.com`
- nslookup: `nslookup svstoyanov.com`

## Expected Timeline
- **Emergency Fix**: 1-2 hours
- **Code Updates**: 2-4 hours
- **Custom Domain SSL**: 24-48 hours (DNS propagation)
- **Full Testing**: 1-2 hours
- **Total**: 1-3 days

## Success Criteria
- ✅ Green padlock icon in browser
- ✅ No mixed content warnings
- ✅ All resources load over HTTPS
- ✅ SSL certificate is valid and trusted
- ✅ No CSP violations
- ✅ Site loads quickly over HTTPS
- ✅ No redirect loops or certificate errors

## Monitoring & Maintenance
- Set up SSL certificate expiration alerts
- Regular security scans
- Monitor for new mixed content issues
- Keep dependencies updated
- Test SSL after any domain changes