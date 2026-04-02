# WordPress Test Site Migration Plan

> Migrate SEO test scenarios from Netlify static site to criar-perros.com (WordPress).
> Test pages look like normal dog blog content — SEO errors are under the hood only.
> Hidden index page lists all planted errors (blocked in robots.txt).
> Date: 2026-04-02

---

## 1. Current State

### Netlify Test Site (nimble-monstera-795cd4.netlify.app)
- Static HTML pages with deliberate SEO errors
- ~95 test pages covering titles, descriptions, canonicals, redirects, schema, etc.
- Minimal content — obviously a test site
- Stays active until WordPress site covers all scenarios, then retired

### WordPress Site (criar-perros.com)
- Real Spanish-language dog blog with dozens of posts
- WordPress (Twenty Twenty-One theme)
- Categories: Breeds, Health, Food, Training, Puppies
- Has real content, real images, real internal linking
- Owned by user — OK to add test content and break SEO

---

## 2. Design Principles

1. **Invisible errors** — a casual visitor sees a normal dog blog. SEO errors are in meta tags, schema, redirects, headers — not in visible content.
2. **Hidden cheat sheet** — a dedicated page lists all planted errors and their locations. This page is blocked in robots.txt.
3. **Real content** — test pages have real dog-related content, not lorem ipsum or "test page 1".
4. **Gradual migration** — keep Netlify alive until WordPress covers everything.
5. **Store section** — fake product pages (dog food, toys, etc.) for Product schema testing. "Buy" button leads to a disclaimer page ("this is a test site").

---

## 3. Site Hierarchy

```
Level 0:  criar-perros.com/                              Organization + WebSite + WebPage
|
+-- Level 1:  /tienda/                                   Store (CollectionPage)
|   +-- Level 11: /tienda/comida/                        Food category (CollectionPage)
|   |   +-- Level 111: /tienda/comida/pienso-premium     Product
|   |   +-- Level 111: /tienda/comida/snacks-dental      Product
|   +-- Level 11: /tienda/juguetes/                      Toys category (CollectionPage)
|       +-- Level 111: /tienda/juguetes/pelota-kong      Product
|
+-- Level 1:  /blog/                                     Blog (CollectionPage)
|   +-- Level 11: /blog/razas/                           Breeds category (CollectionPage)
|   |   +-- Level 111: /blog/razas/labrador              Article
|   |   +-- Level 111: /blog/razas/chihuahua             Article
|   +-- Level 11: /blog/salud/                           Health category (CollectionPage)
|   |   +-- Level 111: /blog/salud/vacunas               Article
|   +-- Level 11: /blog/entrenamiento/                   Training category (CollectionPage)
|       +-- Level 111: /blog/entrenamiento/basico        Article
|
+-- Level 1:  /sobre-nosotros/                           AboutPage
+-- Level 1:  /contacto/                                 ContactPage
+-- Level 1:  /preguntas-frecuentes/                     FAQPage
+-- Level 1:  /eventos/                                  Event
```

---

## 4. Schema Types Coverage

| Page Type | Schema Types | Source |
|---|---|---|
| Homepage | Organization, WebSite, WebPage | Existing (add/fix) |
| Store landing | CollectionPage | New page |
| Product category | CollectionPage | New pages |
| Product detail | Product, Offer, AggregateRating | New pages |
| Blog landing | CollectionPage, Blog | Existing (reorganize) |
| Blog category | CollectionPage | Existing categories |
| Blog post | Article / BlogPosting | Existing posts |
| About | AboutPage | New or existing |
| Contact | ContactPage | New or existing |
| FAQ | FAQPage | New page |
| Events | Event | New page |
| All sub-pages | BreadcrumbList | All non-homepage |

---

## 5. SEO Error Categories to Plant

Errors are embedded in real content — invisible to casual visitors.

### Meta & Title Errors
- Duplicate titles between two blog posts
- Title too long / too short on specific posts
- Missing or duplicate meta descriptions
- Multiple title tags in one page

### Canonical & Indexability
- Wrong canonical (points to different page)
- Missing canonical on some pages
- Noindex on a page that should be indexed

### Redirect & Link Errors
- Internal links pointing to redirecting URLs
- Redirect chains (3 hops)
- Links to 404 pages (broken links)
- Non-canonical homepage links

### Image Errors
- Missing ALT text on some images
- Broken image references
- Large images without optimization

### Schema Errors
- Missing required fields in Product schema
- Organization on non-homepage
- Broken @id references
- Duplicate schema blocks
- Wrong @type for page content

### Structure Errors
- Deep pages (4+ click depth)
- Orphan pages (no internal links)
- Pages with excessive outgoing links

---

## 6. Hidden Index Page

- URL: TBD (e.g., /test-index/ or /qa-errors/)
- Blocked in robots.txt: `Disallow: /test-index/`
- Content: table listing every planted error, which page it's on, and which check should detect it
- Password protected (optional) or just robots-blocked

---

## 7. Store Disclaimer Page

- URL: /tienda/aviso/ or similar
- All "Buy" / "Add to Cart" buttons redirect here
- Message (Spanish): "This is a test site for SEO auditing tools. Products shown are not real. For real dog products, visit [real pet store]."
- Clean, professional design — not an error page

---

## 8. Implementation Order

1. Get WordPress API credentials (application password)
2. Create store structure (/tienda/ + categories + products)
3. Create disclaimer page
4. Reorganize existing blog posts under /blog/ categories
5. Add About, Contact, FAQ, Events pages
6. Plant SEO errors across all pages
7. Add correct schema to clean pages, broken schema to test pages
8. Create hidden index page with error map
9. Update robots.txt
10. Test with the SEO report generator tool
11. When all scenarios covered — retire Netlify site
