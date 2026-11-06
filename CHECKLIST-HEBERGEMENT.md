# Checklist après hébergement (Kuktus)

Utilisez cette liste une fois le site en ligne sur **votre** domaine et **votre** hébergeur réels.

## Données légales et boutique (`index.html` → objet `SHOP`)

| Élément | Fichier / emplacement | Action |
|--------|------------------------|--------|
| Raison sociale / statut | `SHOP.legal`, `SHOP.status` | Remplacer les valeurs d’exemple par la réalité (SAS, SARL, EURL, micro, etc.). |
| SIRET / RCS | `SHOP.siret`, `SHOP.rcs` | Mettre les numéros **officiels** de l’entreprise. |
| TVA | `SHOP.tva` | N° TVA intracommunautaire ou mention « TVA non applicable, art. 293 B » si éligible. |
| Adresse | `SHOP.address` | Siège social ou adresse d’exploitation conforme à l’immatriculation. |
| Téléphone / e-mail | `SHOP.phone`, `SHOP.email` | Contacts réels et surveillés (Service client / commandes). |
| Directeur de publication | `SHOP.director` | Personne désignée (souvent le gérant ou la PDG). |
| Hébergeur | `SHOP.host` | Nom, forme juridique, adresse du siège (obligatoire en France). |
| Médiateur | `SHOP.mediator` | Point de médiation pertinent pour votre secteur (ex. e-commerce consommation). |
| Paiement | `SHOP.payment` | Moyens et prestataire réels (Stripe, PayPlug, etc.). |
| Lien Cultura | `SHOP.culturaRelation` | `cultura_store` / `authorized_reseller` / `independent` selon le **contrat réel**. |
| URL checkout | `SHOP.checkoutUrl` | Si paiement sur un autre domaine, renseigner l’URL exacte. |

## Domaine et SEO

| Élément | Où | Action |
|--------|-----|--------|
| Domaine du site | `SHOP.domain`, balises `canonical`, `og:url`, `og:image` | Aligner sur **le domaine définitif** (avec ou sans `www`, mais **un seul** choix cohérent). |
| `sitemap.xml` | racine | Remplacer l’ancien domaine par le vôtre dans toutes les `<loc>`. |
| `robots.txt` | racine | Mettre à jour l’URL du `Sitemap:`. |
| `flux-google-merchant.tsv` | racine | Remplacer les URL produit et images par votre domaine final. |
| Google Search Console | `<meta name="google-site-verification" content="…">` | Coller le **code exact** fourni par Google (méthode balise HTML). |

## Suivi publicitaire (après création des comptes)

| Élément | Variable `SHOP` | Action |
|--------|-----------------|--------|
| Google Analytics 4 | `gaId` | Ex. `G-XXXXXXXXXX` depuis GA4. |
| Google Ads | `adsId`, `adsPurchaseLabel` | ID de conversion + libellé d’événement achat si utilisé. |
| Meta | `metaPixelId` | ID Pixel depuis Meta Events Manager. |
| TikTok | `tiktokPixelId` | ID depuis TikTok Ads. |
| Microsoft Clarity | `clarityId` | Si vous l’utilisez ; sinon laisser vide `""`. |

Les IDs actuels sont **fictifs** : sans remplacement, les pixels ne mesureront rien.

## Fichier `.htaccess`

- Fourni pour **Apache** (souvent mutualisé classique). Placez-le à la **racine** du site (même dossier que `index.html`).
- **Adaptez** le nom de domaine dans les règles `RewriteCond` / `RewriteRule` si votre domaine n’est pas `kuktus-boutique.fr`.
- Sur **Netlify**, **Cloudflare Pages**, **Vercel**, **GitHub Pages** : `.htaccess` est **ignoré**. Utilisez plutôt `_headers` (déjà présent) et la configuration de redirections du tableau de bord du fournisseur.
- **nginx** : pas de `.htaccess` ; reproduire les redirections HTTPS + canonique dans le `server { }`.

## Tests rapides après mise en ligne

- [ ] Page d’accueil en HTTPS, certificat valide.
- [ ] Une seule variante canonique (`www` **ou** sans `www`, cohérent partout).
- [ ] `?p=mentions-legales` et éventuellement `/mentions-legales` selon `.htaccess` : page légale visible.
- [ ] Bandeau cookies : refus → aucun script GA/Ads/Meta/TikTok en réseau ; acceptation → chargement attendu.
- [ ] Coordonnées affichées = cohérentes avec Google Merchant Center / fiche Google Business si vous les utilisez.

## Rappel important

Le **cloaking** au sens « contenu ou redirection différent pour les crawlers / plateformes publicitaires vs visiteurs » viole les règles des annonceurs et peut constituer une **pratique commerciale trompeuse**. Ce dépôt ne l’implémente pas ; la `.htaccess` proposée ne fait que des redirections standards (HTTPS, domaine canonique, réécriture d’URL lisible).
