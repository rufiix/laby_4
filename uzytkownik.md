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
### Otrzymanie potwierdzenia zakupu

```mermaid
flowchart TB
    n1["Użytkownik"] --> n2(["Otrzymanie potwierdzenia zakupu"])
    n2 -- include --> n3(["Generowanie biletu"]) & n4(["Anulowanie transakcji"])
    n2 -- extend --> n5(["Wybór formy potwierdzenia (drukowany lub elektroniczny)"])
    n1@{ shape: rect}


```

# Diagramy sekwencji
## Sprawdzenie poprawności transakcji
```mermaid
sequenceDiagram
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
    end

```
