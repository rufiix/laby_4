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
    participant S as Biletomat

    U->>S: Uruchamia biletomat
    
    S->>S: Ustawienie domyślnego języka
    
    opt Wyświetlenie listy popularnych języków
        S->>S: Pobranie listy popularnych języków
    end

    S-->>U: Wyświetla ekran powitalny z opcjami

    alt Wybór języka
        U->>S: Wybiera preferowany język
        S-->>U: Dostosowuje interfejs do języka
    else Anulowanie transakcji
        U->>S: Wybiera opcję Anuluj
        S->>S: Procedura anulowania
        S-->>U: Potwierdzenie anulowania / Ekran startowy
    end

```
### Szybki wybor rodzaju biletow

```mermaid	
sequenceDiagram
    autonumber
    actor U as Użytkownik
    participant S as Biletomat

    U->>S: Rozpoczęcie interakcji

    alt Ścieżka wyboru biletu
        U->>S: Wybór kategorii biletu
        
        S->>S: Weryfikacja dostępnych biletów
        
        S-->>U: Wyświetlenie listy biletów z danej kategorii

        opt Brak aktywności przez określony czas
            S-->>U: Wyświetlenie podpowiedzi
        end

        U->>S: Wybór konkretnego biletu
        S-->>U: Wyświetlenie podsumowania wyboru
        U->>S: Potwierdzenie wyboru (Realizacja)

    else Anulowanie transakcji
        U->>S: Wybór opcji Anuluj
        S->>S: Procedura anulowania
        S-->>U: Powrót do ekranu startowego
    end
```
## Sprawdzenie poprawności transakcji
```mermaid
actor A1 as Użytkownik
  participant P1 as Interfejs sprzedaży biletu
  autonumber
  A1->>P1: wybierz_bilet_i_platnosc()
  P1-->>A1: wyswietl_podsumowanie(typBiletu, cena, metodaPlatnosci)
  alt uzytkownik chce kontynuowac
    A1->>P1: potwierdz()
    P1->>P1: kontunuuj()
    else uzytkownik chce anulowac
    A1->>P1: anuluj()
    P1->>P1: przerwij()
```
## Otrzymanie potwierdzenia zakupu
```mermaid
sequenceDiagram
  actor A1 as Użytkownik
  participant P1 as Interfejs sprzedaży biletu
  participant P2 as Serwer autoryzacyjny

    A1->>P1: dokonaj_zakupu()
    P1->>P2: sprawdz_poprawnosc_danych_platnosci()
  P2-->>P1:
  P1->>P1: generuj_potwierdzenie()
  P1-)A1: odbierz_potwierdzenie()
  P1-)A1: wyswietl_komunikat_potwierdzenia_zakonczenia()
  P1-->>A1:

```
