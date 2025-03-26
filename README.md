
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#3 - instructions

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

# Lab 3: Avenger’s Peril

## As a team

Extract from the data below two data sets in long form `deaths` and
`returns`

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

``` r
library(tidyverse)
```

    ## Warning: package 'ggplot2' was built under R version 4.4.2

    ## Warning: package 'forcats' was built under R version 4.4.2

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
deaths <- av %>% 
  pivot_longer(
    cols = starts_with("Death"), 
    names_to = "Time", 
    values_to = "Died") %>% 
  select(URL, Name.Alias, Time, Died) %>% 
  mutate(Time = parse_number(Time))
```

Similarly, deal with the returns of characters.

``` r
returns <- av %>% 
  pivot_longer(
    cols = starts_with("Return"), 
    names_to = "Time", 
    values_to = "Returned") %>% 
  select(URL, Name.Alias, Time, Returned) %>% 
  mutate(Time = parse_number(Time)) 
```

Based on these datasets calculate the average number of deaths an
Avenger suffers.

The average number of deaths an Avenger suffers is 1.121387.

``` r
deaths %>% group_by(URL) %>% filter(Died !="") %>% summarise(totaldeaths = n()) %>% summarise(totalavgdeaths = mean(totaldeaths, na.rm = TRUE))
```

    ## # A tibble: 1 × 1
    ##   totalavgdeaths
    ##            <dbl>
    ## 1           1.12

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

## Gwen’s Work:

### FiveThirtyEight Statement

Quote the statement you are planning to fact-check.

“What’s more, if you’re a fan of the MCU, nobody is safe. Of the nine
Avengers we see on screen — Iron Man, Hulk, Captain America, Thor,
Hawkeye, Black Widow, Scarlet Witch, Quicksilver and The Vision — every
single one of them has died at least once in the course of their time
Avenging in the comics. In fact, Hawkeye died twice!”

### Include your answer

``` r
#View(av)

print.data.frame(deaths %>% group_by(URL) %>% filter(Died !="") %>% summarise(totaldeaths = n()))
```

    ##                                                                     URL
    ## 1                     http://marvel.wikia.com/2ZP45-9-X-51_(Earth-616)#
    ## 2            http://marvel.wikia.com/Abyss_(Ex_Nihilo%27s)_(Earth-616)#
    ## 3                    http://marvel.wikia.com/Adam_Brashear_(Earth-616)#
    ## 4                       http://marvel.wikia.com/Alani_Ryan_(Earth-616)#
    ## 5                http://marvel.wikia.com/Alexander_Summers_(Earth-616)#
    ## 6                           http://marvel.wikia.com/Alexis_(Earth-616)#
    ## 7                      http://marvel.wikia.com/Amadeus_Cho_(Earth-616)#
    ## 8                   http://marvel.wikia.com/America_Chavez_(Earth-616)#
    ## 9                   http://marvel.wikia.com/Angelica_Jones_(Earth-616)#
    ## 10                   http://marvel.wikia.com/Anthony_Druid_(Earth-616)#
    ## 11                    http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 12                 http://marvel.wikia.com/Anthony_Stark_(Earth-96020)#
    ## 13                    http://marvel.wikia.com/Anya_Corazon_(Earth-616)#
    ## 14                            http://marvel.wikia.com/Ares_(Earth-616)#
    ## 15                 http://marvel.wikia.com/Ashley_Crawford_(Earth-616)#
    ## 16                       http://marvel.wikia.com/Ava_Ayala_(Earth-616)#
    ## 17                   http://marvel.wikia.com/Barbara_Morse_(Earth-616)#
    ## 18                  http://marvel.wikia.com/Benjamin_Grimm_(Earth-616)#
    ## 19                   http://marvel.wikia.com/Bonita_Juarez_(Earth-616)#
    ## 20                  http://marvel.wikia.com/Brandon_Sharpe_(Earth-616)#
    ## 21                  http://marvel.wikia.com/Brian_Braddock_(Earth-616)#
    ## 22                      http://marvel.wikia.com/Brunnhilde_(Earth-616)#
    ## 23                http://marvel.wikia.com/Captain_Universe_(Earth-616)#
    ## 24                   http://marvel.wikia.com/Carol_Danvers_(Earth-616)#
    ## 25                  http://marvel.wikia.com/Cassandra_Lang_(Earth-616)#
    ## 26                      http://marvel.wikia.com/Charlie-27_(Earth-691)#
    ## 27                    http://marvel.wikia.com/Chris_Powell_(Earth-616)#
    ## 28                     http://marvel.wikia.com/Clint_Barton_(Earth-616)
    ## 29                    http://marvel.wikia.com/Craig_Hollis_(Earth-616)#
    ## 30             http://marvel.wikia.com/Crystalia_Amaquelin_(Earth-616)#
    ## 31                   http://marvel.wikia.com/Daisy_Johnson_(Earth-616)#
    ## 32                                 http://marvel.wikia.com/Dane_Whitman
    ## 33                     http://marvel.wikia.com/Daniel_Rand_(Earth-616)#
    ## 34                   http://marvel.wikia.com/David_Alleyne_(Earth-616)#
    ## 35                    http://marvel.wikia.com/DeMarr_Davis_(Earth-616)#
    ## 36                        http://marvel.wikia.com/Deathcry_(Earth-616)#
    ## 37              http://marvel.wikia.com/Delroy_Garrett_Jr._(Earth-616)#
    ## 38                   http://marvel.wikia.com/Dennis_Dunphy_(Earth-616)#
    ## 39                    http://marvel.wikia.com/Dennis_Sykes_(Earth-616)#
    ## 40                      http://marvel.wikia.com/Dinah_Soar_(Earth-616)#
    ## 41               http://marvel.wikia.com/Doombot_(Avenger)_(Earth-616)#
    ## 42                    http://marvel.wikia.com/Doreen_Green_(Earth-616)#
    ## 43                     http://marvel.wikia.com/Dorrek_VIII_(Earth-616)#
    ## 44                    http://marvel.wikia.com/Doug_Taggert_(Earth-616)#
    ## 45                       http://marvel.wikia.com/Eden_Fesi_(Earth-616)#
    ## 46                  http://marvel.wikia.com/Elijah_Bradley_(Earth-616)#
    ## 47                   http://marvel.wikia.com/Elvin_Haliday_(Earth-616)#
    ## 48                    http://marvel.wikia.com/Emery_Schaub_(Earth-616)#
    ## 49                     http://marvel.wikia.com/Eric_Brooks_(Earth-616)#
    ## 50                  http://marvel.wikia.com/Eric_Masterson_(Earth-616)#
    ## 51                  http://marvel.wikia.com/Eric_O%27Grady_(Earth-616)#
    ## 52                            http://marvel.wikia.com/Eros_(Earth-616)#
    ## 53                 http://marvel.wikia.com/Eugene_Thompson_(Earth-616)#
    ## 54                       http://marvel.wikia.com/Ex_Nihilo_(Earth-616)#
    ## 55                 http://marvel.wikia.com/Fiona_(Inhuman)_(Earth-616)#
    ## 56                    http://marvel.wikia.com/Gene_Lorrene_(Earth-616)#
    ## 57                       http://marvel.wikia.com/Gilgamesh_(Earth-616)#
    ## 58       http://marvel.wikia.com/Giuletta_Nefaria_(Masque)_(Earth-616)#
    ## 59                    http://marvel.wikia.com/Greer_Nelson_(Earth-616)#
    ## 60                     http://marvel.wikia.com/Greg_Willis_(Earth-616)#
    ## 61                 http://marvel.wikia.com/Heather_Douglas_(Earth-616)#
    ## 62                     http://marvel.wikia.com/Henry_McCoy_(Earth-616)#
    ## 63                        http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 64                         http://marvel.wikia.com/Hercules_(Earth-616)
    ## 65                          http://marvel.wikia.com/Hollow_(Earth-616)#
    ## 66           http://marvel.wikia.com/Human_Torch_(Android)_(Earth-616)#
    ## 67                  http://marvel.wikia.com/Humberto_Lopez_(Earth-616)#
    ## 68                     http://marvel.wikia.com/Isabel_Kane_(Earth-616)#
    ## 69                 http://marvel.wikia.com/Jacques_Duquesne_(Earth-616)
    ## 70           http://marvel.wikia.com/James_Buchanan_Barnes_(Earth-616)#
    ## 71                   http://marvel.wikia.com/James_Howlett_(Earth-616)#
    ## 72                    http://marvel.wikia.com/James_Rhodes_(Earth-616)#
    ## 73                   http://marvel.wikia.com/James_Santini_(Earth-616)#
    ## 74                   http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 75                 http://marvel.wikia.com/Jeanne_Foucault_(Earth-616)#
    ## 76                 http://marvel.wikia.com/Jennifer_Takeda_(Earth-616)#
    ## 77                http://marvel.wikia.com/Jennifer_Walters_(Earth-616)#
    ## 78                    http://marvel.wikia.com/Jessica_Drew_(Earth-616)#
    ## 79                   http://marvel.wikia.com/Jessica_Jones_(Earth-616)#
    ## 80                         http://marvel.wikia.com/Jocasta_(Earth-616)#
    ## 81                       http://marvel.wikia.com/John_Aman_(Earth-616)#
    ## 82                     http://marvel.wikia.com/John_Walker_(Earth-616)#
    ## 83                 http://marvel.wikia.com/Johnathon_Gallo_(Earth-616)#
    ## 84                   http://marvel.wikia.com/Jonathan_Hart_(Earth-616)#
    ## 85                 http://marvel.wikia.com/Julia_Carpenter_(Earth-616)#
    ## 86                     http://marvel.wikia.com/Julie_Power_(Earth-616)#
    ## 87                           http://marvel.wikia.com/Kaluu_(Earth-616)#
    ## 88                http://marvel.wikia.com/Katherine_Bishop_(Earth-616)#
    ## 89                    http://marvel.wikia.com/Kelsey_Leigh_(Earth-616)#
    ## 90                        http://marvel.wikia.com/Ken_Mack_(Earth-616)#
    ## 91                    http://marvel.wikia.com/Kevin_Connor_(Earth-616)#
    ## 92                 http://marvel.wikia.com/Kevin_Masterson_(Earth-616)#
    ## 93           http://marvel.wikia.com/Loki_Laufeyson_(Ikol)_(Earth-616)#
    ## 94                       http://marvel.wikia.com/Luke_Cage_(Earth-616)#
    ## 95                           http://marvel.wikia.com/Lyra_(Earth-8009)#
    ## 96                          http://marvel.wikia.com/Mantis_(Earth-616)#
    ## 97                        http://marvel.wikia.com/Mar-Vell_(Earth-616)#
    ## 98                    http://marvel.wikia.com/Marc_Spector_(Earth-616)#
    ## 99                 http://marvel.wikia.com/Marcus_Milton_(Earth-13034)#
    ## 100                     http://marvel.wikia.com/Maria_Hill_(Earth-616)#
    ## 101    http://marvel.wikia.com/Maria_de_Guadalupe_Santiago_(Earth-616)#
    ## 102              http://marvel.wikia.com/Marrina_Smallwood_(Earth-616)#
    ## 103              http://marvel.wikia.com/Martinex_T%27Naga_(Earth-691)#
    ## 104                   http://marvel.wikia.com/Matthew_Hawk_(Earth-616)#
    ## 105                http://marvel.wikia.com/Matthew_Murdock_(Earth-616)#
    ## 106                     http://marvel.wikia.com/Maya_Lopez_(Earth-616)#
    ## 107                http://marvel.wikia.com/Melissa_Darrow_(Earth-9201)#
    ## 108                http://marvel.wikia.com/Michiko_Musashi_(Earth-616)#
    ## 109                  http://marvel.wikia.com/Miguel_Santos_(Earth-616)#
    ## 110                  http://marvel.wikia.com/Moira_Brandon_(Earth-616)#
    ## 111                   http://marvel.wikia.com/Monica_Chang_(Earth-616)#
    ## 112                 http://marvel.wikia.com/Monica_Rambeau_(Earth-616)#
    ## 113                     http://marvel.wikia.com/Monkey_Joe_(Earth-616)#
    ## 114                 http://marvel.wikia.com/Namor_McKenzie_(Earth-616)#
    ## 115               http://marvel.wikia.com/Natalia_Romanova_(Earth-616)#
    ## 116 http://marvel.wikia.com/Nathaniel_Richards_(Iron_Lad)_(Earth-6311)#
    ## 117             http://marvel.wikia.com/Nicholas_Fury,_Jr._(Earth-616)#
    ## 118                http://marvel.wikia.com/Nicholette_Gold_(Earth-691)#
    ## 119                      http://marvel.wikia.com/Nightmask_(Earth-616)#
    ## 120                       http://marvel.wikia.com/Noh-Varr_(Earth-616)#
    ## 121                   http://marvel.wikia.com/Ororo_Munroe_(Earth-616)#
    ## 122                  http://marvel.wikia.com/Otto_Octavius_(Earth-616)#
    ## 123                http://marvel.wikia.com/Patricia_Walker_(Earth-616)#
    ## 124                   http://marvel.wikia.com/Peter_Parker_(Earth-616)#
    ## 125                  http://marvel.wikia.com/Philip_Javert_(Earth-921)#
    ## 126                http://marvel.wikia.com/Phillip_Coulson_(Earth-616)#
    ## 127                 http://marvel.wikia.com/Pietro_Maximoff_(Earth-616)
    ## 128             http://marvel.wikia.com/Ravonna_Renslayer_(Earth-6311)#
    ## 129                  http://marvel.wikia.com/Reed_Richards_(Earth-616)#
    ## 130                   http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ## 131                  http://marvel.wikia.com/Richard_Rider_(Earth-616)#
    ## 132                    http://marvel.wikia.com/Rita_DeMara_(Earth-616)#
    ## 133                 http://marvel.wikia.com/Robert_Baldwin_(Earth-616)#
    ## 134             http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 135                   http://marvel.wikia.com/Robert_Frank_(Earth-616)#
    ## 136                http://marvel.wikia.com/Robert_Reynolds_(Earth-616)#
    ## 137               http://marvel.wikia.com/Roberto_da_Costa_(Earth-616)#
    ## 138             http://marvel.wikia.com/Rogue_(Anna_Marie)_(Earth-616)#
    ## 139                  http://marvel.wikia.com/Sam_Alexander_(Earth-616)#
    ## 140                 http://marvel.wikia.com/Samuel_Guthrie_(Earth-616)#
    ## 141                  http://marvel.wikia.com/Samuel_Wilson_(Earth-616)#
    ## 142                     http://marvel.wikia.com/Scott_Lang_(Earth-616)#
    ## 143                          http://marvel.wikia.com/Sersi_(Earth-616)#
    ## 144                      http://marvel.wikia.com/Shang-Chi_(Earth-616)#
    ## 145                  http://marvel.wikia.com/Sharon_Carter_(Earth-616)#
    ## 146                  http://marvel.wikia.com/Shiro_Yoshida_(Earth-616)#
    ## 147                 http://marvel.wikia.com/Simon_Williams_(Earth-616)#
    ## 148                   http://marvel.wikia.com/Stakar_Ogord_(Earth-691)#
    ## 149                http://marvel.wikia.com/Stephen_Strange_(Earth-616)#
    ## 150                   http://marvel.wikia.com/Steven_Rogers_(Earth-616)
    ## 151                    http://marvel.wikia.com/Susan_Storm_(Earth-616)#
    ## 152                      http://marvel.wikia.com/T%27Challa_(Earth-616)
    ## 153                http://marvel.wikia.com/Takashi_Matsuya_(Earth-616)#
    ## 154                  http://marvel.wikia.com/Thaddeus_Ross_(Earth-616)#
    ## 155                http://marvel.wikia.com/Thomas_Shepherd_(Earth-616)#
    ## 156                    http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 157                      http://marvel.wikia.com/Tippy-Toe_(Earth-616)#
    ## 158                   http://marvel.wikia.com/Tony_Masters_(Earth-616)#
    ## 159                    http://marvel.wikia.com/Val_Ventura_(Earth-616)#
    ## 160                    http://marvel.wikia.com/Vance_Astro_(Earth-691)#
    ## 161                 http://marvel.wikia.com/Vance_Astrovik_(Earth-616)#
    ## 162                        http://marvel.wikia.com/Veranke_(Earth-616)#
    ## 163                 http://marvel.wikia.com/Victor_Alvarez_(Earth-616)#
    ## 164                  http://marvel.wikia.com/Victor_Mancha_(Earth-616)#
    ## 165                          http://marvel.wikia.com/Vision_(Earth-616)
    ## 166                 http://marvel.wikia.com/Vision_(Jonas)_(Earth-616)#
    ## 167                    http://marvel.wikia.com/Wade_Wilson_(Earth-616)#
    ## 168                  http://marvel.wikia.com/Walter_Newell_(Earth-616)#
    ## 169                  http://marvel.wikia.com/Wanda_Maximoff_(Earth-616)
    ## 170                 http://marvel.wikia.com/Wendell_Vaughn_(Earth-616)#
    ## 171                  http://marvel.wikia.com/William_Baker_(Earth-616)#
    ## 172                 http://marvel.wikia.com/William_Kaplan_(Earth-616)#
    ## 173                   http://marvel.wikia.com/Yondu_Udonta_(Earth-691)#
    ##     totaldeaths
    ## 1             1
    ## 2             1
    ## 3             1
    ## 4             1
    ## 5             1
    ## 6             1
    ## 7             1
    ## 8             1
    ## 9             1
    ## 10            2
    ## 11            1
    ## 12            1
    ## 13            1
    ## 14            2
    ## 15            1
    ## 16            1
    ## 17            1
    ## 18            1
    ## 19            1
    ## 20            1
    ## 21            1
    ## 22            1
    ## 23            1
    ## 24            1
    ## 25            1
    ## 26            1
    ## 27            1
    ## 28            2
    ## 29            1
    ## 30            1
    ## 31            1
    ## 32            1
    ## 33            1
    ## 34            1
    ## 35            1
    ## 36            2
    ## 37            1
    ## 38            2
    ## 39            1
    ## 40            1
    ## 41            1
    ## 42            1
    ## 43            1
    ## 44            1
    ## 45            1
    ## 46            1
    ## 47            1
    ## 48            1
    ## 49            1
    ## 50            2
    ## 51            1
    ## 52            1
    ## 53            1
    ## 54            1
    ## 55            1
    ## 56            1
    ## 57            1
    ## 58            1
    ## 59            1
    ## 60            1
    ## 61            2
    ## 62            1
    ## 63            1
    ## 64            1
    ## 65            1
    ## 66            1
    ## 67            1
    ## 68            1
    ## 69            1
    ## 70            1
    ## 71            1
    ## 72            1
    ## 73            1
    ## 74            1
    ## 75            1
    ## 76            1
    ## 77            1
    ## 78            1
    ## 79            1
    ## 80            5
    ## 81            1
    ## 82            1
    ## 83            1
    ## 84            2
    ## 85            1
    ## 86            1
    ## 87            1
    ## 88            1
    ## 89            1
    ## 90            1
    ## 91            1
    ## 92            1
    ## 93            1
    ## 94            1
    ## 95            1
    ## 96            1
    ## 97            3
    ## 98            1
    ## 99            1
    ## 100           1
    ## 101           1
    ## 102           2
    ## 103           1
    ## 104           2
    ## 105           1
    ## 106           2
    ## 107           1
    ## 108           1
    ## 109           1
    ## 110           1
    ## 111           1
    ## 112           1
    ## 113           1
    ## 114           1
    ## 115           1
    ## 116           1
    ## 117           1
    ## 118           1
    ## 119           1
    ## 120           1
    ## 121           1
    ## 122           1
    ## 123           1
    ## 124           2
    ## 125           1
    ## 126           1
    ## 127           1
    ## 128           2
    ## 129           1
    ## 130           1
    ## 131           1
    ## 132           1
    ## 133           1
    ## 134           1
    ## 135           1
    ## 136           1
    ## 137           1
    ## 138           1
    ## 139           1
    ## 140           1
    ## 141           1
    ## 142           1
    ## 143           1
    ## 144           1
    ## 145           1
    ## 146           1
    ## 147           1
    ## 148           1
    ## 149           1
    ## 150           1
    ## 151           1
    ## 152           1
    ## 153           1
    ## 154           1
    ## 155           1
    ## 156           2
    ## 157           1
    ## 158           1
    ## 159           1
    ## 160           1
    ## 161           1
    ## 162           1
    ## 163           1
    ## 164           1
    ## 165           1
    ## 166           1
    ## 167           1
    ## 168           1
    ## 169           1
    ## 170           1
    ## 171           2
    ## 172           1
    ## 173           1

- The Fact is correct! According to the code above, all the avengers
  died at least once and Hawkeye died 2 times.

## Kylie’s Work:

“Out of 173 listed Avengers, my analysis found that 69 had died at least
one time after they joined the team.”

``` r
deaths %>% group_by(URL) %>% filter(Died == "YES") %>% summarise(actualdeaths = n()) %>% summarise(numavengersdied = n())
```

    ## # A tibble: 1 × 1
    ##   numavengersdied
    ##             <int>
    ## 1              69

Make sure to include the code to derive the (numeric) fact for the
statement

I found that the number of avengers who actually died at least once was
69, which means that the statement given in the article is true.

## Devon’s Work:

### FiveThirtyEight Statement

> “I counted 89 total deaths — some unlucky Avengers7 are basically Meat
> Loaf with an E-ZPass — and on 57 occasions the individual made a
> comeback.”

``` r
deaths %>% group_by(URL) %>% filter(Died == "YES") %>% summarise(actualdeaths = n()) %>% summarise(numavengersdied = n())
```

    ## # A tibble: 1 × 1
    ##   numavengersdied
    ##             <int>
    ## 1              69

``` r
deaths %>% filter(Died == "YES") %>% summarise(numDeaths = n())
```

    ## # A tibble: 1 × 1
    ##   numDeaths
    ##       <int>
    ## 1        89

``` r
returns %>% filter(Returned == "YES") %>% summarise(numReturns = n())
```

    ## # A tibble: 1 × 1
    ##   numReturns
    ##        <int>
    ## 1         57

### Include your answer

This fact is correct there were 89 Deaths and occasions where the
avenger made a comeback.

Include at least one sentence discussing the result of your
fact-checking endeavor.

Upload your changes to the repository. Discuss and refine answers as a
team.
