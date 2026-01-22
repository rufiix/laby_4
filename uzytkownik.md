# Diagram przypadków użycia

## Wybor jezyka

```mermaid
flowchart TB
   n1["Użytkownik"] --> n2(["Wybór biletu"])
   n2 -- include --> n3(["Ustawienie domyślnego języka"]) & n4(["Anulowanie transakcji"])
   n2 --extend--> n5(["Wyświetlenie listy popularnych języków"])
   n1@{ shape: rect}
```
### Szybki wybor rodzaju biletow

```mermaid	
flowchart TB
    n1["Użytkownik"] --> n2(["Szybki wybór rodzaju biletu"])
    n2 -- include --> n3(["Weryfikacja dostępnych biletów"]) & n4(["Anulowanie transakcji"])
    n2 -->|extend| n5(["Wyświetlenie podpowiedzi (jeśli użytkownik nie wybiera przez określony
czas)"])
    n1@{ shape: rect}
```
### Sprawdzenie poprawności transakcji

```mermaid
flowchart TB
    n1["Użytkownik"] --> n2(["Sprawdzenie poprawności transakcji"])
    n2 -- include --> n3(["Wyświetlenie podsumowania transakcji"]) & n4(["Anulowanie transakcji"])
    n2 -- extend --> n5(["Ostrzeżenie o błędnym wyborze (gdy wykryto błąd)"])
    n1@{ shape: rect}

```
### Sprawdzenie poprawności transakcji

```mermaid
flowchart TB
    n1["Użytkownik"] --> n2(["Otrzymanie potwierdzenia zakupu"])
    n2 -- include --> n3(["Generowanie biletu"]) & n4(["Anulowanie transakcji"])
    n2 -- extend --> n5(["Wybór formy potwierdzenia (drukowany lub elektroniczny)"])
    n1@{ shape: rect}


```
### Diagramy Sekwencji
### Wybor jezyka

```mermaid
sequenceDiagram
    autonumber
    actor U as Użytkownik
    participant S as System (Biletomat)

    Note over U, S: Krok 1: Rozpoczęcie interakcji
    U->>S: Uruchamia biletomat

    Note right of S: <<Include>> Ustawienie domyślnego języka
    
    opt <<Extend>> Opcjonalnie
        Note right of S: Wyświetlenie listy popularnych języków
    end

    Note over U, S: Krok 2: Wyświetlenie opcji języka
    S-->>U: Wyświetla ekran powitalny z opcjami wyboru

    par Możliwe scenariusze
        rect rgb(240, 248, 255)
            Note over U, S: Krok 3 i 4: Wybór języka (Ścieżka główna)
            U->>S: Wybiera preferowany język
            S-->>U: Dostosowuje interfejs do wybranego języka
        end
    and
        rect rgb(255, 240, 240)
            Note over U, S: Krok 5: Anulowanie (w dowolnym momencie)
            U->>S: Wybiera opcję "Anuluj"
            Note right of S: <<Include>> Anulowanie transakcji
            S-->>U: Przywraca stan początkowy / kończy pracę
        end
    end


```
