\##Diagram przypadków użycia

\###Wybor jezyka

```MERMAID



flowchart TB

&nbsp;   n1\["Użytkownik"] --> n2(\["Wybór biletu"])

&nbsp;   n2 -- include --> n3(\["Ustawienie domyślnego języka"]) \& n4(\["Anulowanie transakcji"])

&nbsp;   n2 --extend--> n5(\["Wyświetlenie listy popularnych języków"])



&nbsp;   n1@{ shape: rect}



```

