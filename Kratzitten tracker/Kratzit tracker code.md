# Kratzit tracker
Deze code is geschreven door Lucas Rabou in januari 2022.
Iedereen mag de code gebruiken om zijn kratzit ervaringen nog beter te maken.
De volledige code staat onderaan, deze kan gekopieerd worden naar een python omgeving en vervolgens gerund worden.

## Functionaliteten van de code
De code zal tijdens het kratzitten de belangrijkse gegevans van het de spelers bijhouden. Dit zijn de volgende gegevens;
1. Het aantal gedronken beier
2. De gemiddelde snelheid waarop dit bier gedronken wordt ( weergegeven in minuten en seconden [min:sec])
3. De gespeelde tijd (weergegeven in uur, minuten en seconden [uur:min:sec])
4. een paarderace van de spelers om te visualiseren welke speler er vorop ligt.

Aan het einde van eht spel wordt een grafiek geplot die de gemiddelde snelheden van de spelers met elkaar vergelijkt. Deze grafiek verzint voor iedereen een willekeurige kleur, deze kleur wordte gekoppeld aan de initaal van de naam van de speler. Op die manier heeft iedere speler een eigen snelheidslijn.

## De code
De code, die hieronder te vinden is, heeft een aatnal inputs van de gebruiker nodig. Om ervoor te zorgen dat dit allemaal soepel verloopt zal ik nu de belangrijksete stappen uitleggen.
1. Als eerste is het van belang de code te runnen. De code zal blijven runnen totdat de gebruiker ervoor kiest om het spel te stoppen. (Hoe dit met wolgt later)
2. De code zal nu vragen om de namen van de spelers in te voeren. Dit is te herkennen aan de tekst "Enter player name" die verschijnt in de console. Vul hier nu een voor een de namen van de spelers is. 
Let op! deze namen moeten allemaal beginnen met een verschillende letter. Mochten er namen zijn met dezelfde beginletter, verzin dan een bijnaam voor deze persoon met een beginletter die nog niet gebruikt is. 

3. Wanneer alle namen ingevoerd zijn kan het spel gestard worden door 2x op enter te klikken.
4. Iedereen kan nu rustig beginnen met het leegmaken van zijn krat. Wanneer iemand een biertje op heeft voert hij/zij het eerste karakter van zijn naam in op de console. De speler Lucas zou dus de letter L invoeren. (Dit is niet hoofletter gevoelig). De statistieken zullen dan bijgewerkt worden en een volgende speler kan zijn karakter invoeren. Let op! Probeer altijd na het drinken van het biertje gelijk het karakter in te vullen, dit zorgt ervoor dat aan het einde van het spel de gemidelde snelheden en de grafiek beter kloppen.
5. Wanneer het spel is afgelopen kan de code gestopt worden door het woord "stop" in te voeren in de console ipv een karakter. Nu wordden de snelheden van de biertjes geschetst in een grafiek.

Tip! Zet de console op groot scherm en zoom zo ver mogelijk in zodat de paardenrace precies op het scherm past. Dit zorgt voor de beste spelervaring.
Tip! Doe tijdens het spelen de laptop aan de oplader, als je laptop tussendoor uit gaan zal de code namelijk niet opslaan. 

## De console
De console zal er tijdens runnen van de code als volgt uit zien;
```p
Enter inital: L
Gespeelde tijd>  0 : 0 : 7 

BEER COUNTER
Aantal biertjes gedronken door Lucas    : 2
Aantal biertjes gedronken door Britt    : 3

SPEED-TRACKER
Gemiddelde tijd Lucas    is 0 : 1
Gemiddelde tijd Britt    is 0 : 1

 =============================================================================================================
DE BIER PAARDENRACE
             0   1   2   3   4   5   6   7   8   9   10  11  12  13  14  15  16  17  18  19  20  21  22  23 24 
-------------------------------------------------------------------------------------------------------------
Lucas    |          o
Britt    |              o

Enter inital: 3
```

## De code
```

# -*- coding: utf-8 -*-
"""
Created on Fri Jan  7 22:01:26 2022
KRATZITTEN 

@author: Lucas Rabou
"""

import time
import matplotlib.pyplot as plt
import datetime
import math
import numpy as np
print("Deze code is geschreven door Lucas Rabou in januari 2022 te Eindhoven.\n")
print("--- Kratzit tracker ---\n\nWelkom bij de Kratzit tracker. deze app zal het mogelijk amken de belangrijkste statestieken bij te houden voor jullie kratzit avontuur. De statestieken die worden bijgehouden zijn;\n\n1. Aantal gesronken bier\n2. Gemidelde drinksnelheid\n3. De gespeelde tijd\n4. Een paardenrace visualisatie.")
print("\nSPELERSNAMEN INVOEREN:\nHierna zullen de namen van de spelers ingevuld moeten worden, hierbij zijn een aantal dingen belangrijk;\n- Het is niet mogelijk om een naam in te geven die al bestaat. \n- Het is niet mogelijk om een naam in te geven met dezelfde eerste letter van een naam die al ingegeven is")
print("\nBIER BIJHOUDEN:\nNa het invoeren van de namen begint het spel. Wanneer een biertje opgedronken is typ je het eerste karakter van de naam van de speler in op de console. Stel dat Lucas een biertje drinkt, dan wordt de letter L ingetypt (Dit is niet hoofletter gevoelig). Het spel kan gestopt worden door ipv een initiaal het woord 'STOP' te typen (Dit is ook niet hoofdletter gevoelig).")
print("\nCONSOLE:\nAlle data zal in de console verschijnen, ik raad daarom ook aan de console op groot beeld te zetten en in te zoomen totdat de paardenrace het volledige scherm vult.")
print("\nIk wens jullie heel veel kratzit plezier toe en onthoudt; Hardlopers zijn doodlopers")

name= "BEGIN"
Names =[]
Initials = []
while name != "":
    name = input("Enter player name: ")
    if name != "":
        Initial = name[0].upper()
        if Initial not in Initials:
            Initials.append(Initial)
            Names.append(name)
            print(Names[-1], "added to the game.\nWhen all players are added press enter twice to start")
        else:
            print("Deze naam bevat een initaal die al in het spel zit, kies een andere naam met een initaal dat nog niet in het spel zit")
    else:
        print("\nHet spel gaat beginnen!!!")
        
nr_players = len(Initials)

elapsed_minutes = 0
elapsed_hours = 0
start = time.time()
time.clock()
elapsed = 0

Bier_op = {x:0 for x in Initials}
Gem_Tijd = {x:[0,0] for x in Initials}
Tijd_minutes = {x:[] for x in Initials}
Tijd_hours = {x:[] for x in Initials}

Naam = "BEGIN"
while (Naam != "STOP"): 
    Naam = input("Enter inital: ")
    Naam = Naam.upper()
    if Naam in Initials:

        elapsed = time.time()-start
        elapsed_seconds = int(elapsed) 
        elapsed_minutes = int(elapsed_seconds/60)
        elapsed_hours = int(elapsed_minutes/60)
        
        if elapsed_seconds > 60:
            time_seconds = elapsed_seconds - elapsed_minutes*60
            time_minutes = elapsed_minutes - elapsed_hours*60
        else:
            time_minutes = 0
            time_seconds = elapsed_seconds
        
        print("Gespeelde tijd> ", elapsed_hours,":",time_minutes,":",time_seconds,"\n")
        # Vraag om input letter
        
        Bier_op[Naam] =  Bier_op[Naam]+1 # tel het gedronken beer in een dictionairy
        Tijd_hours[Naam].append(elapsed_hours) # houd de uren van ieder biertje bij in een dictionairy
        Tijd_minutes[Naam].append(time_minutes)# houd de minuten van ieder biertje bij in een dictionairy
        
        average_seconds = elapsed_seconds/(Bier_op[Naam])
        fractional, whole = math.modf(average_seconds/60)
        Gem_Biersnelheid = int(whole),":",int(fractional*60)
        Gem_Tijd[Naam][0] = int(whole)
        Gem_Tijd[Naam][1] = int(fractional*60)
    
        #Print het gedronken aantal 
        print("BEER COUNTER")
        for initial in Initials:
            Naam_speler = Names[Initials.index(initial)]
            print("Aantal biertjes gedronken door",Naam_speler[0:7]," "*(7-len(Naam_speler)),":", Bier_op[initial])
            
        print("\nSPEED-TRACKER")
        for initial in Initials:
            Naam_speler = Names[Initials.index(initial)]
            print("Gemiddelde tijd",Naam_speler[0:7],"is"," "*(7-len(Naam_speler)), Gem_Tijd[Initial][0],":",Gem_Tijd[Initial][1])
            
        # plot paardenplot
        
        print("\n","="*109)
        print("DE BIER PAARDENRACE")
        print(" "*12,"{:<4d}{:<4d}{:<4d}{:<4d}{:<4d}{:<4d}{:<4d}{:<4d}{:<4d}{:<4d}{:<4d}{:<4d}{:<4d}{:<4d}{:<4d}{:<4d}{:<4d}{:<4d}{:<4d}{:<4d}{:<4d}{:<4d}{:<4d}{:<3d}{:<3d}".format(0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24))
        print("-"*109)
        for initial in Initials:
            Naam_speler = Names[Initials.index(initial)]
            print(Naam_speler[0:7]," "*(7-len(Naam_speler)),"|", " "*4*Bier_op[initial], "o")  
    elif Naam != "STOP":   
        print("\nFoutieve naam ingegeven, probeer het opnieuw.")
        
    else:
        print("\n\nHet spel is beindigd")
        
    continue
    
plt.show()
#print End statistics
for initial in Initials:
    Naam_speler = Names[Initials.index(initial)]
    print("Aantal TOTAAL aantal biertjes gedronken door",Naam_speler[0:7]," "*(7-len(Naam_speler)),":", Bier_op[initial])

#make first plot (bier tov tijd)
customdate = datetime.datetime(2022, 1, 9, 12, 00) 
plt.figure()
legend = ""
for initial in Initials:
    Naam_speler = Names[Initials.index(initial)]
    x =  [customdate + datetime.timedelta(hours =h,minutes=m) for h,m in zip(Tijd_hours[initial], Tijd_minutes[initial])]
    locals()["x_"+initial] = x 
    y = range(1,len(x)+1)
    locals()["y_"+initial] = y
    rgb = np.random.rand(3,)
    plt.plot(x,y,c=rgb)
    legend = legend+initial

plt.legend(legend)
plt.gcf().autofmt_xdate()
new_list = list(range(1,25))
plt.yticks(new_list)
plt.xlabel("Tijd")
plt.ylabel("Biertjes")
plt.title("De Biermeter")
plt.show()


```
