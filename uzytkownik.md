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
sequenceDiagram
    autonumber
    actor A1 as Użytkownik
    participant P1 as Interfejs sprzedaży biletu
    
    A1->>P1: wybierz_bilet_i_platnosc()
    P1-->>A1: wyswietl_podsumowanie(typBiletu, cena, metodaPlatnosci)
    
    alt Uzytkownik chce kontynuowac
        A1->>P1: potwierdz()
        P1->>P1: przetworz_transakcje()
    else Uzytkownik chce anulowac
        A1->>P1: anuluj()
        P1->>P1: przerwij_sesje()
    end
```
## Otrzymanie potwierdzenia zakupu
```mermaid
sequenceDiagram
  actor A1 as Użytkownik
  participant P1 as Interfejs sprzedaży biletu
  participant P2 as Serwer autoryzacyjny

    A1->>P1: dokonaj_zakupu()
    P1->>P2: sprawdz_poprawnosc_danych_platnosci()
    P2-->>P1: wynik_autoryzacji (OK)
    
    P1->>P1: generuj_potwierdzenie()
    
    P1->>A1: wydaj_bilet_i_paragon()
    P1-->>A1: wyswietl_komunikat_zakonczenia()

```
### Diagramy Klas
## Otrzymanie potwierdzenia zakupu
```mermaid
classDiagram
    class InterfejsSprzedazy {
        -Bilet aktualnyBilet
        +dokonaj_zakupu(DanePlatnosci dane)
        -generuj_potwierdzenie()
        +wydaj_bilet_i_paragon() : Wydruk
        +wyswietl_komunikat_zakonczenia()
    }

    class SerwerAutoryzacyjny {
        +sprawdz_poprawnosc_danych_platnosci(DanePlatnosci dane) StatusAutoryzacji
    }

    class DanePlatnosci {
        +String numerKarty
        +String wlasciciel
        +String kodCVV
    }

    class Bilet {
        +String typ
        +double cena
        +String strefa
    }

    class Paragon {
        +String idTransakcji
        +Date data
        +double kwota
    }

    class StatusAutoryzacji {
        <<enumeration>>
        ZATWIERDZONA
        ODRZUCONA
        BLAD_LACZNOSCI
    }

    %% Relacje
    InterfejsSprzedazy ..> SerwerAutoryzacyjny : korzysta (use)
    InterfejsSprzedazy ..> DanePlatnosci : przetwarza
    InterfejsSprzedazy --> Bilet : zarządza
    InterfejsSprzedazy ..> Paragon : tworzy
    SerwerAutoryzacyjny ..> StatusAutoryzacji : zwraca
```
## Sprawdzenie poprawności transakcji
```mermaid
classDiagram
    %% Główny kontroler interakcji
    class InterfejsSprzedazy {
        -Transakcja aktualnaTransakcja
        
        %% Metody wywoływane przez Użytkownika (Publiczne)
        +wybierz_bilet_i_platnosc(typ, metoda)
        +potwierdz()
        +anuluj()
        
        %% Metody wewnętrzne / Wyjście do Użytkownika (Prywatne/Protected)
        -wyswietl_podsumowanie()
        -przetworz_transakcje()
        -przerwij_sesje()
    }

    %% Klasa reprezentująca dane, które pojawiły się w parametrach
    class Transakcja {
        +String typBiletu
        +double cena
        +String metodaPlatnosci
        +StatusTransakcji status
    }

    %% Opcjonalnie: Typ wyliczeniowy dla statusu (dla precyzji)
    class StatusTransakcji {
        <<enumeration>>
        OCZEKUJE
        ZATWIERDZONA
        ANULOWANA
    }

    %% Relacje
    InterfejsSprzedazy "1" *-- "0..1" Transakcja : zarządza
    Transakcja ..> StatusTransakcji : posiada

## Wizualizacja diagramu klas
```mermaid
classDiagram
    class Użytkownik{
        - String username
        + void uruchomBiletomat()
        + void wybierzPreferowanyJezyk()
        + void wybierzOpcjeAnuluj()
    }

    class Biletomat{
        - String wybranyJezyk
        - String idSesji
        - void ustawienieDomyslnegoJezyka()
        + void wyswietlEkranPowitalnyZOpcjami()
        + void dostosujInterfejsDoJezyka(wybranyJezyk: String)
        - void uruchomProcedureAnulowania()
    }
    class Server{
        - List listaPopularnychJezykow
        + List pobierzListePopularnychJezykow()
    }

    Biletomat-->Server:pobieraDane
    Użytkownik-->Biletomat:ustawiaJezyk

```
