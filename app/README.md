Aplikacja demonstrująca nawigację między dwiema aktywnościami, przekazywanie danych przez `Intent.putExtra()`, odbieranie wyniku przez `ActivityResultLauncher` oraz udostępnianie tekstu intencją niejawną.

![Ekran formularza](docs/screen1.png)
![Ekran podsumowania](docs/screen2.png)

1) Dlaczego rejestracja wyniku jest polem klasy, a nie znajduje się w obsłudze kliknięcia przycisku?**
Rejestracja `registerForActivityResult()` musi zostać wykonana przed osiągnięciem przez aktywność stanu `STARTED` (czyli przed wyświetleniem ekranu użytkownikowi). Jeśli system Android w tle zniszczy pierwszą aktywność z powodu braku pamięci i odtworzy ją po powrocie z drugiego ekranu, mechanizm odbierający wynik musi być już zarejestrowany podczas inicjalizacji klasy. Wywołanie rejestracji w `onClick` rzuciłoby wyjątek `IllegalStateException: LifecycleOwners must call register before they are STARTED`.

2) Co zobaczy ekran wywołujący, gdy użytkownik naciśnie na drugim ekranie przycisk Wstecz?**
Ekran wywołujący (formularz) wróci na wierzch stosu z zachowanymi wszystkimi wpisanymi wcześniej danymi. Nie pojawi się żaden komunikat (`Snackbar`) o potwierdzeniu, ponieważ system automatycznie zwraca kod `RESULT_CANCELED`, a nasz kod w `summaryLauncher` reaguje wyłącznie na `RESULT_OK`.