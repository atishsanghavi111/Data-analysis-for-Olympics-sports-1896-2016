# Data-analysis-for-Olympics-sports-1896-2016


date: “2023-03-31” output: html_document —

Data Preparation
athletes <- read.csv("athlete_events.csv",header = T)
head(athletes)
##   ID                     Name Sex Age Height Weight           Team NOC
## 1  1                A Dijiang   M  24    180     80          China CHN
## 2  2                 A Lamusi   M  23    170     60          China CHN
## 3  3      Gunnar Nielsen Aaby   M  24     NA     NA        Denmark DEN
## 4  4     Edgar Lindenau Aabye   M  34     NA     NA Denmark/Sweden DEN
## 5  5 Christine Jacoba Aaftink   F  21    185     82    Netherlands NED
## 6  5 Christine Jacoba Aaftink   F  21    185     82    Netherlands NED
##         Games Year Season      City         Sport
## 1 1992 Summer 1992 Summer Barcelona    Basketball
## 2 2012 Summer 2012 Summer    London          Judo
## 3 1920 Summer 1920 Summer Antwerpen      Football
## 4 1900 Summer 1900 Summer     Paris    Tug-Of-War
## 5 1988 Winter 1988 Winter   Calgary Speed Skating
## 6 1988 Winter 1988 Winter   Calgary Speed Skating
##                                Event Medal
## 1        Basketball Men's Basketball  <NA>
## 2       Judo Men's Extra-Lightweight  <NA>
## 3            Football Men's Football  <NA>
## 4        Tug-Of-War Men's Tug-Of-War  Gold
## 5   Speed Skating Women's 500 metres  <NA>
## 6 Speed Skating Women's 1,000 metres  <NA>
After importing the “athletes” dataset we observe the first six rows using the head() function. The dataset contains 15 variables including ID, which is the unique identifier for each individual athlete. The Height, Weight, and Medal variable contains missing values represented by “NA”. For Height and Weight NA values, information was probably not recorded at the time of the event. Medal variable has missing values meaning not every athelete won medals (Gold, Silver, Bronze) who participated over the years in the Summer and Winter Olympic events.
Summary Statistics
summary(athletes)
##        ID             Name               Sex                 Age       
##  Min.   :     1   Length:271116      Length:271116      Min.   :10.00  
##  1st Qu.: 34643   Class :character   Class :character   1st Qu.:21.00  
##  Median : 68205   Mode  :character   Mode  :character   Median :24.00  
##  Mean   : 68249                                         Mean   :25.56  
##  3rd Qu.:102097                                         3rd Qu.:28.00  
##  Max.   :135571                                         Max.   :97.00  
##                                                         NA's   :9474   
##      Height          Weight          Team               NOC           
##  Min.   :127.0   Min.   : 25.0   Length:271116      Length:271116     
##  1st Qu.:168.0   1st Qu.: 60.0   Class :character   Class :character  
##  Median :175.0   Median : 70.0   Mode  :character   Mode  :character  
##  Mean   :175.3   Mean   : 70.7                                        
##  3rd Qu.:183.0   3rd Qu.: 79.0                                        
##  Max.   :226.0   Max.   :214.0                                        
##  NA's   :60171   NA's   :62875                                        
##     Games                Year         Season              City          
##  Length:271116      Min.   :1896   Length:271116      Length:271116     
##  Class :character   1st Qu.:1960   Class :character   Class :character  
##  Mode  :character   Median :1988   Mode  :character   Mode  :character  
##                     Mean   :1978                                        
##                     3rd Qu.:2002                                        
##                     Max.   :2016                                        
##                                                                         
##     Sport              Event              Medal          
##  Length:271116      Length:271116      Length:271116     
##  Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character  
##                                                          
##                                                          
##                                                          
## 
From the summary statistics we can observe there are several character variables like Name, Sex, Team, NOC, Games, Season, City, Sport, Event and Medal. The numeric variables are Age, Height, Weight, and Year. However, Year variable is not meaningful in terms of max, min, median, mean, etc. Which is why the Year variables was converted to factor variable to make more sense of different time periods in terms of years in the next step.
Conversion of categorical to factor values for Year variable
athletes$Year <- factor(athletes$Year)
summary(athletes)
##        ID             Name               Sex                 Age       
##  Min.   :     1   Length:271116      Length:271116      Min.   :10.00  
##  1st Qu.: 34643   Class :character   Class :character   1st Qu.:21.00  
##  Median : 68205   Mode  :character   Mode  :character   Median :24.00  
##  Mean   : 68249                                         Mean   :25.56  
##  3rd Qu.:102097                                         3rd Qu.:28.00  
##  Max.   :135571                                         Max.   :97.00  
##                                                         NA's   :9474   
##      Height          Weight          Team               NOC           
##  Min.   :127.0   Min.   : 25.0   Length:271116      Length:271116     
##  1st Qu.:168.0   1st Qu.: 60.0   Class :character   Class :character  
##  Median :175.0   Median : 70.0   Mode  :character   Mode  :character  
##  Mean   :175.3   Mean   : 70.7                                        
##  3rd Qu.:183.0   3rd Qu.: 79.0                                        
##  Max.   :226.0   Max.   :214.0                                        
##  NA's   :60171   NA's   :62875                                        
##     Games                Year           Season              City          
##  Length:271116      1992   : 16413   Length:271116      Length:271116     
##  Class :character   1988   : 14676   Class :character   Class :character  
##  Mode  :character   2000   : 13821   Mode  :character   Mode  :character  
##                     1996   : 13780                                        
##                     2016   : 13688                                        
##                     2008   : 13602                                        
##                     (Other):185136                                        
##     Sport              Event              Medal          
##  Length:271116      Length:271116      Length:271116     
##  Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character  
##                                                          
##                                                          
##                                                          
## 
After the conversion, it is very clear about the number of occurences of each years which represents the number of occurences of individual athletes taking part in various sporitng events.
Remove NA values in the Medal column
athletes_1 <- athletes[!is.na(athletes$Medal),]
View(athletes_1)
The above line of code got rid of all the rows that had NA values for the Medal variable. The main purpose of doing this was too narrow down our focus on the individual athletes who won medals in the olympic games as per the records. The new dataset after removing the NA values from the Medal variable was named “athletes_1”, which became our primary dataset to work on our analysis.
Project Tasks
Subset for Team United States
USA <- athletes_1 %>% filter(Team=="United States") 
USA %>% glimpse()
## Rows: 5,219
## Columns: 15
## $ ID     <int> 84, 145, 150, 153, 165, 351, 391, 423, 454, 699, 762, 807, 807,…
## $ Name   <chr> "Stephen Anthony Abas", "Jeremy Abbott", "Margaret Ives Abbott …
## $ Sex    <chr> "M", "M", "F", "F", "F", "M", "M", "M", "M", "M", "M", "M", "M"…
## $ Age    <int> 26, 28, 23, 23, 20, 23, 23, 22, 19, 22, 21, 22, 22, 36, 31, 27,…
## $ Height <int> 165, 175, NA, 191, 175, 202, 185, 182, 182, NA, NA, 188, 188, N…
## $ Weight <dbl> 55, 70, NA, 88, 56, 104, 102, 84, 68, NA, NA, 78, 78, NA, NA, 7…
## $ Team   <chr> "United States", "United States", "United States", "United Stat…
## $ NOC    <chr> "USA", "USA", "USA", "USA", "USA", "USA", "USA", "USA", "USA", …
## $ Games  <chr> "2004 Summer", "2014 Winter", "1900 Summer", "2008 Summer", "20…
## $ Year   <fct> 2004, 2014, 1900, 2008, 2004, 2000, 1924, 2000, 1932, 1920, 193…
## $ Season <chr> "Summer", "Winter", "Summer", "Summer", "Summer", "Summer", "Wi…
## $ City   <chr> "Athina", "Sochi", "Paris", "Beijing", "Athina", "Sydney", "Cha…
## $ Sport  <chr> "Wrestling", "Figure Skating", "Golf", "Softball", "Taekwondo",…
## $ Event  <chr> "Wrestling Men's Featherweight, Freestyle", "Figure Skating Mix…
## $ Medal  <chr> "Silver", "Bronze", "Gold", "Silver", "Silver", "Gold", "Silver…
View(USA)
To analyze the records of USA athletes, we created a subset of the “athletes_1” dataset using filter function and assigned it to USA.
In the USA dataset, group by the “sport” variable, summarize the total medals of each sport, and arrange in descending order based on number of medals
USA_group <- USA %>% group_by(Sport) %>% count (Sport, sort = TRUE)
USA_group
## # A tibble: 47 × 2
## # Groups:   Sport [47]
##    Sport             n
##    <chr>         <int>
##  1 Athletics      1071
##  2 Swimming       1066
##  3 Basketball      341
##  4 Rowing          333
##  5 Ice Hockey      276
##  6 Shooting        193
##  7 Gymnastics      177
##  8 Diving          140
##  9 Equestrianism   132
## 10 Water Polo      129
## # ℹ 37 more rows
From the above line of codes, we used pipe operator with group by and count function to display the total number of medals won per each sport by the USA athletes. The USA athletes were most successful in Athletics and Swimming, compared to other sports, over the years.
In the USA dataset, group by the “City”, “Year” variable, summarize the total medals won in each city, and arrange in descending order based on number of medals
USA_group_1 <- USA %>% group_by(City, Year) %>% count (City, sort = TRUE)
USA_group_1
## # A tibble: 50 × 3
## # Groups:   City, Year [50]
##    City           Year      n
##    <chr>          <fct> <int>
##  1 Los Angeles    1984    352
##  2 Beijing        2008    309
##  3 Athina         2004    259
##  4 Rio de Janeiro 2016    256
##  5 Atlanta        1996    255
##  6 Sydney         2000    240
##  7 London         2012    238
##  8 Barcelona      1992    222
##  9 Seoul          1988    207
## 10 St. Louis      1904    199
## # ℹ 40 more rows
In the above line of codes, we broke down the USA dataset by the cities and years, displaying total medal counts. From these table, we found the top 5 cities where USA atheltes won Olympic medals were Los Angeles, Beijing, Athina, Rio de Janeiro, and Atlanta.
In the USA dataset, create a variable “usa_swim” by making a subset for the sport swim
usa_swim <- USA %>% filter(Sport=="Swimming") 
usa_swim %>% glimpse()
## Rows: 1,066
## Columns: 15
## $ ID     <int> 813, 1017, 1017, 1017, 1017, 1017, 1017, 1017, 1017, 1380, 1380…
## $ Name   <chr> "Edgar Holmes Adams", "Nathan Ghar-Jun Adrian", "Nathan Ghar-Ju…
## $ Sex    <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "F", "F", "F", "F"…
## $ Age    <int> 36, 19, 23, 23, 23, 27, 27, 27, 27, 22, 22, 22, 17, 15, 21, 20,…
## $ Height <int> NA, 198, 198, 198, 198, 198, 198, 198, 198, 172, 172, 172, 170,…
## $ Weight <dbl> NA, 100, 100, 100, 100, 100, 100, 100, 100, 52, 52, 52, 59, 59,…
## $ Team   <chr> "United States", "United States", "United States", "United Stat…
## $ NOC    <chr> "USA", "USA", "USA", "USA", "USA", "USA", "USA", "USA", "USA", …
## $ Games  <chr> "1904 Summer", "2008 Summer", "2012 Summer", "2012 Summer", "20…
## $ Year   <fct> 1904, 2008, 2012, 2012, 2012, 2016, 2016, 2016, 2016, 1992, 199…
## $ Season <chr> "Summer", "Summer", "Summer", "Summer", "Summer", "Summer", "Su…
## $ City   <chr> "St. Louis", "Beijing", "London", "London", "London", "Rio de J…
## $ Sport  <chr> "Swimming", "Swimming", "Swimming", "Swimming", "Swimming", "Sw…
## $ Event  <chr> "Swimming Men's Plunge For Distance", "Swimming Men's 4 x 100 m…
## $ Medal  <chr> "Silver", "Gold", "Gold", "Silver", "Gold", "Bronze", "Bronze",…
View(usa_swim)
Since Swimming is one of the top sports USA athelets won medals, we further drilled down the dataset to observe the records of all the athletes over the years.
In the athletes dataset, calculate the medal tally per country and sort them in descending order
athletes_2 <- athletes_1 %>% group_by(Team) %>% count (Team, sort = TRUE)
colnames(athletes_2) <- c("Country", "Medal_count")
athletes_2
## # A tibble: 498 × 2
## # Groups:   Country [498]
##    Country       Medal_count
##    <chr>               <int>
##  1 United States        5219
##  2 Soviet Union         2451
##  3 Germany              1984
##  4 Great Britain        1673
##  5 France               1550
##  6 Italy                1527
##  7 Sweden               1434
##  8 Australia            1306
##  9 Canada               1243
## 10 Hungary              1127
## # ℹ 488 more rows
The above line of codes displayed the total medal count per country using the primary dataset “athletes_1”. From the results we observe, USA won most medals and is at the top of the list with 5219 medals. Soviet Union, Germany, Great Britain and France followed USA with their success in Olympics games over the years.
List the top 10 medal winning countries
Top_10 = top_n(ungroup(athletes_2), 10)
## Selecting by Medal_count
Top_10
## # A tibble: 10 × 2
##    Country       Medal_count
##    <chr>               <int>
##  1 United States        5219
##  2 Soviet Union         2451
##  3 Germany              1984
##  4 Great Britain        1673
##  5 France               1550
##  6 Italy                1527
##  7 Sweden               1434
##  8 Australia            1306
##  9 Canada               1243
## 10 Hungary              1127
The above line of codes displayed results for Top 10 medal winning countries with Hungary being at the bottom of the list.
Plot the top 10 countries in a column chart
Top_10 %>% ggplot() + geom_col(mapping = aes(x = Country, y = Medal_count), fill = "blue") +
  labs(title = "Top 10 Medal Winning Countries")


The above line of codes plotted a column chart displaying the Top 10 medal winning countries using geom_col function from “ggplot2” package. From this chart we observe, most of the nations had on average at least 1000 medal counts with USA claiming over 5000 medals.
Calculate medal tally of countries per year
medal_countries <- athletes_1 %>% group_by(Team, Year) %>% count (Team, sort = TRUE)
medal_countries
## # A tibble: 2,012 × 3
## # Groups:   Team, Year [2,012]
##    Team          Year      n
##    <chr>         <fct> <int>
##  1 Soviet Union  1980    488
##  2 United States 1984    359
##  3 Soviet Union  1988    352
##  4 Soviet Union  1976    336
##  5 United States 2008    309
##  6 East Germany  1980    287
##  7 Unified Team  1992    271
##  8 United States 2004    259
##  9 United States 2016    256
## 10 Soviet Union  1972    255
## # ℹ 2,002 more rows
The above line of codes calculated medal tally of countries per year, displaying the counts in descending order. We observe Soviet Union won 488 medals in the Year 1980, which is the most medals won by a nation in a single year, followed by USA in the second place with 359 medals in the year 1984.
From athletes dataset, group by team and medal, and then summarise the count of medals, and display in descending order.
medal_countries_1 <- athletes_1 %>% group_by(Team, Medal) %>% count (Team, sort = TRUE)
medal_countries_1
## # A tibble: 783 × 3
## # Groups:   Team, Medal [783]
##    Team          Medal      n
##    <chr>         <chr>  <int>
##  1 United States Gold    2474
##  2 United States Silver  1512
##  3 United States Bronze  1233
##  4 Soviet Union  Gold    1058
##  5 Soviet Union  Silver   716
##  6 Germany       Gold     679
##  7 Germany       Bronze   678
##  8 Soviet Union  Bronze   677
##  9 Germany       Silver   627
## 10 Great Britain Silver   582
## # ℹ 773 more rows
The above line of codes, when we summarized the count of medals by team and medal category, we observe, USA athletes won the most Gold, Silver, and Bronze medals with a count of 2474, 1512, and 1233 respectively. This is a great record for USA athletes in the Olympics over the other nations.
For team “United States”, “France”, “Germany”, “Great Britain” show medal count by the number of gold, silver, and bronze. Plot the result displaying the teams and the medal counts.
The following line of codes performed subset of USA, France, Germany, and Great Britain from the primary dataset. The purpose was to get a better insught into the records of the athletes representing this nations in terms of Gold, Silver, and Bronze medals won.
Subset for USA
USA <- athletes_1 %>% filter(Team=="United States") 
USA %>% glimpse()
## Rows: 5,219
## Columns: 15
## $ ID     <int> 84, 145, 150, 153, 165, 351, 391, 423, 454, 699, 762, 807, 807,…
## $ Name   <chr> "Stephen Anthony Abas", "Jeremy Abbott", "Margaret Ives Abbott …
## $ Sex    <chr> "M", "M", "F", "F", "F", "M", "M", "M", "M", "M", "M", "M", "M"…
## $ Age    <int> 26, 28, 23, 23, 20, 23, 23, 22, 19, 22, 21, 22, 22, 36, 31, 27,…
## $ Height <int> 165, 175, NA, 191, 175, 202, 185, 182, 182, NA, NA, 188, 188, N…
## $ Weight <dbl> 55, 70, NA, 88, 56, 104, 102, 84, 68, NA, NA, 78, 78, NA, NA, 7…
## $ Team   <chr> "United States", "United States", "United States", "United Stat…
## $ NOC    <chr> "USA", "USA", "USA", "USA", "USA", "USA", "USA", "USA", "USA", …
## $ Games  <chr> "2004 Summer", "2014 Winter", "1900 Summer", "2008 Summer", "20…
## $ Year   <fct> 2004, 2014, 1900, 2008, 2004, 2000, 1924, 2000, 1932, 1920, 193…
## $ Season <chr> "Summer", "Winter", "Summer", "Summer", "Summer", "Summer", "Wi…
## $ City   <chr> "Athina", "Sochi", "Paris", "Beijing", "Athina", "Sydney", "Cha…
## $ Sport  <chr> "Wrestling", "Figure Skating", "Golf", "Softball", "Taekwondo",…
## $ Event  <chr> "Wrestling Men's Featherweight, Freestyle", "Figure Skating Mix…
## $ Medal  <chr> "Silver", "Bronze", "Gold", "Silver", "Silver", "Gold", "Silver…
View(USA)
Subset for France
France <- athletes_1 %>% filter(Team=="France") 
France %>% glimpse()
## Rows: 1,550
## Columns: 15
## $ ID     <int> 56, 73, 73, 73, 93, 583, 583, 629, 777, 1131, 1173, 1173, 1173,…
## $ Name   <chr> "Ren Abadie", "Luc Abalo", "Luc Abalo", "Luc Abalo", "Jol Marc …
## $ Sex    <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "F", "M", "M", "M"…
## $ Age    <int> 21, 23, 27, 31, 38, 23, 27, 24, 24, 23, 20, 20, 20, 24, 28, 17,…
## $ Height <int> NA, 182, 182, 182, 190, 180, 180, 194, NA, 164, 202, 202, 202, …
## $ Weight <dbl> NA, 86, 86, 86, 85, 68, 68, 104, NA, 66, 90, 90, 90, 73, 73, 69…
## $ Team   <chr> "France", "France", "France", "France", "France", "France", "Fr…
## $ NOC    <chr> "FRA", "FRA", "FRA", "FRA", "FRA", "FRA", "FRA", "FRA", "FRA", …
## $ Games  <chr> "1956 Summer", "2008 Summer", "2012 Summer", "2016 Summer", "20…
## $ Year   <fct> 1956, 2008, 2012, 2016, 2008, 2004, 2008, 2012, 1948, 2016, 201…
## $ Season <chr> "Summer", "Summer", "Summer", "Summer", "Summer", "Summer", "Su…
## $ City   <chr> "Melbourne", "Beijing", "London", "Rio de Janeiro", "Beijing", …
## $ Sport  <chr> "Cycling", "Handball", "Handball", "Handball", "Handball", "Cyc…
## $ Event  <chr> "Cycling Men's Road Race, Team", "Handball Men's Handball", "Ha…
## $ Medal  <chr> "Gold", "Gold", "Gold", "Silver", "Gold", "Gold", "Gold", "Gold…
View(France)
Subset for Germany
Germany <- athletes_1 %>% filter(Team=="Germany") 
Germany %>% glimpse()
## Rows: 1,984
## Columns: 15
## $ ID     <int> 702, 702, 702, 775, 849, 850, 967, 1046, 1356, 1356, 1449, 1481…
## $ Name   <chr> "Ronny Ackermann", "Ronny Ackermann", "Ronny Ackermann", "Otto …
## $ Sex    <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M"…
## $ Age    <int> 24, 24, 28, 26, 29, 26, 23, 24, 29, 41, 16, 23, 26, 26, 25, 23,…
## $ Height <int> 184, 184, 184, NA, 189, NA, 172, 189, 189, 189, 163, 183, 183, …
## $ Weight <dbl> 69, 69, 69, NA, 87, NA, 62, 82, 80, 80, 50, 75, NA, 92, 72, NA,…
## $ Team   <chr> "Germany", "Germany", "Germany", "Germany", "Germany", "Germany…
## $ NOC    <chr> "GER", "GER", "GER", "GER", "GER", "GER", "GER", "GER", "GER", …
## $ Games  <chr> "2002 Winter", "2002 Winter", "2006 Winter", "1936 Summer", "20…
## $ Year   <fct> 2002, 2002, 2006, 1936, 2012, 1936, 1960, 1964, 2004, 2016, 196…
## $ Season <chr> "Winter", "Winter", "Winter", "Summer", "Summer", "Summer", "Su…
## $ City   <chr> "Salt Lake City", "Salt Lake City", "Torino", "Berlin", "London…
## $ Sport  <chr> "Nordic Combined", "Nordic Combined", "Nordic Combined", "Fenci…
## $ Event  <chr> "Nordic Combined Men's Team", "Nordic Combined Men's Sprint", "…
## $ Medal  <chr> "Silver", "Silver", "Silver", "Bronze", "Gold", "Gold", "Silver…
View(Germany)
Subset for Great Britain
GB <- athletes_1 %>% filter(Team=="Great Britain") 
GB %>% glimpse()
## Rows: 1,673
## Columns: 15
## $ ID     <int> 509, 519, 519, 830, 830, 832, 832, 840, 964, 980, 980, 980, 980…
## $ Name   <chr> "Gary Abraham", "Harold Maurice Abrahams", "Harold Maurice Abra…
## $ Sex    <chr> "M", "M", "M", "M", "M", "F", "F", "F", "M", "F", "F", "F", "F"…
## $ Age    <int> 21, 24, 24, 21, 25, 29, 33, 24, 32, 19, 19, 23, 23, 22, 19, 23,…
## $ Height <int> 175, 183, 183, 178, 178, 164, 164, 164, NA, 179, 179, 179, 179,…
## $ Weight <dbl> 64, 75, 75, 78, 78, 51, 51, 69, NA, 70, 70, 70, 70, 57, 90, 90,…
## $ Team   <chr> "Great Britain", "Great Britain", "Great Britain", "Great Brita…
## $ NOC    <chr> "GBR", "GBR", "GBR", "GBR", "GBR", "GBR", "GBR", "GBR", "GBR", …
## $ Games  <chr> "1980 Summer", "1924 Summer", "1924 Summer", "1980 Summer", "19…
## $ Year   <fct> 1980, 1924, 1924, 1980, 1984, 2012, 2016, 2014, 1948, 2008, 200…
## $ Season <chr> "Summer", "Summer", "Summer", "Summer", "Summer", "Summer", "Su…
## $ City   <chr> "Moskva", "Paris", "Paris", "Moskva", "Los Angeles", "London", …
## $ Sport  <chr> "Swimming", "Athletics", "Athletics", "Judo", "Judo", "Boxing",…
## $ Event  <chr> "Swimming Men's 4 x 100 metres Medley Relay", "Athletics Men's …
## $ Medal  <chr> "Bronze", "Gold", "Silver", "Silver", "Silver", "Gold", "Gold",…
View(GB)
Medal count for United States
USA %>% group_by(Team, Medal) %>% count (Team, sort = TRUE)
## # A tibble: 3 × 3
## # Groups:   Team, Medal [3]
##   Team          Medal      n
##   <chr>         <chr>  <int>
## 1 United States Gold    2474
## 2 United States Silver  1512
## 3 United States Bronze  1233
The above line of codes displays USA won more Gold than Silver and Bronze medals.
Medal count for France
France %>% group_by(Team, Medal) %>% count (Team, sort = TRUE)
## # A tibble: 3 × 3
## # Groups:   Team, Medal [3]
##   Team   Medal      n
##   <chr>  <chr>  <int>
## 1 France Bronze   577
## 2 France Silver   518
## 3 France Gold     455
The above line of codes displays France won more Bronze than Silver and Gold medals.
Medal count for Germany
Germany %>% group_by(Team, Medal) %>% count (Team, sort = TRUE)
## # A tibble: 3 × 3
## # Groups:   Team, Medal [3]
##   Team    Medal      n
##   <chr>   <chr>  <int>
## 1 Germany Gold     679
## 2 Germany Bronze   678
## 3 Germany Silver   627
The above line of codes displays Germany won more Gold than Bronze and Silver medals. However, the medal counts were very similar for different medal categories.
Medal count for Great Britain
GB %>% group_by(Team, Medal) %>% count (Team, sort = TRUE)
## # A tibble: 3 × 3
## # Groups:   Team, Medal [3]
##   Team          Medal      n
##   <chr>         <chr>  <int>
## 1 Great Britain Silver   582
## 2 Great Britain Bronze   572
## 3 Great Britain Gold     519
The above line of codes displays Great Britain won more Silver than Bronze and Gold medals.
Plot displaying team “United States” and medal counts
ggplot(USA, aes(x = Medal)) + geom_bar(fill = "Blue") +
  labs(title = "Medal counts for USA")


Plot displaying team “France” and medal counts
ggplot(France, aes(x = Medal)) + geom_bar(fill = "Blue") +
  labs(title = "Medal counts for France")


Plot displaying team “Germany” and medal counts
ggplot(Germany, aes(x = Medal)) + geom_bar(fill = "Blue") +
  labs(title = "Medal counts for Germany")


Plot displaying team “Great Britain” and medal counts
ggplot(GB, aes(x = Medal)) + geom_bar(fill = "Blue") +
  labs(title = "Medal counts for Great Britain")


The above bar plots displayed the Gold, Silver, and Bronze medal won by USA, France, Germany, and Great Britain using the tables that showed all the medal counts per each country.
Show the medal count of Gold by team and display in descending order
athletes_1  %>% group_by(Team, Medal) %>% filter(Medal=="Gold") %>% count (Team, sort = TRUE)
## # A tibble: 242 × 3
## # Groups:   Team, Medal [242]
##    Team          Medal     n
##    <chr>         <chr> <int>
##  1 United States Gold   2474
##  2 Soviet Union  Gold   1058
##  3 Germany       Gold    679
##  4 Italy         Gold    535
##  5 Great Britain Gold    519
##  6 France        Gold    455
##  7 Sweden        Gold    451
##  8 Hungary       Gold    432
##  9 Canada        Gold    422
## 10 East Germany  Gold    369
## # ℹ 232 more rows
The above table displayed the Gold medals won by each country is descending order with USA topping the list followed by Soviet Union, Germany, Italy, and Great Britain in the top 5.
Show the medal count of Silver by team and display in descending order
athletes_1  %>% group_by(Team, Medal) %>% filter(Medal=="Silver") %>% count (Team, sort = TRUE)
## # A tibble: 273 × 3
## # Groups:   Team, Medal [273]
##    Team          Medal      n
##    <chr>         <chr>  <int>
##  1 United States Silver  1512
##  2 Soviet Union  Silver   716
##  3 Germany       Silver   627
##  4 Great Britain Silver   582
##  5 France        Silver   518
##  6 Italy         Silver   508
##  7 Sweden        Silver   476
##  8 Australia     Silver   453
##  9 Canada        Silver   413
## 10 Russia        Silver   351
## # ℹ 263 more rows
The above table displayed the Silver medals won by each country is descending order with USA topping the list followed by Soviet Union, Germany, Great Britain, and France in the top 5.
Show the medal count of Bronze by team and display in descending order
athletes_1  %>% group_by(Team, Medal) %>% filter(Medal=="Bronze") %>% count (Team, sort = TRUE)
## # A tibble: 268 × 3
## # Groups:   Team, Medal [268]
##    Team          Medal      n
##    <chr>         <chr>  <int>
##  1 United States Bronze  1233
##  2 Germany       Bronze   678
##  3 Soviet Union  Bronze   677
##  4 France        Bronze   577
##  5 Great Britain Bronze   572
##  6 Australia     Bronze   511
##  7 Sweden        Bronze   507
##  8 Italy         Bronze   484
##  9 Finland       Bronze   415
## 10 Canada        Bronze   408
## # ℹ 258 more rows
The above table displayed the Bronze medals won by each country is descending order with USA topping the list followed by Germany, Soviet Union, France, and Great Britain. One interesting fact is, the top 5 countries were very consistent in winning medals for all three medal categories.
Plot the total medal count of “United States”, “France”, “Germany”, and “Great Britain”
Top_5 = top_n(ungroup(athletes_2), 5)
## Selecting by Medal_count
US_Fra_Ger_GB <- Top_5 [-2,]
US_Fra_Ger_GB
## # A tibble: 4 × 2
##   Country       Medal_count
##   <chr>               <int>
## 1 United States        5219
## 2 Germany              1984
## 3 Great Britain        1673
## 4 France               1550
US_Fra_Ger_GB %>% ggplot() + geom_col(mapping = aes(x = Country, y = Medal_count), fill = "blue") +
  labs(title = "Total Medal Count")


The above line of codes used “athletes_2” dataset which includes the total medal tally per countries in descending order to plot a column chart displaying USA, Great Britain, Germany, and France. From the plot we observe France and Great Britain have similar medal counts with Germany having higher medal counts than them. USA is at the top of the list.
For “United States”, “Russia”, “Germany”, “France”, and “China” compare the medal tally for summer and winter games and plot the results
The below two section of codes filtered the medal tally per category for USA for Summer and Winter Olympics.
Medal count for United States for summer
USA_Summer <- USA %>% group_by(Team, Medal) %>% filter(Season == "Summer") %>% count (Team, sort = TRUE)
USA_Summer
## # A tibble: 3 × 3
## # Groups:   Team, Medal [3]
##   Team          Medal      n
##   <chr>         <chr>  <int>
## 1 United States Gold    2333
## 2 United States Silver  1241
## 3 United States Bronze  1112
Medal count for United States for winter
USA_Winter <- USA %>% group_by(Team, Medal) %>% filter(Season == "Winter") %>% count (Team, sort = TRUE)
USA_Winter
## # A tibble: 3 × 3
## # Groups:   Team, Medal [3]
##   Team          Medal      n
##   <chr>         <chr>  <int>
## 1 United States Silver   271
## 2 United States Gold     141
## 3 United States Bronze   121
From the above two tables we observe, USA is a dominant participant in Summer Olympics than Winter Olympics.
Subset for Russia
Russia <- athletes_1 %>% filter(Team=="Russia")
Russia %>% glimpse()
## Rows: 1,110
## Columns: 15
## $ ID     <int> 67, 90, 455, 455, 455, 455, 455, 543, 548, 580, 1071, 1085, 163…
## $ Name   <chr> "Mariya Vasilyevna Abakumova (-Tarabina)", "Tamila Rashidovna A…
## $ Sex    <chr> "F", "F", "M", "M", "M", "M", "M", "M", "F", "F", "F", "M", "F"…
## $ Age    <int> 22, 21, 19, 19, 24, 24, 24, 25, 23, 28, 20, 22, 21, 25, 29, 29,…
## $ Height <int> 179, 163, 161, 161, 161, 161, 161, 198, 167, 188, 158, 183, 160…
## $ Weight <dbl> 80, 60, 62, 62, 62, 62, 62, 89, 65, 77, 48, 88, 55, 55, 55, 55,…
## $ Team   <chr> "Russia", "Russia", "Russia", "Russia", "Russia", "Russia", "Ru…
## $ NOC    <chr> "RUS", "RUS", "RUS", "RUS", "RUS", "RUS", "RUS", "RUS", "RUS", …
## $ Games  <chr> "2008 Summer", "2004 Summer", "2012 Summer", "2012 Summer", "20…
## $ Year   <fct> 2008, 2004, 2012, 2012, 2016, 2016, 2016, 2004, 2006, 2008, 201…
## $ Season <chr> "Summer", "Summer", "Summer", "Summer", "Summer", "Summer", "Su…
## $ City   <chr> "Beijing", "Athina", "London", "London", "Rio de Janeiro", "Rio…
## $ Sport  <chr> "Athletics", "Cycling", "Gymnastics", "Gymnastics", "Gymnastics…
## $ Event  <chr> "Athletics Women's Javelin Throw", "Cycling Women's Sprint", "G…
## $ Medal  <chr> "Silver", "Silver", "Bronze", "Silver", "Silver", "Silver", "Br…
View(Russia)
Subset for China
China <- athletes_1 %>% filter(Team=="China")
China %>% glimpse()
## Rows: 901
## Columns: 15
## $ ID     <int> 3610, 3610, 3610, 3611, 6381, 7597, 11223, 17282, 17289, 17294,…
## $ Name   <chr> "An Yulong", "An Yulong", "An Yulong", "An Zhongxin", "Ba Yan",…
## $ Sex    <chr> "M", "M", "M", "F", "F", "F", "F", "F", "F", "M", "M", "F", "F"…
## $ Age    <int> 19, 19, 23, 23, 21, 24, 14, 16, 18, 23, 25, 24, 29, 17, 21, 21,…
## $ Height <int> 173, 173, 173, 170, 183, 172, 142, 174, 168, 174, 175, 168, 176…
## $ Weight <dbl> 70, 70, 70, 65, 78, 67, 35, 63, 48, 60, 55, 75, 71, 42, 42, 42,…
## $ Team   <chr> "China", "China", "China", "China", "China", "China", "China", …
## $ NOC    <chr> "CHN", "CHN", "CHN", "CHN", "CHN", "CHN", "CHN", "CHN", "CHN", …
## $ Games  <chr> "1998 Winter", "1998 Winter", "2002 Winter", "1996 Summer", "19…
## $ Year   <fct> 1998, 1998, 2002, 1996, 1984, 2008, 1996, 1996, 2008, 2000, 201…
## $ Season <chr> "Winter", "Winter", "Winter", "Summer", "Summer", "Summer", "Su…
## $ City   <chr> "Nagano", "Nagano", "Salt Lake City", "Atlanta", "Los Angeles",…
## $ Sport  <chr> "Short Track Speed Skating", "Short Track Speed Skating", "Shor…
## $ Event  <chr> "Short Track Speed Skating Men's 500 metres", "Short Track Spee…
## $ Medal  <chr> "Silver", "Bronze", "Bronze", "Silver", "Bronze", "Silver", "Si…
View(China)
The above two sections performed subsets to filter out records for Russia and China from the primary dataset “athletes_1”.
Medal count for Russia for summer
Russia_Summer <- Russia %>% group_by(Team, Medal) %>% filter(Season == "Summer") %>% count (Team, sort = TRUE)
Russia_Summer
## # A tibble: 3 × 3
## # Groups:   Team, Medal [3]
##   Team   Medal      n
##   <chr>  <chr>  <int>
## 1 Russia Bronze   322
## 2 Russia Gold     294
## 3 Russia Silver   278
Medal count for Russia for winter
Russia_Winter <- Russia %>% group_by(Team, Medal) %>% filter(Season == "Winter") %>% count (Team, sort = TRUE)
Russia_Winter
## # A tibble: 3 × 3
## # Groups:   Team, Medal [3]
##   Team   Medal      n
##   <chr>  <chr>  <int>
## 1 Russia Silver    73
## 2 Russia Gold      72
## 3 Russia Bronze    71
From the above two tables we observe, Russia is a dominant participant in Summer than Winter Olympics.
Medal count for Germany for summer
Germany_Summer <- Germany %>% group_by(Team, Medal) %>% filter(Season == "Summer") %>% count (Team, sort = TRUE)
Germany_Summer
## # A tibble: 3 × 3
## # Groups:   Team, Medal [3]
##   Team    Medal      n
##   <chr>   <chr>  <int>
## 1 Germany Bronze   610
## 2 Germany Gold     564
## 3 Germany Silver   513
Medal count for Germany for winter
Germany_Winter <- Germany %>% group_by(Team, Medal) %>% filter(Season == "Winter") %>% count (Team, sort = TRUE)
Germany_Winter
## # A tibble: 3 × 3
## # Groups:   Team, Medal [3]
##   Team    Medal      n
##   <chr>   <chr>  <int>
## 1 Germany Gold     115
## 2 Germany Silver   114
## 3 Germany Bronze    68
From the above two tables we observe, Germany is a dominant participant in Summer than Winter Olympics.
Medal count for France for summer
France_Summer <- France %>% group_by(Team, Medal) %>% filter(Season == "Summer") %>% count (Team, sort = TRUE)
France_Summer
## # A tibble: 3 × 3
## # Groups:   Team, Medal [3]
##   Team   Medal      n
##   <chr>  <chr>  <int>
## 1 France Bronze   502
## 2 France Silver   485
## 3 France Gold     421
Medal count for France for winter
France_Winter <- France %>% group_by(Team, Medal) %>% filter(Season == "Winter") %>% count (Team, sort = TRUE)
France_Winter
## # A tibble: 3 × 3
## # Groups:   Team, Medal [3]
##   Team   Medal      n
##   <chr>  <chr>  <int>
## 1 France Bronze    75
## 2 France Gold      34
## 3 France Silver    33
From the above two tables we observe, France is a dominant participant in Summer than Winter Olympics.
Medal count for China for summer
China_Summer <- China %>% group_by(Team, Medal) %>% filter(Season == "Summer") %>% count (Team, sort = TRUE)
China_Summer
## # A tibble: 3 × 3
## # Groups:   Team, Medal [3]
##   Team  Medal      n
##   <chr> <chr>  <int>
## 1 China Silver   299
## 2 China Gold     294
## 3 China Bronze   238
Medal count for China for winter
China_Winter <- China %>% group_by(Team, Medal) %>% filter(Season == "Winter") %>% count (Team, sort = TRUE)
China_Winter
## # A tibble: 3 × 3
## # Groups:   Team, Medal [3]
##   Team  Medal      n
##   <chr> <chr>  <int>
## 1 China Bronze    30
## 2 China Silver    26
## 3 China Gold      14
From the above two tables we observe, China is a dominant participant in Summer than Winter Olympics.
Plot displaying team “United States” medal counts for Summer
USA_Summer %>% ggplot() + geom_col(mapping = aes(x = Medal, y = n), fill = "blue") +
  labs(title = "Total Medal Count for USA in Summer")


Plot displaying team “United States” medal counts for Winter
USA_Winter %>% ggplot() + geom_col(mapping = aes(x = Medal, y = n), fill = "blue") +
  labs(title = "Total Medal Count for USA in Winter")


Plot displaying team “Russia” medal counts for Summer
Russia_Summer %>% ggplot() + geom_col(mapping = aes(x = Medal, y = n), fill = "blue") +
  labs(title = "Total Medal Count for Russia in Summer")


Plot displaying team “Russia” medal counts for Winter
Russia_Winter %>% ggplot() + geom_col(mapping = aes(x = Medal, y = n), fill = "blue") +
  labs(title = "Total Medal Count for Russia in Winter")


Plot displaying team “Germany” medal counts for Summer
Germany_Summer %>% ggplot() + geom_col(mapping = aes(x = Medal, y = n), fill = "blue") +
  labs(title = "Total Medal Count for Germany in Summer")


Plot displaying team “Germany” medal counts for Winter
Germany_Winter %>% ggplot() + geom_col(mapping = aes(x = Medal, y = n), fill = "blue") +
  labs(title = "Total Medal Count for Germany in Winter")


Plot displaying team “France” medal counts for Summer
France_Summer %>% ggplot() + geom_col(mapping = aes(x = Medal, y = n), fill = "blue") +
  labs(title = "Total Medal Count for France in Summer")


Plot displaying team “France” medal counts for Winter
France_Winter %>% ggplot() + geom_col(mapping = aes(x = Medal, y = n), fill = "blue") +
  labs(title = "Total Medal Count for France in Winter")


Plot displaying team “China” medal counts for Summer
China_Summer %>% ggplot() + geom_col(mapping = aes(x = Medal, y = n), fill = "blue") +
  labs(title = "Total Medal Count for China in Summer")


Plot displaying team “China” medal counts for Winter
China_Winter %>% ggplot() + geom_col(mapping = aes(x = Medal, y = n), fill = "blue") +
  labs(title = "Total Medal Count for China in Winter")


The above 10 plots displayed the result of the medal counts per category for USA, Russia, Germany, France, and China in Summer and Winter Olympics.
##Calculate Medals won by women in the year 2016

Female_Winners <- athletes_1 %>% filter(Sex=="F", Year == 2016) %>% count (Medal, sort = TRUE) 
Female_Winners
##    Medal   n
## 1 Bronze 331
## 2 Silver 320
## 3   Gold 318
In the above table we observe the results of medals won by women in the year 2016 with Bronze having the most medal counts followed by Silver and Gold medals.
Conclusion
In conclusion, using advanced R packages we conducted detailed analysis on the various athletic events and came up with relevant insights guided by several tables and charts regarding the performance and records of various top performing nations in the Olympic games.
