# KEDAR-WIHA Custom Block — README.md

```markdown
# 🧩 KEDAR-WIHA Custom Block

> **PrestaShop 9.0+ Module** — Dodawaj własne bloki HTML/CSS/JS na dowolny hook

[![PrestaShop 9.0+](https://img.shields.io/badge/PrestaShop-9.0+-orange.svg)](https://www.prestashop.com)
[![PHP 8.1+](https://img.shields.io/badge/PHP-8.1+-777BB4.svg)](https://www.php.net)
[![License](https://img.shields.io/badge/License-proprietary-blue.svg)](LICENSE)
[![KEDAR-WIHA.pl](https://img.shields.io/badge/KEDAR%E2%80%91WIHA.pl-autoryzowany%20dystrybutor-8B0000.svg)](https://kedar-wiha.pl)

---

## 📋 Spis treści

- [Opis](#-opis)
- [Funkcjonalności](#-funkcjonalności)
- [Wymagania](#-wymagania)
- [Instalacja](#-instalacja)
- [Konfiguracja](#-konfiguracja)
- [Użycie](#-użycie)
- [Struktura bazy danych](#-struktura-bazy-danych)
- [Dostępne hooki](#-dostępne-hooki)
- [Przykłady użycia](#-przykłady-użycia)
- [Rozwiązywanie problemów](#-rozwiązywanie-problemów)
- [Rozwój i Contributing](#-rozwój-i-contributing)
- [Licencja](#-licencja)
- [Wsparcie](#-wsparcie)

---

## 🔍 Opis

**KEDAR-WIHA Custom Block** to autorski moduł dla PrestaShop 9.0+, który umożliwia elastyczne dodawanie własnych bloków HTML, CSS i JavaScript na dowolny zarejestrowany hook w sklepie.

Moduł został zaprojektowany z myślą o potrzebach sklepu [KEDAR-WIHA.pl](https://kedar-wiha.pl) — autoryzowanego dystrybutora profesjonalnych narzędzi WIHA Polska — ale może być wykorzystany w każdym projekcie PrestaShop wymagającym dynamicznego zarządzania treścią bez modyfikacji kodu źródłowego motywu.

### 🎯 Główne zastosowania

- Dodawanie banerów promocyjnych na stronę główną (`displayHome`)
- Wstrzykiwanie customowych sekcji na strony CMS (`displayCMSPageContent`)
- Rozszerzanie kart produktów o dodatkowe informacje (`displayProductAdditionalInfo`)
- Personalizacja stopki lub nagłówka (`displayFooter`, `displayTop`)
- Dynamiczne bloki warunkowe dla konkretnych stron CMS (`id_cms`)

---

## ✨ Funkcjonalności

### 🧱 Zarządzanie blokami

- ✅ **Dowolny hook** — obsługa wszystkich hooków zarejestrowanych w module poprzez magiczną metodę `__call()`
- ✅ **Rich content** — edytor HTML z obsługą raw code lub WYSIWYG
- ✅ **Inline CSS** — dodawanie stylów bez edycji plików `.css` motywu
- ✅ **Inline JS** — wstrzykiwanie skryptów z zachowaniem kolejności ładowania
- ✅ **Targetowanie CMS** — opcjonalne ograniczanie bloku do konkretnej strony CMS po `id_cms`
- ✅ **Sortowanie** — kontrola kolejności wyświetlania bloków na hooku (`sort_order`)
- ✅ **Status aktywności** — szybkie włączanie/wyłączanie bloków bez usuwania
- ✅ **Bezpieczeństwo** — sanitizacja danych przez `pSQL()` i `Tools::getValue()`

### ⚙️ Panel administracyjny

- Intuicyjny formularz dodawania/edycji bloków
- Lista bloków z podglądem hooka, CMS ID i statusu
- Przyciski akcji: **Włącz/Wyłącz**, **Usuń**, **Edytuj**
- Walidacja pól wymaganych (tytuł, hook)

### 🚀 Performance & Security

- Brak zewnętrznych zależności — czysty PHP + SQL
- Query z indeksami na `hook_name`, `active`, `id_cms` dla szybkiego pobierania
- Renderowanie tylko aktywnych bloków dopasowanych do kontekstu
- Output buforowany i zwracany jako string — kompatybilny z systemem hooków PS9

---

## 📦 Wymagania

| Komponent | Wymagana wersja | Uwagi |
|-----------|----------------|-------|
| **PrestaShop** | `≥ 9.0.0` | Testowano na 9.0.3 z motywem Optima 3.3.0 |
| **PHP** | `≥ 8.1` | Zalecane 8.2+ dla optymalnej wydajności |
| **MySQL/MariaDB** | `≥ 5.7` / `≥ 10.3` | Engine: InnoDB, charset: `utf8mb4_unicode_ci` |
| **Bootstrap** | `4.x` (opcjonalnie) | Tylko dla stylów panelu admina |

---

## 🚀 Instalacja

### Metoda 1: Przez Back Office (rekomendowana)

1. Pobierz archiwum modułu: `kedarwiha_customblock.zip`
2. Zaloguj się do Back Office PrestaShop
3. Przejdź do: **Moduły → Menedżer modułów**
4. Kliknij **"Prześlij moduł"** i wybierz plik `.zip`
5. Po instalacji kliknij **"Konfiguruj"** przy module *KEDAR-WIHA Custom Block*

### Metoda 2: Manualna (FTP/SFTP)

```bash
# 1. Sklonuj repozytorium lub pobierz pliki
git clone https://github.com/PB-MEDIA-Strony-Sklepy-Marketing/prestashop-module_addcustomblocktohooks.git

# 2. Przenieś folder do katalogu modułów PrestaShop
cp -r prestashop-module_addcustomblocktohooks /path/to/prestashop/modules/kedarwiha_customblock

# 3. Ustaw uprawnienia
chmod -R 755 /path/to/prestashop/modules/kedarwiha_customblock

# 4. Zainstaluj przez Back Office lub CLI
php bin/console prestashop:module:install kedarwiha_customblock
```

### ✅ Weryfikacja instalacji

Po instalacji moduł automatycznie:

- Tworzy tabelę `PREFIX_kw_custom_blocks` w bazie danych
- Rejestruje hooki zdefiniowane w metodzie `registerHooks()`
- Dodaje wpis w tabeli `PREFIX_module` z statusem `active = 1`

Sprawdź w Back Office: **Moduły → Menedżer modułów → "KEDAR-WIHA Custom Block"** powinien mieć status **✅ Zainstalowany**.

---

## ⚙️ Konfiguracja

### Dostęp do panelu konfiguracji

```
Back Office → Moduły → Menedżer modułów → KEDAR-WIHA Custom Block → [Konfiguruj]
```

Lub bezpośrednio:

```
/admin/index.php?controller=AdminModules&configure=kedarwiha_customblock&token=XXX
```

### Dodawanie nowego bloku

1. Wypełnij formularz **"Dodaj nowy blok"**:

| Pole                       | Opis                                                                             | Wymagane | Przykład                                    |
| -------------------------- | -------------------------------------------------------------------------------- | -------- | ------------------------------------------- |
| **Tytuł bloku**            | Nazwa identyfikująca blok w panelu admina                                        | ✅        | `Baner promocyjny - Strona Główna`          |
| **Hook**                   | Nazwa hooka PrestaShop, na którym ma się wyświetlić blok                         | ✅        | `displayHome`                               |
| **ID strony CMS**          | Opcjonalne ID strony CMS — jeśli podane, blok wyświetli się tylko na tej stronie | ❌        | `8` (dla strony "Kontakt")                  |
| **Kolejność (sort order)** | Liczba całkowita decydująca o kolejności renderowania bloków na hooku            | ❌        | `0`, `10`, `-5`                             |
| **Zawartość HTML**         | Kod HTML bloku — możesz używać klas Bootstrap 4.x i zmiennych CSS motywu         | ❌        | `<div class="kw-promo-banner">...</div>`    |
| **Custom CSS**             | Style CSS bez tagów `<style>` — zostaną automatycznie owinięte                   | ❌        | `.kw-promo-banner { background: #f5f5f5; }` |
| **Custom JS**              | Kod JavaScript bez tagów `<script>` — zostaną automatycznie owinięte             | ❌        | `console.log('Blok załadowany');`           |

2. Kliknij **"Zapisz blok"**
3. Blok pojawi się na liście — możesz go edytować, wyłączyć lub usunąć

### Zarządzanie istniejącymi blokami

| Akcja               | Opis                                                               |
| ------------------- | ------------------------------------------------------------------ |
| 🔘 **Włącz/Wyłącz** | Przełącznik statusu `active` — bloki nieaktywne nie są renderowane |
| ✏️ **Edytuj**       | Kliknij w tytuł bloku, aby otworzyć formularz edycji               |
| 🗑️ **Usuń**        | Trwałe usunięcie bloku z bazy danych (wymaga potwierdzenia)        |

---

## 🛠️ Użycie

### Struktura renderowanego outputu

Moduł generuje następującą strukturę HTML dla każdego aktywnego bloku:

```html
<!-- Inline CSS (jeśli podany) -->
<style>
  /* Twój custom CSS */
</style>

<!-- Kontener bloku z data-atrybutem -->
<div class="kw-custom-block" data-block-id="123">
  <!-- Twój HTML content -->
</div>

<!-- Inline JS (jeśli podany) -->
<script>
  // Twój custom JS
</script>
```

> ⚠️ **Uwaga:** CSS i JS są wstrzykiwane inline w miejscu hooka. Dla dużych projektów zaleca się ładowanie zasobów przez pliki zewnętrzne i enqueue w `Hook::actionFrontControllerSetMedia`.

### Targetowanie stron CMS

Aby blok wyświetlał się **tylko na konkretnej stronie CMS**:

1. Sprawdź ID strony CMS w Back Office: **Preferencje → SEO i URL → Strony CMS**
2. W formularzu bloku wprowadź to ID w polu **"ID strony CMS"**
3. Zapisz blok

Przykład:

- `id_cms = 8` → blok wyświetli się tylko na stronie "Kontakt"
- `id_cms = NULL` (puste) → blok wyświetli się na wszystkich stronach z danym hookiem

### Kolejność renderowania

Bloki na tym samym hooku są sortowane według:

1. `sort_order ASC` (najpierw niższe wartości)
2. `id ASC` (dla identycznego `sort_order`)

Przykład konfiguracji kolejności:

```
Hook: displayHome
├─ Blok A: sort_order = -10  → renderowany pierwszy
├─ Blok B: sort_order = 0    → renderowany drugi
└─ Blok C: sort_order = 10   → renderowany trzeci
```

---

## 🗄️ Struktura bazy danych

### Tabela: `PREFIX_kw_custom_blocks`

```sql
CREATE TABLE `PREFIX_kw_custom_blocks` (
  `id`         INT UNSIGNED NOT NULL AUTO_INCREMENT,
  `title`      VARCHAR(255) NOT NULL,
  `hook_name`  VARCHAR(100) NOT NULL,
  `content`    MEDIUMTEXT   DEFAULT NULL,
  `custom_css` TEXT         DEFAULT NULL,
  `custom_js`  TEXT         DEFAULT NULL,
  `id_cms`     INT UNSIGNED DEFAULT NULL COMMENT 'Jeśli podane — blok tylko na tej stronie CMS',
  `active`     TINYINT(1)   NOT NULL DEFAULT 1,
  `sort_order` SMALLINT     NOT NULL DEFAULT 0,
  `created_at` DATETIME     NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` DATETIME     NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `idx_kcb_hook`   (`hook_name`),
  KEY `idx_kcb_active` (`active`),
  KEY `idx_kcb_cms`    (`id_cms`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

### Opis kolumn

| Kolumna      | Typ            | Opis                                       |
| ------------ | -------------- | ------------------------------------------ |
| `id`         | `INT UNSIGNED` | Unikalny identyfikator bloku (PK)          |
| `title`      | `VARCHAR(255)` | Tytuł bloku — widoczny tylko w adminie     |
| `hook_name`  | `VARCHAR(100)` | Nazwa hooka PrestaShop (np. `displayHome`) |
| `content`    | `MEDIUMTEXT`   | Zawartość HTML bloku                       |
| `custom_css` | `TEXT`         | Style CSS bez tagów `<style>`              |
| `custom_js`  | `TEXT`         | Skrypty JS bez tagów `<script>`            |
| `id_cms`     | `INT UNSIGNED` | Opcjonalne ID strony CMS do targetowania   |
| `active`     | `TINYINT(1)`   | Status: `1` = aktywny, `0` = nieaktywny    |
| `sort_order` | `SMALLINT`     | Kolejność renderowania na hooku            |
| `created_at` | `DATETIME`     | Data utworzenia rekordu                    |
| `updated_at` | `DATETIME`     | Data ostatniej modyfikacji                 |

---

## 🔗 Dostępne hooki

Moduł rejestruje następujące hooki przy instalacji. Możesz użyć dowolnego z nich w polu **"Hook"** formularza:

### Hooki globalne

```
displayHome
displayTop
displayBanner
displayFooter
displayFooterBefore
displayContentWrapperTop
displayWrapperTop
displayWrapperBottom
```

### Hooki stron CMS

```
displayCMSPageContent
displayCMSPageFooter
```

### Hooki produktów

```
displayProductAdditionalInfo
displayFooterProduct
```

### Hooki koszyka i zamówienia

```
displayShoppingCartFooter
displayOrderConfirmation
```

### Hooki konta klienta

```
displayCustomerAccount
```

### Hooki kolumn bocznych

```
displayLeftColumn
displayRightColumn
```

### Hooki builderów (Optima 3.3.0)

```
displayFooterBuilder
displayHeaderBuilder
```

> 💡 **Tip:** Jeśli potrzebujesz hooka spoza tej listy, dodaj go w metodzie `registerHooks()` w pliku `kedarwiha_customblock.php` i przeinstaluj moduł.

---

## 💡 Przykłady użycia

### 🎯 Przykład 1: Baner promocyjny na stronie głównej

**Konfiguracja bloku:**

- Tytuł: `Promocja VDE - Strona Główna`
- Hook: `displayHome`
- ID CMS: *(puste)*
- Sort order: `0`

**HTML:**

```html
<div class="kw-promo-banner py-4 text-center" style="background: linear-gradient(135deg, #8B0000, #ca0000);">
  <div class="container">
    <h3 class="text-white mb-2 font-weight-bold">🔧 Narzędzia VDE WIHA — Bezpieczeństwo do 1000V</h3>
    <p class="text-white-50 mb-3">Certyfikowane narzędzia izolowane dla profesjonalistów. Darmowa dostawa od 299 zł.</p>
    <a href="/produkty/wkretaki-vde-izolowane" class="btn btn-light btn-sm font-weight-bold text-uppercase">
      Zobacz ofertę
    </a>
  </div>
</div>
```

**CSS:**

```css
.kw-promo-banner {
  border-bottom: 3px solid #FFD700;
  animation: kwFadeIn 0.5s ease both;
}
@keyframes kwFadeIn {
  from { opacity: 0; transform: translateY(-10px); }
  to { opacity: 1; transform: translateY(0); }
}
```

---

### 🎯 Przykład 2: Sekcja "Zaufali nam" tylko na stronie "O nas"

**Konfiguracja bloku:**

- Tytuł: `Zaufali nam - O nas`
- Hook: `displayCMSPageContent`
- ID CMS: `2` *(ID strony "O nas")*
- Sort order: `10`

**HTML:**

```html
<section class="kw-trusted-by py-5">
  <div class="container text-center">
    <h4 class="mb-4 font-weight-bold">Zaufali nam profesjonaliści</h4>
    <div class="d-flex justify-content-center flex-wrap align-items-center gap-4">
      <img src="/img/partners/partner-1.webp" alt="Partner 1" class="img-fluid" style="max-height: 50px; opacity: 0.7;">
      <img src="/img/partners/partner-2.webp" alt="Partner 2" class="img-fluid" style="max-height: 50px; opacity: 0.7;">
      <img src="/img/partners/partner-3.webp" alt="Partner 3" class="img-fluid" style="max-height: 50px; opacity: 0.7;">
    </div>
  </div>
</section>
```

**CSS:**

```css
.kw-trusted-by img {
  transition: opacity 0.3s ease;
  filter: grayscale(100%);
}
.kw-trusted-by img:hover {
  opacity: 1 !important;
  filter: grayscale(0%);
}
```

---

### 🎯 Przykład 3: Skrypt śledzenia konwersji na stronie potwierdzenia zamówienia

**Konfiguracja bloku:**

- Tytuł: `Pixel konwersji - Order Confirmation`
- Hook: `displayOrderConfirmation`
- ID CMS: *(puste)*
- Sort order: `0`

**JS:**

```javascript
document.addEventListener('DOMContentLoaded', function() {
  // Przykład: Google Ads Conversion Tracking
  if (typeof gtag !== 'undefined') {
    gtag('event', 'conversion', {
      'send_to': 'AW-XXXXXXX/YYYYYYY',
      'value': 1.0,
      'currency': 'PLN'
    });
  }

  // Przykład: Facebook Pixel
  if (typeof fbq !== 'undefined') {
    fbq('track', 'Purchase', {
      value: 1.0,
      currency: 'PLN'
    });
  }
});
```

> ⚠️ **Uwaga:** Pamiętaj o RODO i uzyskaniu zgody użytkownika przed ładowaniem skryptów śledzących.

---

## 🔧 Rozwiązywanie problemów

### ❌ Blok nie wyświetla się na stronie

1. **Sprawdź status `active`** — czy blok jest włączony na liście?
2. **Sprawdź hook** — czy hook jest zarejestrowany w motywie i wywoływany w szablonie?
3. **Sprawdź `id_cms`** — czy blok nie jest ograniczony do innej strony CMS?
4. **Wyczyść cache PrestaShop**: **Zaawansowane → Wydajność → Wyczyść cache**
5. **Sprawdź konsolę przeglądarki** — czy nie ma błędów JS blokujących renderowanie?

### ❌ Style CSS nie działają

- Upewnij się, że nie dodajesz tagów `<style>` — moduł robi to automatycznie
- Sprawdź specyficzność selektorów — style motywu mogą nadpisywać Twoje
- Użyj `!important` ostrożnie lub bardziej specyficznych selektorów
- Wyczyść cache CSS w **Zaawansowane → Wydajność**

### ❌ Skrypty JS nie wykonują się

- Upewnij się, że nie dodajesz tagów `<script>` — moduł robi to automatycznie
- Sprawdź, czy skrypt nie zależy na elementach DOM, które jeszcze nie istnieją — użyj `DOMContentLoaded`
- Sprawdź konsolę przeglądarki pod kątem błędów składni lub konfliktów z innymi skryptami

### ❌ Błąd przy instalacji / aktualizacji

1. Sprawdź uprawnienia do katalogu `/modules/kedarwiha_customblock/` (zalecane: `755`)
2. Sprawdź uprawnienia do bazy danych — użytkownik DB musi mieć uprawnienia `CREATE TABLE`
3. Sprawdź logi PrestaShop: `/var/logs/prod.log` lub w Back Office: **Zaawansowane → Logi**
4. Jeśli tabela istnieje z poprzedniej wersji — moduł użyje `CREATE TABLE IF NOT EXISTS`, więc dane powinny zostać zachowane

---

## 🧑‍💻 Rozwój i Contributing

### Struktura projektu

```
kedarwiha_customblock/
├── kedarwiha_customblock.php    # Główny plik modułu (klasa + logika)
├── logo.png                     # Ikona modułu w Back Office
├── views/
│   └── img/                     # Ewentualne zasoby graficzne
├── README.md                    # Ten plik
├── LICENSE                      # Licencja
└── .gitignore                   # Reguły ignorowania plików
```

### Zasady kodowania

- ✅ Przestrzegaj [PrestaShop Coding Standards](https://devdocs.prestashop-project.org/9/modules/concepts/coding-standards/)
- ✅ Używaj `pSQL()` do sanitizacji zapytań SQL
- ✅ Unikaj bezpośrednich zapytań — używaj `Db::getInstance()->executeS()`
- ✅ Dokumentuj publiczne metody PHPDoc
- ✅ Testuj na PrestaShop 9.0.3 przed commitowaniem zmian

### Dodawanie nowych hooków

Aby dodać obsługę nowego hooka:

1. Otwórz plik `kedarwiha_customblock.php`
2. W metodzie `registerHooks()` dodaj nazwę hooka do tablicy `$hooks`:
   
   ```php
   private function registerHooks(): bool
   {
       $hooks = [
           'displayHome',
           // ... inne hooki ...
           'displayMyNewHook',  // ← Dodaj tutaj
       ];
       // ...
   }
   ```
3. Zapisz plik i przeinstaluj moduł (lub ręcznie dodaj hook w tabeli `PREFIX_hook_module`)

### Testowanie lokalne

```bash
# 1. Sklonuj repozytorium
git clone https://github.com/PB-MEDIA-Strony-Sklepy-Marketing/prestashop-module_addcustomblocktohooks.git

# 2. Stwórz symlink do lokalnej instalacji PrestaShop
ln -s /path/to/module kedarwiha_customblock /path/to/prestashop/modules/

# 3. Włącz tryb debugowania w PrestaShop
# Edytuj /config/defines.inc.php:
define('_PS_MODE_DEV_', true);

# 4. Testuj zmiany w Back Office → Moduły
```

### Pull Request workflow

1. **Forkuj** repozytorium
2. **Utwórz branch** z opisową nazwą: `feature/add-display-before-footer-hook`
3. **Wprowadź zmiany** zgodnie z coding standards
4. **Przetestuj** na lokalnej instancji PS 9.0.3
5. **Commituj** z czytelnym message: `feat: add support for displayFooterBefore hook`
6. **Otwórz PR** z opisem zmian, screenshotami (jeśli dotyczy) i listą testów

---

## 📜 Licencja

Ten moduł jest własnością **KEDAR-WIHA.pl** i **PB-MEDIA Strony | Sklepy | Marketing**.

```
Copyright © 2025 KEDAR-WIHA.pl
Wszelkie prawa zastrzeżone.

Niniejszy moduł jest przeznaczony wyłącznie do użytku w ramach sklepu 
internetowego KEDAR-WIHA.pl oraz projektów realizowanych przez PB-MEDIA.

Kopiowanie, modyfikowanie lub dystrybucja kodu bez pisemnej zgody 
właściciela jest zabroniona.
```

> ℹ️ W przypadku zainteresowania komercyjnym wykorzystaniem modułu, prosimy o kontakt: **info@kedar-wiha.pl**

---

## 🆘 Wsparcie

### 📞 Kontakt techniczny

| Kanał             | Dane                                                                                                                    |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **E-mail**        | [info@kedar-wiha.pl](mailto:info@kedar-wiha.pl)                                                                         |
| **Telefon**       | [+48 575 838 766](tel:+48575838766)                                                                                     |
| **WhatsApp**      | [wa.me/48575838766](https://wa.me/48575838766)                                                                          |
| **GitHub Issues** | [Otwórz zgłoszenie](https://github.com/PB-MEDIA-Strony-Sklepy-Marketing/prestashop-module_addcustomblocktohooks/issues) |

### 🔗 Przydatne linki

- 🏪 [Sklep KEDAR-WIHA.pl](https://kedar-wiha.pl)
- 📘 [Dokumentacja PrestaShop 9](https://devdocs.prestashop-project.org/9/)
- 🎨 [Brandbook KEDAR-WIHA.pl](https://github.com/piotroq/DOCS/)
- 🧩 [Repozytorium modułów](https://github.com/piotroq/MODULES/)

### 🐛 Zgłaszanie błędów

Przed zgłoszeniem błędu upewnij się, że:

1. Używasz najnowszej wersji modułu
2. Sprawdziłeś sekcję [Rozwiązywanie problemów](#-rozwiązywanie-problemów)
3. Przetestowałeś na czystej instalacji PrestaShop 9.0.3 (jeśli możliwe)

**W zgłoszeniu podaj:**

- Wersję PrestaShop i PHP
- Krok po kroku opis reprodukcji błędu
- Oczekiwany vs. rzeczywisty rezultat
- Zrzuty ekranu / logi błędów (jeśli dotyczy)

---

## 🔄 Historia wersji

| Wersja  | Data    | Zmiany                                                                                                                                                                                 |
| ------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `1.0.0` | 2025-03 | 🎉 Pierwsza wersja produkcyjna<br>✅ Podstawowa funkcjonalność bloków HTML/CSS/JS<br>✅ Panel administracyjny z CRUD<br>✅ Targetowanie po `id_cms`<br>✅ Sortowanie bloków (`sort_order`) |

---

> **KEDAR-WIHA.pl** — Autoryzowany Dystrybutor Narzędzi WIHA Polska  
> 🛠️ Narzędzia VDE • Klucze dynamometryczne • Zestawy BHP  
> 🌐 [kedar-wiha.pl](https://kedar-wiha.pl) | 📧 info@kedar-wiha.pl | 📞 +48 575 838 766

```
---

✅ **Plik `README.md` jest gotowy do:**
1. Pobrania i zapisania w repozytorium GitHub
2. Zacommitowania:  
   ```bash
   git add README.md
   git commit -m "docs: add comprehensive README.md with usage examples"
   git push origin main
```

3. Wyświetlenia jako główna dokumentacja modułu na GitHub

📌 **Rekomendacja:** Dodaj plik `LICENSE` z treścią licencji oraz `.gitignore` z regułami dla PrestaShop modules przed pierwszym pushem.
