# Product Requirements Document (PRD) - TaskR1.0 (MVP)

### 1. Przegląd Produktu

**Nazwa produktu:** TaskR
**Wersja:** 1.0 (Minimum Viable Product)  
**Grupa docelowa:** Uniwersalni użytkownicy (studenci, profesjonaliści, freelancerzy) poszukujący intuicyjnego narzędzia do osobistego zarządzania zadaniami.

#### 1.1 Opis Produktu

TaskRto zaawansowana aplikacja do zarządzania zadaniami, która umożliwia użytkownikom efektywne planowanie, śledzenie i realizację osobistych celów. Aplikacja w wersji MVP skupia się na dostarczeniu solidnych fundamentów w postaci kluczowych funkcji CRUD, intuicyjnego interfejsu oraz niezawodnej synchronizacji danych dla pojedynczego użytkownika.

#### 1.2 Cele Biznesowe (MVP)

- Stworzenie i weryfikacja podstawowej funkcjonalności produktu.
- Zapewnienie stabilnego, responsywnego i intuicyjnego doświadczenia użytkownika na urządzeniach desktopowych i mobilnych.
- Zbudowanie bazy pod przyszły rozwój o bardziej zaawansowane funkcje i ewentualną monetyzację.

### 2. Stack Technologiczny

**Frontend:**

- React 19 z Vite
- TypeScript
- Tailwind CSS
- Shadcn/ui
- Formik + Yup (walidacja formularzy)
- React Query/TanStack Query (zarządzanie stanem serwera)

**Backend & Database:**

- Supabase (Authentication, Database, Real-time)
- PostgreSQL (przez Supabase)

**Dodatkowe narzędzia:**

- React Router (nawigacja)
- Date-fns lub Day.js (obsługa dat)

### 3. Wymagania Funkcjonalne (MVP)

#### 3.1 Moduł Autentykacji

**Opis:** Użytkownik może utworzyć konto, zalogować się i zarządzać swoim hasłem.

**Wymagania:**

- **Rejestracja:** Formularz z polami: email, hasło, potwierdzenie hasła. Walidacja zgodna z Supabase. Po rejestracji użytkownik jest zalogowany i przekierowany do głównego panelu. Wymagana jest weryfikacja email ("miękka" - użytkownik może korzystać z aplikacji, widząc nieinwazyjny baner z prośbą o weryfikację).
- **Logowanie:** Formularz (email, hasło). Opcja "Zapamiętaj mnie" na standardową długość sesji przeglądarki. Po zalogowaniu przekierowanie do panelu zadań.
- **Resetowanie Hasła:** Standardowy przepływ Supabase (email z linkiem do strony resetowania hasła).
- **Routing:** Użytkownicy niezalogowani próbujący uzyskać dostęp do panelu zadań są przekierowywani na stronę logowania.

#### 3.2 Moduł Zarządzania Zadaniami (CRUD)

**Główny widok:** Domyślnym widokiem jest elastyczna **lista zadań**, z panelem filtrów umieszczonym powyżej.

##### 3.2.1 Dodawanie zadań (CREATE)

- Przycisk dodawania zadania (np. ikona `+`) jest stale widoczny w interfejsie.
- Kliknięcie przycisku otwiera **formularz w oknie modalnym (dialogowym)**.
- **Pola formularza:**
  - Tytuł (wymagane, max 100 znaków)
  - Opis (opcjonalne, max 500 znaków)
  - Status (pole nieedytowalne, domyślnie ustawiane na **`Backlog`**)
  - Priorytet (`Niski`, `Średni`, `Wysoki`, `Krytyczny`)
  - Kolor (do wyboru z predefiniowanej palety 8 kolorów)
  - Termin (opcjonalne, pole wyboru daty i czasu)
- Informacje zwrotne o sukcesie (toast: "Zadanie utworzono") lub błędzie są wyświetlane po zamknięciu modala.

##### 3.2.2 Wyświetlanie zadań (READ)

- Zadania są wyświetlane na liście w formie kart (`TaskCard`).
- **Karta zadania (`TaskCard`) zawiera:**
  - Tytuł zadania.
  - Wskaźnik priorytetu (kolorowa ikona/tag).
  - Termin (wyświetlany relatywnie, np. "za 2 dni", z kolorowym wskaźnikiem: zielony >24h, żółty <24h, czerwony - po terminie).
  - Wskaźnik przypisanego koloru (np. boczna ramka karty).
  - **Menu rozwijane (`Select`) do szybkiej zmiany statusu**.
- Pełne dane zadania (w tym opis) są dostępne w formularzu edycji.
- **Domyślne sortowanie** listy zadań: według **priorytetu** (od Krytycznego do Niskiego).

##### 3.2.3 Edytowanie zadań (UPDATE)

- **Zmiana statusu:** Użytkownik może zmienić status zadania bezpośrednio z karty, używając menu rozwijanego (`Select`). Zmiana jest **zapisywana automatycznie** w bazie danych, a na karcie pojawia się chwilowy wskaźnik ładowania. Toast potwierdza operację ("Status zaktualizowano").
- **Pełna edycja:** Kliknięcie na kartę zadania (poza menu zmiany statusu) otwiera **formularz edycji w tym samym oknie modalnym**, co przy tworzeniu, wypełniony danymi zadania. Użytkownik może modyfikować wszystkie pola.
- Historia zmian ograniczona jest do znacznika `updated_at`.

##### 3.2.4 Usuwanie zadań (DELETE)

- Na karcie zadania lub w formularzu edycji znajduje się opcja usunięcia.
- Usunięcie wymaga **potwierdzenia w oknie modalnym** ("Czy na pewno chcesz usunąć to zadanie? Tej operacji nie można cofnąć.").
- Zadania są usuwane **trwale (hard delete)** z bazy danych. W MVP nie ma funkcji kosza ani "soft delete".

#### 3.3 System Statusów i Priorytetów

- **Statusy (ENUM):** `Backlog`, `W toku` (`in_progress`), `Wykonane` (`done`), `Nie wykonam` (`cancelled`).
- **Priorytety (ENUM):** `Niski` (`low`), `Średni` (`medium`), `Wysoki` (`high`), `Krytyczny` (`critical`).
- Priorytety są wizualnie wyróżnione na karcie zadania.

#### 3.4 System Filtrowania i Wyszukiwania

- Panel filtrów jest umieszczony nad listą zadań.
- **Dostępne filtry (wielokrotny wybór):**
  - Status zadania
  - Priorytet
  - Kolor
- Filtry z różnych kategorii łączone są operatorem `AND`.
- **Wyszukiwanie tekstowe:** Pasek wyszukiwania przeszukuje `tytuł` i `opis` zadań w czasie rzeczywistym (debounce 300ms).
- **Zapisywanie filtrów:** Preferencje filtrowania są automatycznie zapisywane w bazie danych w profilu użytkownika i przywracane podczas kolejnej sesji.
- Dostępny jest przycisk "Wyczyść filtry".

### 4. Wymagania Niefunkcjonalne (MVP)

- **Wydajność:** Czas ładowania aplikacji < 2s. Szybkie odpowiedzi interfejsu < 200ms.
- **Bezpieczeństwo:** Pełne oparcie o mechanizmy Supabase (Auth, RLS). Walidacja po stronie klienta i serwera.
- **Użyteczność (UX/UI):**
  - Responsywny design (mobile-first).
  - Intuicyjny i czysty interfejs.
  - Obsługa trybów Dark/Light.
  - Użyteczne stany puste (np. dla nowego użytkownika bez zadań).
  - System powiadomień "toast" dla informacji zwrotnych.
- **Kompatybilność:** Wsparcie dla najnowszych wersji Chrome, Firefox, Safari, Edge.

### 5. Struktura Bazy Danych (PostgreSQL)

#### 5.1 Tabela `users`

- Zarządzana przez Supabase Auth.

#### 5.2 Tabela `profiles`

- `id` (UUID, PK, FK to `users.id`)
- `preferences` (JSONB, nullable) - do przechowywania np. zapisanych filtrów.

#### 5.3 Tabela `tasks`

- `id` (UUID, PK)
- `user_id` (UUID, FK to `users.id`, NOT NULL)
- `title` (varchar(100), NOT NULL)
- `description` (text)
- `status` (ENUM: `backlog`, `in_progress`, `done`, `cancelled`), NOT NULL
- `priority` (ENUM: `low`, `medium`, `high`, `critical`), NOT NULL
- `color` (varchar(20))
- `deadline` (timestamp with timezone)
- `created_at` (timestamp, DEFAULT now())
- `updated_at` (timestamp, DEFAULT now())

#### 5.4 Indeksy

- `idx_tasks_user_id`
- `idx_tasks_status`
- `idx_tasks_priority`

### 6. Architektura Aplikacji i Główne Komponenty

- **Struktura folderów:** Zgodna z pierwotnym założeniem.
- **Główne komponenty:**
  - `TaskList`: Główny widok z listą zadań.
  - `TaskCard`: Karta pojedynczego zadania z kluczowymi informacjami i menu zmiany statusu.
  - `TaskForm`: Formularz w oknie modalnym do tworzenia/edycji zadań.
  - `FilterPanel`: Panel z kontrolkami do filtrowania i wyszukiwania.
  - `AuthForms`: Formularze logowania/rejestracji.

### 7. Funkcje wykluczone z MVP (Plany na przyszłość)

- Przeciąganie i upuszczanie (Drag & Drop).
- Powiadomienia (w aplikacji, push, email).
- Kosz i przywracanie zadań (Soft delete).
- Akcje masowe na zadaniach.
- Personalizacja tablicy Kanban i etykiet kolorów.
- Pełna zgodność z WCAG i aplikacja PWA.
- Współdzielenie zadań i praca zespołowa.

### 8. Historie Użytkownika

#### 8.1 Pierwsze Użycie Aplikacji

**Scenariusz:** Nowy użytkownik chce rozpocząć korzystanie z aplikacji.

**Kroki:**

1. Użytkownik wchodzi na stronę główną aplikacji.
2. Wybiera opcję "Zarejestruj się".
3. Wypełnia formularz rejestracji (email, hasło, potwierdzenie hasła).
4. System weryfikuje dane i tworzy konto.
5. Użytkownik jest automatycznie zalogowany i przekierowany do głównego panelu.
6. Widzi pusty stan z przyciskiem dodawania pierwszego zadania.
7. System wyświetla nieinwazyjny baner z prośbą o weryfikację email.

#### 8.2 Zarządzanie Codziennymi Zadaniami

**Scenariusz:** Użytkownik chce dodać i zarządzać swoimi codziennymi zadaniami.

**Kroki:**

1. Użytkownik klika przycisk "+" aby dodać nowe zadanie.
2. Wypełnia formularz:
   - Wprowadza tytuł "Przygotować prezentację na spotkanie"
   - Dodaje opis "Prezentacja o wynikach Q2"
   - Ustawia priorytet "Wysoki"
   - Wybiera kolor "Niebieski"
   - Ustawia termin na jutro
3. Zatwierdza formularz.
4. Widzi nowe zadanie na liście z odpowiednim oznaczeniem priorytetu i terminu.
5. Może szybko zmienić status zadania używając menu rozwijanego na karcie.

#### 8.3 Organizacja i Filtrowanie Zadań

**Scenariusz:** Użytkownik chce znaleźć konkretne zadania wśród wielu wpisów.

**Kroki:**

1. Użytkownik używa panelu filtrów nad listą zadań.
2. Wybiera status "W toku" i priorytet "Wysoki".
3. System pokazuje tylko zadania spełniające oba kryteria.
4. Użytkownik może dodatkowo użyć wyszukiwarki tekstowej.
5. Wprowadza frazę "prezentacja" w polu wyszukiwania.
6. System filtruje wyniki w czasie rzeczywistym.
7. Użytkownik może wyczyścić wszystkie filtry jednym przyciskiem.

#### 8.4 Aktualizacja Statusu Zadania

**Scenariusz:** Użytkownik chce oznaczyć zadanie jako wykonane.

**Kroki:**

1. Użytkownik znajduje zadanie na liście.
2. Używa menu rozwijanego na karcie zadania.
3. Zmienia status z "W toku" na "Wykonane".
4. System automatycznie zapisuje zmianę.
5. Użytkownik widzi potwierdzenie zmiany (toast).
6. Zadanie pozostaje na liście, ale jest oznaczone jako wykonane.

#### 8.5 Edycja Istniejącego Zadania

**Scenariusz:** Użytkownik chce zaktualizować szczegóły zadania.

**Kroki:**

1. Użytkownik klika na kartę zadania.
2. Otwiera się formularz edycji z aktualnymi danymi.
3. Użytkownik modyfikuje:
   - Aktualizuje opis
   - Zmienia priorytet
   - Przesuwa termin
4. Zatwierdza zmiany.
5. System aktualizuje zadanie i pokazuje potwierdzenie.

#### 8.6 Usunięcie Zadania

**Scenariusz:** Użytkownik chce usunąć nieaktualne zadanie.

**Kroki:**

1. Użytkownik znajduje zadanie do usunięcia.
2. Klika przycisk usuwania na karcie lub w formularzu edycji.
3. System pokazuje modal z prośbą o potwierdzenie.
4. Użytkownik potwierdza usunięcie.
5. Zadanie jest trwale usunięte z systemu.
6. System pokazuje potwierdzenie usunięcia.

#### 8.7 Powrót do Aplikacji

**Scenariusz:** Użytkownik wraca do aplikacji po przerwie.

**Kroki:**

1. Użytkownik otwiera aplikację.
2. Jeśli sesja jest aktywna, jest automatycznie zalogowany.
3. Widzi swoje ostatnie filtry i sortowanie.
4. Może kontynuować pracę z miejsca, w którym skończył.
5. Jeśli sesja wygasła, jest przekierowany do logowania.

#### 8.8 Resetowanie Hasła

**Scenariusz:** Użytkownik zapomniał hasła.

**Kroki:**

1. Użytkownik klika "Zapomniałem hasła" na ekranie logowania.
2. Wprowadza swój email.
3. Otrzymuje email z linkiem do resetowania.
4. Klika link i jest przekierowany do formularza resetowania.
5. Ustawia nowe hasło.
6. Jest automatycznie zalogowany do aplikacji.
