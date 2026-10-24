# Dokumentacja połączeń – puszka WC, płytki A i B, ekspander MCP

2026-10-14 · @Marcin

Opracowanie na podstawie odręcznych notatek: przewody 230 V w puszce, złącza płytki A, płytka B z CPU i ekspanderem MCP.

## Przegląd systemu

System składa się z puszki z przewodami 230 V (00–27), płytki A ze złączami A34–P35 oraz płytki B z CPU, optoizolacją, ekspanderem 16 bit (MCP) i konwerterem RS485.

| Element | Rola |
| --- | --- |
| Puszka WC | Przewody 00–27: ogrzewanie, mata elektryczna WC i Hol, oświetlenie, włącznik, lustro, zasilanie 230 V |
| Płytka A | Złącza A34, B26, C27, D36, E33, F49, G28, H39, I32, J24, K48, L40, M25, N37, O20, P35; rozdziela sygnały czujników, przekaźników i zasilanie 24 V/5 V |
| Płytka B | CPU, OPTO IN, OPTO OUT, ekspander IO 16 bit (A0–A7, B0–B7), złącza A48, A49, B48, moduł RS485 |
| RS485 | Moduł TTL↔RS485 (VCC, TX, RX, GND; wyjścia A+, B−, GND) połączony z P35 i K48 |

Sygnał od czujnika ruchu jest zwarty do masy przez rezystor, odczytany jako 470 kΩ (na schemacie MCP 4,7 kΩ do VCC) – do potwierdzenia.

## Przewody w puszce (00–27)

Przewody ponumerowane 00–27 tworzą 9 grup (A–I); każda grupa to PE/N/L jednego odbiornika, a pozostałe pary łączą przekaźniki i zasilacz.

| Grupa | Przeznaczenie | Przewody |
| --- | --- | --- |
| A | Zasilanie ogrzewania od licznika (230 V) | 00 – PE, 01 – N, 02 – L |
| B | Zasilanie maty elektrycznej WC | 03 – PE, 04 – N, 05 – L |
| C | Zasilanie maty elektrycznej Hol | 06 – PE, 07 – N, 08 – L |
| J | Przekaźnik WC / przekaźnik Hol | 09 – żółty (przekaźnik WC), 10 – zielony (przekaźnik Hol), 11 – L |
| E | Włącznik światła | 12 – L (sygnał klawisza), 13 – PE (5 V podświetlenia), 14 – N (masa klawisza i diody) |
| G | Zasilanie 230 V sufit | 15 – L1, 16 – L2 (na schemacie: 15 → L, 16 → N) |
| D | Zasilanie światła na suficie | 17 – L, 18 – PE, 19 – N |
| H | Powrót od przekaźników | 20 – L, 23 – PE, 24 – N |
| F | Oświetlenie lustra | 21 – L, 22 – PE, 25 – N |
| I | Zasilanie zaworu | 26 – N, 27 – L |

### Łączenia w puszce

| Łączone przewody | Funkcja |
| --- | --- |
| 00 – 03 – 06 | PE ogrzewania, maty WC i maty Hol |
| 01 – 04 – 07 | N ogrzewania i obu mat |
| 02 – 11 (?) | L ogrzewania do przekaźników |
| 09 – 05 | L maty WC – przekaźnik WC |
| 10 – 08 | L maty Hol – przekaźnik Hol |
| 18 – 22 | PE światła i lustra |
| 19 – 16 | N światła |
| 17 – 15 | L światła |
| 24 – 25 – 26 | N – powrót od przekaźników |
| 20 – 21 | L dla lustra |
| 23 – 27 | L dla zasilacza (w wykazie przewód 23 to PE – do potwierdzenia) |
| 12 | L Sygnał klawisz L40.3 - biały |
| 13 | PE 5V dla podświetlenia klawisza L40.1 - pomarańczowy |
| 14 | N Masa dla klawisza i diody L40.2 -niebieski |

## Złącza płytki A

Płytka A ma 16 złączy; poniżej pinouty z kolorami żył (kolor „–” = nieczytelny w notatkach).

### A34 – Zasilanie 24 V i sygnały sterujące WC (8 pin)

| Pin | Sygnał | Kolor żyły | Połączenie | Zmienna w kodzie |
| --- | --- | --- | --- | --- |
| 1 | +24 V_ON | zielony | 24V |  |
| 2 | GND | niebieski | GND |  |
| 3 | +24 V_OFF (nieużywany, „brak sygnału”) | czerwony | Brak w złączu |  |
| 4 | GND | czarny | GND |  |
| 5 | Sygnał 2. taryfa | brązowy | A34.5-F49.9 | p_2taryfa |
| 6 | Sygnał Zasilanie awaryjne | szary | A34.6-F49.8 | p_zasAwar |
| 7 | Sygnał Noc | biały | A34.7-F49.7 | p_ruch |
| 8 | Sygnał Wieczór | żółty | A34.8-F49.6 | p_wieczor |

### B26 – Czujnik ruchu WC (4 pin)

| Pin | Sygnał | Kolor żyły | Połączenie | Zmienna w kodzie |
| --- | --- | --- | --- | --- |
| 1 | Sygnał czujnika ruchu | niebieski | B26.1-F49.1 | p_ruch |
| 2 | +5 V | pomarańczowy | 5V |  |
| 3 | GND | biały | GND |  |
| 4 | GND | biały | GND |  |

### C27 – Kontaktron w drzwiach WC(2 pin)

| Pin | Sygnał | Kolor żyły | Połączenie | Zmienna w kodzie |
| --- | --- | --- | --- | --- |
| 1 | Sygnał kontaktronu | niebieski | C27.1-F49.3 | p_drzwi |
| 2 | GND | biały | GND |  |

### D36 – Przekaźniki ogrzewania i czujniki temperatury WC(8 pin)

| Pin | Sygnał | Powiązanie | Kolor żyły | Połączenie | Zmienna w kodzie |
| --- | --- | --- | --- | --- | --- |
| 1 | Sygnał wyjściowy czujnika | B48.5 | szary | D36.1-K48.4 | WY_CZUJNIK_RUCHU |
| 2 | Przekaźnik ogrzewania Hol | - | zielony | D36.2-H39.2 |  |
| 3 | Przekaźnik ogrzewania WC | - | żółty | D36.3-H39.3 |  |
| 4 | Sygnał wyjściowy kontaktronu | B48.6 | czarny | D36.4-K48.3 | WY_KONTAKTRON |
| 5 | Czujnik temperatury Hol |  | brązowy | D36.5-N37.3 |  |
| 6 | Czujnik temperatury WC |  | fioletowy | D36.6-O20.2 |  |
| 7 | Czujnik temperatury podłogi WC |  | niebieski | D36.7-I32.2 |  |
| 8 | Czujnik temperatury podłogi Hol |  | biały | D36.8-E33.2 |  |

### E33 – Czujnik temperatury temperatury podłoga Hol (3 pin)

| Pin | Sygnał | Kolor żyły | Połączenie |
| --- | --- | --- | --- |
| 1 | +5 V | czerwony | 5V |
| 2 | Sygnał temperatury | żółty | D36.8-E33.2 |
| 3 | GND | czarny | GND |

### I32 – Czujnik temperatury temparatury podłoga WC (3 pin)

| Pin | Sygnał | Kolor żyły | Połączenie |
| --- | --- | --- | --- |
| 1 | GND | czarny | GND |
| 2 | Sygnał temperatury | żółty | D36.7-I32.2 |
| 3 | +5 V | czerwony | 5V |

### G28 – Czujnik zbliżeniowy pod lustrem (4 pin)

| Pin | Sygnał | Kolor żyły | Połączenie | Zmienna w kodzie |
| --- | --- | --- | --- | --- |
| 1 | Sygnał czujnika zbliżeniowego | niebieski | F49.2-G28.1 | p_zblizeniowy |
| 2 | +5 V | pomarańczowy | 5V |  |
| 3 | GND | biały | GND |  |
| 4 | GND | biały | GND |  |

### F49 – Magistrala A do płytki 2(A49) (12 pin)

| Pin F49 | Sygnał | Kolor żyły | Pin A49 | Nazwa w kodzie | Numer wejścia w MCP | Połączenie |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Czujnik ruchu | jasnozielony | 10 | p_ruch | 0 | B26.1-F49.1 |
| 2 | Czujnik zbliżeniowy | brązowy | 9 | p_zblizeniowy | 1 | F49.2-G28.1 |
| 3 | Kontaktron (drzwi) | szary | 8 | p_drzwi | 2 | C27.1-F49.3 |
| 4 | Klawisz WC | pomarańczowy | 7 | p_klawisz | 3 | F49.4-L40.3 |
| 5 | +5 V | czerwony | 2 |  |  | 5V |
| 6 | Sygnał wieczór | żółty | 4 | p_wieczor | 6 | A34.8-F49.6 |
| 7 | Sygnał noc | biały | 3 | p_noc | 12 | A34.7-F49.7 |
| 8 | Sygnał zasilania awaryjnego | fioletowy | 6 | p_zasAwar | 4 | A34.6-F49.8 |
| 9 | Sygnał 2. taryfa | ciemnobrązowy | 5 | p_2taryfa | 5 | A34.5-F49.9 |
| 10 | GND | niebieski | 12 |  |  | GND |
| 11 | Oświetlenie – sygnał wyjściowy | zielony | 11 | WY_SWIATLO | Procek:A7 | F49.11-P35.8 |
| 12 | GND | czarny | 1 |  |  | GND |

### H39 – Przekaźniki ogrzewania (4 pin)

| Pin | Sygnał | Kolor żyły | Kanał | Połączenie |
| --- | --- | --- | --- | --- |
| 1 | GND | czarny |  | GND |
| 2 | Przekaźnik Hol | żółty | 1 | D36.2-H39.2 |
| 3 | Przekaźnik WC | zielony | 2 | D36.3-H39.3 |
| 4 | +5 V | czerwony |  | 5V |

### J24 – Moduł 6 przekaźników oświetlenia (8 pin)

| Pin | Sygnał | Kolor żyły | Kanał | Połączenie | Zmienna w kodzie |
| --- | --- | --- | --- | --- | --- |
| 1 | GND | czarny |  | GND |  |
| 2 | +5 V | czerwony |  | 5V |  |
| 3 | Przekaźnik 6 | brązowy | 6 | J24.3-P35.4 |  |
| 4 | Przekaźnik 5 | zielony | 5 | J24.4-P35.3 |  |
| 5 | Przekaźnik 4 (zawór) | fioletowy | 4 | J24.5-K48.9 | ZAWOR |
| 6 | Przekaźnik 3 (lustro) | niebieski | 3 | J24.6-K48.10 | LUSTRO |
| 7 | Przekaźnik 2 (światło) | biały | 2 | J24.7-K48.11 | SWIATLO |
| 8 | Przekaźnik 1 (regał) | żółty | 1 | J24.8-K48.12 | SZAFKA |

### K48 – Magistrala B do płytki 2 (B48) (12 pin)

| Pin | Sygnał | Powiązanie (płytka B) | Kolor żyły | Połączenie | Pinou B48 |
| --- | --- | --- | --- | --- | --- |
| 1 | RS-485-A | – | brązowy | K48.1-P35.1 | 8 |
| 2 | RS-485-B | – | zielony | K48.2-P35.2 | 7 |
| 3 | Wyjście – kontaktron | B48.6 / MCP B.6 | pomarańczowy | D36.3-K48.1 | 6 |
| 4 | Wyjście – czujnik ruchu | B48.7 / MCP B.7 | szary | D36.4-K48.3 | 5 |
| 5 | Sygnał od lokalnego czujnika temperatury | CPU.4 | jasnobrązowy |  | 1 |
| 6 | PIN 1 (gabaryt) | – | czarny | K48.6-O20 .6 | 2 |
| 7 | PIN 2 (gabaryt) | – | jasnozielony | K48.7-O20 .7 | 3 |
| 8 | PIN 3 (gabaryt) | – | czerwony | K48.8-O20 .8 | 4 |
| 9 | Przekaźnik 1 (regał) | MCP B.3 | żółty | J24.5-K48.9 | 12 |
| 10 | Przekaźnik 2 (lustro) | MCP B.2 | biały | J24.6-K48.10 | 11 |
| 11 | Przekaźnik 3 (światło) | MCP B.1 | niebieski | J24.7-K48.11 | 10 |
| 12 | Przekaźnik 4 (zawór) | MCP B.0 | fioletowy | J24.8-K48.12 | 9 |

### L40 – Włącznik z podświetleniem (3 pin)

| Pin | Sygnał | Kolor żyły | Przewód w puszce | Połączenie |
| --- | --- | --- | --- | --- |
| 1 | +5 V podświetlenia klawisza | pomarańczowy | 13 | 5V |
| 2 | Masa klawisza i diody | niebieski | 14 | GND |
| 3 | Sygnał klawisza | biały | 12 | F49.4-L40.3 |

### M25 – rezerwa (8 pin)

Wszystkie 8 pinów niewykorzystane (kolory wiązki: brązowy, czarny, zielony, czerwony, fioletowy, niebieski, biały, żółty).

### N37 – panel klawiszy z czujnikiem temperatury Hol(8 pin)

| Pin | Sygnał | Kolor żyły | Połączenie |
| --- | --- | --- | --- |
| 1 | +5 V | czerwony | 5V |
| 2 | +5 V | brązowy | 5V |
| 3 | Sygnał czujnika temperatury | zielony | D36.5-N37.3 |
| 4 | GND | czarny | GND |
| 5 | GND | niebieski | GND |
| 6 | Klawisz 1 | biały | N37.6-P35.6 |
| 7 | Klawisz 2 | żółty | N37.7-P35.7 |
| 8 | Podświetlenie | fioletowy | N37.8-P35.5 |

### O20 – Czujnik temperatury WC i gabaryt (8 pin)

| Pin | Sygnał | Kolor żyły |  |
| --- | --- | --- | --- |
| 1 | +5 V | pomarańczowy | 5V |
| 2 | Sygnał czujnika temperatury | niebieski |  |
| 3 | – | żółty |  |
| 4 | GND | biały | GND |
| 5 | GND | czarny | GND |
| 6 | PIN 1 (gabaryt) | brązowy |  |
| 7 | PIN 2 (gabaryt) | zielony |  |
| 8 | PIN 3 (gabaryt) | czerwony |  |

### P35 – RS-485 i klawisze (8 pin)

| Pin | Sygnał | Kolor żyły | Połączenie | Powiązanie |
| --- | --- | --- | --- | --- |
| 1 | RS485-A | biały | K48.1-P35.1 |  |
| 2 | RS485-B | czarny | K48.2-P35.2 |  |
| 3 | Sygnał dla przekaźnika 5 | brązowy | J24.3-P35.4 |  |
| 4 | Sygnał dla przekaźnika 6 | fioletowy | J24.4-P35.3 |  |
| 5 | Podświetlenie klawisza | czerwony | N37.8-P35.5 |  |
| 6 | Klawisz 1 | zielony | N37.6-P35.6 |  |
| 7 | Klawisz 2 | niebieski | N37.7-P35.7 |  |
| 8 | zapalone światło | żółty | F49.11-P35.8 | WY_SWIATLO |

## Płytka B, ekspander MCP i RS485

Płytka B zawiera CPU, OPTO IN, OPTO OUT, ekspander IO 16 bit (porty A0–A7 i B0–B7), moduł RS485 oraz złącza A49 i B48.

### A49 – wejścia z płytki A (12 pin)

| Pin | Sygnał | Kolor żyły | Wejście CPU / MCP | Przewód |  |
| --- | --- | --- | --- | --- | --- |
| 1 | GND | czarny | – | A49.1-F49.12 | GND |
| 2 | +5 V | czerwony | – | A49.2-F49.5 | 5V |
| 3 | Sygnał noc | biały | B.4 | A49.3-F49.7 | MCP_B4 |
| 4 | Sygnał wieczór | żółty | A.6 | A49.4-F49.6 | MCP_A6 |
| 5 | Sygnał 2. taryfa | brązowy | A.5 | A49.5-F49.9 | MCP_A5 |
| 6 | Sygnał zasilania awaryjnego | fioletowy | A.4 | A49.6-F49.8 | MCP_A4 |
| 7 | Sygnał klawisza WC (wyłączenie) | pomarańczowy | A.3 | A49.7-F49.4 | MCP_A3 |
| 8 | Sygnał kontaktronu od drzwi | szary | A.2 | A49.8-F49.3 | MCP_A2 |
| 9 | Sygnał czujnika zbliżeniowego | ciemnobrązowy | A.1 | A49.9-F49.2 | MCP_A1 |
| 10 | Sygnał czujnika ruchu | jasnozielony | A.0 | A49.10-F49.1 | MCP_A0 |
| 11 | GND | niebieski | – | A49.11-F49.10 | GND |
| 12 | Oświetlenie – wyjście (zapalone światło) | ciemnozielony | CPU.A3 | A49.12-F49.11 | CPU_A3 |

### B48 – magistrala, czujniki i przekaźniki (12 pin)

| Pin | Sygnał | Kolor żyły | Wyprowadzenie | Przewód | Połączenie |
| --- | --- | --- | --- | --- | --- |
| 1 | RS-485-A | brązowy | RS485 A | B48.1-K48.1 | RS_485_A |
| 2 | RS-485-B | czarny | RS485 B | B48.2-K48.2 | RS_485_B |
| 3 | PIN 1 (gabaryt) | jasnozielony | – | B48.3-K48.6 | - |
| 4 | PIN 2 (gabaryt) | czerwony | – | B48.4-K48.7 | - |
| 5 | Sygnał czujnika temperatury | szary | CPU.4 | B48.5-K48.5 | CPU_4 |
| 6 | Wyjście – kontaktron (D36.4) | pomarańczowy | B.6 | B48.6-K48.3 | MCP_B6 |
| 7 | Wyjście – czujnik ruchu (D36.1) | zielony | B.7 | B48.7-K48.4 | MCP_B7 |
| 8 | PIN 3 (gabaryt) | ciemnobrązowy | – | B48.8-K48.8 | - |
| 9 | Przekaźnik 4 (zawór) | fioletowy | B.3 | B48.9-K48.9 | MCP_B3 |
| 10 | Przekaźnik 3 (lustro) | niebieski | B.2 | B48.10-K48.10 | MCP_B2 |
| 11 | Przekaźnik 2 (światło) | biały | B.1 | B48.11-K48.11 | MCP_B1 |
| 12 | Przekaźnik 1 (regał) | żółty | B.0 | B48.12-K48.12 | MCP_B0 |

### Ekspander MCP 16 bit

MCP komunikuje się z CPU przez I²C (SCL, SDA, linie INTA/INTB, RESET z rezystorem 10 kΩ do VCC). Adres modułu to 0

| Port | Kierunek | Przypisanie | Złączka |
| --- | --- | --- | --- |
| A0 | wejście | Czujnik ruchu |  |
| A1 | wejście | Czujnik zbliżeniowy |  |
| A2 | wejście | Kontaktron (drzwi) |  |
| A3 | wejście | Klawisz WC (wyłączenie) |  |
| A4 | wejście | Zasilanie awaryjne |  |
| A5 | wejście | 2. taryfa |  |
| A6 | wejście | Wieczór |  |
| A7 | - |  |  |
| B0 | wyjście | Przekaźnik 1 (regał) |  |
| B1 | wyjście | Przekaźnik 2 (światło) |  |
| B2 | wyjście | Przekaźnik 3 (lustro) |  |
| B3 | wyjście | Przekaźnik 4 (zawór) |  |
| B4 | wejście | Noc |  |
| B6 | wyjście | WY_KONTAKTRON (do K48.3) |  |
| B7 | wyjście | WY_CZUJNIK_RUCHU (do K48.4) |  |

Na schemacie MCP widoczne są rezystory 4,7 kΩ (podciągnięcie do VCC) i 10 kΩ (RESET, tor wejść A0–A3);

### Moduł RS485

Konwerter TTL↔RS485 ma wyprowadzenia VCC, TXD, RXD, GND po stronie CPU oraz A+, B−, GND po stronie magistrali; linia A trafia na K48.1 / B48.1, linia B na K48.2 / B48.2.

## Baza kabli – kolory żył, opisy i sposób połączenia

Źródło: eksport bazy kabli z serwera 192.168.10.254/kable (218 przewodów; szczegóły żył dla kabli płytki A i B: 20, 24–28, 32–37, 39, 40, 48, 49). Numer kabla w bazie = liczba w nazwie złącza (A34, B26, …). Polskie znaki w eksporcie były uszkodzone – nazwy odtworzono z kontekstu. Kolory poniżej to kolory żył przy wtyczce na płytce WC.

### Zestawienie kabli

| Kabel | Złącze WC | Żył | Opis w bazie | Drugi koniec |
| --- | --- | --- | --- | --- |
| 20 | O20 | 8 | czujniki poziomu wody w gabarycie, przycisk wiatraka, temperatura WC | połączenie kabli 20‑21‑22 (WC gabaryt) |
| 24 | J24 | 8 | sterowanie oświetleniem | sufit WC, wtyczka A (moduł 8 przekaźników) |
| 25 | M25 | 8 | dodatkowa funkcjonalność pod lustrem | lustro WC, brak zakończenia (FUTURE) |
| 26 | B26 | 4 | czujnik ruchu nad drzwiami | czujnik ruchu WC |
| 27 | C27 | 2 | kontaktron drzwi wejściowych | kontaktron WC |
| 28 | G28 | 4 | czujnik ruchu (zbliżeniowy) pod lustrem | czujnik zbliżeniowy, lustro WC |
| 32 | I32 | 3 | czujnik temperatury w podłodze WC | czujnik w podłodze WC |
| 33 | E33 | 3 | czujnik temperatury w podłodze Holu | czujnik w podłodze Holu |
| 34 | A34 | 8 | łącznik centrali WC z główną centralą, kabel A (zasilanie) | główna centrala (przedpokój), płytka A (Rozdzielcza 1), wtyczka O |
| 35 | P35 | 8 | łącznik, kabel B (inne) | główna centrala, płytka A, wtyczka L |
| 36 | D36 | 8 | łącznik, kabel C (ogrzewanie) | główna centrala, płytka B (Rozdzielcza 2), wtyczka J |
| 37 | N37 | 8 | Hol, czujnik temperatury, włącznik dwukanałowy; „przerwany niebieski i czarny” | włącznik hol/WC, połączenie 37‑172‑173 |
| 39 | H39 | 4 | przekaźniki ogrzewania w WC | 2× przekaźnik (WC) |
| 40 | L40 | 3 | włącznik światła WC | pojedynczy klawisz |
| 48 | K48 | 12 | do mikroprocesora, kabel A (prawa strona) | płytka B, B48 |
| 49 | F49 | 12 | do mikroprocesora, kabel B (lewa strona) | płytka B, A49 |

### Żyły kabli (kolor i przeznaczenie wg bazy)



| Pin | Kolor | Przeznaczenie |
| --- | --- | --- |
| 1 | pomarańczowy | +5 V zasilanie czujnika temperatury |
| 2 | niebieski | sygnał czujnika temperatury |
| 3 | żółty | – (nieużywany) |
| 4 | biały | GND czujnika temperatury |
| 5 | czarny | GND gabarytu |
| 6 | brązowy | PIN 1 gabarytu |
| 7 | zielony | PIN 2 gabarytu |
| 8 | czerwony | PIN 3 gabarytu |

– po stronie WC kolory jak w tabeli J24; na module przekaźników (Sufit WC, wtyczka A) kolejność pinów jest inna:

| Pin modułu | Kolor | Funkcja |
| --- | --- | --- |
| 1 | czerwony | +5 V |
| 2 | niebieski | przekaźnik 3 (lustro) |
| 3 | żółty | przekaźnik 1 (regał) |
| 4 | biały | przekaźnik 2 (światło) |
| 5 | fioletowy | przekaźnik 4 (zawór) |
| 6 | zielony | przekaźnik 5 |
| 7 | brązowy | przekaźnik 6 |
| 8 | czarny | GND |

– żyły: 1 brązowy, 2 czarny, 3 zielony, 4 czerwony, 5 fioletowy, 6 niebieski, 7 biały, 8 żółty; bez przypisanych funkcji, drugi koniec niepodłączony (FUTURE).



| Złącze | Pin | Kolor | Przeznaczenie |
| --- | --- | --- | --- |
| B26 | 1 | niebieski | sygnał z czujnika ruchu |
| B26 | 2 | pomarańczowy | +5 V czujnika ruchu |
| B26 | 3, 4 | biały | GND czujnika ruchu |
| C27 | 1 | niebieski | sygnał z kontaktronu |
| C27 | 2 | biały | GND |
| G28 | 1 | niebieski | sygnał z czujnika zbliżeniowego |
| G28 | 2 | pomarańczowy | +5 V czujnika zbliżeniowego |
| G28 | 3, 4 | biały | GND czujnika zbliżeniowego |

Żyły samych czujników: ruch – pomarańczowy zasilanie, niebieski sygnał, biały GND; kontaktron – biały GND, niebieski styk; zbliżeniowy – czarny sygnał, niebieski GND, brązowy +5 V (w kablu odpowiednio: niebieski / biały / pomarańczowy).



| Złącze | Pin | Kolor | Przeznaczenie |
| --- | --- | --- | --- |
| I32 (podłoga WC) | 1 | czarny | GND |
| I32 | 2 | żółty | sygnał temperatury podłogi WC |
| I32 | 3 | czerwony | +5 V |
| E33 (podłoga Hol) | 1 | czerwony | +5 V |
| E33 | 2 | żółty | sygnał temperatury podłogi Hol |
| E33 | 3 | czarny | GND |

– kolejność żył we wtyczce A po stronie WC wg bazy:

| Pin | Kolor wg bazy | Funkcja |
| --- | --- | --- |
| 1 | zielony | +24 V_ON |
| 2 | niebieski | GND |
| 3 | żółty | sygnał wieczór |
| 4 | biały | sygnał noc |
| 5 | brązowy | sygnał 2. taryfa |
| 6 | fioletowy | sygnał zasilanie awaryjne |
| 7 | czerwony | +24 V_OFF |
| 8 | czarny | GND |

Po stronie głównej centrali (płytka A, wtyczka O) kolejność jest inna: 1 czerwony, 2 czarny, 3 zielony, 4 niebieski, 5 brązowy, 6 fioletowy, 7 biały, 8 żółty.

– żyły kabla: żółty – zapalone światło w WC (główne lub lustro), biały – RS485‑A, brązowy – sygnał przekaźnika 5, fioletowy – sygnał przekaźnika 6, czerwony – podświetlenie włączników, zielony – klawisz 1, niebieski – klawisz 2, czarny – RS485‑B. Układ pinów wtyczki P35 wg bazy:

| Pin | Kolor | Funkcja |
| --- | --- | --- |
| 1 | fioletowy | przekaźnik 6 |
| 2 | brązowy | przekaźnik 5 |
| 3 | żółty | zapalone światło (wyjście) |
| 4 | biały | RS485‑A |
| 5 | czerwony | podświetlenie włączników |
| 6 | zielony | klawisz 1 |
| 7 | niebieski | klawisz 2 |
| 8 | czarny | RS485‑B |

– rozwiązuje wątpliwe odczyty z notatek:

| Pin | Kolor | Przeznaczenie | Powiązanie |
| --- | --- | --- | --- |
| 1 | czerwony | sygnał czujnika ruchu | K48.4 / B48 |
| 2 | zielony | przekaźnik ogrzewania Hol | H39 |
| 3 | żółty | przekaźnik ogrzewania WC | H39 |
| 4 | czarny | sygnał kontaktronu | K48.3 / B48.6 |
| 5 | brązowy | sygnał czujnika temperatury WC | – |
| 6 | fioletowy | sygnał czujnika temperatury Hol | – |
| 7 | niebieski | sygnał czujnika temperatury podłogi WC | – |
| 8 | biały | sygnał czujnika temperatury podłogi Hol | – |

– uwaga: w bazie opis „przerwany niebieski i czarny” (żyły pin 4 i 5 uszkodzone). Masa od Włącznika

| Pin | Kolor | Przeznaczenie | Koniec (włącznik hol/WC) |
| --- | --- | --- | --- |
| 1 | czerwony | +5 V czujnika temperatury | 172.+5V |
| 2 | brązowy | +5 V | 173 +5 V |
| 3 | zielony | sygnał czujnika temperatury | 172.Sig |
| 4 | czarny | GND czujnika temperatury | 172.GND |
| 5 | niebieski | GND | 173.GND |
| 6 | biały | klawisz włącznika 1 | 173.1 |
| 7 | żółty | klawisz włącznika 2 | 173.2 |
| 8 | fioletowy | podświetlenie włączników (żyła pomarańczowa) | 173.pod |



| Złącze | Pin | Kolor | Przeznaczenie |
| --- | --- | --- | --- |
| H39 | 1 | czarny | GND |
| H39 | 2 | żółty | przekaźnik WC |
| H39 | 3 | zielony | przekaźnik Hol |
| H39 | 4 | czerwony | +5 V |
| L40 | 1 | pomarańczowy | podświetlenie włącznika |
| L40 | 2 | niebieski | GND |
| L40 | 3 | biały | klawisz (styk) |

– kolor żyły, jej funkcja i pin na obu końcach:

| Kolor | Funkcja | Pin B48 (płytka B) | Pin K48 (płytka A) |
| --- | --- | --- | --- |
| brązowy | RS485‑A | 8 | 1 |
| zielony | RS485‑B | 7 | 2 |
| pomarańczowy | wyjście kontaktronu | 6 | 3 |
| szary | wyjście czujnika ruchu | 5 | 4 |
| jasnobrązowy | sygnał lokalnego czujnika temperatury | 1 | 5 |
| czarny | PIN 1 gabarytu | 2 | 6 |
| jasnozielony | PIN 2 gabarytu | 3 | 7 |
| czerwony | PIN 3 gabarytu | 4 | 8 |
| żółty | przekaźnik 1 (regał) | 12 | 9 |
| biały | przekaźnik 2 (światło) | 11 | 10 |
| niebieski | przekaźnik 3 (lustro) | 10 | 11 |
| fioletowy | przekaźnik 4 (zawór) | 9 | 12 |



| Kolor | Funkcja | Pin A49 (płytka B) | Pin F49 (płytka A) |
| --- | --- | --- | --- |
| czarny | GND | 1 | 12 |
| czerwony | +5 V | 2 | 5 |
| biały | sygnał noc | 3 | 7 |
| żółty | sygnał wieczór | 4 | 6 |
| brązowy | 2. taryfa | 5 | 9 |
| fioletowy | zasilanie awaryjne | 6 | 8 |
| pomarańczowy | klawisz włącznika światła | 7 | 4 |
| szary | kontaktron | 8 | 3 |
| jasnobrązowy | czujnik zbliżeniowy | 9 | 2 |
| jasnozielony | czujnik ruchu | 10 | 1 |
| niebieski | GND | 11 | 10 |
| zielony | +24 V_OFF / wyjście oświetlenia | 12 | 11 |

### Różnice między notatkami a bazą kabli

| Miejsce | Notatki | Baza kabli |
| --- | --- | --- |
| A34 pin 3, 4, 7, 8 | 3 czerwony (24_OFF), 4 czarny, 7 biały (noc), 8 żółty (wieczór) | 3 żółty, 4 biały, 7 czerwony, 8 czarny; pin 6 fioletowy (w notatkach „szary”) – możliwe, że baza nie uwzględnia zmiany „czerwony ↔ zielony” |
| P35 pin 1–4, 8 | 1 RS485‑A, 2 RS485‑B, 3 przek. 5, 4 przek. 6, 8 GND | 1 przek. 6, 2 przek. 5, 3 zapalone światło, 4 RS485‑A, 8 RS485‑B (piny 5–7 zgodne) |
| H39 pin 2 / 3 | 2 Hol (żółty), 3 WC (zielony) | 2 WC (żółty), 3 Hol (zielony) |
| K48 pin 9–12 | 9 = przek. 4 … 12 = przek. 1 | 9 = przek. 1 (żółty) … 12 = przek. 4 (fioletowy) – kolory zgodne z B48 |
| B48 | 1 RS485‑A, 2 RS485‑B, 3–4 PIN1–2, 5 temp., 7 ruch, 8 PIN3 | żyły: 1 temp., 2–4 PIN1–3, 5 ruch, 7 RS485‑B, 8 RS485‑A; kolory pinów 6, 9–12 zgodne |
| F49 / A49 | brązowy i ciemnobrązowy | „jasnobrązowy” (zbliżeniowy) i „brązowy” (2. taryfa); pin 12 „zielony” (nie ciemnozielony) |
| Mapowanie wejść MCP | noc = B.4 | baza opisuje noc jako wejście 0x00.8, wieczór 0x00.7, ruch 0x00.1 … (numeracja od 1) |

Przekaźniki w bazie są opisane jako adres MCP 0x01: zawór 0x01.1, lustro 0x01.2, światło 0x01.3, regał 0x01.4.

## Zmiany, poprawki i punkty do potwierdzenia

Notatki zawierają trzy zmiany projektowe i kilka miejsc, które wymagają sprawdzenia w instalacji.



Zmiana pinów w wtyczce: czerwony ↔ zielony.Przekierowanie K39 na nowe miejsce płytki (np. B26, G28) z zasilaniem +5 V i GND; w wtyczce 36 zmieniamy +5 V i GND.Zmiana dla P35 i N34.Po poprawie dłuższy przewód: zamiana przewodów +5 V i GND (kolory według notatki: czerwony ↔ czarny).



Przewód 23 jest wpisany jako PE (grupa H), a w łączeniach występuje jako L dla zasilacza (23–27).Przewody 11 i 02–11 oznaczone w notatce znakiem „?”.Grupa I (przewody 26–27): nazwa odbiornika jest nieczytelna (w bazie kabli numery 26 i 27 oznaczają kable czujnika ruchu i kontaktronu, nie przewody w puszce – to inna numeracja).Kolory żył w B26, G28, C27, N37, O20 i B48 – uzupełnione z bazy kabli (sekcja „Baza kabli”); do sprawdzenia pozostają różnice w układzie pinów B48, K48 (9–12), A34, P35 i H39 wypisane w tabeli „Różnice między notatkami a bazą kabli”.Pin 3 w A34 (+24 V_OFF) jest przekreślony jako „brak sygnału”.Wartość rezystora podciągającego (4,7 kΩ lub 470 kΩ) przy sygnale czujnika ruchu.Przypisanie pinów D36 – wyjaśnione na podstawie bazy kabli (1 ruch, 2 przekaźnik Hol, 3 przekaźnik WC, 4 kontaktron, 5–8 czujniki temperatury); warto potwierdzić w instalacji.
