# Éclipse — le site

Le rapport quotidien qui explique l'actualité économique et financière à celles et ceux à qui personne ne l'a jamais expliquée. Trois langues aujourd'hui (français, anglais, espagnol), d'autres plus tard.

Ce dépôt est **public** : il sert de contenu au site, lu directement par la page. N'y mettez jamais de mot de passe, de jeton, de clé d'API ni de donnée personnelle.

---

## Les trois couches

Le site est construit pour évoluer sans tout reprendre. Chaque couche a un rôle et un rythme.

| Couche | Fichiers | Change quand… | Coût |
|---|---|---|---|
| **Coque** (le code) | `index.html`, `netlify.toml` | une nouvelle fonction est ajoutée | un déploiement Netlify |
| **Réglages et textes** | `site.json`, `lang-fr.json`, `lang-en.json`, `lang-es.json`, `formations.json` | on change le menu, un texte, une langue, une ressource | aucun déploiement |
| **Contenu du jour** | `edition.html`, `editions/AAAA-MM-JJ.html`, `archives.json` | chaque matin (automatique) | aucun déploiement |

Netlify ne redéploie que si `index.html` ou `netlify.toml` change (règle `ignore` de `netlify.toml`). Tout le reste est lu par la page sur GitHub au chargement : le modifier met le site à jour sans consommer de crédit Netlify.

Si GitHub est injoignable, la page retombe sur une copie embarquée dans `index.html`, puis affiche un message clair. Elle ne reste jamais blanche.

---

## Les fichiers

- **`index.html`** — la coque : en-tête, menu (avec recherche), sélecteur de langue, pages, inscription, fenêtre de discussion, pied de page. Une seule page, sans dépendance, qui assainit tout contenu reçu.
- **`site.json`** — le plan du site : nom, langues, entrées du menu, mode d'inscription, réseaux sociaux, recherche, discussion.
- **`lang-XX.json`** — tous les textes d'une langue, en clés à plat (`ui.*`, `nav.*`, `view.*`, `nl.*`, `foot.*`, `chat.*`, `search.*`, `ed.*`, `tag.*`).
- **`formations.json`** — les ressources gratuites proposées dans « Se former » (liens, langue, étiquettes, texte par langue). La date de dernière vérification est en tête du fichier.
- **`edition.html`** — l'édition la plus récente.
- **`editions/AAAA-MM-JJ.html`** — chaque édition, conservée.
- **`archives.json`** — l'index des éditions (date, numéro, titre et sujets par langue) : il alimente les archives et la recherche.

---

## Modifier le site

Tout se fait en modifiant un fichier sur GitHub (icône crayon, puis *Commit changes*). Le site se met à jour en une à deux minutes.

**Changer un texte.** Trouvez la clé dans `lang-fr.json` (par exemple `ui.subscribe`) et modifiez la valeur. Faites de même dans les autres langues. Une clé absente dans une langue retombe sur le français.

**Ajouter une langue.**
1. Copiez `lang-en.json` en `lang-XX.json` (code à deux ou trois lettres) et traduisez les valeurs, jamais les clés.
2. Ajoutez la langue dans `site.json`, section `langs` : `code`, `name`, `tag` (par exemple `de-DE`), `dir` (`ltr`, ou `rtl` pour l'arabe et l'hébreu), `edition` et `style`.
3. Commencez avec `"edition": false` : le site s'affiche dans la langue, les éditions restent dans la langue de repli. Passez à `true` quand les éditions du jour seront produites dans cette langue.

**Ajouter une page au menu.** Ajoutez une entrée dans `nav` (`id`, `type`: `page`, `menu`, `footer`), puis dans chaque `lang-XX.json` les clés `nav.<id>`, `view.<id>.title`, `view.<id>.lead` et `view.<id>.blocks`. Les blocs sont des titres `{"h": …}`, des paragraphes `{"p": …}`, des listes `{"ul": […]}` et des encadrés `{"note": …}`, avec `**gras**` et `[texte](https://lien)`. Aucun HTML brut.

**Ajouter une ressource « Se former ».** Ajoutez un objet à `items` dans `formations.json` avec son `url`, sa `lang`, ses `tags` (`free`, `badge`, `audit`, `paid`) et ses textes `name` et `text` par langue. Mettez `checked` à jour à la date de vérification.

**Réseaux sociaux.** Ajoutez dans `social` (dans `site.json`) des objets `{"label": "Instagram", "url": "https://…"}`. Seuls les liens en `https://` sont acceptés ; ils apparaissent en pied de page.

**Inscription.** `newsletter.mode` vaut `netlify` (formulaire intégré) ou `link` (bouton vers une page d'inscription externe, à indiquer dans `url`, en `https://`). Passer de l'un à l'autre est un simple changement de valeur.

**Désactiver la recherche ou la discussion.** `search.enabled` et `chat.enabled` à `false`.

**Une fonction qui n'existe pas encore** (nouveau type de page, nouveau parcours de discussion, réponse automatique…) demande de modifier la coque : demandez-le, le site sera regénéré et `index.html` remplacé.

---

## Le contrat d'une édition

`edition.html` est un fragment (pas une page complète) :

```html
<article id="edition-root" data-date="AAAA-MM-JJ" data-n="12" data-title-fr="…" data-title-en="…" data-title-es="…">
  <div class="ed-lang" lang="fr"> … </div>
  <div class="ed-lang" lang="en"> … </div>
  <div class="ed-lang" lang="es"> … </div>
</article>
```

Un bloc `ed-lang` par langue dont `edition` vaut `true`. Le contenu est assaini par la page : ni script, ni formulaire, ni cadre, ni image externe, ni style en ligne.

---

## Ce que la page collecte

Rien en dehors de ce que le lecteur envoie lui-même : son adresse e-mail à l'inscription, et les messages écrits dans la fenêtre de discussion. Aucun témoin de suivi, aucun stockage local hormis le choix de langue. Les formulaires sont traités par Netlify Forms.

---

## Principes éditoriaux

Explication, pas conseil. Aucune recommandation d'achat ou de vente. Chaque chiffre est daté et sourcé. Ce qui n'est pas vérifiable est écrit « non vérifié ». Les erreurs sont corrigées et signalées.
