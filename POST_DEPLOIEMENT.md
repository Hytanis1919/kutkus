# Après mise en ligne — checklist Kuktus

Ce fichier liste ce qu’il faut **vérifier ou modifier** une fois le site hébergé sous votre **vrai nom de domaine** (Apache avec `.htaccess` à la racine du dossier public).

---

## 1. Cloaking / redirection « bot vs humain »

**Ne pas** configurer de redirection ou de contenu différent selon l’IP ou l’User-Agent des crawlers (Google, Facebook, etc.). C’est du **cloaking**, interdit par [Google Ads](https://support.google.com/adspolicy/answer/6020954) et les consignes qualité organique ; le risque est un **refus publicitaire** ou une **pénalité SEO**.

Préférez : site unique pour tous, mentions légales complètes, expérience honnête, et sécurité au niveau **hébergeur** (pare-feu, rate limiting) si besoin.

---

## 2. Fichier `.htaccess`

- **Domaine canonique** : dans `.htaccess`, remplacez `www.kuktus-boutique.fr` par votre domaine réel (deux lignes `RewriteCond` / `RewriteRule`).
- **HTTPS local** : commentez le bloc « Forcer HTTPS » si vous testez sans certificat en local.
- **Content-Security-Policy** : une ligne CSP commentée est proposée dans `.htaccess` ; décommentez et **adaptez-la** après avoir listé tous vos domaines tiers (pixels, fonts, Stripe, etc.), sinon le site peut casser.
- Vérifiez que l’hébergeur active **mod_rewrite**, **mod_headers** et idéalement **mod_expires** / **mod_deflate**.

---

## 3. Objet `SHOP` dans `index.html`

Remplacer les valeurs **fictives** par la réalité juridique :

| Champ | Action |
|--------|--------|
| `name` | Nom commercial affiché au client |
| `domain` | URL publique **sans** slash final (ex. `https://www.votredomaine.fr`) |
| `legal`, `status`, `siret`, `rcs`, `tva`, `address` | Données **exactes** du vendeur (facturation, mentions obligatoires) |
| `phone`, `email`, `director` | Coordonnées et directeur de publication réels |
| `host` | Hébergeur réel (nom légal, adresse, contact) |
| `mediator` | Médiateur conforme à votre secteur / obligation |
| `payment` | Moyens et prestataire de paiement réels |
| `culturaRelation` | `"cultura_store"` \| `"authorized_reseller"` \| `"independent"` selon le **contrat** réel avec Cultura |
| `gaId`, `adsId`, `metaPixelId`, `tiktokPixelId`, `clarityId` | IDs depuis **Google Analytics**, **Google Ads**, **Meta**, **TikTok**, **Clarity** |
| `adsPurchaseLabel` | Libellé de conversion « achat » Google Ads, si utilisé |
| `checkoutUrl` | URL réelle de passage commande / paiement si vous redirigez |

---

## 4. Balise Search Console

Dans le `<head>` de `index.html`, la meta `google-site-verification` doit contenir le **code exact** fourni par [Google Search Console](https://search.google.com/search-console) (méthode « Balise HTML »), pas une valeur d’exemple.

---

## 5. Fichiers dérivés du domaine

Après changement de `SHOP.domain`, mettre à jour de la même base :

- `sitemap.xml` — toutes les `<loc>` et images
- `robots.txt` — URL du sitemap
- `flux-google-merchant.tsv` — colonnes lien produit et image
- Balises `<link rel="canonical">`, Open Graph (`og:url`, `og:image`) en tête de `index.html` (ou laisser le JS les réécrire si vous centralisez via `SHOP` uniquement ; vérifiez à l’affichage la cohérence)

---

## 6. Bandeau cookies et conformité

- Vérifier que le texte cookies / confidentialité correspond aux **outils réellement** chargés après consentement.
- Documenter les sous-traitants (hébergeur, transporteurs, paiement, pub) dans la politique.

---

## 7. Tests rapides après déploiement

- Ouvrir `https://votredomaine/` et `https://votredomaine/?p=mentions-legales`
- Vérifier **redirection HTTP → HTTPS** et **hôte canonique**
- Tester le bandeau cookies (refus / acceptation) et l’absence d’erreurs console
- Valider le flux dans **Google Merchant Center** et les balises **Google Ads** une fois les vrais IDs en place

---

## 8. Hébergement non Apache

Si vous êtes sur **Nginx**, **Cloudflare Pages** ou **Netlify**, les équivalents sont dans la config du serveur ou fichier `_headers` (déjà présent pour certains hébergeurs) ; le `.htaccess` ne s’applique pas tel quel.
